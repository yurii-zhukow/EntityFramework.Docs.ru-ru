---
title: Новые возможности EF Core 5.0
description: Обзор новых возможностей в EF Core 5.0
author: ajcvickers
ms.date: 09/10/2020
uid: core/what-is-new/ef-core-5.0/whatsnew
ms.openlocfilehash: 64b72ba2a6f752b9d71ea9b64ab08f4cf92ef03d
ms.sourcegitcommit: 032a1767d7a6e42052a005f660b80372c6521e7e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98129282"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="75255-103">Новые возможности EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="75255-103">What's New in EF Core 5.0</span></span>

<span data-ttu-id="75255-104">В приведенном ниже списке указаны ключевые новые функции EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="75255-104">The following list includes the major new features in EF Core 5.0.</span></span> <span data-ttu-id="75255-105">Полный перечень проблем в этом выпуске см. в [средстве отслеживания проблем](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A5.0.0).</span><span class="sxs-lookup"><span data-stu-id="75255-105">For the full list of issues in the release, see our [issue tracker](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A5.0.0).</span></span>

<span data-ttu-id="75255-106">EF Core 5.0 — это основной выпуск. Он содержит несколько [критических изменений](xref:core/what-is-new/ef-core-5.0/breaking-changes), касающихся улучшений API и поведения. Такие изменения могут негативно повлиять на работу существующих приложений.</span><span class="sxs-lookup"><span data-stu-id="75255-106">As a major release, EF Core 5.0 also contains several [breaking changes](xref:core/what-is-new/ef-core-5.0/breaking-changes), which are API improvements or behavioral changes that may have negative impact on existing applications.</span></span>

## <a name="many-to-many"></a><span data-ttu-id="75255-107">"Много ко многим"</span><span class="sxs-lookup"><span data-stu-id="75255-107">Many-to-many</span></span>

<span data-ttu-id="75255-108">В EF Core 5.0 поддерживаются связи "многие ко многим" без явного сопоставления таблицы соединения.</span><span class="sxs-lookup"><span data-stu-id="75255-108">EF Core 5.0 supports many-to-many relationships without explicitly mapping the join table.</span></span>

<span data-ttu-id="75255-109">Например, рассмотрим такие типы сущностей:</span><span class="sxs-lookup"><span data-stu-id="75255-109">For example, consider these entity types:</span></span>

```csharp
public class Post
{
    public int Id { get; set; }
    public string Name { get; set; }
    public ICollection<Tag> Tags { get; set; }
}

public class Tag
{
    public int Id { get; set; }
    public string Text { get; set; }
    public ICollection<Post> Posts { get; set; }
}
```

<span data-ttu-id="75255-110">По соглашению EF Core 5.0 распознает их как связь "многие ко многим" и автоматически создает в базе данных таблицу соединения `PostTag`.</span><span class="sxs-lookup"><span data-stu-id="75255-110">EF Core 5.0 recognizes this as a many-to-many relationship by convention, and automatically creates a `PostTag` join table in the database.</span></span> <span data-ttu-id="75255-111">Данные могут запрашиваться и обновляться без явных ссылок на таблицу соединения, что значительно упрощает код.</span><span class="sxs-lookup"><span data-stu-id="75255-111">Data can be queried and updated without explicitly referencing the join table, considerably simplifying code.</span></span> <span data-ttu-id="75255-112">При необходимости таблицу соединений по-прежнему можно настроить для явного выполнения запросов.</span><span class="sxs-lookup"><span data-stu-id="75255-112">The join table can still be customized and queried explicitly if needed.</span></span>

