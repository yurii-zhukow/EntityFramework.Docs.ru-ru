---
title: Перехватчики — EF Core
description: Перехват для операций базы данных и других событий
author: ajcvickers
ms.date: 10/08/2020
uid: core/logging-events-diagnostics/interceptors
ms.openlocfilehash: 61ec6968344798af8ecffb878a1e47a6a8e031cd
ms.sourcegitcommit: 42bbf7f68e92c364c5fff63092d3eb02229f568d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/11/2020
ms.locfileid: "94503206"
---
# <a name="interceptors"></a>Перехватчики

Перехватчики Entity Framework Core (EF Core) позволяют перехват, изменение и/или подавление операций EF Core. Сюда входят низкоуровневые операции с базами данных, такие как выполнение команды, а также операции более высокого уровня, такие как вызовы SaveChanges.

Перехватчики отличаются от ведения журнала и диагностики тем, что они позволяют изменять или подавить перехватывать операции. [Простое ведение журнала](xref:core/logging-events-diagnostics/simple-logging) или [Microsoft. Extensions. Logging](xref:core/logging-events-diagnostics/extensions-logging) — это лучший выбор для ведения журнала.

Перехватчики регистрируются для каждого экземпляра DbContext при настройке контекста. Используйте [прослушиватель диагностики](xref:core/logging-events-diagnostics/diagnostic-listeners) для получения одних и тех же сведений, но для всех экземпляров DbContext в процессе.

## <a name="registering-interceptors"></a>Регистрация перехватчиков

Перехватчики регистрируются с помощью <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.AddInterceptors%2A> при [настройке экземпляра DbContext](xref:core/dbcontext-configuration/index). Обычно это делается при переопределении <xref:Microsoft.EntityFrameworkCore.DbContext.OnConfiguring%2A?displayProperty=nameWithType> . Пример:

<!--
public class ExampleContext : BlogsContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder) 
        => optionsBuilder.AddInterceptors(new TaggedQueryCommandInterceptor());
}
-->
[!code-csharp[RegisterInterceptor](../../../samples/core/Miscellaneous/CommandInterception/Program.cs?name=RegisterInterceptor)]

Кроме того, `AddInterceptors` может вызываться как часть <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext%2A> или при создании <xref:Microsoft.EntityFrameworkCore.DbContextOptions> экземпляра для передачи в конструктор DbContext.

> [!TIP]
> Onconfiguring по-прежнему вызывается при использовании AddDbContext или при передаче экземпляра DbContextOptions в конструктор DbContext. Это делает его идеальным местом для применения конфигурации контекста независимо от того, как создается DbContext.

Перехватчики часто не имеют состояния. Это означает, что один экземпляр перехватчика можно использовать для всех экземпляров DbContext. Пример:

<!--
public class TaggedQueryCommandInterceptorContext : BlogsContext
{
    private static readonly TaggedQueryCommandInterceptor _interceptor 
        = new TaggedQueryCommandInterceptor();

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder) 
        => optionsBuilder.AddInterceptors(_interceptor);
}
-->
[!code-csharp[RegisterStatelessInterceptor](../../../samples/core/Miscellaneous/CommandInterception/Program.cs?name=RegisterStatelessInterceptor)]

Каждый экземпляр перехватчика должен реализовывать один или несколько интерфейсов, производных от <xref:Microsoft.EntityFrameworkCore.Diagnostics.IInterceptor> . Каждый экземпляр должен быть зарегистрирован только один раз, даже если он реализует несколько интерфейсов перехвата; EF Core будет направлять события для каждого интерфейса соответствующим образом.

## <a name="database-interception"></a>Перехват базы данных

> [!NOTE]
> Перехват базы данных был добавлен в EF Core 3,0 и доступен только для поставщиков реляционных баз данных.
> Добавлена поддержка точки сохранения в EF Core 5,0.

Перехват низкоуровневых баз данных разбивается на три интерфейса, показанные в следующей таблице.

| Перехватчик                                                            | Операции с базой данных перехвачены
|:-----------------------------------------------------------------------|-------------------------------------------------
| <xref:Microsoft.EntityFrameworkCore.Diagnostics.IDbCommandInterceptor> | Создание команд</br>Выполнение команд</br>Сбои команд</br>Удаление DbDataReader команды
| <xref:Microsoft.EntityFrameworkCore.Diagnostics.IDbConnectionInterceptor> | Открытие и закрытие подключений</br>Ошибки подключения
| <xref:Microsoft.EntityFrameworkCore.Diagnostics.IDbTransactionInterceptor> | Создание транзакций</br>Использование существующих транзакций</br>Фиксация транзакций</br>Откат транзакций</br>Создание и использование точек сохранения</br>Сбои транзакций

