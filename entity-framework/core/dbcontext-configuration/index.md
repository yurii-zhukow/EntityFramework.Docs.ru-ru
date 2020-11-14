---
title: Настройка DbContext в EF Core
description: Принципы настройки экземпляров DbContext с помощью Entity Framework Core
author: ajcvickers
ms.date: 10/27/2016
uid: core/dbcontext-configuration/index
ms.openlocfilehash: 179cb1e4ff7a433c13677ec3f1e457de96004489
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94431148"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="5dba4-103">Настройка DbContext</span><span class="sxs-lookup"><span data-stu-id="5dba4-103">Configuring a DbContext</span></span>

<span data-ttu-id="5dba4-104">В этой статье показаны основные шаблоны настройки `DbContext` с помощью `DbContextOptions` для подключения к базе данных с использованием определенного поставщика EF Core и необязательных расширений функциональности.</span><span class="sxs-lookup"><span data-stu-id="5dba4-104">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="5dba4-105">Настройка DbContext во время разработки</span><span class="sxs-lookup"><span data-stu-id="5dba4-105">Design-time DbContext configuration</span></span>

<span data-ttu-id="5dba4-106">Средства разработки EF Core, такие как средства [миграции](xref:core/managing-schemas/migrations/index), должны иметь возможность обнаруживать и создавать рабочие экземпляры типа `DbContext` для сбора сведений о типах сущностей приложения и их сопоставления со схемой базы данных.</span><span class="sxs-lookup"><span data-stu-id="5dba4-106">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="5dba4-107">Этот процесс можно автоматизировать, так как средство может без труда создать `DbContext` с теми же настройками, которые заданы во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="5dba4-107">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="5dba4-108">Хотя любой шаблон, предоставляющий `DbContext` необходимые сведения о конфигурации, может работать во время выполнения, средства, требующие использования `DbContext` во время разработки, можно использовать только с ограниченным количеством шаблонов.</span><span class="sxs-lookup"><span data-stu-id="5dba4-108">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="5dba4-109">Дополнительные сведения об этом см. в статье [Создание DbContext во время разработки](xref:core/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="5dba4-109">These are covered in more detail in the [Design-Time Context Creation](xref:core/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="5dba4-110">Настройка DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="5dba4-110">Configuring DbContextOptions</span></span>

<span data-ttu-id="5dba4-111">`DbContext` для выполнения любых операций требуется экземпляр `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="5dba4-111">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="5dba4-112">Экземпляр `DbContextOptions` содержит сведения о конфигурации, например:</span><span class="sxs-lookup"><span data-stu-id="5dba4-112">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="5dba4-113">Сведения об используемом поставщике базы данных. Как правило, для выбора поставщика вызывается метод, например `UseSqlServer` или `UseSqlite`.</span><span class="sxs-lookup"><span data-stu-id="5dba4-113">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="5dba4-114">Для этих методов расширения требуется соответствующий пакет поставщика, например `Microsoft.EntityFrameworkCore.SqlServer` или `Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="5dba4-114">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="5dba4-115">Эти методы определены в пространстве имен `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="5dba4-115">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="5dba4-116">Необходимая строка подключения или идентификатор экземпляра базы данных, которые обычно передаются в виде аргумента методу выбора поставщика, упомянутого выше.</span><span class="sxs-lookup"><span data-stu-id="5dba4-116">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="5dba4-117">Сведения обо всех необязательных селекторах поведения на уровне поставщика, которые обычно также объединены в цепочку внутри вызова метода выбора поставщика.</span><span class="sxs-lookup"><span data-stu-id="5dba4-117">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="5dba4-118">Сведения обо всех общих селекторах поведения EF Core, которые обычно объединяются в цепочку после метода выбора поставщика или перед ним.</span><span class="sxs-lookup"><span data-stu-id="5dba4-118">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="5dba4-119">В приведенном ниже примере для `DbContextOptions` заданы такие настройки: поставщик SQL Server; подключение, переданное в переменной `connectionString`; время ожидания для команды на уровне поставщика и селектор поведения EF Core, который делает все запросы, выполняемые в `DbContext`, [неотслеживаемыми](xref:core/querying/tracking#no-tracking-queries) по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5dba4-119">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

```csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]
> <span data-ttu-id="5dba4-120">Методы селектора поставщика и другие методы селектора поведения, упомянутые выше, являются методами расширения в `DbContextOptions` или классах параметров определенного поставщика.</span><span class="sxs-lookup"><span data-stu-id="5dba4-120">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="5dba4-121">Чтобы получить доступ к этим методам расширения, возможно, вам потребуется добавить пространство имен (обычно `Microsoft.EntityFrameworkCore`) в область и включить в проект дополнительные зависимости пакетов.</span><span class="sxs-lookup"><span data-stu-id="5dba4-121">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="5dba4-122">`DbContextOptions` можно предоставить `DbContext`, переопределив метод `OnConfiguring`, или предоставить извне с помощью аргумента конструктора.</span><span class="sxs-lookup"><span data-stu-id="5dba4-122">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="5dba4-123">Если используются оба способа, `OnConfiguring` применяется последним и может перезаписывать параметры, передаваемые в аргумент конструктора.</span><span class="sxs-lookup"><span data-stu-id="5dba4-123">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="5dba4-124">Аргумент конструктора</span><span class="sxs-lookup"><span data-stu-id="5dba4-124">Constructor argument</span></span>

<span data-ttu-id="5dba4-125">Конструктор может просто принять `DbContextOptions` следующим образом:</span><span class="sxs-lookup"><span data-stu-id="5dba4-125">Your constructor can simply accept a `DbContextOptions` as follows:</span></span>

```csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

> [!TIP]
> <span data-ttu-id="5dba4-126">Базовый конструктор DbContext также принимает неуниверсальную версию `DbContextOptions`. Но не следует использовать такую версию для приложений с несколькими типами контекста.</span><span class="sxs-lookup"><span data-stu-id="5dba4-126">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="5dba4-127">Теперь приложение может передать `DbContextOptions` при создании экземпляра контекста следующим образом:</span><span class="sxs-lookup"><span data-stu-id="5dba4-127">Your application can now pass the `DbContextOptions` when instantiating a context, as follows:</span></span>

```csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="5dba4-128">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="5dba4-128">OnConfiguring</span></span>

<span data-ttu-id="5dba4-129">Кроме того, вы можете инициализировать `DbContextOptions` в самом контексте.</span><span class="sxs-lookup"><span data-stu-id="5dba4-129">You can also initialize the `DbContextOptions` within the context itself.</span></span> <span data-ttu-id="5dba4-130">Этот способ можно применить для базовой конфигурации но, как правило, все равно потребуется получить определенные сведения о конфигурации извне, например строку подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="5dba4-130">While you can use this technique for basic configuration, you will typically still need to get certain configuration details from the outside, e.g. a database connection string.</span></span> <span data-ttu-id="5dba4-131">Это можно сделать с помощью API конфигурации или любого другого средства.</span><span class="sxs-lookup"><span data-stu-id="5dba4-131">This can be done with a configuration API or any other means.</span></span>

<span data-ttu-id="5dba4-132">Чтобы инициализировать `DbContextOptions` в контексте, переопределите метод `OnConfiguring` и вызовите методы для предоставленного `DbContextOptionsBuilder`:</span><span class="sxs-lookup"><span data-stu-id="5dba4-132">To initialize `DbContextOptions` within the context, override the `OnConfiguring` method and call the methods on the provided `DbContextOptionsBuilder`:</span></span>

```csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite("Data Source=blog.db");
    }
}
```

<span data-ttu-id="5dba4-133">Приложение может без труда создать экземпляр такого контекста, не передавая какие-либо данные в свой конструктор:</span><span class="sxs-lookup"><span data-stu-id="5dba4-133">An application can simply instantiate such a context without passing anything to its constructor:</span></span>

```csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="5dba4-134">Этот подход не предназначен для тестирования, если только тесты не ориентированы на проверку работы всей базы данных.</span><span class="sxs-lookup"><span data-stu-id="5dba4-134">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="5dba4-135">Использование DbContext с внедрением зависимостей</span><span class="sxs-lookup"><span data-stu-id="5dba4-135">Using DbContext with dependency injection</span></span>

<span data-ttu-id="5dba4-136">EF Core поддерживает использование `DbContext` с контейнером внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="5dba4-136">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="5dba4-137">Тип DbContext можно добавить в контейнер службы с помощью метода `AddDbContext<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="5dba4-137">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="5dba4-138">`AddDbContext<TContext>` сделает тип DbContext (`TContext`) и соответствующий тип `DbContextOptions<TContext>` доступными для внедрения из контейнера службы.</span><span class="sxs-lookup"><span data-stu-id="5dba4-138">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="5dba4-139">Дополнительные сведения о внедрении зависимостей см. в статьях, ссылки на которые приведены в разделе [Дополнительные материалы](#more-reading).</span><span class="sxs-lookup"><span data-stu-id="5dba4-139">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="5dba4-140">Добавление `DbContext` в код внедрения зависимостей:</span><span class="sxs-lookup"><span data-stu-id="5dba4-140">Adding the `DbContext` to dependency injection:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="5dba4-141">Вам потребуется добавить [аргумент конструктора](#constructor-argument) в тип DbContext, который принимает `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="5dba4-141">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="5dba4-142">Код контекста:</span><span class="sxs-lookup"><span data-stu-id="5dba4-142">Context code:</span></span>

```csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="5dba4-143">Код приложения (в ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="5dba4-143">Application code (in ASP.NET Core):</span></span>

```csharp
public class MyController
{
    private readonly BloggingContext _context;

    public MyController(BloggingContext context)
    {
      _context = context;
    }

    ...
}
```

<span data-ttu-id="5dba4-144">Код приложения с использованием ServiceProvider напрямую (менее распространенный способ):</span><span class="sxs-lookup"><span data-stu-id="5dba4-144">Application code (using ServiceProvider directly, less common):</span></span>

```csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="5dba4-145">Предотвращение проблем с потоками DbContext</span><span class="sxs-lookup"><span data-stu-id="5dba4-145">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="5dba4-146">Entity Framework Core не поддерживает выполнение нескольких параллельных операций в одном экземпляре `DbContext`,</span><span class="sxs-lookup"><span data-stu-id="5dba4-146">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="5dba4-147">включая параллельное выполнение асинхронных запросов и любое явное использование экземпляра из нескольких потоков одновременно.</span><span class="sxs-lookup"><span data-stu-id="5dba4-147">This includes both parallel execution of async queries and any explicit concurrent use from multiple threads.</span></span> <span data-ttu-id="5dba4-148">Поэтому обязательно сразу же применяйте `await` к асинхронным вызовам или используйте отдельные экземпляры `DbContext`, выполняемые параллельно.</span><span class="sxs-lookup"><span data-stu-id="5dba4-148">Therefore, always `await` async calls immediately, or use separate `DbContext` instances for operations that execute in parallel.</span></span>

<span data-ttu-id="5dba4-149">Когда EF Core обнаруживает попытку одновременного использования экземпляра `DbContext`, отобразится `InvalidOperationException` с примерно таким сообщением:</span><span class="sxs-lookup"><span data-stu-id="5dba4-149">When EF Core detects an attempt to use a `DbContext` instance concurrently, you'll see an `InvalidOperationException` with a message like this:</span></span>

> <span data-ttu-id="5dba4-150">В этом контексте была начата вторая операция до завершения предыдущей операции.</span><span class="sxs-lookup"><span data-stu-id="5dba4-150">A second operation started on this context before a previous operation completed.</span></span> <span data-ttu-id="5dba4-151">Как правило, так происходит, когда несколько потоков используют один и тот же экземпляр DbContext. Но потокобезопасность членов экземпляров не гарантируется.</span><span class="sxs-lookup"><span data-stu-id="5dba4-151">This is usually caused by different threads using the same instance of DbContext, however instance members are not guaranteed to be thread safe.</span></span>

<span data-ttu-id="5dba4-152">Если одновременный доступ не обнаруживается, это может привести к неопределенному поведению, аварийному завершению работы приложения и повреждению данных.</span><span class="sxs-lookup"><span data-stu-id="5dba4-152">When concurrent access goes undetected, it can result in undefined behavior, application crashes and data corruption.</span></span>

<span data-ttu-id="5dba4-153">Есть распространенные ошибки, из-за которых может случайно возникнуть одновременный доступ к одному и тому же экземпляру `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="5dba4-153">There are common mistakes that can inadvertently cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="5dba4-154">Запуск другой асинхронной операции с одним и тем же экземпляром до завершения предыдущей операции</span><span class="sxs-lookup"><span data-stu-id="5dba4-154">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="5dba4-155">Асинхронные методы позволяют EF Core инициировать операции, обращающиеся к базе данных, без блокировки.</span><span class="sxs-lookup"><span data-stu-id="5dba4-155">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="5dba4-156">Но если вызывающий объект не ожидает завершения одного из этих методов и переходит к выполнению других операций с `DbContext`, структура `DbContext` может быть (и, скорее всего, будет) повреждена.</span><span class="sxs-lookup"><span data-stu-id="5dba4-156">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span>

<span data-ttu-id="5dba4-157">Обязательно сразу же применяйте await к асинхронным методам EF Core.</span><span class="sxs-lookup"><span data-stu-id="5dba4-157">Always await EF Core asynchronous methods immediately.</span></span>

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="5dba4-158">Неявное совместное использование экземпляров DbContext в нескольких потоках путем внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="5dba4-158">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="5dba4-159">Метод расширения [`AddDbContext`](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) по умолчанию регистрирует типы `DbContext` с заданной [областью времени существования](/aspnet/core/fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="5dba4-159">The [`AddDbContext`](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span>

<span data-ttu-id="5dba4-160">Это обеспечивает защиту от проблем с одновременным доступом в большинстве приложений ASP.NET Core, так как есть только один поток, в котором каждый запрос клиента выполняется в определенный момент времени, и каждый такой запрос получает отдельную область внедрения зависимостей (и, следовательно, отдельный экземпляр `DbContext`).</span><span class="sxs-lookup"><span data-stu-id="5dba4-160">This is safe from concurrent access issues in most ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span> <span data-ttu-id="5dba4-161">В модели размещения Blazor Server используется один логический запрос для обслуживания пользовательского канала Blazor. Поэтому при использовании области внедрения по умолчанию для каждого пользовательского канала доступен только один экземпляр DbContext с заданной областью.</span><span class="sxs-lookup"><span data-stu-id="5dba4-161">For Blazor Server hosting model, one logical request is used for maintaining the Blazor user circuit, and thus only one scoped DbContext instance is available per user circuit if the default injection scope is used.</span></span>

<span data-ttu-id="5dba4-162">Если код явно выполняет несколько потоков в параллельном режиме, нужно исключить возможность одновременного доступа к экземплярам `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="5dba4-162">Any code that explicitly executes multiple threads in parallel should ensure that `DbContext` instances aren't ever accessed concurrently.</span></span>

<span data-ttu-id="5dba4-163">При использовании внедрения зависимостей это можно реализовать путем регистрации контекста как ограниченного областью, создания области (с помощью `IServiceScopeFactory`) для каждого потока или регистрации экземпляра `DbContext` как временного (с помощью перегрузки `AddDbContext`, при которой принимается параметр `ServiceLifetime`).</span><span class="sxs-lookup"><span data-stu-id="5dba4-163">Using dependency injection, this can be achieved by either registering the context as scoped, and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="5dba4-164">Дополнительные материалы</span><span class="sxs-lookup"><span data-stu-id="5dba4-164">More reading</span></span>

- <span data-ttu-id="5dba4-165">Дополнительные сведения о внедрении зависимостей см. [здесь](/aspnet/core/fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="5dba4-165">Read [Dependency Injection](/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
- <span data-ttu-id="5dba4-166">Подробную информацию см. в статье [Тестирование кода, использующего EF Core](xref:core/testing/index).</span><span class="sxs-lookup"><span data-stu-id="5dba4-166">Read [Testing](xref:core/testing/index) for more information.</span></span>
