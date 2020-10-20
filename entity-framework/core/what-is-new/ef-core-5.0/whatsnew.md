---
title: Новые возможности EF Core 5.0
description: Обзор новых возможностей в EF Core 5.0
author: ajcvickers
ms.date: 09/10/2020
uid: core/what-is-new/ef-core-5.0/whatsnew
ms.openlocfilehash: 8fa45bf31cb5f1a7e35134f9513a40469719f8c2
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92065619"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="cdfc0-103">Новые возможности EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="cdfc0-103">What's New in EF Core 5.0</span></span>

<span data-ttu-id="cdfc0-104">Все функции, запланированные в EF Core 5.0, реализованы.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-104">All features planned for EF Core 5.0 have now been completed.</span></span> <span data-ttu-id="cdfc0-105">На этой странице представлен обзор интересных изменений, появившихся в каждой предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-105">This page contains an overview of interesting changes introduced in each preview.</span></span>

<span data-ttu-id="cdfc0-106">Эта страница не является аналогом статьи о [планировании для EF Core 5.0](xref:core/what-is-new/ef-core-5.0/plan).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-106">This page does not duplicate the [plan for EF Core 5.0](xref:core/what-is-new/ef-core-5.0/plan).</span></span> <span data-ttu-id="cdfc0-107">В плане описываются общие темы для EF Core 5.0, в том числе все, что планируется включить перед выпуском окончательной версии.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-107">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

## <a name="rc1"></a><span data-ttu-id="cdfc0-108">RC1</span><span class="sxs-lookup"><span data-stu-id="cdfc0-108">RC1</span></span>

### <a name="many-to-many"></a><span data-ttu-id="cdfc0-109">"Много ко многим"</span><span class="sxs-lookup"><span data-stu-id="cdfc0-109">Many-to-many</span></span>

<span data-ttu-id="cdfc0-110">В EF Core 5.0 поддерживаются связи "многие ко многим" без явного сопоставления таблицы соединения.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-110">EF Core 5.0 supports many-to-many relationships without explicitly mapping the join table.</span></span>

<span data-ttu-id="cdfc0-111">Например, рассмотрим такие типы сущностей:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-111">For example, consider these entity types:</span></span>

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

<span data-ttu-id="cdfc0-112">Обратите внимание, что `Post` содержит коллекцию `Tags`, а `Tag` содержит коллекцию `Posts`.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-112">Notice that `Post` contains a collection of `Tags`, and `Tag` contains a collection of `Posts`.</span></span> <span data-ttu-id="cdfc0-113">В соответствии с соглашением в EF Core 5.0 это распознается как отношение "многие ко многим".</span><span class="sxs-lookup"><span data-stu-id="cdfc0-113">EF Core 5.0 recognizes this as a many-to-many relationship by convention.</span></span> <span data-ttu-id="cdfc0-114">Поэтому код в `OnModelCreating` не требуется:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-114">This means no code is required in `OnModelCreating`:</span></span>

```csharp
public class BlogContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="cdfc0-115">Если для создания базы данных используются миграции (или `EnsureCreated`), EF Core автоматически создает таблицу соединения.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-115">When Migrations (or `EnsureCreated`) are used to create the database, EF Core will automatically create the join table.</span></span> <span data-ttu-id="cdfc0-116">Например, в SQL Server для этой модели EF Core создает:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-116">For example, on SQL Server for this model, EF Core generates:</span></span>

```sql
CREATE TABLE [Posts] (
    [Id] int NOT NULL IDENTITY,
    [Name] nvarchar(max) NULL,
    CONSTRAINT [PK_Posts] PRIMARY KEY ([Id])
);

CREATE TABLE [Tag] (
    [Id] int NOT NULL IDENTITY,
    [Text] nvarchar(max) NULL,
    CONSTRAINT [PK_Tag] PRIMARY KEY ([Id])
);

CREATE TABLE [PostTag] (
    [PostsId] int NOT NULL,
    [TagsId] int NOT NULL,
    CONSTRAINT [PK_PostTag] PRIMARY KEY ([PostsId], [TagsId]),
    CONSTRAINT [FK_PostTag_Posts_PostsId] FOREIGN KEY ([PostsId]) REFERENCES [Posts] ([Id]) ON DELETE CASCADE,
    CONSTRAINT [FK_PostTag_Tag_TagsId] FOREIGN KEY ([TagsId]) REFERENCES [Tag] ([Id]) ON DELETE CASCADE
);

CREATE INDEX [IX_PostTag_TagsId] ON [PostTag] ([TagsId]);
```

<span data-ttu-id="cdfc0-117">Создание и связывание сущностей `Blog` и `Post` приводит к автоматическому обновлению таблицы соединения.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-117">Creating and associating `Blog` and `Post` entities results in join table updates happening automatically.</span></span> <span data-ttu-id="cdfc0-118">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-118">For example:</span></span>

```csharp
var beginnerTag = new Tag {Text = "Beginner"};
var advancedTag = new Tag {Text = "Advanced"};
var efCoreTag = new Tag {Text = "EF Core"};

context.AddRange(
    new Post {Name = "EF Core 101", Tags = new List<Tag> {beginnerTag, efCoreTag}},
    new Post {Name = "Writing an EF database provider", Tags = new List<Tag> {advancedTag, efCoreTag}},
    new Post {Name = "Savepoints in EF Core", Tags = new List<Tag> {beginnerTag, efCoreTag}});

context.SaveChanges();
```

<span data-ttu-id="cdfc0-119">После вставки записей и тегов EF автоматически создаст строки в таблице соединения.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-119">After inserting the Posts and Tags, EF will then automatically create rows in the join table.</span></span> <span data-ttu-id="cdfc0-120">Например, в SQL Server.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-120">For example, on SQL Server:</span></span>

```sql
SET NOCOUNT ON;
INSERT INTO [PostTag] ([PostsId], [TagsId])
VALUES (@p6, @p7),
(@p8, @p9),
(@p10, @p11),
(@p12, @p13),
(@p14, @p15),
(@p16, @p17);
```

<span data-ttu-id="cdfc0-121">Включение и другие операции запросов выполняются так же, как для любой другой связи.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-121">For queries, Include and other query operations work just like for any other relationship.</span></span> <span data-ttu-id="cdfc0-122">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-122">For example:</span></span>

```csharp
foreach (var post in context.Posts.Include(e => e.Tags))
{
    Console.Write($"Post \"{post.Name}\" has tags");

    foreach (var tag in post.Tags)
    {
        Console.Write($" '{tag.Text}'");
    }
}
```

<span data-ttu-id="cdfc0-123">Создаваемый код SQL автоматически использует таблицу соединения, чтобы вернуть все связанные теги:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-123">The SQL generated uses the join table automatically to bring back all related Tags:</span></span>

```sql
SELECT [p].[Id], [p].[Name], [t0].[PostsId], [t0].[TagsId], [t0].[Id], [t0].[Text]
FROM [Posts] AS [p]
LEFT JOIN (
    SELECT [p0].[PostsId], [p0].[TagsId], [t].[Id], [t].[Text]
    FROM [PostTag] AS [p0]
    INNER JOIN [Tag] AS [t] ON [p0].[TagsId] = [t].[Id]
) AS [t0] ON [p].[Id] = [t0].[PostsId]
ORDER BY [p].[Id], [t0].[PostsId], [t0].[TagsId], [t0].[Id]
```

<span data-ttu-id="cdfc0-124">В отличие от EF6, EF Core позволяет полностью настраивать таблицу соединения.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-124">Unlike EF6, EF Core allows full customization of the join table.</span></span> <span data-ttu-id="cdfc0-125">Например, приведенный ниже код настраивает связь "многие ко многим", которая также содержит навигацию на сущность соединения и в которой сущность соединения содержит свойство полезной нагрузки.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-125">For example, the code below configures a many-to-many relationship that also has navigations to the join entity, and in which the join entity contains a payload property:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder
        .Entity<Post>()
        .HasMany(p => p.Tags)
        .WithMany(p => p.Posts)
        .UsingEntity<PostTag>(
            j => j
                .HasOne(pt => pt.Tag)
                .WithMany()
                .HasForeignKey(pt => pt.TagId),
            j => j
                .HasOne(pt => pt.Post)
                .WithMany()
                .HasForeignKey(pt => pt.PostId),
            j =>
            {
                j.Property(pt => pt.PublicationDate).HasDefaultValueSql("CURRENT_TIMESTAMP");
                j.HasKey(t => new { t.PostId, t.TagId });
            });
}
```

### <a name="map-entity-types-to-queries"></a><span data-ttu-id="cdfc0-126">Сопоставление типов сущностей с запросами</span><span class="sxs-lookup"><span data-stu-id="cdfc0-126">Map entity types to queries</span></span>

<span data-ttu-id="cdfc0-127">Типы сущностей обычно сопоставляются с таблицами или представлениями так, что EF Core будет извлекать содержимое таблицы или представления при запросе соответствующего типа.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-127">Entity types are commonly mapped to tables or views such that EF Core will pull back the contents of the table or view when querying for that type.</span></span> <span data-ttu-id="cdfc0-128">EF Core 5.0 позволяет сопоставить тип сущности с "определяющим запросом".</span><span class="sxs-lookup"><span data-stu-id="cdfc0-128">EF Core 5.0 allows an entity type to mapped to a "defining query".</span></span> <span data-ttu-id="cdfc0-129">(Эта возможность частично поддерживалось в предыдущих версиях, но была значительно улучшена и имеет другой синтаксис в EF Core 5.0.)</span><span class="sxs-lookup"><span data-stu-id="cdfc0-129">(This was partially supported in previous versions, but is much improved and has different syntax in EF Core 5.0.)</span></span>

<span data-ttu-id="cdfc0-130">Например, рассмотрим две таблицы: одна с актуальными записями, а другая — с устаревшими.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-130">For example, consider two tables; one with modern posts; the other with legacy posts.</span></span> <span data-ttu-id="cdfc0-131">Таблица с актуальными записями содержит ряд дополнительных столбцов, но для приложения необходимо объединить актуальные и устаревшие записи и сопоставить их с типом сущности со всеми необходимыми свойствами:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-131">The modern posts table has some additional columns, but for the purpose of our application we want both modern and legacy posts to be combined and mapped to an entity type with all necessary properties:</span></span>

```csharp
public class Post
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Category { get; set; }
    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

<span data-ttu-id="cdfc0-132">В EF Core 5.0 можно использовать `ToSqlQuery`, чтобы сопоставить этот тип сущности с запросом, который извлекает и объединяет строки из обеих таблиц:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-132">In EF Core 5.0, `ToSqlQuery` can be used to map this entity type to a query that pulls and combines rows from both tables:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Post>().ToSqlQuery(
        @"SELECT Id, Name, Category, BlogId FROM posts
          UNION ALL
          SELECT Id, Name, ""Legacy"", BlogId from legacy_posts");
}
```

<span data-ttu-id="cdfc0-133">Обратите внимание, что в таблице `legacy_posts` нет столбца `Category`, поэтому мы создадим значение по умолчанию для всех устаревших записей.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-133">Notice that the `legacy_posts` table does not have a `Category` column, so we instead synthesize a default value for all legacy posts.</span></span>

<span data-ttu-id="cdfc0-134">Затем этот тип сущности можно использовать обычным образом для запросов LINQ.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-134">This entity type can then be used in the normal way for LINQ queries.</span></span> <span data-ttu-id="cdfc0-135">Например, если выбран диапазон 10.0.0.0/20 для виртуальной сети, для пространства клиентских адресов можно выбрать 10.1.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-135">For example.</span></span> <span data-ttu-id="cdfc0-136">Запрос LINQ:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-136">the LINQ query:</span></span>

```csharp
var posts = context.Posts.Where(e => e.Blog.Name.Contains("Unicorn")).ToList();
```

<span data-ttu-id="cdfc0-137">Создает следующий код SQL в SQLite:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-137">Generates the following SQL on SQLite:</span></span>

```sql
SELECT "p"."Id", "p"."BlogId", "p"."Category", "p"."Name"
FROM (
    SELECT Id, Name, Category, BlogId FROM posts
    UNION ALL
    SELECT Id, Name, "Legacy", BlogId from legacy_posts
) AS "p"
INNER JOIN "Blogs" AS "b" ON "p"."BlogId" = "b"."Id"
WHERE ('Unicorn' = '') OR (instr("b"."Name", 'Unicorn') > 0)
```

<span data-ttu-id="cdfc0-138">Обратите внимание, что запрос, настроенный для типа сущности, используется в качестве отправной точки для составления полного запроса LINQ.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-138">Notice that the query configured for the entity type is used as a starting for composing the full LINQ query.</span></span>

### <a name="event-counters"></a><span data-ttu-id="cdfc0-139">Счетчики событий</span><span class="sxs-lookup"><span data-stu-id="cdfc0-139">Event counters</span></span>

<span data-ttu-id="cdfc0-140">[Счетчики событий .NET](https://devblogs.microsoft.com/dotnet/introducing-diagnostics-improvements-in-net-core-3-0/) позволяют эффективно предоставлять метрики производительности из приложения.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-140">[.NET event counters](https://devblogs.microsoft.com/dotnet/introducing-diagnostics-improvements-in-net-core-3-0/) are a way to efficiently expose performance metrics from an application.</span></span> <span data-ttu-id="cdfc0-141">В EF Core 5.0 счетчики событий включены в категорию `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-141">EF Core 5.0 includes event counters under the `Microsoft.EntityFrameworkCore` category.</span></span> <span data-ttu-id="cdfc0-142">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-142">For example:</span></span>

```
dotnet counters monitor Microsoft.EntityFrameworkCore -p 49496
```

<span data-ttu-id="cdfc0-143">Эта команда предписывает счетчикам dotnet начать сбор событий EF Core для процесса 49496.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-143">This tells dotnet counters to start collecting EF Core events for process 49496.</span></span> <span data-ttu-id="cdfc0-144">В консоли будут выведены выходные данные следующего вида:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-144">This generates output like this in the console:</span></span>

```
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

### <a name="property-bags"></a><span data-ttu-id="cdfc0-145">Контейнеры свойств</span><span class="sxs-lookup"><span data-stu-id="cdfc0-145">Property bags</span></span>

<span data-ttu-id="cdfc0-146">В EF Core 5.0 один тип CLR может быть сопоставлен с несколькими различными типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-146">EF Core 5.0 allows the same CLR type to be mapped to multiple different entity types.</span></span> <span data-ttu-id="cdfc0-147">Такие типы называются общими типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-147">Such types are known as shared-type entity types.</span></span> <span data-ttu-id="cdfc0-148">Эта возможность в сочетании со свойствами индексатора (включенным в предварительную версию 1) позволяет использовать контейнеры свойств как тип сущности.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-148">This feature combined with indexer properties (included in preview 1) allows property bags to be used as entity type.</span></span>

<span data-ttu-id="cdfc0-149">Например, в приведенном ниже DbContext тип BCL `Dictionary<string, object>` настраивается как общий тип сущности для продуктов и категорий.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-149">For example, the DbContext below configures the BCL type `Dictionary<string, object>` as a shared-type entity type for both products and categories.</span></span>

```csharp
public class ProductsContext : DbContext
{
    public DbSet<Dictionary<string, object>> Products => Set<Dictionary<string, object>>("Product");
    public DbSet<Dictionary<string, object>> Categories => Set<Dictionary<string, object>>("Category");

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.SharedTypeEntity<Dictionary<string, object>>("Category", b =>
        {
            b.IndexerProperty<string>("Description");
            b.IndexerProperty<int>("Id");
            b.IndexerProperty<string>("Name").IsRequired();
        });

        modelBuilder.SharedTypeEntity<Dictionary<string, object>>("Product", b =>
        {
            b.IndexerProperty<int>("Id");
            b.IndexerProperty<string>("Name").IsRequired();
            b.IndexerProperty<string>("Description");
            b.IndexerProperty<decimal>("Price");
            b.IndexerProperty<int?>("CategoryId");

            b.HasOne("Category", null).WithMany();
        });
    }
}
```

<span data-ttu-id="cdfc0-150">Объекты словаря ("контейнеры свойств") теперь можно добавлять в контекст как экземпляры сущности и сохранять.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-150">Dictionary objects ("property bags") can now be added to the context as entity instances and saved.</span></span> <span data-ttu-id="cdfc0-151">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-151">For example:</span></span>

```csharp
var beverages = new Dictionary<string, object>
{
    ["Name"] = "Beverages",
    ["Description"] = "Stuff to sip on"
};

context.Categories.Add(beverages);

context.SaveChanges();
```

<span data-ttu-id="cdfc0-152">Затем эти сущности можно запрашивать и обновлять обычным образом:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-152">These entities can then be queried and updated in the normal way:</span></span>

```csharp
var foods = context.Categories.Single(e => e["Name"] == "Foods");
var marmite = context.Products.Single(e => e["Name"] == "Marmite");

marmite["CategoryId"] = foods["Id"];
marmite["Description"] = "Yummy when spread _thinly_ on buttered Toast!";

context.SaveChanges();
```

### <a name="savechanges-interception-and-events"></a><span data-ttu-id="cdfc0-153">Перехват и события SaveChanges</span><span class="sxs-lookup"><span data-stu-id="cdfc0-153">SaveChanges interception and events</span></span>

<span data-ttu-id="cdfc0-154">В EF Core 5.0 появились события .NET и перехватчик EF Core, запускаемый при вызове SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-154">EF Core 5.0 introduces both .NET events and an EF Core interceptor triggered when SaveChanges is called.</span></span>

<span data-ttu-id="cdfc0-155">События просты в использовании, например:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-155">The events are simple to use; for example:</span></span>

```csharp
context.SavingChanges += (sender, args) =>
{
    Console.WriteLine($"Saving changes for {((DbContext)sender).Database.GetConnectionString()}");
};

