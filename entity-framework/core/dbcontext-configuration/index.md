---
title: Время существования, настройка и инициализация DbContext (EF Core)
description: Шаблоны для создания экземпляров DbContext с внедрением зависимостей или без него, а также управление такими экземплярами
author: ajcvickers
ms.date: 11/07/2020
uid: core/dbcontext-configuration/index
ms.openlocfilehash: 10ed474df2c4c52e61083d9d671909be02cd8cef
ms.sourcegitcommit: 032a1767d7a6e42052a005f660b80372c6521e7e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98129035"
---
# <a name="dbcontext-lifetime-configuration-and-initialization"></a><span data-ttu-id="1248c-103">Время существования, настройка и инициализация DbContext</span><span class="sxs-lookup"><span data-stu-id="1248c-103">DbContext Lifetime, Configuration, and Initialization</span></span>

<span data-ttu-id="1248c-104">В этой статье описаны основные шаблоны для инициализации и настройки экземпляра <xref:Microsoft.EntityFrameworkCore.DbContext>.</span><span class="sxs-lookup"><span data-stu-id="1248c-104">This article shows basic patterns for initialization and configuration of a <xref:Microsoft.EntityFrameworkCore.DbContext> instance.</span></span>

## <a name="the-dbcontext-lifetime"></a><span data-ttu-id="1248c-105">Время существования DbContext</span><span class="sxs-lookup"><span data-stu-id="1248c-105">The DbContext lifetime</span></span>

