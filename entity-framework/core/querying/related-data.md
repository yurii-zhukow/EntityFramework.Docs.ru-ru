---
title: "Выполняется загрузка связанных данных — основной EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: 0d7705e0e5368435536e98d319c853ea8c732643
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2018
---
# <a name="loading-related-data"></a>Загрузка связанных данных

Entity Framework Core позволяет использовать свойства навигации в модели для загрузки связанных сущностей. Существует три общих шаблонов O/надежный обмен Сообщениями, используемые для загрузки связанных данных.
* **Безотложная загрузка** означает, что данные загружены из базы данных как часть исходного запроса.
* **Явная загрузка** означает, что связанные данные явно загружен из базы данных в дальнейшем.
* **Отложенная загрузка** означает, что связанные прозрачно загрузки данных из базы данных при доступе к свойству навигации.

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) из репозитория GitHub.

## <a name="eager-loading"></a>Безотложная загрузка

Можно использовать `Include` метод, чтобы задать связанные данные, которые будут включены в результаты запроса. В следующем примере, блоги, которые возвращаются в результатах будут иметь их `Posts` свойство заполняется связанных сообщений.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core будет свойства навигации автоматически исправить вверх с другими сущностями, которые были ранее загружены в экземпляр контекста. Поэтому даже если данные для свойства навигации не включить явным образом, свойство по-прежнему могут быть заполнены, если некоторые или все связанные сущности были ранее загружены.


Связанные данные из нескольких связей может включать в одном запросе.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Включая несколько уровней

Можно выполнить детализацию до связи для включения нескольких уровней связанных данных с помощью `ThenInclude` метод. В следующем примере загружаются все блоги, их связанных сообщений и автор каждой отправки.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> Текущие версии Visual Studio имеются средства завершения неверного кода и может привести к правильные выражения устанавливается флаг синтаксические ошибки при использовании `ThenInclude` метод после свойства навигации коллекции. Это является признаком ошибка в IntelliSense, отслеживаются в https://github.com/dotnet/roslyn/issues/8237. Его можно игнорировать эти ложные синтаксических ошибок, при условии, что код указано правильно и успешно скомпилирован. 

Можно соединить в цепочку несколько вызовов `ThenInclude` чтобы продолжить, включая дополнительные уровни взаимосвязанных данных.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Можно объединить все это для включения связанных данных из нескольких уровней и нескольких корней в одном запросе.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

Может потребоваться включить несколько связанных сущностей для одной сущности, включенного. Например, при запросе `Blog`s, включают `Posts` и затем необходимо включить оба `Author` и `Tags` из `Posts`. Чтобы сделать это, необходимо указать, имеют путь, начиная с корневого каталога. Например `Blog -> Posts -> Author` и `Blog -> Posts -> Tags`. Это значит, что вы получите избыточных соединений, в большинстве случаев будет консолидировать EF соединений при создании кода SQL.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a>Включить для производных типов

Могут включать связанные данные из определен только для производного типа с помощью переходов `Include` и `ThenInclude`. 

Рассмотрим следующую модель:

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

Содержимое `School` навигации всех людей, имеющих студенты могут загружаться заранее с помощью нескольких шаблонов:

- Использование функции cast
```Csharp
context.People.Include(person => ((Student)person).School).ToList()
```

- с помощью `as` оператор
```Csharp
context.People.Include(person => (person as Student).School).ToList()
```

- с помощью перегрузки `Include` , принимающий параметр типа `string`
```Csharp
context.People.Include("Student").ToList()
```

### <a name="ignored-includes"></a>Учитывается включает

Если изменить запрос, чтобы он больше не возвращает экземпляры типа сущности, запрос начинающийся с, операторы include игнорируются.

В следующем примере операторы include основаны на `Blog`, а затем `Select` оператор используется для изменения запроса для возврата анонимного типа. В этом случае операторы include не действуют.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

По умолчанию основные EF регистрируют предупреждения при включении операторы игнорируются. В разделе [входа](../miscellaneous/logging.md) Дополнительные сведения о просмотре выходных данных ведения журнала. Можно изменить поведение при оператор include игнорируется выдать или не выполняют никаких действий. Это делается при настройке параметров для текущего контекста — обычно в `DbContext.OnConfiguring`, или в `Startup.cs` при использовании ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>Явная загрузка

> [!NOTE]  
> Эта функция появилась в версии 1.1 EF Core.

Можно явно загрузить через свойство навигации `DbContext.Entry(...)` API.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

Можно также явно загрузить свойство навигации путем выполнения отдельного запроса, который возвращает связанные сущности. Если включено отслеживание изменений, то при загрузке сущности, EF Core будет автоматически задать свойства навигации объектов загруженных для ссылки на все сущности, которые уже загружены и задать свойства навигации для ссылки на сущности уже загружен загруженных сущность.

### <a name="querying-related-entities"></a>Запрос связанных сущностей

Можно также получить запрос LINQ, который представляет содержимое свойства навигации.

Это дает возможность выполнения действий, таких как запуск статистических операторов для связанных сущностей без их загрузки в память.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Можно также фильтровать связанные сущности загружаются в память.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>«Неспешная» загрузка

> [!NOTE]  
> Эта функция появилась в EF Core 2.1.

Это самый простой способ использовать отложенную загрузку установить [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) пакет и его включение на вызов `UseLazyLoadingProxies`. Пример:
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
Или, при использовании AddDbContext:
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
EF Core затем необходимо включить отложенную загрузку для любого свойства навигации, который может быть переопределен--,, оно должно быть `virtual` и класса, который может быть унаследован. Например, в следующие сущности `Post.Blog` и `Blog.Posts` свойства навигации будут отложенной загрузки.
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
### <a name="lazy-loading-without-proxies"></a>Lazy Загрузка без прокси-серверов

Загрузка Lazy прокси-серверы работают, внедряя `ILazyLoader` служба в сущности, как описано в [конструкторов типа сущности](../modeling/constructors.md). Пример:
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
Это не требует типы наследуется из сущности или свойства навигации должен быть виртуальным и позволяет экземпляры сущности, созданные с `new` для отложенной загрузки один раз присоединены к контексту. Однако он требует ссылку на `ILazyLoader` службы, которая связывает типы сущностей в EF основной сборке. Чтобы избежать этой базовой EF позволяет `ILazyLoader.Load` метод встретившаяся в качестве делегата. Пример:
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
Приведенный выше использует `Load` метод расширения, чтобы сделать немного очистки с помощью делегата:
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
> «LazyLoader» должна вызываться параметра конструктора делегата отложенную загрузку. Конфигурация, чтобы использовать другое имя, это планируется в будущих выпусках.

## <a name="related-data-and-serialization"></a>Связанные данные и сериализации

Поскольку свойства навигации автоматически исправить вверх будет EF Core, можно получить с циклами в граф объекта. Например загрузка блог и он не связан приведет к записи в блоге объект, который ссылается на коллекцию записей. Каждый из этих записей будет ссылкой на блога.

Некоторые платформы сериализации не допускают такие циклы. Например Json.NET вызовет следующее исключение при обнаружении цикла.

> Newtonsoft.Json.JsonSerializationException: Self ссылается обнаружен для свойства «Блог» с типом «MyApplication.Models.Blog» цикл.

При использовании ASP.NET Core можно настроить Json.NET игнорировать циклов, найденные в графе объекта. Это можно сделать в `ConfigureServices(...)` метод в `Startup.cs`.

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
