---
title: Создание DbContext во время разработки — EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: c36dae150085b1ab509288f6fabfdd8ed7201ca8
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812015"
---
# <a name="design-time-dbcontext-creation"></a><span data-ttu-id="2aa90-102">Создание DbContext во время разработки</span><span class="sxs-lookup"><span data-stu-id="2aa90-102">Design-time DbContext Creation</span></span>

<span data-ttu-id="2aa90-103">Для некоторых команд EF Core инструментов (например, для команд [миграции][1] ) во время разработки требуется создать производный экземпляр `DbContext`, чтобы получить сведения о типах сущностей приложения и их сопоставлении со схемой базы данных.</span><span class="sxs-lookup"><span data-stu-id="2aa90-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="2aa90-104">В большинстве случаев желательно, чтобы `DbContext`, созданный таким образом, был настроен аналогично тому, как он будет [настроен во время выполнения][2].</span><span class="sxs-lookup"><span data-stu-id="2aa90-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="2aa90-105">Существуют различные способы создания `DbContext`с помощью инструментов.</span><span class="sxs-lookup"><span data-stu-id="2aa90-105">There are various ways the tools try to create the `DbContext`:</span></span>

## <a name="from-application-services"></a><span data-ttu-id="2aa90-106">Из служб приложений</span><span class="sxs-lookup"><span data-stu-id="2aa90-106">From application services</span></span>

<span data-ttu-id="2aa90-107">Если запускаемый проект использует [ASP.NET Core веб-узел][3] или [универсальный узел .NET Core][4], средства пытаются получить объект DbContext от поставщика услуг приложения.</span><span class="sxs-lookup"><span data-stu-id="2aa90-107">If your startup project uses the [ASP.NET Core Web Host][3] or [.NET Core Generic Host][4], the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="2aa90-108">Сначала средства пытаются получить поставщик услуг путем вызова `Program.CreateHostBuilder()`, вызова `Build()`, а затем доступа к свойству `Services`.</span><span class="sxs-lookup"><span data-stu-id="2aa90-108">The tools first try to obtain the service provider by invoking `Program.CreateHostBuilder()`, calling `Build()`, then accessing the `Services` property.</span></span>

``` csharp
public class Program
{
    public static void Main(string[] args)
        => CreateHostBuilder(args).Build().Run();

    // EF Core uses this method at design time to access the DbContext
    public static IHostBuilder CreateHostBuilder(string[] args)
        => Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(
                webBuilder => webBuilder.UseStartup<Startup>());
}

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
        => services.AddDbContext<ApplicationDbContext>();
}

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

> [!NOTE]
> <span data-ttu-id="2aa90-109">При создании нового ASP.NET Core приложения этот обработчик включается по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2aa90-109">When you create a new ASP.NET Core application, this hook is included by default.</span></span>

<span data-ttu-id="2aa90-110">Сам `DbContext` и все зависимости в его конструкторе должны быть зарегистрированы в качестве служб в поставщике услуг приложения.</span><span class="sxs-lookup"><span data-stu-id="2aa90-110">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="2aa90-111">Для этого можно легко получить [Конструктор на `DbContext`, который принимает экземпляр `DbContextOptions<TContext>` в качестве аргумента][5] и использует [метод`AddDbContext<TContext>`][6].</span><span class="sxs-lookup"><span data-stu-id="2aa90-111">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as an argument][5] and using the [`AddDbContext<TContext>` method][6].</span></span>

## <a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="2aa90-112">Использование конструктора без параметров</span><span class="sxs-lookup"><span data-stu-id="2aa90-112">Using a constructor with no parameters</span></span>

<span data-ttu-id="2aa90-113">Если DbContext невозможно получить от поставщика службы приложений, средства ищут производный тип `DbContext` в проекте.</span><span class="sxs-lookup"><span data-stu-id="2aa90-113">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="2aa90-114">Затем они пытаются создать экземпляр с помощью конструктора без параметров.</span><span class="sxs-lookup"><span data-stu-id="2aa90-114">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="2aa90-115">Это может быть конструктор по умолчанию, если `DbContext` настроен с помощью метода [`OnConfiguring`][7] .</span><span class="sxs-lookup"><span data-stu-id="2aa90-115">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][7] method.</span></span>

## <a name="from-a-design-time-factory"></a><span data-ttu-id="2aa90-116">Из фабрики времени разработки</span><span class="sxs-lookup"><span data-stu-id="2aa90-116">From a design-time factory</span></span>

<span data-ttu-id="2aa90-117">Вы также можете сообщить средствам, как создать DbContext, реализовав интерфейс `IDesignTimeDbContextFactory<TContext>`: Если класс, реализующий этот интерфейс, находится в том же проекте, что и производный `DbContext`, или в проекте запуска приложения, средства обходят другие способы создания DbContext и использования фабрики времени разработки.</span><span class="sxs-lookup"><span data-stu-id="2aa90-117">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
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

> [!NOTE]
> <span data-ttu-id="2aa90-118">Параметр `args` в настоящее время не используется.</span><span class="sxs-lookup"><span data-stu-id="2aa90-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="2aa90-119">Существует ошибка [при отслеживании][8] возможности указания аргументов времени разработки из средств.</span><span class="sxs-lookup"><span data-stu-id="2aa90-119">There is [an issue][8] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="2aa90-120">Фабрика времени разработки может быть особенно полезной, если необходимо настроить DbContext по-разному для времени разработки, чем во время выполнения, если `DbContext` конструктор принимает дополнительные параметры, не зарегистрированные в DI, если вы не используете DI вообще или по какой-то причине предпочитаете не использовать метод `BuildWebHost` в классе `Main` приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2aa90-120">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's `Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
