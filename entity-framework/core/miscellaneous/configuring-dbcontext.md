---
title: Настройка DbContext-EF Core
description: Стратегии настройки DbContext с помощью Entity Framework Core
author: ajcvickers
ms.date: 10/27/2016
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 3afad8d220acecbb01b15bbb855b52a895e6eb66
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92062026"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="43507-103">Настройка DbContext</span><span class="sxs-lookup"><span data-stu-id="43507-103">Configuring a DbContext</span></span>

<span data-ttu-id="43507-104">В этой статье показаны основные шаблоны для настройки с помощью `DbContext` `DbContextOptions` для подключения к базе данных с помощью определенного поставщика EF Core и необязательных поведений.</span><span class="sxs-lookup"><span data-stu-id="43507-104">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="43507-105">Конфигурация DbContext во время разработки</span><span class="sxs-lookup"><span data-stu-id="43507-105">Design-time DbContext configuration</span></span>

<span data-ttu-id="43507-106">EF Core средства разработки, такие как [Миграция](xref:core/managing-schemas/migrations/index) , должны иметь возможность обнаружения и создания рабочего экземпляра `DbContext` типа для сбора сведений о типах сущностей приложения и их сопоставлении со схемой базы данных.</span><span class="sxs-lookup"><span data-stu-id="43507-106">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="43507-107">Этот процесс может быть автоматическим, если средство может легко создать объект таким `DbContext` образом, чтобы он настроился точно так же, как он будет настроен во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="43507-107">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="43507-108">Хотя любой шаблон, предоставляющий необходимые сведения о конфигурации, `DbContext` может работать во время выполнения, средства, требующие использования `DbContext` во время разработки, могут работать только с ограниченным количеством шаблонов.</span><span class="sxs-lookup"><span data-stu-id="43507-108">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="43507-109">Они подробно рассматриваются в разделе [Создание контекста во время разработки](xref:core/miscellaneous/cli/dbcontext-creation) .</span><span class="sxs-lookup"><span data-stu-id="43507-109">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="43507-110">Настройка DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="43507-110">Configuring DbContextOptions</span></span>

