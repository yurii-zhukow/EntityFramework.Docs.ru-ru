---
title: Новые возможности EF Core 5.0
description: Обзор новых возможностей в EF Core 5.0
author: ajcvickers
ms.date: 09/10/2020
uid: core/what-is-new/ef-core-5.0/whatsnew
ms.openlocfilehash: 3efa883cdfac1ecd412112ef06c7763f1a7e12f1
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94429251"
---
# <a name="whats-new-in-ef-core-50"></a>Новые возможности EF Core 5.0

Все функции, запланированные в EF Core 5.0, реализованы. На этой странице представлен обзор интересных изменений, появившихся в каждой предварительной версии.

Эта страница не является аналогом статьи о [планировании для EF Core 5.0](xref:core/what-is-new/ef-core-5.0/plan). В плане описываются общие темы для EF Core 5.0, в том числе все, что планируется включить перед выпуском окончательной версии.

## <a name="rc1"></a>RC1

### <a name="many-to-many"></a>"Много ко многим"

В EF Core 5.0 поддерживаются связи "многие ко многим" без явного сопоставления таблицы соединения.

Например, рассмотрим такие типы сущностей:

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

Обратите внимание, что `Post` содержит коллекцию `Tags`, а `Tag` содержит коллекцию `Posts`. В соответствии с соглашением в EF Core 5.0 это распознается как отношение "многие ко многим". Поэтому код в `OnModelCreating` не требуется:

```csharp
public class BlogContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }
}
```

Если для создания базы данных используются миграции (или `EnsureCreated`), EF Core автоматически создает таблицу соединения. Например, в SQL Server для этой модели EF Core создает:

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

Создание и связывание сущностей `Tag` и `Post` приводит к автоматическому обновлению таблицы соединения. Пример:

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

После вставки записей и тегов EF автоматически создаст строки в таблице соединения. Например, в SQL Server.

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

Включение и другие операции запросов выполняются так же, как для любой другой связи. Пример:

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

Создаваемый код SQL автоматически использует таблицу соединения, чтобы вернуть все связанные теги:

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

В отличие от EF6, EF Core позволяет полностью настраивать таблицу соединения. Например, приведенный ниже код настраивает связь "многие ко многим", которая также содержит навигацию на сущность соединения и в которой сущность соединения содержит свойство полезной нагрузки.

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

