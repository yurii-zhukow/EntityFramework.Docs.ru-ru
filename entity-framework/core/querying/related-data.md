---
title: Загрузка связанных данных — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 915aaa41beb495a046f2d6260e9c3b174d5f3031
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413723"
---
# <a name="loading-related-data"></a>Загрузка связанных данных

Entity Framework Core позволяет использовать свойства навигации в модели для загрузки связанных сущностей. Для загрузки связанных данных используются три общих шаблона объектно-реляционного сопоставления (O/RM).

* **Безотложная загрузка** означает, что данные загружены из базы данных как часть исходного запроса.
* **Явная загрузка** означает, что связанные данные явно загружаются из базы данных позже.
* **Отложенная загрузка** означает, что связанные данные прозрачно загружаются из базы данных при доступе к свойству навигации.

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) из репозитория GitHub.

## <a name="eager-loading"></a>Безотложная загрузка

Вы можете использовать метод `Include`, чтобы указать связанные данные, которые будут включены в результаты запроса. В следующем примере блоги, возвращенные в результатах, будут иметь свойство `Posts`, заполненное соответствующими записями.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> EF Core автоматически исправляет свойства навигации для других сущностей, которые были ранее загружены в экземпляр контекста. Даже если данные для свойства навигации не включены явно, свойство все равно можно заполнить при условии, что ранее были загружены некоторые или все связанные сущности.

Связанные данные из нескольких связей можно включить в один запрос.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Включение нескольких уровней

Вы можете детализировать отношения, чтобы включить несколько уровней связанных данных, используя метод `ThenInclude`. В следующем примере загружаются все блоги, связанные с ними записи и автор каждой записи.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleThenInclude)]

Вы можете связать в цепочку несколько вызовов `ThenInclude`, чтобы продолжить включение уровней связанных данных.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Вы можете объединить все это, чтобы включить связанные данные из нескольких уровней и нескольких корней в один и тот же запрос.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IncludeTree)]

Может потребоваться включить несколько связанных сущностей для одной включенной сущности. Например, при запросе `Blogs` вы включаете `Posts`, а затем хотите включить `Author` и `Tags` из `Posts`. Для этого вам нужно указать каждый путь включения, начиная с корня. Например, `Blog -> Posts -> Author` и `Blog -> Posts -> Tags`. Это не означает, что вы получите избыточные соединения, в большинстве случаев EF будет консолидировать соединения при создании SQL.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

> [!CAUTION]
> Начиная с версии 3.0.0, каждый `Include` приведет к добавлению дополнительного JOIN в запросы SQL, создаваемые реляционными поставщиками, тогда как предыдущие версии создавали дополнительные SQL-запросы. Это может значительно изменить производительность запросов к лучшему или к худшему. В частности, может возникнуть потребность разделения запросов LINQ с чрезмерно большим числом операторов `Include` на несколько отдельных запросов LINQ, чтобы избежать проблемы декартова взрыва.

### <a name="include-on-derived-types"></a>Использование метода Include с производными типами

Вы можете включать связанные данные из навигаций, определенных только с производным типом, используя `Include` и `ThenInclude`.

Рассмотрим следующую модель:

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

Содержимое навигации `School` всех людей, которые являются учениками, может быть активно загружено с помощью нескольких шаблонов:

* С помощью приведения.

  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

* С помощью оператора `as`.

  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

* С помощью перегрузки `Include`, принимающий параметр типа `string`.

  ```csharp
  context.People.Include("School").ToList()
  ```

## <a name="explicit-loading"></a>Явная загрузка

Вы можете явно загрузить свойство навигации с помощью API `DbContext.Entry(...)`.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#Eager)]

Вы также можете явно загрузить свойство навигации, выполнив отдельный запрос, который возвращает связанные сущности. Если отслеживание изменений включено, то при загрузке сущности EF Core автоматически установит свойства навигации вновь загруженной сущности для ссылки на любые уже загруженные сущности и задаст свойства навигации уже загруженных сущностей, чтобы ссылаться на вновь загруженные сущности.

### <a name="querying-related-entities"></a>Запрос связанных сущностей

Можно также получить запрос LINQ, который представляет содержимое свойства навигации.

Это позволяет выполнять такие задачи, как запуск оператора агрегирования в связанных сущностях без их загрузки в память.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Можно также фильтровать связанные сущности, которые загружаются в память.

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>Отложенная загрузка

Самый простой способ использовать отложенную загрузку — установить пакет [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) и включить его с помощью вызова `UseLazyLoadingProxies`. Пример:

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```

При использовании AddDbContext:

```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```

Затем EF Core включит отложенную загрузку любого свойства навигации, которое может быть переопределено, то есть оно должно быть типа `virtual` и быть в классе, который может быть унаследован. Например, в следующих сущностях свойства навигации `Post.Blog` и `Blog.Posts` будут загружены отложено.

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

### <a name="lazy-loading-without-proxies"></a>Отложенная загрузка без прокси-серверов

Прокси-серверы с отложенной загрузкой работают, внедряя службу `ILazyLoader` в сущность, как описано в статье [Entity Type Constructors](../modeling/constructors.md) (Конструкторы типов сущностей). Пример:

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

Не требуется, чтобы наследуемые типы сущностей или свойства навигации были виртуальными, и экземплярам сущностей, созданным с помощью `new`, можно отложено загружаться после прикрепления к контексту. Однако для этого требуется ссылка на службу `ILazyLoader`, которая определена в пакете [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/). Этот пакет содержит минимальный набор типов, поэтому он не оказывает большого влияния. Тем не менее, чтобы полностью избежать зависимости от любых пакетов EF Core в типах сущностей, можно ввести метод `ILazyLoader.Load` в качестве делегата. Пример:

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

В приведенном выше коде используется метод расширения `Load`, чтобы упростить использование делегата:

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
> Параметр конструктора для делегата с отложенной загрузкой должен называться "lazyLoader". Конфигурация для использования отличного от этого имени запланирована в будущей версии.

## <a name="related-data-and-serialization"></a>Связанные данные и сериализация

Так как EF Core автоматически исправляет свойства навигации, вы можете завершить циклы в графе объектов. Например, загрузка блога и связанных записей приведет к созданию объекта блога, который ссылается на коллекцию записей. Каждая из этих записей будет ссылкой на блог.

Некоторые платформы сериализации не допускают такие циклы. Например Json.NET вызовет следующее исключение при обнаружении цикла.

> Newtonsoft.Json.JsonSerializationException: самоссылающийся цикл обнаружен для свойства "Blog" с типом MyApplication.Models.Blog.

Если вы используете ASP.NET Core, вы можете настроить Json.NET для игнорирования циклов, которые он находит в графе объектов. Это делается в методе `ConfigureServices(...)` в `Startup.cs`.

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

Другой вариант — дополнение одного из свойств навигации атрибутом `[JsonIgnore]`, который указывает Json.NET игнорировать это свойство навигации во время сериализации.
