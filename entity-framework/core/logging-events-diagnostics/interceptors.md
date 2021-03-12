---
title: Перехватчики — EF Core
description: Перехват для операций базы данных и других событий
author: ajcvickers
ms.date: 10/08/2020
uid: core/logging-events-diagnostics/interceptors
ms.openlocfilehash: 459c0495e9a2f81e2e84388988f04ca9787080cc
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103024229"
---
# <a name="interceptors"></a><span data-ttu-id="71305-103">Перехватчики</span><span class="sxs-lookup"><span data-stu-id="71305-103">Interceptors</span></span>

<span data-ttu-id="71305-104">Перехватчики Entity Framework Core (EF Core) позволяют перехват, изменение и/или подавление операций EF Core.</span><span class="sxs-lookup"><span data-stu-id="71305-104">Entity Framework Core (EF Core) interceptors enable interception, modification, and/or suppression of EF Core operations.</span></span> <span data-ttu-id="71305-105">Сюда входят низкоуровневые операции с базами данных, такие как выполнение команды, а также операции более высокого уровня, такие как вызовы SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="71305-105">This includes low-level database operations such as executing a command, as well as higher-level operations, such as calls to SaveChanges.</span></span>

<span data-ttu-id="71305-106">Перехватчики отличаются от ведения журнала и диагностики, так как они позволяют изменять или подавлять перехватываемую операцию.</span><span class="sxs-lookup"><span data-stu-id="71305-106">Interceptors are different from logging and diagnostics in that they allow modification or suppression of the operation being intercepted.</span></span> <span data-ttu-id="71305-107">Для ведения журнала лучше всего использовать [средство простого ведения журнала](xref:core/logging-events-diagnostics/simple-logging) или [Microsoft.Extensions.Logging](xref:core/logging-events-diagnostics/extensions-logging).</span><span class="sxs-lookup"><span data-stu-id="71305-107">[Simple logging](xref:core/logging-events-diagnostics/simple-logging) or [Microsoft.Extensions.Logging](xref:core/logging-events-diagnostics/extensions-logging) are better choices for logging.</span></span>

<span data-ttu-id="71305-108">Перехватчики регистрируются для каждого экземпляра DbContext во время настройки контекста.</span><span class="sxs-lookup"><span data-stu-id="71305-108">Interceptors are registered per DbContext instance when the context is configured.</span></span> <span data-ttu-id="71305-109">С помощью [прослушивателя диагностики](xref:core/logging-events-diagnostics/diagnostic-listeners) можно получить ту же информацию, но для всех экземпляров DbContext в процессе.</span><span class="sxs-lookup"><span data-stu-id="71305-109">Use a [diagnostic listener](xref:core/logging-events-diagnostics/diagnostic-listeners) to get the same information but for all DbContext instances in the process.</span></span>

## <a name="registering-interceptors"></a><span data-ttu-id="71305-110">Регистрация перехватчиков</span><span class="sxs-lookup"><span data-stu-id="71305-110">Registering interceptors</span></span>

<span data-ttu-id="71305-111">Перехватчики регистрируются с помощью <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.AddInterceptors%2A> при [настройке экземпляра DbContext](xref:core/dbcontext-configuration/index).</span><span class="sxs-lookup"><span data-stu-id="71305-111">Interceptors are registered using <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.AddInterceptors%2A> when [configuring a DbContext instance](xref:core/dbcontext-configuration/index).</span></span> <span data-ttu-id="71305-112">Обычно это делается при переопределении <xref:Microsoft.EntityFrameworkCore.DbContext.OnConfiguring%2A?displayProperty=nameWithType> .</span><span class="sxs-lookup"><span data-stu-id="71305-112">This is commonly done in an override of <xref:Microsoft.EntityFrameworkCore.DbContext.OnConfiguring%2A?displayProperty=nameWithType>.</span></span> <span data-ttu-id="71305-113">Например:</span><span class="sxs-lookup"><span data-stu-id="71305-113">For example:</span></span>

<!--
public class ExampleContext : BlogsContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.AddInterceptors(new TaggedQueryCommandInterceptor());
}
-->
[!code-csharp[RegisterInterceptor](../../../samples/core/Miscellaneous/CommandInterception/Program.cs?name=RegisterInterceptor)]

<span data-ttu-id="71305-114">Кроме того, `AddInterceptors` может вызываться как часть <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext%2A> или при создании <xref:Microsoft.EntityFrameworkCore.DbContextOptions> экземпляра для передачи в конструктор DbContext.</span><span class="sxs-lookup"><span data-stu-id="71305-114">Alternately, `AddInterceptors` can be called as part of <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext%2A> or when creating a <xref:Microsoft.EntityFrameworkCore.DbContextOptions> instance to pass to the DbContext constructor.</span></span>

> [!TIP]
> <span data-ttu-id="71305-115">Onconfiguring по-прежнему вызывается при использовании AddDbContext или при передаче экземпляра DbContextOptions в конструктор DbContext.</span><span class="sxs-lookup"><span data-stu-id="71305-115">OnConfiguring is still called when AddDbContext is used or a DbContextOptions instance is passed to the DbContext constructor.</span></span> <span data-ttu-id="71305-116">Это делает его идеальным местом для применения конфигурации контекста независимо от того, как создается DbContext.</span><span class="sxs-lookup"><span data-stu-id="71305-116">This makes it the ideal place to apply context configuration regardless of how the DbContext is constructed.</span></span>

<span data-ttu-id="71305-117">Перехватчики часто не имеют состояния. Это означает, что один экземпляр перехватчика можно использовать для всех экземпляров DbContext.</span><span class="sxs-lookup"><span data-stu-id="71305-117">Interceptors are often stateless, which means that a single interceptor instance can be used for all DbContext instances.</span></span> <span data-ttu-id="71305-118">Например:</span><span class="sxs-lookup"><span data-stu-id="71305-118">For example:</span></span>

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

