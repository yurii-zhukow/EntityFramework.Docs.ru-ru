---
title: Сравнение одиночных и разделенных запросов в EF Core
description: Преобразование запроса в одиночный и разделенный запросы в SQL с помощью Entity Framework Core
author: smitpatel
ms.date: 10/03/2019
uid: core/querying/single-split-queries
ms.openlocfilehash: 1c99d931c01b99de199710ffe661e1aac7a37263
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94431204"
---
# <a name="single-vs-split-queries"></a><span data-ttu-id="de853-103">Сравнение одиночных и разделенных запросов</span><span class="sxs-lookup"><span data-stu-id="de853-103">Single vs. split queries</span></span>

## <a name="single-queries"></a><span data-ttu-id="de853-104">Отдельные запросы</span><span class="sxs-lookup"><span data-stu-id="de853-104">Single queries</span></span>

<span data-ttu-id="de853-105">В реляционных базах данных все связанные сущности загружаются с помощью запросов JOIN в одиночном запросе.</span><span class="sxs-lookup"><span data-stu-id="de853-105">In relational databases, all related entities are loaded by introducing JOINs in single query.</span></span>

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Post] AS [p] ON [b].[BlogId] = [p].[BlogId]
ORDER BY [b].[BlogId], [p].[PostId]
```

<span data-ttu-id="de853-106">Если в обычном блоге есть несколько связанных записей, тогда строки для этих записей будут дублировать данные блога.</span><span class="sxs-lookup"><span data-stu-id="de853-106">If a typical blog has multiple related posts, rows for these posts will duplicate the blog's information.</span></span> <span data-ttu-id="de853-107">Такое дублирование приводит к возникновению "картезианского взрыва" (нарушение корректности синхронизации).</span><span class="sxs-lookup"><span data-stu-id="de853-107">This duplication leads to the so-called "cartesian explosion" problem.</span></span> <span data-ttu-id="de853-108">Чем больше связей "один ко многим" будет загружено, тем большим будет объем дублирующихся данных, что негативно повлияет на производительность приложения.</span><span class="sxs-lookup"><span data-stu-id="de853-108">As more one-to-many relationships are loaded, the amount of duplicated data may grow and adversely affect the performance of your application.</span></span>

## <a name="split-queries"></a><span data-ttu-id="de853-109">Разделенные запросы</span><span class="sxs-lookup"><span data-stu-id="de853-109">Split queries</span></span>

> [!NOTE]
> <span data-ttu-id="de853-110">Эта возможность появилась в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="de853-110">This feature is introduced in EF Core 5.0.</span></span> <span data-ttu-id="de853-111">Это работает только при использовании `Include`.</span><span class="sxs-lookup"><span data-stu-id="de853-111">It only works when using `Include`.</span></span> <span data-ttu-id="de853-112">[Эта проблема](https://github.com/dotnet/efcore/issues/21234) заключается в поддержке отслеживания разделенного запроса при загрузке связанных данных в проекции без `Include`.</span><span class="sxs-lookup"><span data-stu-id="de853-112">[This issue](https://github.com/dotnet/efcore/issues/21234) is tracking support for split query when loading related data in projection without `Include`.</span></span>

<span data-ttu-id="de853-113">EF позволяет вам указать, что конкретный запрос LINQ нужно *разделить* на несколько SQL-запросов.</span><span class="sxs-lookup"><span data-stu-id="de853-113">EF allows you to specify that a given LINQ query should be *split* into multiple SQL queries.</span></span> <span data-ttu-id="de853-114">Вместо запросов JOIN разделенные запросы создают дополнительный SQL-запрос для каждой включенной навигации по коллекции:</span><span class="sxs-lookup"><span data-stu-id="de853-114">Instead of JOINs, split queries generate an additional SQL query for each included collection navigation:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Program.cs?name=AsSplitQuery&highlight=5)]

<span data-ttu-id="de853-115">Будет создан следующий SQL-запрос:</span><span class="sxs-lookup"><span data-stu-id="de853-115">It will produce the following SQL:</span></span>

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM [Blogs] AS [b]
ORDER BY [b].[BlogId]

SELECT [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title], [b].[BlogId]
FROM [Blogs] AS [b]
INNER JOIN [Post] AS [p] ON [b].[BlogId] = [p].[BlogId]
ORDER BY [b].[BlogId]
```

> [!NOTE]
> <span data-ttu-id="de853-116">Сущности со связями "один к одному" всегда загружаются с помощью запросов JOIN в одном запросе, так как они не влияют на производительность.</span><span class="sxs-lookup"><span data-stu-id="de853-116">One-to-one related entities are always loaded via JOINs in the same query, as it has no performance impact.</span></span>

## <a name="enabling-split-queries-globally"></a><span data-ttu-id="de853-117">Глобальное включение разделения запросов</span><span class="sxs-lookup"><span data-stu-id="de853-117">Enabling split queries globally</span></span>

<span data-ttu-id="de853-118">Можно также настроить разделение запросов по умолчанию для контекста приложения.</span><span class="sxs-lookup"><span data-stu-id="de853-118">You can also configure split queries as the default for your application's context:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/SplitQueriesBloggingContext.cs?name=QuerySplittingBehaviorSplitQuery&highlight=6)]

<span data-ttu-id="de853-119">Если разделение запросов настроено по умолчанию, можно по-прежнему настроить определенные запросы для выполнения в виде отдельных запросов.</span><span class="sxs-lookup"><span data-stu-id="de853-119">When split queries are configured as the default, it's still possible to configure specific queries to execute as single queries:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Program.cs?name=AsSingleQuery&highlight=5)]

