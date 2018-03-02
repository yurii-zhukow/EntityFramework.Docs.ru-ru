---
title: "Выполняется загрузка связанных данных — основной EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: dadc6235c3879ae27ad5c99988a5e594872045df
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="loading-related-data"></a><span data-ttu-id="41d33-102">Загрузка связанных данных</span><span class="sxs-lookup"><span data-stu-id="41d33-102">Loading Related Data</span></span>

<span data-ttu-id="41d33-103">Entity Framework Core позволяет использовать свойства навигации в модели для загрузки связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="41d33-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="41d33-104">Существует три общих шаблонов O/надежный обмен Сообщениями, используемые для загрузки связанных данных.</span><span class="sxs-lookup"><span data-stu-id="41d33-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="41d33-105">**Безотложная загрузка** означает, что данные загружены из базы данных как часть исходного запроса.</span><span class="sxs-lookup"><span data-stu-id="41d33-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="41d33-106">**Явная загрузка** означает, что связанные данные явно загружен из базы данных в дальнейшем.</span><span class="sxs-lookup"><span data-stu-id="41d33-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="41d33-107">**Отложенная загрузка** означает, что связанные прозрачно загрузки данных из базы данных при доступе к свойству навигации.</span><span class="sxs-lookup"><span data-stu-id="41d33-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="41d33-108">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="41d33-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="41d33-109">Безотложная загрузка</span><span class="sxs-lookup"><span data-stu-id="41d33-109">Eager loading</span></span>

<span data-ttu-id="41d33-110">Можно использовать `Include` метод, чтобы задать связанные данные, которые будут включены в результаты запроса.</span><span class="sxs-lookup"><span data-stu-id="41d33-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="41d33-111">В следующем примере, блоги, которые возвращаются в результатах будут иметь их `Posts` свойство заполняется связанных сообщений.</span><span class="sxs-lookup"><span data-stu-id="41d33-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="41d33-112">Entity Framework Core будет свойства навигации автоматически исправить вверх с другими сущностями, которые были ранее загружены в экземпляр контекста.</span><span class="sxs-lookup"><span data-stu-id="41d33-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="41d33-113">Поэтому даже если данные для свойства навигации не включить явным образом, свойство по-прежнему могут быть заполнены, если некоторые или все связанные сущности были ранее загружены.</span><span class="sxs-lookup"><span data-stu-id="41d33-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="41d33-114">Связанные данные из нескольких связей может включать в одном запросе.</span><span class="sxs-lookup"><span data-stu-id="41d33-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="41d33-115">Включая несколько уровней</span><span class="sxs-lookup"><span data-stu-id="41d33-115">Including multiple levels</span></span>

<span data-ttu-id="41d33-116">Можно выполнить детализацию до связи для включения нескольких уровней связанных данных с помощью `ThenInclude` метод.</span><span class="sxs-lookup"><span data-stu-id="41d33-116">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="41d33-117">В следующем примере загружаются все блоги, их связанных сообщений и автор каждой отправки.</span><span class="sxs-lookup"><span data-stu-id="41d33-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="41d33-118">Текущие версии Visual Studio имеются средства завершения неверного кода и может привести к правильные выражения устанавливается флаг синтаксические ошибки при использовании `ThenInclude` метод после свойства навигации коллекции.</span><span class="sxs-lookup"><span data-stu-id="41d33-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="41d33-119">Это является признаком ошибка в IntelliSense, отслеживаются в https://github.com/dotnet/roslyn/issues/8237.</span><span class="sxs-lookup"><span data-stu-id="41d33-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="41d33-120">Его можно игнорировать эти ложные синтаксических ошибок, при условии, что код указано правильно и успешно скомпилирован.</span><span class="sxs-lookup"><span data-stu-id="41d33-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="41d33-121">Можно соединить в цепочку несколько вызовов `ThenInclude` чтобы продолжить, включая дополнительные уровни взаимосвязанных данных.</span><span class="sxs-lookup"><span data-stu-id="41d33-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="41d33-122">Можно объединить все это для включения связанных данных из нескольких уровней и нескольких корней в одном запросе.</span><span class="sxs-lookup"><span data-stu-id="41d33-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="41d33-123">Может потребоваться включить несколько связанных сущностей для одной сущности, включенного.</span><span class="sxs-lookup"><span data-stu-id="41d33-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="41d33-124">Например, при запросе `Blog`s, включают `Posts` и затем необходимо включить оба `Author` и `Tags` из `Posts`.</span><span class="sxs-lookup"><span data-stu-id="41d33-124">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="41d33-125">Чтобы сделать это, необходимо указать, имеют путь, начиная с корневого каталога.</span><span class="sxs-lookup"><span data-stu-id="41d33-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="41d33-126">Например `Blog -> Posts -> Author` и `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="41d33-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="41d33-127">Это значит, что вы получите избыточных соединений, в большинстве случаев будет консолидировать EF соединений при создании кода SQL.</span><span class="sxs-lookup"><span data-stu-id="41d33-127">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="41d33-128">Включить для производных типов</span><span class="sxs-lookup"><span data-stu-id="41d33-128">Include on derived types</span></span>