<span data-ttu-id="1248c-106">Время существования `DbContext` начинается, когда создается экземпляр, и заканчивается, когда экземпляр [удаляется](/dotnet/standard/garbage-collection/unmanaged).</span><span class="sxs-lookup"><span data-stu-id="1248c-106">The lifetime of a `DbContext` begins when the instance is created and ends when the instance is [disposed](/dotnet/standard/garbage-collection/unmanaged).</span></span> <span data-ttu-id="1248c-107">Экземпляр `DbContext` предназначен для выполнения _одной_ [единицы работы](https://www.martinfowler.com/eaaCatalog/unitOfWork.html).</span><span class="sxs-lookup"><span data-stu-id="1248c-107">A `DbContext` instance is designed to be used for a _single_ [unit-of-work](https://www.martinfowler.com/eaaCatalog/unitOfWork.html).</span></span> <span data-ttu-id="1248c-108">Это означает, что время существования экземпляра `DbContext` обычно невелико.</span><span class="sxs-lookup"><span data-stu-id="1248c-108">This means that the lifetime of a `DbContext` instance is usually very short.</span></span>

> [!TIP]
> <span data-ttu-id="1248c-109">Цитируя Мартина Фаулера (по ссылке выше): "С помощью единицы работы отслеживаются все операции во время бизнес-транзакции, которые могут повлиять на базу данных.</span><span class="sxs-lookup"><span data-stu-id="1248c-109">To quote Martin Fowler from the link above, "A Unit of Work keeps track of everything you do during a business transaction that can affect the database.</span></span> <span data-ttu-id="1248c-110">Когда вы закончите работу, такая единица определяет, что нужно сделать, чтобы изменить базу данных в результате ваших действий".</span><span class="sxs-lookup"><span data-stu-id="1248c-110">When you're done, it figures out everything that needs to be done to alter the database as a result of your work."</span></span>

<span data-ttu-id="1248c-111">Обычная единица работы при использовании Entity Framework Core (EF Core) включает следующее:</span><span class="sxs-lookup"><span data-stu-id="1248c-111">A typical unit-of-work when using Entity Framework Core (EF Core) involves:</span></span>

- <span data-ttu-id="1248c-112">Создание экземпляра `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1248c-112">Creation of a `DbContext` instance</span></span>
- <span data-ttu-id="1248c-113">Отслеживание экземпляров сущности по контексту.</span><span class="sxs-lookup"><span data-stu-id="1248c-113">Tracking of entity instances by the context.</span></span> <span data-ttu-id="1248c-114">Сущности отслеживаются, когда они:</span><span class="sxs-lookup"><span data-stu-id="1248c-114">Entities become tracked by</span></span>
  - <span data-ttu-id="1248c-115">[возвращаются запросом;](xref:core/querying/tracking)</span><span class="sxs-lookup"><span data-stu-id="1248c-115">Being [returned from a query](xref:core/querying/tracking)</span></span>
  - <span data-ttu-id="1248c-116">[добавляются или присоединяются к контексту.](xref:core/saving/disconnected-entities)</span><span class="sxs-lookup"><span data-stu-id="1248c-116">Being [added or attached to the context](xref:core/saving/disconnected-entities)</span></span>
- <span data-ttu-id="1248c-117">При необходимости отслеживаемые сущности изменяются для реализации бизнес-правила.</span><span class="sxs-lookup"><span data-stu-id="1248c-117">Changes are made to the tracked entities as needed to implement the business rule</span></span>
- <span data-ttu-id="1248c-118">Вызывается <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> или <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A>.</span><span class="sxs-lookup"><span data-stu-id="1248c-118"><xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> or <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A> is called.</span></span> <span data-ttu-id="1248c-119">EF Core обнаруживает внесенные изменения и записывает их в базу данных.</span><span class="sxs-lookup"><span data-stu-id="1248c-119">EF Core detects the changes made and writes them to the database.</span></span>
- <span data-ttu-id="1248c-120">Экземпляр `DbContext` удаляется.</span><span class="sxs-lookup"><span data-stu-id="1248c-120">The `DbContext` instance is disposed</span></span>

> [!IMPORTANT]
>
> - <span data-ttu-id="1248c-121">Очень важно также удалить <xref:Microsoft.EntityFrameworkCore.DbContext> после использования.</span><span class="sxs-lookup"><span data-stu-id="1248c-121">It is very important to dispose the <xref:Microsoft.EntityFrameworkCore.DbContext> after use.</span></span> <span data-ttu-id="1248c-122">Это гарантирует, что все неуправляемые ресурсы будут освобождены и что регистрация всех событий или других перехватчиков будет отменена. Так можно предотвратить утечки памяти в тех случаях, если на экземпляр сохраняются ссылки.</span><span class="sxs-lookup"><span data-stu-id="1248c-122">This ensures both that any unmanaged resources are freed, and that any events or other hooks are unregistered so as to prevent memory leaks in case the instance remains referenced.</span></span>
> - <span data-ttu-id="1248c-123">[DbContext **не является потокобезопасным**](#avoiding-dbcontext-threading-issues).</span><span class="sxs-lookup"><span data-stu-id="1248c-123">[DbContext is **not thread-safe**](#avoiding-dbcontext-threading-issues).</span></span> <span data-ttu-id="1248c-124">Не передавайте контексты в другие потоки.</span><span class="sxs-lookup"><span data-stu-id="1248c-124">Do not share contexts between threads.</span></span> <span data-ttu-id="1248c-125">Выполняйте оператор [await](/dotnet/csharp/language-reference/operators/await) для всех вызовов синхронизации, прежде чем продолжать использование экземпляра контекста.</span><span class="sxs-lookup"><span data-stu-id="1248c-125">Make sure to [await](/dotnet/csharp/language-reference/operators/await) all async calls before continuing to use the context instance.</span></span>
> - <span data-ttu-id="1248c-126">Исключение <xref:System.InvalidOperationException>, генероируемое кодом EF Core, может перевести контекст в невосстанавливаемое состояние.</span><span class="sxs-lookup"><span data-stu-id="1248c-126">An <xref:System.InvalidOperationException> thrown by EF Core code can put the context into an unrecoverable state.</span></span> <span data-ttu-id="1248c-127">Такие исключения указывают на ошибку в программе и не допускают восстановление.</span><span class="sxs-lookup"><span data-stu-id="1248c-127">Such exceptions indicate a program error and are not designed to be recovered from.</span></span>

## <a name="dbcontext-in-dependency-injection-for-aspnet-core"></a><span data-ttu-id="1248c-128">DbContext во внедрении зависимостей для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1248c-128">DbContext in dependency injection for ASP.NET Core</span></span>

<span data-ttu-id="1248c-129">Во многих веб-приложениях каждый HTTP-запрос соответствует одной единице работы.</span><span class="sxs-lookup"><span data-stu-id="1248c-129">In many web applications, each HTTP request corresponds to a single unit-of-work.</span></span> <span data-ttu-id="1248c-130">По этой причине в веб-приложениях время существования контекста по умолчанию привязывается ко времени существования запроса.</span><span class="sxs-lookup"><span data-stu-id="1248c-130">This makes tying the context lifetime to that of the request a good default for web applications.</span></span>

<span data-ttu-id="1248c-131">Приложения ASP.NET Core [настраиваются с использованием внедрения зависимостей](/aspnet/core/fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="1248c-131">ASP.NET Core applications are [configured using dependency injection](/aspnet/core/fundamentals/startup).</span></span> <span data-ttu-id="1248c-132">Вы можете добавить EF Core в такую конфигурацию с помощью <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext%2A> в методе [`ConfigureServices`](/aspnet/core/fundamentals/startup#the-configureservices-method) файла `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="1248c-132">EF Core can be added to this configuration using <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext%2A> in the [`ConfigureServices`](/aspnet/core/fundamentals/startup#the-configureservices-method) method of `Startup.cs`.</span></span> <span data-ttu-id="1248c-133">Пример:</span><span class="sxs-lookup"><span data-stu-id="1248c-133">For example:</span></span>

<!--
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddControllers();

            services.AddDbContext<ApplicationDbContext>(
                options => options.UseSqlServer("name=ConnectionStrings:DefaultConnection"));
        }
-->
[!code-csharp[ConfigureServices](../../../samples/core/Miscellaneous/ConfiguringDbContext/WebApp/Startup.cs?name=ConfigureServices)]

<span data-ttu-id="1248c-134">В этом примере регистрируется подкласс `DbContext` с именем `ApplicationDbContext` в качестве службы с заданной областью в поставщике службы приложений ASP.NET Core (т. е. в</span><span class="sxs-lookup"><span data-stu-id="1248c-134">This example registers a `DbContext` subclass called `ApplicationDbContext` as a scoped service in the ASP.NET Core application service provider (a.k.a.</span></span> <span data-ttu-id="1248c-135">контейнере внедрения зависимостей).</span><span class="sxs-lookup"><span data-stu-id="1248c-135">the dependency injection container).</span></span> <span data-ttu-id="1248c-136">Контекст при этом настраивается для использования поставщика базы данных SQL Server и считывания строки подключения из конфигурации ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1248c-136">The context is configured to use the SQL Server database provider and will read the connection string from ASP.NET Core configuration.</span></span> <span data-ttu-id="1248c-137">Обычно не имеет значения, _где_ в `ConfigureServices` выполняется вызов к `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="1248c-137">It typically does not matter _where_ in `ConfigureServices` the call to `AddDbContext` is made.</span></span>

<span data-ttu-id="1248c-138">Класс `ApplicationDbContext` должен предоставить доступ к общедоступному конструктору с помощью параметра `DbContextOptions<ApplicationDbContext>`.</span><span class="sxs-lookup"><span data-stu-id="1248c-138">The `ApplicationDbContext` class must expose a public constructor with a `DbContextOptions<ApplicationDbContext>` parameter.</span></span> <span data-ttu-id="1248c-139">Именно так передается конфигурация контекста из `AddDbContext` в `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1248c-139">This is how context configuration from `AddDbContext` is passed to the `DbContext`.</span></span> <span data-ttu-id="1248c-140">Пример:</span><span class="sxs-lookup"><span data-stu-id="1248c-140">For example:</span></span>

<!--
    public class ApplicationDbContext : DbContext
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
-->
[!code-csharp[ApplicationDbContext](../../../samples/core/Miscellaneous/ConfiguringDbContext/WebApp/ApplicationDbContext.cs?name=ApplicationDbContext)]

<span data-ttu-id="1248c-141">Затем `ApplicationDbContext` можно использовать в контроллерах ASP.NET Core или других службах посредством внедрения конструктора.</span><span class="sxs-lookup"><span data-stu-id="1248c-141">`ApplicationDbContext` can then be used in ASP.NET Core controllers or other services through constructor injection.</span></span> <span data-ttu-id="1248c-142">Пример:</span><span class="sxs-lookup"><span data-stu-id="1248c-142">For example:</span></span>

<!--
    public class MyController
    {
        private readonly ApplicationDbContext _context;

        public MyController(ApplicationDbContext context)
        {
            _context = context;
        }
    }
-->
[!code-csharp[MyController](../../../samples/core/Miscellaneous/ConfiguringDbContext/WebApp/Controllers/MyController.cs?name=MyController)]

<span data-ttu-id="1248c-143">Результатом будет экземпляр `ApplicationDbContext`, созданный для каждого запроса и переданный контроллеру для выполнения единицы работы до удаления при завершении запроса.</span><span class="sxs-lookup"><span data-stu-id="1248c-143">The final result is an `ApplicationDbContext` instance created for each request and passed to the controller to perform a unit-of-work before being disposed when the request ends.</span></span>

<span data-ttu-id="1248c-144">Дополнительные сведения о параметрах конфигурации см. далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="1248c-144">Read further in this article to learn more about configuration options.</span></span> <span data-ttu-id="1248c-145">Кроме того, дополнительные сведения о конфигурации и внедрении зависимостей можно найти в статьях [Запуск приложения в ASP.NET Core](/aspnet/core/fundamentals/startup) и [Внедрение зависимостей в ASP.NET Core](/aspnet/core/fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1248c-145">In addition, see [App startup in ASP.NET Core](/aspnet/core/fundamentals/startup) and [Dependency injection in ASP.NET Core](/aspnet/core/fundamentals/dependency-injection) for more information on configuration and dependency injection in ASP.NET Core.</span></span>

<!-- See also [Using Dependency Injection](TODO) for advanced dependency injection configuration with EF Core. -->

## <a name="simple-dbcontext-initialization-with-new"></a><span data-ttu-id="1248c-146">Простая инициализация DbContext с помощью оператора new</span><span class="sxs-lookup"><span data-stu-id="1248c-146">Simple DbContext initialization with 'new'</span></span>

<span data-ttu-id="1248c-147">Экземпляры `DbContext` можно создать средствами .NET, например с помощью оператора `new` в C#.</span><span class="sxs-lookup"><span data-stu-id="1248c-147">`DbContext` instances can be constructed in the normal .NET way, for example with `new` in C#.</span></span> <span data-ttu-id="1248c-148">Настройку можно выполнить путем переопределения метода `OnConfiguring` или передачи параметров в конструктор.</span><span class="sxs-lookup"><span data-stu-id="1248c-148">Configuration can be performed by overriding the `OnConfiguring` method, or by passing options to the constructor.</span></span> <span data-ttu-id="1248c-149">Пример:</span><span class="sxs-lookup"><span data-stu-id="1248c-149">For example:</span></span>

<!--
    public class ApplicationDbContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Test");
        }
    }
-->
[!code-csharp[ApplicationDbContext](../../../samples/core/Miscellaneous/ConfiguringDbContext/WithNew/ApplicationDbContext.cs?name=ApplicationDbContext)]

<span data-ttu-id="1248c-150">Этот шаблон также упрощает передачу сведений о конфигурации, например строки подключения, через конструктор `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1248c-150">This pattern also makes it easy to pass configuration like the connection string via the `DbContext` constructor.</span></span> <span data-ttu-id="1248c-151">Пример:</span><span class="sxs-lookup"><span data-stu-id="1248c-151">For example:</span></span>

<!--
    public class ApplicationDbContext : DbContext
    {
        private readonly string _connectionString;

        public ApplicationDbContext(string connectionString)
        {
            _connectionString = connectionString;
        }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(_connectionString);
        }
    }