<span data-ttu-id="71305-119">Каждый экземпляр перехватчика должен реализовывать один или несколько интерфейсов, производных от <xref:Microsoft.EntityFrameworkCore.Diagnostics.IInterceptor> .</span><span class="sxs-lookup"><span data-stu-id="71305-119">Every interceptor instance must implement one or more interface derived from <xref:Microsoft.EntityFrameworkCore.Diagnostics.IInterceptor>.</span></span> <span data-ttu-id="71305-120">Каждый экземпляр должен быть зарегистрирован только один раз, даже если он реализует несколько интерфейсов перехвата; EF Core будет направлять события для каждого интерфейса соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="71305-120">Each instance should only be registered once even if it implements multiple interception interfaces; EF Core will route events for each interface as appropriate.</span></span>

## <a name="database-interception"></a><span data-ttu-id="71305-121">Перехват базы данных</span><span class="sxs-lookup"><span data-stu-id="71305-121">Database interception</span></span>

> [!NOTE]
> <span data-ttu-id="71305-122">Перехват базы данных появился в EF Core 3,0 и доступен только для поставщиков реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="71305-122">Database interception was introduced in EF Core 3.0 and is only available for relational database providers.</span></span>
> <span data-ttu-id="71305-123">Поддержка точки сохранения появилась в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="71305-123">Savepoint support was introduced in EF Core 5.0.</span></span>

<span data-ttu-id="71305-124">Перехват низкоуровневых баз данных разбивается на три интерфейса, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="71305-124">Low-level database interception is split into the three interfaces shown in the following table.</span></span>

| <span data-ttu-id="71305-125">Перехватчик</span><span class="sxs-lookup"><span data-stu-id="71305-125">Interceptor</span></span>                                                            | <span data-ttu-id="71305-126">Операции с базой данных перехвачены</span><span class="sxs-lookup"><span data-stu-id="71305-126">Database operations intercepted</span></span>
|:-----------------------------------------------------------------------|-------------------------------------------------
| <xref:Microsoft.EntityFrameworkCore.Diagnostics.IDbCommandInterceptor> | <span data-ttu-id="71305-127">Создание команд</span><span class="sxs-lookup"><span data-stu-id="71305-127">Creating commands</span></span></br><span data-ttu-id="71305-128">Выполнение команд</span><span class="sxs-lookup"><span data-stu-id="71305-128">Executing commands</span></span></br><span data-ttu-id="71305-129">Сбои команд</span><span class="sxs-lookup"><span data-stu-id="71305-129">Command failures</span></span></br><span data-ttu-id="71305-130">Удаление DbDataReader команды</span><span class="sxs-lookup"><span data-stu-id="71305-130">Disposing the command's DbDataReader</span></span>
| <xref:Microsoft.EntityFrameworkCore.Diagnostics.IDbConnectionInterceptor> | <span data-ttu-id="71305-131">Открытие и закрытие подключений</span><span class="sxs-lookup"><span data-stu-id="71305-131">Opening and closing connections</span></span></br><span data-ttu-id="71305-132">Ошибки подключения</span><span class="sxs-lookup"><span data-stu-id="71305-132">Connection failures</span></span>
| <xref:Microsoft.EntityFrameworkCore.Diagnostics.IDbTransactionInterceptor> | <span data-ttu-id="71305-133">Создание транзакций</span><span class="sxs-lookup"><span data-stu-id="71305-133">Creating transactions</span></span></br><span data-ttu-id="71305-134">Использование существующих транзакций</span><span class="sxs-lookup"><span data-stu-id="71305-134">Using existing transactions</span></span></br><span data-ttu-id="71305-135">Фиксация транзакций</span><span class="sxs-lookup"><span data-stu-id="71305-135">Committing transactions</span></span></br><span data-ttu-id="71305-136">Откат транзакций</span><span class="sxs-lookup"><span data-stu-id="71305-136">Rolling back transactions</span></span></br><span data-ttu-id="71305-137">Создание и использование точек сохранения</span><span class="sxs-lookup"><span data-stu-id="71305-137">Creating and using savepoints</span></span></br><span data-ttu-id="71305-138">Сбои транзакций</span><span class="sxs-lookup"><span data-stu-id="71305-138">Transaction failures</span></span>

<span data-ttu-id="71305-139">Базовые классы <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbCommandInterceptor> , <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbConnectionInterceptor> и <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbTransactionInterceptor> содержат реализации No-Op для каждого метода в соответствующем интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="71305-139">The base classes <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbCommandInterceptor>, <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbConnectionInterceptor>, and <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbTransactionInterceptor> contain no-op implementations for each method in the corresponding interface.</span></span> <span data-ttu-id="71305-140">Используйте базовые классы, чтобы избежать необходимости реализовывать неиспользуемые методы перехвата.</span><span class="sxs-lookup"><span data-stu-id="71305-140">Use the base classes to avoid the need to implement unused interception methods.</span></span>

<span data-ttu-id="71305-141">Методы для каждого типа перехватчика поступают парами, первая из которых вызывается перед запуском операции базы данных, а вторая после завершения операции.</span><span class="sxs-lookup"><span data-stu-id="71305-141">The methods on each interceptor type come in pairs, with the first being called before the database operation is started, and the second after the operation has completed.</span></span> <span data-ttu-id="71305-142">Например, если выбран диапазон 10.0.0.0/20 для виртуальной сети, для пространства клиентских адресов можно выбрать 10.1.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="71305-142">For example.</span></span> <span data-ttu-id="71305-143">Например, <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbCommandInterceptor.ReaderExecuting%2A?displayProperty=nameWithType> вызывается перед выполнением запроса и <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbCommandInterceptor.ReaderExecuted%2A?displayProperty=nameWithType> вызывается после отправки запроса в базу данных.</span><span class="sxs-lookup"><span data-stu-id="71305-143">For example, <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbCommandInterceptor.ReaderExecuting%2A?displayProperty=nameWithType> is called before a query is executed, and <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbCommandInterceptor.ReaderExecuted%2A?displayProperty=nameWithType> is called after query has been sent to the database.</span></span>