Базовые классы <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbCommandInterceptor> , <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbConnectionInterceptor> и <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbTransactionInterceptor> содержат реализации No-Op для каждого метода в соответствующем интерфейсе. Используйте базовые классы, чтобы избежать необходимости реализовывать неиспользуемые методы перехвата.

Методы для каждого типа перехватчика поступают парами, первая из которых вызывается перед запуском операции базы данных, а вторая после завершения операции. Например, если выбран диапазон 10.0.0.0/20 для виртуальной сети, для пространства клиентских адресов можно выбрать 10.1.0.0/24. Например, <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbCommandInterceptor.ReaderExecuting%2A?displayProperty=nameWithType> вызывается перед выполнением запроса и <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbCommandInterceptor.ReaderExecuted%2A?displayProperty=nameWithType> вызывается после отправки запроса в базу данных.

Каждая пара методов имеет как синхронизацию, так и асинхронные варианты. Это позволяет выполнять асинхронный ввод-вывод, например запрос маркера доступа, в рамках перехвата асинхронной операции с базой данных.

### <a name="example-command-interception-to-add-query-hints"></a>Пример: перехват команд для добавления указания запроса

> [!TIP]  
> [Пример перехватчика команд можно скачать](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/CommandInterception) с сайта GitHub.

<xref:Microsoft.EntityFrameworkCore.Diagnostics.IDbCommandInterceptor>Можно использовать для изменения SQL перед его отправкой в базу данных. В этом примере показано, как изменить SQL, включив в него указание запроса.

Часто сложную часть перехвата определяет, соответствует ли команда запросу, который необходимо изменить. Анализ SQL является одним из вариантов, но он, как правило, является ненадежным. Кроме того, можно использовать [Теги запроса EF Core](xref:core/querying/tags) , чтобы пометить каждый запрос, который необходимо изменить. Пример:

<!--
            var blogs1 = context.Blogs.TagWith("Use hint: robust plan").ToList();
-->
[!code-csharp[TaggedQuery](../../../samples/core/Miscellaneous/CommandInterception/Program.cs?name=TaggedQuery)]

Затем этот тег может быть обнаружен в перехватчике, так как он всегда будет включаться в качестве комментария в первой строке текста команды. При обнаружении тега SQL-запрос изменяется, чтобы добавить подходящее указание.

<!--
public class TaggedQueryCommandInterceptor : DbCommandInterceptor
{
    public override InterceptionResult<DbDataReader> ReaderExecuting(
        DbCommand command,
        CommandEventData eventData,
        InterceptionResult<DbDataReader> result)
    {
        ManipulateCommand(command);

        return result;
    }

    public override ValueTask<InterceptionResult<DbDataReader>> ReaderExecutingAsync(
        DbCommand command,
        CommandEventData eventData,
        InterceptionResult<DbDataReader> result,
        CancellationToken cancellationToken = default)
    {
        ManipulateCommand(command);

        return new ValueTask<InterceptionResult<DbDataReader>>(result);
    }

    private static void ManipulateCommand(DbCommand command)
    {
        if (command.CommandText.StartsWith("-- Use hint: robust plan", StringComparison.Ordinal))
        {
            command.CommandText += " OPTION (ROBUST PLAN)";
        }
    }
}
-->
[!code-csharp[TaggedQueryCommandInterceptor](../../../samples/core/Miscellaneous/CommandInterception/TaggedQueryCommandInterceptor.cs?name=TaggedQueryCommandInterceptor)]

Примечание:

* Перехватчик наследует от <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbCommandInterceptor> , чтобы не реализовывать каждый метод в интерфейсе перехватчика.
* Перехватчик реализует как синхронные, так и асинхронные методы. Это гарантирует, что для синхронизации и асинхронных запросов применяется одно и то же указание запроса.
* Перехватчик реализует `Executing` методы, которые вызываются EF Core с созданным SQL _перед_ отправкой в базу данных. Сравните это с `Executed` методами, которые вызываются после возвращения вызова базы данных.

При выполнении кода в этом примере создается следующее при отметке запроса:

```sql
-- Use hint: robust plan

SELECT [b].[Id], [b].[Name]
FROM [Blogs] AS [b] OPTION (ROBUST PLAN)
```