-->
[!code-csharp[ApplicationDbContext](../../../samples/core/Miscellaneous/ConfiguringDbContext/WithNewAndArgs/ApplicationDbContext.cs?name=ApplicationDbContext)]

<span data-ttu-id="1248c-152">Кроме того, вы можете использовать `DbContextOptionsBuilder` для создания объекта `DbContextOptions`, который затем передается в конструктор `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1248c-152">Alternately, `DbContextOptionsBuilder` can be used to create a `DbContextOptions` object that is then passed to the `DbContext` constructor.</span></span> <span data-ttu-id="1248c-153">Это позволяет явно создать `DbContext`, настроенный для внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="1248c-153">This allows a `DbContext` configured for dependency injection to also be constructed explicitly.</span></span> <span data-ttu-id="1248c-154">Например, при использовании `ApplicationDbContext`, определенного для указанных выше веб-приложений ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="1248c-154">For example, when using `ApplicationDbContext` defined for ASP.NET Core web apps above:</span></span>

<!--
    public class ApplicationDbContext : DbContext
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
-->
[!code-csharp[ApplicationDbContext](../../../samples/core/Miscellaneous/ConfiguringDbContext/WebApp/ApplicationDbContext.cs?name=ApplicationDbContext)]

<span data-ttu-id="1248c-155">Вы можете создать `DbContextOptions`, а конструктор может быть вызван явно:</span><span class="sxs-lookup"><span data-stu-id="1248c-155">The `DbContextOptions` can be created and the constructor can be called explicitly:</span></span>

<!--
            var contextOptions = new DbContextOptionsBuilder<ApplicationDbContext>()
                .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Test")
                .Options;

            using var context = new ApplicationDbContext(contextOptions);
-->
[!code-csharp[UseNewForWebApp](../../../samples/core/Miscellaneous/ConfiguringDbContext/WebApp/UseNewForWebApp.cs?name=UseNewForWebApp)]

## <a name="using-a-dbcontext-factory-eg-for-blazor"></a><span data-ttu-id="1248c-156">Использование фабрики DbContext (например, для Blazor)</span><span class="sxs-lookup"><span data-stu-id="1248c-156">Using a DbContext factory (e.g. for Blazor)</span></span>

<span data-ttu-id="1248c-157">Некоторые типы приложений (например, [ASP.NET Core Blazor](/aspnet/core/blazor/)) используют внедрение зависимостей, но не создают область службы, соответствующую нужному времени существования `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1248c-157">Some application types (e.g. [ASP.NET Core Blazor](/aspnet/core/blazor/)) use dependency injection but do not create a service scope that aligns with the desired `DbContext` lifetime.</span></span> <span data-ttu-id="1248c-158">Даже если такое соответствие отсутствует, приложению, возможно, потребуется выполнять несколько единиц работы в рамках такой области,</span><span class="sxs-lookup"><span data-stu-id="1248c-158">Even where such an alignment does exist, the application may need to perform multiple units-of-work within this scope.</span></span> <span data-ttu-id="1248c-159">например в одном HTTP-запросе.</span><span class="sxs-lookup"><span data-stu-id="1248c-159">For example, multiple units-of-work within a single HTTP request.</span></span>

