---
title: Безотложная загрузка связанных данных — EF Core
description: Безотложная загрузка связанных данных с помощью Entity Framework Core
author: roji
ms.date: 9/8/2020
uid: core/querying/related-data/eager
ms.openlocfilehash: 97ec45a0f8bfecce4d4a59e5d1c36c0268d96052
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92062580"
---
# <a name="eager-loading-of-related-data"></a><span data-ttu-id="8ac74-103">Безотложная загрузка связанных данных</span><span class="sxs-lookup"><span data-stu-id="8ac74-103">Eager Loading of Related Data</span></span>

## <a name="eager-loading"></a><span data-ttu-id="8ac74-104">Безотложная загрузка</span><span class="sxs-lookup"><span data-stu-id="8ac74-104">Eager loading</span></span>

<span data-ttu-id="8ac74-105">Вы можете использовать метод `Include`, чтобы указать связанные данные, которые будут включены в результаты запроса.</span><span class="sxs-lookup"><span data-stu-id="8ac74-105">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="8ac74-106">В следующем примере блоги, возвращенные в результатах, будут иметь свойство `Posts`, заполненное соответствующими записями.</span><span class="sxs-lookup"><span data-stu-id="8ac74-106">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#SingleInclude)]

> [!TIP]
> <span data-ttu-id="8ac74-107">EF Core автоматически исправляет свойства навигации для других сущностей, которые были ранее загружены в экземпляр контекста.</span><span class="sxs-lookup"><span data-stu-id="8ac74-107">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="8ac74-108">Даже если данные для свойства навигации не включены явно, свойство все равно можно заполнить при условии, что ранее были загружены некоторые или все связанные сущности.</span><span class="sxs-lookup"><span data-stu-id="8ac74-108">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

<span data-ttu-id="8ac74-109">Связанные данные из нескольких связей можно включить в один запрос.</span><span class="sxs-lookup"><span data-stu-id="8ac74-109">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#MultipleIncludes)]

## <a name="including-multiple-levels"></a><span data-ttu-id="8ac74-110">Включение нескольких уровней</span><span class="sxs-lookup"><span data-stu-id="8ac74-110">Including multiple levels</span></span>

<span data-ttu-id="8ac74-111">Вы можете детализировать отношения, чтобы включить несколько уровней связанных данных, используя метод `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="8ac74-111">You can drill down through relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="8ac74-112">В следующем примере загружаются все блоги, связанные с ними записи и автор каждой записи.</span><span class="sxs-lookup"><span data-stu-id="8ac74-112">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#SingleThenInclude)]

<span data-ttu-id="8ac74-113">Вы можете связать в цепочку несколько вызовов `ThenInclude`, чтобы продолжить включение уровней связанных данных.</span><span class="sxs-lookup"><span data-stu-id="8ac74-113">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#MultipleThenIncludes)]

<span data-ttu-id="8ac74-114">Вы можете объединить все эти вызовы, чтобы включить связанные данные из нескольких уровней и нескольких корней в один и тот же запрос.</span><span class="sxs-lookup"><span data-stu-id="8ac74-114">You can combine all of the calls to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#IncludeTree)]

<span data-ttu-id="8ac74-115">Может потребоваться включить несколько связанных сущностей для одной включенной сущности.</span><span class="sxs-lookup"><span data-stu-id="8ac74-115">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="8ac74-116">Например, при запросе `Blogs` вы включаете `Posts`, а затем хотите включить `Author` и `Tags` из `Posts`.</span><span class="sxs-lookup"><span data-stu-id="8ac74-116">For example, when querying `Blogs`, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="8ac74-117">Для включения обеих сущностей вам нужно указать каждый путь включения, начиная с корня.</span><span class="sxs-lookup"><span data-stu-id="8ac74-117">To include both, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="8ac74-118">Например, `Blog -> Posts -> Author` и `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="8ac74-118">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="8ac74-119">Это не означает, что вы получите избыточные соединения, в большинстве случаев EF будет комбинировать соединения при создании SQL.</span><span class="sxs-lookup"><span data-stu-id="8ac74-119">It doesn't mean you'll get redundant joins; in most cases, EF will combine the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#MultipleLeafIncludes)]