С другой стороны, если запрос не имеет тегов, он отправляется в базу данных без изменений:

```sql
SELECT [b].[Id], [b].[Name]
FROM [Blogs] AS [b]
```

### <a name="example-connection-interception-for-sql-azure-authentication-using-add"></a>Пример. перехват подключения для проверки подлинности SQL Azure с помощью ADD

> [!TIP]  
> [Пример перехватчика подключения можно скачать](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/ConnectionInterception) с сайта GitHub.

<xref:Microsoft.EntityFrameworkCore.Diagnostics.IDbConnectionInterceptor>Можно использовать для управления до того, <xref:System.Data.Common.DbConnection> как он будет использоваться для подключения к базе данных. Это можно использовать для получения маркера доступа Azure Active Directory (AAD). Пример:

<!--
public class AadAuthenticationInterceptor : DbConnectionInterceptor
{
    public override InterceptionResult ConnectionOpening(
        DbConnection connection,
        ConnectionEventData eventData,
        InterceptionResult result)
        => throw new InvalidOperationException("Open connections asynchronously when using AAD authentication.");

    public override async ValueTask<InterceptionResult> ConnectionOpeningAsync(
        DbConnection connection,
        ConnectionEventData eventData,
        InterceptionResult result,
        CancellationToken cancellationToken = default)
    {
        var sqlConnection = (SqlConnection)connection;

        var provider = new AzureServiceTokenProvider();
        // Note: in some situations the access token may not be cached automatically the Azure Token Provider.
        // Depending on the kind of token requested, you may need to implement your own caching here.
        sqlConnection.AccessToken = await provider.GetAccessTokenAsync("https://database.windows.net/", null, cancellationToken);

        return result;
    }
}
-->
[!code-csharp[AadAuthenticationInterceptor](../../../samples/core/Miscellaneous/ConnectionInterception/AadAuthenticationInterceptor.cs?name=AadAuthenticationInterceptor)]

