---
title: Эффективное обновление — EF Core
description: Обзор производительности для эффективного обновления с помощью Entity Framework Core
author: roji
ms.date: 12/1/2020
uid: core/performance/efficient-updating
ms.openlocfilehash: 92766d2339fb04ed5ebc3123429171cc9be424b1
ms.sourcegitcommit: 4860d036ea0fb392c28799907bcc924c987d2d7b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97657756"
---
# <a name="efficient-updating"></a><span data-ttu-id="4ebe6-103">Эффективное обновление</span><span class="sxs-lookup"><span data-stu-id="4ebe6-103">Efficient Updating</span></span>

## <a name="batching"></a><span data-ttu-id="4ebe6-104">Пакетная обработка</span><span class="sxs-lookup"><span data-stu-id="4ebe6-104">Batching</span></span>

<span data-ttu-id="4ebe6-105">EF Core помогает снизить объем обращений, автоматически объединяя все обновления в единый цикл.</span><span class="sxs-lookup"><span data-stu-id="4ebe6-105">EF Core helps minimize roundtrips by automatically batching together all updates in a single roundtrip.</span></span> <span data-ttu-id="4ebe6-106">Рассмотрим следующий пример.</span><span class="sxs-lookup"><span data-stu-id="4ebe6-106">Consider the following:</span></span>