## <a name="single-and-split-queries"></a><span data-ttu-id="8ac74-120">Отдельные и разделенные запросы</span><span class="sxs-lookup"><span data-stu-id="8ac74-120">Single and split queries</span></span>

### <a name="single-queries"></a><span data-ttu-id="8ac74-121">Отдельные запросы</span><span class="sxs-lookup"><span data-stu-id="8ac74-121">Single queries</span></span>

<span data-ttu-id="8ac74-122">В реляционных базах данных все связанные сущности по умолчанию загружаются с помощью запросов JOIN:</span><span class="sxs-lookup"><span data-stu-id="8ac74-122">In relational databases, all related entities are by default loaded by introducing JOINs:</span></span>

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Post] AS [p] ON [b].[BlogId] = [p].[BlogId]
ORDER BY [b].[BlogId], [p].[PostId]
```

<span data-ttu-id="8ac74-123">Если в обычном блоге есть несколько связанных записей, в строках для таких записей будут дублироваться сведения о блоге, что приведет к возникновению так называемой проблемы картезианского взрыва.</span><span class="sxs-lookup"><span data-stu-id="8ac74-123">If a typical blog has multiple related posts, rows for these posts will duplicate the blog's information, leading to the so-called "cartesian explosion" problem.</span></span> <span data-ttu-id="8ac74-124">Чем больше связей "один ко многим" будет загружено, тем большим будет объем дублирующихся данных, что негативно повлияет на производительность приложения.</span><span class="sxs-lookup"><span data-stu-id="8ac74-124">As more one-to-many relationships are loaded, the amount of duplicated data may grow and adversely affect the performance of your application.</span></span> <span data-ttu-id="8ac74-125">По умолчанию EF Core выдает предупреждение, если обнаруживает запросы загрузки коллекций, которые могут вызвать проблемы с производительностью.</span><span class="sxs-lookup"><span data-stu-id="8ac74-125">By default, EF Core emits a warning if it detects queries loading collection includes that may cause performance issues.</span></span>

### <a name="split-queries"></a><span data-ttu-id="8ac74-126">Разделенные запросы</span><span class="sxs-lookup"><span data-stu-id="8ac74-126">Split queries</span></span>

> [!NOTE]
> <span data-ttu-id="8ac74-127">Эта возможность появилась в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="8ac74-127">This feature is introduced in EF Core 5.0.</span></span>

<span data-ttu-id="8ac74-128">EF позволяет вам указать, что конкретный запрос LINQ нужно *разделить* на несколько SQL-запросов.</span><span class="sxs-lookup"><span data-stu-id="8ac74-128">EF allows you to specify that a given LINQ query should be *split* into multiple SQL queries.</span></span> <span data-ttu-id="8ac74-129">Вместо запросов JOIN разделенные запросы выполняют дополнительный SQL-запрос для каждого включенного перехода "один ко многим":</span><span class="sxs-lookup"><span data-stu-id="8ac74-129">Instead of JOINs, split queries perform an additional SQL query for each included one-to-many navigation:</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs?name=AsSplitQuery&highlight=5)]

<span data-ttu-id="8ac74-130">Будет создан следующий SQL-запрос:</span><span class="sxs-lookup"><span data-stu-id="8ac74-130">It will produce the following SQL:</span></span>

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
> <span data-ttu-id="8ac74-131">Сущности со связями "один к одному" всегда загружаются с помощью запросов JOIN в одном запросе, так как они не влияют на производительность.</span><span class="sxs-lookup"><span data-stu-id="8ac74-131">One-to-one related entities are always loaded via JOINs in the same query, as this has no performance impact.</span></span>

### <a name="enabling-split-queries-globally"></a><span data-ttu-id="8ac74-132">Глобальное включение разделения запросов</span><span class="sxs-lookup"><span data-stu-id="8ac74-132">Enabling split queries globally</span></span>

<span data-ttu-id="8ac74-133">Можно также настроить разделение запросов по умолчанию для контекста приложения.</span><span class="sxs-lookup"><span data-stu-id="8ac74-133">You can also configure split queries as the default for your application's context:</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/SplitQueriesBloggingContext.cs?name=QuerySplittingBehaviorSplitQuery&highlight=6)]

<span data-ttu-id="8ac74-134">Если разделение запросов настроено по умолчанию, можно по-прежнему настроить определенные запросы для выполнения в виде отдельных запросов.</span><span class="sxs-lookup"><span data-stu-id="8ac74-134">When split queries are configured as the default, it is still possible to configure specific queries to execute as single queries:</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs?name=AsSingleQuery&highlight=5)]

<span data-ttu-id="8ac74-135">Если режим разделения запроса не указан явно (ни глобально, ни в запросе) и EF Core обнаруживает, что в отдельном запросе загружается несколько коллекций, то выдается предупреждение, чтобы привлечь внимание к потенциальным проблемам с производительностью.</span><span class="sxs-lookup"><span data-stu-id="8ac74-135">If the query splitting mode isn't explicitly specified - neither globally nor on the query - and EF Core detects that a single query loads multiple collection includes, a warning is emitted to draw attention to the potential resulting performance issues.</span></span> <span data-ttu-id="8ac74-136">Если установить режим запроса в значение SingleQuery, то предупреждение выдаваться не будет.</span><span class="sxs-lookup"><span data-stu-id="8ac74-136">Setting the query mode to SingleQuery will cause the warning not to be generated.</span></span>

### <a name="characteristics-of-split-queries"></a><span data-ttu-id="8ac74-137">Характеристики разделенных запросов</span><span class="sxs-lookup"><span data-stu-id="8ac74-137">Characteristics of split queries</span></span>

<span data-ttu-id="8ac74-138">Хотя разделение запросов позволяет избежать проблем с производительностью, связанных с запросами JOIN и картезианским взрывом, этот способ также имеет некоторые недостатки.</span><span class="sxs-lookup"><span data-stu-id="8ac74-138">While split query avoids the performance issues associated with JOINs and cartesian explosion, it also has some drawbacks:</span></span>

* <span data-ttu-id="8ac74-139">Большинство баз данных обеспечивают согласованность данных для отдельных запросов, но не для нескольких запросов.</span><span class="sxs-lookup"><span data-stu-id="8ac74-139">While most databases guarantee data consistency for single queries, no such guarantees exist for multiple queries.</span></span> <span data-ttu-id="8ac74-140">Если при выполнении запросов параллельно выполняется обновление базы данных, результирующие данные могут оказаться несогласованными.</span><span class="sxs-lookup"><span data-stu-id="8ac74-140">If the database is updated concurrently when executing your queries, resulting data may not be consistent.</span></span> <span data-ttu-id="8ac74-141">Для предотвращения этого можно включить запросы в сериализуемую транзакцию или транзакцию моментального снимка, хотя это также может ухудшить производительность.</span><span class="sxs-lookup"><span data-stu-id="8ac74-141">You can mitigate it by wrapping the queries in a serializable or snapshot transaction, although doing so may create performance issues of its own.</span></span> <span data-ttu-id="8ac74-142">Дополнительные сведения см. в документации конкретной базы данных.</span><span class="sxs-lookup"><span data-stu-id="8ac74-142">For more information, see your database's documentation.</span></span>
* <span data-ttu-id="8ac74-143">В настоящее время каждый запрос подразумевает дополнительный сетевой цикл обмена данными с базой данных.</span><span class="sxs-lookup"><span data-stu-id="8ac74-143">Each query currently implies an additional network roundtrip to your database.</span></span> <span data-ttu-id="8ac74-144">Множественный сетевой обмен данными может привести к снижению производительности, особенно если задержка при обращении к базе данных высока (например, в случае облачных служб).</span><span class="sxs-lookup"><span data-stu-id="8ac74-144">Multiple network roundtrip can degrade performance, especially where latency to the database is high (for example, cloud services).</span></span>
* <span data-ttu-id="8ac74-145">Хотя некоторые базы данных позволяют одновременно принимать результаты выполнения нескольких запросов (SQL Server с MARS, Sqlite), большинство из них разрешают выполнение только одного запроса на определенный момент времени.</span><span class="sxs-lookup"><span data-stu-id="8ac74-145">While some databases allow consuming the results of multiple queries at the same time (SQL Server with MARS, Sqlite), most allow only a single query to be active at any given point.</span></span> <span data-ttu-id="8ac74-146">Поэтому перед выполнением последующих запросов все результаты предыдущих запросов необходимо поместить в буфер памяти приложения, что приводит к увеличению требований к памяти.</span><span class="sxs-lookup"><span data-stu-id="8ac74-146">So all results from earlier queries must be buffered in your application's memory before executing later queries, which leads to increased memory requirements.</span></span>

<span data-ttu-id="8ac74-147">К сожалению, единой стратегии загрузки связанных сущностей, которая подходит для всех сценариев, не существует.</span><span class="sxs-lookup"><span data-stu-id="8ac74-147">Unfortunately, there isn't one strategy for loading related entities that fits all scenarios.</span></span> <span data-ttu-id="8ac74-148">Взвешенно оцените преимущества и недостатки использования отдельных и разделенных запросов и выберите оптимальный подход.</span><span class="sxs-lookup"><span data-stu-id="8ac74-148">Carefully consider the advantages and disadvantages of single and split queries, and select the one that fits your needs.</span></span>

## <a name="filtered-include"></a><span data-ttu-id="8ac74-149">Включение с фильтрацией</span><span class="sxs-lookup"><span data-stu-id="8ac74-149">Filtered include</span></span>

> [!NOTE]
> <span data-ttu-id="8ac74-150">Эта возможность появилась в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="8ac74-150">This feature is introduced in EF Core 5.0.</span></span>

<span data-ttu-id="8ac74-151">При применении Include для загрузки связанных данных можно применить определенные перечислимые операции к включенной навигации коллекции, что позволяет фильтровать и сортировать результаты.</span><span class="sxs-lookup"><span data-stu-id="8ac74-151">When applying Include to load related data, you can apply certain enumerable operations on the included collection navigation, which allows for filtering and sorting of the results.</span></span>

<span data-ttu-id="8ac74-152">Поддерживаются операции: `Where`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `Skip` и `Take`.</span><span class="sxs-lookup"><span data-stu-id="8ac74-152">Supported operations are: `Where`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `Skip`, and `Take`.</span></span>

<span data-ttu-id="8ac74-153">Такие операции должны применяться к навигации коллекции в лямбда-выражении, переданном в метод Include, как показано в примере ниже:</span><span class="sxs-lookup"><span data-stu-id="8ac74-153">Such operations should be applied on the collection navigation in the lambda passed to the Include method, as shown in example below:</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#FilteredInclude)]

<span data-ttu-id="8ac74-154">Каждая включаемая навигация позволяет выполнять только один уникальный набор операций фильтра.</span><span class="sxs-lookup"><span data-stu-id="8ac74-154">Each included navigation allows only one unique set of filter operations.</span></span> <span data-ttu-id="8ac74-155">В тех случаях, когда для данной навигации по коллекции применяются несколько операций Include (`blog.Posts` в приведенных ниже примерах), операции фильтрации можно указывать только в одном из них:</span><span class="sxs-lookup"><span data-stu-id="8ac74-155">In cases where multiple Include operations are applied for a given collection navigation (`blog.Posts` in the examples below), filter operations can only be specified on one of them:</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#MultipleLeafIncludesFiltered1)]

<span data-ttu-id="8ac74-156">Вместо этого можно применить идентичные операции для каждой навигации, которая включается несколько раз:</span><span class="sxs-lookup"><span data-stu-id="8ac74-156">Instead, identical operations can be applied for each navigation that is included multiple times:</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#MultipleLeafIncludesFiltered2)]

> [!CAUTION]
> <span data-ttu-id="8ac74-157">При отслеживании запросов результаты включения с фильтрацией могут быть неожиданными из-за [исправления навигации](xref:core/querying/tracking).</span><span class="sxs-lookup"><span data-stu-id="8ac74-157">In case of tracking queries, results of Filtered Include may be unexpected due to [navigation fixup](xref:core/querying/tracking).</span></span> <span data-ttu-id="8ac74-158">Все связанные сущности, которые были запрошены ранее и сохранены в средстве отслеживания изменений, будут представлены в результатах запроса включения с фильтрацией, даже если они не соответствуют требованиям фильтра.</span><span class="sxs-lookup"><span data-stu-id="8ac74-158">All relevant entities that have been queried for previously and have been stored in the Change Tracker will be present in the results of Filtered Include query, even if they don't meet the requirements of the filter.</span></span> <span data-ttu-id="8ac74-159">При использовании в таких ситуациях включения с фильтрацией рассмотрите возможность работы с запросами `NoTracking` или повторного создания DbContext.</span><span class="sxs-lookup"><span data-stu-id="8ac74-159">Consider using `NoTracking` queries or re-create the DbContext when using Filtered Include in those situations.</span></span>

<span data-ttu-id="8ac74-160">Пример.</span><span class="sxs-lookup"><span data-stu-id="8ac74-160">Example:</span></span>

```csharp
var orders = context.Orders.Where(o => o.Id > 1000).ToList();

