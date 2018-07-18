---
title: Обработка сбоев фиксации транзакции - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
caps.latest.revision: 3
ms.openlocfilehash: 95ac69a005a70f31086bd4d3c0088bd3e82d28e2
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122561"
---
# <a name="handling-transaction-commit-failures"></a><span data-ttu-id="33cbc-102">Обработка сбоев фиксации транзакции</span><span class="sxs-lookup"><span data-stu-id="33cbc-102">Handling transaction commit failures</span></span>
> [!NOTE]
> <span data-ttu-id="33cbc-103">**EF6.1 и более поздних версий только** -функции, интерфейсы API, и т.д., описанных на этой странице появились в версии 6.1 Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="33cbc-103">**EF6.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="33cbc-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="33cbc-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="33cbc-105">Как часть 6.1 мы представляем новый функция устойчивости подключений для Entity FRAMEWORK: возможность обнаружить и восстановить автоматически, когда к временному сбою подключения на подтверждения фиксации транзакций.</span><span class="sxs-lookup"><span data-stu-id="33cbc-105">As part of 6.1 we are introducing a new connection resiliency feature for EF: the ability to detect and recover automatically when transient connection failures affect the acknowledgement of transaction commits.</span></span> <span data-ttu-id="33cbc-106">Подробные сведения о сценарий лучше всего описывается в записи блога [подключение к базе данных SQL и проблема идемпотентности](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span><span class="sxs-lookup"><span data-stu-id="33cbc-106">The full details of the scenario are best described in the blog post [SQL Database Connectivity and the Idempotency Issue](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span></span>  <span data-ttu-id="33cbc-107">Таким образом сценарий: при возникновении исключения во время фиксации транзакции существуют две возможные причины:</span><span class="sxs-lookup"><span data-stu-id="33cbc-107">In summary, the scenario is that when an exception is raised during a transaction commit there are two possible causes:</span></span>  

1. <span data-ttu-id="33cbc-108">Не удалось выполнить фиксацию транзакции на сервере</span><span class="sxs-lookup"><span data-stu-id="33cbc-108">The transaction commit failed on the server</span></span>
2. <span data-ttu-id="33cbc-109">Успешность фиксации транзакции на сервере, но проблема с подключением, препятствующих переходу уведомление о выполнении из доставляется клиенту</span><span class="sxs-lookup"><span data-stu-id="33cbc-109">The transaction commit succeeded on the server but a connectivity issue prevented the success notification from reaching the client</span></span>  

<span data-ttu-id="33cbc-110">Когда происходит первая ситуация или пользователя приложения можно повторить операцию, но когда вторая ситуация происходит следует избегать повторных попыток, и приложение может восстановиться автоматически.</span><span class="sxs-lookup"><span data-stu-id="33cbc-110">When the first situation happens the application or the user can retry the operation, but when the second situation occurs retries should be avoided and the application could recover automatically.</span></span> <span data-ttu-id="33cbc-111">Проблема в том, если отсутствует возможность определить, какой параметр имел фактическая причина исключения была обнаружена во время фиксации, приложение не может выбрать правильный способ действий.</span><span class="sxs-lookup"><span data-stu-id="33cbc-111">The challenge is that without the ability to detect what was the actual reason an exception was reported during commit, the application cannot choose the right course of action.</span></span> <span data-ttu-id="33cbc-112">Новая функция в EF 6.1 позволяет EF еще раз проверьте с базой данных, если транзакция выполнена успешно и прозрачно использовать правильный способ действий.</span><span class="sxs-lookup"><span data-stu-id="33cbc-112">The new feature in EF 6.1 allows EF to double-check with the database if the transaction succeeded and take the right course of action transparently.</span></span>  

## <a name="using-the-feature"></a><span data-ttu-id="33cbc-113">С помощью функции</span><span class="sxs-lookup"><span data-stu-id="33cbc-113">Using the feature</span></span>  

<span data-ttu-id="33cbc-114">Чтобы включить функцию требуется включить вызов [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) в конструкторе вашего **DbConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="33cbc-114">In order to enable the feature you need include a call to [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) in the constructor of your **DbConfiguration**.</span></span> <span data-ttu-id="33cbc-115">Если вы не знакомы с **DbConfiguration**, см. в разделе [код конфигурации](~/ef6/fundamentals/configuring/code-based.md).</span><span class="sxs-lookup"><span data-stu-id="33cbc-115">If you are unfamiliar with **DbConfiguration**, see [Code Based Configuration](~/ef6/fundamentals/configuring/code-based.md).</span></span> <span data-ttu-id="33cbc-116">Эта функция может использоваться в сочетании с автоматическими повторными попытками, предложенная нами в EF6, которые помогают в ситуации, в котором фактически не удалось зафиксировать транзакцию на сервере из-за временного сбоя:</span><span class="sxs-lookup"><span data-stu-id="33cbc-116">This feature can be used in combination with the automatic retries we introduced in EF6, which help in the situation in which the transaction actually failed to commit on the server due to a transient failure:</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

public class MyConfiguration : DbConfiguration  
{
  public MyConfiguration()  
  {  
    SetTransactionHandler(SqlProviderServices.ProviderInvariantName, () => new CommitFailureHandler());  
    SetExecutionStrategy(SqlProviderServices.ProviderInvariantName, () => new SqlAzureExecutionStrategy());  
  }  
}
```  

## <a name="how-transactions-are-tracked"></a><span data-ttu-id="33cbc-117">Способ отслеживания операций</span><span class="sxs-lookup"><span data-stu-id="33cbc-117">How transactions are tracked</span></span>  

<span data-ttu-id="33cbc-118">Когда эта функция включена, EF автоматически добавит новую таблицу в базе данных с именем **__Transactions**.</span><span class="sxs-lookup"><span data-stu-id="33cbc-118">When the feature is enabled, EF will automatically add a new table to the database called **__Transactions**.</span></span> <span data-ttu-id="33cbc-119">Новая строка вставляется в этой таблице, каждый раз, транзакция создается с EF и этой строки проверены на существование, если происходит сбой транзакции во время фиксации.</span><span class="sxs-lookup"><span data-stu-id="33cbc-119">A new row is inserted in this table every time a transaction is created by EF and that row is checked for existence if a transaction failure occurs during commit.</span></span>  

<span data-ttu-id="33cbc-120">Несмотря на то, что EF сделает максимум усилий, чтобы очистить строки из таблицы, когда они больше не нужны, таблицы могут увеличиваться, если приложение завершает работу преждевременно и по этой причине, которые могут потребоваться для очистки таблицы вручную в некоторых случаях.</span><span class="sxs-lookup"><span data-stu-id="33cbc-120">Although EF will do a best effort to prune rows from the table when they aren’t needed anymore, the table can grow if the application exits prematurely and for that reason you may need to purge the table manually in some cases.</span></span>  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a><span data-ttu-id="33cbc-121">Способ обработки сбоев с предыдущими версиями</span><span class="sxs-lookup"><span data-stu-id="33cbc-121">How to handle commit failures with previous Versions</span></span>

<span data-ttu-id="33cbc-122">Прежде чем EF 6.1 не механизм обработки сбоев в продукте EF.</span><span class="sxs-lookup"><span data-stu-id="33cbc-122">Before EF 6.1 there was not mechanism to handle commit failures in the EF product.</span></span> <span data-ttu-id="33cbc-123">Существует несколько способов работы с этой ситуации, которые могут применяться в предыдущих версиях EF6:</span><span class="sxs-lookup"><span data-stu-id="33cbc-123">There are several ways to dealing with this situation that can be applied to previous versions of EF6:</span></span>  

### <a name="option-1---do-nothing"></a><span data-ttu-id="33cbc-124">Вариант 1: не выполнять никаких действий</span><span class="sxs-lookup"><span data-stu-id="33cbc-124">Option 1 - Do nothing</span></span>  

<span data-ttu-id="33cbc-125">Вероятность сбоя подключения во время фиксации транзакции недостаточно, поэтому оно может быть приемлемым для вашего приложения, просто ошибкой, если данное состояние возникает, фактически.</span><span class="sxs-lookup"><span data-stu-id="33cbc-125">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>  

## <a name="option-2---use-the-database-to-reset-state"></a><span data-ttu-id="33cbc-126">Вариант 2: использование базы данных для сброса состояния</span><span class="sxs-lookup"><span data-stu-id="33cbc-126">Option 2 - Use the database to reset state</span></span>  

1. <span data-ttu-id="33cbc-127">Отменить текущее DbContext</span><span class="sxs-lookup"><span data-stu-id="33cbc-127">Discard the current DbContext</span></span>  
2. <span data-ttu-id="33cbc-128">Создать новый DbContext и восстановить состояние приложения из базы данных</span><span class="sxs-lookup"><span data-stu-id="33cbc-128">Create a new DbContext and restore the state of your application from the database</span></span>  
3. <span data-ttu-id="33cbc-129">Информировать пользователей о том, что последняя операция не выполнена успешно</span><span class="sxs-lookup"><span data-stu-id="33cbc-129">Inform the user that the last operation might not have been completed successfully</span></span>  

## <a name="option-3---manually-track-the-transaction"></a><span data-ttu-id="33cbc-130">Вариант 3: вручную отслеживания транзакции</span><span class="sxs-lookup"><span data-stu-id="33cbc-130">Option 3 - Manually track the transaction</span></span>  

1. <span data-ttu-id="33cbc-131">Добавление таблицы не отслеживаются для базы данных, используемой для отслеживания состояния транзакций.</span><span class="sxs-lookup"><span data-stu-id="33cbc-131">Add a non-tracked table to the database used to track the status of the transactions.</span></span>  
2. <span data-ttu-id="33cbc-132">Вставьте строку в таблице в начале каждой транзакции.</span><span class="sxs-lookup"><span data-stu-id="33cbc-132">Insert a row into the table at the beginning of each transaction.</span></span>  
3. <span data-ttu-id="33cbc-133">В случае сбоя подключения во время фиксации, проверьте наличие соответствующей строки в базе данных.</span><span class="sxs-lookup"><span data-stu-id="33cbc-133">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>  
    - <span data-ttu-id="33cbc-134">При наличии строки выполняться как обычно, так как транзакция была успешно зафиксирована</span><span class="sxs-lookup"><span data-stu-id="33cbc-134">If the row is present, continue normally, as the transaction was committed successfully</span></span>  
    - <span data-ttu-id="33cbc-135">Если строки отсутствует, используйте стратегию выполнения повторить попытку текущей операции.</span><span class="sxs-lookup"><span data-stu-id="33cbc-135">If the row is absent, use an execution strategy to retry the current operation.</span></span>  
4. <span data-ttu-id="33cbc-136">Если фиксация выполнена успешно, удалите соответствующей строки во избежание увеличение размера таблицы.</span><span class="sxs-lookup"><span data-stu-id="33cbc-136">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>  

<span data-ttu-id="33cbc-137">[В этой записи блога](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) содержит пример кода для решения этой проблемы на SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="33cbc-137">[This blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contains sample code for accomplishing this on SQL Azure.</span></span>  
