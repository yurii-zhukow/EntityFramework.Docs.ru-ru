---
title: Настройка DbContext-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 734acad86e364abbfd1522fe79d4a847b1acfb52
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149029"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="f7175-102">Настройка DbContext</span><span class="sxs-lookup"><span data-stu-id="f7175-102">Configuring a DbContext</span></span>

<span data-ttu-id="f7175-103">В этой статье показаны основные шаблоны для настройки `DbContext` с `DbContextOptions` помощью для подключения к базе данных с помощью определенного поставщика EF Core и необязательных поведений.</span><span class="sxs-lookup"><span data-stu-id="f7175-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="f7175-104">Конфигурация DbContext во время разработки</span><span class="sxs-lookup"><span data-stu-id="f7175-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="f7175-105">EF Core средства разработки, такие как [Миграция](xref:core/managing-schemas/migrations/index) , должны иметь возможность обнаружения и создания рабочего экземпляра `DbContext` типа для сбора сведений о типах сущностей приложения и их сопоставлении со схемой базы данных.</span><span class="sxs-lookup"><span data-stu-id="f7175-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="f7175-106">Этот процесс может быть автоматическим, если средство может легко создать объект `DbContext` таким образом, чтобы он настроился точно так же, как он будет настроен во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="f7175-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="f7175-107">Хотя любой шаблон, предоставляющий необходимые сведения о `DbContext` конфигурации, может работать во время выполнения, средства, требующие `DbContext` использования во время разработки, могут работать только с ограниченным количеством шаблонов.</span><span class="sxs-lookup"><span data-stu-id="f7175-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="f7175-108">Они подробно рассматриваются в разделе [Создание контекста во время разработки](xref:core/miscellaneous/cli/dbcontext-creation) .</span><span class="sxs-lookup"><span data-stu-id="f7175-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="f7175-109">Настройка DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="f7175-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="f7175-110">`DbContext`должен иметь экземпляр `DbContextOptions` для выполнения любой работы.</span><span class="sxs-lookup"><span data-stu-id="f7175-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="f7175-111">`DbContextOptions` Экземпляр содержит сведения о конфигурации, такие как:</span><span class="sxs-lookup"><span data-stu-id="f7175-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="f7175-112">Используемый поставщик базы данных, который обычно выбирается путем вызова метода, например `UseSqlServer` или. `UseSqlite`</span><span class="sxs-lookup"><span data-stu-id="f7175-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="f7175-113">Для этих методов расширения требуется соответствующий пакет поставщика, например `Microsoft.EntityFrameworkCore.SqlServer` или. `Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="f7175-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="f7175-114">Методы определены в `Microsoft.EntityFrameworkCore` пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="f7175-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="f7175-115">Все необходимые строки подключения или идентификаторы экземпляра базы данных, обычно передаваемые в качестве аргумента методу выбора поставщика, упомянутого выше.</span><span class="sxs-lookup"><span data-stu-id="f7175-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="f7175-116">Любые необязательные селекторы поведения на уровне поставщика, обычно также связанные внутри вызова метода выбора поставщика</span><span class="sxs-lookup"><span data-stu-id="f7175-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="f7175-117">Все общие EF Core селекторы поведения, обычно связанные в цепочке после или перед методом выбора поставщика</span><span class="sxs-lookup"><span data-stu-id="f7175-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="f7175-118">В следующем примере выполняется настройка `DbContextOptions` для использования поставщика SQL Server, соединения, содержащегося `connectionString` в переменной, времени ожидания команды на уровне поставщика, а также селектора поведения EF Core, который делает все запросы, `DbContext` выполняемые в [без отслеживания](xref:core/querying/tracking#no-tracking-queries) по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="f7175-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="f7175-119">Методы выбора поставщика и другие методы выбора поведения, упомянутые выше, являются `DbContextOptions` методами расширения для классов параметров или, зависящих от поставщика.</span><span class="sxs-lookup"><span data-stu-id="f7175-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="f7175-120">Чтобы получить доступ к этим методам расширения, вам может потребоваться пространство имен (обычно `Microsoft.EntityFrameworkCore`) в области и включить в проект дополнительные зависимости пакетов.</span><span class="sxs-lookup"><span data-stu-id="f7175-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="f7175-121">Можно передать в, `DbContext` переопределив `OnConfiguring` метод или извне с помощью аргумента конструктора. `DbContextOptions`</span><span class="sxs-lookup"><span data-stu-id="f7175-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="f7175-122">Если используются оба параметра, `OnConfiguring` применяется последняя и могут перезаписываться параметры, передаваемые в аргумент конструктора.</span><span class="sxs-lookup"><span data-stu-id="f7175-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="f7175-123">Аргумент конструктора</span><span class="sxs-lookup"><span data-stu-id="f7175-123">Constructor argument</span></span>

<span data-ttu-id="f7175-124">Контекстный код с конструктором:</span><span class="sxs-lookup"><span data-stu-id="f7175-124">Context code with constructor:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

> [!TIP]  
> <span data-ttu-id="f7175-125">Базовый конструктор DbContext также принимает не универсальную версию `DbContextOptions`, но использование неуниверсальной версии не рекомендуется для приложений с несколькими типами контекста.</span><span class="sxs-lookup"><span data-stu-id="f7175-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="f7175-126">Код приложения для инициализации из аргумента конструктора:</span><span class="sxs-lookup"><span data-stu-id="f7175-126">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="f7175-127">Настройка</span><span class="sxs-lookup"><span data-stu-id="f7175-127">OnConfiguring</span></span>

<span data-ttu-id="f7175-128">Контекстный код `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="f7175-128">Context code with `OnConfiguring`:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite("Data Source=blog.db");
    }
}
```

<span data-ttu-id="f7175-129">Код приложения для инициализации, `DbContext` в котором `OnConfiguring`используется:</span><span class="sxs-lookup"><span data-stu-id="f7175-129">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="f7175-130">Этот подход не предназначен для тестирования, если только тесты не нацелены на полную базу данных.</span><span class="sxs-lookup"><span data-stu-id="f7175-130">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="f7175-131">Использование DbContext с внедрением зависимостей</span><span class="sxs-lookup"><span data-stu-id="f7175-131">Using DbContext with dependency injection</span></span>

<span data-ttu-id="f7175-132">EF Core поддерживает использование `DbContext` с контейнером внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="f7175-132">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="f7175-133">Тип DbContext можно добавить в контейнер службы с помощью `AddDbContext<TContext>` метода.</span><span class="sxs-lookup"><span data-stu-id="f7175-133">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="f7175-134">`AddDbContext<TContext>`сделает тип DbContext, `TContext`и соответствующий `DbContextOptions<TContext>` объект доступным для внедрения из контейнера службы.</span><span class="sxs-lookup"><span data-stu-id="f7175-134">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="f7175-135">Дополнительные сведения о внедрении [зависимостей см.](#more-reading) ниже.</span><span class="sxs-lookup"><span data-stu-id="f7175-135">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="f7175-136">Добавление в `DbContext` встраивание зависимостей:</span><span class="sxs-lookup"><span data-stu-id="f7175-136">Adding the `DbContext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="f7175-137">Для этого необходимо добавить [аргумент конструктора](#constructor-argument) в тип DbContext, который принимает `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="f7175-137">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="f7175-138">Код контекста:</span><span class="sxs-lookup"><span data-stu-id="f7175-138">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="f7175-139">Код приложения (в ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="f7175-139">Application code (in ASP.NET Core):</span></span>

``` csharp
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

<span data-ttu-id="f7175-140">Код приложения (с использованием метода ServiceProvider напрямую, менее распространенный):</span><span class="sxs-lookup"><span data-stu-id="f7175-140">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="f7175-141">Предотвращение проблем с многопоточной обDbContextкой</span><span class="sxs-lookup"><span data-stu-id="f7175-141">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="f7175-142">Entity Framework Core не поддерживает выполнение нескольких параллельных операций на одном и том `DbContext` же экземпляре.</span><span class="sxs-lookup"><span data-stu-id="f7175-142">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="f7175-143">Сюда входит параллельное выполнение асинхронных запросов и любое явное одновременное использование из нескольких потоков.</span><span class="sxs-lookup"><span data-stu-id="f7175-143">This includes both parallel execution of async queries and any explicit concurrent use from multiple threads.</span></span> <span data-ttu-id="f7175-144">Таким образом, `await` всегда асинхронные вызовы выполняются немедленно или `DbContext` используют отдельные экземпляры для операций, выполняемых параллельно.</span><span class="sxs-lookup"><span data-stu-id="f7175-144">Therefore, always `await` async calls immediately, or use separate `DbContext` instances for operations that execute in parallel.</span></span>

<span data-ttu-id="f7175-145">Когда EF Core обнаруживает одновременную попытку `DbContext` использования экземпляра, вы `InvalidOperationException` увидите сообщение с таким образом:</span><span class="sxs-lookup"><span data-stu-id="f7175-145">When EF Core detects an attempt to use a `DbContext` instance concurrently, you'll see an `InvalidOperationException` with a message like this:</span></span> 

> <span data-ttu-id="f7175-146">Вторая операция запущена в этом контексте до завершения предыдущей операции.</span><span class="sxs-lookup"><span data-stu-id="f7175-146">A second operation started on this context before a previous operation completed.</span></span> <span data-ttu-id="f7175-147">Обычно это вызвано различными потоками, использующими один и тот же экземпляр DbContext, однако не гарантируется, что члены экземпляров будут потокобезопасными.</span><span class="sxs-lookup"><span data-stu-id="f7175-147">This is usually caused by different threads using the same instance of DbContext, however instance members are not guaranteed to be thread safe.</span></span>

<span data-ttu-id="f7175-148">Если одновременный доступ не обнаруживается, это может привести к неопределенному поведению, сбоям приложений и повреждению данных.</span><span class="sxs-lookup"><span data-stu-id="f7175-148">When concurrent access goes undetected, it can result in undefined behavior, application crashes and data corruption.</span></span>

<span data-ttu-id="f7175-149">Существуют распространенные ошибки, которые могут случайно вызвать параллельный доступ к одному и `DbContext` тому же экземпляру:</span><span class="sxs-lookup"><span data-stu-id="f7175-149">There are common mistakes that can inadvertently cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="f7175-150">Невозможность ожидания завершения асинхронной операции перед запуском любой другой операции с тем же DbContext</span><span class="sxs-lookup"><span data-stu-id="f7175-150">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="f7175-151">Асинхронные методы позволяют EF Core инициировать операции, обращающиеся к базе данных без блокировки.</span><span class="sxs-lookup"><span data-stu-id="f7175-151">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="f7175-152">Но если вызывающий объект не ожидает завершения одного из этих методов и переходит к выполнению других операций `DbContext`в, состояние `DbContext` может быть (и, скорее всего, будет) повреждено.</span><span class="sxs-lookup"><span data-stu-id="f7175-152">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span> 

<span data-ttu-id="f7175-153">Всегда ожидайте EF Core асинхронных методов немедленно.</span><span class="sxs-lookup"><span data-stu-id="f7175-153">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="f7175-154">Неявное совместное использование экземпляров DbContext в нескольких потоках с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="f7175-154">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="f7175-155">Метод расширения регистрирует `DbContext` типы с [жизненным циклом в области действия](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) по умолчанию. [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)</span><span class="sxs-lookup"><span data-stu-id="f7175-155">The [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span> 

<span data-ttu-id="f7175-156">Это может быть связано с проблемами параллельного доступа в ASP.NET Core приложениях, поскольку существует только один поток, выполняющий каждый клиентский запрос в определенный момент времени, и поскольку каждый запрос получает отдельную область внедрения зависимостей ( `DbContext` и, следовательно, отдельный элемент экземпляр).</span><span class="sxs-lookup"><span data-stu-id="f7175-156">This is safe from concurrent access issues in ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span>

<span data-ttu-id="f7175-157">Однако любой код, который явным образом выполняет несколько потоков в параллельном режиме `DbContext` , должен обеспечить одновременное обращение к экземплярам.</span><span class="sxs-lookup"><span data-stu-id="f7175-157">However any code that explicitly executes multiple threads in parallel should ensure that `DbContext` instances aren't ever accessed concurrently.</span></span>

<span data-ttu-id="f7175-158">С помощью внедрения зависимостей это можно сделать, зарегистрировав контекст в области видимости и создавая области (с помощью `IServiceScopeFactory`) для каждого потока или `DbContext` зарегистрировав как временную `AddDbContext` (с помощью перегрузки, которая принимает `ServiceLifetime` параметр).</span><span class="sxs-lookup"><span data-stu-id="f7175-158">Using dependency injection, this can be achieved by either registering the context as scoped and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="f7175-159">Дополнительные материалы</span><span class="sxs-lookup"><span data-stu-id="f7175-159">More reading</span></span>

* <span data-ttu-id="f7175-160">Дополнительные сведения об использовании DI см. в статье [внедрение зависимостей](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) .</span><span class="sxs-lookup"><span data-stu-id="f7175-160">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="f7175-161">Дополнительные сведения см. в статье [тестирование](testing/index.md) .</span><span class="sxs-lookup"><span data-stu-id="f7175-161">Read [Testing](testing/index.md) for more information.</span></span>