[!code-csharp[Main](../../../samples/core/Performance/Program.cs#SaveChangesBatching)]

<span data-ttu-id="4ebe6-107">Приведенный выше код загружает блог из базы данных, изменяет его имя, а затем добавляет два новых блога. чтобы применить эту возможность, в базу данных отправляются две инструкции SQL INSERT и одна инструкция UPDATE.</span><span class="sxs-lookup"><span data-stu-id="4ebe6-107">The above loads a blog from the database, changes its name, and then adds two new blogs; to apply this, two SQL INSERT statements and one UPDATE statement are sent to the database.</span></span> <span data-ttu-id="4ebe6-108">Вместо того чтобы отправлять их по одному, при добавлении экземпляров блога EF Core отслеживает эти изменения внутренне и выполняет их в одном цикле обработки при <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> вызове.</span><span class="sxs-lookup"><span data-stu-id="4ebe6-108">Rather than sending them one by one, as Blog instances are added, EF Core tracks these changes internally, and executes them in a single roundtrip when <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> is called.</span></span>

<span data-ttu-id="4ebe6-109">Количество инструкций, которые EF обрабатывает пакеты в одном цикле, зависит от используемого поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="4ebe6-109">The number of statements that EF batches in a single roundtrip depends on the database provider being used.</span></span> <span data-ttu-id="4ebe6-110">Например, анализ производительности показывает пакетную обработку, как правило, менее эффективно для SQL Server, если задействовано менее 4 инструкций.</span><span class="sxs-lookup"><span data-stu-id="4ebe6-110">For example, performance analysis has shown batching to be generally less efficient for SQL Server when less than 4 statements are involved.</span></span> <span data-ttu-id="4ebe6-111">Аналогичным образом, преимущества пакетной обработки снижаются до 40 инструкций для SQL Server, поэтому EF Core будет по умолчанию выполнять до 42 инструкций в одном пакете и выполнять дополнительные инструкции в отдельных обращениях.</span><span class="sxs-lookup"><span data-stu-id="4ebe6-111">Similarly, the benefits of batching degrade after around 40 statements for SQL Server, so EF Core will by default only execute up to 42 statements in a single batch, and execute additional statements in separate roundtrips.</span></span>

<span data-ttu-id="4ebe6-112">Пользователи также могут настроить эти пороговые значения для достижения потенциально большей производительности, но перед изменением нужно тщательно протестировать:</span><span class="sxs-lookup"><span data-stu-id="4ebe6-112">Users can also tweak these thresholds to achieve potentially higher performance - but benchmark carefully before modifying these:</span></span>

[!code-csharp[Main](../../../samples/core/Performance/BatchTweakingContext.cs#BatchTweaking)]

## <a name="bulk-updates"></a><span data-ttu-id="4ebe6-113">Групповые обновления</span><span class="sxs-lookup"><span data-stu-id="4ebe6-113">Bulk updates</span></span>

<span data-ttu-id="4ebe6-114">Предположим, что вы хотите дать всем своим сотрудникам возможность подняться.</span><span class="sxs-lookup"><span data-stu-id="4ebe6-114">Let's assume you want to give all your employees a raise.</span></span> <span data-ttu-id="4ebe6-115">Типичная реализация этого метода в EF Core будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="4ebe6-115">A typical implementation for this in EF Core would look like the following:</span></span>

[!code-csharp[Main](../../../samples/core/Performance/Program.cs#UpdateWithoutBulk)]

<span data-ttu-id="4ebe6-116">Хотя это вполне допустимый код, давайте проанализируем, что он делает с точки зрения производительности:</span><span class="sxs-lookup"><span data-stu-id="4ebe6-116">While this is perfectly valid code, let's analyze what it does from a performance perspective:</span></span>

* <span data-ttu-id="4ebe6-117">Выполняется цикл обработки базы данных для загрузки всех соответствующих сотрудников. Обратите внимание, что при этом все данные строки сотрудников переносятся на клиент, даже если потребуется только заработная плата.</span><span class="sxs-lookup"><span data-stu-id="4ebe6-117">A database roundtrip is performed, to load all the relevant employees; note that this brings all the Employees' row data to the client, even if only the salary will be needed.</span></span>
* <span data-ttu-id="4ebe6-118">Отслеживание изменений EF Core создает моментальные снимки при загрузке сущностей, а затем сравнивает эти моментальные снимки с экземплярами, чтобы определить, какие свойства изменились.</span><span class="sxs-lookup"><span data-stu-id="4ebe6-118">EF Core's change tracking creates snapshots when loading the entities, and then compares those snapshots to the instances to find out which properties changed.</span></span>
* <span data-ttu-id="4ebe6-119">Для сохранения всех изменений выполняется второй обмен данными с базой данных.</span><span class="sxs-lookup"><span data-stu-id="4ebe6-119">A second database roundtrip is performed to save all the changes.</span></span> <span data-ttu-id="4ebe6-120">Хотя все изменения выполняются в едином цикле благодаря пакетной обработке, EF Core по-прежнему отправляет инструкцию UPDATE для каждого сотрудника, который должен быть выполнен базой данных.</span><span class="sxs-lookup"><span data-stu-id="4ebe6-120">While all changes are done in a single roundtrip thanks to batching, EF Core still sends an UPDATE statement per employee, which must be executed by the database.</span></span>

<span data-ttu-id="4ebe6-121">Реляционные базы данных также поддерживают *групповые обновления*, поэтому приведенные выше инструкции могут быть перезаписаны как следующая отдельная инструкция SQL:</span><span class="sxs-lookup"><span data-stu-id="4ebe6-121">Relational databases also support *bulk updates*, so the above could be rewritten as the following single SQL statement:</span></span>

```sql
UPDATE [Employees] SET [Salary] = [Salary] + 1000;
```

<span data-ttu-id="4ebe6-122">Эта операция выполняет всю операцию в едином цикле, не загружая или не отправляя фактические данные в базу данных, и не использует механизм отслеживания изменений EF, который накладывает дополнительные издержки.</span><span class="sxs-lookup"><span data-stu-id="4ebe6-122">This performs the entire operation in a single roundtrip, without loading or sending any actual data to the database, and without making use of EF's change tracking machinery, which imposes an additional overhead.</span></span>

<span data-ttu-id="4ebe6-123">Увы, EF в настоящее время не предоставляет интерфейсы API для выполнения операций обновления.</span><span class="sxs-lookup"><span data-stu-id="4ebe6-123">Unfortunately, EF doesn't currently provide APIs for performing bulk updates.</span></span> <span data-ttu-id="4ebe6-124">Пока они не будут введены, можно использовать необработанный SQL для выполнения операции, в которой важна производительность.</span><span class="sxs-lookup"><span data-stu-id="4ebe6-124">Until these are introduced, you can use raw SQL to perform the operation where performance is sensitive:</span></span>

[!code-csharp[Main](../../../samples/core/Performance/Program.cs#UpdateWithBulk)]