<span data-ttu-id="41d33-129">Могут включать связанные данные из определен только для производного типа с помощью переходов `Include` и `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="41d33-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="41d33-130">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="41d33-130">Given the following model:</span></span>

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

<span data-ttu-id="41d33-131">Содержимое `School` навигации всех людей, имеющих студенты могут загружаться заранее с помощью нескольких шаблонов:</span><span class="sxs-lookup"><span data-stu-id="41d33-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="41d33-132">Использование функции cast</span><span class="sxs-lookup"><span data-stu-id="41d33-132">using cast</span></span>
```Csharp
context.People.Include(person => ((Student)person).School).ToList()
```

- <span data-ttu-id="41d33-133">с помощью `as` оператор</span><span class="sxs-lookup"><span data-stu-id="41d33-133">using `as` operator</span></span>
```Csharp
context.People.Include(person => (person as Student).School).ToList()
```

- <span data-ttu-id="41d33-134">с помощью перегрузки `Include` , принимающий параметр типа `string`</span><span class="sxs-lookup"><span data-stu-id="41d33-134">using overload of `Include` that takes parameter of type `string`</span></span>
```Csharp
context.People.Include("Student").ToList()
```

### <a name="ignored-includes"></a><span data-ttu-id="41d33-135">Учитывается включает</span><span class="sxs-lookup"><span data-stu-id="41d33-135">Ignored includes</span></span>

<span data-ttu-id="41d33-136">Если изменить запрос, чтобы он больше не возвращает экземпляры типа сущности, запрос начинающийся с, операторы include игнорируются.</span><span class="sxs-lookup"><span data-stu-id="41d33-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="41d33-137">В следующем примере операторы include основаны на `Blog`, а затем `Select` оператор используется для изменения запроса для возврата анонимного типа.</span><span class="sxs-lookup"><span data-stu-id="41d33-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="41d33-138">В этом случае операторы include не действуют.</span><span class="sxs-lookup"><span data-stu-id="41d33-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="41d33-139">По умолчанию основные EF регистрируют предупреждения при включении операторы игнорируются.</span><span class="sxs-lookup"><span data-stu-id="41d33-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="41d33-140">В разделе [входа](../miscellaneous/logging.md) Дополнительные сведения о просмотре выходных данных ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="41d33-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="41d33-141">Можно изменить поведение при оператор include игнорируется выдать или не выполняют никаких действий.</span><span class="sxs-lookup"><span data-stu-id="41d33-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="41d33-142">Это делается при настройке параметров для текущего контекста — обычно в `DbContext.OnConfiguring`, или в `Startup.cs` при использовании ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="41d33-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="41d33-143">Явная загрузка</span><span class="sxs-lookup"><span data-stu-id="41d33-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="41d33-144">Эта функция появилась в версии 1.1 EF Core.</span><span class="sxs-lookup"><span data-stu-id="41d33-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="41d33-145">Можно явно загрузить через свойство навигации `DbContext.Entry(...)` API.</span><span class="sxs-lookup"><span data-stu-id="41d33-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="41d33-146">Можно также явно загрузить свойство навигации путем выполнения отдельного запроса, который возвращает связанные сущности.</span><span class="sxs-lookup"><span data-stu-id="41d33-146">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="41d33-147">Если включено отслеживание изменений, то при загрузке сущности, EF Core будет автоматически задать свойства навигации объектов загруженных для ссылки на все сущности, которые уже загружены и задать свойства навигации для ссылки на сущности уже загружен загруженных сущность.</span><span class="sxs-lookup"><span data-stu-id="41d33-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="41d33-148">Запрос связанных сущностей</span><span class="sxs-lookup"><span data-stu-id="41d33-148">Querying related entities</span></span>

<span data-ttu-id="41d33-149">Можно также получить запрос LINQ, который представляет содержимое свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="41d33-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="41d33-150">Это дает возможность выполнения действий, таких как запуск статистических операторов для связанных сущностей без их загрузки в память.</span><span class="sxs-lookup"><span data-stu-id="41d33-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="41d33-151">Можно также фильтровать связанные сущности загружаются в память.</span><span class="sxs-lookup"><span data-stu-id="41d33-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="41d33-152">«Неспешная» загрузка</span><span class="sxs-lookup"><span data-stu-id="41d33-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="41d33-153">Эта функция появилась в EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="41d33-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="41d33-154">Это самый простой способ использовать отложенную загрузку установить [Microsoft.EntityFramworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) пакет и его включение на вызов `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="41d33-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFramworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="41d33-155">Пример:</span><span class="sxs-lookup"><span data-stu-id="41d33-155">For example:</span></span>
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="41d33-156">Или, при использовании AddDbContext:</span><span class="sxs-lookup"><span data-stu-id="41d33-156">Or when using AddDbContext:</span></span>
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
<span data-ttu-id="41d33-157">EF Core затем необходимо включить отложенную загрузку для любого свойства навигации, который может быть переопределен--,, оно должно быть `virtual` и класса, который может быть унаследован.</span><span class="sxs-lookup"><span data-stu-id="41d33-157">EF Core will then enable lazy-loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="41d33-158">Например, в следующие сущности `Post.Blog` и `Blog.Posts` свойства навигации будут отложенной загрузки.</span><span class="sxs-lookup"><span data-stu-id="41d33-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
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
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="41d33-159">Lazy Загрузка без прокси-серверов</span><span class="sxs-lookup"><span data-stu-id="41d33-159">Lazy-loading without proxies</span></span>

