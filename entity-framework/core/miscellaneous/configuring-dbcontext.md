---
title: Настройка DbContext — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: f5a9ae17471391442170d8c40264e4db6922cb08
ms.sourcegitcommit: 39080d38e1adea90db741257e60dc0e7ed08aa82
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/03/2018
ms.locfileid: "50980006"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="ef763-102">Настройка DbContext</span><span class="sxs-lookup"><span data-stu-id="ef763-102">Configuring a DbContext</span></span>

<span data-ttu-id="ef763-103">В этой статье показаны базовые шаблоны для настройки `DbContext` через `DbContextOptions` для подключения к базе данных с помощью определенного поставщика EF Core и необязательного поведения.</span><span class="sxs-lookup"><span data-stu-id="ef763-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="ef763-104">Конфигурация DbContext во время разработки</span><span class="sxs-lookup"><span data-stu-id="ef763-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="ef763-105">Средства EF Core во время разработки, такие как [миграций](xref:core/managing-schemas/migrations/index) должны иметь возможность находить и создать экземпляр рабочего `DbContext` тип для сбора сведений о типах сущностей и их сопоставлении схеме базы данных приложения.</span><span class="sxs-lookup"><span data-stu-id="ef763-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="ef763-106">Этот процесс может выполняться автоматически, до тех пор, пока средство может легко создавать `DbContext` таким образом, что он будет назначен точно так же как будет настраиваться во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="ef763-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="ef763-107">Хотя любой шаблон, который предоставляет сведения о конфигурации, необходимые для `DbContext` можно работать во время выполнения, средства, которые требуют использования `DbContext` во время разработки можно работать только с ограниченный набор шаблонов.</span><span class="sxs-lookup"><span data-stu-id="ef763-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="ef763-108">Эти файлы рассматриваются более подробно в [создание контекста разработки](xref:core/miscellaneous/cli/dbcontext-creation) раздел.</span><span class="sxs-lookup"><span data-stu-id="ef763-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="ef763-109">Настройка DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="ef763-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="ef763-110">`DbContext` необходимо иметь экземпляр `DbContextOptions` для какой-либо работы.</span><span class="sxs-lookup"><span data-stu-id="ef763-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="ef763-111">`DbContextOptions` Экземпляр содержит сведения о конфигурации, такие как:</span><span class="sxs-lookup"><span data-stu-id="ef763-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="ef763-112">Поставщик базы данных для использования, обычно выбирается путем вызова метода, таких как `UseSqlServer` или `UseSqlite`.</span><span class="sxs-lookup"><span data-stu-id="ef763-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="ef763-113">Эти методы расширения требуется соответствующий пакет поставщика, например `Microsoft.EntityFrameworkCore.SqlServer` или `Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="ef763-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="ef763-114">Эти методы определяются в `Microsoft.EntityFrameworkCore` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="ef763-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="ef763-115">Любой необходимую строку подключения или идентификатор экземпляра базы данных, обычно передается в качестве аргумента в метод выбора поставщика, упомянутых выше</span><span class="sxs-lookup"><span data-stu-id="ef763-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="ef763-116">Любое необязательное поведение уровня поставщика селекторы, также цепочку внутри вызова метода выбора поставщика</span><span class="sxs-lookup"><span data-stu-id="ef763-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="ef763-117">Все общие селекторы поведение EF Core, обычно связанных после или до метода выбора поставщика</span><span class="sxs-lookup"><span data-stu-id="ef763-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="ef763-118">В следующем примере настраивается `DbContextOptions` использование поставщика SQL Server, подключение содержащихся в `connectionString` переменную, время ожидания команды уровня поставщика и селектор поведения EF Core, который делает все запросы, выполняемые в `DbContext` [Отключение отслеживания](xref:core/querying/tracking#no-tracking-queries) по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="ef763-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="ef763-119">Методы выбора поставщика и другие методы выбора поведение, упомянутых выше являются методами расширений в `DbContextOptions` или классы поставщика параметров.</span><span class="sxs-lookup"><span data-stu-id="ef763-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="ef763-120">Чтобы получить доступ к эти методы расширения, может потребоваться пространство имен (обычно `Microsoft.EntityFrameworkCore`) в области видимости и включить зависимости дополнительных пакетов в проект.</span><span class="sxs-lookup"><span data-stu-id="ef763-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="ef763-121">`DbContextOptions` Может быть задано для `DbContext` путем переопределения `OnConfiguring` метод или извне через аргумент конструктора.</span><span class="sxs-lookup"><span data-stu-id="ef763-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="ef763-122">Если указаны оба, `OnConfiguring` применяется последним и перезаписывают параметры, передаваемое аргументом конструктора.</span><span class="sxs-lookup"><span data-stu-id="ef763-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="ef763-123">Аргумент конструктора</span><span class="sxs-lookup"><span data-stu-id="ef763-123">Constructor argument</span></span>

<span data-ttu-id="ef763-124">Контекст кода с помощью конструктора:</span><span class="sxs-lookup"><span data-stu-id="ef763-124">Context code with constructor:</span></span>

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
> <span data-ttu-id="ef763-125">Базовый конструктор DbContext также принимает нестандартную версию `DbContextOptions`, но неуниверсальная версия не рекомендуется использовать для приложений с несколькими типами контекста.</span><span class="sxs-lookup"><span data-stu-id="ef763-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="ef763-126">Код приложения для инициализации из аргумента конструктора:</span><span class="sxs-lookup"><span data-stu-id="ef763-126">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="ef763-127">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="ef763-127">OnConfiguring</span></span>

<span data-ttu-id="ef763-128">Контекст кода с помощью `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="ef763-128">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="ef763-129">Код приложения для инициализации `DbContext` , использующий `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="ef763-129">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="ef763-130">Этот подход не применяться для тестирования, если тесты предназначенных для всей базы данных.</span><span class="sxs-lookup"><span data-stu-id="ef763-130">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="ef763-131">С помощью DbContext с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="ef763-131">Using DbContext with dependency injection</span></span>

<span data-ttu-id="ef763-132">EF Core поддерживает использование `DbContext` с контейнер внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="ef763-132">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="ef763-133">Ваш тип DbContext можно добавить в контейнер службы с помощью `AddDbContext<TContext>` метод.</span><span class="sxs-lookup"><span data-stu-id="ef763-133">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="ef763-134">`AddDbContext<TContext>` сделает оба ваш тип DbContext `TContext`и соответствующих `DbContextOptions<TContext>` для внедрения из контейнера служб.</span><span class="sxs-lookup"><span data-stu-id="ef763-134">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="ef763-135">См. в разделе [дополнительные чтения](#more-reading) ниже дополнительные сведения о внедрении зависимостей.</span><span class="sxs-lookup"><span data-stu-id="ef763-135">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="ef763-136">Добавление `Dbcontext` для внедрения зависимостей:</span><span class="sxs-lookup"><span data-stu-id="ef763-136">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="ef763-137">Это требует добавления [аргумент конструктора](#constructor-argument) для вашего типа DbContext, который принимает `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="ef763-137">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="ef763-138">Контекст кода:</span><span class="sxs-lookup"><span data-stu-id="ef763-138">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="ef763-139">Код приложения (в ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="ef763-139">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="ef763-140">Код приложения (с использованием ServiceProvider напрямую, менее распространено):</span><span class="sxs-lookup"><span data-stu-id="ef763-140">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a><span data-ttu-id="ef763-141">Дополнительные материалы</span><span class="sxs-lookup"><span data-stu-id="ef763-141">More reading</span></span>

* <span data-ttu-id="ef763-142">Чтение [начало работы в ASP.NET Core](../get-started/aspnetcore/index.md) Дополнительные сведения об использовании EF с ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ef763-142">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="ef763-143">Чтение [внедрения зависимостей](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) Дополнительные сведения об использовании внедрения Зависимостей.</span><span class="sxs-lookup"><span data-stu-id="ef763-143">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="ef763-144">Чтение [тестирования](testing/index.md) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="ef763-144">Read [Testing](testing/index.md) for more information.</span></span>