<span data-ttu-id="43507-111">`DbContext` должен иметь экземпляр `DbContextOptions` для выполнения любой работы.</span><span class="sxs-lookup"><span data-stu-id="43507-111">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="43507-112">`DbContextOptions`Экземпляр содержит сведения о конфигурации, такие как:</span><span class="sxs-lookup"><span data-stu-id="43507-112">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="43507-113">Используемый поставщик базы данных, который обычно выбирается путем вызова метода, например `UseSqlServer` или `UseSqlite` .</span><span class="sxs-lookup"><span data-stu-id="43507-113">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="43507-114">Для этих методов расширения требуется соответствующий пакет поставщика, например `Microsoft.EntityFrameworkCore.SqlServer` или `Microsoft.EntityFrameworkCore.Sqlite` .</span><span class="sxs-lookup"><span data-stu-id="43507-114">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="43507-115">Методы определены в `Microsoft.EntityFrameworkCore` пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="43507-115">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="43507-116">Все необходимые строки подключения или идентификаторы экземпляра базы данных, обычно передаваемые в качестве аргумента методу выбора поставщика, упомянутого выше.</span><span class="sxs-lookup"><span data-stu-id="43507-116">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="43507-117">Любые необязательные селекторы поведения на уровне поставщика, обычно также связанные внутри вызова метода выбора поставщика</span><span class="sxs-lookup"><span data-stu-id="43507-117">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="43507-118">Все общие EF Core селекторы поведения, обычно связанные в цепочке после или перед методом выбора поставщика</span><span class="sxs-lookup"><span data-stu-id="43507-118">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="43507-119">В следующем примере выполняется настройка `DbContextOptions` для использования поставщика SQL Server, соединения, содержащегося в `connectionString` переменной, времени ожидания команды на уровне поставщика, и селектора поведения EF Core, который по умолчанию выполняет все запросы, выполняемые в `DbContext` [неотслеживаемом](xref:core/querying/tracking#no-tracking-queries) виде:</span><span class="sxs-lookup"><span data-stu-id="43507-119">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

```csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]
> <span data-ttu-id="43507-120">Методы выбора поставщика и другие методы выбора поведения, упомянутые выше, являются методами расширения для `DbContextOptions` классов параметров или, зависящих от поставщика.</span><span class="sxs-lookup"><span data-stu-id="43507-120">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="43507-121">Чтобы получить доступ к этим методам расширения, вам может потребоваться пространство имен (обычно `Microsoft.EntityFrameworkCore` ) в области и включить в проект дополнительные зависимости пакетов.</span><span class="sxs-lookup"><span data-stu-id="43507-121">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="43507-122">`DbContextOptions`Можно передать в, `DbContext` переопределив `OnConfiguring` метод или извне с помощью аргумента конструктора.</span><span class="sxs-lookup"><span data-stu-id="43507-122">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="43507-123">Если используются оба параметра, `OnConfiguring` применяется последняя и могут перезаписываться параметры, передаваемые в аргумент конструктора.</span><span class="sxs-lookup"><span data-stu-id="43507-123">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="43507-124">Аргумент конструктора</span><span class="sxs-lookup"><span data-stu-id="43507-124">Constructor argument</span></span>

<span data-ttu-id="43507-125">Конструктор может просто принять, `DbContextOptions` как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="43507-125">Your constructor can simply accept a `DbContextOptions` as follows:</span></span>

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
> <span data-ttu-id="43507-126">Базовый конструктор DbContext также принимает не универсальную версию `DbContextOptions` , но использование неуниверсальной версии не рекомендуется для приложений с несколькими типами контекста.</span><span class="sxs-lookup"><span data-stu-id="43507-126">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="43507-127">Теперь приложение может передавать `DbContextOptions` при создании экземпляра контекста следующим образом:</span><span class="sxs-lookup"><span data-stu-id="43507-127">Your application can now pass the `DbContextOptions` when instantiating a context, as follows:</span></span>

```csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="43507-128">Настройка</span><span class="sxs-lookup"><span data-stu-id="43507-128">OnConfiguring</span></span>

<span data-ttu-id="43507-129">Можно также инициализировать в `DbContextOptions` самом контексте.</span><span class="sxs-lookup"><span data-stu-id="43507-129">You can also initialize the `DbContextOptions` within the context itself.</span></span> <span data-ttu-id="43507-130">Хотя этот метод можно использовать для базовой конфигурации, вам, как правило, необходимо получить определенные сведения о конфигурации извне, например строку подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="43507-130">While you can use this technique for basic configuration, you will typically still need to get certain configuration details from the outside, e.g. a database connection string.</span></span> <span data-ttu-id="43507-131">Это можно сделать с помощью API конфигурации или любого другого средства.</span><span class="sxs-lookup"><span data-stu-id="43507-131">This can be done with a configuration API or any other means.</span></span>

<span data-ttu-id="43507-132">Для инициализации `DbContextOptions` в контексте Переопределите `OnConfiguring` метод и вызовите методы для предоставленного `DbContextOptionsBuilder` :</span><span class="sxs-lookup"><span data-stu-id="43507-132">To initialize `DbContextOptions` within the context, override the `OnConfiguring` method and call the methods on the provided `DbContextOptionsBuilder`:</span></span>

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

<span data-ttu-id="43507-133">Приложение может просто создать экземпляр такого контекста, не передавая что-либо в свой конструктор:</span><span class="sxs-lookup"><span data-stu-id="43507-133">An application can simply instantiate such a context without passing anything to its constructor:</span></span>

```csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="43507-134">Этот подход не предназначен для тестирования, если только тесты не нацелены на полную базу данных.</span><span class="sxs-lookup"><span data-stu-id="43507-134">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="43507-135">Использование DbContext с внедрением зависимостей</span><span class="sxs-lookup"><span data-stu-id="43507-135">Using DbContext with dependency injection</span></span>

<span data-ttu-id="43507-136">EF Core поддерживает использование `DbContext` с контейнером внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="43507-136">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="43507-137">Тип DbContext можно добавить в контейнер службы с помощью `AddDbContext<TContext>` метода.</span><span class="sxs-lookup"><span data-stu-id="43507-137">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="43507-138">`AddDbContext<TContext>` сделает тип DbContext, `TContext` и соответствующий объект `DbContextOptions<TContext>` доступным для внедрения из контейнера службы.</span><span class="sxs-lookup"><span data-stu-id="43507-138">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="43507-139">Дополнительные сведения о внедрении [зависимостей см.](#more-reading) ниже.</span><span class="sxs-lookup"><span data-stu-id="43507-139">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="43507-140">Добавление в `DbContext` встраивание зависимостей:</span><span class="sxs-lookup"><span data-stu-id="43507-140">Adding the `DbContext` to dependency injection:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="43507-141">Для этого необходимо добавить [аргумент конструктора](#constructor-argument) в тип DbContext, который принимает `DbContextOptions<TContext>` .</span><span class="sxs-lookup"><span data-stu-id="43507-141">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="43507-142">Код контекста:</span><span class="sxs-lookup"><span data-stu-id="43507-142">Context code:</span></span>

```csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="43507-143">Код приложения (в ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="43507-143">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="43507-144">Код приложения (с использованием метода ServiceProvider напрямую, менее распространенный):</span><span class="sxs-lookup"><span data-stu-id="43507-144">Application code (using ServiceProvider directly, less common):</span></span>

```csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="43507-145">Предотвращение проблем с многопоточной обDbContextкой</span><span class="sxs-lookup"><span data-stu-id="43507-145">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="43507-146">Entity Framework Core не поддерживает выполнение нескольких параллельных операций на одном и том же `DbContext` экземпляре.</span><span class="sxs-lookup"><span data-stu-id="43507-146">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="43507-147">Сюда входит параллельное выполнение асинхронных запросов и любое явное одновременное использование из нескольких потоков.</span><span class="sxs-lookup"><span data-stu-id="43507-147">This includes both parallel execution of async queries and any explicit concurrent use from multiple threads.</span></span> <span data-ttu-id="43507-148">Таким образом, всегда `await` асинхронные вызовы выполняются немедленно или используют отдельные `DbContext` экземпляры для операций, выполняемых параллельно.</span><span class="sxs-lookup"><span data-stu-id="43507-148">Therefore, always `await` async calls immediately, or use separate `DbContext` instances for operations that execute in parallel.</span></span>

<span data-ttu-id="43507-149">Когда EF Core обнаруживает `DbContext` одновременную попытку использования экземпляра, вы увидите `InvalidOperationException` сообщение с таким образом:</span><span class="sxs-lookup"><span data-stu-id="43507-149">When EF Core detects an attempt to use a `DbContext` instance concurrently, you'll see an `InvalidOperationException` with a message like this:</span></span>

> <span data-ttu-id="43507-150">Вторая операция запущена в этом контексте до завершения предыдущей операции.</span><span class="sxs-lookup"><span data-stu-id="43507-150">A second operation started on this context before a previous operation completed.</span></span> <span data-ttu-id="43507-151">Обычно это вызвано различными потоками, использующими один и тот же экземпляр DbContext, однако не гарантируется, что члены экземпляров будут потокобезопасными.</span><span class="sxs-lookup"><span data-stu-id="43507-151">This is usually caused by different threads using the same instance of DbContext, however instance members are not guaranteed to be thread safe.</span></span>

<span data-ttu-id="43507-152">Если одновременный доступ не обнаруживается, это может привести к неопределенному поведению, сбоям приложений и повреждению данных.</span><span class="sxs-lookup"><span data-stu-id="43507-152">When concurrent access goes undetected, it can result in undefined behavior, application crashes and data corruption.</span></span>

<span data-ttu-id="43507-153">Существуют распространенные ошибки, которые могут случайно вызвать параллельный доступ к одному и тому же `DbContext` экземпляру:</span><span class="sxs-lookup"><span data-stu-id="43507-153">There are common mistakes that can inadvertently cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="43507-154">Невозможность ожидания завершения асинхронной операции перед запуском любой другой операции с тем же DbContext</span><span class="sxs-lookup"><span data-stu-id="43507-154">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="43507-155">Асинхронные методы позволяют EF Core инициировать операции, обращающиеся к базе данных без блокировки.</span><span class="sxs-lookup"><span data-stu-id="43507-155">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="43507-156">Но если вызывающий объект не ожидает завершения одного из этих методов и переходит к выполнению других операций в `DbContext` , состояние `DbContext` может быть (и, скорее всего, будет) повреждено.</span><span class="sxs-lookup"><span data-stu-id="43507-156">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span>

<span data-ttu-id="43507-157">Всегда ожидайте EF Core асинхронных методов немедленно.</span><span class="sxs-lookup"><span data-stu-id="43507-157">Always await EF Core asynchronous methods immediately.</span></span>

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="43507-158">Неявное совместное использование экземпляров DbContext в нескольких потоках с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="43507-158">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="43507-159">[`AddDbContext`](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)Метод расширения регистрирует `DbContext` типы с [жизненным циклом в области действия](/aspnet/core/fundamentals/dependency-injection#service-lifetimes) по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="43507-159">The [`AddDbContext`](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span>

<span data-ttu-id="43507-160">Это может быть защищено от проблем с одновременным доступом в большинстве ASP.NET Core приложений, поскольку существует только один поток, выполняющий каждый запрос клиента в определенный момент времени, и поскольку каждый запрос получает отдельную область внедрения зависимостей (и, следовательно, отдельный `DbContext` экземпляр).</span><span class="sxs-lookup"><span data-stu-id="43507-160">This is safe from concurrent access issues in most ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span> <span data-ttu-id="43507-161">Для модели размещения сервера Блазор используется один логический запрос для обслуживания пользовательского канала Блазор, поэтому при использовании области введения по умолчанию для каждого пользовательского канала доступен только один экземпляр DbContext с заданной областью.</span><span class="sxs-lookup"><span data-stu-id="43507-161">For Blazor Server hosting model, one logical request is used for maintaining the Blazor user circuit, and thus only one scoped DbContext instance is available per user circuit if the default injection scope is used.</span></span>

<span data-ttu-id="43507-162">Любой код, явно выполняющий несколько потоков в параллельном режиме, должен обеспечить `DbContext` одновременное обращение к экземплярам.</span><span class="sxs-lookup"><span data-stu-id="43507-162">Any code that explicitly executes multiple threads in parallel should ensure that `DbContext` instances aren't ever accessed concurrently.</span></span>

<span data-ttu-id="43507-163">С помощью внедрения зависимостей можно либо зарегистрировать контекст в области, либо создать области (с помощью `IServiceScopeFactory` ) для каждого потока, либо зарегистрировать `DbContext` как временную (с помощью перегрузки, `AddDbContext` которая принимает `ServiceLifetime` параметр).</span><span class="sxs-lookup"><span data-stu-id="43507-163">Using dependency injection, this can be achieved by either registering the context as scoped, and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="43507-164">Дополнительные материалы</span><span class="sxs-lookup"><span data-stu-id="43507-164">More reading</span></span>

- <span data-ttu-id="43507-165">Дополнительные сведения об использовании DI см. в статье [внедрение зависимостей](/aspnet/core/fundamentals/dependency-injection) .</span><span class="sxs-lookup"><span data-stu-id="43507-165">Read [Dependency Injection](/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
- <span data-ttu-id="43507-166">Дополнительные сведения см. в статье [тестирование](xref:core/miscellaneous/testing/index) .</span><span class="sxs-lookup"><span data-stu-id="43507-166">Read [Testing](xref:core/miscellaneous/testing/index) for more information.</span></span>