context.SavedChanges += (sender, args) =>
{
    Console.WriteLine($"Saved {args.EntitiesSavedCount} changes for {((DbContext)sender).Database.GetConnectionString()}");
};
```

<span data-ttu-id="cdfc0-156">Обратите внимание на указанные ниже моменты.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-156">Notice that:</span></span>
* <span data-ttu-id="cdfc0-157">Отправителем события является экземпляр `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-157">The event sender is the `DbContext` instance</span></span>
* <span data-ttu-id="cdfc0-158">Аргументы события `SavedChanges` содержат число сущностей, сохраненных в базе данных.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-158">The args for the `SavedChanges` event contains the number of entities saved to the database</span></span>

<span data-ttu-id="cdfc0-159">Перехватчик определяется `ISaveChangesInterceptor`, но часто удобнее наследовать его от `SaveChangesInterceptor`, чтобы не приходилось реализовывать каждый метод.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-159">The interceptor is defined by `ISaveChangesInterceptor`, but it is often convienient to inherit from `SaveChangesInterceptor` to avoid implementing every method.</span></span> <span data-ttu-id="cdfc0-160">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-160">For example:</span></span>

```csharp
public class MySaveChangesInterceptor : SaveChangesInterceptor
{
    public override InterceptionResult<int> SavingChanges(
        DbContextEventData eventData,
        InterceptionResult<int> result)
    {
        Console.WriteLine($"Saving changes for {eventData.Context.Database.GetConnectionString()}");

        return result;
    }

    public override ValueTask<InterceptionResult<int>> SavingChangesAsync(
        DbContextEventData eventData,
        InterceptionResult<int> result,
        CancellationToken cancellationToken = new CancellationToken())
    {
        Console.WriteLine($"Saving changes asynchronously for {eventData.Context.Database.GetConnectionString()}");

        return new ValueTask<InterceptionResult<int>>(result);
    }
}
```

<span data-ttu-id="cdfc0-161">Обратите внимание на указанные ниже моменты.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-161">Notice that:</span></span>
* <span data-ttu-id="cdfc0-162">Перехватчик имеет как синхронные, так и асинхронные методы.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-162">The interceptor has both sync and async methods.</span></span> <span data-ttu-id="cdfc0-163">Это может быть полезно, если требуется выполнить асинхронный ввод-вывод, например запись на сервер аудита.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-163">This can be useful if you need to perform async I/O, such as writing to an audit server.</span></span>
* <span data-ttu-id="cdfc0-164">Перехватчик позволяет пропустить метод SaveChanges, используя механизм `InterceptionResult`, общий для всех перехватчиков.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-164">The interceptor allows SaveChanges to be skipped using the `InterceptionResult` mechanism common to all interceptors.</span></span>

<span data-ttu-id="cdfc0-165">Недостаток перехватчиков заключается в том, что они должны регистрироваться в объекте DbContext при его создании.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-165">The downside of interceptors is that they must be registered on the DbContext when it is being constructed.</span></span> <span data-ttu-id="cdfc0-166">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-166">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .AddInterceptors(new MySaveChangesInterceptor())
        .UseSqlite("Data Source = test.db");
```

<span data-ttu-id="cdfc0-167">События же, напротив, могут регистрироваться в экземпляре DbContext в любое время.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-167">In contrast, the events can be registered on the DbContext instance at any time.</span></span>

### <a name="exclude-tables-from-migrations"></a><span data-ttu-id="cdfc0-168">Исключение таблиц из миграций</span><span class="sxs-lookup"><span data-stu-id="cdfc0-168">Exclude tables from migrations</span></span>

<span data-ttu-id="cdfc0-169">Иногда полезно иметь один тип сущности, сопоставленный в нескольких экземплярах DbContext.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-169">It is sometimes useful to have a single entity type mapped in multiple DbContexts.</span></span> <span data-ttu-id="cdfc0-170">Это особенно справедливо при использовании [ограниченных контекстов](https://www.martinfowler.com/bliki/BoundedContext.html), для каждого из которых часто используется отдельный тип DbContext.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-170">This is especially true when using [bounded contexts](https://www.martinfowler.com/bliki/BoundedContext.html), for which it is common to have a different DbContext type for each bounded context.</span></span>

<span data-ttu-id="cdfc0-171">Например, тип `User` может требоваться как для контекста авторизации, так и для контекста отчетов.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-171">For example, a `User` type may be needed by both an authorization context and a reporting context.</span></span> <span data-ttu-id="cdfc0-172">При внесении изменения в тип `User` миграции для обоих экземпляров DbContext будут пытаться обновить базу данных.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-172">If a change is made to the `User` type, then migrations for both DbContexts will attempt to update the database.</span></span> <span data-ttu-id="cdfc0-173">Чтобы избежать этого, модель для одного из контекстов можно настроить так, чтобы таблица исключалась из миграций.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-173">To prevent this, the model for one of the contexts can be configured to exclude the table from its migrations.</span></span>

<span data-ttu-id="cdfc0-174">В приведенном ниже коде `AuthorizationContext` создает миграции для изменений в таблице `Users`, а `ReportingContext` не создает, благодаря чему предотвращаются конфликты миграций.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-174">In the code below, the `AuthorizationContext` will generate migrations for changes to the `Users` table, but the `ReportingContext` will not, preventing the migrations from clashing.</span></span>

```csharp
public class AuthorizationContext : DbContext
{
    public DbSet<User> Users { get; set; }
}

public class ReportingContext : DbContext
{
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>().ToTable("Users", t => t.ExcludeFromMigrations());
    }
}
```

### <a name="required-11-dependents"></a><span data-ttu-id="cdfc0-175">Обязательные зависимости "один к одному"</span><span class="sxs-lookup"><span data-stu-id="cdfc0-175">Required 1:1 dependents</span></span>

<span data-ttu-id="cdfc0-176">В EF Core 3.1 зависимая сторона связи "один к одному" всегда считалась необязательной.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-176">In EF Core 3.1, the dependent end of a one-to-one relationship was always considered optional.</span></span> <span data-ttu-id="cdfc0-177">Это было особенно очевидно при использовании принадлежащих сущностей.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-177">This was most apparent when using owned entities.</span></span> <span data-ttu-id="cdfc0-178">Например, рассмотрим следующую модель и конфигурацию:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-178">For example, consider the following model and configuration:</span></span>

```csharp
public class Person
{
    public int Id { get; set; }
    public string Name { get; set; }

    public Address HomeAddress { get; set; }
    public Address WorkAddress { get; set; }
}

public class Address
{
    public string Line1 { get; set; }
    public string Line2 { get; set; }
    public string City { get; set; }
    public string Region { get; set; }
    public string Country { get; set; }
    public string Postcode { get; set; }
}
```

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Person>(b =>
    {
        b.OwnsOne(e => e.HomeAddress,
            b =>
            {
                b.Property(e => e.Line1).IsRequired();
                b.Property(e => e.City).IsRequired();
                b.Property(e => e.Region).IsRequired();
                b.Property(e => e.Postcode).IsRequired();
            });

        b.OwnsOne(e => e.WorkAddress);
    });
}
```

<span data-ttu-id="cdfc0-179">В результате миграции создают следующую таблицу для SQLite:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-179">This results in Migrations creating the following table for SQLite:</span></span>

```sql
CREATE TABLE "People" (
    "Id" INTEGER NOT NULL CONSTRAINT "PK_People" PRIMARY KEY AUTOINCREMENT,
    "Name" TEXT NULL,
    "HomeAddress_Line1" TEXT NULL,
    "HomeAddress_Line2" TEXT NULL,
    "HomeAddress_City" TEXT NULL,
    "HomeAddress_Region" TEXT NULL,
    "HomeAddress_Country" TEXT NULL,
    "HomeAddress_Postcode" TEXT NULL,
    "WorkAddress_Line1" TEXT NULL,
    "WorkAddress_Line2" TEXT NULL,
    "WorkAddress_City" TEXT NULL,
    "WorkAddress_Region" TEXT NULL,
    "WorkAddress_Country" TEXT NULL,
    "WorkAddress_Postcode" TEXT NULL
);
```

<span data-ttu-id="cdfc0-180">Обратите внимание, что все столбцы допускают значение NULL, даже несмотря на то, что некоторые свойства `HomeAddress` настроены как обязательные.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-180">Notice that all the columns are nullable, even though some of the `HomeAddress` properties have been configured as required.</span></span> <span data-ttu-id="cdfc0-181">Кроме того, если все столбцы домашнего или рабочего адреса имеют значение NULL, при запросе `Person` EF Core оставит свойства `HomeAddress` и (или) `WorkAddress` со значением NULL вместо задания пустого экземпляра `Address`.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-181">Also, when querying for a `Person`, if all the columns for either the home or work address are null, then EF Core will leave the `HomeAddress` and/or `WorkAddress` properties as null, rather than setting an empty instance of `Address`.</span></span>

<span data-ttu-id="cdfc0-182">В EF Core 5.0 навигацию `HomeAddress` теперь можно настроить как обязательную зависимость.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-182">In EF Core 5.0, the `HomeAddress` navigation can now be configured as as a required dependent.</span></span> <span data-ttu-id="cdfc0-183">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-183">For example:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Person>(b =>
    {
        b.OwnsOne(e => e.HomeAddress,
            b =>
            {
                b.Property(e => e.Line1).IsRequired();
                b.Property(e => e.City).IsRequired();
                b.Property(e => e.Region).IsRequired();
                b.Property(e => e.Postcode).IsRequired();
            });
        b.Navigation(e => e.HomeAddress).IsRequired();

        b.OwnsOne(e => e.WorkAddress);
    });
}
```

<span data-ttu-id="cdfc0-184">Таблица, создаваемая миграциями, теперь будет включать столбцы, не допускающие значения NULL, для обязательных свойств требуемой зависимости:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-184">The table created by Migrations will now included non-nullable columns for the required properties of the required dependent:</span></span>

```sql
CREATE TABLE "People" (
    "Id" INTEGER NOT NULL CONSTRAINT "PK_People" PRIMARY KEY AUTOINCREMENT,
    "Name" TEXT NULL,
    "HomeAddress_Line1" TEXT NOT NULL,
    "HomeAddress_Line2" TEXT NULL,
    "HomeAddress_City" TEXT NOT NULL,
    "HomeAddress_Region" TEXT NOT NULL,
    "HomeAddress_Country" TEXT NULL,
    "HomeAddress_Postcode" TEXT NOT NULL,
    "WorkAddress_Line1" TEXT NULL,
    "WorkAddress_Line2" TEXT NULL,
    "WorkAddress_City" TEXT NULL,
    "WorkAddress_Region" TEXT NULL,
    "WorkAddress_Country" TEXT NULL,
    "WorkAddress_Postcode" TEXT NULL
);
```

<span data-ttu-id="cdfc0-185">Кроме того, EF Core теперь будет вызывать исключение при попытке сохранить владельца, который имеет обязательную зависимость со значением NULL.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-185">In addition, EF Core will now throw an exception if an attempt is made to save an owner which has a null required dependent.</span></span> <span data-ttu-id="cdfc0-186">В этом примере EF Core выдает исключение при попытке сохранить `Person` со значением NULL для `HomeAddress`.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-186">In this example, EF Core will throw when attempting to save a `Person` with a null `HomeAddress`.</span></span>

<span data-ttu-id="cdfc0-187">Наконец, EF Core будет по-прежнему создавать экземпляр обязательной зависимости, даже если все ее столбцы имеют значения NULL.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-187">Finally, EF Core will still create an instance of a required dependent even when all the columns for the required dependent have null values.</span></span>

### <a name="options-for-migration-generation"></a><span data-ttu-id="cdfc0-188">Варианты создания миграции</span><span class="sxs-lookup"><span data-stu-id="cdfc0-188">Options for migration generation</span></span>

<span data-ttu-id="cdfc0-189">В EF Core 5.0 улучшен контроль над созданием миграций в различных целях.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-189">EF Core 5.0 introduces greater control over generation of migrations for different purposes.</span></span> <span data-ttu-id="cdfc0-190">Обеспечены следующие возможности:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-190">This includes the ability to:</span></span>

* <span data-ttu-id="cdfc0-191">определение того, создается ли миграция для скрипта или для немедленного выполнения;</span><span class="sxs-lookup"><span data-stu-id="cdfc0-191">Know if the migration is being generated for a script or for immediate execution</span></span>
* <span data-ttu-id="cdfc0-192">определение того, создается ли идемпотентный скрипт;</span><span class="sxs-lookup"><span data-stu-id="cdfc0-192">Know if an idempotent script is being generated</span></span>
* <span data-ttu-id="cdfc0-193">определение того, должен ли скрипт исключать инструкции транзакций (см. раздел _Скрипты миграции с транзакциями_ ниже).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-193">Know if the script should exclude transaction statements (See _Migrations scripts with transactions_ below.)</span></span>

<span data-ttu-id="cdfc0-194">Это поведение задается перечислением `MigrationsSqlGenerationOptions`, которое теперь можно передать в `IMigrator.GenerateScript`.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-194">This behavior is specified by an the `MigrationsSqlGenerationOptions` enum, which can now be passed to `IMigrator.GenerateScript`.</span></span>

<span data-ttu-id="cdfc0-195">Кроме того, улучшено создание идемпотентных скриптов с вызовами `EXEC` в SQL Server, когда это необходимо.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-195">Also included in this work is better generation of idempotent scripts with calls to `EXEC` on SQL Server when needed.</span></span> <span data-ttu-id="cdfc0-196">Аналогичные улучшения доступны для скриптов, создаваемых другими поставщиками баз данных, включая PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-196">This work also enables similar improvements to the scripts generated by other database providers, including PostgreSQL.</span></span>

### <a name="migrations-scripts-with-transactions"></a><span data-ttu-id="cdfc0-197">Скрипты миграции с транзакциями</span><span class="sxs-lookup"><span data-stu-id="cdfc0-197">Migrations scripts with transactions</span></span>

<span data-ttu-id="cdfc0-198">Скрипты SQL, создаваемые миграциями, теперь содержат инструкции для начала и фиксации транзакций, необходимых для миграции.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-198">SQL scripts generated from migrations now contain statements to begin and commit transactions as appropriate for the migration.</span></span> <span data-ttu-id="cdfc0-199">Например, приведенный ниже скрипт миграции был создан на основе двух миграций.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-199">For example, the migration script below was generated from two migrations.</span></span> <span data-ttu-id="cdfc0-200">Обратите внимание, что каждая миграция теперь применяется внутри транзакции.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-200">Notice that each migration is now applied inside a transaction.</span></span>

```sql
BEGIN TRANSACTION;
GO

CREATE TABLE [Groups] (
    [Id] int NOT NULL IDENTITY,
    [Name] nvarchar(max) NULL,
    CONSTRAINT [PK_Groups] PRIMARY KEY ([Id])
);
GO

CREATE TABLE [Members] (
    [Id] int NOT NULL IDENTITY,
    [Name] nvarchar(max) NULL,
    [GroupId] int NULL,
    CONSTRAINT [PK_Members] PRIMARY KEY ([Id]),
    CONSTRAINT [FK_Members_Groups_GroupId] FOREIGN KEY ([GroupId]) REFERENCES [Groups] ([Id]) ON DELETE NO ACTION
);
GO

CREATE INDEX [IX_Members_GroupId] ON [Members] ([GroupId]);
GO

INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
VALUES (N'20200910194835_One', N'6.0.0-alpha.1.20460.2');
GO

COMMIT;
GO

BEGIN TRANSACTION;
GO

EXEC sp_rename N'[Groups].[Name]', N'GroupName', N'COLUMN';
GO

INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
VALUES (N'20200910195234_Two', N'6.0.0-alpha.1.20460.2');
GO

COMMIT;
```

<span data-ttu-id="cdfc0-201">Как было упомянуто в предыдущем разделе, такое использование транзакций можно отключить, если транзакции должны обрабатываться по-другому.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-201">As mentioned in the previous section, this use of transactions can be disabled if transactions need to be handled differently.</span></span>

### <a name="see-pending-migrations"></a><span data-ttu-id="cdfc0-202">См. раздел, посвященный ожидающим выполнения транзакциям.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-202">See pending migrations</span></span>

<span data-ttu-id="cdfc0-203">Эту функцию предоставил участник сообщества [@Psypher9](https://github.com/Psypher9).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-203">This feature was contributed from the community by [@Psypher9](https://github.com/Psypher9).</span></span> <span data-ttu-id="cdfc0-204">Большое спасибо за публикацию!</span><span class="sxs-lookup"><span data-stu-id="cdfc0-204">Many thanks for the contribution!</span></span>

<span data-ttu-id="cdfc0-205">Команда `dotnet ef migrations list` теперь показывает, какие миграции еще не были применены к базе данных.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-205">The `dotnet ef migrations list` command now shows which migrations have not yet been applied to the database.</span></span> <span data-ttu-id="cdfc0-206">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-206">For example:</span></span>

```
ajcvickers@avickers420u:~/AllTogetherNow/Daily$ dotnet ef migrations list
Build started...
Build succeeded.
20200910201647_One
20200910201708_Two
20200910202050_Three (Pending)
ajcvickers@avickers420u:~/AllTogetherNow/Daily$
```

<span data-ttu-id="cdfc0-207">Кроме того, теперь есть команда `Get-Migration` для консоли диспетчера пакетов с аналогичной функциональностью.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-207">In addition, there is now a `Get-Migration` command for the Package Manager Console with the same functionality.</span></span>

### <a name="modelbuilder-api-for-value-comparers"></a><span data-ttu-id="cdfc0-208">API ModelBuilder для функций сравнения значений</span><span class="sxs-lookup"><span data-stu-id="cdfc0-208">ModelBuilder API for value comparers</span></span>

<span data-ttu-id="cdfc0-209">В EF Core для правильного обнаружения изменений свойств пользовательских изменяемых типов [требуется функция сравнения значений](xref:core/modeling/value-comparers).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-209">EF Core properties for custom mutable types [require a value comparer](xref:core/modeling/value-comparers) for property changes to be detected correctly.</span></span> <span data-ttu-id="cdfc0-210">Теперь ее можно указать в процессе настройки преобразования значения для типа.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-210">This can now be specified as part of configuring the value conversion for the type.</span></span> <span data-ttu-id="cdfc0-211">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-211">For example:</span></span>

```csharp
modelBuilder
    .Entity<EntityType>()
    .Property(e => e.MyProperty)
    .HasConversion(
        v => JsonSerializer.Serialize(v, null),
        v => JsonSerializer.Deserialize<List<int>>(v, null),
        new ValueComparer<List<int>>(
            (c1, c2) => c1.SequenceEqual(c2),
            c => c.Aggregate(0, (a, v) => HashCode.Combine(a, v.GetHashCode())),
            c => c.ToList()));
