---
title: Загрузка связанных данных — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: d3a1810599771befb451715d93454fff63949771
ms.sourcegitcommit: 31536e52b838a84680d2e93e5bb52fb16df72a97
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2020
ms.locfileid: "86238311"
---
# <a name="loading-related-data"></a><span data-ttu-id="3a3d2-102">Загрузка связанных данных</span><span class="sxs-lookup"><span data-stu-id="3a3d2-102">Loading Related Data</span></span>

<span data-ttu-id="3a3d2-103">Entity Framework Core позволяет использовать свойства навигации в модели для загрузки связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="3a3d2-104">Для загрузки связанных данных используются три общих шаблона объектно-реляционного сопоставления (O/RM).</span><span class="sxs-lookup"><span data-stu-id="3a3d2-104">There are three common O/RM patterns used to load related data.</span></span>

* <span data-ttu-id="3a3d2-105">**Безотложная загрузка** означает, что данные загружены из базы данных как часть исходного запроса.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="3a3d2-106">**Явная загрузка** означает, что связанные данные явно загружаются из базы данных позже.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="3a3d2-107">**Отложенная загрузка** означает, что связанные данные прозрачно загружаются из базы данных при доступе к свойству навигации.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="3a3d2-108">Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-108">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="3a3d2-109">Безотложная загрузка</span><span class="sxs-lookup"><span data-stu-id="3a3d2-109">Eager loading</span></span>

<span data-ttu-id="3a3d2-110">Вы можете использовать метод `Include`, чтобы указать связанные данные, которые будут включены в результаты запроса.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="3a3d2-111">В следующем примере блоги, возвращенные в результатах, будут иметь свойство `Posts`, заполненное соответствующими записями.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="3a3d2-112">EF Core автоматически исправляет свойства навигации для других сущностей, которые были ранее загружены в экземпляр контекста.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="3a3d2-113">Даже если данные для свойства навигации не включены явно, свойство все равно можно заполнить при условии, что ранее были загружены некоторые или все связанные сущности.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

<span data-ttu-id="3a3d2-114">Связанные данные из нескольких связей можно включить в один запрос.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="3a3d2-115">Включение нескольких уровней</span><span class="sxs-lookup"><span data-stu-id="3a3d2-115">Including multiple levels</span></span>

<span data-ttu-id="3a3d2-116">Вы можете детализировать отношения, чтобы включить несколько уровней связанных данных, используя метод `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-116">You can drill down through relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="3a3d2-117">В следующем примере загружаются все блоги, связанные с ними записи и автор каждой записи.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleThenInclude)]

<span data-ttu-id="3a3d2-118">Вы можете связать в цепочку несколько вызовов `ThenInclude`, чтобы продолжить включение уровней связанных данных.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-118">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="3a3d2-119">Вы можете объединить все это, чтобы включить связанные данные из нескольких уровней и нескольких корней в один и тот же запрос.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-119">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="3a3d2-120">Может потребоваться включить несколько связанных сущностей для одной включенной сущности.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-120">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="3a3d2-121">Например, при запросе `Blogs` вы включаете `Posts`, а затем хотите включить `Author` и `Tags` из `Posts`.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-121">For example, when querying `Blogs`, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="3a3d2-122">Для этого вам нужно указать каждый путь включения, начиная с корня.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-122">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="3a3d2-123">Например, `Blog -> Posts -> Author` и `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-123">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="3a3d2-124">Это не означает, что вы получите избыточные соединения, в большинстве случаев EF будет консолидировать соединения при создании SQL.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-124">This does not mean you will get redundant joins; in most cases, EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="single-and-split-queries"></a><span data-ttu-id="3a3d2-125">Отдельные и разделенные запросы</span><span class="sxs-lookup"><span data-stu-id="3a3d2-125">Single and split queries</span></span>

> [!NOTE]
> <span data-ttu-id="3a3d2-126">Эта возможность появилась в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-126">This feature is introduced in EF Core 5.0.</span></span>