> [!TIP]
> [Microsoft. Data. SqlClient](https://www.nuget.org/packages/Microsoft.Data.SqlClient/) теперь поддерживает проверку подлинности AAD через строку подключения. Подробнее см. в разделе <xref:Microsoft.Data.SqlClient.SqlAuthenticationMethod>.

> [!WARNING]
> Обратите внимание, что перехватчик создает исключение при вызове синхронизации для открытия соединения. Это обусловлено тем, что для получения маркера доступа нет метода, не являющегося асинхронным, и нет [универсального и простого способа вызова асинхронного метода из контекста, не являющегося асинхронным, без риска взаимоблокировки](https://devblogs.microsoft.com/dotnet/configureawait-faq/).

> [!WARNING]
> в некоторых ситуациях маркер доступа может не кэшироваться автоматически поставщиком маркеров Azure. В зависимости от типа запрошенного маркера может потребоваться реализовать собственное кэширование здесь.

### <a name="example-advanced-command-interception-for-caching"></a>Пример. Расширенное перехват команд для кэширования

> [!TIP]  
> [Пример расширенного перехватчика команд можно скачать](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/CachingInterception) с сайта GitHub.

Перехватчики EF Core могут:

* Укажите, EF Core подавить выполнение операции, для которой выполняется перехват
* Изменение результата операции, возвращенной в EF Core

В этом примере показан перехватчик, использующий эти функции для поведения как для примитивного кэша второго уровня. Результаты кэшированных запросов возвращаются для конкретного запроса, что позволяет избежать циклического обращения к базе данных.

> [!WARNING]
> При изменении поведения EF Core по умолчанию следует соблюдать осторожность. EF Core может вести себя непредвиденным образом, если он получает некорректный результат, который не может правильно обработать его. Кроме того, в этом примере демонстрируются понятия перехватчика; Он не предназначен как шаблон для надежной реализации кэша второго уровня.

В этом примере приложение часто выполняет запрос для получения последнего "ежедневного сообщения":

<!--
        async Task<string> GetDailyMessage(DailyMessageContext context)
            => (await context.DailyMessages.TagWith("Get_Daily_Message").OrderBy(e => e.Id).LastAsync()).Message;
-->
[!code-csharp[GetDailyMessage](../../../samples/core/Miscellaneous/CachingInterception/Program.cs?name=GetDailyMessage)]

Этот запрос [помечен](xref:core/querying/tags) таким образом, чтобы его можно было легко обнаружить в перехватчике. Идея состоит в том, чтобы только запросить новое сообщение в базе данных каждый день. В других случаях приложение будет использовать кэшированный результат. (В примере для имитации нового дня в примере используется задержка 10 секунд.)

#### <a name="interceptor-state"></a>Состояние перехватчика

Этот перехватчик находится в состоянии: он сохраняет идентификатор и текст последнего запрошенного ежедневно сообщения, а также время выполнения этого запроса. Из-за этого состояния также требуется [Блокировка](/dotnet/csharp/language-reference/keywords/lock-statement) , так как для кэширования требуется, чтобы один и тот же перехватчик использовался несколькими экземплярами контекста.

<!--
    private readonly object _lock = new object();
    private int _id;
    private string _message;
    private DateTime _queriedAt;
-->
[!code-csharp[InterceptorState](../../../samples/core/Miscellaneous/CachingInterception/CachingCommandInterceptor.cs?name=InterceptorState)]

#### <a name="before-execution"></a>Перед выполнением

В `Executing` методе (т. е. перед вызовом базы данных) перехватчик обнаруживает запрос с тегами и проверяет, есть ли в нем кэшированный результат. Если такой результат найден, запрос подавляется и вместо него используются кэшированные результаты.

<!--
    public override ValueTask<InterceptionResult<DbDataReader>> ReaderExecutingAsync(
        DbCommand command,
        CommandEventData eventData,
        InterceptionResult<DbDataReader> result,
        CancellationToken cancellationToken = default)
    {
        if (command.CommandText.StartsWith("-- Get_Daily_Message", StringComparison.Ordinal))
        {
            lock (_lock)
            {
                if (_message != null
                    && DateTime.UtcNow < _queriedAt + new TimeSpan(0, 0, 10))
                {
                    command.CommandText = "-- Get_Daily_Message: Skipping DB call; using cache.";
                    result = InterceptionResult<DbDataReader>.SuppressWithResult(new CachedDailyMessageDataReader(_id, _message));
                }
            }
        }

        return new ValueTask<InterceptionResult<DbDataReader>>(result);
    }
-->
[!code-csharp[ReaderExecutingAsync](../../../samples/core/Miscellaneous/CachingInterception/CachingCommandInterceptor.cs?name=ReaderExecutingAsync)]

Обратите внимание, что код вызывает <xref:Microsoft.EntityFrameworkCore.Diagnostics.InterceptionResult%601.SuppressWithResult%2A?displayProperty=nameWithType> и передает замену, <xref:System.Data.Common.DbDataReader> содержащую кэшированные данные. Затем возвращается этот Интерцептионресулт, что приводит к подавлению выполнения запроса. Взамен вместо этого используется EF Core в качестве результатов запроса.

Этот перехватчик также управляет текстом команды. Эта манипуляция не является обязательной, но улучшает ясность в сообщениях журнала. Текст команды не обязательно должен быть допустимым SQL, так как запрос теперь не будет выполняться.

#### <a name="after-execution"></a>После выполнения

Если кэшированное сообщение недоступно или срок его действия истек, то приведенный выше код не подавляет результат. Таким образом, EF Core будет выполнять запрос в нормальном режиме. Затем он вернется в метод перехватчика `Executed` после выполнения. На этом этапе, если результат еще не является кэшированным модулем чтения, идентификатор и строка нового сообщения точно изменяются от реального читателя и кэшируются для следующего использования этого запроса.

<!--
    public override async ValueTask<DbDataReader> ReaderExecutedAsync(
        DbCommand command,
        CommandExecutedEventData eventData,
        DbDataReader result,
        CancellationToken cancellationToken = default)
    {
        if (command.CommandText.StartsWith("-- Get_Daily_Message", StringComparison.Ordinal)
            && !(result is CachedDailyMessageDataReader))
        {
            try
            {
                await result.ReadAsync(cancellationToken);

                lock (_lock)
                {
                    _id = result.GetInt32(0);
                    _message = result.GetString(1);
                    _queriedAt = DateTime.UtcNow;
                    return new CachedDailyMessageDataReader(_id, message);
                }
            }
            finally
            {
                await result.DisposeAsync();
            }
        }

        return result;
    }
-->
[!code-csharp[ReaderExecutedAsync](../../../samples/core/Miscellaneous/CachingInterception/CachingCommandInterceptor.cs?name=ReaderExecutedAsync)]

#### <a name="demonstration"></a>Демонстрация

[Пример перехватчика кэширования](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/CachingInterception) содержит простое консольное приложение, которое запрашивает ежедневные сообщения для тестирования кэширования:

<!--
        // 1. Initialize the database with some daily messages.
        using (var context = new DailyMessageContext())
        {
            await context.Database.EnsureDeletedAsync();
            await context.Database.EnsureCreatedAsync();

            context.AddRange(
                new DailyMessage { Message = "Remember: All builds are GA; no builds are RTM." },
                new DailyMessage { Message = "Keep calm and drink tea" });

            await context.SaveChangesAsync();
        }

        // 2. Query for the most recent daily message. It will be cached for 10 seconds.
        using (var context = new DailyMessageContext())
        {
            Console.WriteLine(await GetDailyMessage(context));
        }

        // 3. Insert a new daily message.
        using (var context = new DailyMessageContext())
        {
            context.Add(new DailyMessage { Message = "Free beer for unicorns" });

            await context.SaveChangesAsync();
        }

        // 4. Cached message is used until cache expires.
        using (var context = new DailyMessageContext())
        {
            Console.WriteLine(await GetDailyMessage(context));
        }

        // 5. Pretend it's the next day.
        Thread.Sleep(10000);

        // 6. Cache is expired, so the last message will noe be queried again.
        using (var context = new DailyMessageContext())
        {
            Console.WriteLine(await GetDailyMessage(context));
        }
        
        #region GetDailyMessage
        async Task<string> GetDailyMessage(DailyMessageContext context)
            => (await context.DailyMessages.TagWith("Get_Daily_Message").OrderBy(e => e.Id).LastAsync()).Message;
        #endregion
-->
[!code-csharp[Main](../../../samples/core/Miscellaneous/CachingInterception/Program.cs?name=Main)]

Результат должен быть таким:

```output
info: 10/15/2020 12:32:11.801 RelationalEventId.CommandExecuted[20101] (Microsoft.EntityFrameworkCore.Database.Command)
      Executed DbCommand (0ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      -- Get_Daily_Message

      SELECT "d"."Id", "d"."Message"
      FROM "DailyMessages" AS "d"
      ORDER BY "d"."Id" DESC
      LIMIT 1

Keep calm and drink tea

info: 10/15/2020 12:32:11.821 RelationalEventId.CommandExecuted[20101] (Microsoft.EntityFrameworkCore.Database.Command)
      Executed DbCommand (0ms) [Parameters=[@p0='Free beer for unicorns' (Size = 22)], CommandType='Text', CommandTimeout='30']
      INSERT INTO "DailyMessages" ("Message")
      VALUES (@p0);
      SELECT "Id"
      FROM "DailyMessages"
      WHERE changes() = 1 AND "rowid" = last_insert_rowid();

info: 10/15/2020 12:32:11.826 RelationalEventId.CommandExecuted[20101] (Microsoft.EntityFrameworkCore.Database.Command)
      Executed DbCommand (0ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      -- Get_Daily_Message: Skipping DB call; using cache.

Keep calm and drink tea

info: 10/15/2020 12:32:21.833 RelationalEventId.CommandExecuted[20101] (Microsoft.EntityFrameworkCore.Database.Command)
      Executed DbCommand (0ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      -- Get_Daily_Message

      SELECT "d"."Id", "d"."Message"
      FROM "DailyMessages" AS "d"
      ORDER BY "d"."Id" DESC
      LIMIT 1

Free beer for unicorns
```

Обратите внимание, что в выходных данных журнала приложение по-новому использует кэшированное сообщение до истечения времени ожидания, после чего база данных снова запрашивается для любого нового сообщения.

## <a name="savechanges-interception"></a>SaveChanges перехвата

> [!NOTE]
> Перехват SaveChanges был добавлен в EF Core 5,0.

> [!TIP]  
> Вы можете [скачать пример перехватчика SaveChanges](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/SaveChangesInterception) из GitHub.

<xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A><xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A>точки перехвата определяются`ISaveChangesInterceptor` <!-- Issue #2748 --> . Как и для других перехватчиков, `SaveChangesInterceptor` <!-- Issue #2748 --> базовый класс с методами без операций предоставляется для удобства.

> [!TIP]
> Перехватчики являются мощными. Однако во многих случаях может быть проще переопределить метод SaveChanges или использовать [события .NET для SaveChanges](xref:core/logging-events-diagnostics/events) , предоставляемых в DbContext.

### <a name="example-savechanges-interception-for-auditing"></a>Пример: SaveChanges перехват для аудита

SaveChanges можно перехватывать, чтобы создать независимую запись аудита внесенных изменений.

> [!NOTE]
> Это не является надежным решением для аудита. Это простой пример, который используется для демонстрации функций перехвата.

#### <a name="the-application-context"></a>Контекст приложения

В [примере для аудита](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/SaveChangesInterception) используется простая DbContext с блогами и записями.

<!--
public class BlogsContext : DbContext
{
    private readonly AuditingInterceptor _auditingInterceptor = new AuditingInterceptor("DataSource=audit.db");

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .AddInterceptors(_auditingInterceptor)
            .UseSqlite("DataSource=blogs.db");

    public DbSet<Blog> Blogs { get; set; }
}

public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }

    public Blog Blog { get; set; }
}
-->
[!code-csharp[BlogsContext](../../../samples/core/Miscellaneous/SaveChangesInterception/BlogsContext.cs?name=BlogsContext)]