```

### <a name="entityentry-trygetvalue-methods"></a><span data-ttu-id="cdfc0-212">Методы EntityEntry TryGetValue</span><span class="sxs-lookup"><span data-stu-id="cdfc0-212">EntityEntry TryGetValue methods</span></span>

<span data-ttu-id="cdfc0-213">Эту функцию предоставил участник сообщества [@m4ss1m0g](https://github.com/m4ss1m0g).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-213">This feature was contributed from the community by [@m4ss1m0g](https://github.com/m4ss1m0g).</span></span> <span data-ttu-id="cdfc0-214">Большое спасибо за публикацию!</span><span class="sxs-lookup"><span data-stu-id="cdfc0-214">Many thanks for the contribution!</span></span>

<span data-ttu-id="cdfc0-215">Метод `TryGetValue` был добавлен в `EntityEntry.CurrentValues` и `EntityEntry.OriginalValues`.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-215">A `TryGetValue` method has been added to `EntityEntry.CurrentValues` and `EntityEntry.OriginalValues`.</span></span> <span data-ttu-id="cdfc0-216">Это позволяет запрашивать значение свойства, не проверяя предварительно, сопоставлено ли свойство в модели EF.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-216">This allows the value of a property to be requested without first checking if the property is mapped in the EF model.</span></span> <span data-ttu-id="cdfc0-217">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-217">For example:</span></span>

```csharp
if (entry.CurrentValues.TryGetValue(propertyName, out var value))
{
    Console.WriteLine(value);
}
```

### <a name="default-max-batch-size-for-sql-server"></a><span data-ttu-id="cdfc0-218">Максимальный размер пакета по умолчанию для SQL Server</span><span class="sxs-lookup"><span data-stu-id="cdfc0-218">Default max batch size for SQL Server</span></span>

<span data-ttu-id="cdfc0-219">Начиная с EF Core 5.0 максимальный размер пакета по умолчанию для SaveChanges в SQL Server равен 42.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-219">Starting with EF Core 5.0, the default maximum batch size for SaveChanges on SQL Server is now 42.</span></span> <span data-ttu-id="cdfc0-220">Как хорошо известно, это также ответ на главный вопрос жизни, вселенной и вообще.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-220">As is well known, this is also the answer to the Ultimate Question of Life, the Universe, and Everything.</span></span> <span data-ttu-id="cdfc0-221">Однако, вероятно, это совпадение, так как данное значение было получено в результате [анализа производительности пакетной обработки](https://github.com/dotnet/efcore/issues/9270).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-221">However, this is probably a coincidence, since the value was obtained through [analysis of batching performance](https://github.com/dotnet/efcore/issues/9270).</span></span> <span data-ttu-id="cdfc0-222">Не думаем, что мы нашли окончательный ответ, хотя вполне возможно, что Земля была создана лишь с целью познать, почему SQL Server работает именно так.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-222">We do not believe that we have discovered a form of the Ultimate Question, although it does seem somewhat plausible that the Earth was created to understand why SQL Server works the way it does.</span></span>

### <a name="default-environment-to-development"></a><span data-ttu-id="cdfc0-223">Использование среды разработки по умолчанию</span><span class="sxs-lookup"><span data-stu-id="cdfc0-223">Default environment to Development</span></span>

<span data-ttu-id="cdfc0-224">Программы командной строки EF Core теперь автоматически задают для переменных среды `ASPNETCORE_ENVIRONMENT` _и_ `DOTNET_ENVIRONMENT` значение Development.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-224">The EF Core command line tools now automatically configure the `ASPNETCORE_ENVIRONMENT` _and_ `DOTNET_ENVIRONMENT` environment variables to "Development".</span></span> <span data-ttu-id="cdfc0-225">Это обеспечивает согласованную работу при использовании универсального узла и ASP.NET Core во время разработки.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-225">This brings the experience when using the generic host in line with the experience for ASP.NET Core during development.</span></span> <span data-ttu-id="cdfc0-226">См. описание проблемы [№ 19903](https://github.com/dotnet/efcore/issues/19903).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-226">See [#19903](https://github.com/dotnet/efcore/issues/19903).</span></span>

### <a name="better-migrations-column-ordering"></a><span data-ttu-id="cdfc0-227">Оптимизация порядка столбцов при миграции</span><span class="sxs-lookup"><span data-stu-id="cdfc0-227">Better migrations column ordering</span></span>

<span data-ttu-id="cdfc0-228">Столбцы для несопоставленных базовых классов теперь упорядочиваются после столбцов для сопоставленных типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-228">The columns for unmapped base classes are now ordered after other columns for mapped entity types.</span></span> <span data-ttu-id="cdfc0-229">Обратите внимание, что это касается только новых создаваемых таблиц.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-229">Note this only impacts newly created tables.</span></span> <span data-ttu-id="cdfc0-230">Порядок столбцов для существующих таблиц не меняется.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-230">The column order for existing tables remains unchanged.</span></span> <span data-ttu-id="cdfc0-231">См. описание проблемы [№ 11314](https://github.com/dotnet/efcore/issues/11314).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-231">See [#11314](https://github.com/dotnet/efcore/issues/11314).</span></span>

### <a name="query-improvements"></a><span data-ttu-id="cdfc0-232">Улучшения запросов</span><span class="sxs-lookup"><span data-stu-id="cdfc0-232">Query improvements</span></span>

<span data-ttu-id="cdfc0-233">В EF Core 5.0 RC1 появились дополнительные улучшения преобразования запросов.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-233">EF Core 5.0 RC1 contains some additional query translation improvements:</span></span>

* <span data-ttu-id="cdfc0-234">Преобразование `is` в Cosmos — см. описание проблемы [№ 16391](https://github.com/dotnet/efcore/issues/16391).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-234">Translation of `is` on Cosmos--see [#16391](https://github.com/dotnet/efcore/issues/16391)</span></span>
* <span data-ttu-id="cdfc0-235">Сопоставленные пользователем функции теперь можно аннотировать для управления распространением значений NULL — см. описание проблемы [№ 19609](https://github.com/dotnet/efcore/issues/19609).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-235">User-mapped functions can now be annotated to control null propagation--see [#19609](https://github.com/dotnet/efcore/issues/19609)</span></span>
* <span data-ttu-id="cdfc0-236">Поддержка преобразования GroupBy с условными статистическими выражениями — см. описание проблемы [№ 11711](https://github.com/dotnet/efcore/issues/11711).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-236">Support for translation of GroupBy with conditional aggregates--see [#11711](https://github.com/dotnet/efcore/issues/11711)</span></span>
* <span data-ttu-id="cdfc0-237">Преобразование оператора Distinct с групповым элементом перед статистическим выражением — см. описание проблемы [№ 17376](https://github.com/dotnet/efcore/issues/17376).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-237">Translation of Distinct operator over group element before aggregate--see [#17376](https://github.com/dotnet/efcore/issues/17376)</span></span>

### <a name="model-building-for-fields"></a><span data-ttu-id="cdfc0-238">Формирование модели для полей</span><span class="sxs-lookup"><span data-stu-id="cdfc0-238">Model building for fields</span></span>

<span data-ttu-id="cdfc0-239">Наконец, в EF Core RC1 теперь можно использовать лямбда-методы в ModelBuilder как для полей, так и для свойств.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-239">Finally for RC1, EF Core now allows use of the lambda methods in the ModelBuilder for fields as well as properties.</span></span> <span data-ttu-id="cdfc0-240">Например, если свойства вам по какой-либо причине не нравятся и вы хотите использовать открытые поля, теперь такие поля можно сопоставить с помощью построителей лямбда-выражений:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-240">For example, if you are averse to properties for some reason and decide to use public fields, then these fields can now be mapped using the lambda builders:</span></span>

```csharp
public class Post
{
    public int Id;
    public string Name;
    public string Category;
    public int BlogId;
    public Blog Blog;
}

public class Blog
{
    public int Id;
    public string Name;
    public ICollection<Post> Posts;
}
```

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>(b =>
    {
        b.Property(e => e.Id);
        b.Property(e => e.Name);
    });

    modelBuilder.Entity<Post>(b =>
    {
        b.Property(e => e.Id);
        b.Property(e => e.Name);
        b.Property(e => e.Category);
        b.Property(e => e.BlogId);
        b.HasOne(e => e.Blog).WithMany(e => e.Posts);
    });
}
```

<span data-ttu-id="cdfc0-241">Однако, хотя такая возможность и имеется, мы не рекомендуем пользоваться ею.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-241">While this is now possible, we are certainly not recommending that you do this.</span></span> <span data-ttu-id="cdfc0-242">Кроме того, обратите внимание, что возможности сопоставления полей в EF Core не были расширены. Просто вместо строковых методов, которые раньше требовались всегда, теперь можно использовать лямбда-методы.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-242">Also, note that this does not add any additional field mapping capabilities to EF Core, it only allows the lambda methods to be used instead of always requiring the string methods.</span></span> <span data-ttu-id="cdfc0-243">Это бывает полезно нечасто, так как поля редко являются открытыми.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-243">This is seldom useful since fields are rarely public.</span></span>

## <a name="preview-8"></a><span data-ttu-id="cdfc0-244">предварительная версия 8</span><span class="sxs-lookup"><span data-stu-id="cdfc0-244">Preview 8</span></span>

### <a name="table-per-type-tpt-mapping"></a><span data-ttu-id="cdfc0-245">Сопоставление типа "одна таблица на тип" (TPT)</span><span class="sxs-lookup"><span data-stu-id="cdfc0-245">Table-per-type (TPT) mapping</span></span>

<span data-ttu-id="cdfc0-246">По умолчанию в EF Core иерархия наследования типов .NET сопоставляется с одной таблицей базы данных.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-246">By default, EF Core maps an inheritance hierarchy of .NET types to a single database table.</span></span> <span data-ttu-id="cdfc0-247">Это называется сопоставлением "одна таблица на иерархию" (TPH).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-247">This is known as table-per-hierarchy (TPH) mapping.</span></span> <span data-ttu-id="cdfc0-248">В EF Core 5.0 каждый тип .NET в иерархии наследования также может сопоставляться с отдельной таблицей базы данных. Это называется сопоставлением "одна таблица на тип" (TPT).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-248">EF Core 5.0 also allows mapping each .NET type in an inheritance hierarchy to a different database table; known as table-per-type (TPT) mapping.</span></span>

<span data-ttu-id="cdfc0-249">Например, возьмем следующую модель с сопоставленной иерархией.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-249">For example, consider this model with a mapped hierarchy:</span></span>

```csharp
public class Animal
{
    public int Id { get; set; }
    public string Species { get; set; }
}

public class Pet : Animal
{
    public string Name { get; set; }
}

public class Cat : Pet
{
    public string EducationLevel { get; set; }
}

public class Dog : Pet
{
    public string FavoriteToy { get; set; }
}
```

<span data-ttu-id="cdfc0-250">По умолчанию в EF Core она будет сопоставлена с одной таблицей.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-250">By default, EF Core will map this to a single table:</span></span>

```sql
CREATE TABLE [Animals] (
    [Id] int NOT NULL IDENTITY,
    [Species] nvarchar(max) NULL,
    [Discriminator] nvarchar(max) NOT NULL,
    [Name] nvarchar(max) NULL,
    [EdcuationLevel] nvarchar(max) NULL,
    [FavoriteToy] nvarchar(max) NULL,
    CONSTRAINT [PK_Animals] PRIMARY KEY ([Id])
);
```

<span data-ttu-id="cdfc0-251">Однако сопоставление каждого типа сущности с отдельной таблицей приведет к тому, что для каждого типа будет одна таблица.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-251">However, mapping each entity type to a different table will instead result in one table per type:</span></span>

```sql
CREATE TABLE [Animals] (
    [Id] int NOT NULL IDENTITY,
    [Species] nvarchar(max) NULL,
    CONSTRAINT [PK_Animals] PRIMARY KEY ([Id])
);

CREATE TABLE [Pets] (
    [Id] int NOT NULL,
    [Name] nvarchar(max) NULL,
    CONSTRAINT [PK_Pets] PRIMARY KEY ([Id]),
    CONSTRAINT [FK_Pets_Animals_Id] FOREIGN KEY ([Id]) REFERENCES [Animals] ([Id]) ON DELETE NO ACTION
);

CREATE TABLE [Cats] (
    [Id] int NOT NULL,
    [EdcuationLevel] nvarchar(max) NULL,
    CONSTRAINT [PK_Cats] PRIMARY KEY ([Id]),
    CONSTRAINT [FK_Cats_Animals_Id] FOREIGN KEY ([Id]) REFERENCES [Animals] ([Id]) ON DELETE NO ACTION,
    CONSTRAINT [FK_Cats_Pets_Id] FOREIGN KEY ([Id]) REFERENCES [Pets] ([Id]) ON DELETE NO ACTION
);

CREATE TABLE [Dogs] (
    [Id] int NOT NULL,
    [FavoriteToy] nvarchar(max) NULL,
    CONSTRAINT [PK_Dogs] PRIMARY KEY ([Id]),
    CONSTRAINT [FK_Dogs_Animals_Id] FOREIGN KEY ([Id]) REFERENCES [Animals] ([Id]) ON DELETE NO ACTION,
    CONSTRAINT [FK_Dogs_Pets_Id] FOREIGN KEY ([Id]) REFERENCES [Pets] ([Id]) ON DELETE NO ACTION
);
```

<span data-ttu-id="cdfc0-252">Обратите внимание, что возможность создания ограничений внешнего ключа, как показано выше, появилась после создания ветви кода для предварительной версии 8.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-252">Note that creation of the foreign key constraints shown above were added after branching the code for preview 8.</span></span>

<span data-ttu-id="cdfc0-253">Типы сущностей можно сопоставлять с отдельными таблицами с помощью атрибутов сопоставления:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-253">Entity types can be mapped to different tables using mapping attributes:</span></span>

```csharp
[Table("Animals")]
public class Animal
{
    public int Id { get; set; }
    public string Species { get; set; }
}

[Table("Pets")]
public class Pet : Animal
{
    public string Name { get; set; }
}

[Table("Cats")]
public class Cat : Pet
{
    public string EdcuationLevel { get; set; }
}

[Table("Dogs")]
public class Dog : Pet
{
    public string FavoriteToy { get; set; }
}
```

<span data-ttu-id="cdfc0-254">Или с помощью конфигурации `ModelBuilder`:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-254">Or using `ModelBuilder` configuration:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Animal>().ToTable("Animals");
    modelBuilder.Entity<Pet>().ToTable("Pets");
    modelBuilder.Entity<Cat>().ToTable("Cats");
    modelBuilder.Entity<Dog>().ToTable("Dogs");
}
```

<span data-ttu-id="cdfc0-255">Документация отслеживается по проблеме [#1979](https://github.com/dotnet/EntityFramework.Docs/issues/1979).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-255">Documentation is tracked by issue [#1979](https://github.com/dotnet/EntityFramework.Docs/issues/1979).</span></span>

### <a name="migrations-rebuild-sqlite-tables"></a><span data-ttu-id="cdfc0-256">Миграции: перестроение таблиц SQLite</span><span class="sxs-lookup"><span data-stu-id="cdfc0-256">Migrations: Rebuild SQLite tables</span></span>

<span data-ttu-id="cdfc0-257">По сравнению с другими базами данных SQLite имеет относительно ограниченные возможности работы со схемами.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-257">Compared to other database, SQLite is relatively limited in its schema manipulation capabilities.</span></span> <span data-ttu-id="cdfc0-258">Например, для удаления столбца из существующей таблицы требуется удалить и повторно создать всю таблицу.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-258">For example, dropping a column from an existing table requires that the entire table be dropped and re-created.</span></span> <span data-ttu-id="cdfc0-259">Функция миграции в EF Core 5.0 теперь поддерживает автоматическое перестроение таблицы, если этого требуют изменения схемы.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-259">EF Core 5.0 Migrations now supports automatic rebuilding the table for schema changes that require it.</span></span>

<span data-ttu-id="cdfc0-260">Например, представьте, что имеется таблица `Unicorns`, созданная для типа сущности `Unicorn`.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-260">For example, imagine we have a `Unicorns` table created for a `Unicorn` entity type:</span></span>

```csharp
public class Unicorn
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }
}
```

```sql
CREATE TABLE "Unicorns" (
    "Id" INTEGER NOT NULL CONSTRAINT "PK_Unicorns" PRIMARY KEY AUTOINCREMENT,
    "Name" TEXT NULL,
    "Age" INTEGER NOT NULL
);
```

<span data-ttu-id="cdfc0-261">Но оказывается, что хранить сведения о возрасте единорога не имеет смысла, поэтому мы удалим это свойство, добавим новую миграцию и обновим базу данных.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-261">We then learn that storing the age of a unicorn is considered very rude, so let's remove that property, add a new migration, and update the database.</span></span> <span data-ttu-id="cdfc0-262">В EF Core 3.1 это изменение завершится ошибкой, так как столбец удалить нельзя.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-262">This update will fail when using EF Core 3.1 because the column cannot be dropped.</span></span> <span data-ttu-id="cdfc0-263">В EF Core 5.0 функция миграция перестроит таблицу.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-263">In EF Core 5.0, Migrations will instead rebuild the table:</span></span>

```sql
CREATE TABLE "ef_temp_Unicorns" (
    "Id" INTEGER NOT NULL CONSTRAINT "PK_Unicorns" PRIMARY KEY AUTOINCREMENT,
    "Name" TEXT NULL
);