<span data-ttu-id="41d33-160">Загрузка Lazy прокси-серверы работают, внедряя `ILazyLoader` служба в сущности, как описано в [конструкторов типа сущности](../modeling/constructors.md).</span><span class="sxs-lookup"><span data-stu-id="41d33-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="41d33-161">Пример:</span><span class="sxs-lookup"><span data-stu-id="41d33-161">For example:</span></span>
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
<span data-ttu-id="41d33-162">Это не требует типы наследуется из сущности или свойства навигации должен быть виртуальным и позволяет экземпляры сущности, созданные с `new` для отложенной загрузки один раз присоединены к контексту.</span><span class="sxs-lookup"><span data-stu-id="41d33-162">This doesn't require entity types to be inherited from or navigation properties to be virtual and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="41d33-163">Однако он требует ссылку на `ILazyLoader` службы, которая связывает типы сущностей в EF основной сборке.</span><span class="sxs-lookup"><span data-stu-id="41d33-163">However, it requires a reference to the `ILazyLoader` service, which couples entity types to the EF Core assembly.</span></span> <span data-ttu-id="41d33-164">Чтобы избежать этой базовой EF позволяет `ILazyLoader.Load` метод встретившаяся в качестве делегата.</span><span class="sxs-lookup"><span data-stu-id="41d33-164">To avoid this EF Core allows the `ILazyLoader.Load` method to be injected as a delegate.</span></span> <span data-ttu-id="41d33-165">Пример:</span><span class="sxs-lookup"><span data-stu-id="41d33-165">For example:</span></span>
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
<span data-ttu-id="41d33-166">Приведенный выше использует `Load` метод расширения, чтобы сделать немного очистки с помощью делегата:</span><span class="sxs-lookup"><span data-stu-id="41d33-166">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
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
> <span data-ttu-id="41d33-167">«LazyLoader» должна вызываться параметра конструктора делегата отложенную загрузку.</span><span class="sxs-lookup"><span data-stu-id="41d33-167">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="41d33-168">Конфигурация, чтобы использовать другое имя, это планируется в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="41d33-168">Configuration to use a different name this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="41d33-169">Связанные данные и сериализации</span><span class="sxs-lookup"><span data-stu-id="41d33-169">Related data and serialization</span></span>

<span data-ttu-id="41d33-170">Поскольку свойства навигации автоматически исправить вверх будет EF Core, можно получить с циклами в граф объекта.</span><span class="sxs-lookup"><span data-stu-id="41d33-170">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="41d33-171">Например загрузка блог и он не связан приведет к записи в блоге объект, который ссылается на коллекцию записей.</span><span class="sxs-lookup"><span data-stu-id="41d33-171">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="41d33-172">Каждый из этих записей будет ссылкой на блога.</span><span class="sxs-lookup"><span data-stu-id="41d33-172">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="41d33-173">Некоторые платформы сериализации не допускают такие циклы.</span><span class="sxs-lookup"><span data-stu-id="41d33-173">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="41d33-174">Например Json.NET вызовет следующее исключение при обнаружении цикла.</span><span class="sxs-lookup"><span data-stu-id="41d33-174">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="41d33-175">Newtonsoft.Json.JsonSerializationException: Self ссылается обнаружен для свойства «Блог» с типом «MyApplication.Models.Blog» цикл.</span><span class="sxs-lookup"><span data-stu-id="41d33-175">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="41d33-176">При использовании ASP.NET Core можно настроить Json.NET игнорировать циклов, найденные в графе объекта.</span><span class="sxs-lookup"><span data-stu-id="41d33-176">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="41d33-177">Это можно сделать в `ConfigureServices(...)` метод в `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="41d33-177">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

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