Обратите внимание, что для каждого экземпляра DbContext зарегистрирован новый экземпляр перехватчика. Это обусловлено тем, что перехватчик аудита содержит состояние, связанное с экземпляром текущего контекста.

#### <a name="the-audit-context"></a>Контекст аудита

Образец также содержит второй DbContext и модель, используемую для базы данных аудита.

<!--
public class AuditContext : DbContext
{
    private readonly string _connectionString;

    public AuditContext(string connectionString)
    {
        _connectionString = connectionString;
    }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.UseSqlite(_connectionString);

    public DbSet<SaveChangesAudit> SaveChangesAudits { get; set; }
}

public class SaveChangesAudit
{
    public int Id { get; set; }
    public Guid AuditId { get; set; }
    public DateTime StartTime { get; set; }
    public DateTime EndTime { get; set; }
    public bool Succeeded { get; set; }
    public string ErrorMessage { get; set; }

    public ICollection<EntityAudit> Entities { get; } = new List<EntityAudit>();
}

public class EntityAudit
{
    public int Id { get; set; }
    public EntityState State { get; set; }
    public string AuditMessage { get; set; }

    public SaveChangesAudit SaveChangesAudit { get; set; }
}
-->
[!code-csharp[AuditContext](../../../samples/core/Miscellaneous/SaveChangesInterception/AuditContext.cs?name=AuditContext)]