<span data-ttu-id="3a3d2-127">В реляционных базах данных все связанные сущности по умолчанию загружаются с помощью запросов JOIN:</span><span class="sxs-lookup"><span data-stu-id="3a3d2-127">In relational databases, all related entities are by default loaded by introducing JOINs:</span></span>

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Post] AS [p] ON [b].[BlogId] = [p].[BlogId]
ORDER BY [b].[BlogId], [p].[PostId]
```

<span data-ttu-id="3a3d2-128">Если в обычном блоге есть несколько связанных записей, в строках для таких записей будут дублироваться сведения о блоге, что приведет к возникновению так называемой проблемы картезианского взрыва.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-128">If a typical blog has multiple related posts, rows for these posts will duplicate the blog's information, leading to the so-called "cartesian explosion" problem.</span></span> <span data-ttu-id="3a3d2-129">Чем больше связей "один ко многим" будет загружено, тем большим будет объем дублирующихся данных, что негативно повлияет на производительность приложения.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-129">As more one-to-many relationships are loaded, the amount of duplicated data may grow and adversely affect the performance of your application.</span></span>

<span data-ttu-id="3a3d2-130">EF позволяет вам указать, что конкретный запрос LINQ нужно *разделить* на несколько SQL-запросов.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-130">EF allows you to specify that a given LINQ query should be *split* into multiple SQL queries.</span></span> <span data-ttu-id="3a3d2-131">Вместо запросов JOIN разделенные запросы выполняют дополнительный SQL-запрос для каждого включенного перехода "один ко многим":</span><span class="sxs-lookup"><span data-stu-id="3a3d2-131">Instead of JOINs, split queries perform an additional SQL query for each included one-to-many navigation:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs?name=AsSplitQuery&highlight=5)]

<span data-ttu-id="3a3d2-132">Будет создан следующий SQL-запрос:</span><span class="sxs-lookup"><span data-stu-id="3a3d2-132">This will produce the following SQL:</span></span>

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM [Blogs] AS [b]
ORDER BY [b].[BlogId]

SELECT [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title], [b].[BlogId]
FROM [Blogs] AS [b]
INNER JOIN [Post] AS [p] ON [b].[BlogId] = [p].[BlogId]
ORDER BY [b].[BlogId]
```

<span data-ttu-id="3a3d2-133">Хотя это позволяет избежать проблем с производительностью, связанных с запросами JOIN и картезианским взрывом, этот способ также имеет некоторые недостатки:</span><span class="sxs-lookup"><span data-stu-id="3a3d2-133">While this avoids the performance issues associated with JOINs and cartesian explosion, it also has some drawbacks:</span></span>

* <span data-ttu-id="3a3d2-134">Большинство баз данных обеспечивают согласованность данных для отдельных запросов, но не для нескольких запросов.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-134">While most databases guarantee data consistency for single queries, no such guarantees exist for multiple queries.</span></span> <span data-ttu-id="3a3d2-135">Это означает, что, если при выполнении запросов также одновременно выполняется обновление базы данных, итоговые данные могут быть несогласованны.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-135">This means that if the database is being updated concurrently as your queries are being executed, resulting data may not be consistent.</span></span> <span data-ttu-id="3a3d2-136">Чтобы решить такую проблему, вы можете включить запросы в сериализуемую транзакцию или транзакцию моментального снимка, хотя это тоже может ухудшить производительность.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-136">This may be mitigated by wrapping the queries in a serializable or snapshot transaction, although this may create performance issues of its own.</span></span> <span data-ttu-id="3a3d2-137">Дополнительные сведения см. в документации по вашей базе данных.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-137">Consult your database's documentation for more details.</span></span>
* <span data-ttu-id="3a3d2-138">Сейчас выполнение каждого запроса подразумевает запуск дополнительного цикла обмена данными с базой данных по сети. Это может ухудшить производительность, особенно при высокой задержке при обращении к базе данных (например, в облачных службах).</span><span class="sxs-lookup"><span data-stu-id="3a3d2-138">Each query currently implies an additional network roundtrip to your database; this can degrade performance, especially where latency to the database is high (e.g. cloud services).</span></span> <span data-ttu-id="3a3d2-139">В будущем этот процесс в EF Core будет улучшен благодаря пакетной отправке запросов в рамках одного цикла обмена данными.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-139">EF Core will improve this in the future by batching the queries into a single roundtrip.</span></span>
* <span data-ttu-id="3a3d2-140">Хотя некоторые базы данных позволяют одновременно принимать результаты выполнения нескольких запросов (SQL Server с MARS, Sqlite), большинство из них разрешают выполнение только одного запроса на определенный момент времени.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-140">While some databases allow consuming the results of multiple queries at the same time (SQL Server with MARS, Sqlite), most allow only a single query to be active at any given point.</span></span> <span data-ttu-id="3a3d2-141">Это означает, что до выполнения последующих запросов все результаты выполненных ранее запросов должны буферизоваться в памяти приложения, что может значительно увеличивать требования к памяти.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-141">This means that all results from earlier queries must be buffered in your application's memory before executing later queries, increasing your memory requirements in a potentially significant way.</span></span>

