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
# <a name="loading-related-data"></a>Загрузка связанных данных

Entity Framework Core позволяет использовать свойства навигации в модели для загрузки связанных сущностей. Для загрузки связанных данных используются три общих шаблона объектно-реляционного сопоставления (O/RM).
* **Безотложная загрузка** означает, что данные загружены из базы данных как часть исходного запроса.
* **Явная загрузка** означает, что связанные данные явно загружаются из базы данных позже.
* **Отложенная загрузка** означает, что связанные данные прозрачно загружаются из базы данных при доступе к свойству навигации.

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) из репозитория GitHub.

## <a name="eager-loading"></a>Безотложная загрузка

Вы можете использовать метод `Include`, чтобы указать связанные данные, которые будут включены в результаты запроса. В следующем примере блоги, возвращенные в результатах, будут иметь свойство `Posts`, заполненное соответствующими записями.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> EF Core автоматически исправляет свойства навигации для других сущностей, которые были ранее загружены в экземпляр контекста. Даже если данные для свойства навигации не включены явно, свойство все равно можно заполнить при условии, что ранее были загружены некоторые или все связанные сущности.


Связанные данные из нескольких связей можно включить в один запрос.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Включение нескольких уровней

Вы можете детализировать отношения, чтобы включить несколько уровней связанных данных, используя метод `ThenInclude`. В следующем примере загружаются все блоги, связанные с ними записи и автор каждой записи.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> Текущие версии Visual Studio предлагают неправильные варианты завершения кода и могут быть причиной того, что правильные выражения могут быть помечены как содержащие синтаксические ошибки при использовании метода `ThenInclude` после свойства навигации коллекции. Это признак ошибки IntelliSense, отслеживаемой в https://github.com/dotnet/roslyn/issues/8237. Эти ложные синтаксические ошибки можно игнорировать, если код верен и может быть успешно скомпилирован. 

Вы можете связать в цепочку несколько вызовов `ThenInclude`, чтобы продолжить включение уровней связанных данных.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Вы можете объединить все это, чтобы включить связанные данные из нескольких уровней и нескольких корней в один и тот же запрос.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

Может потребоваться включить несколько связанных сущностей для одной включенной сущности. Например, при запросе `Blog` вы включаете `Posts`, а затем хотите включить `Author` и `Tags` из `Posts`. Для этого вам нужно указать каждый путь включения, начиная с корня. Например, `Blog -> Posts -> Author` и `Blog -> Posts -> Tags`. Это не означает, что вы получите избыточные соединения, в большинстве случаев EF будет консолидировать объединения при создании SQL.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a>Использование метода Include с производными типами

Вы можете включать связанные данные из навигаций, определенных только с производным типом, используя `Include` и `ThenInclude`. 

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

Содержимое навигации `School` всех людей, которые являются учениками, может быть активно загружено с помощью нескольких шаблонов:

- С помощью приведения.
  ```Csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- С помощью оператора `as`.
  ```Csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- С помощью перегрузки `Include`, принимающий параметр типа `string`.
  ```Csharp
  context.People.Include("Student").ToList()
  ```

### <a name="ignored-includes"></a>Игнорируемые включения

Если вы измените запрос так, чтобы он больше не возвращал экземпляры типа сущности, с которого начинался запрос, то операторы include игнорируются.

В следующем примере операторы include основаны на `Blog`, а затем оператор `Select` используется, чтобы изменить запрос для возврата анонимного типа. В этом случае операторы include не действуют.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

По умолчанию EF Core будет регистрировать предупреждение, если операторы include игнорируются. Дополнительные сведения о просмотре выходных данных журнала см. в [этой статье](../miscellaneous/logging.md). Вы можете изменить поведение, когда оператор include будет игнорироваться, чтобы либо не возвращать данные, либо ничего не делать. Это можно указать при настройке параметров для вашего контекста — обычно в `DbContext.OnConfiguring` или в `Startup.cs`, если вы используете ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>Явная загрузка

> [!NOTE]  
> Эта возможность появилась в EF Core 1.1.

Вы можете явно загрузить свойство навигации с помощью API `DbContext.Entry(...)`.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

Вы также можете явно загрузить свойство навигации, выполнив отдельный запрос, который возвращает связанные сущности. Если отслеживание изменений включено, то при загрузке сущности EF Core автоматически установит свойства навигации вновь загруженной сущности для ссылки на любые уже загруженные сущности и задаст свойства навигации уже загруженных сущностей, чтобы ссылаться на вновь загруженные сущности.

### <a name="querying-related-entities"></a>Запрос связанных сущностей

Можно также получить запрос LINQ, который представляет содержимое свойства навигации.

Это позволяет выполнять такие задачи, как запуск оператора агрегирования в связанных сущностях без их загрузки в память.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Можно также фильтровать связанные сущности, которые загружаются в память.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>«Неспешная» загрузка

> [!NOTE]  
> Эта возможность появилась в EF Core 2.1.

Самый простой способ использовать отложенную загрузку — установить пакет [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) и включить его с помощью вызова `UseLazyLoadingProxies`. Пример:
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
При использовании AddDbContext:
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
Затем EF Core включит отложенную загрузку любого свойства навигации, которое может быть переопределено, то есть оно должно быть типа `virtual` и быть в классе, который может быть унаследован. Например, в следующих сущностях свойства навигации `Post.Blog` и `Blog.Posts` будут загружены отложено.
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
### <a name="lazy-loading-without-proxies"></a>Отложенная загрузка без прокси-серверов

Прокси-серверы с отложенной загрузкой работают, внедряя службу `ILazyLoader` в сущность, как описано в статье [Entity Type Constructors](../modeling/constructors.md) (Конструкторы типов сущностей). Пример:
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
Это не требует, чтобы наследуемые типы сущностей или свойства навигации были виртуальными и позволяет экземплярам сущностей, созданным с помощью `new`, отложено загружаться после прикрепления к контексту. Однако для этого требуется ссылка на службу `ILazyLoader`, которая связывает типы сущностей со сборкой EF Core. Чтобы избежать этого, EF Core позволяет внедрить метод `ILazyLoader.Load` в качестве делегата. Пример:
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
В приведенном выше коде используется метод расширения `Load`, чтобы упростить использование делегата:
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
> Параметр конструктора для делегата с отложенной загрузкой должен называться "lazyLoader". Конфигурация для использования другого имени запланирована в будущей версии.

## <a name="related-data-and-serialization"></a>Связанные данные и сериализация

Так как EF Core автоматически исправляет свойства навигации, вы можете завершить циклы в графе объектов. Например, загрузка блога и связанных записей приведет к созданию объекта блога, который ссылается на коллекцию записей. Каждая из этих записей будет ссылкой на блог.

Некоторые платформы сериализации не допускают такие циклы. Например Json.NET вызовет следующее исключение при обнаружении цикла.

> Newtonsoft.Json.JsonSerializationException: самоссылающийся цикл обнаружен для свойства "Blog" с типом MyApplication.Models.Blog.

Если вы используете ASP.NET Core, вы можете настроить Json.NET для игнорирования циклов, которые он находит в графе объектов. Это делается в методе `ConfigureServices(...)` в `Startup.cs`.

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
