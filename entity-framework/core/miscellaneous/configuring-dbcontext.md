---
title: "Настройка DbContext - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 96abf3b94be3e1d19f833644f1c2f6f13fe0e730
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2017
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="f28cb-102">Настройка DbContext</span><span class="sxs-lookup"><span data-stu-id="f28cb-102">Configuring a DbContext</span></span>

<span data-ttu-id="f28cb-103">В этой статье показано шаблоны для настройки `DbContext` с `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="f28cb-103">This article shows patterns for configuring a `DbContext` with `DbContextOptions`.</span></span> <span data-ttu-id="f28cb-104">Параметры в основном используются для выбора и настройки хранилища данных.</span><span class="sxs-lookup"><span data-stu-id="f28cb-104">Options are primarily used to select and configure the data store.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="f28cb-105">Настройка DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="f28cb-105">Configuring DbContextOptions</span></span>

<span data-ttu-id="f28cb-106">`DbContext`необходимо иметь установленный экземпляр `DbContextOptions` для выполнения.</span><span class="sxs-lookup"><span data-stu-id="f28cb-106">`DbContext` must have an instance of `DbContextOptions` in order to execute.</span></span> <span data-ttu-id="f28cb-107">Это можно сделать путем переопределения `OnConfiguring`, или извне, предоставленных через аргумент конструктора.</span><span class="sxs-lookup"><span data-stu-id="f28cb-107">This can be configured by overriding `OnConfiguring`, or supplied externally via a constructor argument.</span></span>

<span data-ttu-id="f28cb-108">Если используются оба `OnConfiguring` выполняется на предоставленных параметрах, а значит она — друг к другу и может перезаписать значениями, заданными в аргументе конструктора.</span><span class="sxs-lookup"><span data-stu-id="f28cb-108">If both are used, `OnConfiguring` is executed on the supplied options, meaning it is additive and can overwrite  options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="f28cb-109">Аргумент конструктора</span><span class="sxs-lookup"><span data-stu-id="f28cb-109">Constructor argument</span></span>

<span data-ttu-id="f28cb-110">Контекст кода с помощью конструктора</span><span class="sxs-lookup"><span data-stu-id="f28cb-110">Context code with constructor</span></span>

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
> <span data-ttu-id="f28cb-111">Базовый конструктор DbContext также принимает неуниверсальном версии `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="f28cb-111">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`.</span></span> <span data-ttu-id="f28cb-112">С помощью неуниверсальная версия не рекомендуется для приложений с несколькими типами контекста.</span><span class="sxs-lookup"><span data-stu-id="f28cb-112">Using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="f28cb-113">Код приложения для инициализации из аргумента конструктора</span><span class="sxs-lookup"><span data-stu-id="f28cb-113">Application code to initialize from constructor argument</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="f28cb-114">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="f28cb-114">OnConfiguring</span></span>

> [!WARNING]  
> <span data-ttu-id="f28cb-115">`OnConfiguring`происходит в последнюю очередь и перезаписывают параметры, полученные из DI или конструктора.</span><span class="sxs-lookup"><span data-stu-id="f28cb-115">`OnConfiguring` occurs last and can overwrite options obtained from DI or the constructor.</span></span> <span data-ttu-id="f28cb-116">Этот подход непригодны для тестирования (если целевая базы данных).</span><span class="sxs-lookup"><span data-stu-id="f28cb-116">This approach does not lend itself to testing (unless you target the full database).</span></span>

<span data-ttu-id="f28cb-117">Контекст кода с помощью `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="f28cb-117">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="f28cb-118">Код приложения для инициализации с `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="f28cb-118">Application code to initialize with `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

## <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="f28cb-119">Внедрение зависимостей при помощи DbContext</span><span class="sxs-lookup"><span data-stu-id="f28cb-119">Using DbContext with dependency injection</span></span>