<span data-ttu-id="3a3d2-142">К сожалению, единой стратегии загрузки связанных сущностей, которая подходит для всех сценариев, не существует.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-142">Unfortunately, there isn't one strategy for loading related entities that fits all scenarios.</span></span> <span data-ttu-id="3a3d2-143">Взвешенно оцените преимущества и недостатки использования отдельных и разделенных запросов и выберите оптимальный подход.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-143">Carefully consider the advantages and disadvantages of single and split queries, and select the one that fits your needs.</span></span>

> [!NOTE]
> <span data-ttu-id="3a3d2-144">Сущности со связями "один к одному" всегда загружаются с помощью запросов JOIN, так как они не влияют на производительность.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-144">One-to-one related entities are always loaded via JOINs, as this has no performance impact.</span></span>
>
> <span data-ttu-id="3a3d2-145">На данный момент использование разделения запросов в SQL Server требует настройки параметра `MultipleActiveResultSets=true` в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-145">At the moment, use of query splitting on SQL Server requires settings `MultipleActiveResultSets=true` in your connection string.</span></span> <span data-ttu-id="3a3d2-146">В будущей предварительной версии это требование будет отменено.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-146">This requirement will be removed in a future preview.</span></span>
>
> <span data-ttu-id="3a3d2-147">В будущих предварительных версиях EF Core 5.0 вы сможете задать разделение запросов в качестве поведения по умолчанию для вашего контекста.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-147">Future previews of EF Core 5.0 will allow specifying query splitting as the default for your context.</span></span>

### <a name="filtered-include"></a><span data-ttu-id="3a3d2-148">Включение с фильтрацией</span><span class="sxs-lookup"><span data-stu-id="3a3d2-148">Filtered include</span></span>

> [!NOTE]
> <span data-ttu-id="3a3d2-149">Эта возможность появилась в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-149">This feature is introduced in EF Core 5.0.</span></span>

<span data-ttu-id="3a3d2-150">При применении Include для загрузки связанных данных можно применить определенные перечислимые операции к включенной навигации коллекции, что позволяет фильтровать и сортировать результаты.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-150">When applying Include to load related data, you can apply certain enumerable operations on the included collection navigation, which allows for filtering and sorting of the results.</span></span>

<span data-ttu-id="3a3d2-151">Поддерживаются операции: `Where`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `Skip` и `Take`.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-151">Supported operations are: `Where`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `Skip`, and `Take`.</span></span>

<span data-ttu-id="3a3d2-152">Такие операции должны применяться к навигации коллекции в лямбда-выражении, переданном в метод Include, как показано в примере ниже:</span><span class="sxs-lookup"><span data-stu-id="3a3d2-152">Such operations should be applied on the collection navigation in the lambda passed to the Include method, as shown in example below:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#FilteredInclude)]

<span data-ttu-id="3a3d2-153">Каждая включаемая навигация позволяет выполнять только один уникальный набор операций фильтра.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-153">Each included navigation allows only one unique set of filter operations.</span></span> <span data-ttu-id="3a3d2-154">В тех случаях, когда для данной навигации по коллекции применяются несколько операций Include (`blog.Posts` в приведенных ниже примерах), операции фильтрации можно указывать только в одном из них:</span><span class="sxs-lookup"><span data-stu-id="3a3d2-154">In cases where multiple Include operations are applied for a given collection navigation (`blog.Posts` in the examples below), filter operations can only be specified on one of them:</span></span> 

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleLeafIncludesFiltered1)]

<span data-ttu-id="3a3d2-155">Кроме того, можно применить идентичные операции для каждой навигации, которая включается несколько раз:</span><span class="sxs-lookup"><span data-stu-id="3a3d2-155">Alternatively, identical operations can be applied for each navigation that is included multiple times:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleLeafIncludesFiltered2)]

> [!CAUTION]
> <span data-ttu-id="3a3d2-156">При отслеживании запросов результаты включения с фильтрацией могут быть неожиданными из-за [исправления навигации](tracking.md).</span><span class="sxs-lookup"><span data-stu-id="3a3d2-156">In case of tracking queries, results of Filtered Include may be unexpected due to [navigation fixup](tracking.md).</span></span> <span data-ttu-id="3a3d2-157">Все связанные сущности, которые были запрошены ранее и сохранены в средстве отслеживания изменений, будут представлены в результатах запроса включения с фильтрацией, даже если они не соответствуют требованиям фильтра.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-157">All relevant entities that have been querried for previously and have been stored in the Change Tracker will be present in the results of Filtered Include query, even if they don't meet the requirements of the filter.</span></span> <span data-ttu-id="3a3d2-158">При использовании в таких ситуациях включения с фильтрацией рассмотрите возможность работы с запросами `NoTracking` или повторного создания DbContext.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-158">Consider using `NoTracking` queries or re-create the DbContext when using Filtered Include in those situations.</span></span>