<span data-ttu-id="1248c-160">В таких случаях можно использовать <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContextFactory%2A> для регистрации фабрики для создания экземпляров `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1248c-160">In these cases, <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContextFactory%2A> can be used to register a factory for creation of `DbContext` instances.</span></span> <span data-ttu-id="1248c-161">Пример:</span><span class="sxs-lookup"><span data-stu-id="1248c-161">For example:</span></span>

<!--
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddDbContextFactory<ApplicationDbContext>(options =>
                options.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Test"));
        }
-->
[!code-csharp[ConfigureServices](../../../samples/core/Miscellaneous/ConfiguringDbContext/WithContextFactory/FactoryServicesExample.cs?name=ConfigureServices)]

<span data-ttu-id="1248c-162">Класс `ApplicationDbContext` должен предоставить доступ к общедоступному конструктору с помощью параметра `DbContextOptions<ApplicationDbContext>`.</span><span class="sxs-lookup"><span data-stu-id="1248c-162">The `ApplicationDbContext` class must expose a public constructor with a `DbContextOptions<ApplicationDbContext>` parameter.</span></span> <span data-ttu-id="1248c-163">Такой же шаблон используется в традиционных приложениях ASP.NET Core (см. раздел выше).</span><span class="sxs-lookup"><span data-stu-id="1248c-163">This is the same pattern as used in the traditional ASP.NET Core section above.</span></span>

<!--
    public class ApplicationDbContext : DbContext
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
-->
[!code-csharp[ApplicationDbContext](../../../samples/core/Miscellaneous/ConfiguringDbContext/WebApp/ApplicationDbContext.cs?name=ApplicationDbContext)]

<span data-ttu-id="1248c-164">Затем фабрику `DbContextFactory` можно использовать в других службах посредством внедрения конструктора.</span><span class="sxs-lookup"><span data-stu-id="1248c-164">The `DbContextFactory` factory can then be used in other services through constructor injection.</span></span> <span data-ttu-id="1248c-165">Пример:</span><span class="sxs-lookup"><span data-stu-id="1248c-165">For example:</span></span>

<!--
        private readonly IDbContextFactory<ApplicationDbContext> _contextFactory;

        public MyController(IDbContextFactory<ApplicationDbContext> contextFactory)
        {
            _contextFactory = contextFactory;
        }
-->
[!code-csharp[Construct](../../../samples/core/Miscellaneous/ConfiguringDbContext/WithContextFactory/MyController.cs?name=Construct)]

<span data-ttu-id="1248c-166">Внедренную фабрику затем можно использовать для создания экземпляров DbContext в коде службы.</span><span class="sxs-lookup"><span data-stu-id="1248c-166">The injected factory can then be used to construct DbContext instances in the service code.</span></span> <span data-ttu-id="1248c-167">Пример:</span><span class="sxs-lookup"><span data-stu-id="1248c-167">For example:</span></span>

<!--
        public void DoSomething()
        {
            using (var context = _contextFactory.CreateDbContext())
            {
                // ...
            }
        }
-->
[!code-csharp[DoSomething](../../../samples/core/Miscellaneous/ConfiguringDbContext/WithContextFactory/MyController.cs?name=DoSomething)]

<span data-ttu-id="1248c-168">Обратите внимание, что созданными таким образом экземплярами `DbContext` **не** управляет поставщик служб приложения. Поэтому приложение должно удалять их.</span><span class="sxs-lookup"><span data-stu-id="1248c-168">Notice that the `DbContext` instances created in this way are **not** managed by the application's service provider and therefore must be disposed by the application.</span></span>

<span data-ttu-id="1248c-169">Дополнительные сведения об использовании EF Core с Blazor см. в статье [ASP.NET Core Blazor Server с Entity Framework Core](/aspnet/core/blazor/blazor-server-ef-core).</span><span class="sxs-lookup"><span data-stu-id="1248c-169">See [ASP.NET Core Blazor Server with Entity Framework Core](/aspnet/core/blazor/blazor-server-ef-core) for more information on using EF Core with Blazor.</span></span>

## <a name="dbcontextoptions"></a><span data-ttu-id="1248c-170">DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="1248c-170">DbContextOptions</span></span>

<span data-ttu-id="1248c-171">Начальная точка для всех конфигураций `DbContext` — это <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1248c-171">The starting point for all `DbContext` configuration is <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>.</span></span> <span data-ttu-id="1248c-172">Есть три способа получения этого построителя:</span><span class="sxs-lookup"><span data-stu-id="1248c-172">There are three ways to get this builder:</span></span>

- <span data-ttu-id="1248c-173">В `AddDbContext` и связанных методах.</span><span class="sxs-lookup"><span data-stu-id="1248c-173">In `AddDbContext` and related methods</span></span>
- <span data-ttu-id="1248c-174">В `OnConfiguring`</span><span class="sxs-lookup"><span data-stu-id="1248c-174">In `OnConfiguring`</span></span>
- <span data-ttu-id="1248c-175">При явном создании с помощью оператора `new`.</span><span class="sxs-lookup"><span data-stu-id="1248c-175">Constructed explicitly with `new`</span></span>

<span data-ttu-id="1248c-176">Примеры всех этих вариантов приведены в предыдущих разделах.</span><span class="sxs-lookup"><span data-stu-id="1248c-176">Examples of each of these are shown in the preceding sections.</span></span> <span data-ttu-id="1248c-177">Такую же конфигурацию можно применить независимо от того, каким способом получен построитель.</span><span class="sxs-lookup"><span data-stu-id="1248c-177">The same configuration can be applied regardless of where the builder comes from.</span></span> <span data-ttu-id="1248c-178">Кроме того, `OnConfiguring` вызывается в любом случае и независимо от способа создания контекста.</span><span class="sxs-lookup"><span data-stu-id="1248c-178">In addition, `OnConfiguring` is always called regardless of how the context is constructed.</span></span> <span data-ttu-id="1248c-179">Это означает, что `OnConfiguring` можно использовать для выполнения дополнительной настройки даже при использовании `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="1248c-179">This means `OnConfiguring` can be used to perform additional configuration even when `AddDbContext` is being used.</span></span>

### <a name="configuring-the-database-provider"></a><span data-ttu-id="1248c-180">Настройка поставщика базы данных</span><span class="sxs-lookup"><span data-stu-id="1248c-180">Configuring the database provider</span></span>

<span data-ttu-id="1248c-181">Каждый экземпляр `DbContext` необходимо настроить для использования только одного поставщика баз данных.</span><span class="sxs-lookup"><span data-stu-id="1248c-181">Each `DbContext` instance must be configured to use one and only one database provider.</span></span> <span data-ttu-id="1248c-182">Разные экземпляры подтипа `DbContext` можно использовать с разными поставщиками баз данных, но для отдельного экземпляра нужно назначить только один поставщик. Поставщик базы данных настраивается с помощью специального вызова `Use*`.</span><span class="sxs-lookup"><span data-stu-id="1248c-182">(Different instances of a `DbContext` subtype can be used with different database providers, but a single instance must only use one.) A database provider is configured using a specific `Use*`" call.</span></span> <span data-ttu-id="1248c-183">Например, следующий код позволяет использовать поставщик базы данных SQL Server:</span><span class="sxs-lookup"><span data-stu-id="1248c-183">For example, to use the SQL Server database provider:</span></span>

<!--
    public class ApplicationDbContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Test");
        }
    }
-->
[!code-csharp[ApplicationDbContext](../../../samples/core/Miscellaneous/ConfiguringDbContext/WithNew/ApplicationDbContext.cs?name=ApplicationDbContext)]

<span data-ttu-id="1248c-184">Такие методы `Use*` являются методами расширения, реализованными поставщиком базы данных.</span><span class="sxs-lookup"><span data-stu-id="1248c-184">These `Use*`" methods are extension methods implemented by the database provider.</span></span> <span data-ttu-id="1248c-185">Это означает, что пакет NuGet поставщика базы данных нужно установить до использования метода расширения.</span><span class="sxs-lookup"><span data-stu-id="1248c-185">This means that the database provider NuGet package must be installed before the extension method can be used.</span></span>

> [!TIP]
> <span data-ttu-id="1248c-186">Поставщики базы данных EF Core активно используют [методы расширения](/dotnet/csharp/programming-guide/classes-and-structs/extension-methods).</span><span class="sxs-lookup"><span data-stu-id="1248c-186">EF Core database providers make extensive use of [extension methods](/dotnet/csharp/programming-guide/classes-and-structs/extension-methods).</span></span> <span data-ttu-id="1248c-187">Если компилятор указывает, что метод не найден, убедитесь, что пакет NuGet поставщика установлен и что ваш код включает `using Microsoft.EntityFrameworkCore;`.</span><span class="sxs-lookup"><span data-stu-id="1248c-187">If the compiler indicates that a method cannot be found, then make sure that the provider's NuGet package is installed and that you have `using Microsoft.EntityFrameworkCore;` in your code.</span></span>

