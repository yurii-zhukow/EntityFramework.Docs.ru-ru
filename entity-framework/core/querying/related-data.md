---
title: Загрузка связанных данных — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: 5f1fb9376300739ab0e306d9d60e7ec71aa2d2e7
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812655"
---
# <a name="loading-related-data"></a><span data-ttu-id="2c66d-102">Загрузка связанных данных</span><span class="sxs-lookup"><span data-stu-id="2c66d-102">Loading Related Data</span></span>

<span data-ttu-id="2c66d-103">Entity Framework Core позволяет использовать свойства навигации в модели для загрузки связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="2c66d-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="2c66d-104">Для загрузки связанных данных используются три общих шаблона объектно-реляционного сопоставления (O/RM).</span><span class="sxs-lookup"><span data-stu-id="2c66d-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="2c66d-105">**Безотложная загрузка** означает, что данные загружены из базы данных как часть исходного запроса.</span><span class="sxs-lookup"><span data-stu-id="2c66d-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="2c66d-106">**Явная загрузка** означает, что связанные данные явно загружаются из базы данных позже.</span><span class="sxs-lookup"><span data-stu-id="2c66d-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="2c66d-107">**Отложенная загрузка** означает, что связанные данные прозрачно загружаются из базы данных при доступе к свойству навигации.</span><span class="sxs-lookup"><span data-stu-id="2c66d-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="2c66d-108">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="2c66d-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="2c66d-109">Безотложная загрузка</span><span class="sxs-lookup"><span data-stu-id="2c66d-109">Eager loading</span></span>

<span data-ttu-id="2c66d-110">Вы можете использовать метод `Include`, чтобы указать связанные данные, которые будут включены в результаты запроса.</span><span class="sxs-lookup"><span data-stu-id="2c66d-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="2c66d-111">В следующем примере блоги, возвращенные в результатах, будут иметь свойство `Posts`, заполненное соответствующими записями.</span><span class="sxs-lookup"><span data-stu-id="2c66d-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="2c66d-112">EF Core автоматически исправляет свойства навигации для других сущностей, которые были ранее загружены в экземпляр контекста.</span><span class="sxs-lookup"><span data-stu-id="2c66d-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="2c66d-113">Даже если данные для свойства навигации не включены явно, свойство все равно можно заполнить при условии, что ранее были загружены некоторые или все связанные сущности.</span><span class="sxs-lookup"><span data-stu-id="2c66d-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="2c66d-114">Связанные данные из нескольких связей можно включить в один запрос.</span><span class="sxs-lookup"><span data-stu-id="2c66d-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="2c66d-115">Включение нескольких уровней</span><span class="sxs-lookup"><span data-stu-id="2c66d-115">Including multiple levels</span></span>

<span data-ttu-id="2c66d-116">Вы можете детализировать отношения, чтобы включить несколько уровней связанных данных, используя метод `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="2c66d-116">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="2c66d-117">В следующем примере загружаются все блоги, связанные с ними записи и автор каждой записи.</span><span class="sxs-lookup"><span data-stu-id="2c66d-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="2c66d-118">Текущие версии Visual Studio предлагают неправильные варианты завершения кода и могут быть причиной того, что правильные выражения могут быть помечены как содержащие синтаксические ошибки при использовании метода `ThenInclude` после свойства навигации коллекции.</span><span class="sxs-lookup"><span data-stu-id="2c66d-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="2c66d-119">Это признак ошибки IntelliSense, отслеживаемой в https://github.com/dotnet/roslyn/issues/8237.</span><span class="sxs-lookup"><span data-stu-id="2c66d-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="2c66d-120">Эти ложные синтаксические ошибки можно игнорировать, если код верен и может быть успешно скомпилирован.</span><span class="sxs-lookup"><span data-stu-id="2c66d-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="2c66d-121">Вы можете связать в цепочку несколько вызовов `ThenInclude`, чтобы продолжить включение уровней связанных данных.</span><span class="sxs-lookup"><span data-stu-id="2c66d-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="2c66d-122">Вы можете объединить все это, чтобы включить связанные данные из нескольких уровней и нескольких корней в один и тот же запрос.</span><span class="sxs-lookup"><span data-stu-id="2c66d-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="2c66d-123">Может потребоваться включить несколько связанных сущностей для одной включенной сущности.</span><span class="sxs-lookup"><span data-stu-id="2c66d-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="2c66d-124">Например, при запросе `Blog` вы включаете `Posts`, а затем хотите включить `Author` и `Tags` из `Posts`.</span><span class="sxs-lookup"><span data-stu-id="2c66d-124">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="2c66d-125">Для этого вам нужно указать каждый путь включения, начиная с корня.</span><span class="sxs-lookup"><span data-stu-id="2c66d-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="2c66d-126">Например, `Blog -> Posts -> Author` и `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="2c66d-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="2c66d-127">Это не означает, что вы получите избыточные соединения, в большинстве случаев EF будет консолидировать объединения при создании SQL.</span><span class="sxs-lookup"><span data-stu-id="2c66d-127">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="2c66d-128">Использование метода Include с производными типами</span><span class="sxs-lookup"><span data-stu-id="2c66d-128">Include on derived types</span></span>