<span data-ttu-id="3a3d2-159">Пример.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-159">Example:</span></span>

```csharp
var orders = context.Orders.Where(o => o.Id > 1000).ToList();

// customer entities will have references to all orders where Id > 1000, rathat than > 5000
var filtered = context.Customers.Include(c => c.Orders.Where(o => o.Id > 5000)).ToList();
```

### <a name="include-on-derived-types"></a><span data-ttu-id="3a3d2-160">Использование метода Include с производными типами</span><span class="sxs-lookup"><span data-stu-id="3a3d2-160">Include on derived types</span></span>

<span data-ttu-id="3a3d2-161">Вы можете включать связанные данные из навигаций, определенных только с производным типом, используя `Include` и `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-161">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span>

<span data-ttu-id="3a3d2-162">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="3a3d2-162">Given the following model:</span></span>

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

<span data-ttu-id="3a3d2-163">Содержимое навигации `School` всех людей, которые являются учениками, может быть активно загружено с помощью нескольких шаблонов:</span><span class="sxs-lookup"><span data-stu-id="3a3d2-163">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

* <span data-ttu-id="3a3d2-164">С помощью приведения.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-164">using cast</span></span>

  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

* <span data-ttu-id="3a3d2-165">С помощью оператора `as`.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-165">using `as` operator</span></span>

  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

* <span data-ttu-id="3a3d2-166">С помощью перегрузки `Include`, принимающий параметр типа `string`.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-166">using overload of `Include` that takes parameter of type `string`</span></span>

  ```csharp
  context.People.Include("School").ToList()
  ```

## <a name="explicit-loading"></a><span data-ttu-id="3a3d2-167">Явная загрузка</span><span class="sxs-lookup"><span data-stu-id="3a3d2-167">Explicit loading</span></span>

<span data-ttu-id="3a3d2-168">Вы можете явно загрузить свойство навигации с помощью API `DbContext.Entry(...)`.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-168">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="3a3d2-169">Вы также можете явно загрузить свойство навигации, выполнив отдельный запрос, который возвращает связанные сущности.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-169">You can also explicitly load a navigation property by executing a separate query that returns the related entities.</span></span> <span data-ttu-id="3a3d2-170">Если отслеживание изменений включено, то при загрузке сущности EF Core автоматически установит свойства навигации вновь загруженной сущности для ссылки на любые уже загруженные сущности и задаст свойства навигации уже загруженных сущностей, чтобы ссылаться на вновь загруженные сущности.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-170">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="3a3d2-171">Запрос связанных сущностей</span><span class="sxs-lookup"><span data-stu-id="3a3d2-171">Querying related entities</span></span>

<span data-ttu-id="3a3d2-172">Можно также получить запрос LINQ, который представляет содержимое свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-172">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="3a3d2-173">Это позволяет выполнять такие задачи, как запуск оператора агрегирования в связанных сущностях без их загрузки в память.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-173">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="3a3d2-174">Можно также фильтровать связанные сущности, которые загружаются в память.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-174">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="3a3d2-175">Отложенная загрузка</span><span class="sxs-lookup"><span data-stu-id="3a3d2-175">Lazy loading</span></span>

<span data-ttu-id="3a3d2-176">Самый простой способ использовать отложенную загрузку — установить пакет [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) и включить его с помощью вызова `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-176">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="3a3d2-177">Пример:</span><span class="sxs-lookup"><span data-stu-id="3a3d2-177">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```

<span data-ttu-id="3a3d2-178">При использовании AddDbContext:</span><span class="sxs-lookup"><span data-stu-id="3a3d2-178">Or when using AddDbContext:</span></span>

```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```

<span data-ttu-id="3a3d2-179">Затем EF Core включит отложенную загрузку любого свойства навигации, которое может быть переопределено, то есть оно должно быть типа `virtual` и быть в классе, который может быть унаследован.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-179">EF Core will then enable lazy loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="3a3d2-180">Например, в следующих сущностях свойства навигации `Post.Blog` и `Blog.Posts` будут загружены отложено.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-180">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>

```csharp
public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public virtual Blog Blog { get; set; }
}
```

### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="3a3d2-181">Отложенная загрузка без прокси-серверов</span><span class="sxs-lookup"><span data-stu-id="3a3d2-181">Lazy loading without proxies</span></span>