<span data-ttu-id="1248c-188">В представленной ниже таблице приведены примеры для популярных поставщиков баз данных.</span><span class="sxs-lookup"><span data-stu-id="1248c-188">The following table contains examples for common database providers.</span></span>

| <span data-ttu-id="1248c-189">Система базы данных</span><span class="sxs-lookup"><span data-stu-id="1248c-189">Database system</span></span>              | <span data-ttu-id="1248c-190">Пример конфигурации</span><span class="sxs-lookup"><span data-stu-id="1248c-190">Example configuration</span></span>                         | <span data-ttu-id="1248c-191">Пакет NuGet</span><span class="sxs-lookup"><span data-stu-id="1248c-191">NuGet package</span></span>
|:-----------------------------|-----------------------------------------------|--------------
| <span data-ttu-id="1248c-192">SQL Server или Azure SQL</span><span class="sxs-lookup"><span data-stu-id="1248c-192">SQL Server or Azure SQL</span></span>      | `.UseSqlServer(connectionString)`             | [<span data-ttu-id="1248c-193">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="1248c-193">Microsoft.EntityFrameworkCore.SqlServer</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)
| <span data-ttu-id="1248c-194">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="1248c-194">Azure Cosmos DB</span></span>              | `.UseCosmos(connectionString, databaseName)`  | [<span data-ttu-id="1248c-195">Microsoft.EntityFrameworkCore.Cosmos</span><span class="sxs-lookup"><span data-stu-id="1248c-195">Microsoft.EntityFrameworkCore.Cosmos</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/)
| <span data-ttu-id="1248c-196">SQLite</span><span class="sxs-lookup"><span data-stu-id="1248c-196">SQLite</span></span>                       | `.UseSqlite(connectionString)`                | [<span data-ttu-id="1248c-197">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="1248c-197">Microsoft.EntityFrameworkCore.Sqlite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/)
| <span data-ttu-id="1248c-198">Выполняющаяся в памяти база данных EF Core</span><span class="sxs-lookup"><span data-stu-id="1248c-198">EF Core in-memory database</span></span>   | `.UseInMemoryDatabase(databaseName)`          | [<span data-ttu-id="1248c-199">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="1248c-199">Microsoft.EntityFrameworkCore.InMemory</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/)
| <span data-ttu-id="1248c-200">PostgreSQL\*</span><span class="sxs-lookup"><span data-stu-id="1248c-200">PostgreSQL\*</span></span>                  | `.UseNpgsql(connectionString)`                | [<span data-ttu-id="1248c-201">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="1248c-201">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL/)
| <span data-ttu-id="1248c-202">MySQL или MariaDB\*</span><span class="sxs-lookup"><span data-stu-id="1248c-202">MySQL/MariaDB\*</span></span>               | `.UseMySql((connectionString)`                | [<span data-ttu-id="1248c-203">Pomelo.EntityFrameworkCore.MySql</span><span class="sxs-lookup"><span data-stu-id="1248c-203">Pomelo.EntityFrameworkCore.MySql</span></span>](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql/)
| <span data-ttu-id="1248c-204">Oracle\*</span><span class="sxs-lookup"><span data-stu-id="1248c-204">Oracle\*</span></span>                      | `.UseOracle(connectionString)`                | [<span data-ttu-id="1248c-205">Oracle.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="1248c-205">Oracle.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Oracle.EntityFrameworkCore/)

<span data-ttu-id="1248c-206">\* Корпорация Майкрософт не предоставляет эти поставщики баз данных.</span><span class="sxs-lookup"><span data-stu-id="1248c-206">\*These database providers are not shipped by Microsoft.</span></span> <span data-ttu-id="1248c-207">Дополнительные сведения можно найти в статье [Поставщики баз данных](xref:core/providers/index).</span><span class="sxs-lookup"><span data-stu-id="1248c-207">See [Database Providers](xref:core/providers/index) for more information about database providers.</span></span>

> [!WARNING]
> <span data-ttu-id="1248c-208">Выполняющаяся в памяти база данных EF Core не предназначена для использования в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="1248c-208">The EF Core in-memory database is not designed for production use.</span></span> <span data-ttu-id="1248c-209">Кроме того, это не самый лучший вариант даже для тестирования.</span><span class="sxs-lookup"><span data-stu-id="1248c-209">In addition, it may not be the best choice even for testing.</span></span> <span data-ttu-id="1248c-210">Дополнительные сведения см. в статье [Тестирование кода, использующего EF Core](xref:core/testing/index).</span><span class="sxs-lookup"><span data-stu-id="1248c-210">See [Testing Code That Uses EF Core](xref:core/testing/index) for more information.</span></span>

<span data-ttu-id="1248c-211">Дополнительные сведения об использовании строк подключения с EF Core см. в статье [Строки подключения](xref:core/miscellaneous/connection-strings).</span><span class="sxs-lookup"><span data-stu-id="1248c-211">See [Connection Strings](xref:core/miscellaneous/connection-strings) for more information on using connection strings with EF Core.</span></span>

<span data-ttu-id="1248c-212">Необязательная настройка, зависящая от поставщика базы данных, выполняется в дополнительном построителе, предоставляемом поставщиком.</span><span class="sxs-lookup"><span data-stu-id="1248c-212">Optional configuration specific to the database provider is performed in an additional provider-specific builder.</span></span> <span data-ttu-id="1248c-213">Например, вы можете использовать <xref:Microsoft.EntityFrameworkCore.Infrastructure.SqlServerDbContextOptionsBuilder.EnableRetryOnFailure%2A> для настройки повторных попыток для обеспечения устойчивости при подключении к Azure SQL:</span><span class="sxs-lookup"><span data-stu-id="1248c-213">For example, using <xref:Microsoft.EntityFrameworkCore.Infrastructure.SqlServerDbContextOptionsBuilder.EnableRetryOnFailure%2A> to configure retries for connection resiliency when connecting to Azure SQL:</span></span>

<!--
    public class ApplicationDbContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder
                .UseSqlServer(
                    @"Server=(localdb)\mssqllocaldb;Database=Test",
                    providerOptions =>
                        {
                            providerOptions.EnableRetryOnFailure();
                        });
        }
    }
-->
[!code-csharp[ApplicationDbContext](../../../samples/core/Miscellaneous/ConfiguringDbContext/AdditionalProviderConfiguration/ApplicationDbContext.cs?name=ApplicationDbContext)]

> [!TIP]
> <span data-ttu-id="1248c-214">Этот же поставщик базы данных используется для SQL Server и Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="1248c-214">The same database provider is used for SQL Server and Azure SQL.</span></span> <span data-ttu-id="1248c-215">Но мы рекомендуем обеспечить [устойчивость подключения](xref:core/miscellaneous/connection-resiliency) к Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="1248c-215">However, it is recommended that [connection resiliency](xref:core/miscellaneous/connection-resiliency) be used when connecting to SQL Azure.</span></span>