<span data-ttu-id="75255-113">Дополнительные сведения см. в [полной документации по связи "многие ко многим"](xref:core/modeling/relationships#many-to-many).</span><span class="sxs-lookup"><span data-stu-id="75255-113">For further information, [see the full documentation on many-to-many](xref:core/modeling/relationships#many-to-many).</span></span>

## <a name="split-queries"></a><span data-ttu-id="75255-114">Разделенные запросы</span><span class="sxs-lookup"><span data-stu-id="75255-114">Split queries</span></span>

<span data-ttu-id="75255-115">Начиная с EF Core 3.0, EF Core всегда создает один SQL-запрос для каждого LINQ-запроса.</span><span class="sxs-lookup"><span data-stu-id="75255-115">Starting with EF Core 3.0, EF Core always generates a single SQL query for each LINQ query.</span></span> <span data-ttu-id="75255-116">Это обеспечивает согласованность данных, возвращаемых в рамках ограничений используемого режима транзакций.</span><span class="sxs-lookup"><span data-stu-id="75255-116">This ensures consistency of the data returned within the constraints of the transaction mode in use.</span></span> <span data-ttu-id="75255-117">Однако если запрос использует `Include` или проекцию для возврата нескольких связанных коллекций, это может привести к снижению производительности.</span><span class="sxs-lookup"><span data-stu-id="75255-117">However, this can become very slow when the query uses `Include` or a projection to bring back multiple related collections.</span></span>

<span data-ttu-id="75255-118">Теперь EF Core 5.0 позволяет разделять один LINQ-запрос, включая связанные коллекции, на несколько SQL-запросов.</span><span class="sxs-lookup"><span data-stu-id="75255-118">EF Core 5.0 now allows a single LINQ query including related collections to be split into multiple SQL queries.</span></span> <span data-ttu-id="75255-119">Это может значительно повысить производительность, но может также привести к несогласованности возвращаемых результатов в случае изменения данных между двумя запросами.</span><span class="sxs-lookup"><span data-stu-id="75255-119">This can significantly improve performance, but can result in inconsistency in the results returned if the data changes between the two queries.</span></span> <span data-ttu-id="75255-120">Для устранения этой проблемы и обеспечения согласованности с разделенными запросами можно использовать сериализуемые транзакции или транзакции с моментальными снимками, но это может привести к дополнительным затратам на производительность и вызвать различия в поведении.</span><span class="sxs-lookup"><span data-stu-id="75255-120">Serializable or snapshot transactions can be used to mitigate this and achieve consistency with split queries, but that may bring other performance costs and behavioral difference.</span></span>

<span data-ttu-id="75255-121">Рассмотрим запрос, который извлекает два уровня связанных коллекций с помощью `Include`:</span><span class="sxs-lookup"><span data-stu-id="75255-121">For example, consider a query that pulls in two levels of related collections using `Include`:</span></span>

```csharp
var artists = context.Artists
    .Include(e => e.Albums)
    .ToList();
```

<span data-ttu-id="75255-122">Если используется поставщик SQLite, EF Core по умолчанию создаст следующий SQL:</span><span class="sxs-lookup"><span data-stu-id="75255-122">By default, EF Core will generate the following SQL when using the SQLite provider:</span></span>

```sql
SELECT a."Id", a."Name", a0."Id", a0."ArtistId", a0."Title"
FROM "Artists" AS a
LEFT JOIN "Album" AS a0 ON a."Id" = a0."ArtistId"
ORDER BY a."Id", a0."Id"
```

<span data-ttu-id="75255-123">При использовании разделенных запросов вместо этого создается следующий код SQL.</span><span class="sxs-lookup"><span data-stu-id="75255-123">With split queries, the following SQL is generated instead:</span></span>

```sql
SELECT a."Id", a."Name"
FROM "Artists" AS a
ORDER BY a."Id"

SELECT a0."Id", a0."ArtistId", a0."Title", a."Id"
FROM "Artists" AS a
INNER JOIN "Album" AS a0 ON a."Id" = a0."ArtistId"
ORDER BY a."Id"
```

<span data-ttu-id="75255-124">Разделение запросов можно включить, разместив новый оператор `AsSplitQuery` в любом месте запроса LINQ или глобально в модели `OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="75255-124">Split queries can be enabled by placing the new `AsSplitQuery` operator anywhere in your LINQ query, or globally in your model's `OnConfiguring`.</span></span> <span data-ttu-id="75255-125">Дополнительные сведения см. в [полной документации по разделенным запросам](xref:core/querying/single-split-queries).</span><span class="sxs-lookup"><span data-stu-id="75255-125">For further information, [see the full documentation on split queries](xref:core/querying/single-split-queries).</span></span>

## <a name="simple-logging-and-improved-diagnostics"></a><span data-ttu-id="75255-126">Простое ведение журнала и улучшенная диагностика</span><span class="sxs-lookup"><span data-stu-id="75255-126">Simple logging and improved diagnostics</span></span>

<span data-ttu-id="75255-127">В EF Core 5.0 появился простой способ настройки ведения журнала с помощью нового метода `LogTo`.</span><span class="sxs-lookup"><span data-stu-id="75255-127">EF Core 5.0 introduces a simple way to set up logging via the new `LogTo` method.</span></span> <span data-ttu-id="75255-128">Следующий код задает вывод в консоль сообщений журнала, включая весь код SQL, создаваемый EF Core.</span><span class="sxs-lookup"><span data-stu-id="75255-128">The following will cause logging messages to be written to the console, including all SQL generated by EF Core:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder.LogTo(Console.WriteLine);
```

<span data-ttu-id="75255-129">Кроме того, теперь можно вызвать метод `ToQueryString` для любого запроса LINQ, извлекая код SQL, который будет выполнен запросом.</span><span class="sxs-lookup"><span data-stu-id="75255-129">In addition, it is now possible to call `ToQueryString` on any LINQ query, retrieving the SQL that the query would execute:</span></span>

```csharp
Console.WriteLine(
    ctx.Artists
    .Where(a => a.Name == "Pink Floyd")
    .ToQueryString());
```

<span data-ttu-id="75255-130">Наконец, различные типы EF Core были дополнены расширенным свойством `DebugView`, которое предоставляет подробные сведения о внутренних компонентах.</span><span class="sxs-lookup"><span data-stu-id="75255-130">Finally, various EF Core types have been fitted with an enhanced `DebugView` property which provides a detailed view into the internals.</span></span> <span data-ttu-id="75255-131">Например, можно обратиться к <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DebugView%2A?displayProperty=nameWithType>, чтобы узнать, какие сущности отслеживаются в данный момент времени.</span><span class="sxs-lookup"><span data-stu-id="75255-131">For example, <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DebugView%2A?displayProperty=nameWithType> can be consulted to see exactly which entities are being tracked in a given moment.</span></span>

<span data-ttu-id="75255-132">Дополнительные сведения см. [в документации по ведению журнала и перехватам](xref:core/logging-events-diagnostics/index).</span><span class="sxs-lookup"><span data-stu-id="75255-132">For further information, [see the documentation on logging and interception](xref:core/logging-events-diagnostics/index).</span></span>

## <a name="filtered-include"></a><span data-ttu-id="75255-133">Включение с фильтрацией</span><span class="sxs-lookup"><span data-stu-id="75255-133">Filtered include</span></span>

<span data-ttu-id="75255-134">Метод `Include` теперь поддерживает фильтрацию включенных сущностей.</span><span class="sxs-lookup"><span data-stu-id="75255-134">The `Include` method now supports filtering of the entities included:</span></span>

```csharp
var blogs = context.Blogs
    .Include(e => e.Posts.Where(p => p.Title.Contains("Cheese")))
    .ToList();
```

<span data-ttu-id="75255-135">Этот запрос возвращает блоги со всеми связанными записями, но только в том случае, если заголовок записи содержит Cheese.</span><span class="sxs-lookup"><span data-stu-id="75255-135">This query will return blogs together with each associated post, but only when the post title contains "Cheese".</span></span>

<span data-ttu-id="75255-136">Дополнительные сведения см. в [полной документации по разделенным запросам](xref:core/querying/related-data/eager#filtered-include).</span><span class="sxs-lookup"><span data-stu-id="75255-136">For further information, [see the full documentation on split queries](xref:core/querying/related-data/eager#filtered-include).</span></span>

## <a name="table-per-type-tpt-mapping"></a><span data-ttu-id="75255-137">Сопоставление типа "одна таблица на тип" (TPT)</span><span class="sxs-lookup"><span data-stu-id="75255-137">Table-per-type (TPT) mapping</span></span>

<span data-ttu-id="75255-138">По умолчанию в EF Core иерархия наследования типов .NET сопоставляется с одной таблицей базы данных.</span><span class="sxs-lookup"><span data-stu-id="75255-138">By default, EF Core maps an inheritance hierarchy of .NET types to a single database table.</span></span> <span data-ttu-id="75255-139">Это называется сопоставлением "одна таблица на иерархию" (TPH).</span><span class="sxs-lookup"><span data-stu-id="75255-139">This is known as table-per-hierarchy (TPH) mapping.</span></span> <span data-ttu-id="75255-140">В EF Core 5.0 каждый тип .NET в иерархии наследования также может сопоставляться с отдельной таблицей базы данных. Это называется сопоставлением "одна таблица на тип" (TPT).</span><span class="sxs-lookup"><span data-stu-id="75255-140">EF Core 5.0 also allows mapping each .NET type in an inheritance hierarchy to a different database table; known as table-per-type (TPT) mapping.</span></span>

<span data-ttu-id="75255-141">Например, возьмем следующую модель с сопоставленной иерархией.</span><span class="sxs-lookup"><span data-stu-id="75255-141">For example, consider this model with a mapped hierarchy:</span></span>

```csharp
public class Animal
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public class Cat : Animal
{
    public string EducationLevel { get; set; }
}

public class Dog : Animal
{
    public string FavoriteToy { get; set; }
}
```

<span data-ttu-id="75255-142">При использовании сопоставления "одна таблица на тип" (TPT) для каждого типа в иерархии создается таблица базы данных.</span><span class="sxs-lookup"><span data-stu-id="75255-142">With TPT, a database table is created for each type in the hierarchy:</span></span>

```sql
CREATE TABLE [Animals] (
    [Id] int NOT NULL IDENTITY,
    [Name] nvarchar(max) NULL,
    CONSTRAINT [PK_Animals] PRIMARY KEY ([Id])
);

CREATE TABLE [Cats] (
    [Id] int NOT NULL,
    [EdcuationLevel] nvarchar(max) NULL,
    CONSTRAINT [PK_Cats] PRIMARY KEY ([Id]),
    CONSTRAINT [FK_Cats_Animals_Id] FOREIGN KEY ([Id]) REFERENCES [Animals] ([Id]) ON DELETE NO ACTION,
);

CREATE TABLE [Dogs] (
    [Id] int NOT NULL,
    [FavoriteToy] nvarchar(max) NULL,
    CONSTRAINT [PK_Dogs] PRIMARY KEY ([Id]),
    CONSTRAINT [FK_Dogs_Animals_Id] FOREIGN KEY ([Id]) REFERENCES [Animals] ([Id]) ON DELETE NO ACTION,
);
```

<span data-ttu-id="75255-143">Дополнительные сведения см. в [полной документации по сопоставлению "одна таблица на тип" (TPT)](xref:core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="75255-143">For further information, [see the full documentation on TPT](xref:core/modeling/inheritance).</span></span>

## <a name="flexible-entity-mapping"></a><span data-ttu-id="75255-144">Гибкое сопоставление сущностей</span><span class="sxs-lookup"><span data-stu-id="75255-144">Flexible entity mapping</span></span>

<span data-ttu-id="75255-145">Типы сущностей обычно сопоставляются с таблицами или представлениями так, что EF Core будет извлекать содержимое таблицы или представления при запросе соответствующего типа.</span><span class="sxs-lookup"><span data-stu-id="75255-145">Entity types are commonly mapped to tables or views such that EF Core will pull back the contents of the table or view when querying for that type.</span></span> <span data-ttu-id="75255-146">EF Core 5.0 добавляет дополнительные варианты сопоставления, в которых сущность может быть сопоставлена с SQL-запросом (называемым определяющим запросом) или с функцией с табличным значением.</span><span class="sxs-lookup"><span data-stu-id="75255-146">EF Core 5.0 adds additional mapping options, where an entity can be mapped to a SQL query (called a "defining query"), or to a table-valued function (TVF):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Post>().ToSqlQuery(
        @"SELECT Id, Name, Category, BlogId FROM posts
          UNION ALL
          SELECT Id, Name, ""Legacy"", BlogId from legacy_posts");

    modelBuilder.Entity<Blog>().ToFunction("BlogsReturningFunction");
}
```

<span data-ttu-id="75255-147">Функции с табличным значением также могут сопоставляться с методом .NET, а не с DbSet, что позволяет передавать параметры. При этом сопоставление можно настроить с <xref:Microsoft.EntityFrameworkCore.RelationalModelBuilderExtensions.HasDbFunction%2A>.</span><span class="sxs-lookup"><span data-stu-id="75255-147">Table-valued functions can also be mapped to a .NET method rather than to a DbSet, allowing parameters to be passed; the mapping can be set up with <xref:Microsoft.EntityFrameworkCore.RelationalModelBuilderExtensions.HasDbFunction%2A>.</span></span>

<span data-ttu-id="75255-148">Наконец, теперь можно сопоставлять сущность с представлением при запросе (или с функцией или определяющим запросом) и с таблицей при обновлении.</span><span class="sxs-lookup"><span data-stu-id="75255-148">Finally, it is now possible to map an entity to a view when querying (or to a function or defining query), but to a table when updating:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder
        .Entity<Blog>()
        .ToTable("Blogs")
        .ToView("BlogsView");
}
```

## <a name="shared-type-entity-types-and-property-bags"></a><span data-ttu-id="75255-149">Общие типы сущностей и контейнеры свойств</span><span class="sxs-lookup"><span data-stu-id="75255-149">Shared-type entity types and property bags</span></span>

<span data-ttu-id="75255-150">В EF Core 5.0 один тип CLR может быть сопоставлен с несколькими различными типами сущностей, которые называются общими.</span><span class="sxs-lookup"><span data-stu-id="75255-150">EF Core 5.0 allows the same CLR type to be mapped to multiple different entity types; such types are known as shared-type entity types.</span></span> <span data-ttu-id="75255-151">Хотя с этой функцией можно использовать любой тип CLR, .NET `Dictionary` предлагает полезный вариант применения, который мы называем контейнерами свойств.</span><span class="sxs-lookup"><span data-stu-id="75255-151">While any CLR type can be used with this feature, .NET `Dictionary` offers a particularly compelling use-case which we call "property bags":</span></span>

```csharp
public class ProductsContext : DbContext
{
    public DbSet<Dictionary<string, object>> Products => Set<Dictionary<string, object>>("Product");

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.SharedTypeEntity<Dictionary<string, object>>("Product", b =>
        {
            b.IndexerProperty<int>("Id");
            b.IndexerProperty<string>("Name").IsRequired();
            b.IndexerProperty<decimal>("Price");
        });
    }
}
```

<span data-ttu-id="75255-152">Впоследствии можно выполнять запросы к этим сущностям и обновлять их так же, как и в случае с обычными типами сущностей с собственным выделенным типом CLR.</span><span class="sxs-lookup"><span data-stu-id="75255-152">These entities can then be queried and updated just like normal entity types with their own, dedicated CLR type.</span></span> <span data-ttu-id="75255-153">Дополнительные сведения можно найти в документации по [контейнерам свойств](xref:core/modeling/shadow-properties).</span><span class="sxs-lookup"><span data-stu-id="75255-153">More information can be found in the documentation on [property bags](xref:core/modeling/shadow-properties).</span></span>

## <a name="required-11-dependents"></a><span data-ttu-id="75255-154">Обязательные зависимости "один к одному"</span><span class="sxs-lookup"><span data-stu-id="75255-154">Required 1:1 dependents</span></span>

<span data-ttu-id="75255-155">В EF Core 3.1 зависимая сторона связи "один к одному" всегда считалась необязательной.</span><span class="sxs-lookup"><span data-stu-id="75255-155">In EF Core 3.1, the dependent end of a one-to-one relationship was always considered optional.</span></span> <span data-ttu-id="75255-156">Это было особенно очевидно при использовании принадлежащих сущностей, поскольку все их столбцы создавались в базе данных как допускающие значение NULL, даже если они были настроены в соответствии с требованиями модели.</span><span class="sxs-lookup"><span data-stu-id="75255-156">This was most apparent when using owned entities, as all the owned entity's column were created as nullable in the database, even if they were configured as required in the model.</span></span>

<span data-ttu-id="75255-157">В EF Core 5.0 навигацию к принадлежащей сущности теперь можно настроить как обязательную зависимость.</span><span class="sxs-lookup"><span data-stu-id="75255-157">In EF Core 5.0, a navigation to an owned entity can be configured as as a required dependent.</span></span> <span data-ttu-id="75255-158">Пример:</span><span class="sxs-lookup"><span data-stu-id="75255-158">For example:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Person>(b =>
    {
        b.OwnsOne(e => e.HomeAddress,
            b =>
            {
                b.Property(e => e.City).IsRequired();
                b.Property(e => e.Postcode).IsRequired();
            });
        b.Navigation(e => e.HomeAddress).IsRequired();
    });
}
```

## <a name="dbcontextfactory"></a><span data-ttu-id="75255-159">DbContextFactory</span><span class="sxs-lookup"><span data-stu-id="75255-159">DbContextFactory</span></span>

<span data-ttu-id="75255-160">В EF Core 5.0 введены методы `AddDbContextFactory` и `AddPooledDbContextFactory` для регистрации фабрики с целью создания экземпляров DbContext в контейнере внедрения зависимостей приложения. Это может быть полезно, если в коде приложения требуется создавать и удалять экземпляры контекста вручную.</span><span class="sxs-lookup"><span data-stu-id="75255-160">EF Core 5.0 introduces `AddDbContextFactory` and `AddPooledDbContextFactory` to register a factory for creating DbContext instances in the application's dependency injection (D.I.) container; this can be useful when application code needs to create and dispose context instances manually.</span></span>

```csharp
services.AddDbContextFactory<SomeDbContext>(b =>
    b.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Test"));
```

<span data-ttu-id="75255-161">На этом этапе службы приложений, такие как контроллеры ASP.NET Core, могут добавляться с помощью `IDbContextFactory<TContext>` и использовать его для создания экземпляров контекста.</span><span class="sxs-lookup"><span data-stu-id="75255-161">At this point, application services such as ASP.NET Core controllers can then be injected with `IDbContextFactory<TContext>`, and use it to instantiate context instances:</span></span>

```csharp
public class MyController
{
    private readonly IDbContextFactory<SomeDbContext> _contextFactory;

    public MyController(IDbContextFactory<SomeDbContext> contextFactory)
        => _contextFactory = contextFactory;

    public void DoSomeThing()
    {
        using (var context = _contextFactory.CreateDbContext())
        {
            // ...
        }
    }
}
```

<span data-ttu-id="75255-162">Дополнительные сведения см. в [полной документации по DbContextFactory](xref:core/dbcontext-configuration/index#using-a-dbcontext-factory-eg-for-blazor).</span><span class="sxs-lookup"><span data-stu-id="75255-162">For further information, [see the full documentation on DbContextFactory](xref:core/dbcontext-configuration/index#using-a-dbcontext-factory-eg-for-blazor).</span></span>

## <a name="sqlite-table-rebuilds"></a><span data-ttu-id="75255-163">Перестроение таблиц SQLite</span><span class="sxs-lookup"><span data-stu-id="75255-163">SQLite table rebuilds</span></span>

<span data-ttu-id="75255-164">По сравнению с другими базами данных таблицы SQLite относительно ограничены возможностями обработки схем. Например, в этой системе не поддерживается удаление столбца из существующей таблицы.</span><span class="sxs-lookup"><span data-stu-id="75255-164">Compared to other databases, SQLite is relatively limited in its schema manipulation capabilities; for example, dropping a column from an existing table is not supported.</span></span> <span data-ttu-id="75255-165">EF Core 5.0 позволяет обойти эти ограничения путем автоматического создания новой таблицы, копирования в нее данных из старой таблицы, удаления старой таблицы и переименования новой.</span><span class="sxs-lookup"><span data-stu-id="75255-165">EF Core 5.0 works around these limitations by automatically creating a new table, copying the data from the old table, dropping the old table and renaming the new one.</span></span> <span data-ttu-id="75255-166">Таким образом, таблица перестраивается, в связи с чем к ней можно безопасно применять не поддерживаемые ранее операции миграции.</span><span class="sxs-lookup"><span data-stu-id="75255-166">This "rebuilds" the table, and allows previously unsupported migration operations to be safely applied.</span></span>

<span data-ttu-id="75255-167">Дополнительные сведения о том, какие операции миграции теперь поддерживаются за счет перестроения таблиц, см. на [этой странице документации](xref:core/providers/sqlite/limitations#migrations-limitations).</span><span class="sxs-lookup"><span data-stu-id="75255-167">For details on which migration operations are now supported via table rebuilds, [see this documentation page](xref:core/providers/sqlite/limitations#migrations-limitations).</span></span>

## <a name="database-collations"></a><span data-ttu-id="75255-168">Параметры сортировки баз данных</span><span class="sxs-lookup"><span data-stu-id="75255-168">Database collations</span></span>

<span data-ttu-id="75255-169">В EF Core 5.0 реализована поддержка возможности задавать параметры сортировки текста на уровне базы данных, столбца или запроса.</span><span class="sxs-lookup"><span data-stu-id="75255-169">EF Core 5.0 introduces support for specifying text collations at the database, column or query level.</span></span> <span data-ttu-id="75255-170">Это позволяет гибким образом определять чувствительность к регистру и другим текстовым аспектам, не снижая при этом эффективность запросов.</span><span class="sxs-lookup"><span data-stu-id="75255-170">This allows case sensitivity and other textual aspects to be configured in a way that is both flexible and does not compromise query performance.</span></span>

<span data-ttu-id="75255-171">Например, следующий параметр настраивает столбец `Name` в SQL Server как чувствительный к регистру символов, в связи с чем все создаваемые для этого столбца индексы будут работать соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="75255-171">For example, the following will configure the `Name` column to be case-sensitive on SQL Server, and any indexes created on the column will function accordingly:</span></span>

```csharp
modelBuilder
    .Entity<User>()
    .Property(e => e.Name)
    .UseCollation("SQL_Latin1_General_CP1_CS_AS");
```

<span data-ttu-id="75255-172">Дополнительные сведения см. в [полной документации по параметрам сортировки и чувствительности к регистру](xref:core/miscellaneous/collations-and-case-sensitivity).</span><span class="sxs-lookup"><span data-stu-id="75255-172">For further information, [see the full documentation on collations and case sensitivity](xref:core/miscellaneous/collations-and-case-sensitivity).</span></span>

## <a name="event-counters"></a><span data-ttu-id="75255-173">Счетчики событий</span><span class="sxs-lookup"><span data-stu-id="75255-173">Event counters</span></span>

<span data-ttu-id="75255-174">В EF Core 5.0 предоставляются [счетчики событий](https://devblogs.microsoft.com/dotnet/introducing-diagnostics-improvements-in-net-core-3-0), которые можно использовать для отслеживания производительности приложения и выявления различных аномалий.</span><span class="sxs-lookup"><span data-stu-id="75255-174">EF Core 5.0 exposes [event counters](https://devblogs.microsoft.com/dotnet/introducing-diagnostics-improvements-in-net-core-3-0) which can be used to track your application's performance and spot various anomalies.</span></span> <span data-ttu-id="75255-175">Просто подключитесь к процессу, в котором выполняется EF, с помощью средства [dotnet-counters](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters).</span><span class="sxs-lookup"><span data-stu-id="75255-175">Simply attach to a process running EF with the [dotnet-counters](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters) tool:</span></span>

```console
> dotnet counters monitor Microsoft.EntityFrameworkCore -p 49496

[Microsoft.EntityFrameworkCore]
    Active DbContexts                                               1
    Execution Strategy Operation Failures (Count / 1 sec)           0
    Execution Strategy Operation Failures (Total)                   0
    Optimistic Concurrency Failures (Count / 1 sec)                 0
    Optimistic Concurrency Failures (Total)                         0
    Queries (Count / 1 sec)                                     1,755
    Queries (Total)                                            98,402
    Query Cache Hit Rate (%)                                      100
    SaveChanges (Count / 1 sec)                                     0
    SaveChanges (Total)                                             1
```

<span data-ttu-id="75255-176">Дополнительные сведения см. в [полной документации по счетчикам событий](xref:core/logging-events-diagnostics/event-counters).</span><span class="sxs-lookup"><span data-stu-id="75255-176">For further information, [see the full documentation on event counters](xref:core/logging-events-diagnostics/event-counters).</span></span>

## <a name="other-features"></a><span data-ttu-id="75255-177">Другие возможности</span><span class="sxs-lookup"><span data-stu-id="75255-177">Other features</span></span>

### <a name="model-building"></a><span data-ttu-id="75255-178">Построение модели</span><span class="sxs-lookup"><span data-stu-id="75255-178">Model building</span></span>

* <span data-ttu-id="75255-179">Были представлены API построения модели, позволяющие упростить настройку [функций сравнения значений](xref:core/modeling/value-comparers).</span><span class="sxs-lookup"><span data-stu-id="75255-179">Model building APIs have been introduced for easier configuration of [value comparers](xref:core/modeling/value-comparers).</span></span>
* <span data-ttu-id="75255-180">Теперь вычисляемые столбцы можно настроить как [*хранимые* или *виртуальные*](xref:core/modeling/generated-properties#computed-columns).</span><span class="sxs-lookup"><span data-stu-id="75255-180">Computed columns can now be configured as [*stored* or *virtual*](xref:core/modeling/generated-properties#computed-columns).</span></span>
* <span data-ttu-id="75255-181">Точность и масштаб теперь можно настраивать с помощью [текучего API](xref:core/modeling/entity-properties#precision-and-scale).</span><span class="sxs-lookup"><span data-stu-id="75255-181">Precision and scale can now be configured [via the Fluent API](xref:core/modeling/entity-properties#precision-and-scale).</span></span>
* <span data-ttu-id="75255-182">Были представлены новые API-интерфейсы построения модели для [свойств навигации](xref:core/modeling/relationships#configuring-navigation-properties).</span><span class="sxs-lookup"><span data-stu-id="75255-182">New model building APIs have been introduced for [navigation properties](xref:core/modeling/relationships#configuring-navigation-properties).</span></span>
* <span data-ttu-id="75255-183">Были представлены новые API-интерфейсы построения модели для полей, аналогичные свойствам.</span><span class="sxs-lookup"><span data-stu-id="75255-183">New model building APIs have been introduced for fields, similar to properties.</span></span>
* <span data-ttu-id="75255-184">Типы .NET [PhysicalAddress](/dotnet/api/system.net.networkinformation.physicaladdress) и [IPAddress](/dotnet/api/system.net.ipaddress) теперь можно сопоставлять со строковыми столбцами базы данных.</span><span class="sxs-lookup"><span data-stu-id="75255-184">The .NET [PhysicalAddress](/dotnet/api/system.net.networkinformation.physicaladdress) and [IPAddress](/dotnet/api/system.net.ipaddress) types can now be mapped to database string columns.</span></span>
* <span data-ttu-id="75255-185">Резервное поле теперь можно настраивать с помощью [нового атрибута `[BackingField]`](xref:core/modeling/backing-field).</span><span class="sxs-lookup"><span data-stu-id="75255-185">A backing field can now be configured via [the new `[BackingField]` attribute](xref:core/modeling/backing-field).</span></span>
* <span data-ttu-id="75255-186">Теперь разрешены резервные поля, допускающие значения NULL. За счет этого обеспечивается лучшая поддержка генерируемых хранилищем значений по умолчанию в тех случаях, когда значение по умолчанию CLR не является хорошим значением-меткой (то есть `bool`).</span><span class="sxs-lookup"><span data-stu-id="75255-186">Nullable backing fields are now allowed, providing better support for store-generated defaults where the CLR default isn't a good sentinel value (notable `bool`).</span></span>
* <span data-ttu-id="75255-187">С помощью нового атрибута `[Index]` можно указывать в типе сущности индекс вместо использования текучего API.</span><span class="sxs-lookup"><span data-stu-id="75255-187">A new `[Index]` attribute can be used on an entity type to specify an index, instead of using the Fluent API.</span></span>
* <span data-ttu-id="75255-188">С помощью нового атрибута `[Keyless]` можно настраивать тип сущности как [не имеющий ключа](xref:core/modeling/keyless-entity-types).</span><span class="sxs-lookup"><span data-stu-id="75255-188">A new `[Keyless]` attribute can be used to configure an entity type [as having no key](xref:core/modeling/keyless-entity-types).</span></span>
* <span data-ttu-id="75255-189">По умолчанию [EF Core теперь рассматривает дискриминатор как *полный*](xref:core/modeling/inheritance#table-per-hierarchy-and-discriminator-configuration), то есть не видит значения дискриминатора, которые не настроены приложением в модели.</span><span class="sxs-lookup"><span data-stu-id="75255-189">By default, [EF Core now regards discriminators as *complete*](xref:core/modeling/inheritance#table-per-hierarchy-and-discriminator-configuration), meaning that it expects to never see discriminator values not configured by the application in the model.</span></span> <span data-ttu-id="75255-190">Такое поведение позволяет в определенной степени повысить производительность и может быть отключено, если столбец дискриминатора может содержать неизвестные значения.</span><span class="sxs-lookup"><span data-stu-id="75255-190">This allows for some performance improvements, and can be disabled if your discriminator column might hold unknown values.</span></span>

### <a name="query"></a><span data-ttu-id="75255-191">Запрос</span><span class="sxs-lookup"><span data-stu-id="75255-191">Query</span></span>

* <span data-ttu-id="75255-192">В исключениях ошибок преобразования запросов теперь приводятся более явные причины возникновения сбоя, помогающие выявить проблему.</span><span class="sxs-lookup"><span data-stu-id="75255-192">Query translation failure exceptions now contain more explicit reasons about the reasons for the failure, to help pinpoint the issue.</span></span>
* <span data-ttu-id="75255-193">Запросы без отслеживания теперь могут выполнять [разрешение идентификаторов](xref:core/querying/tracking#identity-resolution), что позволяет исключить возвращение нескольких экземпляров сущности для одного и того же объекта базы данных.</span><span class="sxs-lookup"><span data-stu-id="75255-193">No-tracking queries can now perform [identity resolution](xref:core/querying/tracking#identity-resolution), avoiding multiple entity instances being returned for the same database object.</span></span>
* <span data-ttu-id="75255-194">Добавлена поддержка GroupBy с условными статистическими выражениями (например, `GroupBy(o => o.OrderDate).Select(g => g.Count(i => i.OrderDate != null))`).</span><span class="sxs-lookup"><span data-stu-id="75255-194">Added support for GroupBy with conditional aggregates (e.g. `GroupBy(o => o.OrderDate).Select(g => g.Count(i => i.OrderDate != null))`).</span></span>
* <span data-ttu-id="75255-195">Добавлена поддержка преобразования оператора Distinct над элементами группы перед выполнением статистического вычисления.</span><span class="sxs-lookup"><span data-stu-id="75255-195">Added support for translating the Distinct operator over group elements before aggregate.</span></span>
* <span data-ttu-id="75255-196">Преобразование [`Reverse`](/dotnet/api/system.linq.queryable.reverse).</span><span class="sxs-lookup"><span data-stu-id="75255-196">Translation of [`Reverse`](/dotnet/api/system.linq.queryable.reverse).</span></span>
* <span data-ttu-id="75255-197">Оптимизировано связанное с `DateTime` преобразование для SQL Server (например, [`DateDiffWeek`](xref:Microsoft.EntityFrameworkCore.SqlServerDbFunctionsExtensions.DateDiffWeek%2A), [`DateFromParts`](xref:Microsoft.EntityFrameworkCore.SqlServerDbFunctionsExtensions.DateFromParts%2A)).</span><span class="sxs-lookup"><span data-stu-id="75255-197">Improved translation around `DateTime` for SQL Server (e.g. [`DateDiffWeek`](xref:Microsoft.EntityFrameworkCore.SqlServerDbFunctionsExtensions.DateDiffWeek%2A), [`DateFromParts`](xref:Microsoft.EntityFrameworkCore.SqlServerDbFunctionsExtensions.DateFromParts%2A)).</span></span>
* <span data-ttu-id="75255-198">Преобразование новых методов для массивов байтов (например, [`Contains`](/dotnet/api/system.linq.enumerable.contains), [`Length`](/dotnet/api/system.array.length), [`SequenceEqual`](/dotnet/api/system.linq.enumerable.sequenceequal)).</span><span class="sxs-lookup"><span data-stu-id="75255-198">Translation of new methods on byte arrays (e.g. [`Contains`](/dotnet/api/system.linq.enumerable.contains), [`Length`](/dotnet/api/system.array.length), [`SequenceEqual`](/dotnet/api/system.linq.enumerable.sequenceequal)).</span></span>
* <span data-ttu-id="75255-199">Преобразование некоторых дополнительных побитовых операторов, например дополнения до двух.</span><span class="sxs-lookup"><span data-stu-id="75255-199">Translation of some additional bitwise operators, such as two's complement.</span></span>
* <span data-ttu-id="75255-200">Преобразование `FirstOrDefault` для строк.</span><span class="sxs-lookup"><span data-stu-id="75255-200">Translation of `FirstOrDefault` over strings.</span></span>
* <span data-ttu-id="75255-201">Оптимизировано преобразование запросов, связанных с семантикой значений NULL, что позволяет получить более строгие и эффективные запросы.</span><span class="sxs-lookup"><span data-stu-id="75255-201">Improved query translation around null semantics, resulting in tighter, more efficient queries.</span></span>
* <span data-ttu-id="75255-202">Сопоставленные пользователем функции теперь можно аннотировать для управления распространением значений NULL, что также позволяет получить более строгие и эффективные запросы.</span><span class="sxs-lookup"><span data-stu-id="75255-202">User-mapped functions can now be annotated to control null propagation, again resulting in tighter, more efficient queries.</span></span>
* <span data-ttu-id="75255-203">Значительно сокращен код SQL, содержащий блоки CASE.</span><span class="sxs-lookup"><span data-stu-id="75255-203">SQL containing CASE blocks is now considerably more concise.</span></span>
* <span data-ttu-id="75255-204">Функцию SQL Server [`DATELENGTH`](https://docs.microsoft.com/sql/t-sql/functions/datalength-transact-sql) теперь можно вызывать в запросах с помощью нового метода [`EF.Functions.DataLength`](xref:Microsoft.EntityFrameworkCore.SqlServerDbFunctionsExtensions.DataLength%2A).</span><span class="sxs-lookup"><span data-stu-id="75255-204">The SQL Server [`DATELENGTH`](https://docs.microsoft.com/sql/t-sql/functions/datalength-transact-sql) function can now be called in queries via the new [`EF.Functions.DataLength`](xref:Microsoft.EntityFrameworkCore.SqlServerDbFunctionsExtensions.DataLength%2A) method.</span></span>
* <span data-ttu-id="75255-205">`EnableDetailedErrors` добавляет [дополнительные сведения в исключения](xref:core/logging-events-diagnostics/simple-logging#detailed-query-exceptions).</span><span class="sxs-lookup"><span data-stu-id="75255-205">`EnableDetailedErrors` adds [additional details to exceptions](xref:core/logging-events-diagnostics/simple-logging#detailed-query-exceptions).</span></span>

### <a name="saving"></a><span data-ttu-id="75255-206">Сохранение</span><span class="sxs-lookup"><span data-stu-id="75255-206">Saving</span></span>

* <span data-ttu-id="75255-207">[Перехват](xref:core/logging-events-diagnostics/interceptors#savechanges-interception) и [события](xref:core/logging-events-diagnostics/events) SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="75255-207">SaveChanges [interception](xref:core/logging-events-diagnostics/interceptors#savechanges-interception) and [events](xref:core/logging-events-diagnostics/events).</span></span>
* <span data-ttu-id="75255-208">Реализованы API-интерфейсы для управления [точками сохранения транзакций](xref:core/saving/transactions#savepoints).</span><span class="sxs-lookup"><span data-stu-id="75255-208">APIs have been introduced for controlling [transaction savepoints](xref:core/saving/transactions#savepoints).</span></span> <span data-ttu-id="75255-209">Кроме того, EF Core будет автоматически создавать точку сохранения при вызове `SaveChanges`, если транзакция уже выполняется, и выполнять откат к этой точке в случае сбоя.</span><span class="sxs-lookup"><span data-stu-id="75255-209">In addition, EF Core will automatically create a savepoint when `SaveChanges` is called and a transaction is already in progress, and roll back to it in case of failure.</span></span>
* <span data-ttu-id="75255-210">Идентификатор транзакции может явно задаваться приложением, что позволяет упростить корреляцию событий транзакций в журнале и других местах.</span><span class="sxs-lookup"><span data-stu-id="75255-210">A transaction ID can be explicitly set by the application, allowing for easier correlation of transaction events in logging and elsewhere.</span></span>
* <span data-ttu-id="75255-211">Заданный по умолчанию максимальный размер пакета для SQL Server изменен до 42 по результатам анализа производительности пакетной обработки.</span><span class="sxs-lookup"><span data-stu-id="75255-211">The default maximum batch size for SQL Server has been changed to 42 based on an analysis of batching performance.</span></span>

### <a name="migrations-and-scaffolding"></a><span data-ttu-id="75255-212">Миграция и формирование шаблонов</span><span class="sxs-lookup"><span data-stu-id="75255-212">Migrations and scaffolding</span></span>

* <span data-ttu-id="75255-213">Таблицы теперь можно [исключать из миграции](xref:core/modeling/entity-types#excluding-from-migrations).</span><span class="sxs-lookup"><span data-stu-id="75255-213">Tables can now be [excluded from migrations](xref:core/modeling/entity-types#excluding-from-migrations).</span></span>
* <span data-ttu-id="75255-214">Новая команда [`dotnet ef migrations list`](xref:core/cli/dotnet#dotnet-ef-migrations-list) показывает, какие миграции еще не были применены к базе данных (команда [`Get-Migration`](xref:core/cli/powershell#get-migration) выполняет те же действия в консоли управления пакетами).</span><span class="sxs-lookup"><span data-stu-id="75255-214">A new [`dotnet ef migrations list`](xref:core/cli/dotnet#dotnet-ef-migrations-list) command now shows which migrations have not yet been applied to the database ([`Get-Migration`](xref:core/cli/powershell#get-migration) does the same in the Package Management Console).</span></span>
* <span data-ttu-id="75255-215">Скрипты миграции теперь содержат инструкции транзакций, необходимые для более эффективной обработки сбоев приложения миграции.</span><span class="sxs-lookup"><span data-stu-id="75255-215">Migrations scripts now contain transaction statements where appropriate to improve handling cases where migration application fails.</span></span>
* <span data-ttu-id="75255-216">Столбцы для несопоставленных базовых классов теперь упорядочиваются после столбцов для сопоставленных типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="75255-216">The columns for unmapped base classes are now ordered after other columns for mapped entity types.</span></span> <span data-ttu-id="75255-217">Обратите внимание, что это касается только вновь созданных таблиц: порядок столбцов для существующих таблиц остается неизменным.</span><span class="sxs-lookup"><span data-stu-id="75255-217">Note this only impacts newly created tables; the column order for existing tables remains unchanged.</span></span>
* <span data-ttu-id="75255-218">Теперь при создании миграции можно учитывать, является ли создаваемая миграция идемпотентной и будут ли выходные данные выполняться немедленно или создаваться в виде скрипта.</span><span class="sxs-lookup"><span data-stu-id="75255-218">Migration generation can now be aware if the migration being generated is idempotent, and whether the output will be executed immediately or generated as a script.</span></span>
* <span data-ttu-id="75255-219">Добавлены новые параметры командной строки, позволяющие задавать пространства имен для [миграции](xref:core/managing-schemas/migrations/index#namespaces) и [формирования шаблонов](xref:core/managing-schemas/scaffolding#directories-and-namespaces).</span><span class="sxs-lookup"><span data-stu-id="75255-219">New command-line parameters have been added for specifying namespaces in [Migrations](xref:core/managing-schemas/migrations/index#namespaces) and [scaffolding](xref:core/managing-schemas/scaffolding#directories-and-namespaces).</span></span>
* <span data-ttu-id="75255-220">Команда [dotnet ef database update](xref:core/cli/dotnet#dotnet-ef-database-update) теперь принимает новый параметр `--connection`, позволяющий задать строку подключения.</span><span class="sxs-lookup"><span data-stu-id="75255-220">The [dotnet ef database update](xref:core/cli/dotnet#dotnet-ef-database-update) command now accepts a new `--connection` parameter for specifying the connection string.</span></span>
* <span data-ttu-id="75255-221">При формировании шаблонов для существующих баз данных имена таблиц преобразуются в форму единственного числа, поэтому таблицы с именами `People` и `Addresses` будут формироваться по типам сущностей с именами `Person` и `Address`.</span><span class="sxs-lookup"><span data-stu-id="75255-221">Scaffolding existing databases now singularizes table names, so tables named `People` and `Addresses` will be scaffolded to entity types called `Person` and `Address`.</span></span> <span data-ttu-id="75255-222">[Исходные имена базы данных по-прежнему можно сохранить](xref:core/managing-schemas/scaffolding#preserving-names).</span><span class="sxs-lookup"><span data-stu-id="75255-222">[Original database names can still be preserved](xref:core/managing-schemas/scaffolding#preserving-names).</span></span>
* <span data-ttu-id="75255-223">Новый параметр [`--no-onconfiguring`](xref:core/cli/dotnet#dotnet-ef-dbcontext-scaffold) может указывать EF Core на необходимость исключить `OnModelConfiguring` при формировании шаблона модели.</span><span class="sxs-lookup"><span data-stu-id="75255-223">The new [`--no-onconfiguring`](xref:core/cli/dotnet#dotnet-ef-dbcontext-scaffold) option can instruct EF Core to exclude `OnModelConfiguring` when scaffolding a model.</span></span>

### <a name="cosmos"></a><span data-ttu-id="75255-224">Cosmos</span><span class="sxs-lookup"><span data-stu-id="75255-224">Cosmos</span></span>

* <span data-ttu-id="75255-225">Расширены [параметры подключения Cosmos](xref:core/providers/cosmos/index#cosmos-options).</span><span class="sxs-lookup"><span data-stu-id="75255-225">[Cosmos connection settings](xref:core/providers/cosmos/index#cosmos-options) have been expanded.</span></span>
* <span data-ttu-id="75255-226">Теперь оптимистическая блокировка [поддерживается в Cosmos с помощью тегов ETag](xref:core/providers/cosmos/index#optimistic-concurrency-with-etags).</span><span class="sxs-lookup"><span data-stu-id="75255-226">Optimistic concurrency is now [supported on Cosmos via the use of ETags](xref:core/providers/cosmos/index#optimistic-concurrency-with-etags).</span></span>
* <span data-ttu-id="75255-227">Новый метод `WithPartitionKey` позволяет включать в модель и в запросы [ключ секции](xref:core/providers/cosmos/index#partition-keys) Cosmos.</span><span class="sxs-lookup"><span data-stu-id="75255-227">The new `WithPartitionKey` method allows the Cosmos [partition key](xref:core/providers/cosmos/index#partition-keys) to be included both in the model and in queries.</span></span>
* <span data-ttu-id="75255-228">Строковые методы [`Contains`](/dotnet/api/system.string.contains), [`StartsWith`](/dotnet/api/system.string.startswith) и [`EndsWith`](/dotnet/api/system.string.endswith) теперь преобразованы для Cosmos.</span><span class="sxs-lookup"><span data-stu-id="75255-228">The string methods [`Contains`](/dotnet/api/system.string.contains), [`StartsWith`](/dotnet/api/system.string.startswith) and [`EndsWith`](/dotnet/api/system.string.endswith) are now translated for Cosmos.</span></span>
* <span data-ttu-id="75255-229">Оператор C# `is` теперь преобразован для Cosmos.</span><span class="sxs-lookup"><span data-stu-id="75255-229">The C# `is` operator is now translated on Cosmos.</span></span>

### <a name="sqlite"></a><span data-ttu-id="75255-230">Sqlite</span><span class="sxs-lookup"><span data-stu-id="75255-230">Sqlite</span></span>

* <span data-ttu-id="75255-231">Теперь поддерживаются вычисляемые столбцы.</span><span class="sxs-lookup"><span data-stu-id="75255-231">Computed columns are now supported.</span></span>
* <span data-ttu-id="75255-232">Теперь получение двоичных и строковых данных с помощью GetBytes, GetChars и GetTextReader осуществляется более эффективно за счет использования SqliteBlob и потоков.</span><span class="sxs-lookup"><span data-stu-id="75255-232">Retrieving binary and string data with GetBytes, GetChars, and GetTextReader is now more efficient by making use of SqliteBlob and streams.</span></span>
* <span data-ttu-id="75255-233">Инициализация SqliteConnection выполняется в отложенном режиме.</span><span class="sxs-lookup"><span data-stu-id="75255-233">Initialization of SqliteConnection is now lazy.</span></span>

### <a name="other"></a><span data-ttu-id="75255-234">Другие</span><span class="sxs-lookup"><span data-stu-id="75255-234">Other</span></span>

* <span data-ttu-id="75255-235">Теперь можно создавать прокси-объекты отслеживания изменений, которые автоматически реализуют интерфейсы [INotifyPropertyChanging](/dotnet/api/system.componentmodel.inotifypropertychanging) и [INotifyPropertyChanged](/dotnet/api/system.componentmodel.inotifypropertychanged).</span><span class="sxs-lookup"><span data-stu-id="75255-235">Change-tracking proxies can be generated that automatically implement [INotifyPropertyChanging](/dotnet/api/system.componentmodel.inotifypropertychanging) and [INotifyPropertyChanged](/dotnet/api/system.componentmodel.inotifypropertychanged).</span></span> <span data-ttu-id="75255-236">Таким образом обеспечивается альтернативный подход к отслеживанию изменений, при котором не требуется проверка на наличие изменений при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="75255-236">This provides an alternative approach to change-tracking that doesn't scan for changes when `SaveChanges` is called.</span></span>
* <span data-ttu-id="75255-237">Теперь возможно изменение <xref:System.Data.Common.DbConnection> или строки подключения в уже инициализированном экземпляре DbContext.</span><span class="sxs-lookup"><span data-stu-id="75255-237">A <xref:System.Data.Common.DbConnection> or connection string can now be changed on an already-initialized DbContext.</span></span>
* <span data-ttu-id="75255-238">Новый метод <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Clear%2A?displayProperty=nameWithType>.0#Microsoft_EntityFrameworkCore_ChangeTracking_ChangeTracker_Clear) очищает экземпляр DbContext от всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="75255-238">The new <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Clear%2A?displayProperty=nameWithType>.0#Microsoft_EntityFrameworkCore_ChangeTracking_ChangeTracker_Clear) method clears the DbContext of all tracked entities.</span></span> <span data-ttu-id="75255-239">Обычно это не требуется, если выполняется рекомендация по созданию нового экземпляра контекста с кратким сроком существования для каждой единицы работы.</span><span class="sxs-lookup"><span data-stu-id="75255-239">This should usually not be needed when using the best practice of creating a new, short-lived context instance for each unit-of-work.</span></span> <span data-ttu-id="75255-240">Однако если необходимо сбросить состояние экземпляра DbContext, то использование нового метода `Clear()` является более эффективным и надежным подходом, чем массовое отсоединение всех сущностей.</span><span class="sxs-lookup"><span data-stu-id="75255-240">However, if there is a need to reset the state of a DbContext instance, then using the new `Clear()` method is more efficient and robust than mass-detaching all entities.</span></span>
* <span data-ttu-id="75255-241">Программы командной строки EF Core теперь автоматически задают для переменных среды `ASPNETCORE_ENVIRONMENT` _и_ `DOTNET_ENVIRONMENT` значение Development.</span><span class="sxs-lookup"><span data-stu-id="75255-241">The EF Core command line tools now automatically configure the `ASPNETCORE_ENVIRONMENT` _and_ `DOTNET_ENVIRONMENT` environment variables to "Development".</span></span> <span data-ttu-id="75255-242">Это обеспечивает согласованную работу при использовании универсального узла и ASP.NET Core во время разработки.</span><span class="sxs-lookup"><span data-stu-id="75255-242">This brings the experience when using the generic host in line with the experience for ASP.NET Core during development.</span></span>
* <span data-ttu-id="75255-243">Пользовательские аргументы командной строки могут передаваться в <xref:Microsoft.EntityFrameworkCore.Design.IDesignTimeDbContextFactory%601>, что позволяет приложениям управлять способами создания и инициализации контекста.</span><span class="sxs-lookup"><span data-stu-id="75255-243">Custom command-line arguments can be flowed into <xref:Microsoft.EntityFrameworkCore.Design.IDesignTimeDbContextFactory%601>, allowing applications to control how the context is created and initialized.</span></span>
* <span data-ttu-id="75255-244">Теперь [в SQL Server можно настроить](xref:core/providers/sql-server/indexes#fill-factor) коэффициент заполнения индекса.</span><span class="sxs-lookup"><span data-stu-id="75255-244">The index fill factor can now be [configured on SQL Server](xref:core/providers/sql-server/indexes#fill-factor).</span></span>
* <span data-ttu-id="75255-245">Реализовано новое свойство <xref:Microsoft.EntityFrameworkCore.RelationalDatabaseFacadeExtensions.IsRelational%2A>, которое позволяет различать случаи использования реляционного и нереляционного поставщика (например, InMemory).</span><span class="sxs-lookup"><span data-stu-id="75255-245">The new <xref:Microsoft.EntityFrameworkCore.RelationalDatabaseFacadeExtensions.IsRelational%2A> property can be used to distinguish when using a relational provider and a non-relation provider (such as InMemory).</span></span>