// customer entities will have references to all orders where Id > 1000, rather than > 5000
var filtered = context.Customers.Include(c => c.Orders.Where(o => o.Id > 5000)).ToList();
```

## <a name="include-on-derived-types"></a><span data-ttu-id="8ac74-161">Использование метода Include с производными типами</span><span class="sxs-lookup"><span data-stu-id="8ac74-161">Include on derived types</span></span>

<span data-ttu-id="8ac74-162">Вы можете включать связанные данные из навигации, определенной только с производным типом, используя `Include` и `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="8ac74-162">You can include related data from navigation defined only on a derived type using `Include` and `ThenInclude`.</span></span>

<span data-ttu-id="8ac74-163">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="8ac74-163">Given the following model:</span></span>

```csharp
public class SchoolContext : DbContext
{
    public DbSet<Person> People { get; set; }
    public DbSet<School> Schools { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<School>().HasMany(s => s.Students).WithOne(s => s.School);
    }
}

public class Person
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public class Student : Person
{
    public School School { get; set; }
}

public class School
{
    public int Id { get; set; }
    public string Name { get; set; }

    public List<Student> Students { get; set; }
}
```

<span data-ttu-id="8ac74-164">Содержимое навигации `School` всех людей, которые являются учениками, может быть активно загружено с помощью нескольких шаблонов:</span><span class="sxs-lookup"><span data-stu-id="8ac74-164">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

* <span data-ttu-id="8ac74-165">С помощью приведения.</span><span class="sxs-lookup"><span data-stu-id="8ac74-165">using cast</span></span>

  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

* <span data-ttu-id="8ac74-166">С помощью оператора `as`.</span><span class="sxs-lookup"><span data-stu-id="8ac74-166">using `as` operator</span></span>

  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

* <span data-ttu-id="8ac74-167">С помощью перегрузки `Include`, принимающий параметр типа `string`.</span><span class="sxs-lookup"><span data-stu-id="8ac74-167">using overload of `Include` that takes parameter of type `string`</span></span>

  ```csharp
  context.People.Include("School").ToList()
  ```