<span data-ttu-id="1248c-216">Подробные сведения о настройке для конкретных поставщиков см. в статье [Поставщики базы данных](xref:core/providers/index).</span><span class="sxs-lookup"><span data-stu-id="1248c-216">See [Database Providers](xref:core/providers/index) for more information on provider-specific configuration.</span></span>

### <a name="other-dbcontext-configuration"></a><span data-ttu-id="1248c-217">Другая конфигурация DbContext</span><span class="sxs-lookup"><span data-stu-id="1248c-217">Other DbContext configuration</span></span>

<span data-ttu-id="1248c-218">Другую конфигурацию `DbContext` можно связать до или после (это не играет роли) вызова`Use*`.</span><span class="sxs-lookup"><span data-stu-id="1248c-218">Other `DbContext` configuration can be chained either before or after (it makes no difference which) the `Use*` call.</span></span> <span data-ttu-id="1248c-219">Например, чтобы включить ведение журнала для конфиденциальных данных:</span><span class="sxs-lookup"><span data-stu-id="1248c-219">For example, to turn on sensitive-data logging:</span></span>

<!--
    public class ApplicationDbContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder
                .EnableSensitiveDataLogging()
                .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Test");
        }
    }
-->
[!code-csharp[ApplicationDbContext](../../../samples/core/Miscellaneous/ConfiguringDbContext/AdditionalConfiguration/ApplicationDbContext.cs?name=ApplicationDbContext)]

<span data-ttu-id="1248c-220">В следующей таблице приведены примеры популярных методов, вызываемых для `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1248c-220">The following table contains examples of common methods called on `DbContextOptionsBuilder`.</span></span>

| <span data-ttu-id="1248c-221">Метод DbContextOptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="1248c-221">DbContextOptionsBuilder method</span></span>                                                             | <span data-ttu-id="1248c-222">Действие</span><span class="sxs-lookup"><span data-stu-id="1248c-222">What it does</span></span>                                                | <span data-ttu-id="1248c-223">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="1248c-223">Learn more</span></span>
|:-------------------------------------------------------------------------------------------|-------------------------------------------------------------|--------------
| <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.UseQueryTrackingBehavior%2A>   | <span data-ttu-id="1248c-224">Задает поведение отслеживания по умолчанию для запросов.</span><span class="sxs-lookup"><span data-stu-id="1248c-224">Sets the default tracking behavior for queries</span></span>              | [<span data-ttu-id="1248c-225">Поведение отслеживания запросов</span><span class="sxs-lookup"><span data-stu-id="1248c-225">Query Tracking Behavior</span></span>](xref:core/querying/tracking)
| <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.LogTo%2A>                      | <span data-ttu-id="1248c-226">Простой способ для получения журналов EF Core (для EF Core 5.0 и более поздних версий).</span><span class="sxs-lookup"><span data-stu-id="1248c-226">A simple way to get EF Core logs (EF Core 5.0 and later)</span></span>    | [<span data-ttu-id="1248c-227">Ведение журналов, регистрация событий и диагностика</span><span class="sxs-lookup"><span data-stu-id="1248c-227">Logging, Events, and Diagnostics</span></span>](xref:core/logging-events-diagnostics/index)
| <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.UseLoggerFactory%2A>           | <span data-ttu-id="1248c-228">Регистрирует фабрику `Microsoft.Extensions.Logging`.</span><span class="sxs-lookup"><span data-stu-id="1248c-228">Registers an `Microsoft.Extensions.Logging` factory</span></span>         | [<span data-ttu-id="1248c-229">Ведение журналов, регистрация событий и диагностика</span><span class="sxs-lookup"><span data-stu-id="1248c-229">Logging, Events, and Diagnostics</span></span>](xref:core/logging-events-diagnostics/index)
| <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.EnableSensitiveDataLogging%2A> | <span data-ttu-id="1248c-230">Отвечает за включение данных приложения в исключениях и ведение журналов.</span><span class="sxs-lookup"><span data-stu-id="1248c-230">Includes application data in exceptions and logging</span></span>         | [<span data-ttu-id="1248c-231">Ведение журналов, регистрация событий и диагностика</span><span class="sxs-lookup"><span data-stu-id="1248c-231">Logging, Events, and Diagnostics</span></span>](xref:core/logging-events-diagnostics/index)
| <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.EnableDetailedErrors%2A>       | <span data-ttu-id="1248c-232">Подробные ошибки запросов (в ущерб производительности).</span><span class="sxs-lookup"><span data-stu-id="1248c-232">More detailed query errors (at the expense of performance)</span></span>  | [<span data-ttu-id="1248c-233">Ведение журналов, регистрация событий и диагностика</span><span class="sxs-lookup"><span data-stu-id="1248c-233">Logging, Events, and Diagnostics</span></span>](xref:core/logging-events-diagnostics/index)
| <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.ConfigureWarnings%2A>          | <span data-ttu-id="1248c-234">Позволяет игнорировать или генерировать предупреждения и другие события.</span><span class="sxs-lookup"><span data-stu-id="1248c-234">Ignore or throw for warnings and other events</span></span>               | [<span data-ttu-id="1248c-235">Ведение журналов, регистрация событий и диагностика</span><span class="sxs-lookup"><span data-stu-id="1248c-235">Logging, Events, and Diagnostics</span></span>](xref:core/logging-events-diagnostics/index)
| <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.AddInterceptors%2A>            | <span data-ttu-id="1248c-236">Регистрирует перехватчики EF Core.</span><span class="sxs-lookup"><span data-stu-id="1248c-236">Registers EF Core interceptors</span></span>                              | [<span data-ttu-id="1248c-237">Ведение журналов, регистрация событий и диагностика</span><span class="sxs-lookup"><span data-stu-id="1248c-237">Logging, Events, and Diagnostics</span></span>](xref:core/logging-events-diagnostics/index)
| <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies%2A>            | <span data-ttu-id="1248c-238">Позволяет использовать динамические прокси-серверы для отложенной загрузки.</span><span class="sxs-lookup"><span data-stu-id="1248c-238">Use dynamic proxies for lazy-loading</span></span>                        | [<span data-ttu-id="1248c-239">Отложенная загрузка</span><span class="sxs-lookup"><span data-stu-id="1248c-239">Lazy Loading</span></span>](xref:core/querying/related-data/lazy)
| <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseChangeTrackingProxies%2A>         | <span data-ttu-id="1248c-240">Позволяет использовать динамические прокси-серверы для отслеживания изменений.</span><span class="sxs-lookup"><span data-stu-id="1248c-240">Use dynamic proxies for change-tracking</span></span>                     | <span data-ttu-id="1248c-241">Ожидается в ближайшее время...</span><span class="sxs-lookup"><span data-stu-id="1248c-241">Coming soon...</span></span>

> [!NOTE]
> <span data-ttu-id="1248c-242"><xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies%2A> и <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseChangeTrackingProxies%2A> — это методы расширения из пакета NuGet [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/).</span><span class="sxs-lookup"><span data-stu-id="1248c-242"><xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies%2A> and <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseChangeTrackingProxies%2A> are extension methods from the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) NuGet package.</span></span> <span data-ttu-id="1248c-243">Такой тип вызова ".UseSomething()" рекомендуется для настройки и (или) использования расширений EF Core в других пакетах.</span><span class="sxs-lookup"><span data-stu-id="1248c-243">This kind of ".UseSomething()" call is the recommended way to configure and/or use EF Core extensions contained in other packages.</span></span>

### <a name="dbcontextoptions-verses-dbcontextoptionstcontext"></a><span data-ttu-id="1248c-244">`DbContextOptions` и `DbContextOptions<TContext>`</span><span class="sxs-lookup"><span data-stu-id="1248c-244">`DbContextOptions` verses `DbContextOptions<TContext>`</span></span>

<span data-ttu-id="1248c-245">Большинство подклассов `DbContext`, которые принимают `DbContextOptions`, должны использовать [универсальный](/dotnet/csharp/programming-guide/generics/) вариант `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="1248c-245">Most `DbContext` subclasses that accept a `DbContextOptions` should use the [generic](/dotnet/csharp/programming-guide/generics/) `DbContextOptions<TContext>` variation.</span></span> <span data-ttu-id="1248c-246">Пример:</span><span class="sxs-lookup"><span data-stu-id="1248c-246">For example:</span></span>