INSERT INTO "ef_temp_Unicorns" ("Id", "Name")
SELECT "Id", "Name"
FROM Unicorns;

PRAGMA foreign_keys = 0;

DROP TABLE "Unicorns";

ALTER TABLE "ef_temp_Unicorns" RENAME TO "Unicorns";

PRAGMA foreign_keys = 1;
```

<span data-ttu-id="cdfc0-264">Обратите внимание на указанные ниже моменты.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-264">Notice that:</span></span>
* <span data-ttu-id="cdfc0-265">Для новой таблицы создается временная таблица с требуемой схемой.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-265">A temporary table is created with the desired schema for the new table</span></span>
* <span data-ttu-id="cdfc0-266">Данные копируются из текущей таблицы во временную.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-266">Data is copied from the current table into the temporary table</span></span>
* <span data-ttu-id="cdfc0-267">Принудительное применение внешнего ключа отключается.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-267">Foreign key enforcement is switched off</span></span>
* <span data-ttu-id="cdfc0-268">Текущая таблица удаляется.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-268">The current table is dropped</span></span>
* <span data-ttu-id="cdfc0-269">Временная таблица переименовывается в новую.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-269">The temporary table is renamed to be the new table</span></span>

<span data-ttu-id="cdfc0-270">Документация отслеживается по проблеме [#2583](https://github.com/dotnet/EntityFramework.Docs/issues/2583).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-270">Documentation is tracked by issue [#2583](https://github.com/dotnet/EntityFramework.Docs/issues/2583).</span></span>

### <a name="table-valued-functions"></a><span data-ttu-id="cdfc0-271">Функции с табличным значением</span><span class="sxs-lookup"><span data-stu-id="cdfc0-271">Table-valued functions</span></span>

<span data-ttu-id="cdfc0-272">Эту функцию предоставил участник сообщества [@pmiddleton](https://github.com/pmiddleton).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-272">This feature was contributed from the community by [@pmiddleton](https://github.com/pmiddleton).</span></span> <span data-ttu-id="cdfc0-273">Большое спасибо за публикацию!</span><span class="sxs-lookup"><span data-stu-id="cdfc0-273">Many thanks for the contribution!</span></span>

<span data-ttu-id="cdfc0-274">EF Core 5.0 превосходно поддерживает сопоставление методов .NET с функциями с табличным значением (TVF).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-274">EF Core 5.0 includes first-class support for mapping .NET methods to table-valued functions (TVFs).</span></span> <span data-ttu-id="cdfc0-275">Эти функции затем можно использовать в запросах LINQ, причем дополнительная композиция результатов функции также преобразовывается в SQL.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-275">These functions can then be used in LINQ queries where additional composition on the results of the function will also be translated to SQL.</span></span>

<span data-ttu-id="cdfc0-276">Например, рассмотрим такую функцию с табличным значением, определенную в базе данных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-276">For example, consider this TVF defined in a SQL Server database:</span></span>

```sql
CREATE FUNCTION GetReports(@employeeId int)
RETURNS @reports TABLE
(
    Name nvarchar(50) NOT NULL,
    IsDeveloper bit NOT NULL
)
AS
BEGIN
    WITH cteEmployees AS
    (
        SELECT Id, Name, ManagerId, IsDeveloper
        FROM Employees
        WHERE Id = @employeeId
        UNION ALL
        SELECT e.Id, e.Name, e.ManagerId, e.IsDeveloper
        FROM Employees e
        INNER JOIN cteEmployees cteEmp ON cteEmp.Id = e.ManagerId
    )
    INSERT INTO @reports
    SELECT Name, IsDeveloper
    FROM cteEmployees
    WHERE Id != @employeeId

    RETURN
END
```

<span data-ttu-id="cdfc0-277">Для использования этой функции в модели EF Core требуются два типа сущностей:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-277">The EF Core model requires two entity types to use this TVF:</span></span>
* <span data-ttu-id="cdfc0-278">тип `Employee`, который сопоставляется с таблицей Employees обычным образом;</span><span class="sxs-lookup"><span data-stu-id="cdfc0-278">An `Employee` type that maps to the Employees table in the normal way</span></span>
* <span data-ttu-id="cdfc0-279">тип `Report`, который соответствует форме, возвращаемой функцией с табличным значением.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-279">A `Report` type that matches the shape returned by the TVF</span></span>

```csharp
public class Employee
{
    public int Id { get; set; }
    public string Name { get; set; }
    public bool IsDeveloper { get; set; }

    public int? ManagerId { get; set; }
    public virtual Employee Manager { get; set; }
}
```

```csharp
public class Report
{
    public string Name { get; set; }
    public bool IsDeveloper { get; set; }
}
```

<span data-ttu-id="cdfc0-280">Эти типы должны быть включены в модель EF Core.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-280">These types must be included in the EF Core model:</span></span>

```csharp
modelBuilder.Entity<Employee>();
modelBuilder.Entity(typeof(Report)).HasNoKey();
```

<span data-ttu-id="cdfc0-281">Обратите внимание, что класс `Report` не имеет первичного ключа и поэтому должен быть настроен в его качестве.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-281">Notice that `Report` has no primary key and so must be configured as such.</span></span>

<span data-ttu-id="cdfc0-282">Наконец, метод .NET должен быть сопоставлен с функцией с табличным значением в базе данных.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-282">Finally, a .NET method must be mapped to the TVF in the database.</span></span> <span data-ttu-id="cdfc0-283">Этот метод можно определить в DbContext с помощью нового метода `FromExpression`.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-283">This method can be defined on the DbContext using the new `FromExpression` method:</span></span>

```csharp
public IQueryable<Report> GetReports(int managerId)
    => FromExpression(() => GetReports(managerId));
```

<span data-ttu-id="cdfc0-284">Этот метод использует параметр и возвращаемый тип, которые соответствуют приведенной выше функции с табличным значением.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-284">This method uses a parameter and return type that match the TVF defined above.</span></span> <span data-ttu-id="cdfc0-285">Затем метод добавляется в модель EF Core в OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-285">The method is then added to the EF Core model in OnModelCreating:</span></span>

```csharp
modelBuilder.HasDbFunction(() => GetReports(default));
```

<span data-ttu-id="cdfc0-286">(Лямбда-выражение используется здесь, чтобы упростить передачу `MethodInfo` в EF Core.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-286">(Using a lambda here is an easy way to pass the `MethodInfo` to EF Core.</span></span> <span data-ttu-id="cdfc0-287">Аргументы, передаваемые в метод, игнорируются.)</span><span class="sxs-lookup"><span data-stu-id="cdfc0-287">The arguments passed to the method are ignored.)</span></span>

<span data-ttu-id="cdfc0-288">Теперь можно создавать запросы, которые вызывают `GetReports` и составляются по результатам.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-288">We can now write queries that call `GetReports` and compose over the results.</span></span> <span data-ttu-id="cdfc0-289">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-289">For example:</span></span>

```csharp
from e in context.Employees
from rc in context.GetReports(e.Id)
where rc.IsDeveloper == true
select new
{
  ManagerName = e.Name,
  EmployeeName = rc.Name,
})
```

<span data-ttu-id="cdfc0-290">В SQL Server этот код преобразуется в следующий.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-290">On SQL Server, this translates to:</span></span>

```sql
SELECT [e].[Name] AS [ManagerName], [g].[Name] AS [EmployeeName]
FROM [Employees] AS [e]
CROSS APPLY [dbo].[GetReports]([e].[Id]) AS [g]
WHERE [g].[IsDeveloper] = CAST(1 AS bit)
```

<span data-ttu-id="cdfc0-291">Обратите внимание, что SQL-запрос основан на таблице `Employees`, вызывает `GetReports`, а затем добавляет дополнительное предложение WHERE на основе результатов функции.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-291">Notice that the SQL is rooted in the `Employees` table, calls `GetReports`, and then adds an additional WHERE clause on the results of the function.</span></span>

### <a name="flexible-queryupdate-mapping"></a><span data-ttu-id="cdfc0-292">Гибкое сопоставление запросов и обновлений</span><span class="sxs-lookup"><span data-stu-id="cdfc0-292">Flexible query/update mapping</span></span>

<span data-ttu-id="cdfc0-293">EF Core 5.0 позволяет сопоставлять один и тот же тип сущности с различными объектами базы данных.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-293">EF Core 5.0 allows mapping the same entity type to different database objects.</span></span> <span data-ttu-id="cdfc0-294">Этими объектами могут быть таблицы, представления или функции.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-294">These objects may be tables, views, or functions.</span></span>

<span data-ttu-id="cdfc0-295">Например, тип сущности может быть сопоставлен как с представлением базы данных, так и с таблицей базы данных.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-295">For example, an entity type can be mapped to both a database view and a database table:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder
        .Entity<Blog>()
        .ToTable("Blogs")
        .ToView("BlogsView");
}
```

<span data-ttu-id="cdfc0-296">По умолчанию в таком случае EF Core будет запрашивать данные из представления и отправлять обновления в таблицу.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-296">By default, EF Core will then query from the view and send updates to the table.</span></span> <span data-ttu-id="cdfc0-297">Например, выполнение следующего кода:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-297">For example, executing the following code:</span></span>

```csharp
var blog = context.Set<Blog>().Single(e => e.Name == "One Unicorn");

blog.Name = "1unicorn2";

context.SaveChanges();
```

<span data-ttu-id="cdfc0-298">приводит к выполнению запроса к представлению и последующему обновлению таблицы.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-298">Results in a query against the view, and then an update to the table:</span></span>

```sql
SELECT TOP(2) [b].[Id], [b].[Name], [b].[Url]
FROM [BlogsView] AS [b]
WHERE [b].[Name] = N'One Unicorn'

SET NOCOUNT ON;
UPDATE [Blogs] SET [Name] = @p0
WHERE [Id] = @p1;
SELECT @@ROWCOUNT;
```

### <a name="context-wide-split-query-configuration"></a><span data-ttu-id="cdfc0-299">Конфигурация с разделением запросов в пределах контекста</span><span class="sxs-lookup"><span data-stu-id="cdfc0-299">Context-wide split-query configuration</span></span>

<span data-ttu-id="cdfc0-300">Разделение запросов (см. ниже) теперь можно настроить по умолчанию для любого запроса, выполняемого DbContext.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-300">Split queries (see below) can now be configured as the default for any query executed by the DbContext.</span></span> <span data-ttu-id="cdfc0-301">Эта конфигурация доступна только для реляционных поставщиков и поэтому должна указываться в конфигурации `UseProvider`.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-301">This configuration is only available for relational providers, and so must be specified as part of the `UseProvider` configuration.</span></span> <span data-ttu-id="cdfc0-302">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-302">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(
            Your.SqlServerConnectionString,
            b => b.UseQuerySplittingBehavior(QuerySplittingBehavior.SplitQuery));