<span data-ttu-id="de853-120">EF Core по умолчанию использует режим одиночных запросов без всякой конфигурации.</span><span class="sxs-lookup"><span data-stu-id="de853-120">EF Core uses single query mode by default in the absence of any configuration.</span></span> <span data-ttu-id="de853-121">Поскольку это может вызвать проблемы с производительностью, EF Core выдает предупреждение при соблюдении следующих условий:</span><span class="sxs-lookup"><span data-stu-id="de853-121">Since it may cause performance issues, EF Core generates a warning whenever following conditions are met:</span></span>

- <span data-ttu-id="de853-122">EF Core обнаруживает, что запрос загружает несколько коллекций.</span><span class="sxs-lookup"><span data-stu-id="de853-122">EF Core detects that the query loads multiple collections.</span></span>
- <span data-ttu-id="de853-123">Пользователь не настроил режим разбиения запроса глобально.</span><span class="sxs-lookup"><span data-stu-id="de853-123">User hasn't configured query splitting mode globally.</span></span>
- <span data-ttu-id="de853-124">Пользователь не использовал в запросе оператор `AsSingleQuery`/`AsSplitQuery`.</span><span class="sxs-lookup"><span data-stu-id="de853-124">User hasn't used `AsSingleQuery`/`AsSplitQuery` operator on the query.</span></span>

<span data-ttu-id="de853-125">Чтобы отключить предупреждение, настройте режим разбиения запроса глобально или на уровне запроса, использовав соответствующее значение.</span><span class="sxs-lookup"><span data-stu-id="de853-125">To turn off the warning, configure query splitting mode globally or at the query level to an appropriate value.</span></span>

## <a name="characteristics-of-split-queries"></a><span data-ttu-id="de853-126">Характеристики разделенных запросов</span><span class="sxs-lookup"><span data-stu-id="de853-126">Characteristics of split queries</span></span>

<span data-ttu-id="de853-127">Хотя разделение запросов позволяет избежать проблем с производительностью, связанных с запросами JOIN и картезианским взрывом, этот способ также имеет некоторые недостатки.</span><span class="sxs-lookup"><span data-stu-id="de853-127">While split query avoids the performance issues associated with JOINs and cartesian explosion, it also has some drawbacks:</span></span>

- <span data-ttu-id="de853-128">Большинство баз данных обеспечивают согласованность данных для отдельных запросов, но не для нескольких запросов.</span><span class="sxs-lookup"><span data-stu-id="de853-128">While most databases guarantee data consistency for single queries, no such guarantees exist for multiple queries.</span></span> <span data-ttu-id="de853-129">Если при выполнении запросов параллельно выполняется обновление базы данных, результирующие данные могут оказаться несогласованными.</span><span class="sxs-lookup"><span data-stu-id="de853-129">If the database is updated concurrently when executing your queries, resulting data may not be consistent.</span></span> <span data-ttu-id="de853-130">Для предотвращения этого можно включить запросы в сериализуемую транзакцию или транзакцию моментального снимка, хотя это также может ухудшить производительность.</span><span class="sxs-lookup"><span data-stu-id="de853-130">You can mitigate it by wrapping the queries in a serializable or snapshot transaction, although doing so may create performance issues of its own.</span></span> <span data-ttu-id="de853-131">Дополнительные сведения см. в документации конкретной базы данных.</span><span class="sxs-lookup"><span data-stu-id="de853-131">For more information, see your database's documentation.</span></span>
- <span data-ttu-id="de853-132">В настоящее время каждый запрос подразумевает дополнительный сетевой цикл обмена данными с базой данных.</span><span class="sxs-lookup"><span data-stu-id="de853-132">Each query currently implies an additional network roundtrip to your database.</span></span> <span data-ttu-id="de853-133">Множественный сетевой обмен данными может привести к снижению производительности, особенно если задержка при обращении к базе данных высока (например, в случае облачных служб).</span><span class="sxs-lookup"><span data-stu-id="de853-133">Multiple network roundtrip can degrade performance, especially where latency to the database is high (for example, cloud services).</span></span>
- <span data-ttu-id="de853-134">Хотя некоторые базы данных позволяют одновременно принимать результаты выполнения нескольких запросов (SQL Server с MARS, Sqlite), большинство из них разрешают выполнение только одного запроса на определенный момент времени.</span><span class="sxs-lookup"><span data-stu-id="de853-134">While some databases allow consuming the results of multiple queries at the same time (SQL Server with MARS, Sqlite), most allow only a single query to be active at any given point.</span></span> <span data-ttu-id="de853-135">Поэтому перед выполнением последующих запросов все результаты предыдущих запросов необходимо поместить в буфер памяти приложения, что приводит к увеличению требований к памяти.</span><span class="sxs-lookup"><span data-stu-id="de853-135">So all results from earlier queries must be buffered in your application's memory before executing later queries, which leads to increased memory requirements.</span></span>

<span data-ttu-id="de853-136">К сожалению, единой стратегии загрузки связанных сущностей, которая подходит для всех сценариев, не существует.</span><span class="sxs-lookup"><span data-stu-id="de853-136">Unfortunately, there isn't one strategy for loading related entities that fits all scenarios.</span></span> <span data-ttu-id="de853-137">Взвешенно оцените преимущества и недостатки использования отдельных и разделенных запросов и выберите оптимальный подход.</span><span class="sxs-lookup"><span data-stu-id="de853-137">Carefully consider the advantages and disadvantages of single and split queries to select the one that fits your needs.</span></span>