#### <a name="the-interceptor"></a>Перехватчик

Общая идея аудита с перехватчиком:

* Сообщение аудита создается в начале SaveChanges и записывается в базу данных аудита.
* Можно продолжать использовать SaveChanges
* Если SaveChanges завершается успешно, сообщение аудита обновляется и указывает на успешное выполнение.
* Если происходит сбой команды SaveChanges, то сообщение аудита обновляется, чтобы указать на ошибку.

Первый этап обрабатывается перед отправкой изменений в базу данных с помощью переопределений `ISaveChangesInterceptor.SavingChanges` <!-- Issue #2748 -->  и `ISaveChangesInterceptor.SavingChangesAsync`<!-- Issue #2748 -->.

<!--
    public async ValueTask<InterceptionResult<int>> SavingChangesAsync(
        DbContextEventData eventData,
        InterceptionResult<int> result,
        CancellationToken cancellationToken = default)
    {
        _audit = CreateAudit(eventData.Context);

        using (var auditContext = new AuditContext(_connectionString))
        {
            auditContext.Add(_audit);
            await auditContext.SaveChangesAsync();
        }

        return result;
    }

    public InterceptionResult<int> SavingChanges(
        DbContextEventData eventData,
        InterceptionResult<int> result)
    {
        _audit = CreateAudit(eventData.Context);

        using (var auditContext = new AuditContext(_connectionString))
        {
            auditContext.Add(_audit);
            auditContext.SaveChanges();
        }

        return result;
    }
-->
[!code-csharp[SavingChanges](../../../samples/core/Miscellaneous/SaveChangesInterception/AuditingInterceptor.cs?name=SavingChanges)]

Переопределение методов Sync и Async гарантирует, что аудит будет выполняться независимо от того, вызваны ли методы SaveChanges или SaveChangesAsync. Также обратите внимание, что асинхронная перегрузка сама по себе может выполнять неблокирующие асинхронные операции ввода-вывода в базе данных аудита. Можно создать исключение из метода Sync Савингчанжес, чтобы гарантировать, что все операции ввода-вывода базы данных будут асинхронными. Это требует, чтобы приложение всегда вызывало SaveChangesAsync, а не SaveChanges.

#### <a name="the-audit-message"></a>Сообщение аудита