<span data-ttu-id="2c66d-129">Вы можете включать связанные данные из навигаций, определенных только с производным типом, используя `Include` и `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="2c66d-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="2c66d-130">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="2c66d-130">Given the following model:</span></span>

```Csharp
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

<span data-ttu-id="2c66d-131">Содержимое навигации `School` всех людей, которые являются учениками, может быть активно загружено с помощью нескольких шаблонов:</span><span class="sxs-lookup"><span data-stu-id="2c66d-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="2c66d-132">С помощью приведения.</span><span class="sxs-lookup"><span data-stu-id="2c66d-132">using cast</span></span>
  ```Csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- <span data-ttu-id="2c66d-133">С помощью оператора `as`.</span><span class="sxs-lookup"><span data-stu-id="2c66d-133">using `as` operator</span></span>
  ```Csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- <span data-ttu-id="2c66d-134">С помощью перегрузки `Include`, принимающий параметр типа `string`.</span><span class="sxs-lookup"><span data-stu-id="2c66d-134">using overload of `Include` that takes parameter of type `string`</span></span>
  ```Csharp
  context.People.Include("Student").ToList()
  ```

### <a name="ignored-includes"></a><span data-ttu-id="2c66d-135">Игнорируемые включения</span><span class="sxs-lookup"><span data-stu-id="2c66d-135">Ignored includes</span></span>

<span data-ttu-id="2c66d-136">Если вы измените запрос так, чтобы он больше не возвращал экземпляры типа сущности, с которого начинался запрос, то операторы include игнорируются.</span><span class="sxs-lookup"><span data-stu-id="2c66d-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="2c66d-137">В следующем примере операторы include основаны на `Blog`, а затем оператор `Select` используется, чтобы изменить запрос для возврата анонимного типа.</span><span class="sxs-lookup"><span data-stu-id="2c66d-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="2c66d-138">В этом случае операторы include не действуют.</span><span class="sxs-lookup"><span data-stu-id="2c66d-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="2c66d-139">По умолчанию EF Core будет регистрировать предупреждение, если операторы include игнорируются.</span><span class="sxs-lookup"><span data-stu-id="2c66d-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="2c66d-140">Дополнительные сведения о просмотре выходных данных журнала см. в [этой статье](../miscellaneous/logging.md).</span><span class="sxs-lookup"><span data-stu-id="2c66d-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="2c66d-141">Вы можете изменить поведение, когда оператор include будет игнорироваться, чтобы либо не возвращать данные, либо ничего не делать.</span><span class="sxs-lookup"><span data-stu-id="2c66d-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="2c66d-142">Это можно указать при настройке параметров для вашего контекста — обычно в `DbContext.OnConfiguring` или в `Startup.cs`, если вы используете ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2c66d-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="2c66d-143">Явная загрузка</span><span class="sxs-lookup"><span data-stu-id="2c66d-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="2c66d-144">Эта возможность появилась в EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="2c66d-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="2c66d-145">Вы можете явно загрузить свойство навигации с помощью API `DbContext.Entry(...)`.</span><span class="sxs-lookup"><span data-stu-id="2c66d-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="2c66d-146">Вы также можете явно загрузить свойство навигации, выполнив отдельный запрос, который возвращает связанные сущности.</span><span class="sxs-lookup"><span data-stu-id="2c66d-146">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="2c66d-147">Если отслеживание изменений включено, то при загрузке сущности EF Core автоматически установит свойства навигации вновь загруженной сущности для ссылки на любые уже загруженные сущности и задаст свойства навигации уже загруженных сущностей, чтобы ссылаться на вновь загруженные сущности.</span><span class="sxs-lookup"><span data-stu-id="2c66d-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="2c66d-148">Запрос связанных сущностей</span><span class="sxs-lookup"><span data-stu-id="2c66d-148">Querying related entities</span></span>