<span data-ttu-id="71305-144">Каждая пара методов имеет как синхронизацию, так и асинхронные варианты.</span><span class="sxs-lookup"><span data-stu-id="71305-144">Each pair of methods have both sync and async variations.</span></span> <span data-ttu-id="71305-145">Это позволяет выполнять асинхронный ввод-вывод, например запрос маркера доступа, в рамках перехвата асинхронной операции с базой данных.</span><span class="sxs-lookup"><span data-stu-id="71305-145">This allows for asynchronous I/O, such as requesting an access token, to happen as part of intercepting an async database operation.</span></span>

### <a name="example-command-interception-to-add-query-hints"></a><span data-ttu-id="71305-146">Пример: перехват команд для добавления указания запроса</span><span class="sxs-lookup"><span data-stu-id="71305-146">Example: Command interception to add query hints</span></span>

> [!TIP]
> <span data-ttu-id="71305-147">[Пример перехватчика команд можно скачать](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Miscellaneous/CommandInterception) с сайта GitHub.</span><span class="sxs-lookup"><span data-stu-id="71305-147">You can [download the command interceptor sample](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Miscellaneous/CommandInterception) from GitHub.</span></span>

<span data-ttu-id="71305-148"><xref:Microsoft.EntityFrameworkCore.Diagnostics.IDbCommandInterceptor>Можно использовать для изменения SQL перед его отправкой в базу данных.</span><span class="sxs-lookup"><span data-stu-id="71305-148">An <xref:Microsoft.EntityFrameworkCore.Diagnostics.IDbCommandInterceptor> can be used to modify SQL before it is sent to the database.</span></span> <span data-ttu-id="71305-149">В этом примере показано, как изменить SQL, включив в него указание запроса.</span><span class="sxs-lookup"><span data-stu-id="71305-149">This example shows how to modify the SQL to include a query hint.</span></span>

<span data-ttu-id="71305-150">Часто сложную часть перехвата определяет, соответствует ли команда запросу, который необходимо изменить.</span><span class="sxs-lookup"><span data-stu-id="71305-150">Often, the trickiest part of the interception is determining when the command corresponds to the query that needs to be modified.</span></span> <span data-ttu-id="71305-151">Анализ SQL является одним из вариантов, но он, как правило, является ненадежным.</span><span class="sxs-lookup"><span data-stu-id="71305-151">Parsing the SQL is one option, but tends to be fragile.</span></span> <span data-ttu-id="71305-152">Кроме того, можно использовать [Теги запроса EF Core](xref:core/querying/tags) , чтобы пометить каждый запрос, который необходимо изменить.</span><span class="sxs-lookup"><span data-stu-id="71305-152">Another option is to use [EF Core query tags](xref:core/querying/tags) to tag each query that should be modified.</span></span> <span data-ttu-id="71305-153">Например:</span><span class="sxs-lookup"><span data-stu-id="71305-153">For example:</span></span>

<!--
            var blogs1 = context.Blogs.TagWith("Use hint: robust plan").ToList();
-->
[!code-csharp[TaggedQuery](../../../samples/core/Miscellaneous/CommandInterception/Program.cs?name=TaggedQuery)]

<span data-ttu-id="71305-154">Затем этот тег может быть обнаружен в перехватчике, так как он всегда будет включаться в качестве комментария в первой строке текста команды.</span><span class="sxs-lookup"><span data-stu-id="71305-154">This tag can then be detected in the interceptor as it will always be included as a comment in the first line of the command text.</span></span> <span data-ttu-id="71305-155">При обнаружении тега SQL-запрос изменяется, чтобы добавить подходящее указание.</span><span class="sxs-lookup"><span data-stu-id="71305-155">On detecting the tag, the query SQL is modified to add the appropriate hint:</span></span>

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

<span data-ttu-id="71305-156">Примечание:</span><span class="sxs-lookup"><span data-stu-id="71305-156">Notice:</span></span>

* <span data-ttu-id="71305-157">Перехватчик наследует от <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbCommandInterceptor> , чтобы не реализовывать каждый метод в интерфейсе перехватчика.</span><span class="sxs-lookup"><span data-stu-id="71305-157">The interceptor inherits from <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbCommandInterceptor> to avoid having to implement every method in the interceptor interface.</span></span>
* <span data-ttu-id="71305-158">Перехватчик реализует как синхронные, так и асинхронные методы.</span><span class="sxs-lookup"><span data-stu-id="71305-158">The interceptor implements both sync and async methods.</span></span> <span data-ttu-id="71305-159">Это гарантирует, что для синхронизации и асинхронных запросов применяется одно и то же указание запроса.</span><span class="sxs-lookup"><span data-stu-id="71305-159">This ensures that the same query hint is applied to sync and async queries.</span></span>
* <span data-ttu-id="71305-160">Перехватчик реализует `Executing` методы, которые вызываются EF Core с созданным SQL _перед_ отправкой в базу данных.</span><span class="sxs-lookup"><span data-stu-id="71305-160">The interceptor implements the `Executing` methods which are called by EF Core with the generated SQL _before_ it is sent to the database.</span></span> <span data-ttu-id="71305-161">Сравните это с `Executed` методами, которые вызываются после возвращения вызова базы данных.</span><span class="sxs-lookup"><span data-stu-id="71305-161">Contrast this with the `Executed` methods, which are called after the database call has returned.</span></span>

<span data-ttu-id="71305-162">При выполнении кода в этом примере создается следующее при отметке запроса:</span><span class="sxs-lookup"><span data-stu-id="71305-162">Running the code in this example generates the following when a query is tagged:</span></span>

```sql
-- Use hint: robust plan

SELECT [b].[Id], [b].[Name]
FROM [Blogs] AS [b] OPTION (ROBUST PLAN)
```

<span data-ttu-id="71305-163">С другой стороны, если запрос не имеет тегов, он отправляется в базу данных без изменений:</span><span class="sxs-lookup"><span data-stu-id="71305-163">On the other hand, when a query is not tagged, then it is sent to the database unmodified:</span></span>