<span data-ttu-id="3a3d2-182">Прокси-серверы с отложенной загрузкой работают, внедряя службу `ILazyLoader` в сущность, как описано в статье [Entity Type Constructors](../modeling/constructors.md) (Конструкторы типов сущностей).</span><span class="sxs-lookup"><span data-stu-id="3a3d2-182">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="3a3d2-183">Пример:</span><span class="sxs-lookup"><span data-stu-id="3a3d2-183">For example:</span></span>

```csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```

<span data-ttu-id="3a3d2-184">Не требуется, чтобы наследуемые типы сущностей или свойства навигации были виртуальными, и экземплярам сущностей, созданным с помощью `new`, можно отложено загружаться после прикрепления к контексту.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-184">This doesn't require entity types to be inherited from or navigation properties to be virtual, and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="3a3d2-185">Однако для этого требуется ссылка на службу `ILazyLoader`, которая определена в пакете [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/).</span><span class="sxs-lookup"><span data-stu-id="3a3d2-185">However, it requires a reference to the `ILazyLoader` service, which is defined in the [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) package.</span></span> <span data-ttu-id="3a3d2-186">Этот пакет содержит минимальный набор типов, поэтому он не оказывает большого влияния.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-186">This package contains a minimal set of types so that there is very little impact in depending on it.</span></span> <span data-ttu-id="3a3d2-187">Тем не менее, чтобы полностью избежать зависимости от любых пакетов EF Core в типах сущностей, можно ввести метод `ILazyLoader.Load` в качестве делегата.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-187">However, to completely avoid depending on any EF Core packages in the entity types, it is possible to inject the `ILazyLoader.Load` method as a delegate.</span></span> <span data-ttu-id="3a3d2-188">Пример:</span><span class="sxs-lookup"><span data-stu-id="3a3d2-188">For example:</span></span>

```csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```

<span data-ttu-id="3a3d2-189">В приведенном выше коде используется метод расширения `Load`, чтобы упростить использование делегата:</span><span class="sxs-lookup"><span data-stu-id="3a3d2-189">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>

```csharp
public static class PocoLoadingExtensions
{
    public static TRelated Load<TRelated>(
        this Action<object, string> loader,
        object entity,
        ref TRelated navigationField,
        [CallerMemberName] string navigationName = null)
        where TRelated : class
    {
        loader?.Invoke(entity, navigationName);

        return navigationField;
    }
}
```

> [!NOTE]  
> <span data-ttu-id="3a3d2-190">Параметр конструктора для делегата с отложенной загрузкой должен называться "lazyLoader".</span><span class="sxs-lookup"><span data-stu-id="3a3d2-190">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="3a3d2-191">Конфигурация для использования отличного от этого имени запланирована в будущей версии.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-191">Configuration to use a different name than this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="3a3d2-192">Связанные данные и сериализация</span><span class="sxs-lookup"><span data-stu-id="3a3d2-192">Related data and serialization</span></span>

<span data-ttu-id="3a3d2-193">Так как EF Core автоматически исправляет свойства навигации, вы можете завершить циклы в графе объектов.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-193">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="3a3d2-194">Например, загрузка блога и связанных записей приведет к созданию объекта блога, который ссылается на коллекцию записей.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-194">For example, loading a blog and its related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="3a3d2-195">Каждая из этих записей будет ссылкой на блог.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-195">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="3a3d2-196">Некоторые платформы сериализации не допускают такие циклы.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-196">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="3a3d2-197">Например Json.NET вызовет следующее исключение при обнаружении цикла.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-197">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="3a3d2-198">Newtonsoft.Json.JsonSerializationException: обнаружен самоссылающийся цикл для свойства "Blog" с типом "MyApplication.Models.Blog".</span><span class="sxs-lookup"><span data-stu-id="3a3d2-198">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="3a3d2-199">Если вы используете ASP.NET Core, вы можете настроить Json.NET для игнорирования циклов, которые он находит в графе объектов.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-199">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="3a3d2-200">Это делается в методе `ConfigureServices(...)` в `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-200">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```

<span data-ttu-id="3a3d2-201">Другой вариант — дополнение одного из свойств навигации атрибутом `[JsonIgnore]`, который указывает Json.NET игнорировать это свойство навигации во время сериализации.</span><span class="sxs-lookup"><span data-stu-id="3a3d2-201">Another alternative is to decorate one of the navigation properties with the `[JsonIgnore]` attribute, which instructs Json.NET to not traverse that navigation property while serializing.</span></span>