<!--
    public sealed class SealedApplicationDbContext : DbContext
    {
        public SealedApplicationDbContext(DbContextOptions<SealedApplicationDbContext> contextOptions)
            : base(contextOptions)
        {
        }
    }
-->
[!code-csharp[SealedApplicationDbContext](../../../samples/core/Miscellaneous/ConfiguringDbContext/InheritDbContext/ApplicationDbContext.cs?name=SealedApplicationDbContext)]

<span data-ttu-id="1248c-247">Это гарантирует, что для конкретного подтипа `DbContext` будут разрешены правильные параметры из внедрения зависимостей, даже если зарегистрировано несколько подтипов `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1248c-247">This ensures that the correct options for the specific `DbContext` subtype are resolved from dependency injection, even when multiple `DbContext` subtypes are registered.</span></span>

> [!TIP]
> <span data-ttu-id="1248c-248">Запечатывать DbContext не обязательно, но рекомендуется для классов, от которых не будут осуществляться наследование.</span><span class="sxs-lookup"><span data-stu-id="1248c-248">Your DbContext does not need to be sealed, but sealing is best practice to do so for classes not designed to be inherited from.</span></span>

<span data-ttu-id="1248c-249">Но если сам подтип `DbContext` предполагает наследование, он должен предоставить доступ к защищенному конструктору, принимающему неуниверсальный `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="1248c-249">However, if the `DbContext` subtype is itself intended to be inherited from, then it should expose a protected constructor taking a non-generic `DbContextOptions`.</span></span> <span data-ttu-id="1248c-250">Пример:</span><span class="sxs-lookup"><span data-stu-id="1248c-250">For example:</span></span>

<!--
    public abstract class ApplicationDbContextBase : DbContext
    {
        protected ApplicationDbContextBase(DbContextOptions contextOptions)
            : base(contextOptions)
        {
        }
    }
-->
[!code-csharp[ApplicationDbContextBase](../../../samples/core/Miscellaneous/ConfiguringDbContext/InheritDbContext/ApplicationDbContext.cs?name=ApplicationDbContextBase)]

<span data-ttu-id="1248c-251">Это позволяет нескольким конкретным подклассам вызвать такой базовый конструктор с помощью разных универсальных экземпляров `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="1248c-251">This allows multiple concrete subclasses to call this base constructor using their different generic `DbContextOptions<TContext>` instances.</span></span> <span data-ttu-id="1248c-252">Пример:</span><span class="sxs-lookup"><span data-stu-id="1248c-252">For example:</span></span>

<!--
    public sealed class ApplicationDbContext1 : ApplicationDbContextBase
    {
        public ApplicationDbContext1(DbContextOptions<ApplicationDbContext1> contextOptions)
            : base(contextOptions)
        {
        }
    }

    public sealed class ApplicationDbContext2 : ApplicationDbContextBase
    {
        public ApplicationDbContext2(DbContextOptions<ApplicationDbContext2> contextOptions)
            : base(contextOptions)
        {
        }
    }
-->
[!code-csharp[ApplicationDbContext12](../../../samples/core/Miscellaneous/ConfiguringDbContext/InheritDbContext/ApplicationDbContext.cs?name=ApplicationDbContext12)]

<span data-ttu-id="1248c-253">Обратите внимание, что этот шаблон аналогичен наследованию непосредственно от `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1248c-253">Notice that this is exactly the same pattern as when inheriting from `DbContext` directly.</span></span> <span data-ttu-id="1248c-254">То есть конструктор `DbContext` принимает неуниверсальные `DbContextOptions` по этой причине.</span><span class="sxs-lookup"><span data-stu-id="1248c-254">That is, the `DbContext` constructor itself accepts a non-generic `DbContextOptions` for this reason.</span></span>

<span data-ttu-id="1248c-255">Подкласс `DbContext`, который предусматривает создание экземпляра и от которого будет осуществляться наследование, должен предоставить доступ к обеим формам конструктора.</span><span class="sxs-lookup"><span data-stu-id="1248c-255">A `DbContext` subclass intended to be both instantiated and inherited from should expose both forms of constructor.</span></span> <span data-ttu-id="1248c-256">Пример:</span><span class="sxs-lookup"><span data-stu-id="1248c-256">For example:</span></span>

<!--
    public class ApplicationDbContext : DbContext
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> contextOptions)
            : base(contextOptions)
        {
        }

        protected ApplicationDbContext(DbContextOptions contextOptions)
            : base(contextOptions)
        {
        }
    }
-->
[!code-csharp[ApplicationDbContext](../../../samples/core/Miscellaneous/ConfiguringDbContext/InheritDbContext/ApplicationDbContext.cs?name=ApplicationDbContext)]

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="1248c-257">Настройка DbContext во время разработки</span><span class="sxs-lookup"><span data-stu-id="1248c-257">Design-time DbContext configuration</span></span>