<span data-ttu-id="2c66d-149">Можно также получить запрос LINQ, который представляет содержимое свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="2c66d-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="2c66d-150">Это позволяет выполнять такие задачи, как запуск оператора агрегирования в связанных сущностях без их загрузки в память.</span><span class="sxs-lookup"><span data-stu-id="2c66d-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="2c66d-151">Можно также фильтровать связанные сущности, которые загружаются в память.</span><span class="sxs-lookup"><span data-stu-id="2c66d-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="2c66d-152">«Неспешная» загрузка</span><span class="sxs-lookup"><span data-stu-id="2c66d-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="2c66d-153">Эта возможность появилась в EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="2c66d-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="2c66d-154">Самый простой способ использовать отложенную загрузку — установить пакет [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) и включить его с помощью вызова `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="2c66d-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="2c66d-155">Пример:</span><span class="sxs-lookup"><span data-stu-id="2c66d-155">For example:</span></span>
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="2c66d-156">При использовании AddDbContext:</span><span class="sxs-lookup"><span data-stu-id="2c66d-156">Or when using AddDbContext:</span></span>
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
<span data-ttu-id="2c66d-157">Затем EF Core включит отложенную загрузку любого свойства навигации, которое может быть переопределено, то есть оно должно быть типа `virtual` и быть в классе, который может быть унаследован.</span><span class="sxs-lookup"><span data-stu-id="2c66d-157">EF Core will then enable lazy-loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="2c66d-158">Например, в следующих сущностях свойства навигации `Post.Blog` и `Blog.Posts` будут загружены отложено.</span><span class="sxs-lookup"><span data-stu-id="2c66d-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
```Csharp
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
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="2c66d-159">Отложенная загрузка без прокси-серверов</span><span class="sxs-lookup"><span data-stu-id="2c66d-159">Lazy-loading without proxies</span></span>

<span data-ttu-id="2c66d-160">Прокси-серверы с отложенной загрузкой работают, внедряя службу `ILazyLoader` в сущность, как описано в статье [Entity Type Constructors](../modeling/constructors.md) (Конструкторы типов сущностей).</span><span class="sxs-lookup"><span data-stu-id="2c66d-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="2c66d-161">Пример:</span><span class="sxs-lookup"><span data-stu-id="2c66d-161">For example:</span></span>
```Csharp
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
        get => LazyLoader?.Load(this, ref _posts);
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
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="2c66d-162">Это не требует, чтобы наследуемые типы сущностей или свойства навигации были виртуальными и позволяет экземплярам сущностей, созданным с помощью `new`, отложено загружаться после прикрепления к контексту.</span><span class="sxs-lookup"><span data-stu-id="2c66d-162">This doesn't require entity types to be inherited from or navigation properties to be virtual and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="2c66d-163">Однако для этого требуется ссылка на службу `ILazyLoader`, которая связывает типы сущностей со сборкой EF Core.</span><span class="sxs-lookup"><span data-stu-id="2c66d-163">However, it requires a reference to the `ILazyLoader` service, which couples entity types to the EF Core assembly.</span></span> <span data-ttu-id="2c66d-164">Чтобы избежать этого, EF Core позволяет внедрить метод `ILazyLoader.Load` в качестве делегата.</span><span class="sxs-lookup"><span data-stu-id="2c66d-164">To avoid this EF Core allows the `ILazyLoader.Load` method to be injected as a delegate.</span></span> <span data-ttu-id="2c66d-165">Пример:</span><span class="sxs-lookup"><span data-stu-id="2c66d-165">For example:</span></span>
```Csharp
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
        get => LazyLoader?.Load(this, ref _posts);
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
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="2c66d-166">В приведенном выше коде используется метод расширения `Load`, чтобы упростить использование делегата:</span><span class="sxs-lookup"><span data-stu-id="2c66d-166">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
```Csharp
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
> <span data-ttu-id="2c66d-167">Параметр конструктора для делегата с отложенной загрузкой должен называться "lazyLoader".</span><span class="sxs-lookup"><span data-stu-id="2c66d-167">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="2c66d-168">Конфигурация для использования другого имени запланирована в будущей версии.</span><span class="sxs-lookup"><span data-stu-id="2c66d-168">Configuration to use a different name this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="2c66d-169">Связанные данные и сериализация</span><span class="sxs-lookup"><span data-stu-id="2c66d-169">Related data and serialization</span></span>

<span data-ttu-id="2c66d-170">Так как EF Core автоматически исправляет свойства навигации, вы можете завершить циклы в графе объектов.</span><span class="sxs-lookup"><span data-stu-id="2c66d-170">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="2c66d-171">Например, загрузка блога и связанных записей приведет к созданию объекта блога, который ссылается на коллекцию записей.</span><span class="sxs-lookup"><span data-stu-id="2c66d-171">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="2c66d-172">Каждая из этих записей будет ссылкой на блог.</span><span class="sxs-lookup"><span data-stu-id="2c66d-172">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="2c66d-173">Некоторые платформы сериализации не допускают такие циклы.</span><span class="sxs-lookup"><span data-stu-id="2c66d-173">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="2c66d-174">Например Json.NET вызовет следующее исключение при обнаружении цикла.</span><span class="sxs-lookup"><span data-stu-id="2c66d-174">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="2c66d-175">Newtonsoft.Json.JsonSerializationException: самоссылающийся цикл обнаружен для свойства "Blog" с типом MyApplication.Models.Blog.</span><span class="sxs-lookup"><span data-stu-id="2c66d-175">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="2c66d-176">Если вы используете ASP.NET Core, вы можете настроить Json.NET для игнорирования циклов, которые он находит в графе объектов.</span><span class="sxs-lookup"><span data-stu-id="2c66d-176">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="2c66d-177">Это делается в методе `ConfigureServices(...)` в `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="2c66d-177">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

``` csharp
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