<span data-ttu-id="f28cb-120">EF поддерживает использование `DbContext` с контейнер внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="f28cb-120">EF supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="f28cb-121">Тип DbContext можно добавить в контейнер службы с помощью `AddDbContext<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="f28cb-121">Your DbContext type can be added to the service container by using `AddDbContext<TContext>`.</span></span>

<span data-ttu-id="f28cb-122">`AddDbContext`сделает как ваш тип DbContext `TContext`, и `DbContextOptions<TContext>` для внедрения из контейнера службы.</span><span class="sxs-lookup"><span data-stu-id="f28cb-122">`AddDbContext` will make both your DbContext type, `TContext`, and `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="f28cb-123">В разделе [дополнительные чтения](#more-reading) ниже сведения о внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="f28cb-123">See [more reading](#more-reading) below for information on dependency injection.</span></span>

<span data-ttu-id="f28cb-124">Добавление dbcontext внедрения зависимости</span><span class="sxs-lookup"><span data-stu-id="f28cb-124">Adding dbcontext to dependency injection</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="f28cb-125">Это требует добавления [аргумента конструктора](#constructor-argument) типу DbContext, принимающий `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="f28cb-125">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions`.</span></span>

<span data-ttu-id="f28cb-126">Контекст кода:</span><span class="sxs-lookup"><span data-stu-id="f28cb-126">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="f28cb-127">Код приложения (в ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="f28cb-127">Application code (in ASP.NET Core):</span></span>

``` csharp
public MyController(BloggingContext context)
```

<span data-ttu-id="f28cb-128">Код приложения (с использованием ServiceProvider напрямую, реже):</span><span class="sxs-lookup"><span data-stu-id="f28cb-128">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="using-idesigntimedbcontextfactorytcontext"></a><span data-ttu-id="f28cb-129">С помощью`IDesignTimeDbContextFactory<TContext>`</span><span class="sxs-lookup"><span data-stu-id="f28cb-129">Using `IDesignTimeDbContextFactory<TContext>`</span></span>

<span data-ttu-id="f28cb-130">В качестве альтернативы для вариантов, представленных выше, может также предоставлять реализацию метода `IDesignTimeDbContextFactory<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="f28cb-130">As an alternative to the options above, you may also provide an implementation of `IDesignTimeDbContextFactory<TContext>`.</span></span> <span data-ttu-id="f28cb-131">EF средства могут использовать эту фабрику для создания экземпляра вашей DbContext.</span><span class="sxs-lookup"><span data-stu-id="f28cb-131">EF tools can use this factory to create an instance of your DbContext.</span></span> <span data-ttu-id="f28cb-132">Это может потребоваться для включения определенных взаимодействия времени разработки, такие как миграция.</span><span class="sxs-lookup"><span data-stu-id="f28cb-132">This may be required in order to enable specific design-time experiences such as migrations.</span></span>

<span data-ttu-id="f28cb-133">Реализуйте этот интерфейс для службы времени проектирования для типов контекста, у которых нет открытого конструктора по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f28cb-133">Implement this interface to enable design-time services for context types that do not have a public default constructor.</span></span> <span data-ttu-id="f28cb-134">Реализации этого интерфейса, которые находятся в той же сборке, как производный контекст автоматически обнаруживает использование службы во время разработки.</span><span class="sxs-lookup"><span data-stu-id="f28cb-134">Design-time services will automatically discover implementations of this interface that are in the same assembly as the derived context.</span></span>

<span data-ttu-id="f28cb-135">Пример</span><span class="sxs-lookup"><span data-stu-id="f28cb-135">Example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

## <a name="more-reading"></a><span data-ttu-id="f28cb-136">Дополнительные чтения</span><span class="sxs-lookup"><span data-stu-id="f28cb-136">More reading</span></span>

* <span data-ttu-id="f28cb-137">Чтение [Приступая к работе в ASP.NET Core](../get-started/aspnetcore/index.md) Дополнительные сведения об использовании EF с ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f28cb-137">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="f28cb-138">Чтение [внедрения зависимостей](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) для получения дополнительных сведений об использовании DI.</span><span class="sxs-lookup"><span data-stu-id="f28cb-138">Read [Dependency Injection](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) to learn more about using DI.</span></span>
* <span data-ttu-id="f28cb-139">Чтение [тестирования](testing/index.md) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="f28cb-139">Read [Testing](testing/index.md) for more information.</span></span>
