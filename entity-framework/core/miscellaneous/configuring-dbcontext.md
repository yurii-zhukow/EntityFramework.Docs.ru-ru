---
title: Настройка DbContext - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 6980acd53b0a74055af7a1e04b476f4625c327c9
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152394"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="1394f-102">Настройка DbContext</span><span class="sxs-lookup"><span data-stu-id="1394f-102">Configuring a DbContext</span></span>

<span data-ttu-id="1394f-103">В этой статье показаны основные шаблоны для настройки `DbContext` через `DbContextOptions` для подключения к базе данных с помощью конкретного поставщика EF Core и необязательного поведения.</span><span class="sxs-lookup"><span data-stu-id="1394f-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="1394f-104">Конфигурация DbContext во время разработки</span><span class="sxs-lookup"><span data-stu-id="1394f-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="1394f-105">EF основных компонентов во время разработки средств, таких как [миграций](xref:core/managing-schemas/migrations/index) необходимо иметь для обнаружения и создания экземпляра рабочего `DbContext` тип для сбора подробных сведений о типах сущностей и их сопоставлении схемы базы данных приложения.</span><span class="sxs-lookup"><span data-stu-id="1394f-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="1394f-106">Этот процесс может выполняться автоматически, при условии, что можно легко создать средство `DbContext` таким образом, что он будет настроен точно так же как бы настраивается во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="1394f-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="1394f-107">Хотя любой шаблон, предоставляющий сведения о конфигурации, необходимые для `DbContext` можно работать во время выполнения, которые требуют использования средств `DbContext` во время разработки можно работать только с ограниченным числом шаблонов.</span><span class="sxs-lookup"><span data-stu-id="1394f-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="1394f-108">Они описаны более подробно в [создания контекста разработки](xref:core/miscellaneous/cli/dbcontext-creation) раздела.</span><span class="sxs-lookup"><span data-stu-id="1394f-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="1394f-109">Настройка DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="1394f-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="1394f-110">`DbContext` необходимо иметь установленный экземпляр `DbContextOptions` для выполнения какой-либо работы.</span><span class="sxs-lookup"><span data-stu-id="1394f-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="1394f-111">`DbContextOptions` Экземпляр содержит конфиденциальную информацию конфигурации, такие как:</span><span class="sxs-lookup"><span data-stu-id="1394f-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="1394f-112">Поставщик базы данных для использования, обычно выбирается путем вызова метода, такие как `UseSqlServer` или `UseSqlite`</span><span class="sxs-lookup"><span data-stu-id="1394f-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`</span></span>
- <span data-ttu-id="1394f-113">Любой необходимую строку подключения или идентификатор экземпляра базы данных, обычно передается в качестве аргумента указанного выше метода выбора поставщика</span><span class="sxs-lookup"><span data-stu-id="1394f-113">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="1394f-114">Любого селектора уровня поставщика необязательно поведение также привязанные внутри вызова метода выбора поставщика</span><span class="sxs-lookup"><span data-stu-id="1394f-114">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="1394f-115">Все общие селекторы поведение EF Core, обычно связанных данных после или до выбора метода поставщика</span><span class="sxs-lookup"><span data-stu-id="1394f-115">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="1394f-116">В следующем примере настраивается `DbContextOptions` использование поставщика SQL Server, подключение содержащихся в `connectionString` переменной, время ожидания команды на уровне поставщика и селектора поведение EF Core, который делает все запросы, выполняемые в `DbContext` [нет отслеживания](xref:core/querying/tracking#no-tracking-queries) по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="1394f-116">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="1394f-117">Методы выбора поставщика и другие методы выбора поведение вышеупомянутых являются методами расширения на `DbContextOptions` или классы поставщика параметров.</span><span class="sxs-lookup"><span data-stu-id="1394f-117">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="1394f-118">Для доступа к эти методы расширения, необходимо иметь пространства имен (обычно `Microsoft.EntityFrameworkCore`) в области видимости и включить дополнительные зависимости в проект.</span><span class="sxs-lookup"><span data-stu-id="1394f-118">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="1394f-119">`DbContextOptions` Может быть задано для `DbContext` путем переопределения `OnConfiguring` метода или внешне через аргумент конструктора.</span><span class="sxs-lookup"><span data-stu-id="1394f-119">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="1394f-120">Если используются оба `OnConfiguring` применяется последним и перезаписывают значениями, заданными в аргументе конструктора.</span><span class="sxs-lookup"><span data-stu-id="1394f-120">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="1394f-121">Аргумент конструктора</span><span class="sxs-lookup"><span data-stu-id="1394f-121">Constructor argument</span></span>

<span data-ttu-id="1394f-122">Контекст кода с помощью конструктора:</span><span class="sxs-lookup"><span data-stu-id="1394f-122">Context code with constructor:</span></span>

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
> <span data-ttu-id="1394f-123">Базовый конструктор DbContext также принимает неуниверсальном версии `DbContextOptions`, но неуниверсальная версия не рекомендуется использовать в приложениях, где несколько типов контекста.</span><span class="sxs-lookup"><span data-stu-id="1394f-123">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="1394f-124">Код приложения для инициализации из аргумента конструктора:</span><span class="sxs-lookup"><span data-stu-id="1394f-124">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="1394f-125">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="1394f-125">OnConfiguring</span></span>

<span data-ttu-id="1394f-126">Контекст кода с помощью `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="1394f-126">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="1394f-127">Код приложения для инициализации `DbContext` , использующий `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="1394f-127">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="1394f-128">Этот подход непригодны для тестирования, если тесты целевой базы данных.</span><span class="sxs-lookup"><span data-stu-id="1394f-128">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="1394f-129">Внедрение зависимостей при помощи DbContext</span><span class="sxs-lookup"><span data-stu-id="1394f-129">Using DbContext with dependency injection</span></span>

<span data-ttu-id="1394f-130">EF Core поддерживает использование `DbContext` с контейнер внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="1394f-130">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="1394f-131">Тип DbContext можно добавить в контейнер службы с помощью `AddDbContext<TContext>` метод.</span><span class="sxs-lookup"><span data-stu-id="1394f-131">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="1394f-132">`AddDbContext<TContext>` сделает как ваш тип DbContext `TContext`и соответствующий `DbContextOptions<TContext>` доступны для внедрения из контейнера службы.</span><span class="sxs-lookup"><span data-stu-id="1394f-132">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="1394f-133">В разделе [дополнительные чтения](#more-reading) ниже дополнительные сведения о внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="1394f-133">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="1394f-134">Добавление `Dbcontext` для внедрения зависимостей:</span><span class="sxs-lookup"><span data-stu-id="1394f-134">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="1394f-135">Это требует добавления [аргумента конструктора](#constructor-argument) типу DbContext, принимающий `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="1394f-135">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="1394f-136">Контекст кода:</span><span class="sxs-lookup"><span data-stu-id="1394f-136">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="1394f-137">Код приложения (в ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="1394f-137">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="1394f-138">Код приложения (с использованием ServiceProvider напрямую, реже):</span><span class="sxs-lookup"><span data-stu-id="1394f-138">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a><span data-ttu-id="1394f-139">Дополнительные чтения</span><span class="sxs-lookup"><span data-stu-id="1394f-139">More reading</span></span>

* <span data-ttu-id="1394f-140">Чтение [Приступая к работе в ASP.NET Core](../get-started/aspnetcore/index.md) Дополнительные сведения об использовании EF с ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1394f-140">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="1394f-141">Чтение [внедрения зависимостей](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) для получения дополнительных сведений об использовании DI.</span><span class="sxs-lookup"><span data-stu-id="1394f-141">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="1394f-142">Чтение [тестирования](testing/index.md) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="1394f-142">Read [Testing](testing/index.md) for more information.</span></span>