> [!NOTE]
> Поддержка формирования шаблонов для связей "многие ко многим" из базы данных еще не добавлена. Отслеживайте решение этого вопроса [здесь](https://github.com/dotnet/efcore/issues/22475).

### <a name="map-entity-types-to-queries"></a>Сопоставление типов сущностей с запросами

Типы сущностей обычно сопоставляются с таблицами или представлениями так, что EF Core будет извлекать содержимое таблицы или представления при запросе соответствующего типа. EF Core 5.0 позволяет сопоставить тип сущности с "определяющим запросом". (Эта возможность частично поддерживалось в предыдущих версиях, но была значительно улучшена и имеет другой синтаксис в EF Core 5.0.)

Например, рассмотрим две таблицы: одна с актуальными записями, а другая — с устаревшими. Таблица с актуальными записями содержит ряд дополнительных столбцов, но для приложения необходимо объединить актуальные и устаревшие записи и сопоставить их с типом сущности со всеми необходимыми свойствами:

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

В EF Core 5.0 можно использовать `ToSqlQuery`, чтобы сопоставить этот тип сущности с запросом, который извлекает и объединяет строки из обеих таблиц:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Post>().ToSqlQuery(
        @"SELECT Id, Name, Category, BlogId FROM posts
          UNION ALL
          SELECT Id, Name, ""Legacy"", BlogId from legacy_posts");
}
```

Обратите внимание, что в таблице `legacy_posts` нет столбца `Category`, поэтому мы создадим значение по умолчанию для всех устаревших записей.

Затем этот тип сущности можно использовать обычным образом для запросов LINQ. Например, если выбран диапазон 10.0.0.0/20 для виртуальной сети, для пространства клиентских адресов можно выбрать 10.1.0.0/24. Запрос LINQ:

```csharp
var posts = context.Posts.Where(e => e.Blog.Name.Contains("Unicorn")).ToList();
```

Создает следующий код SQL в SQLite:

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

Обратите внимание, что запрос, настроенный для типа сущности, используется в качестве отправной точки для составления полного запроса LINQ.

### <a name="event-counters"></a>Счетчики событий

[Счетчики событий .NET](https://devblogs.microsoft.com/dotnet/introducing-diagnostics-improvements-in-net-core-3-0/) позволяют эффективно предоставлять метрики производительности из приложения. В EF Core 5.0 счетчики событий включены в категорию `Microsoft.EntityFrameworkCore`. Пример:

```console
dotnet counters monitor Microsoft.EntityFrameworkCore -p 49496
```

Эта команда предписывает счетчикам dotnet начать сбор событий EF Core для процесса 49496. В консоли будут выведены выходные данные следующего вида:

```console
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

### <a name="property-bags"></a>Контейнеры свойств

В EF Core 5.0 один тип CLR может быть сопоставлен с несколькими различными типами сущностей. Такие типы называются общими типами сущностей. Эта возможность в сочетании со свойствами индексатора (включенным в предварительную версию 1) позволяет использовать контейнеры свойств как тип сущности.

Например, в приведенном ниже DbContext тип BCL `Dictionary<string, object>` настраивается как общий тип сущности для продуктов и категорий.

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

Объекты словаря ("контейнеры свойств") теперь можно добавлять в контекст как экземпляры сущности и сохранять. Пример:

```csharp
var beverages = new Dictionary<string, object>
{
    ["Name"] = "Beverages",
    ["Description"] = "Stuff to sip on"
};

context.Categories.Add(beverages);

context.SaveChanges();
```

Затем эти сущности можно запрашивать и обновлять обычным образом:

```csharp
var foods = context.Categories.Single(e => e["Name"] == "Foods");
var marmite = context.Products.Single(e => e["Name"] == "Marmite");

marmite["CategoryId"] = foods["Id"];
marmite["Description"] = "Yummy when spread _thinly_ on buttered Toast!";

context.SaveChanges();
```

### <a name="savechanges-interception-and-events"></a>Перехват и события SaveChanges

В EF Core 5.0 появились события .NET и перехватчик EF Core, запускаемый при вызове SaveChanges.

События просты в использовании, например:

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

Обратите внимание на указанные ниже моменты.

* Отправителем события является экземпляр `DbContext`.
* Аргументы события `SavedChanges` содержат число сущностей, сохраненных в базе данных.

Перехватчик определяется `ISaveChangesInterceptor`, но часто удобнее наследовать его от `SaveChangesInterceptor`, чтобы не приходилось реализовывать каждый метод. Пример:

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

Обратите внимание на указанные ниже моменты.

* Перехватчик имеет как синхронные, так и асинхронные методы. Это может быть полезно, если требуется выполнить асинхронный ввод-вывод, например запись на сервер аудита.
* Перехватчик позволяет пропустить метод SaveChanges, используя механизм `InterceptionResult`, общий для всех перехватчиков.

Недостаток перехватчиков заключается в том, что они должны регистрироваться в объекте DbContext при его создании. Пример:

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .AddInterceptors(new MySaveChangesInterceptor())
        .UseSqlite("Data Source = test.db");
```

События же, напротив, могут регистрироваться в экземпляре DbContext в любое время.

### <a name="exclude-tables-from-migrations"></a>Исключение таблиц из миграций

Иногда полезно иметь один тип сущности, сопоставленный в нескольких экземплярах DbContext. Это особенно справедливо при использовании [ограниченных контекстов](https://www.martinfowler.com/bliki/BoundedContext.html), для каждого из которых часто используется отдельный тип DbContext.

Например, тип `User` может требоваться как для контекста авторизации, так и для контекста отчетов. При внесении изменения в тип `User` миграции для обоих экземпляров DbContext будут пытаться обновить базу данных. Чтобы избежать этого, модель для одного из контекстов можно настроить так, чтобы таблица исключалась из миграций.

В приведенном ниже коде `AuthorizationContext` создает миграции для изменений в таблице `Users`, а `ReportingContext` не создает, благодаря чему предотвращаются конфликты миграций.

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

### <a name="required-11-dependents"></a>Обязательные зависимости "один к одному"

В EF Core 3.1 зависимая сторона связи "один к одному" всегда считалась необязательной. Это было особенно очевидно при использовании принадлежащих сущностей. Например, рассмотрим следующую модель и конфигурацию:

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

В результате миграции создают следующую таблицу для SQLite:

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

Обратите внимание, что все столбцы допускают значение NULL, даже несмотря на то, что некоторые свойства `HomeAddress` настроены как обязательные. Кроме того, если все столбцы домашнего или рабочего адреса имеют значение NULL, при запросе `Person` EF Core оставит свойства `HomeAddress` и (или) `WorkAddress` со значением NULL вместо задания пустого экземпляра `Address`.

В EF Core 5.0 навигацию `HomeAddress` теперь можно настроить как обязательную зависимость. Пример:

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

Таблица, создаваемая миграциями, теперь будет включать столбцы, не допускающие значения NULL, для обязательных свойств требуемой зависимости:

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

Кроме того, EF Core теперь будет вызывать исключение при попытке сохранить владельца, который имеет обязательную зависимость со значением NULL. В этом примере EF Core выдает исключение при попытке сохранить `Person` со значением NULL для `HomeAddress`.

Наконец, EF Core будет по-прежнему создавать экземпляр обязательной зависимости, даже если все ее столбцы имеют значения NULL.

### <a name="options-for-migration-generation"></a>Варианты создания миграции

В EF Core 5.0 улучшен контроль над созданием миграций в различных целях. Обеспечены следующие возможности:

* определение того, создается ли миграция для скрипта или для немедленного выполнения;
* определение того, создается ли идемпотентный скрипт;
* определение того, должен ли скрипт исключать инструкции транзакций (см. раздел _Скрипты миграции с транзакциями_ ниже).

Это поведение задается перечислением `MigrationsSqlGenerationOptions`, которое теперь можно передать в `IMigrator.GenerateScript`.

Кроме того, улучшено создание идемпотентных скриптов с вызовами `EXEC` в SQL Server, когда это необходимо. Аналогичные улучшения доступны для скриптов, создаваемых другими поставщиками баз данных, включая PostgreSQL.

### <a name="migrations-scripts-with-transactions"></a>Скрипты миграции с транзакциями

Скрипты SQL, создаваемые миграциями, теперь содержат инструкции для начала и фиксации транзакций, необходимых для миграции. Например, приведенный ниже скрипт миграции был создан на основе двух миграций. Обратите внимание, что каждая миграция теперь применяется внутри транзакции.

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

Как было упомянуто в предыдущем разделе, такое использование транзакций можно отключить, если транзакции должны обрабатываться по-другому.

### <a name="see-pending-migrations"></a>См. раздел, посвященный ожидающим выполнения транзакциям.

Эту функцию предоставил участник сообщества [@Psypher9](https://github.com/Psypher9). Большое спасибо за публикацию!

Команда `dotnet ef migrations list` теперь показывает, какие миграции еще не были применены к базе данных. Пример:

```console
ajcvickers@avickers420u:~/AllTogetherNow/Daily$ dotnet ef migrations list
Build started...
Build succeeded.
20200910201647_One
20200910201708_Two
20200910202050_Three (Pending)
ajcvickers@avickers420u:~/AllTogetherNow/Daily$
```

Кроме того, теперь есть команда `Get-Migration` для консоли диспетчера пакетов с аналогичной функциональностью.

### <a name="modelbuilder-api-for-value-comparers"></a>API ModelBuilder для функций сравнения значений

В EF Core для правильного обнаружения изменений свойств пользовательских изменяемых типов [требуется функция сравнения значений](xref:core/modeling/value-comparers). Теперь ее можно указать в процессе настройки преобразования значения для типа. Пример:

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

### <a name="entityentry-trygetvalue-methods"></a>Методы EntityEntry TryGetValue

Эту функцию предоставил участник сообщества [@m4ss1m0g](https://github.com/m4ss1m0g). Большое спасибо за публикацию!

Метод `TryGetValue` был добавлен в `EntityEntry.CurrentValues` и `EntityEntry.OriginalValues`. Это позволяет запрашивать значение свойства, не проверяя предварительно, сопоставлено ли свойство в модели EF. Пример:

```csharp
if (entry.CurrentValues.TryGetValue(propertyName, out var value))
{
    Console.WriteLine(value);
}
```

### <a name="default-max-batch-size-for-sql-server"></a>Максимальный размер пакета по умолчанию для SQL Server

Начиная с EF Core 5.0 максимальный размер пакета по умолчанию для SaveChanges в SQL Server равен 42. Как хорошо известно, это также ответ на главный вопрос жизни, вселенной и вообще. Однако, вероятно, это совпадение, так как данное значение было получено в результате [анализа производительности пакетной обработки](https://github.com/dotnet/efcore/issues/9270). Не думаем, что мы нашли окончательный ответ, хотя вполне возможно, что Земля была создана лишь с целью познать, почему SQL Server работает именно так.

### <a name="default-environment-to-development"></a>Использование среды разработки по умолчанию

Программы командной строки EF Core теперь автоматически задают для переменных среды `ASPNETCORE_ENVIRONMENT` _и_ `DOTNET_ENVIRONMENT` значение Development. Это обеспечивает согласованную работу при использовании универсального узла и ASP.NET Core во время разработки. См. описание проблемы [№ 19903](https://github.com/dotnet/efcore/issues/19903).

### <a name="better-migrations-column-ordering"></a>Оптимизация порядка столбцов при миграции

Столбцы для несопоставленных базовых классов теперь упорядочиваются после столбцов для сопоставленных типов сущностей. Обратите внимание, что это касается только новых создаваемых таблиц. Порядок столбцов для существующих таблиц не меняется. См. описание проблемы [№ 11314](https://github.com/dotnet/efcore/issues/11314).

### <a name="query-improvements"></a>Улучшения запросов

В EF Core 5.0 RC1 появились дополнительные улучшения преобразования запросов.

* Преобразование `is` в Cosmos — см. описание проблемы [№ 16391](https://github.com/dotnet/efcore/issues/16391).
* Сопоставленные пользователем функции теперь можно аннотировать для управления распространением значений NULL — см. описание проблемы [№ 19609](https://github.com/dotnet/efcore/issues/19609).
* Поддержка преобразования GroupBy с условными статистическими выражениями — см. описание проблемы [№ 11711](https://github.com/dotnet/efcore/issues/11711).
* Преобразование оператора Distinct с групповым элементом перед статистическим выражением — см. описание проблемы [№ 17376](https://github.com/dotnet/efcore/issues/17376).

### <a name="model-building-for-fields"></a>Формирование модели для полей

Наконец, в EF Core RC1 теперь можно использовать лямбда-методы в ModelBuilder как для полей, так и для свойств. Например, если свойства вам по какой-либо причине не нравятся и вы хотите использовать открытые поля, теперь такие поля можно сопоставить с помощью построителей лямбда-выражений:

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

Однако, хотя такая возможность и имеется, мы не рекомендуем пользоваться ею. Кроме того, обратите внимание, что возможности сопоставления полей в EF Core не были расширены. Просто вместо строковых методов, которые раньше требовались всегда, теперь можно использовать лямбда-методы. Это бывает полезно нечасто, так как поля редко являются открытыми.

## <a name="preview-8"></a>предварительная версия 8

### <a name="table-per-type-tpt-mapping"></a>Сопоставление типа "одна таблица на тип" (TPT)

По умолчанию в EF Core иерархия наследования типов .NET сопоставляется с одной таблицей базы данных. Это называется сопоставлением "одна таблица на иерархию" (TPH). В EF Core 5.0 каждый тип .NET в иерархии наследования также может сопоставляться с отдельной таблицей базы данных. Это называется сопоставлением "одна таблица на тип" (TPT).

Например, возьмем следующую модель с сопоставленной иерархией.

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

По умолчанию в EF Core она будет сопоставлена с одной таблицей.

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

Однако сопоставление каждого типа сущности с отдельной таблицей приведет к тому, что для каждого типа будет одна таблица.

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

Обратите внимание, что возможность создания ограничений внешнего ключа, как показано выше, появилась после создания ветви кода для предварительной версии 8.

Типы сущностей можно сопоставлять с отдельными таблицами с помощью атрибутов сопоставления:

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

Или с помощью конфигурации `ModelBuilder`:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Animal>().ToTable("Animals");
    modelBuilder.Entity<Pet>().ToTable("Pets");
    modelBuilder.Entity<Cat>().ToTable("Cats");
    modelBuilder.Entity<Dog>().ToTable("Dogs");
}
```

Документация отслеживается по проблеме [#1979](https://github.com/dotnet/EntityFramework.Docs/issues/1979).

### <a name="migrations-rebuild-sqlite-tables"></a>Миграции: перестроение таблиц SQLite

По сравнению с другими базами данных SQLite имеет относительно ограниченные возможности работы со схемами. Например, для удаления столбца из существующей таблицы требуется удалить и повторно создать всю таблицу. Функция миграции в EF Core 5.0 теперь поддерживает автоматическое перестроение таблицы, если этого требуют изменения схемы.

Например, представьте, что имеется таблица `Unicorns`, созданная для типа сущности `Unicorn`.

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

Но оказывается, что хранить сведения о возрасте единорога не имеет смысла, поэтому мы удалим это свойство, добавим новую миграцию и обновим базу данных. В EF Core 3.1 это изменение завершится ошибкой, так как столбец удалить нельзя. В EF Core 5.0 функция миграция перестроит таблицу.

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

Обратите внимание на указанные ниже моменты.

* Для новой таблицы создается временная таблица с требуемой схемой.
* Данные копируются из текущей таблицы во временную.
* Принудительное применение внешнего ключа отключается.
* Текущая таблица удаляется.
* Временная таблица переименовывается в новую.

Документация отслеживается по проблеме [#2583](https://github.com/dotnet/EntityFramework.Docs/issues/2583).

### <a name="table-valued-functions"></a>Функции с табличным значением

Эту функцию предоставил участник сообщества [@pmiddleton](https://github.com/pmiddleton). Большое спасибо за публикацию!

EF Core 5.0 превосходно поддерживает сопоставление методов .NET с функциями с табличным значением (TVF). Эти функции затем можно использовать в запросах LINQ, причем дополнительная композиция результатов функции также преобразовывается в SQL.

Например, рассмотрим такую функцию с табличным значением, определенную в базе данных SQL Server.

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

Для использования этой функции в модели EF Core требуются два типа сущностей:

* тип `Employee`, который сопоставляется с таблицей Employees обычным образом;
* тип `Report`, который соответствует форме, возвращаемой функцией с табличным значением.

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

Эти типы должны быть включены в модель EF Core.

```csharp
modelBuilder.Entity<Employee>();
modelBuilder.Entity(typeof(Report)).HasNoKey();
```

Обратите внимание, что класс `Report` не имеет первичного ключа и поэтому должен быть настроен в его качестве.

Наконец, метод .NET должен быть сопоставлен с функцией с табличным значением в базе данных. Этот метод можно определить в DbContext с помощью нового метода `FromExpression`.

```csharp
public IQueryable<Report> GetReports(int managerId)
    => FromExpression(() => GetReports(managerId));
```

Этот метод использует параметр и возвращаемый тип, которые соответствуют приведенной выше функции с табличным значением. Затем метод добавляется в модель EF Core в OnModelCreating.

```csharp
modelBuilder.HasDbFunction(() => GetReports(default));
```

(Лямбда-выражение используется здесь, чтобы упростить передачу `MethodInfo` в EF Core. Аргументы, передаваемые в метод, игнорируются.)

Теперь можно создавать запросы, которые вызывают `GetReports` и составляются по результатам. Пример:

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

В SQL Server этот код преобразуется в следующий.

```sql
SELECT [e].[Name] AS [ManagerName], [g].[Name] AS [EmployeeName]
FROM [Employees] AS [e]
CROSS APPLY [dbo].[GetReports]([e].[Id]) AS [g]
WHERE [g].[IsDeveloper] = CAST(1 AS bit)
```

Обратите внимание, что SQL-запрос основан на таблице `Employees`, вызывает `GetReports`, а затем добавляет дополнительное предложение WHERE на основе результатов функции.

### <a name="flexible-queryupdate-mapping"></a>Гибкое сопоставление запросов и обновлений

EF Core 5.0 позволяет сопоставлять один и тот же тип сущности с различными объектами базы данных. Этими объектами могут быть таблицы, представления или функции.

Например, тип сущности может быть сопоставлен как с представлением базы данных, так и с таблицей базы данных.

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder
        .Entity<Blog>()
        .ToTable("Blogs")
        .ToView("BlogsView");
}
```

По умолчанию в таком случае EF Core будет запрашивать данные из представления и отправлять обновления в таблицу. Например, выполнение следующего кода:

```csharp
var blog = context.Set<Blog>().Single(e => e.Name == "One Unicorn");

blog.Name = "1unicorn2";

context.SaveChanges();
```

приводит к выполнению запроса к представлению и последующему обновлению таблицы.

```sql
SELECT TOP(2) [b].[Id], [b].[Name], [b].[Url]
FROM [BlogsView] AS [b]
WHERE [b].[Name] = N'One Unicorn'

SET NOCOUNT ON;
UPDATE [Blogs] SET [Name] = @p0
WHERE [Id] = @p1;
SELECT @@ROWCOUNT;
```

### <a name="context-wide-split-query-configuration"></a>Конфигурация с разделением запросов в пределах контекста

Разделение запросов (см. ниже) теперь можно настроить по умолчанию для любого запроса, выполняемого DbContext. Эта конфигурация доступна только для реляционных поставщиков и поэтому должна указываться в конфигурации `UseProvider`. Пример:

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(
            Your.SqlServerConnectionString,
            b => b.UseQuerySplittingBehavior(QuerySplittingBehavior.SplitQuery));
```

Документация отслеживается по проблеме [#2407](https://github.com/dotnet/EntityFramework.Docs/issues/2407).

### <a name="physicaladdress-mapping"></a>Сопоставление PhysicalAddress

Эту функцию предоставил участник сообщества [@ralmsdeveloper](https://github.com/ralmsdeveloper). Большое спасибо за публикацию!

Стандартный [класс PhysicalAddress](/dotnet/api/system.net.networkinformation.physicaladdress) в .NET теперь автоматически сопоставляется со строковым столбцом для баз данных, у которых пока нет собственной поддержки. Дополнительные сведения см. в примерах для `IPAddress` ниже.

## <a name="preview-7"></a>предварительная версия 7

### <a name="dbcontextfactory"></a>DbContextFactory

В EF Core 5.0 введены методы `AddDbContextFactory` и `AddPooledDbContextFactory` для регистрации производства с целью создания экземпляров DbContext в контейнере внедрения зависимостей приложения. Пример:

```csharp
services.AddDbContextFactory<SomeDbContext>(b =>
    b.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Test"));
```

Службы приложений, такие как контроллеры ASP.NET Core, могут затем использовать `IDbContextFactory<TContext>` в конструкторе службы. Пример:

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

После этого по мере необходимости можно создавать и использовать экземпляры DbContext. Пример:

```csharp
public void DoSomeThing()
{
    using (var context = _contextFactory.CreateDbContext())
    {
        // ...
    }
}
```

Обратите внимание, что созданные таким образом экземпляры DbContext _не_ управляются поставщиком служб приложения и поэтому должны удаляться приложением. Такое разделение очень полезно для приложений Blazor, в которых рекомендуется использовать `IDbContextFactory`, но может быть полезно и в других сценариях.

Экземпляры DbContext можно объединять в пул путем вызова `AddPooledDbContextFactory`. Объединение в пул выполняется так же, как для `AddDbContextPool`, и имеет те же ограничения.

Документация отслеживается по проблеме [#2523](https://github.com/dotnet/EntityFramework.Docs/issues/2523).

### <a name="reset-dbcontext-state"></a>Сброс состояния DbContext

В EF Core 5.0 появился метод `ChangeTracker.Clear()`, который очищает DbContext всех отслеживаемых сущностей. Обычно это не требуется, если выполняется рекомендация по созданию нового экземпляра контекста с кратким сроком существования для каждой единицы работы. Однако если необходимо сбросить состояние экземпляра DbContext, то использование нового метода `Clear()` является более производительным и надежным подходом, чем массовое отсоединение всех сущностей.

Документация отслеживается по проблеме [#2524](https://github.com/dotnet/EntityFramework.Docs/issues/2524).

### <a name="new-pattern-for-store-generated-defaults"></a>Новый шаблон для значений по умолчанию, создаваемых хранилищем

EF Core позволяет явно задать значение для столбца, у которого может быть ограничение на значение по умолчанию. В качестве индикатора для этого в EF Core используется значение, принятое по умолчанию для типа свойства в CLR. Если значение отлично от значения CLR по умолчанию, оно будет вставлено. В противном случае используется значение по умолчанию для базы данных.

Это создает проблемы для типов, для которых значение CLR по умолчанию является неподходящим индикатором, особенно для свойств `bool`. В таких случаях EF Core 5.0 теперь допускает значение NULL для резервного поля. Пример:

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

Обратите внимание, что резервное поле допускает значение NULL, а общедоступное свойство — нет. В результате значение индикатора может быть `null`, и это не влияет на общую контактную зону типа сущности. Если `IsValid` не задается, используется значение по умолчанию для базы данных, так как значением резервного поля остается NULL. Если задается `true` или `false`, это значение сохраняется явным образом в базе данных.

Документация отслеживается по проблеме [#2525](https://github.com/dotnet/EntityFramework.Docs/issues/2525).

### <a name="cosmos-partition-keys"></a>Ключи секций Cosmos

EF Core позволяет включать ключ секции Cosmos в модель EF. Пример:

```csharp
modelBuilder.Entity<Customer>().HasPartitionKey(b => b.AlternateKey)
```

Начиная с предварительной версии 7 ключ секции включается в первичный ключ типа сущности и используется для повышения производительности некоторых запросов.

Документация отслеживается по проблеме [#2471](https://github.com/dotnet/EntityFramework.Docs/issues/2471).

### <a name="cosmos-configuration"></a>Конфигурация Cosmos

В EF Core 5.0 улучшена настройка Cosmos и подключений Cosmos.

Ранее в EF Core требовалось явно указывать конечную точку и ключ при подключении к базе данных Cosmos. В EF Core 5.0 вместо этого можно использовать строку подключения. Кроме того, в EF Core 5.0 можно явно задавать экземпляр класса WebProxy. Пример:

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseCosmos("my-cosmos-connection-string", "MyDb",
            cosmosOptionsBuilder =>
            {
                cosmosOptionsBuilder.WebProxy(myProxyInstance);
            });
```

Кроме того, теперь также можно настраивать множество других значений времени ожидания, ограничений и т. д. Пример:

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

Наконец, режимом соединения по умолчанию теперь является `ConnectionMode.Gateway`, что, как правило, обеспечивает большую совместимость.

Документация отслеживается по проблеме [#2471](https://github.com/dotnet/EntityFramework.Docs/issues/2471).

### <a name="scaffold-dbcontext-now-singularizes"></a>Формирование шаблонов DbContext теперь производится в форме единственного числа

Ранее при формировании шаблона DbContext на основе существующей базы данных в EF Core создавались имена типов сущностей, совпадающие с именами таблиц в базе данных. Например, для таблиц `People` и `Addresses` создавались типы сущностей с именами `People` и `Addresses`.

В предыдущих выпусках это поведение можно было настраивать путем регистрации службы преобразования во множественную форму. Теперь в EF Core 5.0 в качестве службы преобразования во множественную форму по умолчанию используется пакет [Humanizer](https://www.nuget.org/packages/Humanizer.Core/). Это означает, что таблицы `People` и `Addresses` теперь будут реконструированы в типы сущностей с именами `Person` и `Address`.

### <a name="savepoints"></a>Точки сохранения

EF Core теперь поддерживает [точки сохранения](/sql/t-sql/language-elements/save-transaction-transact-sql#remarks) для лучшего контроля над транзакциями, выполняющими несколько операций.

Точки сохранения можно создавать, освобождать и откатывать вручную. Пример:

```csharp
context.Database.CreateSavepoint("MySavePoint");
```

Кроме того, при неудачном выполнении `SaveChanges` в EF Core теперь выполняется откат к последней точке сохранения. Это позволяет повторно выполнять вызов SaveChanges без повторного выполнения всей транзакции.

Документация отслеживается по проблеме [#2429](https://github.com/dotnet/EntityFramework.Docs/issues/2429).

## <a name="preview-6"></a>предварительная версия 6

### <a name="split-queries-for-related-collections"></a>Разделение запросов для связанных коллекций

Начиная с EF Core 3.0, EF Core всегда создает один SQL-запрос для каждого LINQ-запроса. Это обеспечивает согласованность данных, возвращаемых в рамках ограничений используемого режима транзакций. Однако если запрос использует `Include` или проекцию для возврата нескольких связанных коллекций, это может привести к снижению производительности.

Теперь EF Core 5.0 позволяет разделять один LINQ-запрос, включая связанные коллекции, на несколько SQL-запросов. Это может значительно повысить производительность, но может также привести к несогласованности возвращаемых результатов в случае изменения данных между двумя запросами. Для устранения этой проблемы и обеспечения согласованности с разделенными запросами можно использовать сериализуемые транзакции или транзакции с моментальными снимками, но это может привести к дополнительным затратам на производительность и вызвать различия в поведении.

#### <a name="split-queries-with-include"></a>Разделение запросов с помощью Include

Рассмотрим запрос, который извлекает два уровня связанных коллекций с помощью `Include`:

```csharp
var artists = context.Artists
    .Include(e => e.Albums).ThenInclude(e => e.Tags)
    .ToList();
```

Если используется поставщик SQLite, EF Core по умолчанию создаст следующий SQL:

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

Для изменения этого поведения можно использовать новый API `AsSplitQuery`. Пример:

```csharp
var artists = context.Artists
    .AsSplitQuery()
    .Include(e => e.Albums).ThenInclude(e => e.Tags)
    .ToList();
```

AsSplitQuery доступен для всех поставщиков реляционных баз данных и может использоваться в любом месте запроса так же, как AsNoTracking. Теперь EF Core будет создавать следующие три SQL-запроса:

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

Поддерживаются все операции в корне запроса. В их число входят операции OrderBy/Skip/Take, Join, FirstOrDefault и аналогичные операции выбора одного результата.

Обратите внимание, что включения с фильтрацией с операциями OrderBy/Skip/Take не поддерживаются в предварительной версии 6, но они доступны в ежедневных сборках и будут включены в предварительную версию 7.

#### <a name="split-queries-with-collection-projections"></a>Разделение запросов с помощью проекций коллекции

`AsSplitQuery` можно также использовать при загрузке коллекций в проекции. Пример:

```csharp
context.Artists
    .AsSplitQuery()
    .Select(e => new
    {
        Artist = e,
        Albums = e.Albums,
    }).ToList();
```

При использовании поставщика SQLite этот LINQ-запрос создает следующие два SQL-запроса:

```sql
SELECT "a"."Id", "a"."Name"
FROM "Artists" AS "a"
ORDER BY "a"."Id"

SELECT "a0"."Id", "a0"."ArtistId", "a0"."Title", "a"."Id"
FROM "Artists" AS "a"
INNER JOIN "Album" AS "a0" ON "a"."Id" = "a0"."ArtistId"
ORDER BY "a"."Id"
```

Обратите внимание, что поддерживается только материализация коллекции. Любая композиция после `e.Albums` в приведенном выше случае не приведет к разделению запроса. Улучшения в этой области отслеживаются по проблеме [#21234](https://github.com/dotnet/efcore/issues/21234).

### <a name="indexattribute"></a>IndexAttribute

Новый IndexAttribute можно поместить в тип сущности, чтобы указать индекс для одного столбца. Пример:

```csharp
[Index(nameof(FullName), IsUnique = true)]
public class User
{
    public int Id { get; set; }

    [MaxLength(128)]
    public string FullName { get; set; }
}
```

Для SQL Server функция миграции создаст следующий SQL:

```sql
CREATE UNIQUE INDEX [IX_Users_FullName]
    ON [Users] ([FullName])
    WHERE [FullName] IS NOT NULL;
```

IndexAttribute можно также использовать для указания индекса, охватывающего несколько столбцов. Пример:

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

В SQL Server будет выведен следующий результат:

```sql
CREATE UNIQUE INDEX [IX_Users_FirstName_LastName]
    ON [Users] ([FirstName], [LastName])
    WHERE [FirstName] IS NOT NULL AND [LastName] IS NOT NULL;
```

Документация отслеживается по проблеме [#2407](https://github.com/dotnet/EntityFramework.Docs/issues/2407).

### <a name="improved-query-translation-exceptions"></a>Улучшенные исключения преобразования запросов

Мы продолжаем улучшать сообщения об исключениях, создаваемые при сбое преобразования запроса. Например, в этом запросе используется несопоставленное свойство `IsSigned`:

```csharp
var artists = context.Artists.Where(e => e.IsSigned).ToList();
```

EF Core выдаст следующее исключение, указывающее на сбой преобразования из-за несопоставленного `IsSigned`:

```exception
Unhandled exception. System.InvalidOperationException: The LINQ expression 'DbSet<Artist>()
   .Where(a => a.IsSigned)' could not be translated. Additional information: Translation of member 'IsSigned' on entity type 'Artist' failed. Possibly the specified member is not mapped. Either rewrite the query in a form that can be translated, or switch to client evaluation explicitly by inserting a call to either AsEnumerable(), AsAsyncEnumerable(), ToList(), or ToListAsync(). See <https://go.microsoft.com/fwlink/?linkid=2101038> for more information.
```

Аналогично, при попытке преобразовать сравнения строк с семантикой, зависящей от языка и региональных параметров, теперь создаются более понятные сообщения об исключениях. Например, этот запрос пытается использовать `StringComparison.CurrentCulture`:

```csharp
var artists = context.Artists
    .Where(e => e.Name.Equals("The Unicorns", StringComparison.CurrentCulture))
    .ToList();
```

Теперь EF Core выдаст следующее исключение:

```exception
Unhandled exception. System.InvalidOperationException: The LINQ expression 'DbSet<Artist>()
     .Where(a => a.Name.Equals(
         value: "The Unicorns",
         comparisonType: CurrentCulture))' could not be translated. Additional information: Translation of 'string.Equals' method which takes 'StringComparison' argument is not supported. See <https://go.microsoft.com/fwlink/?linkid=2129535> for more information. Either rewrite the query in a form that can be translated, or switch to client evaluation explicitly by inserting a call to either AsEnumerable(), AsAsyncEnumerable(), ToList(), or ToListAsync(). See <https://go.microsoft.com/fwlink/?linkid=2101038> for more information.
```

### <a name="specify-transaction-id"></a>Указание идентификатора транзакции

Эту функцию предоставил участник сообщества [@Marusyk](https://github.com/Marusyk). Большое спасибо за публикацию!

EF Core предоставляет идентификатор транзакции для корреляции транзакций между вызовами. Этот идентификатор обычно задается в EF Core при запуске транзакции. Если транзакцию запускает приложение, эта функция позволяет приложению явно задать идентификатор транзакции для его правильной корреляции везде, где она используется. Пример:

```csharp
using (context.Database.UseTransaction(myTransaction, myId))
{
   ...
}
```

### <a name="ipaddress-mapping"></a>Сопоставление IPAddress

Эту функцию предоставил участник сообщества [@ralmsdeveloper](https://github.com/ralmsdeveloper). Большое спасибо за публикацию!

Стандартный класс [IPAddress](/dotnet/api/system.net.ipaddress) .NET теперь автоматически сопоставляется со строковым столбцом для баз данных, у которых пока нет собственной поддержки. Например, рассмотрим сопоставление этого типа сущности:

```csharp
public class Host
{
    public int Id { get; set; }
    public IPAddress Address { get; set; }
}
```

В SQL Server функция миграции создаст следующую таблицу:

```sql
CREATE TABLE [Host] (
    [Id] int NOT NULL,
    [Address] nvarchar(45) NULL,
    CONSTRAINT [PK_Host] PRIMARY KEY ([Id]));
```

Затем можно добавить сущности обычным образом:

```csharp
context.AddRange(
    new Host { Address = IPAddress.Parse("127.0.0.1")},
    new Host { Address = IPAddress.Parse("0000:0000:0000:0000:0000:0000:0000:0001")});
```

Результирующий SQL вставит нормализованный IPv4- или IPv6-адрес:

```sql
Executed DbCommand (14ms) [Parameters=[@p0='1', @p1='127.0.0.1' (Size = 45), @p2='2', @p3='::1' (Size = 45)], CommandType='Text', CommandTimeout='30']
      SET NOCOUNT ON;
      INSERT INTO [Host] ([Id], [Address])
      VALUES (@p0, @p1), (@p2, @p3);
```

### <a name="exclude-onconfiguring-when-scaffolding"></a>Исключение OnConfiguring при формировании шаблонов

При формировании DbContext из существующей базы данных EF Core по умолчанию создает перегрузку OnConfiguring со строкой подключения для немедленного использования контекста. Однако это бесполезно, если у вас уже есть разделяемый класс с OnConfiguring, или если вы настраиваете контекст другим способом.

Теперь командам формирования шаблонов можно указать пропустить создание OnConfiguring. Пример:

```console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer --no-onconfiguring
```

Или в консоли диспетчера пакетов:

```console
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer -NoOnConfiguring
```

Обратите внимание, что рекомендуется использовать [именованную строку подключения и защищенное хранилище, такое как секреты пользователей](xref:core/managing-schemas/scaffolding#configuration-and-user-secrets).

### <a name="translations-for-firstordefault-on-strings"></a>Преобразования для FirstOrDefault в строках

Эту функцию предоставил участник сообщества [@dvoreckyaa](https://github.com/dvoreckyaa). Большое спасибо за публикацию!

FirstOrDefault и аналогичные операторы для символов в строках теперь можно преобразовывать. Например, следующий LINQ-запрос:

```csharp
context.Customers.Where(c => c.ContactName.FirstOrDefault() == 'A').ToList();
```

будет преобразован в следующий SQL при использовании SQL Server:

```sql
SELECT [c].[Id], [c].[ContactName]
FROM [Customer] AS [c]
WHERE SUBSTRING([c].[ContactName], 1, 1) = N'A'
```

### <a name="simplify-case-blocks"></a>Упрощение блоков case

Теперь EF Core создает улучшенные запросы с блоками case. Например, следующий LINQ-запрос:

```csharp
context.Weapons
    .OrderBy(w => w.Name.CompareTo("Marcus' Lancer") == 0)
    .ThenBy(w => w.Id)
```

раньше преобразовывался в SQL Server в:

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

Но теперь преобразуется в:

```sql
SELECT [w].[Id], [w].[AmmunitionType], [w].[IsAutomatic], [w].[Name], [w].[OwnerFullName], [w].[SynergyWithId]
FROM [Weapons] AS [w]
ORDER BY CASE
    WHEN ([w].[Name] = N'Marcus'' Lancer') AND [w].[Name] IS NOT NULL THEN CAST(1 AS bit)
    ELSE CAST(0 AS bit)
END, [w].[Id]");
```

## <a name="preview-5"></a>Предварительная версия 5

### <a name="database-collations"></a>Параметры сортировки баз данных

Модель EF теперь позволяет указывать параметры сортировки по умолчанию для базы данных. Это будет передаваться в создаваемые миграции для задания параметров сортировки при создании базы данных. Пример:

```csharp
modelBuilder.UseCollation("German_PhoneBook_CI_AS");
```

Затем функция миграции формирует следующий код для создания базы данных в SQL Server:

```sql
CREATE DATABASE [Test]
COLLATE German_PhoneBook_CI_AS;
```

Можно также указать параметры сортировки, которые будут использоваться для конкретных столбцов. Пример:

```csharp
modelBuilder
    .Entity<User>()
    .Property(e => e.Name)
    .UseCollation("German_PhoneBook_CI_AS");
```

Если функция миграции не используется, параметры сортировки реконструируются из базы данных при формировании шаблонов DbContext.

Наконец, `EF.Functions.Collate()` позволяет использовать различные параметры сортировки для нерегламентированных запросов. Пример:

```csharp
context.Users.Single(e => EF.Functions.Collate(e.Name, "French_CI_AS") == "Jean-Michel Jarre");
```

Будет создан следующий запрос для SQL Server:

```sql
SELECT TOP(2) [u].[Id], [u].[Name]
FROM [Users] AS [u]
WHERE [u].[Name] COLLATE French_CI_AS = N'Jean-Michel Jarre'
```

Обратите внимание, что нерегламентированные параметры сортировки следует использовать с осторожностью, так как они могут негативно повлиять на производительность базы данных.

Это документируется в проблеме [#2273](https://github.com/dotnet/EntityFramework.Docs/issues/2273).

### <a name="flow-arguments-into-idesigntimedbcontextfactory"></a>Передача аргументов в интерфейс IDesignTimeDbContextFactory

Теперь можно передавать аргументы из командной строки в метод `CreateDbContext` интерфейса [IDesignTimeDbContextFactory](/dotnet/api/microsoft.entityframeworkcore.design.idesigntimedbcontextfactory-1). Например, чтобы указать, что используется сборка для разработки, можно передать в командной строке настраиваемый аргумент, например `dev`:

```console
dotnet ef migrations add two --verbose --dev
```

Затем этот аргумент будет передан в фабрику, где его можно использовать для управления созданием и инициализацией контекста. Пример:

```csharp
public class MyDbContextFactory : IDesignTimeDbContextFactory<SomeDbContext>
{
    public SomeDbContext CreateDbContext(string[] args)
        => new SomeDbContext(args.Contains("--dev"));
}
```

Это документируется в проблеме [#2419](https://github.com/dotnet/EntityFramework.Docs/issues/2419).

### <a name="no-tracking-queries-with-identity-resolution"></a>Запросы без отслеживания с разрешением идентификаторов

Теперь можно настроить запросы без отслеживания на выполнение разрешения идентификаторов. Например, следующий запрос будет создавать новый экземпляр Blog для каждого объекта Post, даже если у каждого Blog один и тот же первичный ключ.

```csharp
context.Posts.AsNoTracking().Include(e => e.Blog).ToList();
```

Однако за счет того, что этот запрос обычно выполняется немного медленнее и всегда использует больше памяти, его можно изменить так, чтобы создавался только один экземпляр Blog:

```csharp
context.Posts.AsNoTracking().PerformIdentityResolution().Include(e => e.Blog).ToList();
```

Обратите внимание, что этот вариант подходит только для запросов без отслеживания, так как все запросы с отслеживанием уже действуют таким образом. Кроме того, после проверки API будет изменен синтаксис `PerformIdentityResolution`. См. [#19877](https://github.com/dotnet/efcore/issues/19877#issuecomment-637371073).

Это документируется в проблеме [#1895](https://github.com/dotnet/EntityFramework.Docs/issues/1895).

### <a name="stored-persisted-computed-columns"></a>Сохраняемые вычисляемые столбцы

Большинство баз данных позволяют сохранять значения вычисляемых столбцов после вычислений. Хотя для этого требуется место на диске, вычисляемый столбец вычисляется только один раз при обновлении, а не при каждом получении его значения. Это также позволяет индексировать столбец для некоторых баз данных.

EF Core 5.0 поддерживает настройку вычисляемых столбцов как сохраняемых. Пример:

```csharp
modelBuilder
    .Entity<User>()
    .Property(e => e.SomethingComputed)
    .HasComputedColumnSql("my sql", stored: true);
```

### <a name="sqlite-computed-columns"></a>Вычисляемые столбцы SQLite

EF Core теперь поддерживает вычисляемые столбцы в базах данных SQLite.

## <a name="preview-4"></a>Предварительная версия 4

### <a name="configure-database-precisionscale-in-model"></a>Настройка точности и масштаба для базы данных в модели

Теперь можно указать точность и масштаб для свойства с помощью построителя модели. Пример:

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(b => b.Numeric)
    .HasPrecision(16, 4);
```

Точность и масштаб по-прежнему можно задать, используя полный тип базы данных, например "decimal(16,4)".

Документация отслеживается по проблеме [#527](https://github.com/dotnet/EntityFramework.Docs/issues/527).

### <a name="specify-sql-server-index-fill-factor"></a>Указание коэффициента заполнения индекса SQL Server

Теперь при создании индекса в SQL Server можно указать коэффициент заполнения. Пример:

```csharp
modelBuilder
    .Entity<Customer>()
    .HasIndex(e => e.Name)
    .HasFillFactor(90);
```

## <a name="preview-3"></a>Предварительная версия 3

### <a name="filtered-include"></a>Включение с фильтрацией

Метод Include теперь поддерживает фильтрацию включенных сущностей. Пример:

```csharp
var blogs = context.Blogs
    .Include(e => e.Posts.Where(p => p.Title.Contains("Cheese")))
    .ToList();
```

Этот запрос возвращает блоги со всеми связанными записями, но только в том случае, если заголовок записи содержит Cheese.

Для сокращения числа включаемых сущностей также можно использовать методы Skip и Take. Пример:

```csharp
var blogs = context.Blogs
    .Include(e => e.Posts.OrderByDescending(post => post.Title).Take(5)))
    .ToList();
```

Этот запрос возвратит блоги, включив не более пяти записей для каждого блога.

Подробные сведения см. в [документации по методу Include](xref:core/querying/related-data#filtered-include).

### <a name="new-modelbuilder-api-for-navigation-properties"></a>Новый API ModelBuilder для свойств навигации

Свойства навигации главным образом настраиваются при [определении связей](xref:core/modeling/relationships). Однако новый метод `Navigation` можно использовать в случаях, когда свойства навигации требуют дополнительной настройки. Например, чтобы задать резервное поле для навигации, если не удалось найти поле по соглашению:

```csharp
modelBuilder.Entity<Blog>().Navigation(e => e.Posts).HasField("_myposts");
```

Обратите внимание, что API `Navigation` не является заменой для конфигурации связи. Вместо этого он допускает дополнительную настройку свойств навигации в уже обнаруженных или определенных связях.

См. [документацию по настройке свойств навигации](xref:core/modeling/relationships#configuring-navigation-properties).

### <a name="new-command-line-parameters-for-namespaces-and-connection-strings"></a>Новые параметры командной строки для пространств имен и строк подключения

Теперь можно указывать пространства имен в командной строке во время миграции и формирования шаблонов. Это можно сделать, например, затем, чтобы реконструировать базу данных, поместив в нее классы контекста и модели в различных пространствах имен:

```dotnetcli
dotnet ef dbcontext scaffold "connection string" Microsoft.EntityFrameworkCore.SqlServer --context-namespace "My.Context" --namespace "My.Model"
```

Подробные сведения см. в документации по [миграции](xref:core/managing-schemas/migrations/index#namespaces) и [реконструированию](xref:core/managing-schemas/scaffolding#directories-and-namespaces).

---
Кроме того, теперь можно передать строку подключения команде `database-update`.

```dotnetcli
dotnet ef database update --connection "connection string"
```

Подробные сведения см. в [документации по средствам](xref:core/cli/dotnet#dotnet-ef-database-update).

Аналогичные параметры также были добавлены в команды PowerShell, используемые в консоли диспетчера пакетов VS.

### <a name="enabledetailederrors-has-returned"></a>Возвращение EnableDetailedErrors

По соображениям производительности EF не выполняет дополнительные проверки на NULL при считывании значений из базы данных. Это может привести к возникновению исключений при обнаружении непредвиденных значений NULL, для которых будет сложно определить причину.

При использовании `EnableDetailedErrors` будет добавлена дополнительная проверка на NULL для запросов, чтобы облегчить определение причин этих ошибок за счет небольшого снижения производительности.

Пример:

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .EnableDetailedErrors()
        .EnableSensitiveDataLogging() // Often also useful with EnableDetailedErrors
        .UseSqlServer(Your.SqlServerConnectionString);
```

Документация отслеживается по проблеме [#955](https://github.com/dotnet/EntityFramework.Docs/issues/955).

### <a name="cosmos-partition-keys"></a>Ключи секций Cosmos

Теперь можно указать в запросе ключ секции, используемый для данного запроса. Пример:

```csharp
await context.Set<Customer>()
             .WithPartitionKey(myPartitionKey)
             .FirstAsync();
```

Документация отслеживается по проблеме [#2199](https://github.com/dotnet/EntityFramework.Docs/issues/2199).

### <a name="support-for-the-sql-server-datalength-function"></a>Поддержка функции DATALENGTH SQL Server

К ней можно обратиться с помощью нового метода `EF.Functions.DataLength`. Пример:

```csharp
var count = context.Orders.Count(c => 100 < EF.Functions.DataLength(c.OrderDate));
```

## <a name="preview-2"></a>Предварительная версия 2

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a>Использование атрибута C# для указания резервного поля свойств

Теперь атрибут C# можно использовать для указания резервного поля для свойства. Этот атрибут позволяет EF Core в обычном порядке выполнять запись и чтение из резервного поля, даже если резервное поле не может быть найдено автоматически. Пример:

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

Документация отслеживается по проблеме [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).

### <a name="complete-discriminator-mapping"></a>Завершение сопоставления дискриминатора

EF Core использует столбец дискриминатора для [сопоставления TPH в иерархии наследования](xref:core/modeling/inheritance). Некоторые улучшения производительности будут поддерживаться до тех пор, пока EF Core будут известны все возможные значения для дискриминатора. Теперь эти улучшения реализованы в EF Core 5.0.

Например, в предыдущих версиях EF Core всегда создавался следующий код SQL для запроса, возвращающего все типы в иерархии:

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
WHERE [a].[Discriminator] IN (N'Animal', N'Cat', N'Dog', N'Human')
```

Теперь при настройке полного сопоставления дискриминатора EF Core 5.0 будет формировать следующее:

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
```

Начиная с предварительной версии 3 это поведение будет доступно по умолчанию.

### <a name="performance-improvements-in-microsoftdatasqlite"></a>Улучшения производительности в Microsoft.Data.Sqlite

В производительность SQLIte были внесены два улучшения:

* Теперь получение двоичных и строковых данных с помощью GetBytes, GetChars и GetTextReader осуществляется более эффективно за счет использования SqliteBlob и потоков.
* Инициализация SqliteConnection выполняется в отложенном режиме.

Эти улучшения добавлены в поставщик Microsoft.Data.Sqlite ADO.NET и, следовательно, позволяют повысить производительность за пределами EF Core.

## <a name="preview-1"></a>Предварительная версия 1

### <a name="simple-logging"></a>Простое ведение журнала

Эта функция добавляет возможность, аналогичную `Database.Log` в EF6. Это значит, что она позволяет легко получать журналы из EF Core без настройки какой-либо внешней платформы ведения журналов.

Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 5 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

Дополнительная документация отслеживается по проблеме [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).

### <a name="simple-way-to-get-generated-sql"></a>Простой способ получения созданного кода SQL

В EF Core 5.0 представлен метод расширения `ToQueryString`, который будет возвращать код SQL, создаваемый в EF Core при выполнении запроса LINQ.

Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 9 января 2020 г](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).

Дополнительная документация отслеживается по проблеме [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a>Использование атрибута C# для указания того, что сущность не имеет ключа

Теперь можно настроить тип сущности, не имеющей ключа, используя новый атрибут `KeylessAttribute`. Пример:

```csharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

Документация отслеживается по проблеме [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a>Возможность изменения подключения или строки подключения в инициализированном DbContext

Теперь создание экземпляра DbContext без подключения или строки подключения стало еще проще. Кроме того, подключение или строку подключения можно изменить в экземпляре контекста. Эта возможность позволяет одному и тому же экземпляру контекста динамически подключаться к разным базам данных.

Документация отслеживается по проблеме [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).

### <a name="change-tracking-proxies"></a>Прокси-серверы отслеживания изменений

Теперь EF Core может создавать прокси-серверы среды выполнения, которые автоматически реализуют интерфейсы [INotifyPropertyChanging](/dotnet/api/system.componentmodel.inotifypropertychanging) и [INotifyPropertyChanged](/dotnet/api/system.componentmodel.inotifypropertychanged). Затем они сообщают об изменениях значений свойств сущности напрямую в EF Core, что позволяет не выполнять проверку на наличие изменений. Однако прокси-серверы имеют собственный набор ограничений, поэтому они предназначены не для всех.

Документация отслеживается по проблеме [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).

### <a name="enhanced-debug-views"></a>Улучшенные представления отладки

В представлениях отладки удобно просматривать внутренние компоненты EF Core при отладке. Представление отладки для модели было реализовано некоторое время назад. В EF Core 5.0 представление модели стало более удобочитаемым. Кроме того, добавлено новое представление отладки для отслеживаемых сущностей в диспетчере состояний.

Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 12 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).

Дополнительная документация отслеживается по проблеме [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).

### <a name="improved-handling-of-database-null-semantics"></a>Улучшенная обработка семантики значений NULL в базе данных

Реляционные базы данных обычно обрабатывают NULL как неизвестное значение и, следовательно, как не равное любому другому значению NULL. C# рассматривает NULL как определенное значение, которое аналогично любому другому значению NULL. EF Core по умолчанию преобразует запросы так, чтобы они использовали семантику значений NULL C#. В EF Core 5.0 эффективность таких преобразований значительно повышена.

Документация отслеживается по проблеме [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).

### <a name="indexer-properties"></a>Свойства индексатора

EF Core 5.0 поддерживает сопоставление свойств индексатора C#. Эти свойства позволяют сущностям выступать в качестве контейнеров свойств, в которых столбцы сопоставляются с именованными свойствами в контейнере.

Документация отслеживается по проблеме [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).

### <a name="generation-of-check-constraints-for-enum-mappings"></a>Создание ограничений CHECK для сопоставлений перечислений

Функция миграции в EF Core 5.0 теперь может создавать ограничения CHECK для сопоставлений свойств перечисления. Пример:

```sql
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

Документация отслеживается по проблеме [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).

### <a name="isrelational"></a>Является реляционным

В дополнение к существующим методам `IsSqlServer`, `IsSqlite` и `IsInMemory` был добавлен новый метод `IsRelational`. Этот метод можно использовать для проверки того, использует ли DbContext какой-либо поставщик реляционной базы данных. Пример:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

Документация отслеживается по проблеме [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).

### <a name="cosmos-optimistic-concurrency-with-etags"></a>Оптимистический параллелизм Cosmos с тегами ETag

Поставщик базы данных Azure Cosmos DB теперь поддерживает оптимистичный параллелизм с использованием тегов ETag. Используйте построитель моделей в OnModelCreating для настройки ETag:

```csharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

Затем SaveChanges генерирует исключение `DbUpdateConcurrencyException` при конфликте параллелизма, которое [можно обработать](xref:core/saving/concurrency) для реализации повторных попыток и т. д.

Документация отслеживается по проблеме [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).

### <a name="query-translations-for-more-datetime-constructs"></a>Преобразования запросов для дополнительных конструкций DateTime

Теперь запросы, содержащие новую конструкцию DateTime, поддерживают преобразование.

Кроме того, теперь сопоставлены следующие функции SQL Server:

* DateDiffWeek
* DateFromParts

Пример:

```csharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translations-for-more-byte-array-constructs"></a>Преобразования запросов для дополнительных конструкций массивов байтов

Запросы, использующие свойства Contains, Length, SequenceEqual и т. д. в массиве byte[], преобразуются в SQL.

Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 5 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

Дополнительная документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-reverse"></a>Преобразование запросов, использующих Reverse

Теперь можно преобразовывать запросы, использующие `Reverse`. Пример:

```csharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-bitwise-operators"></a>Преобразование запросов для побитовых операторов

Запросы, использующие побитовые операторы, теперь преобразуются в ряде дополнительных случаев, например:

```csharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-strings-on-cosmos"></a>Преобразование запросов для строк в Cosmos

Запросы, использующие строковые методы Contains, StartsWith и EndsWith, теперь преобразуются при использовании поставщика Azure Cosmos DB.

Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).
