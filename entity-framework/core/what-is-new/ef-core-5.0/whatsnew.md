---
title: Новые возможности EF Core 5.0
description: Обзор новых возможностей в EF Core 5.0
author: ajcvickers
ms.date: 07/20/2020
uid: core/what-is-new/ef-core-5.0/whatsnew
ms.openlocfilehash: d42b2811d07516e9febedbc51fcb206000d38371
ms.sourcegitcommit: 51148929e3889c48227d96c95c4e310d53a3d2c9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/21/2020
ms.locfileid: "86873387"
---
# <a name="whats-new-in-ef-core-50"></a>Новые возможности EF Core 5.0

Сейчас EF Core 5.0 находится на стадии разработки. На этой странице представлен обзор интересных изменений, появившихся в каждой предварительной версии.

Эта страница не является аналогом статьи о [планировании для EF Core 5.0](xref:core/what-is-new/ef-core-5.0/plan). В плане описываются общие темы для EF Core 5.0, в том числе все, что планируется включить перед выпуском окончательной версии.

Мы будем добавлять ссылки на официальную документацию по мере ее публикации.

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
public void DoSomehing()
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

EF Core теперь поддерживает [точки сохранения](/SQL/t-sql/language-elements/save-transaction-transact-sql?view=sql-server-ver15#remarks) для лучшего контроля над транзакциями, выполняющими несколько операций.

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

```CSharp
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

```CSharp
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

```CSharp
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

```CSharp
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

```CSharp
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

```CSharp
var artists = context.Artists.Where(e => e.IsSigned).ToList();
```

EF Core выдаст следующее исключение, указывающее на сбой преобразования из-за несопоставленного `IsSigned`:

> Необработанное исключение. "System.InvalidOperationException: не удалось преобразовать выражение LINQ "DbSet<Artist>() .Where(a => a.IsSigned)". Дополнительные сведения: не удалось преобразовать член "IsSigned" для типа сущности "Artist". Возможно, указанный элемент не сопоставлен. Перепишите запрос в поддерживающей преобразование форме или явно переключитесь на оценку клиента, вставив вызов либо AsEnumerable(), AsAsyncEnumerable(), ToList(), либо ToListAsync(). Дополнительные сведения см. в разделе https://go.microsoft.com/fwlink/?linkid=2101038.

Аналогично, при попытке преобразовать сравнения строк с семантикой, зависящей от языка и региональных параметров, теперь создаются более понятные сообщения об исключениях. Например, этот запрос пытается использовать `StringComparison.CurrentCulture`:

```CSharp
var artists = context.Artists
    .Where(e => e.Name.Equals("The Unicorns", StringComparison.CurrentCulture))
    .ToList();
```

Теперь EF Core выдаст следующее исключение:

> Необработанное исключение. "System.InvalidOperationException: не удается преобразовать выражение LINQ "DbSet<Artist>() .Where(a => a.Name.Equals( value: "The Unicorns", comparisonType: CurrentCulture))". Дополнительные сведения: преобразование метода "string.Equals", принимающего аргумент "StringComparison", не поддерживается. Дополнительные сведения см. в разделе https://go.microsoft.com/fwlink/?linkid=2129535. Перепишите запрос в поддерживающей преобразование форме или явно переключитесь на оценку клиента, вставив вызов либо AsEnumerable(), AsAsyncEnumerable(), ToList(), либо ToListAsync(). Дополнительные сведения см. в разделе https://go.microsoft.com/fwlink/?linkid=2101038.

### <a name="specify-transaction-id"></a>Указание идентификатора транзакции

Эту функцию предоставил участник сообщества [@Marusyk](https://github.com/Marusyk). Большое спасибо за публикацию!

EF Core предоставляет идентификатор транзакции для корреляции транзакций между вызовами. Этот идентификатор обычно задается в EF Core при запуске транзакции. Если транзакцию запускает приложение, эта функция позволяет приложению явно задать идентификатор транзакции для его правильной корреляции везде, где она используется. Пример:

```CSharp
using (context.Database.UseTransaction(myTransaction, myId))
{
   ...
}
```

### <a name="ipaddress-mapping"></a>Сопоставление IPAddress

Эту функцию предоставил участник сообщества [@ralmsdeveloper](https://github.com/ralmsdeveloper). Большое спасибо за публикацию!

Стандартный класс [IPAddress](/dotnet/api/system.net.ipaddress) .NET теперь автоматически сопоставляется со строковым столбцом для баз данных, у которых пока нет собственной поддержки. Например, рассмотрим сопоставление этого типа сущности:

```CSharp
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

```CSharp
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

```
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer --no-onconfiguring
```

Или в консоли диспетчера пакетов:

```
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer -NoOnConfiguring 
``` 

Обратите внимание, что рекомендуется использовать [именованную строку подключения и защищенное хранилище, такое как секреты пользователей](/core/managing-schemas/scaffolding?tabs=vs#configuration-and-user-secrets).

### <a name="translations-for-firstordefault-on-strings"></a>Преобразования для FirstOrDefault в строках

Эту функцию предоставил участник сообщества [@dvoreckyaa](https://github.com/dvoreckyaa). Большое спасибо за публикацию!

FirstOrDefault и аналогичные операторы для символов в строках теперь можно преобразовывать. Например, следующий LINQ-запрос:

```CSharp
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

```CSharp
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

```CSharp
modelBuilder.UseCollation("German_PhoneBook_CI_AS");
```

Затем функция миграции формирует следующий код для создания базы данных в SQL Server:

```sql
CREATE DATABASE [Test]
COLLATE German_PhoneBook_CI_AS;
```

Можно также указать параметры сортировки, которые будут использоваться для конкретных столбцов. Пример:

```CSharp
 modelBuilder
     .Entity<User>()
     .Property(e => e.Name)
     .UseCollation("German_PhoneBook_CI_AS");
```

Если функция миграции не используется, параметры сортировки реконструируются из базы данных при формировании шаблонов DbContext.

Наконец, `EF.Functions.Collate()` позволяет использовать различные параметры сортировки для нерегламентированных запросов. Пример:

```CSharp
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

Теперь можно передавать аргументы из командной строки в метод `CreateDbContext` интерфейса [IDesignTimeDbContextFactory](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.design.idesigntimedbcontextfactory-1?view=efcore-3.1). Например, чтобы указать, что используется сборка для разработки, можно передать в командной строке настраиваемый аргумент, например `dev`:

```
dotnet ef migrations add two --verbose --dev
``` 

Затем этот аргумент будет передан в фабрику, где его можно использовать для управления созданием и инициализацией контекста. Пример:

```CSharp
public class MyDbContextFactory : IDesignTimeDbContextFactory<SomeDbContext>
{
    public SomeDbContext CreateDbContext(string[] args) 
        => new SomeDbContext(args.Contains("--dev"));
}
```

Это документируется в проблеме [#2419](https://github.com/dotnet/EntityFramework.Docs/issues/2419).

### <a name="no-tracking-queries-with-identity-resolution"></a>Запросы без отслеживания с разрешением идентификаторов

Теперь можно настроить запросы без отслеживания на выполнение разрешения идентификаторов. Например, следующий запрос будет создавать новый экземпляр Blog для каждого объекта Post, даже если у каждого Blog один и тот же первичный ключ. 

```CSharp
context.Posts.AsNoTracking().Include(e => e.Blog).ToList();
```

Однако за счет того, что этот запрос обычно выполняется немного медленнее и всегда использует больше памяти, его можно изменить так, чтобы создавался только один экземпляр Blog:

```CSharp
context.Posts.AsNoTracking().PerformIdentityResolution().Include(e => e.Blog).ToList();
```

Обратите внимание, что этот вариант подходит только для запросов без отслеживания, так как все запросы с отслеживанием уже действуют таким образом. Кроме того, после проверки API будет изменен синтаксис `PerformIdentityResolution`. См. [#19877](https://github.com/dotnet/efcore/issues/19877#issuecomment-637371073).

Это документируется в проблеме [#1895](https://github.com/dotnet/EntityFramework.Docs/issues/1895).

### <a name="stored-persisted-computed-columns"></a>Сохраняемые вычисляемые столбцы

Большинство баз данных позволяют сохранять значения вычисляемых столбцов после вычислений. Хотя для этого требуется место на диске, вычисляемый столбец вычисляется только один раз при обновлении, а не при каждом получении его значения. Это также позволяет индексировать столбец для некоторых баз данных.

EF Core 5.0 поддерживает настройку вычисляемых столбцов как сохраняемых. Пример:
 
```CSharp
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

```CSharp
modelBuilder
    .Entity<Blog>()
    .Property(b => b.Numeric)
    .HasPrecision(16, 4);
```

Точность и масштаб по-прежнему можно задать, используя полный тип базы данных, например "decimal(16,4)". 

Документация отслеживается по проблеме [#527](https://github.com/dotnet/EntityFramework.Docs/issues/527).

### <a name="specify-sql-server-index-fill-factor"></a>Указание коэффициента заполнения индекса SQL Server

Теперь при создании индекса в SQL Server можно указать коэффициент заполнения. Пример:

```CSharp
modelBuilder
    .Entity<Customer>()
    .HasIndex(e => e.Name)
    .HasFillFactor(90);
```

## <a name="preview-3"></a>Предварительная версия 3

### <a name="filtered-include"></a>Включение с фильтрацией

Метод Include теперь поддерживает фильтрацию включенных сущностей. Пример:

```CSharp
var blogs = context.Blogs
    .Include(e => e.Posts.Where(p => p.Title.Contains("Cheese")))
    .ToList();
```

Этот запрос возвращает блоги со всеми связанными записями, но только в том случае, если заголовок записи содержит Cheese.

Для сокращения числа включаемых сущностей также можно использовать методы Skip и Take. Пример:
 
```CSharp
var blogs = context.Blogs
    .Include(e => e.Posts.OrderByDescending(post => post.Title).Take(5)))
    .ToList();
```
Этот запрос возвратит блоги, включив не более пяти записей для каждого блога.

Подробные сведения см. в [документации по методу Include](xref:core/querying/related-data#filtered-include).

### <a name="new-modelbuilder-api-for-navigation-properties"></a>Новый API ModelBuilder для свойств навигации

Свойства навигации главным образом настраиваются при [определении связей](xref:core/modeling/relationships). Однако новый метод `Navigation` можно использовать в случаях, когда свойства навигации требуют дополнительной настройки. Например, чтобы задать резервное поле для навигации, если не удалось найти поле по соглашению:

```CSharp
modelBuilder.Entity<Blog>().Navigation(e => e.Posts).HasField("_myposts");
```

Обратите внимание, что API `Navigation` не является заменой для конфигурации связи. Вместо этого он допускает дополнительную настройку свойств навигации в уже обнаруженных или определенных связях.

См. [документацию по настройке свойств навигации](xref:core/modeling/relationships#configuring-navigation-properties).

### <a name="new-command-line-parameters-for-namespaces-and-connection-strings"></a>Новые параметры командной строки для пространств имен и строк подключения 

Теперь можно указывать пространства имен в командной строке во время миграции и формирования шаблонов. Это можно сделать, например, затем, чтобы реконструировать базу данных, поместив в нее классы контекста и модели в различных пространствах имен: 

```
dotnet ef dbcontext scaffold "connection string" Microsoft.EntityFrameworkCore.SqlServer --context-namespace "My.Context" --namespace "My.Model"
```

Подробные сведения см. в документации по [миграции](xref:core/managing-schemas/migrations/index#namespaces) и [реконструированию](xref:core/managing-schemas/scaffolding#directories-and-namespaces).

---
Кроме того, теперь можно передать строку подключения команде `database-update`.

```
dotnet ef database update --connection "connection string"
```

Подробные сведения см. в [документации по средствам](xref:core/miscellaneous/cli/dotnet#dotnet-ef-database-update).

Аналогичные параметры также были добавлены в команды PowerShell, используемые в консоли диспетчера пакетов VS.

### <a name="enabledetailederrors-has-returned"></a>Возвращение EnableDetailedErrors

По соображениям производительности EF не выполняет дополнительные проверки на NULL при считывании значений из базы данных. Это может привести к возникновению исключений при обнаружении непредвиденных значений NULL, для которых будет сложно определить причину.

При использовании `EnableDetailedErrors` будет добавлена дополнительная проверка на NULL для запросов, чтобы облегчить определение причин этих ошибок за счет небольшого снижения производительности.  

Пример:
```CSharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .EnableDetailedErrors()
        .EnableSensitiveDataLogging() // Often also useful with EnableDetailedErrors 
        .UseSqlServer(Your.SqlServerConnectionString);
```

Документация отслеживается по проблеме [#955](https://github.com/dotnet/EntityFramework.Docs/issues/955).

### <a name="cosmos-partition-keys"></a>Ключи секций Cosmos

Теперь можно указать в запросе ключ секции, используемый для данного запроса. Пример:

```CSharp
await context.Set<Customer>()
             .WithPartitionKey(myPartitionKey)
             .FirstAsync();
```

Документация отслеживается по проблеме [#2199](https://github.com/dotnet/EntityFramework.Docs/issues/2199).

### <a name="support-for-the-sql-server-datalength-function"></a>Поддержка функции DATALENGTH SQL Server

К ней можно обратиться с помощью нового метода `EF.Functions.DataLength`. Пример:
```CSharp
var count = context.Orders.Count(c => 100 < EF.Functions.DataLength(c.OrderDate));
``` 

## <a name="preview-2"></a>Предварительная версия 2

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a>Использование атрибута C# для указания резервного поля свойств

Теперь атрибут C# можно использовать для указания резервного поля для свойства. Этот атрибут позволяет EF Core в обычном порядке выполнять запись и чтение из резервного поля, даже если резервное поле не может быть найдено автоматически. Пример:

```CSharp
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

EF Core использует столбец дискриминатора для [сопоставления TPH в иерархии наследования](/ef/core/modeling/inheritance). Некоторые улучшения производительности будут поддерживаться до тех пор, пока EF Core будут известны все возможные значения для дискриминатора. Теперь эти улучшения реализованы в EF Core 5.0.

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

```CSharp
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

Теперь EF Core может создавать прокси-серверы среды выполнения, которые автоматически реализуют интерфейсы [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) и [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1). Затем они сообщают об изменениях значений свойств сущности напрямую в EF Core, что позволяет не выполнять проверку на наличие изменений. Однако прокси-серверы имеют собственный набор ограничений, поэтому они предназначены не для всех.

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

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

Документация отслеживается по проблеме [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).

### <a name="isrelational"></a>Является реляционным

В дополнение к существующим методам `IsSqlServer`, `IsSqlite` и `IsInMemory` был добавлен новый метод `IsRelational`. Этот метод можно использовать для проверки того, использует ли DbContext какой-либо поставщик реляционной базы данных. Пример:

```CSharp
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

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

Затем SaveChanges генерирует исключение `DbUpdateConcurrencyException` при конфликте параллелизма, которое [можно обработать](https://docs.microsoft.com/ef/core/saving/concurrency) для реализации повторных попыток и т. д.

Документация отслеживается по проблеме [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).

### <a name="query-translations-for-more-datetime-constructs"></a>Преобразования запросов для дополнительных конструкций DateTime

Теперь запросы, содержащие новую конструкцию DateTime, поддерживают преобразование.

Кроме того, теперь сопоставлены следующие функции SQL Server:

* DateDiffWeek
* DateFromParts

Пример:

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translations-for-more-byte-array-constructs"></a>Преобразования запросов для дополнительных конструкций массивов байтов

Запросы, использующие свойства Contains, Length, SequenceEqual и т. д. в массиве byte[], преобразуются в SQL.

Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 5 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

Дополнительная документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-reverse"></a>Преобразование запросов, использующих Reverse

Теперь можно преобразовывать запросы, использующие `Reverse`. Пример:

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-bitwise-operators"></a>Преобразование запросов для побитовых операторов

Запросы, использующие побитовые операторы, теперь преобразуются в ряде дополнительных случаев, например:

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-strings-on-cosmos"></a>Преобразование запросов для строк в Cosmos

Запросы, использующие строковые методы Contains, StartsWith и EndsWith, теперь преобразуются при использовании поставщика Azure Cosmos DB.

Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).