Каждый метод перехватчика имеет `eventData` параметр, предоставляющий контекстные сведения о перехваченном событии. В этом случае текущее приложение DbContext включается в данные события, которое затем используется для создания сообщения аудита.

<!--
    private static SaveChangesAudit CreateAudit(DbContext context)
    {
        context.ChangeTracker.DetectChanges();

        var audit = new SaveChangesAudit
        {
            AuditId = Guid.NewGuid(),
            StartTime = DateTime.UtcNow
        };

        foreach (var entry in context.ChangeTracker.Entries())
        {
            var auditMessage = entry.State switch
            {
                EntityState.Deleted => CreateDeletedMessage(entry),
                EntityState.Modified => CreateModifiedMessage(entry),
                EntityState.Added => CreateAddedMessage(entry),
                _ => null
            };

            if (auditMessage != null)
            {
                audit.Entities.Add(new EntityAudit { State = entry.State, AuditMessage = auditMessage });
            }
        }

        return audit;

        string CreateAddedMessage(EntityEntry entry)
            => entry.Properties.Aggregate(
                $"Inserting {entry.Metadata.DisplayName()} with ",
                (auditString, property) => auditString + $"{property.Metadata.Name}: '{property.CurrentValue}' ");

        string CreateModifiedMessage(EntityEntry entry)
            => entry.Properties.Where(property => property.IsModified || property.Metadata.IsPrimaryKey()).Aggregate(
                $"Updating {entry.Metadata.DisplayName()} with ",
                (auditString, property) => auditString + $"{property.Metadata.Name}: '{property.CurrentValue}' ");

        string CreateDeletedMessage(EntityEntry entry)
            => entry.Properties.Where(property => property.Metadata.IsPrimaryKey()).Aggregate(
                $"Deleting {entry.Metadata.DisplayName()} with ",
                (auditString, property) => auditString + $"{property.Metadata.Name}: '{property.CurrentValue}' ");
    }
-->
[!code-csharp[CreateAudit](../../../samples/core/Miscellaneous/SaveChangesInterception/AuditingInterceptor.cs?name=CreateAudit)]

Результатом является `SaveChangesAudit` сущность с коллекцией `EntityAudit` сущностей, по одной для каждой вставки, обновления или удаления. Затем перехватчик вставляет эти сущности в базу данных аудита.

> [!TIP]
> ToString переопределяется в каждом классе данных EF Core событий для создания эквивалентного сообщения журнала для события. Например, при вызове метода `ContextInitializedEventData.ToString` создается "Entity Framework Core 5.0.0 Initialized" блогсконтекст "с помощью поставщика" Microsoft. EntityFrameworkCore. SQLite "с параметрами: None".

#### <a name="detecting-success"></a>Обнаружение успеха

Сущность Audit хранится на перехватчике, поэтому к нему можно получить доступ снова после того, как SaveChanges завершится успешно или неудачно. В случае успеха `ISaveChangesInterceptor.SavedChanges` <!-- Issue #2748 --> или `ISaveChangesInterceptor.SavedChangesAsync` <!-- Issue #2748 --> вызывается метод ;

<!--
    public int SavedChanges(SaveChangesCompletedEventData eventData, int result)
    {
        using (var auditContext = new AuditContext(_connectionString))
        {
            auditContext.Attach(_audit);
            _audit.Succeeded = true;
            _audit.EndTime = DateTime.UtcNow;

            auditContext.SaveChanges();
        }

        return result;
    }

    public async ValueTask<int> SavedChangesAsync(
        SaveChangesCompletedEventData eventData,
        int result,
        CancellationToken cancellationToken = new CancellationToken())
    {
        using (var auditContext = new AuditContext(_connectionString))
        {
            auditContext.Attach(_audit);
            _audit.Succeeded = true;
            _audit.EndTime = DateTime.UtcNow;

            await auditContext.SaveChangesAsync(cancellationToken);
        }

        return result;
    }
-->
[!code-csharp[SavedChanges](../../../samples/core/Miscellaneous/SaveChangesInterception/AuditingInterceptor.cs?name=SavedChanges)]

Сущность Audit присоединяется к контексту аудита, так как он уже существует в базе данных и нуждается в обновлении. Затем мы устанавливаем `Succeeded` и `EndTime` , который помечает эти свойства как измененные, поэтому SaveChanges будет отсылать обновление в базу данных аудита.

#### <a name="detecting-failure"></a>Обнаружение сбоя