```

<span data-ttu-id="cdfc0-303">Документация отслеживается по проблеме [#2407](https://github.com/dotnet/EntityFramework.Docs/issues/2407).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-303">Documentation is tracked by issue [#2407](https://github.com/dotnet/EntityFramework.Docs/issues/2407).</span></span>

### <a name="physicaladdress-mapping"></a><span data-ttu-id="cdfc0-304">Сопоставление PhysicalAddress</span><span class="sxs-lookup"><span data-stu-id="cdfc0-304">PhysicalAddress mapping</span></span>

<span data-ttu-id="cdfc0-305">Эту функцию предоставил участник сообщества [@ralmsdeveloper](https://github.com/ralmsdeveloper).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-305">This feature was contributed from the community by [@ralmsdeveloper](https://github.com/ralmsdeveloper).</span></span> <span data-ttu-id="cdfc0-306">Большое спасибо за публикацию!</span><span class="sxs-lookup"><span data-stu-id="cdfc0-306">Many thanks for the contribution!</span></span>

<span data-ttu-id="cdfc0-307">Стандартный [класс PhysicalAddress](/dotnet/api/system.net.networkinformation.physicaladdress) в .NET теперь автоматически сопоставляется со строковым столбцом для баз данных, у которых пока нет собственной поддержки.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-307">The standard .NET [PhysicalAddress class](/dotnet/api/system.net.networkinformation.physicaladdress) is now automatically mapped to a string column for databases that do not already have native support.</span></span> <span data-ttu-id="cdfc0-308">Дополнительные сведения см. в примерах для `IPAddress` ниже.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-308">For more information, see the examples for `IPAddress` below.</span></span>

## <a name="preview-7"></a><span data-ttu-id="cdfc0-309">предварительная версия 7</span><span class="sxs-lookup"><span data-stu-id="cdfc0-309">Preview 7</span></span>

### <a name="dbcontextfactory"></a><span data-ttu-id="cdfc0-310">DbContextFactory</span><span class="sxs-lookup"><span data-stu-id="cdfc0-310">DbContextFactory</span></span>

<span data-ttu-id="cdfc0-311">В EF Core 5.0 введены методы `AddDbContextFactory` и `AddPooledDbContextFactory` для регистрации производства с целью создания экземпляров DbContext в контейнере внедрения зависимостей приложения.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-311">EF Core 5.0 introduces `AddDbContextFactory` and `AddPooledDbContextFactory` to register a factory for creating DbContext instances in the application's dependency injection (D.I.) container.</span></span> <span data-ttu-id="cdfc0-312">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-312">For example:</span></span>

```csharp
services.AddDbContextFactory<SomeDbContext>(b =>
    b.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Test"));
```

<span data-ttu-id="cdfc0-313">Службы приложений, такие как контроллеры ASP.NET Core, могут затем использовать `IDbContextFactory<TContext>` в конструкторе службы.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-313">Application services such as ASP.NET Core controllers can then depend on `IDbContextFactory<TContext>` in the service constructor.</span></span> <span data-ttu-id="cdfc0-314">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-314">For example:</span></span>

```csharp
public class MyController
{
    private readonly IDbContextFactory<SomeDbContext> _contextFactory;

    public MyController(IDbContextFactory<SomeDbContext> contextFactory)
    {
        _contextFactory = contextFactory;
    }
}
```

<span data-ttu-id="cdfc0-315">После этого по мере необходимости можно создавать и использовать экземпляры DbContext.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-315">DbContext instances can then be created and used as needed.</span></span> <span data-ttu-id="cdfc0-316">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-316">For example:</span></span>

```csharp
public void DoSomeThing()
{
    using (var context = _contextFactory.CreateDbContext())
    {
        // ...
    }
}
```

<span data-ttu-id="cdfc0-317">Обратите внимание, что созданные таким образом экземпляры DbContext _не_ управляются поставщиком служб приложения и поэтому должны удаляться приложением.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-317">Note that the DbContext instances created in this way are _not_ managed by the application's service provider and therefore must be disposed by the application.</span></span> <span data-ttu-id="cdfc0-318">Такое разделение очень полезно для приложений Blazor, в которых рекомендуется использовать `IDbContextFactory`, но может быть полезно и в других сценариях.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-318">This decoupling is very useful for Blazor applications, where using `IDbContextFactory` is recommended, but may also be useful in other scenarios.</span></span>

<span data-ttu-id="cdfc0-319">Экземпляры DbContext можно объединять в пул путем вызова `AddPooledDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-319">DbContext instances can be pooled by calling `AddPooledDbContextFactory`.</span></span> <span data-ttu-id="cdfc0-320">Объединение в пул выполняется так же, как для `AddDbContextPool`, и имеет те же ограничения.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-320">This pooling works the same way as for `AddDbContextPool`, and also has the same limitations.</span></span>

<span data-ttu-id="cdfc0-321">Документация отслеживается по проблеме [#2523](https://github.com/dotnet/EntityFramework.Docs/issues/2523).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-321">Documentation is tracked by issue [#2523](https://github.com/dotnet/EntityFramework.Docs/issues/2523).</span></span>

### <a name="reset-dbcontext-state"></a><span data-ttu-id="cdfc0-322">Сброс состояния DbContext</span><span class="sxs-lookup"><span data-stu-id="cdfc0-322">Reset DbContext state</span></span>

<span data-ttu-id="cdfc0-323">В EF Core 5.0 появился метод `ChangeTracker.Clear()`, который очищает DbContext всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-323">EF Core 5.0 introduces `ChangeTracker.Clear()` which clears the DbContext of all tracked entities.</span></span> <span data-ttu-id="cdfc0-324">Обычно это не требуется, если выполняется рекомендация по созданию нового экземпляра контекста с кратким сроком существования для каждой единицы работы.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-324">This should usually not be needed when using the best practice of creating a new, short-lived context instance for each unit-of-work.</span></span> <span data-ttu-id="cdfc0-325">Однако если необходимо сбросить состояние экземпляра DbContext, то использование нового метода `Clear()` является более производительным и надежным подходом, чем массовое отсоединение всех сущностей.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-325">However, if there is a need to reset the state of a DbContext instance, then using the new `Clear()` method is more performant and robust than mass-detaching all entities.</span></span>

<span data-ttu-id="cdfc0-326">Документация отслеживается по проблеме [#2524](https://github.com/dotnet/EntityFramework.Docs/issues/2524).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-326">Documentation is tracked by issue [#2524](https://github.com/dotnet/EntityFramework.Docs/issues/2524).</span></span>

### <a name="new-pattern-for-store-generated-defaults"></a><span data-ttu-id="cdfc0-327">Новый шаблон для значений по умолчанию, создаваемых хранилищем</span><span class="sxs-lookup"><span data-stu-id="cdfc0-327">New pattern for store-generated defaults</span></span>

<span data-ttu-id="cdfc0-328">EF Core позволяет явно задать значение для столбца, у которого может быть ограничение на значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-328">EF Core allows an explicit value to be set for a column that may also have default value constraint.</span></span> <span data-ttu-id="cdfc0-329">В качестве индикатора для этого в EF Core используется значение, принятое по умолчанию для типа свойства в CLR. Если значение отлично от значения CLR по умолчанию, оно будет вставлено. В противном случае используется значение по умолчанию для базы данных.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-329">EF Core uses the CLR default of type property type as a sentinel for this; if the value is not the CLR default, then it is inserted, otherwise the database default is used.</span></span>

<span data-ttu-id="cdfc0-330">Это создает проблемы для типов, для которых значение CLR по умолчанию является неподходящим индикатором, особенно для свойств `bool`.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-330">This creates problems for types where the CLR default is not a good sentinel--most notably, `bool` properties.</span></span> <span data-ttu-id="cdfc0-331">В таких случаях EF Core 5.0 теперь допускает значение NULL для резервного поля.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-331">EF Core 5.0 now allows the backing field to be nullable for cases like this.</span></span> <span data-ttu-id="cdfc0-332">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-332">For example:</span></span>

```csharp
public class Blog
{
    private bool? _isValid;

    public bool IsValid
    {
        get => _isValid ?? false;
        set => _isValid = value;
    }
}
```

<span data-ttu-id="cdfc0-333">Обратите внимание, что резервное поле допускает значение NULL, а общедоступное свойство — нет.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-333">Note that the backing field is nullable, but the publicly exposed property is not.</span></span> <span data-ttu-id="cdfc0-334">В результате значение индикатора может быть `null`, и это не влияет на общую контактную зону типа сущности.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-334">This allows the sentinel value to be `null` without impacting the public surface of the entity type.</span></span> <span data-ttu-id="cdfc0-335">Если `IsValid` не задается, используется значение по умолчанию для базы данных, так как значением резервного поля остается NULL.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-335">In this case, if the `IsValid` is never set, then the database default will be used since the backing field remains null.</span></span> <span data-ttu-id="cdfc0-336">Если задается `true` или `false`, это значение сохраняется явным образом в базе данных.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-336">If either `true` or `false` are set, then this value is saved explicitly to the database.</span></span>

<span data-ttu-id="cdfc0-337">Документация отслеживается по проблеме [#2525](https://github.com/dotnet/EntityFramework.Docs/issues/2525).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-337">Documentation is tracked by issue [#2525](https://github.com/dotnet/EntityFramework.Docs/issues/2525).</span></span>

### <a name="cosmos-partition-keys"></a><span data-ttu-id="cdfc0-338">Ключи секций Cosmos</span><span class="sxs-lookup"><span data-stu-id="cdfc0-338">Cosmos partition keys</span></span>

<span data-ttu-id="cdfc0-339">EF Core позволяет включать ключ секции Cosmos в модель EF.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-339">EF Core allows the Cosmos partition key is included in the EF model.</span></span> <span data-ttu-id="cdfc0-340">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-340">For example:</span></span>

```csharp
modelBuilder.Entity<Customer>().HasPartitionKey(b => b.AlternateKey)
```

<span data-ttu-id="cdfc0-341">Начиная с предварительной версии 7 ключ секции включается в первичный ключ типа сущности и используется для повышения производительности некоторых запросов.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-341">Starting with preview 7, the partition key is included in the entity type's PK and is used to improved performance in some queries.</span></span>

<span data-ttu-id="cdfc0-342">Документация отслеживается по проблеме [#2471](https://github.com/dotnet/EntityFramework.Docs/issues/2471).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-342">Documentation is tracked by issue [#2471](https://github.com/dotnet/EntityFramework.Docs/issues/2471).</span></span>

### <a name="cosmos-configuration"></a><span data-ttu-id="cdfc0-343">Конфигурация Cosmos</span><span class="sxs-lookup"><span data-stu-id="cdfc0-343">Cosmos configuration</span></span>

<span data-ttu-id="cdfc0-344">В EF Core 5.0 улучшена настройка Cosmos и подключений Cosmos.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-344">EF Core 5.0 improves configuration of Cosmos and Cosmos connections.</span></span>

<span data-ttu-id="cdfc0-345">Ранее в EF Core требовалось явно указывать конечную точку и ключ при подключении к базе данных Cosmos.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-345">Previously, EF Core required the end-point and key to be specified explicitly when connecting to a Cosmos database.</span></span> <span data-ttu-id="cdfc0-346">В EF Core 5.0 вместо этого можно использовать строку подключения.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-346">EF Core 5.0 allows use of a connection string instead.</span></span> <span data-ttu-id="cdfc0-347">Кроме того, в EF Core 5.0 можно явно задавать экземпляр класса WebProxy.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-347">In addition, EF Core 5.0 allows the WebProxy instance to be explicitly set.</span></span> <span data-ttu-id="cdfc0-348">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-348">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseCosmos("my-cosmos-connection-string", "MyDb",
            cosmosOptionsBuilder =>
            {
                cosmosOptionsBuilder.WebProxy(myProxyInstance);
            });
```

<span data-ttu-id="cdfc0-349">Кроме того, теперь также можно настраивать множество других значений времени ожидания, ограничений и т. д.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-349">Many other timeout values, limits, etc. can now also be configured.</span></span> <span data-ttu-id="cdfc0-350">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-350">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseCosmos("my-cosmos-connection-string", "MyDb",
            cosmosOptionsBuilder =>
            {
                cosmosOptionsBuilder.LimitToEndpoint();
                cosmosOptionsBuilder.RequestTimeout(requestTimeout);
                cosmosOptionsBuilder.OpenTcpConnectionTimeout(timeout);
                cosmosOptionsBuilder.IdleTcpConnectionTimeout(timeout);
                cosmosOptionsBuilder.GatewayModeMaxConnectionLimit(connectionLimit);
                cosmosOptionsBuilder.MaxTcpConnectionsPerEndpoint(connectionLimit);
                cosmosOptionsBuilder.MaxRequestsPerTcpConnection(requestLimit);
            });
```

<span data-ttu-id="cdfc0-351">Наконец, режимом соединения по умолчанию теперь является `ConnectionMode.Gateway`, что, как правило, обеспечивает большую совместимость.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-351">Finally, the default connection mode is now `ConnectionMode.Gateway`, which is generally more compatible.</span></span>

<span data-ttu-id="cdfc0-352">Документация отслеживается по проблеме [#2471](https://github.com/dotnet/EntityFramework.Docs/issues/2471).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-352">Documentation is tracked by issue [#2471](https://github.com/dotnet/EntityFramework.Docs/issues/2471).</span></span>

### <a name="scaffold-dbcontext-now-singularizes"></a><span data-ttu-id="cdfc0-353">Формирование шаблонов DbContext теперь производится в форме единственного числа</span><span class="sxs-lookup"><span data-stu-id="cdfc0-353">Scaffold-DbContext now singularizes</span></span>

<span data-ttu-id="cdfc0-354">Ранее при формировании шаблона DbContext на основе существующей базы данных в EF Core создавались имена типов сущностей, совпадающие с именами таблиц в базе данных.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-354">Previously when scaffolding a DbContext from an existing database, EF Core will create entity type names that match the table names in the database.</span></span> <span data-ttu-id="cdfc0-355">Например, для таблиц `People` и `Addresses` создавались типы сущностей с именами `People` и `Addresses`.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-355">For example, tables `People` and `Addresses` resulted in entity types named `People` and `Addresses`.</span></span>

<span data-ttu-id="cdfc0-356">В предыдущих выпусках это поведение можно было настраивать путем регистрации службы преобразования во множественную форму.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-356">In previous releases, this behavior was configurable through registration of a pluralization service.</span></span> <span data-ttu-id="cdfc0-357">Теперь в EF Core 5.0 в качестве службы преобразования во множественную форму по умолчанию используется пакет [Humanizer](https://www.nuget.org/packages/Humanizer.Core/).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-357">Now in EF Core 5.0, the [Humanizer](https://www.nuget.org/packages/Humanizer.Core/) package is used as a default pluralization service.</span></span> <span data-ttu-id="cdfc0-358">Это означает, что таблицы `People` и `Addresses` теперь будут реконструированы в типы сущностей с именами `Person` и `Address`.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-358">This means tables `People` and `Addresses` will now be reverse engineered to entity types named `Person` and `Address`.</span></span>

### <a name="savepoints"></a><span data-ttu-id="cdfc0-359">Точки сохранения</span><span class="sxs-lookup"><span data-stu-id="cdfc0-359">Savepoints</span></span>

<span data-ttu-id="cdfc0-360">EF Core теперь поддерживает [точки сохранения](/sql/t-sql/language-elements/save-transaction-transact-sql#remarks) для лучшего контроля над транзакциями, выполняющими несколько операций.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-360">EF Core now supports [savepoints](/sql/t-sql/language-elements/save-transaction-transact-sql#remarks) for greater control over transactions that execute multiple operations.</span></span>

<span data-ttu-id="cdfc0-361">Точки сохранения можно создавать, освобождать и откатывать вручную.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-361">Savepoints can be manually created, released, and rolled back.</span></span> <span data-ttu-id="cdfc0-362">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-362">For example:</span></span>

```csharp
context.Database.CreateSavepoint("MySavePoint");
```

<span data-ttu-id="cdfc0-363">Кроме того, при неудачном выполнении `SaveChanges` в EF Core теперь выполняется откат к последней точке сохранения.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-363">In addition, EF Core will now roll back to the last savepoint when executing `SaveChanges` fails.</span></span> <span data-ttu-id="cdfc0-364">Это позволяет повторно выполнять вызов SaveChanges без повторного выполнения всей транзакции.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-364">This allows SaveChanges to be re-tried without re-trying the entire transaction.</span></span>

<span data-ttu-id="cdfc0-365">Документация отслеживается по проблеме [#2429](https://github.com/dotnet/EntityFramework.Docs/issues/2429).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-365">Documentation is tracked by issue [#2429](https://github.com/dotnet/EntityFramework.Docs/issues/2429).</span></span>

## <a name="preview-6"></a><span data-ttu-id="cdfc0-366">предварительная версия 6</span><span class="sxs-lookup"><span data-stu-id="cdfc0-366">Preview 6</span></span>

### <a name="split-queries-for-related-collections"></a><span data-ttu-id="cdfc0-367">Разделение запросов для связанных коллекций</span><span class="sxs-lookup"><span data-stu-id="cdfc0-367">Split queries for related collections</span></span>

<span data-ttu-id="cdfc0-368">Начиная с EF Core 3.0, EF Core всегда создает один SQL-запрос для каждого LINQ-запроса.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-368">Starting with EF Core 3.0, EF Core always generates a single SQL query for each LINQ query.</span></span> <span data-ttu-id="cdfc0-369">Это обеспечивает согласованность данных, возвращаемых в рамках ограничений используемого режима транзакций.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-369">This ensures consistency of the data returned within the constraints of the transaction mode in use.</span></span> <span data-ttu-id="cdfc0-370">Однако если запрос использует `Include` или проекцию для возврата нескольких связанных коллекций, это может привести к снижению производительности.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-370">However, this can become very slow when the query uses `Include` or a projection to bring back multiple related collections.</span></span>

<span data-ttu-id="cdfc0-371">Теперь EF Core 5.0 позволяет разделять один LINQ-запрос, включая связанные коллекции, на несколько SQL-запросов.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-371">EF Core 5.0 now allows a single LINQ query including related collections to be split into multiple SQL queries.</span></span> <span data-ttu-id="cdfc0-372">Это может значительно повысить производительность, но может также привести к несогласованности возвращаемых результатов в случае изменения данных между двумя запросами.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-372">This can significantly improve performance, but can result in inconsistency in the results returned if the data changes between the two queries.</span></span> <span data-ttu-id="cdfc0-373">Для устранения этой проблемы и обеспечения согласованности с разделенными запросами можно использовать сериализуемые транзакции или транзакции с моментальными снимками, но это может привести к дополнительным затратам на производительность и вызвать различия в поведении.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-373">Serializable or snapshot transactions can be used to mitigate this and achieve consistency with split queries, but that may bring other performance costs and behavioral difference.</span></span>

#### <a name="split-queries-with-include"></a><span data-ttu-id="cdfc0-374">Разделение запросов с помощью Include</span><span class="sxs-lookup"><span data-stu-id="cdfc0-374">Split queries with Include</span></span>

<span data-ttu-id="cdfc0-375">Рассмотрим запрос, который извлекает два уровня связанных коллекций с помощью `Include`:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-375">For example, consider a query that pulls in two levels of related collections using `Include`:</span></span>

```csharp
var artists = context.Artists
    .Include(e => e.Albums).ThenInclude(e => e.Tags)
    .ToList();
```

<span data-ttu-id="cdfc0-376">Если используется поставщик SQLite, EF Core по умолчанию создаст следующий SQL:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-376">By default, EF Core will generate the following SQL when using the SQLite provider:</span></span>

```sql
SELECT "a"."Id", "a"."Name", "t0"."Id", "t0"."ArtistId", "t0"."Title", "t0"."Id0", "t0"."AlbumId", "t0"."Name"
FROM "Artists" AS "a"
LEFT JOIN (
    SELECT "a0"."Id", "a0"."ArtistId", "a0"."Title", "t"."Id" AS "Id0", "t"."AlbumId", "t"."Name"
    FROM "Album" AS "a0"
    LEFT JOIN "Tag" AS "t" ON "a0"."Id" = "t"."AlbumId"
) AS "t0" ON "a"."Id" = "t0"."ArtistId"
ORDER BY "a"."Id", "t0"."Id", "t0"."Id0"
```

<span data-ttu-id="cdfc0-377">Для изменения этого поведения можно использовать новый API `AsSplitQuery`.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-377">The new `AsSplitQuery` API can be used to change this behavior.</span></span> <span data-ttu-id="cdfc0-378">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-378">For example:</span></span>

```csharp
var artists = context.Artists
    .AsSplitQuery()
    .Include(e => e.Albums).ThenInclude(e => e.Tags)
    .ToList();
```

<span data-ttu-id="cdfc0-379">AsSplitQuery доступен для всех поставщиков реляционных баз данных и может использоваться в любом месте запроса так же, как AsNoTracking.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-379">AsSplitQuery is available for all relational database providers and can be used anywhere in the query, just like AsNoTracking.</span></span> <span data-ttu-id="cdfc0-380">Теперь EF Core будет создавать следующие три SQL-запроса:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-380">EF Core will now generate the following three SQL queries:</span></span>

```sql
SELECT "a"."Id", "a"."Name"
FROM "Artists" AS "a"
ORDER BY "a"."Id"

SELECT "a0"."Id", "a0"."ArtistId", "a0"."Title", "a"."Id"
FROM "Artists" AS "a"
INNER JOIN "Album" AS "a0" ON "a"."Id" = "a0"."ArtistId"
ORDER BY "a"."Id", "a0"."Id"

SELECT "t"."Id", "t"."AlbumId", "t"."Name", "a"."Id", "a0"."Id"
FROM "Artists" AS "a"
INNER JOIN "Album" AS "a0" ON "a"."Id" = "a0"."ArtistId"
INNER JOIN "Tag" AS "t" ON "a0"."Id" = "t"."AlbumId"
ORDER BY "a"."Id", "a0"."Id"
```

<span data-ttu-id="cdfc0-381">Поддерживаются все операции в корне запроса.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-381">All operations on the query root are supported.</span></span> <span data-ttu-id="cdfc0-382">В их число входят операции OrderBy/Skip/Take, Join, FirstOrDefault и аналогичные операции выбора одного результата.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-382">This includes OrderBy/Skip/Take, Join operations, FirstOrDefault and similar single result selecting operations.</span></span>

<span data-ttu-id="cdfc0-383">Обратите внимание, что включения с фильтрацией с операциями OrderBy/Skip/Take не поддерживаются в предварительной версии 6, но они доступны в ежедневных сборках и будут включены в предварительную версию 7.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-383">Note that filtered Includes with OrderBy/Skip/Take are not supported in preview 6, but are available in the daily builds and will be included in preview 7.</span></span>

#### <a name="split-queries-with-collection-projections"></a><span data-ttu-id="cdfc0-384">Разделение запросов с помощью проекций коллекции</span><span class="sxs-lookup"><span data-stu-id="cdfc0-384">Split queries with collection projections</span></span>

<span data-ttu-id="cdfc0-385">`AsSplitQuery` можно также использовать при загрузке коллекций в проекции.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-385">`AsSplitQuery` can also be used when collections are loaded in projections.</span></span> <span data-ttu-id="cdfc0-386">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-386">For example:</span></span>

```csharp
context.Artists
    .AsSplitQuery()
    .Select(e => new
    {
        Artist = e,
        Albums = e.Albums,
    }).ToList();
```

<span data-ttu-id="cdfc0-387">При использовании поставщика SQLite этот LINQ-запрос создает следующие два SQL-запроса:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-387">This LINQ query generates the following two SQL queries when using the SQLite provider:</span></span>

```sql
SELECT "a"."Id", "a"."Name"
FROM "Artists" AS "a"
ORDER BY "a"."Id"

SELECT "a0"."Id", "a0"."ArtistId", "a0"."Title", "a"."Id"
FROM "Artists" AS "a"
INNER JOIN "Album" AS "a0" ON "a"."Id" = "a0"."ArtistId"
ORDER BY "a"."Id"
```

<span data-ttu-id="cdfc0-388">Обратите внимание, что поддерживается только материализация коллекции.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-388">Note that only materialization of the collection is supported.</span></span> <span data-ttu-id="cdfc0-389">Любая композиция после `e.Albums` в приведенном выше случае не приведет к разделению запроса.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-389">Any composition after `e.Albums` in above case won't result in a split query.</span></span> <span data-ttu-id="cdfc0-390">Улучшения в этой области отслеживаются по проблеме [#21234](https://github.com/dotnet/efcore/issues/21234).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-390">Improvements in this area are tracked by [#21234](https://github.com/dotnet/efcore/issues/21234).</span></span>

### <a name="indexattribute"></a><span data-ttu-id="cdfc0-391">IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="cdfc0-391">IndexAttribute</span></span>

<span data-ttu-id="cdfc0-392">Новый IndexAttribute можно поместить в тип сущности, чтобы указать индекс для одного столбца.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-392">The new IndexAttribute can be placed on an entity type to specify an index for a single column.</span></span> <span data-ttu-id="cdfc0-393">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-393">For example:</span></span>

```csharp
[Index(nameof(FullName), IsUnique = true)]
public class User
{
    public int Id { get; set; }

    [MaxLength(128)]
    public string FullName { get; set; }
}
```

<span data-ttu-id="cdfc0-394">Для SQL Server функция миграции создаст следующий SQL:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-394">For SQL Server, Migrations will then generate the following SQL:</span></span>

```sql
CREATE UNIQUE INDEX [IX_Users_FullName]
    ON [Users] ([FullName])
    WHERE [FullName] IS NOT NULL;
```

<span data-ttu-id="cdfc0-395">IndexAttribute можно также использовать для указания индекса, охватывающего несколько столбцов.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-395">IndexAttribute can also be used to specify an index spanning multiple columns.</span></span> <span data-ttu-id="cdfc0-396">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-396">For example:</span></span>

```csharp
[Index(nameof(FirstName), nameof(LastName), IsUnique = true)]
public class User
{
    public int Id { get; set; }

    [MaxLength(64)]
    public string FirstName { get; set; }

    [MaxLength(64)]
    public string LastName { get; set; }
}
```

<span data-ttu-id="cdfc0-397">В SQL Server будет выведен следующий результат:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-397">For SQL Server, this results in:</span></span>

```sql
CREATE UNIQUE INDEX [IX_Users_FirstName_LastName]
    ON [Users] ([FirstName], [LastName])
    WHERE [FirstName] IS NOT NULL AND [LastName] IS NOT NULL;
```

<span data-ttu-id="cdfc0-398">Документация отслеживается по проблеме [#2407](https://github.com/dotnet/EntityFramework.Docs/issues/2407).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-398">Documentation is tracked by issue [#2407](https://github.com/dotnet/EntityFramework.Docs/issues/2407).</span></span>

### <a name="improved-query-translation-exceptions"></a><span data-ttu-id="cdfc0-399">Улучшенные исключения преобразования запросов</span><span class="sxs-lookup"><span data-stu-id="cdfc0-399">Improved query translation exceptions</span></span>

<span data-ttu-id="cdfc0-400">Мы продолжаем улучшать сообщения об исключениях, создаваемые при сбое преобразования запроса.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-400">We are continuing to improve the exception messages generated when query translation fails.</span></span> <span data-ttu-id="cdfc0-401">Например, в этом запросе используется несопоставленное свойство `IsSigned`:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-401">For example, this query uses the unmapped property `IsSigned`:</span></span>

```csharp
var artists = context.Artists.Where(e => e.IsSigned).ToList();
```

<span data-ttu-id="cdfc0-402">EF Core выдаст следующее исключение, указывающее на сбой преобразования из-за несопоставленного `IsSigned`:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-402">EF Core will throw the following exception indicating that translation failed because `IsSigned` is not mapped:</span></span>

> <span data-ttu-id="cdfc0-403">Необработанное исключение.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-403">Unhandled exception.</span></span> <span data-ttu-id="cdfc0-404">"System.InvalidOperationException: не удалось преобразовать выражение LINQ "DbSet<Artist>() .Where(a => a.IsSigned)".</span><span class="sxs-lookup"><span data-stu-id="cdfc0-404">System.InvalidOperationException: The LINQ expression 'DbSet<Artist>() .Where(a => a.IsSigned)' could not be translated.</span></span> <span data-ttu-id="cdfc0-405">Дополнительные сведения: не удалось преобразовать член "IsSigned" для типа сущности "Artist".</span><span class="sxs-lookup"><span data-stu-id="cdfc0-405">Additional information: Translation of member 'IsSigned' on entity type 'Artist' failed.</span></span> <span data-ttu-id="cdfc0-406">Возможно, указанный элемент не сопоставлен.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-406">Possibly the specified member is not mapped.</span></span> <span data-ttu-id="cdfc0-407">Перепишите запрос в поддерживающей преобразование форме или явно переключитесь на оценку клиента, вставив вызов либо AsEnumerable(), AsAsyncEnumerable(), ToList(), либо ToListAsync().</span><span class="sxs-lookup"><span data-stu-id="cdfc0-407">Either rewrite the query in a form that can be translated, or switch to client evaluation explicitly by inserting a call to either AsEnumerable(), AsAsyncEnumerable(), ToList(), or ToListAsync().</span></span> <span data-ttu-id="cdfc0-408">Дополнительные сведения см. в разделе https://go.microsoft.com/fwlink/?linkid=2101038.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-408">See https://go.microsoft.com/fwlink/?linkid=2101038 for more information.</span></span>

<span data-ttu-id="cdfc0-409">Аналогично, при попытке преобразовать сравнения строк с семантикой, зависящей от языка и региональных параметров, теперь создаются более понятные сообщения об исключениях.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-409">Similarly, better exception messages are now generated when attempting to translate string comparisons with culture-dependent semantics.</span></span> <span data-ttu-id="cdfc0-410">Например, этот запрос пытается использовать `StringComparison.CurrentCulture`:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-410">For example, this query attempts to use `StringComparison.CurrentCulture`:</span></span>

```csharp
var artists = context.Artists
    .Where(e => e.Name.Equals("The Unicorns", StringComparison.CurrentCulture))
    .ToList();
```

<span data-ttu-id="cdfc0-411">Теперь EF Core выдаст следующее исключение:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-411">EF Core will now throw the following exception:</span></span>

> <span data-ttu-id="cdfc0-412">Необработанное исключение.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-412">Unhandled exception.</span></span> <span data-ttu-id="cdfc0-413">"System.InvalidOperationException: не удается преобразовать выражение LINQ "DbSet<Artist>() .Where(a => a.Name.Equals( value: "The Unicorns", comparisonType: CurrentCulture))".</span><span class="sxs-lookup"><span data-stu-id="cdfc0-413">System.InvalidOperationException: The LINQ expression 'DbSet<Artist>() .Where(a => a.Name.Equals( value: "The Unicorns", comparisonType: CurrentCulture))' could not be translated.</span></span> <span data-ttu-id="cdfc0-414">Дополнительные сведения: преобразование метода "string.Equals", принимающего аргумент "StringComparison", не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-414">Additional information: Translation of 'string.Equals' method which takes 'StringComparison' argument is not supported.</span></span> <span data-ttu-id="cdfc0-415">Дополнительные сведения см. в разделе https://go.microsoft.com/fwlink/?linkid=2129535.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-415">See https://go.microsoft.com/fwlink/?linkid=2129535 for more information.</span></span> <span data-ttu-id="cdfc0-416">Перепишите запрос в поддерживающей преобразование форме или явно переключитесь на оценку клиента, вставив вызов либо AsEnumerable(), AsAsyncEnumerable(), ToList(), либо ToListAsync().</span><span class="sxs-lookup"><span data-stu-id="cdfc0-416">Either rewrite the query in a form that can be translated, or switch to client evaluation explicitly by inserting a call to either AsEnumerable(), AsAsyncEnumerable(), ToList(), or ToListAsync().</span></span> <span data-ttu-id="cdfc0-417">Дополнительные сведения см. в разделе https://go.microsoft.com/fwlink/?linkid=2101038.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-417">See https://go.microsoft.com/fwlink/?linkid=2101038 for more information.</span></span>

### <a name="specify-transaction-id"></a><span data-ttu-id="cdfc0-418">Указание идентификатора транзакции</span><span class="sxs-lookup"><span data-stu-id="cdfc0-418">Specify transaction ID</span></span>

<span data-ttu-id="cdfc0-419">Эту функцию предоставил участник сообщества [@Marusyk](https://github.com/Marusyk).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-419">This feature was contributed from the community by [@Marusyk](https://github.com/Marusyk).</span></span> <span data-ttu-id="cdfc0-420">Большое спасибо за публикацию!</span><span class="sxs-lookup"><span data-stu-id="cdfc0-420">Many thanks for the contribution!</span></span>

<span data-ttu-id="cdfc0-421">EF Core предоставляет идентификатор транзакции для корреляции транзакций между вызовами.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-421">EF Core exposes a transaction ID for correlation of transactions across calls.</span></span> <span data-ttu-id="cdfc0-422">Этот идентификатор обычно задается в EF Core при запуске транзакции.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-422">This ID typically set by EF Core when a transaction is started.</span></span> <span data-ttu-id="cdfc0-423">Если транзакцию запускает приложение, эта функция позволяет приложению явно задать идентификатор транзакции для его правильной корреляции везде, где она используется.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-423">If the application starts the transaction instead, then this feature allows the application to explicitly set the transaction ID so it is correlated correctly everywhere it is used.</span></span> <span data-ttu-id="cdfc0-424">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-424">For example:</span></span>

```csharp
using (context.Database.UseTransaction(myTransaction, myId))
{
   ...
}
```

### <a name="ipaddress-mapping"></a><span data-ttu-id="cdfc0-425">Сопоставление IPAddress</span><span class="sxs-lookup"><span data-stu-id="cdfc0-425">IPAddress mapping</span></span>

<span data-ttu-id="cdfc0-426">Эту функцию предоставил участник сообщества [@ralmsdeveloper](https://github.com/ralmsdeveloper).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-426">This feature was contributed from the community by [@ralmsdeveloper](https://github.com/ralmsdeveloper).</span></span> <span data-ttu-id="cdfc0-427">Большое спасибо за публикацию!</span><span class="sxs-lookup"><span data-stu-id="cdfc0-427">Many thanks for the contribution!</span></span>

<span data-ttu-id="cdfc0-428">Стандартный класс [IPAddress](/dotnet/api/system.net.ipaddress) .NET теперь автоматически сопоставляется со строковым столбцом для баз данных, у которых пока нет собственной поддержки.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-428">The standard .NET [IPAddress class](/dotnet/api/system.net.ipaddress) is now automatically mapped to a string column for databases that do not already have native support.</span></span> <span data-ttu-id="cdfc0-429">Например, рассмотрим сопоставление этого типа сущности:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-429">For example, consider mapping this entity type:</span></span>

```csharp
public class Host
{
    public int Id { get; set; }
    public IPAddress Address { get; set; }
}
```

<span data-ttu-id="cdfc0-430">В SQL Server функция миграции создаст следующую таблицу:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-430">On SQL Server, this will result in Migrations creating the following table:</span></span>

```sql
CREATE TABLE [Host] (
    [Id] int NOT NULL,
    [Address] nvarchar(45) NULL,
    CONSTRAINT [PK_Host] PRIMARY KEY ([Id]));
```

<span data-ttu-id="cdfc0-431">Затем можно добавить сущности обычным образом:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-431">Entities can then be added in the normal way:</span></span>

```csharp
context.AddRange(
    new Host { Address = IPAddress.Parse("127.0.0.1")},
    new Host { Address = IPAddress.Parse("0000:0000:0000:0000:0000:0000:0000:0001")});
```

<span data-ttu-id="cdfc0-432">Результирующий SQL вставит нормализованный IPv4- или IPv6-адрес:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-432">And the resulting SQL will insert the normalized IPv4 or IPv6 address:</span></span>

```sql
Executed DbCommand (14ms) [Parameters=[@p0='1', @p1='127.0.0.1' (Size = 45), @p2='2', @p3='::1' (Size = 45)], CommandType='Text', CommandTimeout='30']
      SET NOCOUNT ON;
      INSERT INTO [Host] ([Id], [Address])
      VALUES (@p0, @p1), (@p2, @p3);
```

### <a name="exclude-onconfiguring-when-scaffolding"></a><span data-ttu-id="cdfc0-433">Исключение OnConfiguring при формировании шаблонов</span><span class="sxs-lookup"><span data-stu-id="cdfc0-433">Exclude OnConfiguring when scaffolding</span></span>

<span data-ttu-id="cdfc0-434">При формировании DbContext из существующей базы данных EF Core по умолчанию создает перегрузку OnConfiguring со строкой подключения для немедленного использования контекста.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-434">When a DbContext is scaffolded from an existing database, EF Core by default creates an OnConfiguring overload with a connection string so that the context is immediately usable.</span></span> <span data-ttu-id="cdfc0-435">Однако это бесполезно, если у вас уже есть разделяемый класс с OnConfiguring, или если вы настраиваете контекст другим способом.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-435">However, this is not useful if you already have a partial class with OnConfiguring, or if you are configuring the context some other way.</span></span>

<span data-ttu-id="cdfc0-436">Теперь командам формирования шаблонов можно указать пропустить создание OnConfiguring.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-436">To address this, the scaffolding commands can now be instructed to omit generation of OnConfiguring.</span></span> <span data-ttu-id="cdfc0-437">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-437">For example:</span></span>

```
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer --no-onconfiguring
```

<span data-ttu-id="cdfc0-438">Или в консоли диспетчера пакетов:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-438">Or in the Package Manager Console:</span></span>

```
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer -NoOnConfiguring
```

<span data-ttu-id="cdfc0-439">Обратите внимание, что рекомендуется использовать [именованную строку подключения и защищенное хранилище, такое как секреты пользователей](xref:core/managing-schemas/scaffolding#configuration-and-user-secrets).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-439">Note that we recommend using [a named connection string and secure storage like User Secrets](xref:core/managing-schemas/scaffolding#configuration-and-user-secrets).</span></span>

### <a name="translations-for-firstordefault-on-strings"></a><span data-ttu-id="cdfc0-440">Преобразования для FirstOrDefault в строках</span><span class="sxs-lookup"><span data-stu-id="cdfc0-440">Translations for FirstOrDefault on strings</span></span>

<span data-ttu-id="cdfc0-441">Эту функцию предоставил участник сообщества [@dvoreckyaa](https://github.com/dvoreckyaa).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-441">This feature was contributed from the community by [@dvoreckyaa](https://github.com/dvoreckyaa).</span></span> <span data-ttu-id="cdfc0-442">Большое спасибо за публикацию!</span><span class="sxs-lookup"><span data-stu-id="cdfc0-442">Many thanks for the contribution!</span></span>

<span data-ttu-id="cdfc0-443">FirstOrDefault и аналогичные операторы для символов в строках теперь можно преобразовывать.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-443">FirstOrDefault and similar operators for characters in strings are now translated.</span></span> <span data-ttu-id="cdfc0-444">Например, следующий LINQ-запрос:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-444">For example, this LINQ query:</span></span>

```csharp
context.Customers.Where(c => c.ContactName.FirstOrDefault() == 'A').ToList();
```

<span data-ttu-id="cdfc0-445">будет преобразован в следующий SQL при использовании SQL Server:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-445">Will be translated to the following SQL when using SQL Server:</span></span>

```sql
SELECT [c].[Id], [c].[ContactName]
FROM [Customer] AS [c]
WHERE SUBSTRING([c].[ContactName], 1, 1) = N'A'
```

### <a name="simplify-case-blocks"></a><span data-ttu-id="cdfc0-446">Упрощение блоков case</span><span class="sxs-lookup"><span data-stu-id="cdfc0-446">Simplify case blocks</span></span>

<span data-ttu-id="cdfc0-447">Теперь EF Core создает улучшенные запросы с блоками case.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-447">EF Core now generates better queries with CASE blocks.</span></span> <span data-ttu-id="cdfc0-448">Например, следующий LINQ-запрос:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-448">For example, this LINQ query:</span></span>

```csharp
context.Weapons
    .OrderBy(w => w.Name.CompareTo("Marcus' Lancer") == 0)
    .ThenBy(w => w.Id)
```

<span data-ttu-id="cdfc0-449">раньше преобразовывался в SQL Server в:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-449">Was on SQL Server formally translated to:</span></span>

```sql
SELECT [w].[Id], [w].[AmmunitionType], [w].[IsAutomatic], [w].[Name], [w].[OwnerFullName], [w].[SynergyWithId]
FROM [Weapons] AS [w]
ORDER BY CASE
    WHEN (CASE
        WHEN [w].[Name] = N'Marcus'' Lancer' THEN 0
        WHEN [w].[Name] > N'Marcus'' Lancer' THEN 1
        WHEN [w].[Name] < N'Marcus'' Lancer' THEN -1
    END = 0) AND CASE
        WHEN [w].[Name] = N'Marcus'' Lancer' THEN 0
        WHEN [w].[Name] > N'Marcus'' Lancer' THEN 1
        WHEN [w].[Name] < N'Marcus'' Lancer' THEN -1
    END IS NOT NULL THEN CAST(1 AS bit)
    ELSE CAST(0 AS bit)
END, [w].[Id]");
```

<span data-ttu-id="cdfc0-450">Но теперь преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-450">But is now translated to:</span></span>

```sql
SELECT [w].[Id], [w].[AmmunitionType], [w].[IsAutomatic], [w].[Name], [w].[OwnerFullName], [w].[SynergyWithId]
FROM [Weapons] AS [w]
ORDER BY CASE
    WHEN ([w].[Name] = N'Marcus'' Lancer') AND [w].[Name] IS NOT NULL THEN CAST(1 AS bit)
    ELSE CAST(0 AS bit)
END, [w].[Id]");
```

## <a name="preview-5"></a><span data-ttu-id="cdfc0-451">Предварительная версия 5</span><span class="sxs-lookup"><span data-stu-id="cdfc0-451">Preview 5</span></span>

### <a name="database-collations"></a><span data-ttu-id="cdfc0-452">Параметры сортировки баз данных</span><span class="sxs-lookup"><span data-stu-id="cdfc0-452">Database collations</span></span>

<span data-ttu-id="cdfc0-453">Модель EF теперь позволяет указывать параметры сортировки по умолчанию для базы данных.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-453">The default collation for a database can now be specified in the EF model.</span></span> <span data-ttu-id="cdfc0-454">Это будет передаваться в создаваемые миграции для задания параметров сортировки при создании базы данных.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-454">This will flow through to generated migrations to set the collation when the database is created.</span></span> <span data-ttu-id="cdfc0-455">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-455">For example:</span></span>

```csharp
modelBuilder.UseCollation("German_PhoneBook_CI_AS");
```

<span data-ttu-id="cdfc0-456">Затем функция миграции формирует следующий код для создания базы данных в SQL Server:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-456">Migrations then generates the following to create the database on SQL Server:</span></span>

```sql
CREATE DATABASE [Test]
COLLATE German_PhoneBook_CI_AS;
```

<span data-ttu-id="cdfc0-457">Можно также указать параметры сортировки, которые будут использоваться для конкретных столбцов.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-457">The collation to use for specific database columns can also be specified.</span></span> <span data-ttu-id="cdfc0-458">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-458">For example:</span></span>

```csharp
modelBuilder
    .Entity<User>()
    .Property(e => e.Name)
    .UseCollation("German_PhoneBook_CI_AS");
```

<span data-ttu-id="cdfc0-459">Если функция миграции не используется, параметры сортировки реконструируются из базы данных при формировании шаблонов DbContext.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-459">For those not using migrations, collations are now reverse-engineered from the database when scaffolding a DbContext.</span></span>

<span data-ttu-id="cdfc0-460">Наконец, `EF.Functions.Collate()` позволяет использовать различные параметры сортировки для нерегламентированных запросов.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-460">Finally, the `EF.Functions.Collate()` allows for ad-hoc queries using different collations.</span></span> <span data-ttu-id="cdfc0-461">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-461">For example:</span></span>

```csharp
context.Users.Single(e => EF.Functions.Collate(e.Name, "French_CI_AS") == "Jean-Michel Jarre");
```

<span data-ttu-id="cdfc0-462">Будет создан следующий запрос для SQL Server:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-462">This will generate the following query for SQL Server:</span></span>

```sql
SELECT TOP(2) [u].[Id], [u].[Name]
FROM [Users] AS [u]
WHERE [u].[Name] COLLATE French_CI_AS = N'Jean-Michel Jarre'
```

<span data-ttu-id="cdfc0-463">Обратите внимание, что нерегламентированные параметры сортировки следует использовать с осторожностью, так как они могут негативно повлиять на производительность базы данных.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-463">Note that ad-hoc collations should be used with care as they can negatively impact database performance.</span></span>

<span data-ttu-id="cdfc0-464">Это документируется в проблеме [#2273](https://github.com/dotnet/EntityFramework.Docs/issues/2273).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-464">Documentation is tracked by issue [#2273](https://github.com/dotnet/EntityFramework.Docs/issues/2273).</span></span>

### <a name="flow-arguments-into-idesigntimedbcontextfactory"></a><span data-ttu-id="cdfc0-465">Передача аргументов в интерфейс IDesignTimeDbContextFactory</span><span class="sxs-lookup"><span data-stu-id="cdfc0-465">Flow arguments into IDesignTimeDbContextFactory</span></span>

<span data-ttu-id="cdfc0-466">Теперь можно передавать аргументы из командной строки в метод `CreateDbContext` интерфейса [IDesignTimeDbContextFactory](/dotnet/api/microsoft.entityframeworkcore.design.idesigntimedbcontextfactory-1).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-466">Arguments are now flowed from the command line into the `CreateDbContext` method of [IDesignTimeDbContextFactory](/dotnet/api/microsoft.entityframeworkcore.design.idesigntimedbcontextfactory-1).</span></span> <span data-ttu-id="cdfc0-467">Например, чтобы указать, что используется сборка для разработки, можно передать в командной строке настраиваемый аргумент, например `dev`:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-467">For example, to indicate this is a dev build, a custom argument (e.g. `dev`) can be passed on the command line:</span></span>

```
dotnet ef migrations add two --verbose --dev
```

<span data-ttu-id="cdfc0-468">Затем этот аргумент будет передан в фабрику, где его можно использовать для управления созданием и инициализацией контекста.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-468">This argument will then flow into the factory, where it can be used to control how the context is created and initialized.</span></span> <span data-ttu-id="cdfc0-469">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-469">For example:</span></span>

```csharp
public class MyDbContextFactory : IDesignTimeDbContextFactory<SomeDbContext>
{
    public SomeDbContext CreateDbContext(string[] args)
        => new SomeDbContext(args.Contains("--dev"));
}
```

<span data-ttu-id="cdfc0-470">Это документируется в проблеме [#2419](https://github.com/dotnet/EntityFramework.Docs/issues/2419).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-470">Documentation is tracked by issue [#2419](https://github.com/dotnet/EntityFramework.Docs/issues/2419).</span></span>

### <a name="no-tracking-queries-with-identity-resolution"></a><span data-ttu-id="cdfc0-471">Запросы без отслеживания с разрешением идентификаторов</span><span class="sxs-lookup"><span data-stu-id="cdfc0-471">No-tracking queries with identity resolution</span></span>

<span data-ttu-id="cdfc0-472">Теперь можно настроить запросы без отслеживания на выполнение разрешения идентификаторов.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-472">No-tracking queries can now be configured to perform identity resolution.</span></span> <span data-ttu-id="cdfc0-473">Например, следующий запрос будет создавать новый экземпляр Blog для каждого объекта Post, даже если у каждого Blog один и тот же первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-473">For example, the following query will create a new Blog instance for each Post, even if each Blog has the same primary key.</span></span>

```csharp
context.Posts.AsNoTracking().Include(e => e.Blog).ToList();
```

<span data-ttu-id="cdfc0-474">Однако за счет того, что этот запрос обычно выполняется немного медленнее и всегда использует больше памяти, его можно изменить так, чтобы создавался только один экземпляр Blog:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-474">However, at the expense of usually being slightly slower and always using more memory, this query can be changed to ensure only a single Blog instance is created:</span></span>

```csharp
context.Posts.AsNoTracking().PerformIdentityResolution().Include(e => e.Blog).ToList();
```

<span data-ttu-id="cdfc0-475">Обратите внимание, что этот вариант подходит только для запросов без отслеживания, так как все запросы с отслеживанием уже действуют таким образом.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-475">Note that this is only useful for no-tracking queries since all tracking queries already exhibit this behavior.</span></span> <span data-ttu-id="cdfc0-476">Кроме того, после проверки API будет изменен синтаксис `PerformIdentityResolution`.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-476">Also, following API review, the `PerformIdentityResolution` syntax will be changed.</span></span> <span data-ttu-id="cdfc0-477">См. [#19877](https://github.com/dotnet/efcore/issues/19877#issuecomment-637371073).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-477">See [#19877](https://github.com/dotnet/efcore/issues/19877#issuecomment-637371073).</span></span>

<span data-ttu-id="cdfc0-478">Это документируется в проблеме [#1895](https://github.com/dotnet/EntityFramework.Docs/issues/1895).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-478">Documentation is tracked by issue [#1895](https://github.com/dotnet/EntityFramework.Docs/issues/1895).</span></span>

### <a name="stored-persisted-computed-columns"></a><span data-ttu-id="cdfc0-479">Сохраняемые вычисляемые столбцы</span><span class="sxs-lookup"><span data-stu-id="cdfc0-479">Stored (persisted) computed columns</span></span>

<span data-ttu-id="cdfc0-480">Большинство баз данных позволяют сохранять значения вычисляемых столбцов после вычислений.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-480">Most databases allow computed column values to be stored after computation.</span></span> <span data-ttu-id="cdfc0-481">Хотя для этого требуется место на диске, вычисляемый столбец вычисляется только один раз при обновлении, а не при каждом получении его значения.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-481">While this takes up disk space, the computed column is calculated only once on update, instead of each time its value is retrieved.</span></span> <span data-ttu-id="cdfc0-482">Это также позволяет индексировать столбец для некоторых баз данных.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-482">This also allows the column to be indexed for some databases.</span></span>

<span data-ttu-id="cdfc0-483">EF Core 5.0 поддерживает настройку вычисляемых столбцов как сохраняемых.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-483">EF Core 5.0 allows computed columns to be configured as stored.</span></span> <span data-ttu-id="cdfc0-484">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-484">For example:</span></span>

```csharp
modelBuilder
    .Entity<User>()
    .Property(e => e.SomethingComputed)
    .HasComputedColumnSql("my sql", stored: true);
```

### <a name="sqlite-computed-columns"></a><span data-ttu-id="cdfc0-485">Вычисляемые столбцы SQLite</span><span class="sxs-lookup"><span data-stu-id="cdfc0-485">SQLite computed columns</span></span>

<span data-ttu-id="cdfc0-486">EF Core теперь поддерживает вычисляемые столбцы в базах данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-486">EF Core now supports computed columns in SQLite databases.</span></span>

## <a name="preview-4"></a><span data-ttu-id="cdfc0-487">Предварительная версия 4</span><span class="sxs-lookup"><span data-stu-id="cdfc0-487">Preview 4</span></span>

### <a name="configure-database-precisionscale-in-model"></a><span data-ttu-id="cdfc0-488">Настройка точности и масштаба для базы данных в модели</span><span class="sxs-lookup"><span data-stu-id="cdfc0-488">Configure database precision/scale in model</span></span>

<span data-ttu-id="cdfc0-489">Теперь можно указать точность и масштаб для свойства с помощью построителя модели.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-489">Precision and scale for a property can now be specified using the model builder.</span></span> <span data-ttu-id="cdfc0-490">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-490">For example:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(b => b.Numeric)
    .HasPrecision(16, 4);
```

<span data-ttu-id="cdfc0-491">Точность и масштаб по-прежнему можно задать, используя полный тип базы данных, например "decimal(16,4)".</span><span class="sxs-lookup"><span data-stu-id="cdfc0-491">Precision and scale can still be set via the full database type, such as "decimal(16,4)".</span></span>

<span data-ttu-id="cdfc0-492">Документация отслеживается по проблеме [#527](https://github.com/dotnet/EntityFramework.Docs/issues/527).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-492">Documentation is tracked by issue [#527](https://github.com/dotnet/EntityFramework.Docs/issues/527).</span></span>

### <a name="specify-sql-server-index-fill-factor"></a><span data-ttu-id="cdfc0-493">Указание коэффициента заполнения индекса SQL Server</span><span class="sxs-lookup"><span data-stu-id="cdfc0-493">Specify SQL Server index fill factor</span></span>

<span data-ttu-id="cdfc0-494">Теперь при создании индекса в SQL Server можно указать коэффициент заполнения.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-494">The fill factor can now be specified when creating an index on SQL Server.</span></span> <span data-ttu-id="cdfc0-495">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-495">For example:</span></span>

```csharp
modelBuilder
    .Entity<Customer>()
    .HasIndex(e => e.Name)
    .HasFillFactor(90);
```

## <a name="preview-3"></a><span data-ttu-id="cdfc0-496">Предварительная версия 3</span><span class="sxs-lookup"><span data-stu-id="cdfc0-496">Preview 3</span></span>

### <a name="filtered-include"></a><span data-ttu-id="cdfc0-497">Включение с фильтрацией</span><span class="sxs-lookup"><span data-stu-id="cdfc0-497">Filtered Include</span></span>

<span data-ttu-id="cdfc0-498">Метод Include теперь поддерживает фильтрацию включенных сущностей.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-498">The Include method now supports filtering of the entities included.</span></span> <span data-ttu-id="cdfc0-499">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-499">For example:</span></span>

```csharp
var blogs = context.Blogs
    .Include(e => e.Posts.Where(p => p.Title.Contains("Cheese")))
    .ToList();
```

<span data-ttu-id="cdfc0-500">Этот запрос возвращает блоги со всеми связанными записями, но только в том случае, если заголовок записи содержит Cheese.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-500">This query will return blogs together with each associated post, but only when the post title contains "Cheese".</span></span>

<span data-ttu-id="cdfc0-501">Для сокращения числа включаемых сущностей также можно использовать методы Skip и Take.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-501">Skip and Take can also be used to reduce the number of included entities.</span></span> <span data-ttu-id="cdfc0-502">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-502">For example:</span></span>

```csharp
var blogs = context.Blogs
    .Include(e => e.Posts.OrderByDescending(post => post.Title).Take(5)))
    .ToList();
```
<span data-ttu-id="cdfc0-503">Этот запрос возвратит блоги, включив не более пяти записей для каждого блога.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-503">This query will return blogs with at most five posts included on each blog.</span></span>

<span data-ttu-id="cdfc0-504">Подробные сведения см. в [документации по методу Include](xref:core/querying/related-data#filtered-include).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-504">See the [Include documentation](xref:core/querying/related-data#filtered-include) for full details.</span></span>

### <a name="new-modelbuilder-api-for-navigation-properties"></a><span data-ttu-id="cdfc0-505">Новый API ModelBuilder для свойств навигации</span><span class="sxs-lookup"><span data-stu-id="cdfc0-505">New ModelBuilder API for navigation properties</span></span>

<span data-ttu-id="cdfc0-506">Свойства навигации главным образом настраиваются при [определении связей](xref:core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-506">Navigation properties are primarily configured when [defining relationships](xref:core/modeling/relationships).</span></span> <span data-ttu-id="cdfc0-507">Однако новый метод `Navigation` можно использовать в случаях, когда свойства навигации требуют дополнительной настройки.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-507">However, the new `Navigation` method can be used in the cases where navigation properties need additional configuration.</span></span> <span data-ttu-id="cdfc0-508">Например, чтобы задать резервное поле для навигации, если не удалось найти поле по соглашению:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-508">For example, to set a backing field for the navigation when the field would not be found by convention:</span></span>

```csharp
modelBuilder.Entity<Blog>().Navigation(e => e.Posts).HasField("_myposts");
```

<span data-ttu-id="cdfc0-509">Обратите внимание, что API `Navigation` не является заменой для конфигурации связи.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-509">Note that the `Navigation` API does not replace relationship configuration.</span></span> <span data-ttu-id="cdfc0-510">Вместо этого он допускает дополнительную настройку свойств навигации в уже обнаруженных или определенных связях.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-510">Instead it allows additional configuration of navigation properties in already discovered or defined relationships.</span></span>

<span data-ttu-id="cdfc0-511">См. [документацию по настройке свойств навигации](xref:core/modeling/relationships#configuring-navigation-properties).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-511">See the [Configuring Navigation Properties documentation](xref:core/modeling/relationships#configuring-navigation-properties).</span></span>

### <a name="new-command-line-parameters-for-namespaces-and-connection-strings"></a><span data-ttu-id="cdfc0-512">Новые параметры командной строки для пространств имен и строк подключения</span><span class="sxs-lookup"><span data-stu-id="cdfc0-512">New command-line parameters for namespaces and connection strings</span></span>

<span data-ttu-id="cdfc0-513">Теперь можно указывать пространства имен в командной строке во время миграции и формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-513">Migrations and scaffolding now allow namespaces to be specified on the command line.</span></span> <span data-ttu-id="cdfc0-514">Это можно сделать, например, затем, чтобы реконструировать базу данных, поместив в нее классы контекста и модели в различных пространствах имен:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-514">For example, to reverse engineer a database putting the context and model classes in different namespaces:</span></span>

```dotnetcli
dotnet ef dbcontext scaffold "connection string" Microsoft.EntityFrameworkCore.SqlServer --context-namespace "My.Context" --namespace "My.Model"
```

<span data-ttu-id="cdfc0-515">Подробные сведения см. в документации по [миграции](xref:core/managing-schemas/migrations/index#namespaces) и [реконструированию](xref:core/managing-schemas/scaffolding#directories-and-namespaces).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-515">See the [Migrations](xref:core/managing-schemas/migrations/index#namespaces) and [Reverse Engineering](xref:core/managing-schemas/scaffolding#directories-and-namespaces) documentation for full details.</span></span>

---
<span data-ttu-id="cdfc0-516">Кроме того, теперь можно передать строку подключения команде `database-update`.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-516">Also, a connection string can now be passed to the `database-update` command:</span></span>

```dotnetcli
dotnet ef database update --connection "connection string"
```

<span data-ttu-id="cdfc0-517">Подробные сведения см. в [документации по средствам](xref:core/miscellaneous/cli/dotnet#dotnet-ef-database-update).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-517">See the [Tools documentation](xref:core/miscellaneous/cli/dotnet#dotnet-ef-database-update) for full details.</span></span>

<span data-ttu-id="cdfc0-518">Аналогичные параметры также были добавлены в команды PowerShell, используемые в консоли диспетчера пакетов VS.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-518">Equivalent parameters have also been added to the PowerShell commands used in the VS Package Manager Console.</span></span>

### <a name="enabledetailederrors-has-returned"></a><span data-ttu-id="cdfc0-519">Возвращение EnableDetailedErrors</span><span class="sxs-lookup"><span data-stu-id="cdfc0-519">EnableDetailedErrors has returned</span></span>

<span data-ttu-id="cdfc0-520">По соображениям производительности EF не выполняет дополнительные проверки на NULL при считывании значений из базы данных.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-520">For performance reasons, EF doesn't do additional null-checks when reading values from the database.</span></span> <span data-ttu-id="cdfc0-521">Это может привести к возникновению исключений при обнаружении непредвиденных значений NULL, для которых будет сложно определить причину.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-521">This can result in exceptions that are hard to root-cause when an unexpected null is encountered.</span></span>

<span data-ttu-id="cdfc0-522">При использовании `EnableDetailedErrors` будет добавлена дополнительная проверка на NULL для запросов, чтобы облегчить определение причин этих ошибок за счет небольшого снижения производительности.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-522">Using `EnableDetailedErrors` will add extra null checking to queries such that, for a small performance overhead, these errors are easier to trace back to a root cause.</span></span>

<span data-ttu-id="cdfc0-523">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-523">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .EnableDetailedErrors()
        .EnableSensitiveDataLogging() // Often also useful with EnableDetailedErrors
        .UseSqlServer(Your.SqlServerConnectionString);
```

<span data-ttu-id="cdfc0-524">Документация отслеживается по проблеме [#955](https://github.com/dotnet/EntityFramework.Docs/issues/955).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-524">Documentation is tracked by issue [#955](https://github.com/dotnet/EntityFramework.Docs/issues/955).</span></span>

### <a name="cosmos-partition-keys"></a><span data-ttu-id="cdfc0-525">Ключи секций Cosmos</span><span class="sxs-lookup"><span data-stu-id="cdfc0-525">Cosmos partition keys</span></span>

<span data-ttu-id="cdfc0-526">Теперь можно указать в запросе ключ секции, используемый для данного запроса.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-526">The partition key to use for a given query can now be specified in the query.</span></span> <span data-ttu-id="cdfc0-527">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-527">For example:</span></span>

```csharp
await context.Set<Customer>()
             .WithPartitionKey(myPartitionKey)
             .FirstAsync();
```

<span data-ttu-id="cdfc0-528">Документация отслеживается по проблеме [#2199](https://github.com/dotnet/EntityFramework.Docs/issues/2199).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-528">Documentation is tracked by issue [#2199](https://github.com/dotnet/EntityFramework.Docs/issues/2199).</span></span>

### <a name="support-for-the-sql-server-datalength-function"></a><span data-ttu-id="cdfc0-529">Поддержка функции DATALENGTH SQL Server</span><span class="sxs-lookup"><span data-stu-id="cdfc0-529">Support for the SQL Server DATALENGTH function</span></span>

<span data-ttu-id="cdfc0-530">К ней можно обратиться с помощью нового метода `EF.Functions.DataLength`.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-530">This can be accessed using the new `EF.Functions.DataLength` method.</span></span> <span data-ttu-id="cdfc0-531">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-531">For example:</span></span>

```csharp
var count = context.Orders.Count(c => 100 < EF.Functions.DataLength(c.OrderDate));
```

## <a name="preview-2"></a><span data-ttu-id="cdfc0-532">Предварительная версия 2</span><span class="sxs-lookup"><span data-stu-id="cdfc0-532">Preview 2</span></span>

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a><span data-ttu-id="cdfc0-533">Использование атрибута C# для указания резервного поля свойств</span><span class="sxs-lookup"><span data-stu-id="cdfc0-533">Use a C# attribute to specify a property backing field</span></span>

<span data-ttu-id="cdfc0-534">Теперь атрибут C# можно использовать для указания резервного поля для свойства.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-534">A C# attribute can now be used to specify the backing field for a property.</span></span> <span data-ttu-id="cdfc0-535">Этот атрибут позволяет EF Core в обычном порядке выполнять запись и чтение из резервного поля, даже если резервное поле не может быть найдено автоматически.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-535">This attribute allows EF Core to still write to and read from the backing field as would normally happen, even when the backing field cannot be found automatically.</span></span> <span data-ttu-id="cdfc0-536">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-536">For example:</span></span>

```csharp
public class Blog
{
    private string _mainTitle;

    public int Id { get; set; }

    [BackingField(nameof(_mainTitle))]
    public string Title
    {
        get => _mainTitle;
        set => _mainTitle = value;
    }
}
```

<span data-ttu-id="cdfc0-537">Документация отслеживается по проблеме [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-537">Documentation is tracked by issue [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span></span>

### <a name="complete-discriminator-mapping"></a><span data-ttu-id="cdfc0-538">Завершение сопоставления дискриминатора</span><span class="sxs-lookup"><span data-stu-id="cdfc0-538">Complete discriminator mapping</span></span>

<span data-ttu-id="cdfc0-539">EF Core использует столбец дискриминатора для [сопоставления TPH в иерархии наследования](xref:core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-539">EF Core uses a discriminator column for [TPH mapping of an inheritance hierarchy](xref:core/modeling/inheritance).</span></span> <span data-ttu-id="cdfc0-540">Некоторые улучшения производительности будут поддерживаться до тех пор, пока EF Core будут известны все возможные значения для дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-540">Some performance enhancements are possible so long as EF Core knows all possible values for the discriminator.</span></span> <span data-ttu-id="cdfc0-541">Теперь эти улучшения реализованы в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-541">EF Core 5.0 now implements these enhancements.</span></span>

<span data-ttu-id="cdfc0-542">Например, в предыдущих версиях EF Core всегда создавался следующий код SQL для запроса, возвращающего все типы в иерархии:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-542">For example, previous versions of EF Core would always generate this SQL for a query returning all types in a hierarchy:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
WHERE [a].[Discriminator] IN (N'Animal', N'Cat', N'Dog', N'Human')
```

<span data-ttu-id="cdfc0-543">Теперь при настройке полного сопоставления дискриминатора EF Core 5.0 будет формировать следующее:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-543">EF Core 5.0 will now generate the following when a complete discriminator mapping is configured:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
```

<span data-ttu-id="cdfc0-544">Начиная с предварительной версии 3 это поведение будет доступно по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-544">It will be the default behavior starting with preview 3.</span></span>

### <a name="performance-improvements-in-microsoftdatasqlite"></a><span data-ttu-id="cdfc0-545">Улучшения производительности в Microsoft.Data.Sqlite</span><span class="sxs-lookup"><span data-stu-id="cdfc0-545">Performance improvements in Microsoft.Data.Sqlite</span></span>

<span data-ttu-id="cdfc0-546">В производительность SQLIte были внесены два улучшения:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-546">We have made two performance improvements for SQLIte:</span></span>

* <span data-ttu-id="cdfc0-547">Теперь получение двоичных и строковых данных с помощью GetBytes, GetChars и GetTextReader осуществляется более эффективно за счет использования SqliteBlob и потоков.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-547">Retrieving binary and string data with GetBytes, GetChars, and GetTextReader is now more efficient by making use of SqliteBlob and streams.</span></span>
* <span data-ttu-id="cdfc0-548">Инициализация SqliteConnection выполняется в отложенном режиме.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-548">Initialization of SqliteConnection is now lazy.</span></span>

<span data-ttu-id="cdfc0-549">Эти улучшения добавлены в поставщик Microsoft.Data.Sqlite ADO.NET и, следовательно, позволяют повысить производительность за пределами EF Core.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-549">These improvements are in the ADO.NET Microsoft.Data.Sqlite provider and hence also improve performance outside of EF Core.</span></span>

## <a name="preview-1"></a><span data-ttu-id="cdfc0-550">Предварительная версия 1</span><span class="sxs-lookup"><span data-stu-id="cdfc0-550">Preview 1</span></span>

### <a name="simple-logging"></a><span data-ttu-id="cdfc0-551">Простое ведение журнала</span><span class="sxs-lookup"><span data-stu-id="cdfc0-551">Simple logging</span></span>

<span data-ttu-id="cdfc0-552">Эта функция добавляет возможность, аналогичную `Database.Log` в EF6.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-552">This feature adds functionality similar to `Database.Log` in EF6.</span></span> <span data-ttu-id="cdfc0-553">Это значит, что она позволяет легко получать журналы из EF Core без настройки какой-либо внешней платформы ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-553">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="cdfc0-554">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 5 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-554">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="cdfc0-555">Дополнительная документация отслеживается по проблеме [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-555">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="cdfc0-556">Простой способ получения созданного кода SQL</span><span class="sxs-lookup"><span data-stu-id="cdfc0-556">Simple way to get generated SQL</span></span>

<span data-ttu-id="cdfc0-557">В EF Core 5.0 представлен метод расширения `ToQueryString`, который будет возвращать код SQL, создаваемый в EF Core при выполнении запроса LINQ.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-557">EF Core 5.0 introduces the `ToQueryString` extension method, which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="cdfc0-558">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 9 января 2020 г](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-558">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="cdfc0-559">Дополнительная документация отслеживается по проблеме [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-559">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a><span data-ttu-id="cdfc0-560">Использование атрибута C# для указания того, что сущность не имеет ключа</span><span class="sxs-lookup"><span data-stu-id="cdfc0-560">Use a C# attribute to indicate that an entity has no key</span></span>

<span data-ttu-id="cdfc0-561">Теперь можно настроить тип сущности, не имеющей ключа, используя новый атрибут `KeylessAttribute`.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-561">An entity type can now be configured as having no key using the new `KeylessAttribute`.</span></span> <span data-ttu-id="cdfc0-562">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-562">For example:</span></span>

```csharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

<span data-ttu-id="cdfc0-563">Документация отслеживается по проблеме [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-563">Documentation is tracked by issue [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="cdfc0-564">Возможность изменения подключения или строки подключения в инициализированном DbContext</span><span class="sxs-lookup"><span data-stu-id="cdfc0-564">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="cdfc0-565">Теперь создание экземпляра DbContext без подключения или строки подключения стало еще проще.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-565">It is now easier to create a DbContext instance without any connection or connection string.</span></span> <span data-ttu-id="cdfc0-566">Кроме того, подключение или строку подключения можно изменить в экземпляре контекста.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-566">Also, the connection or connection string can now be mutated on the context instance.</span></span> <span data-ttu-id="cdfc0-567">Эта возможность позволяет одному и тому же экземпляру контекста динамически подключаться к разным базам данных.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-567">This feature allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="cdfc0-568">Документация отслеживается по проблеме [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-568">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="cdfc0-569">Прокси-серверы отслеживания изменений</span><span class="sxs-lookup"><span data-stu-id="cdfc0-569">Change-tracking proxies</span></span>

<span data-ttu-id="cdfc0-570">Теперь EF Core может создавать прокси-серверы среды выполнения, которые автоматически реализуют интерфейсы [INotifyPropertyChanging](/dotnet/api/system.componentmodel.inotifypropertychanging) и [INotifyPropertyChanged](/dotnet/api/system.componentmodel.inotifypropertychanged).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-570">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](/dotnet/api/system.componentmodel.inotifypropertychanging) and [INotifyPropertyChanged](/dotnet/api/system.componentmodel.inotifypropertychanged).</span></span> <span data-ttu-id="cdfc0-571">Затем они сообщают об изменениях значений свойств сущности напрямую в EF Core, что позволяет не выполнять проверку на наличие изменений.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-571">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span> <span data-ttu-id="cdfc0-572">Однако прокси-серверы имеют собственный набор ограничений, поэтому они предназначены не для всех.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-572">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="cdfc0-573">Документация отслеживается по проблеме [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-573">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="cdfc0-574">Улучшенные представления отладки</span><span class="sxs-lookup"><span data-stu-id="cdfc0-574">Enhanced debug views</span></span>

<span data-ttu-id="cdfc0-575">В представлениях отладки удобно просматривать внутренние компоненты EF Core при отладке.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-575">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span> <span data-ttu-id="cdfc0-576">Представление отладки для модели было реализовано некоторое время назад.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-576">A debug view for the Model was implemented some time ago.</span></span> <span data-ttu-id="cdfc0-577">В EF Core 5.0 представление модели стало более удобочитаемым. Кроме того, добавлено новое представление отладки для отслеживаемых сущностей в диспетчере состояний.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-577">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="cdfc0-578">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 12 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-578">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="cdfc0-579">Дополнительная документация отслеживается по проблеме [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-579">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="cdfc0-580">Улучшенная обработка семантики значений NULL в базе данных</span><span class="sxs-lookup"><span data-stu-id="cdfc0-580">Improved handling of database null semantics</span></span>

<span data-ttu-id="cdfc0-581">Реляционные базы данных обычно обрабатывают NULL как неизвестное значение и, следовательно, как не равное любому другому значению NULL.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-581">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span> <span data-ttu-id="cdfc0-582">C# рассматривает NULL как определенное значение, которое аналогично любому другому значению NULL.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-582">While C# treats null as a defined value, which compares equal to any other null.</span></span> <span data-ttu-id="cdfc0-583">EF Core по умолчанию преобразует запросы так, чтобы они использовали семантику значений NULL C#.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-583">EF Core by default translates queries so that they use C# null semantics.</span></span> <span data-ttu-id="cdfc0-584">В EF Core 5.0 эффективность таких преобразований значительно повышена.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-584">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="cdfc0-585">Документация отслеживается по проблеме [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-585">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="cdfc0-586">Свойства индексатора</span><span class="sxs-lookup"><span data-stu-id="cdfc0-586">Indexer properties</span></span>

<span data-ttu-id="cdfc0-587">EF Core 5.0 поддерживает сопоставление свойств индексатора C#.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-587">EF Core 5.0 supports mapping of C# indexer properties.</span></span> <span data-ttu-id="cdfc0-588">Эти свойства позволяют сущностям выступать в качестве контейнеров свойств, в которых столбцы сопоставляются с именованными свойствами в контейнере.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-588">These properties allow entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="cdfc0-589">Документация отслеживается по проблеме [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-589">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="cdfc0-590">Создание ограничений CHECK для сопоставлений перечислений</span><span class="sxs-lookup"><span data-stu-id="cdfc0-590">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="cdfc0-591">Функция миграции в EF Core 5.0 теперь может создавать ограничения CHECK для сопоставлений свойств перечисления.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-591">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span> <span data-ttu-id="cdfc0-592">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-592">For example:</span></span>

```sql
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="cdfc0-593">Документация отслеживается по проблеме [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-593">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="isrelational"></a><span data-ttu-id="cdfc0-594">Является реляционным</span><span class="sxs-lookup"><span data-stu-id="cdfc0-594">IsRelational</span></span>

<span data-ttu-id="cdfc0-595">В дополнение к существующим методам `IsSqlServer`, `IsSqlite` и `IsInMemory` был добавлен новый метод `IsRelational`.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-595">A new `IsRelational` method has been added in addition to the existing `IsSqlServer`, `IsSqlite`, and `IsInMemory`.</span></span> <span data-ttu-id="cdfc0-596">Этот метод можно использовать для проверки того, использует ли DbContext какой-либо поставщик реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-596">This method can be used to test if the DbContext is using any relational database provider.</span></span> <span data-ttu-id="cdfc0-597">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-597">For example:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

<span data-ttu-id="cdfc0-598">Документация отслеживается по проблеме [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-598">Documentation is tracked by issue [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span></span>

### <a name="cosmos-optimistic-concurrency-with-etags"></a><span data-ttu-id="cdfc0-599">Оптимистический параллелизм Cosmos с тегами ETag</span><span class="sxs-lookup"><span data-stu-id="cdfc0-599">Cosmos optimistic concurrency with ETags</span></span>

<span data-ttu-id="cdfc0-600">Поставщик базы данных Azure Cosmos DB теперь поддерживает оптимистичный параллелизм с использованием тегов ETag.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-600">The Azure Cosmos DB database provider now supports optimistic concurrency using ETags.</span></span> <span data-ttu-id="cdfc0-601">Используйте построитель моделей в OnModelCreating для настройки ETag:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-601">Use the model builder in OnModelCreating to configure an ETag:</span></span>

```csharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

<span data-ttu-id="cdfc0-602">Затем SaveChanges генерирует исключение `DbUpdateConcurrencyException` при конфликте параллелизма, которое [можно обработать](xref:core/saving/concurrency) для реализации повторных попыток и т. д.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-602">SaveChanges will then throw an `DbUpdateConcurrencyException` on a concurrency conflict, which [can be handled](xref:core/saving/concurrency) to implement retries, etc.</span></span>

<span data-ttu-id="cdfc0-603">Документация отслеживается по проблеме [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-603">Documentation is tracked by issue [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="cdfc0-604">Преобразования запросов для дополнительных конструкций DateTime</span><span class="sxs-lookup"><span data-stu-id="cdfc0-604">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="cdfc0-605">Теперь запросы, содержащие новую конструкцию DateTime, поддерживают преобразование.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-605">Queries containing new DateTime construction are now translated.</span></span>

<span data-ttu-id="cdfc0-606">Кроме того, теперь сопоставлены следующие функции SQL Server:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-606">In addition, the following SQL Server functions are now mapped:</span></span>

* <span data-ttu-id="cdfc0-607">DateDiffWeek</span><span class="sxs-lookup"><span data-stu-id="cdfc0-607">DateDiffWeek</span></span>
* <span data-ttu-id="cdfc0-608">DateFromParts</span><span class="sxs-lookup"><span data-stu-id="cdfc0-608">DateFromParts</span></span>

<span data-ttu-id="cdfc0-609">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-609">For example:</span></span>

```csharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

<span data-ttu-id="cdfc0-610">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-610">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="cdfc0-611">Преобразования запросов для дополнительных конструкций массивов байтов</span><span class="sxs-lookup"><span data-stu-id="cdfc0-611">Query translations for more byte array constructs</span></span>

<span data-ttu-id="cdfc0-612">Запросы, использующие свойства Contains, Length, SequenceEqual и т. д. в массиве byte[], преобразуются в SQL.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-612">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="cdfc0-613">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 5 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-613">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="cdfc0-614">Дополнительная документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-614">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="cdfc0-615">Преобразование запросов, использующих Reverse</span><span class="sxs-lookup"><span data-stu-id="cdfc0-615">Query translation for Reverse</span></span>

<span data-ttu-id="cdfc0-616">Теперь можно преобразовывать запросы, использующие `Reverse`.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-616">Queries using `Reverse` are now translated.</span></span> <span data-ttu-id="cdfc0-617">Пример:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-617">For example:</span></span>

```csharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="cdfc0-618">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-618">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="cdfc0-619">Преобразование запросов для побитовых операторов</span><span class="sxs-lookup"><span data-stu-id="cdfc0-619">Query translation for bitwise operators</span></span>

<span data-ttu-id="cdfc0-620">Запросы, использующие побитовые операторы, теперь преобразуются в ряде дополнительных случаев, например:</span><span class="sxs-lookup"><span data-stu-id="cdfc0-620">Queries using bitwise operators are now translated in more cases For example:</span></span>

```csharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="cdfc0-621">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-621">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="cdfc0-622">Преобразование запросов для строк в Cosmos</span><span class="sxs-lookup"><span data-stu-id="cdfc0-622">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="cdfc0-623">Запросы, использующие строковые методы Contains, StartsWith и EndsWith, теперь преобразуются при использовании поставщика Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="cdfc0-623">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="cdfc0-624">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="cdfc0-624">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