<span data-ttu-id="1248c-258">Средства разработки EF Core, такие как средства для [миграции EF Core](xref:core/managing-schemas/migrations/index), должны иметь возможность обнаруживать и создавать рабочие экземпляры типа `DbContext` для сбора сведений о типах сущностей приложения и их сопоставления со схемой базы данных.</span><span class="sxs-lookup"><span data-stu-id="1248c-258">EF Core design-time tools such as those for [EF Core migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="1248c-259">Этот процесс можно автоматизировать, так как средство может без труда создать `DbContext` с теми же настройками, которые заданы во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="1248c-259">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="1248c-260">Хотя любой шаблон, предоставляющий `DbContext` необходимые сведения о конфигурации, может работать во время выполнения, средства, требующие использования `DbContext` во время разработки, можно использовать только с ограниченным количеством шаблонов.</span><span class="sxs-lookup"><span data-stu-id="1248c-260">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="1248c-261">Дополнительные сведения об этом см. в статье [Создание DbContext во время разработки](xref:core/cli/dbcontext-creation).</span><span class="sxs-lookup"><span data-stu-id="1248c-261">These are covered in more detail in [Design-Time Context Creation](xref:core/cli/dbcontext-creation).</span></span>

## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="1248c-262">Предотвращение проблем с потоками DbContext</span><span class="sxs-lookup"><span data-stu-id="1248c-262">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="1248c-263">Entity Framework Core не поддерживает выполнение нескольких параллельных операций в одном экземпляре `DbContext`,</span><span class="sxs-lookup"><span data-stu-id="1248c-263">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="1248c-264">включая параллельное выполнение асинхронных запросов и любое явное использование экземпляра из нескольких потоков одновременно.</span><span class="sxs-lookup"><span data-stu-id="1248c-264">This includes both parallel execution of async queries and any explicit concurrent use from multiple threads.</span></span> <span data-ttu-id="1248c-265">Поэтому обязательно сразу же применяйте `await` к асинхронным вызовам или используйте отдельные экземпляры `DbContext`, выполняемые параллельно.</span><span class="sxs-lookup"><span data-stu-id="1248c-265">Therefore, always `await` async calls immediately, or use separate `DbContext` instances for operations that execute in parallel.</span></span>

<span data-ttu-id="1248c-266">Когда EF Core обнаруживает попытку одновременного использования экземпляра `DbContext`, отобразится `InvalidOperationException` с примерно таким сообщением:</span><span class="sxs-lookup"><span data-stu-id="1248c-266">When EF Core detects an attempt to use a `DbContext` instance concurrently, you'll see an `InvalidOperationException` with a message like this:</span></span>

> <span data-ttu-id="1248c-267">В этом контексте была начата вторая операция до завершения предыдущей операции.</span><span class="sxs-lookup"><span data-stu-id="1248c-267">A second operation started on this context before a previous operation completed.</span></span> <span data-ttu-id="1248c-268">Как правило, так происходит, когда несколько потоков используют один и тот же экземпляр DbContext. Но потокобезопасность членов экземпляров не гарантируется.</span><span class="sxs-lookup"><span data-stu-id="1248c-268">This is usually caused by different threads using the same instance of DbContext, however instance members are not guaranteed to be thread safe.</span></span>

<span data-ttu-id="1248c-269">Если одновременный доступ не обнаруживается, это может привести к неопределенному поведению, аварийному завершению работы приложения и повреждению данных.</span><span class="sxs-lookup"><span data-stu-id="1248c-269">When concurrent access goes undetected, it can result in undefined behavior, application crashes and data corruption.</span></span>

<span data-ttu-id="1248c-270">Есть распространенные ошибки, из-за которых может случайно возникнуть одновременный доступ к одному и тому же экземпляру `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1248c-270">There are common mistakes that can inadvertently cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="asynchronous-operation-pitfalls"></a><span data-ttu-id="1248c-271">Ошибки, связанные с асинхронными операциями</span><span class="sxs-lookup"><span data-stu-id="1248c-271">Asynchronous operation pitfalls</span></span>

<span data-ttu-id="1248c-272">Асинхронные методы позволяют EF Core инициировать операции, обращающиеся к базе данных, без блокировки.</span><span class="sxs-lookup"><span data-stu-id="1248c-272">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="1248c-273">Но если вызывающий объект не ожидает завершения одного из этих методов и переходит к выполнению других операций с `DbContext`, структура `DbContext` может быть (и, скорее всего, будет) повреждена.</span><span class="sxs-lookup"><span data-stu-id="1248c-273">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span>

<span data-ttu-id="1248c-274">Обязательно сразу же применяйте await к асинхронным методам EF Core.</span><span class="sxs-lookup"><span data-stu-id="1248c-274">Always await EF Core asynchronous methods immediately.</span></span>

### <a name="implicitly-sharing-dbcontext-instances-via-dependency-injection"></a><span data-ttu-id="1248c-275">Неявное совместное использование экземпляров DbContext путем внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="1248c-275">Implicitly sharing DbContext instances via dependency injection</span></span>

<span data-ttu-id="1248c-276">Метод расширения [`AddDbContext`](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) по умолчанию регистрирует типы `DbContext` с заданной [областью времени существования](/aspnet/core/fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="1248c-276">The [`AddDbContext`](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span>

<span data-ttu-id="1248c-277">Это обеспечивает защиту от проблем с одновременным доступом в большинстве приложений ASP.NET Core, так как есть только один поток, в котором каждый запрос клиента выполняется в определенный момент времени, и каждый такой запрос получает отдельную область внедрения зависимостей (и, следовательно, отдельный экземпляр `DbContext`).</span><span class="sxs-lookup"><span data-stu-id="1248c-277">This is safe from concurrent access issues in most ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span> <span data-ttu-id="1248c-278">В модели размещения Blazor Server используется один логический запрос для обслуживания пользовательского канала Blazor. Поэтому при использовании области внедрения по умолчанию для каждого пользовательского канала доступен только один экземпляр DbContext с заданной областью.</span><span class="sxs-lookup"><span data-stu-id="1248c-278">For Blazor Server hosting model, one logical request is used for maintaining the Blazor user circuit, and thus only one scoped DbContext instance is available per user circuit if the default injection scope is used.</span></span>

<span data-ttu-id="1248c-279">Если код явно выполняет несколько потоков в параллельном режиме, нужно исключить возможность одновременного доступа к экземплярам `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1248c-279">Any code that explicitly executes multiple threads in parallel should ensure that `DbContext` instances aren't ever accessed concurrently.</span></span>

<span data-ttu-id="1248c-280">При использовании внедрения зависимостей это можно реализовать путем регистрации контекста как ограниченного областью, создания области (с помощью `IServiceScopeFactory`) для каждого потока или регистрации экземпляра `DbContext` как временного (с помощью перегрузки `AddDbContext`, при которой принимается параметр `ServiceLifetime`).</span><span class="sxs-lookup"><span data-stu-id="1248c-280">Using dependency injection, this can be achieved by either registering the context as scoped, and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="1248c-281">Дополнительные материалы</span><span class="sxs-lookup"><span data-stu-id="1248c-281">More reading</span></span>

- <span data-ttu-id="1248c-282">Дополнительные сведения о внедрении зависимостей см. [здесь](/aspnet/core/fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1248c-282">Read [Dependency Injection](/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
- <span data-ttu-id="1248c-283">Подробную информацию см. в статье [Тестирование кода, использующего EF Core](xref:core/testing/index).</span><span class="sxs-lookup"><span data-stu-id="1248c-283">Read [Testing](xref:core/testing/index) for more information.</span></span>