Сбой обрабатывается во многом так же, как и успешно, но в `ISaveChangesInterceptor.SaveChangesFailed` <!-- Issue #2748 --> или `ISaveChangesInterceptor.SaveChangesFailedAsync` <!-- Issue #2748 --> метод. Данные события содержат созданное исключение.

<!--
    public void SaveChangesFailed(DbContextErrorEventData eventData)
    {
        using (var auditContext = new AuditContext(_connectionString))
        {
            auditContext.Attach(_audit);
            _audit.Succeeded = false;
            _audit.EndTime = DateTime.UtcNow;
            _audit.ErrorMessage = eventData.Exception.Message;

            auditContext.SaveChanges();
        }
    }

    public async Task SaveChangesFailedAsync(
        DbContextErrorEventData eventData,
        CancellationToken cancellationToken = new CancellationToken())
    {
        using (var auditContext = new AuditContext(_connectionString))
        {
            auditContext.Attach(_audit);
            _audit.Succeeded = false;
            _audit.EndTime = DateTime.UtcNow;
            _audit.ErrorMessage = eventData.Exception.InnerException?.Message;

            await auditContext.SaveChangesAsync(cancellationToken);
        }
    }
-->
[!code-csharp[SaveChangesFailed](../../../samples/core/Miscellaneous/SaveChangesInterception/AuditingInterceptor.cs?name=SaveChangesFailed)]

#### <a name="demonstration"></a>Демонстрация

[Образец аудита](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/SaveChangesInterception) содержит простое консольное приложение, которое вносит изменения в базу данных блогов, а затем отображает созданный аудит.

<!--
        // Insert, update, and delete some entities

        using (var context = new BlogsContext())
        {
            context.Add(
                new Blog
                {
                    Name = "EF Blog",
                    Posts =
                    {
                        new Post { Title = "EF Core 3.1!" },
                        new Post { Title = "EF Core 5.0!" }
                    }
                });

            await context.SaveChangesAsync();
        }

        using (var context = new BlogsContext())
        {
            var blog = context.Blogs.Include(e => e.Posts).Single();

            blog.Name = "EF Core Blog";
            context.Remove(blog.Posts.First());
            blog.Posts.Add(new Post { Title = "EF Core 6.0!" });

            context.SaveChanges();
        }

        // Do an insert that will fail

        using (var context = new BlogsContext())
        {
            try
            {
                context.Add(new Post { Id = 3, Title = "EF Core 3.1!" });

                await context.SaveChangesAsync();
            }
            catch (DbUpdateException)
            {
            }
        }

        // Look at the audit trail

        using (var context = new AuditContext("DataSource=audit.db"))
        {
            foreach (var audit in context.SaveChangesAudits.Include(e => e.Entities).ToList())
            {
                Console.WriteLine(
                    $"Audit {audit.AuditId} from {audit.StartTime} to {audit.EndTime} was{(audit.Succeeded ? "" : " not")} successful.");

                foreach (var entity in audit.Entities)
                {
                    Console.WriteLine($"  {entity.AuditMessage}");
                }

                if (!audit.Succeeded)
                {
                    Console.WriteLine($"  Error: {audit.ErrorMessage}");
                }
            }
        }
-->
[!code-csharp[Program](../../../samples/core/Miscellaneous/SaveChangesInterception/Program.cs?name=Program)]

В результате отобразится содержимое базы данных аудита:

```output
Audit 52e94327-1767-4046-a3ca-4c6b1eecbca6 from 10/14/2020 9:10:17 PM to 10/14/2020 9:10:17 PM was successful.
  Inserting Blog with Id: '-2147482647' Name: 'EF Blog'
  Inserting Post with Id: '-2147482647' BlogId: '-2147482647' Title: 'EF Core 3.1!'
  Inserting Post with Id: '-2147482646' BlogId: '-2147482647' Title: 'EF Core 5.0!'
Audit 8450f57a-5030-4211-a534-eb66b8da7040 from 10/14/2020 9:10:17 PM to 10/14/2020 9:10:17 PM was successful.
  Inserting Post with Id: '-2147482645' BlogId: '1' Title: 'EF Core 6.0!'
  Updating Blog with Id: '1' Name: 'EF Core Blog'
  Deleting Post with Id: '1'
Audit 201fef4d-66a7-43ad-b9b6-b57e9d3f37b3 from 10/14/2020 9:10:17 PM to 10/14/2020 9:10:17 PM was not successful.
  Inserting Post with Id: '3' BlogId: '' Title: 'EF Core 3.1!'
  Error: SQLite Error 19: 'UNIQUE constraint failed: Post.Id'.
```