```sql
SELECT [b].[Id], [b].[Name]
FROM [Blogs] AS [b]
```

### <a name="example-connection-interception-for-sql-azure-authentication-using-add"></a><span data-ttu-id="71305-164">Пример. перехват подключения для проверки подлинности SQL Azure с помощью ADD</span><span class="sxs-lookup"><span data-stu-id="71305-164">Example: Connection interception for SQL Azure authentication using ADD</span></span>

> [!TIP]
> <span data-ttu-id="71305-165">[Пример перехватчика подключения можно скачать](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Miscellaneous/ConnectionInterception) с сайта GitHub.</span><span class="sxs-lookup"><span data-stu-id="71305-165">You can [download the connection interceptor sample](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Miscellaneous/ConnectionInterception) from GitHub.</span></span>

<span data-ttu-id="71305-166"><xref:Microsoft.EntityFrameworkCore.Diagnostics.IDbConnectionInterceptor>Можно использовать для управления до того, <xref:System.Data.Common.DbConnection> как он будет использоваться для подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="71305-166">An <xref:Microsoft.EntityFrameworkCore.Diagnostics.IDbConnectionInterceptor> can be used to manipulate the <xref:System.Data.Common.DbConnection> before it is used to connect to the database.</span></span> <span data-ttu-id="71305-167">Это можно использовать для получения маркера доступа Azure Active Directory (AAD).</span><span class="sxs-lookup"><span data-stu-id="71305-167">This can be used to obtain an Azure Active Directory (AAD) access token.</span></span> <span data-ttu-id="71305-168">Например:</span><span class="sxs-lookup"><span data-stu-id="71305-168">For example:</span></span>

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
> <span data-ttu-id="71305-169">[Microsoft. Data. SqlClient](https://www.nuget.org/packages/Microsoft.Data.SqlClient/) теперь поддерживает проверку подлинности AAD через строку подключения.</span><span class="sxs-lookup"><span data-stu-id="71305-169">[Microsoft.Data.SqlClient](https://www.nuget.org/packages/Microsoft.Data.SqlClient/) now supports AAD authentication via connection string.</span></span> <span data-ttu-id="71305-170">Дополнительные сведения см. в разделе <xref:Microsoft.Data.SqlClient.SqlAuthenticationMethod>.</span><span class="sxs-lookup"><span data-stu-id="71305-170">See <xref:Microsoft.Data.SqlClient.SqlAuthenticationMethod> for more information.</span></span>

> [!WARNING]
> <span data-ttu-id="71305-171">Обратите внимание, что перехватчик создает исключение при вызове синхронизации для открытия соединения.</span><span class="sxs-lookup"><span data-stu-id="71305-171">Notice that the interceptor throws if a sync call is made to open the connection.</span></span> <span data-ttu-id="71305-172">Это обусловлено тем, что для получения маркера доступа нет метода, не являющегося асинхронным, и нет [универсального и простого способа вызова асинхронного метода из контекста, не являющегося асинхронным, без риска взаимоблокировки](https://devblogs.microsoft.com/dotnet/configureawait-faq/).</span><span class="sxs-lookup"><span data-stu-id="71305-172">This is because there is no non-async method to obtain the access token and there is [no universal and simple way to call an async method from non-async context without risking deadlock](https://devblogs.microsoft.com/dotnet/configureawait-faq/).</span></span>

> [!WARNING]
> <span data-ttu-id="71305-173">в некоторых ситуациях маркер доступа может не кэшироваться автоматически поставщиком маркеров Azure.</span><span class="sxs-lookup"><span data-stu-id="71305-173">in some situations the access token may not be cached automatically the Azure Token Provider.</span></span> <span data-ttu-id="71305-174">В зависимости от типа запрошенного маркера может потребоваться реализовать собственное кэширование здесь.</span><span class="sxs-lookup"><span data-stu-id="71305-174">Depending on the kind of token requested, you may need to implement your own caching here.</span></span>

### <a name="example-advanced-command-interception-for-caching"></a><span data-ttu-id="71305-175">Пример. Расширенное перехват команд для кэширования</span><span class="sxs-lookup"><span data-stu-id="71305-175">Example: Advanced command interception for caching</span></span>

> [!TIP]
> <span data-ttu-id="71305-176">[Пример расширенного перехватчика команд можно скачать](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Miscellaneous/CachingInterception) с сайта GitHub.</span><span class="sxs-lookup"><span data-stu-id="71305-176">You can [download the advanced command interceptor sample](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Miscellaneous/CachingInterception) from GitHub.</span></span>

<span data-ttu-id="71305-177">Перехватчики EF Core могут:</span><span class="sxs-lookup"><span data-stu-id="71305-177">EF Core interceptors can:</span></span>

* <span data-ttu-id="71305-178">Укажите, EF Core подавить выполнение операции, для которой выполняется перехват</span><span class="sxs-lookup"><span data-stu-id="71305-178">Tell EF Core to suppress executing the operation being intercepted</span></span>
* <span data-ttu-id="71305-179">Изменение результата операции, возвращенной в EF Core</span><span class="sxs-lookup"><span data-stu-id="71305-179">Change the result of the operation reported back to EF Core</span></span>

<span data-ttu-id="71305-180">В этом примере показан перехватчик, использующий эти функции для поведения как для примитивного кэша второго уровня.</span><span class="sxs-lookup"><span data-stu-id="71305-180">This example shows an interceptor that uses these features to behave like a primitive second-level cache.</span></span> <span data-ttu-id="71305-181">Результаты кэшированных запросов возвращаются для конкретного запроса, что позволяет избежать циклического обращения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="71305-181">Cached query results are returned for a specific query, avoiding a database roundtrip.</span></span>

> [!WARNING]
> <span data-ttu-id="71305-182">При изменении поведения EF Core по умолчанию следует соблюдать осторожность.</span><span class="sxs-lookup"><span data-stu-id="71305-182">Take care when changing the EF Core default behavior in this way.</span></span> <span data-ttu-id="71305-183">EF Core может вести себя непредвиденным образом, если он получает некорректный результат, который не может правильно обработать его.</span><span class="sxs-lookup"><span data-stu-id="71305-183">EF Core may behave in unexpected ways if it gets an abnormal result that it cannot process correctly.</span></span> <span data-ttu-id="71305-184">Кроме того, в этом примере демонстрируются понятия перехватчика; Он не предназначен как шаблон для надежной реализации кэша второго уровня.</span><span class="sxs-lookup"><span data-stu-id="71305-184">Also, this example demonstrates interceptor concepts; it is not intended as a template for a robust second-level cache implementation.</span></span>

<span data-ttu-id="71305-185">В этом примере приложение часто выполняет запрос для получения последнего "ежедневного сообщения":</span><span class="sxs-lookup"><span data-stu-id="71305-185">In this example, the application frequently executes a query to obtain the most recent "daily message":</span></span>

<!--
        async Task<string> GetDailyMessage(DailyMessageContext context)
            => (await context.DailyMessages.TagWith("Get_Daily_Message").OrderBy(e => e.Id).LastAsync()).Message;
-->
[!code-csharp[GetDailyMessage](../../../samples/core/Miscellaneous/CachingInterception/Program.cs?name=GetDailyMessage)]

<span data-ttu-id="71305-186">Этот запрос [помечен](xref:core/querying/tags) таким образом, чтобы его можно было легко обнаружить в перехватчике.</span><span class="sxs-lookup"><span data-stu-id="71305-186">This query is [tagged](xref:core/querying/tags) so that it can be easily detected in the interceptor.</span></span> <span data-ttu-id="71305-187">Идея состоит в том, чтобы только запросить новое сообщение в базе данных каждый день.</span><span class="sxs-lookup"><span data-stu-id="71305-187">The idea is to only query the database for a new message once every day.</span></span> <span data-ttu-id="71305-188">В других случаях приложение будет использовать кэшированный результат.</span><span class="sxs-lookup"><span data-stu-id="71305-188">At other times the application will use a cached result.</span></span> <span data-ttu-id="71305-189">(В примере для имитации нового дня в примере используется задержка 10 секунд.)</span><span class="sxs-lookup"><span data-stu-id="71305-189">(The sample uses delay of 10 seconds in the sample to simulate a new day.)</span></span>

#### <a name="interceptor-state"></a><span data-ttu-id="71305-190">Состояние перехватчика</span><span class="sxs-lookup"><span data-stu-id="71305-190">Interceptor state</span></span>

<span data-ttu-id="71305-191">Этот перехватчик находится в состоянии: он сохраняет идентификатор и текст последнего запрошенного ежедневно сообщения, а также время выполнения этого запроса.</span><span class="sxs-lookup"><span data-stu-id="71305-191">This interceptor is stateful: it stores the ID and message text of the most recent daily message queried, plus the time when that query was executed.</span></span> <span data-ttu-id="71305-192">Из-за этого состояния также требуется [Блокировка](/dotnet/csharp/language-reference/keywords/lock-statement) , так как для кэширования требуется, чтобы один и тот же перехватчик использовался несколькими экземплярами контекста.</span><span class="sxs-lookup"><span data-stu-id="71305-192">Because of this state we also need a [lock](/dotnet/csharp/language-reference/keywords/lock-statement) since the caching requires that same interceptor must be used by multiple context instances.</span></span>

<!--
    private readonly object _lock = new object();
    private int _id;
    private string _message;
    private DateTime _queriedAt;
-->
[!code-csharp[InterceptorState](../../../samples/core/Miscellaneous/CachingInterception/CachingCommandInterceptor.cs?name=InterceptorState)]

#### <a name="before-execution"></a><span data-ttu-id="71305-193">Перед выполнением</span><span class="sxs-lookup"><span data-stu-id="71305-193">Before execution</span></span>

<span data-ttu-id="71305-194">В `Executing` методе (т. е. перед вызовом базы данных) перехватчик обнаруживает запрос с тегами и проверяет, есть ли в нем кэшированный результат.</span><span class="sxs-lookup"><span data-stu-id="71305-194">In the `Executing` method (i.e. before making a database call), the interceptor detects the tagged query and then checks if there is a cached result.</span></span> <span data-ttu-id="71305-195">Если такой результат найден, запрос подавляется и вместо него используются кэшированные результаты.</span><span class="sxs-lookup"><span data-stu-id="71305-195">If such a result is found, then the query is suppressed and cached results are used instead.</span></span>

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

<span data-ttu-id="71305-196">Обратите внимание, что код вызывает <xref:Microsoft.EntityFrameworkCore.Diagnostics.InterceptionResult%601.SuppressWithResult%2A?displayProperty=nameWithType> и передает замену, <xref:System.Data.Common.DbDataReader> содержащую кэшированные данные.</span><span class="sxs-lookup"><span data-stu-id="71305-196">Notice how the code calls <xref:Microsoft.EntityFrameworkCore.Diagnostics.InterceptionResult%601.SuppressWithResult%2A?displayProperty=nameWithType> and passes a replacement <xref:System.Data.Common.DbDataReader> containing the cached data.</span></span> <span data-ttu-id="71305-197">Затем возвращается этот Интерцептионресулт, что приводит к подавлению выполнения запроса.</span><span class="sxs-lookup"><span data-stu-id="71305-197">This InterceptionResult is then returned, causing suppression of query execution.</span></span> <span data-ttu-id="71305-198">Взамен вместо этого используется EF Core в качестве результатов запроса.</span><span class="sxs-lookup"><span data-stu-id="71305-198">The replacement reader is instead used by EF Core as the results of the query.</span></span>

<span data-ttu-id="71305-199">Этот перехватчик также управляет текстом команды.</span><span class="sxs-lookup"><span data-stu-id="71305-199">This interceptor also manipulates the command text.</span></span> <span data-ttu-id="71305-200">Эта манипуляция не является обязательной, но улучшает ясность в сообщениях журнала.</span><span class="sxs-lookup"><span data-stu-id="71305-200">This manipulation is not required, but improves clarity in log messages.</span></span> <span data-ttu-id="71305-201">Текст команды не обязательно должен быть допустимым SQL, так как запрос теперь не будет выполняться.</span><span class="sxs-lookup"><span data-stu-id="71305-201">The command text does not need to be valid SQL since the query is now not going to be executed.</span></span>

#### <a name="after-execution"></a><span data-ttu-id="71305-202">После выполнения</span><span class="sxs-lookup"><span data-stu-id="71305-202">After execution</span></span>

<span data-ttu-id="71305-203">Если кэшированное сообщение недоступно или срок его действия истек, то приведенный выше код не подавляет результат.</span><span class="sxs-lookup"><span data-stu-id="71305-203">If no cached message is available, or if it has expired, then the code above does not suppress the result.</span></span> <span data-ttu-id="71305-204">Таким образом, EF Core будет выполнять запрос в нормальном режиме.</span><span class="sxs-lookup"><span data-stu-id="71305-204">EF Core will therefore execute the query as normal.</span></span> <span data-ttu-id="71305-205">Затем он вернется в метод перехватчика `Executed` после выполнения.</span><span class="sxs-lookup"><span data-stu-id="71305-205">It will then return to the interceptor's `Executed` method after execution.</span></span> <span data-ttu-id="71305-206">На этом этапе, если результат еще не является кэшированным модулем чтения, идентификатор и строка нового сообщения точно изменяются от реального читателя и кэшируются для следующего использования этого запроса.</span><span class="sxs-lookup"><span data-stu-id="71305-206">At this point if the result is not already a cached reader, then the new message ID and string is exacted from the real reader and cached ready for the next use of this query.</span></span>

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

#### <a name="demonstration"></a><span data-ttu-id="71305-207">Демонстрация</span><span class="sxs-lookup"><span data-stu-id="71305-207">Demonstration</span></span>

<span data-ttu-id="71305-208">[Пример перехватчика кэширования](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Miscellaneous/CachingInterception) содержит простое консольное приложение, которое запрашивает ежедневные сообщения для тестирования кэширования:</span><span class="sxs-lookup"><span data-stu-id="71305-208">The [caching interceptor sample](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Miscellaneous/CachingInterception) contains a simple console application that queries for daily messages to test the caching:</span></span>

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

<span data-ttu-id="71305-209">Результат должен быть таким:</span><span class="sxs-lookup"><span data-stu-id="71305-209">This results in the following output:</span></span>

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

<span data-ttu-id="71305-210">Обратите внимание, что в выходных данных журнала приложение по-новому использует кэшированное сообщение до истечения времени ожидания, после чего база данных снова запрашивается для любого нового сообщения.</span><span class="sxs-lookup"><span data-stu-id="71305-210">Notice from the log output that the application continues to use the cached message until the timeout expires, at which point the database is queried again for any new message.</span></span>

## <a name="savechanges-interception"></a><span data-ttu-id="71305-211">SaveChanges перехвата</span><span class="sxs-lookup"><span data-stu-id="71305-211">SaveChanges interception</span></span>

> [!NOTE]
> <span data-ttu-id="71305-212">Перехват SaveChanges появился в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="71305-212">SaveChanges interception was introduced in EF Core 5.0.</span></span>

> [!TIP]
> <span data-ttu-id="71305-213">Вы можете [скачать пример перехватчика SaveChanges](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Miscellaneous/SaveChangesInterception) из GitHub.</span><span class="sxs-lookup"><span data-stu-id="71305-213">You can [download the SaveChanges interceptor sample](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Miscellaneous/SaveChangesInterception) from GitHub.</span></span>

<span data-ttu-id="71305-214"><xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A><xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A>точки перехвата определяются <xref:Microsoft.EntityFrameworkCore.Diagnostics.ISaveChangesInterceptor> интерфейсом.</span><span class="sxs-lookup"><span data-stu-id="71305-214"><xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> and <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A> interception points are defined by the <xref:Microsoft.EntityFrameworkCore.Diagnostics.ISaveChangesInterceptor> interface.</span></span> <span data-ttu-id="71305-215">Как и для других перехватчиков, в <xref:Microsoft.EntityFrameworkCore.Diagnostics.SaveChangesInterceptor> качестве удобства предоставляется базовый класс с методами, не имеющими Op.</span><span class="sxs-lookup"><span data-stu-id="71305-215">As for other interceptors, the <xref:Microsoft.EntityFrameworkCore.Diagnostics.SaveChangesInterceptor> base class with no-op methods is provided as a convenience.</span></span>

> [!TIP]
> <span data-ttu-id="71305-216">Перехватчики являются мощными.</span><span class="sxs-lookup"><span data-stu-id="71305-216">Interceptors are powerful.</span></span> <span data-ttu-id="71305-217">Однако во многих случаях может быть проще переопределить метод SaveChanges или использовать [события .NET для SaveChanges](xref:core/logging-events-diagnostics/events) , предоставляемых в DbContext.</span><span class="sxs-lookup"><span data-stu-id="71305-217">However, in many cases it may be easier to override the SaveChanges method or use the [.NET events for SaveChanges](xref:core/logging-events-diagnostics/events) exposed on DbContext.</span></span>

### <a name="example-savechanges-interception-for-auditing"></a><span data-ttu-id="71305-218">Пример: SaveChanges перехват для аудита</span><span class="sxs-lookup"><span data-stu-id="71305-218">Example: SaveChanges interception for auditing</span></span>

<span data-ttu-id="71305-219">SaveChanges можно перехватывать, чтобы создать независимую запись аудита внесенных изменений.</span><span class="sxs-lookup"><span data-stu-id="71305-219">SaveChanges can be intercepted to create an independent audit record of the changes made.</span></span>

> [!NOTE]
> <span data-ttu-id="71305-220">Это не является надежным решением для аудита.</span><span class="sxs-lookup"><span data-stu-id="71305-220">This is not intended to be a robust auditing solution.</span></span> <span data-ttu-id="71305-221">Это простой пример, который используется для демонстрации функций перехвата.</span><span class="sxs-lookup"><span data-stu-id="71305-221">Rather it is a simplistic example used to demonstrate the features of interception.</span></span>

#### <a name="the-application-context"></a><span data-ttu-id="71305-222">Контекст приложения</span><span class="sxs-lookup"><span data-stu-id="71305-222">The application context</span></span>

<span data-ttu-id="71305-223">В [примере для аудита](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Miscellaneous/SaveChangesInterception) используется простая DbContext с блогами и записями.</span><span class="sxs-lookup"><span data-stu-id="71305-223">The [sample for auditing](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Miscellaneous/SaveChangesInterception) uses a simple DbContext with blogs and posts.</span></span>

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

<span data-ttu-id="71305-224">Обратите внимание, что для каждого экземпляра DbContext зарегистрирован новый экземпляр перехватчика.</span><span class="sxs-lookup"><span data-stu-id="71305-224">Notice that a new instance of the interceptor is registered for each DbContext instance.</span></span> <span data-ttu-id="71305-225">Это обусловлено тем, что перехватчик аудита содержит состояние, связанное с экземпляром текущего контекста.</span><span class="sxs-lookup"><span data-stu-id="71305-225">This is because the auditing interceptor contains state linked to the current context instance.</span></span>

#### <a name="the-audit-context"></a><span data-ttu-id="71305-226">Контекст аудита</span><span class="sxs-lookup"><span data-stu-id="71305-226">The audit context</span></span>

<span data-ttu-id="71305-227">Образец также содержит второй DbContext и модель, используемую для базы данных аудита.</span><span class="sxs-lookup"><span data-stu-id="71305-227">The sample also contains a second DbContext and model used for the auditing database.</span></span>

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

#### <a name="the-interceptor"></a><span data-ttu-id="71305-228">Перехватчик</span><span class="sxs-lookup"><span data-stu-id="71305-228">The interceptor</span></span>

<span data-ttu-id="71305-229">Общая идея аудита с перехватчиком:</span><span class="sxs-lookup"><span data-stu-id="71305-229">The general idea for auditing with the interceptor is:</span></span>

* <span data-ttu-id="71305-230">Сообщение аудита создается в начале SaveChanges и записывается в базу данных аудита.</span><span class="sxs-lookup"><span data-stu-id="71305-230">An audit message is created at the beginning of SaveChanges and is written to the auditing database</span></span>
* <span data-ttu-id="71305-231">Можно продолжать использовать SaveChanges</span><span class="sxs-lookup"><span data-stu-id="71305-231">SaveChanges is allowed to continue</span></span>
* <span data-ttu-id="71305-232">Если SaveChanges завершается успешно, сообщение аудита обновляется и указывает на успешное выполнение.</span><span class="sxs-lookup"><span data-stu-id="71305-232">If SaveChanges succeeds, then the audit message is updated to indicate success</span></span>
* <span data-ttu-id="71305-233">Если происходит сбой команды SaveChanges, то сообщение аудита обновляется, чтобы указать на ошибку.</span><span class="sxs-lookup"><span data-stu-id="71305-233">If SaveChanges fails, then the audit message is updated to indicate the failure</span></span>

<span data-ttu-id="71305-234">Первый этап обрабатывается перед отправкой изменений в базу данных с помощью переопределений <xref:Microsoft.EntityFrameworkCore.Diagnostics.ISaveChangesInterceptor.SavingChanges%2A?displayProperty=nameWithType> и <xref:Microsoft.EntityFrameworkCore.Diagnostics.ISaveChangesInterceptor.SavingChangesAsync%2A?displayProperty=nameWithType> .</span><span class="sxs-lookup"><span data-stu-id="71305-234">The first stage is handled before any changes are sent to the database using overrides of <xref:Microsoft.EntityFrameworkCore.Diagnostics.ISaveChangesInterceptor.SavingChanges%2A?displayProperty=nameWithType> and <xref:Microsoft.EntityFrameworkCore.Diagnostics.ISaveChangesInterceptor.SavingChangesAsync%2A?displayProperty=nameWithType>.</span></span>

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

<span data-ttu-id="71305-235">Переопределение методов Sync и Async гарантирует, что аудит будет выполняться независимо от того, `SaveChanges` вызывается ли метод или `SaveChangesAsync` .</span><span class="sxs-lookup"><span data-stu-id="71305-235">Overriding both sync and async methods ensures that auditing will happen regardless of whether `SaveChanges` or `SaveChangesAsync` are called.</span></span> <span data-ttu-id="71305-236">Также обратите внимание, что асинхронная перегрузка сама по себе может выполнять неблокирующие асинхронные операции ввода-вывода в базе данных аудита.</span><span class="sxs-lookup"><span data-stu-id="71305-236">Notice also that the async overload is itself able to perform non-blocking async I/O to the auditing database.</span></span> <span data-ttu-id="71305-237">Можно создать исключение из `SavingChanges` метода Sync, чтобы гарантировать, что все операции ввода-вывода базы данных будут асинхронными.</span><span class="sxs-lookup"><span data-stu-id="71305-237">You may wish to throw from the sync `SavingChanges` method to ensure that all database I/O is async.</span></span> <span data-ttu-id="71305-238">Это требует, чтобы приложение всегда вызывало `SaveChangesAsync` и никогда `SaveChanges` .</span><span class="sxs-lookup"><span data-stu-id="71305-238">This then requires that the application always calls `SaveChangesAsync` and never `SaveChanges`.</span></span>

#### <a name="the-audit-message"></a><span data-ttu-id="71305-239">Сообщение аудита</span><span class="sxs-lookup"><span data-stu-id="71305-239">The audit message</span></span>

<span data-ttu-id="71305-240">Каждый метод перехватчика имеет `eventData` параметр, предоставляющий контекстные сведения о перехваченном событии.</span><span class="sxs-lookup"><span data-stu-id="71305-240">Every interceptor method has an `eventData` parameter providing contextual information about the event being intercepted.</span></span> <span data-ttu-id="71305-241">В этом случае текущее приложение DbContext включается в данные события, которое затем используется для создания сообщения аудита.</span><span class="sxs-lookup"><span data-stu-id="71305-241">In this case the current application DbContext is included in the event data, which is then used to create an audit message.</span></span>

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

<span data-ttu-id="71305-242">Результатом является `SaveChangesAudit` сущность с коллекцией `EntityAudit` сущностей, по одной для каждой вставки, обновления или удаления.</span><span class="sxs-lookup"><span data-stu-id="71305-242">The result is a `SaveChangesAudit` entity with a collection of `EntityAudit` entities, one for each insert, update, or delete.</span></span> <span data-ttu-id="71305-243">Затем перехватчик вставляет эти сущности в базу данных аудита.</span><span class="sxs-lookup"><span data-stu-id="71305-243">The interceptor then inserts these entities into the audit database.</span></span>

> [!TIP]
> <span data-ttu-id="71305-244">ToString переопределяется в каждом классе данных EF Core событий для создания эквивалентного сообщения журнала для события.</span><span class="sxs-lookup"><span data-stu-id="71305-244">ToString is overridden in every EF Core event data class to generate the equivalent log message for the event.</span></span> <span data-ttu-id="71305-245">Например, при вызове метода `ContextInitializedEventData.ToString` создается "Entity Framework Core 5.0.0 Initialized" блогсконтекст "с помощью поставщика" Microsoft. EntityFrameworkCore. SQLite "с параметрами: None".</span><span class="sxs-lookup"><span data-stu-id="71305-245">For example, calling `ContextInitializedEventData.ToString` generates "Entity Framework Core 5.0.0 initialized 'BlogsContext' using provider 'Microsoft.EntityFrameworkCore.Sqlite' with options: None".</span></span>

#### <a name="detecting-success"></a><span data-ttu-id="71305-246">Обнаружение успеха</span><span class="sxs-lookup"><span data-stu-id="71305-246">Detecting success</span></span>

<span data-ttu-id="71305-247">Сущность Audit хранится на перехватчике, поэтому к нему можно получить доступ снова после того, как SaveChanges завершится успешно или неудачно.</span><span class="sxs-lookup"><span data-stu-id="71305-247">The audit entity is stored on the interceptor so that it can be accessed again once SaveChanges either succeeds or fails.</span></span> <span data-ttu-id="71305-248">Для успешного выполнения <xref:Microsoft.EntityFrameworkCore.Diagnostics.ISaveChangesInterceptor.SavedChanges%2A?displayProperty=nameWithType> или <xref:Microsoft.EntityFrameworkCore.Diagnostics.ISaveChangesInterceptor.SavedChangesAsync%2A?displayProperty=nameWithType> вызывается.</span><span class="sxs-lookup"><span data-stu-id="71305-248">For success, <xref:Microsoft.EntityFrameworkCore.Diagnostics.ISaveChangesInterceptor.SavedChanges%2A?displayProperty=nameWithType> or <xref:Microsoft.EntityFrameworkCore.Diagnostics.ISaveChangesInterceptor.SavedChangesAsync%2A?displayProperty=nameWithType> is called.</span></span>

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

<span data-ttu-id="71305-249">Сущность Audit присоединяется к контексту аудита, так как он уже существует в базе данных и нуждается в обновлении.</span><span class="sxs-lookup"><span data-stu-id="71305-249">The audit entity is attached to the audit context, since it already exists in the database and needs to be updated.</span></span> <span data-ttu-id="71305-250">Затем мы устанавливаем `Succeeded` и `EndTime` , который помечает эти свойства как измененные, поэтому SaveChanges будет отсылать обновление в базу данных аудита.</span><span class="sxs-lookup"><span data-stu-id="71305-250">We then set `Succeeded` and `EndTime`, which marks these properties as modified so SaveChanges will send an update to the audit database.</span></span>

#### <a name="detecting-failure"></a><span data-ttu-id="71305-251">Обнаружение сбоя</span><span class="sxs-lookup"><span data-stu-id="71305-251">Detecting failure</span></span>

<span data-ttu-id="71305-252">Сбой обрабатывается во многом так же, как и успешно, но <xref:Microsoft.EntityFrameworkCore.Diagnostics.ISaveChangesInterceptor.SaveChangesFailed%2A?displayProperty=nameWithType> в <xref:Microsoft.EntityFrameworkCore.Diagnostics.ISaveChangesInterceptor.SaveChangesFailedAsync%2A?displayProperty=nameWithType> методе или.</span><span class="sxs-lookup"><span data-stu-id="71305-252">Failure is handled in much the same way as success, but in the <xref:Microsoft.EntityFrameworkCore.Diagnostics.ISaveChangesInterceptor.SaveChangesFailed%2A?displayProperty=nameWithType> or <xref:Microsoft.EntityFrameworkCore.Diagnostics.ISaveChangesInterceptor.SaveChangesFailedAsync%2A?displayProperty=nameWithType> method.</span></span> <span data-ttu-id="71305-253">Данные события содержат созданное исключение.</span><span class="sxs-lookup"><span data-stu-id="71305-253">The event data contains the exception that was thrown.</span></span>

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

#### <a name="demonstration"></a><span data-ttu-id="71305-254">Демонстрация</span><span class="sxs-lookup"><span data-stu-id="71305-254">Demonstration</span></span>

<span data-ttu-id="71305-255">[Образец аудита](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Miscellaneous/SaveChangesInterception) содержит простое консольное приложение, которое вносит изменения в базу данных блогов, а затем отображает созданный аудит.</span><span class="sxs-lookup"><span data-stu-id="71305-255">The [auditing sample](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Miscellaneous/SaveChangesInterception) contains a simple console application that makes changes to the blogging database and then shows the auditing that was created.</span></span>

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

<span data-ttu-id="71305-256">В результате отобразится содержимое базы данных аудита:</span><span class="sxs-lookup"><span data-stu-id="71305-256">The result shows the contents of the auditing database:</span></span>

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
