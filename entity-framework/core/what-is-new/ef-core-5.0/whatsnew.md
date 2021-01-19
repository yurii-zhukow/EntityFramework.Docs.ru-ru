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
# <a name="whats-new-in-ef-core-50"></a>Новые возможности EF Core 5.0

В приведенном ниже списке указаны ключевые новые функции EF Core 5.0. Полный перечень проблем в этом выпуске см. в [средстве отслеживания проблем](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A5.0.0).

EF Core 5.0 — это основной выпуск. Он содержит несколько [критических изменений](xref:core/what-is-new/ef-core-5.0/breaking-changes), касающихся улучшений API и поведения. Такие изменения могут негативно повлиять на работу существующих приложений.

## <a name="many-to-many"></a>"Много ко многим"

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

По соглашению EF Core 5.0 распознает их как связь "многие ко многим" и автоматически создает в базе данных таблицу соединения `PostTag`. Данные могут запрашиваться и обновляться без явных ссылок на таблицу соединения, что значительно упрощает код. При необходимости таблицу соединений по-прежнему можно настроить для явного выполнения запросов.

Дополнительные сведения см. в [полной документации по связи "многие ко многим"](xref:core/modeling/relationships#many-to-many).

## <a name="split-queries"></a>Разделенные запросы

Начиная с EF Core 3.0, EF Core всегда создает один SQL-запрос для каждого LINQ-запроса. Это обеспечивает согласованность данных, возвращаемых в рамках ограничений используемого режима транзакций. Однако если запрос использует `Include` или проекцию для возврата нескольких связанных коллекций, это может привести к снижению производительности.

Теперь EF Core 5.0 позволяет разделять один LINQ-запрос, включая связанные коллекции, на несколько SQL-запросов. Это может значительно повысить производительность, но может также привести к несогласованности возвращаемых результатов в случае изменения данных между двумя запросами. Для устранения этой проблемы и обеспечения согласованности с разделенными запросами можно использовать сериализуемые транзакции или транзакции с моментальными снимками, но это может привести к дополнительным затратам на производительность и вызвать различия в поведении.

Рассмотрим запрос, который извлекает два уровня связанных коллекций с помощью `Include`:

```csharp
var artists = context.Artists
    .Include(e => e.Albums)
    .ToList();
```

Если используется поставщик SQLite, EF Core по умолчанию создаст следующий SQL:

```sql
SELECT a."Id", a."Name", a0."Id", a0."ArtistId", a0."Title"
FROM "Artists" AS a
LEFT JOIN "Album" AS a0 ON a."Id" = a0."ArtistId"
ORDER BY a."Id", a0."Id"
```

При использовании разделенных запросов вместо этого создается следующий код SQL.

```sql
SELECT a."Id", a."Name"
FROM "Artists" AS a
ORDER BY a."Id"

SELECT a0."Id", a0."ArtistId", a0."Title", a."Id"
FROM "Artists" AS a
INNER JOIN "Album" AS a0 ON a."Id" = a0."ArtistId"
ORDER BY a."Id"
```

Разделение запросов можно включить, разместив новый оператор `AsSplitQuery` в любом месте запроса LINQ или глобально в модели `OnConfiguring`. Дополнительные сведения см. в [полной документации по разделенным запросам](xref:core/querying/single-split-queries).

## <a name="simple-logging-and-improved-diagnostics"></a>Простое ведение журнала и улучшенная диагностика

В EF Core 5.0 появился простой способ настройки ведения журнала с помощью нового метода `LogTo`. Следующий код задает вывод в консоль сообщений журнала, включая весь код SQL, создаваемый EF Core.

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder.LogTo(Console.WriteLine);
```

Кроме того, теперь можно вызвать метод `ToQueryString` для любого запроса LINQ, извлекая код SQL, который будет выполнен запросом.

```csharp
Console.WriteLine(
    ctx.Artists
    .Where(a => a.Name == "Pink Floyd")
    .ToQueryString());
```

Наконец, различные типы EF Core были дополнены расширенным свойством `DebugView`, которое предоставляет подробные сведения о внутренних компонентах. Например, можно обратиться к <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DebugView%2A?displayProperty=nameWithType>, чтобы узнать, какие сущности отслеживаются в данный момент времени.

Дополнительные сведения см. [в документации по ведению журнала и перехватам](xref:core/logging-events-diagnostics/index).

## <a name="filtered-include"></a>Включение с фильтрацией

Метод `Include` теперь поддерживает фильтрацию включенных сущностей.

```csharp
var blogs = context.Blogs
    .Include(e => e.Posts.Where(p => p.Title.Contains("Cheese")))
    .ToList();
```

Этот запрос возвращает блоги со всеми связанными записями, но только в том случае, если заголовок записи содержит Cheese.

Дополнительные сведения см. в [полной документации по разделенным запросам](xref:core/querying/related-data/eager#filtered-include).

## <a name="table-per-type-tpt-mapping"></a>Сопоставление типа "одна таблица на тип" (TPT)

По умолчанию в EF Core иерархия наследования типов .NET сопоставляется с одной таблицей базы данных. Это называется сопоставлением "одна таблица на иерархию" (TPH). В EF Core 5.0 каждый тип .NET в иерархии наследования также может сопоставляться с отдельной таблицей базы данных. Это называется сопоставлением "одна таблица на тип" (TPT).

Например, возьмем следующую модель с сопоставленной иерархией.

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

При использовании сопоставления "одна таблица на тип" (TPT) для каждого типа в иерархии создается таблица базы данных.

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

Дополнительные сведения см. в [полной документации по сопоставлению "одна таблица на тип" (TPT)](xref:core/modeling/inheritance).

## <a name="flexible-entity-mapping"></a>Гибкое сопоставление сущностей

Типы сущностей обычно сопоставляются с таблицами или представлениями так, что EF Core будет извлекать содержимое таблицы или представления при запросе соответствующего типа. EF Core 5.0 добавляет дополнительные варианты сопоставления, в которых сущность может быть сопоставлена с SQL-запросом (называемым определяющим запросом) или с функцией с табличным значением.

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

Функции с табличным значением также могут сопоставляться с методом .NET, а не с DbSet, что позволяет передавать параметры. При этом сопоставление можно настроить с <xref:Microsoft.EntityFrameworkCore.RelationalModelBuilderExtensions.HasDbFunction%2A>.

Наконец, теперь можно сопоставлять сущность с представлением при запросе (или с функцией или определяющим запросом) и с таблицей при обновлении.

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder
        .Entity<Blog>()
        .ToTable("Blogs")
        .ToView("BlogsView");
}
```

## <a name="shared-type-entity-types-and-property-bags"></a>Общие типы сущностей и контейнеры свойств

В EF Core 5.0 один тип CLR может быть сопоставлен с несколькими различными типами сущностей, которые называются общими. Хотя с этой функцией можно использовать любой тип CLR, .NET `Dictionary` предлагает полезный вариант применения, который мы называем контейнерами свойств.

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

Впоследствии можно выполнять запросы к этим сущностям и обновлять их так же, как и в случае с обычными типами сущностей с собственным выделенным типом CLR. Дополнительные сведения можно найти в документации по [контейнерам свойств](xref:core/modeling/shadow-properties).

## <a name="required-11-dependents"></a>Обязательные зависимости "один к одному"

В EF Core 3.1 зависимая сторона связи "один к одному" всегда считалась необязательной. Это было особенно очевидно при использовании принадлежащих сущностей, поскольку все их столбцы создавались в базе данных как допускающие значение NULL, даже если они были настроены в соответствии с требованиями модели.

В EF Core 5.0 навигацию к принадлежащей сущности теперь можно настроить как обязательную зависимость. Пример:

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

## <a name="dbcontextfactory"></a>DbContextFactory

В EF Core 5.0 введены методы `AddDbContextFactory` и `AddPooledDbContextFactory` для регистрации фабрики с целью создания экземпляров DbContext в контейнере внедрения зависимостей приложения. Это может быть полезно, если в коде приложения требуется создавать и удалять экземпляры контекста вручную.

```csharp
services.AddDbContextFactory<SomeDbContext>(b =>
    b.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Test"));
```

На этом этапе службы приложений, такие как контроллеры ASP.NET Core, могут добавляться с помощью `IDbContextFactory<TContext>` и использовать его для создания экземпляров контекста.

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

Дополнительные сведения см. в [полной документации по DbContextFactory](xref:core/dbcontext-configuration/index#using-a-dbcontext-factory-eg-for-blazor).

## <a name="sqlite-table-rebuilds"></a>Перестроение таблиц SQLite

По сравнению с другими базами данных таблицы SQLite относительно ограничены возможностями обработки схем. Например, в этой системе не поддерживается удаление столбца из существующей таблицы. EF Core 5.0 позволяет обойти эти ограничения путем автоматического создания новой таблицы, копирования в нее данных из старой таблицы, удаления старой таблицы и переименования новой. Таким образом, таблица перестраивается, в связи с чем к ней можно безопасно применять не поддерживаемые ранее операции миграции.

Дополнительные сведения о том, какие операции миграции теперь поддерживаются за счет перестроения таблиц, см. на [этой странице документации](xref:core/providers/sqlite/limitations#migrations-limitations).

## <a name="database-collations"></a>Параметры сортировки баз данных

В EF Core 5.0 реализована поддержка возможности задавать параметры сортировки текста на уровне базы данных, столбца или запроса. Это позволяет гибким образом определять чувствительность к регистру и другим текстовым аспектам, не снижая при этом эффективность запросов.

Например, следующий параметр настраивает столбец `Name` в SQL Server как чувствительный к регистру символов, в связи с чем все создаваемые для этого столбца индексы будут работать соответствующим образом.

```csharp
modelBuilder
    .Entity<User>()
    .Property(e => e.Name)
    .UseCollation("SQL_Latin1_General_CP1_CS_AS");
```

Дополнительные сведения см. в [полной документации по параметрам сортировки и чувствительности к регистру](xref:core/miscellaneous/collations-and-case-sensitivity).

## <a name="event-counters"></a>Счетчики событий

В EF Core 5.0 предоставляются [счетчики событий](https://devblogs.microsoft.com/dotnet/introducing-diagnostics-improvements-in-net-core-3-0), которые можно использовать для отслеживания производительности приложения и выявления различных аномалий. Просто подключитесь к процессу, в котором выполняется EF, с помощью средства [dotnet-counters](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters).

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

Дополнительные сведения см. в [полной документации по счетчикам событий](xref:core/logging-events-diagnostics/event-counters).

## <a name="other-features"></a>Другие возможности

### <a name="model-building"></a>Построение модели

* Были представлены API построения модели, позволяющие упростить настройку [функций сравнения значений](xref:core/modeling/value-comparers).
* Теперь вычисляемые столбцы можно настроить как [*хранимые* или *виртуальные*](xref:core/modeling/generated-properties#computed-columns).
* Точность и масштаб теперь можно настраивать с помощью [текучего API](xref:core/modeling/entity-properties#precision-and-scale).
* Были представлены новые API-интерфейсы построения модели для [свойств навигации](xref:core/modeling/relationships#configuring-navigation-properties).
* Были представлены новые API-интерфейсы построения модели для полей, аналогичные свойствам.
* Типы .NET [PhysicalAddress](/dotnet/api/system.net.networkinformation.physicaladdress) и [IPAddress](/dotnet/api/system.net.ipaddress) теперь можно сопоставлять со строковыми столбцами базы данных.
* Резервное поле теперь можно настраивать с помощью [нового атрибута `[BackingField]`](xref:core/modeling/backing-field).
* Теперь разрешены резервные поля, допускающие значения NULL. За счет этого обеспечивается лучшая поддержка генерируемых хранилищем значений по умолчанию в тех случаях, когда значение по умолчанию CLR не является хорошим значением-меткой (то есть `bool`).
* С помощью нового атрибута `[Index]` можно указывать в типе сущности индекс вместо использования текучего API.
* С помощью нового атрибута `[Keyless]` можно настраивать тип сущности как [не имеющий ключа](xref:core/modeling/keyless-entity-types).
* По умолчанию [EF Core теперь рассматривает дискриминатор как *полный*](xref:core/modeling/inheritance#table-per-hierarchy-and-discriminator-configuration), то есть не видит значения дискриминатора, которые не настроены приложением в модели. Такое поведение позволяет в определенной степени повысить производительность и может быть отключено, если столбец дискриминатора может содержать неизвестные значения.

### <a name="query"></a>Запрос

* В исключениях ошибок преобразования запросов теперь приводятся более явные причины возникновения сбоя, помогающие выявить проблему.
* Запросы без отслеживания теперь могут выполнять [разрешение идентификаторов](xref:core/querying/tracking#identity-resolution), что позволяет исключить возвращение нескольких экземпляров сущности для одного и того же объекта базы данных.
* Добавлена поддержка GroupBy с условными статистическими выражениями (например, `GroupBy(o => o.OrderDate).Select(g => g.Count(i => i.OrderDate != null))`).
* Добавлена поддержка преобразования оператора Distinct над элементами группы перед выполнением статистического вычисления.
* Преобразование [`Reverse`](/dotnet/api/system.linq.queryable.reverse).
* Оптимизировано связанное с `DateTime` преобразование для SQL Server (например, [`DateDiffWeek`](xref:Microsoft.EntityFrameworkCore.SqlServerDbFunctionsExtensions.DateDiffWeek%2A), [`DateFromParts`](xref:Microsoft.EntityFrameworkCore.SqlServerDbFunctionsExtensions.DateFromParts%2A)).
* Преобразование новых методов для массивов байтов (например, [`Contains`](/dotnet/api/system.linq.enumerable.contains), [`Length`](/dotnet/api/system.array.length), [`SequenceEqual`](/dotnet/api/system.linq.enumerable.sequenceequal)).
* Преобразование некоторых дополнительных побитовых операторов, например дополнения до двух.
* Преобразование `FirstOrDefault` для строк.
* Оптимизировано преобразование запросов, связанных с семантикой значений NULL, что позволяет получить более строгие и эффективные запросы.
* Сопоставленные пользователем функции теперь можно аннотировать для управления распространением значений NULL, что также позволяет получить более строгие и эффективные запросы.
* Значительно сокращен код SQL, содержащий блоки CASE.
* Функцию SQL Server [`DATELENGTH`](https://docs.microsoft.com/sql/t-sql/functions/datalength-transact-sql) теперь можно вызывать в запросах с помощью нового метода [`EF.Functions.DataLength`](xref:Microsoft.EntityFrameworkCore.SqlServerDbFunctionsExtensions.DataLength%2A).
* `EnableDetailedErrors` добавляет [дополнительные сведения в исключения](xref:core/logging-events-diagnostics/simple-logging#detailed-query-exceptions).

### <a name="saving"></a>Сохранение

* [Перехват](xref:core/logging-events-diagnostics/interceptors#savechanges-interception) и [события](xref:core/logging-events-diagnostics/events) SaveChanges.
* Реализованы API-интерфейсы для управления [точками сохранения транзакций](xref:core/saving/transactions#savepoints). Кроме того, EF Core будет автоматически создавать точку сохранения при вызове `SaveChanges`, если транзакция уже выполняется, и выполнять откат к этой точке в случае сбоя.
* Идентификатор транзакции может явно задаваться приложением, что позволяет упростить корреляцию событий транзакций в журнале и других местах.
* Заданный по умолчанию максимальный размер пакета для SQL Server изменен до 42 по результатам анализа производительности пакетной обработки.

### <a name="migrations-and-scaffolding"></a>Миграция и формирование шаблонов

* Таблицы теперь можно [исключать из миграции](xref:core/modeling/entity-types#excluding-from-migrations).
* Новая команда [`dotnet ef migrations list`](xref:core/cli/dotnet#dotnet-ef-migrations-list) показывает, какие миграции еще не были применены к базе данных (команда [`Get-Migration`](xref:core/cli/powershell#get-migration) выполняет те же действия в консоли управления пакетами).
* Скрипты миграции теперь содержат инструкции транзакций, необходимые для более эффективной обработки сбоев приложения миграции.
* Столбцы для несопоставленных базовых классов теперь упорядочиваются после столбцов для сопоставленных типов сущностей. Обратите внимание, что это касается только вновь созданных таблиц: порядок столбцов для существующих таблиц остается неизменным.
* Теперь при создании миграции можно учитывать, является ли создаваемая миграция идемпотентной и будут ли выходные данные выполняться немедленно или создаваться в виде скрипта.
* Добавлены новые параметры командной строки, позволяющие задавать пространства имен для [миграции](xref:core/managing-schemas/migrations/index#namespaces) и [формирования шаблонов](xref:core/managing-schemas/scaffolding#directories-and-namespaces).
* Команда [dotnet ef database update](xref:core/cli/dotnet#dotnet-ef-database-update) теперь принимает новый параметр `--connection`, позволяющий задать строку подключения.
* При формировании шаблонов для существующих баз данных имена таблиц преобразуются в форму единственного числа, поэтому таблицы с именами `People` и `Addresses` будут формироваться по типам сущностей с именами `Person` и `Address`. [Исходные имена базы данных по-прежнему можно сохранить](xref:core/managing-schemas/scaffolding#preserving-names).
* Новый параметр [`--no-onconfiguring`](xref:core/cli/dotnet#dotnet-ef-dbcontext-scaffold) может указывать EF Core на необходимость исключить `OnModelConfiguring` при формировании шаблона модели.

### <a name="cosmos"></a>Cosmos

* Расширены [параметры подключения Cosmos](xref:core/providers/cosmos/index#cosmos-options).
* Теперь оптимистическая блокировка [поддерживается в Cosmos с помощью тегов ETag](xref:core/providers/cosmos/index#optimistic-concurrency-with-etags).
* Новый метод `WithPartitionKey` позволяет включать в модель и в запросы [ключ секции](xref:core/providers/cosmos/index#partition-keys) Cosmos.
* Строковые методы [`Contains`](/dotnet/api/system.string.contains), [`StartsWith`](/dotnet/api/system.string.startswith) и [`EndsWith`](/dotnet/api/system.string.endswith) теперь преобразованы для Cosmos.
* Оператор C# `is` теперь преобразован для Cosmos.

### <a name="sqlite"></a>Sqlite

* Теперь поддерживаются вычисляемые столбцы.
* Теперь получение двоичных и строковых данных с помощью GetBytes, GetChars и GetTextReader осуществляется более эффективно за счет использования SqliteBlob и потоков.
* Инициализация SqliteConnection выполняется в отложенном режиме.

### <a name="other"></a>Другие

* Теперь можно создавать прокси-объекты отслеживания изменений, которые автоматически реализуют интерфейсы [INotifyPropertyChanging](/dotnet/api/system.componentmodel.inotifypropertychanging) и [INotifyPropertyChanged](/dotnet/api/system.componentmodel.inotifypropertychanged). Таким образом обеспечивается альтернативный подход к отслеживанию изменений, при котором не требуется проверка на наличие изменений при вызове `SaveChanges`.
* Теперь возможно изменение <xref:System.Data.Common.DbConnection> или строки подключения в уже инициализированном экземпляре DbContext.
* Новый метод <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Clear%2A?displayProperty=nameWithType>.0#Microsoft_EntityFrameworkCore_ChangeTracking_ChangeTracker_Clear) очищает экземпляр DbContext от всех отслеживаемых сущностей. Обычно это не требуется, если выполняется рекомендация по созданию нового экземпляра контекста с кратким сроком существования для каждой единицы работы. Однако если необходимо сбросить состояние экземпляра DbContext, то использование нового метода `Clear()` является более эффективным и надежным подходом, чем массовое отсоединение всех сущностей.
* Программы командной строки EF Core теперь автоматически задают для переменных среды `ASPNETCORE_ENVIRONMENT` _и_ `DOTNET_ENVIRONMENT` значение Development. Это обеспечивает согласованную работу при использовании универсального узла и ASP.NET Core во время разработки.
* Пользовательские аргументы командной строки могут передаваться в <xref:Microsoft.EntityFrameworkCore.Design.IDesignTimeDbContextFactory%601>, что позволяет приложениям управлять способами создания и инициализации контекста.
* Теперь [в SQL Server можно настроить](xref:core/providers/sql-server/indexes#fill-factor) коэффициент заполнения индекса.
* Реализовано новое свойство <xref:Microsoft.EntityFrameworkCore.RelationalDatabaseFacadeExtensions.IsRelational%2A>, которое позволяет различать случаи использования реляционного и нереляционного поставщика (например, InMemory).
