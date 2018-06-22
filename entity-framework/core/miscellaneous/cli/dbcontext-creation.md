---
title: Создание DbContext во время разработки - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 8b38d300d31038bdf5cd877aa3c42b7f5f97eabc
ms.sourcegitcommit: 7113e8675f26cbb546200824512078bf360225df
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/28/2018
ms.locfileid: "30202488"
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="c1746-102">Создание DbContext во время разработки</span><span class="sxs-lookup"><span data-stu-id="c1746-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="c1746-103">Некоторые команды EF основные инструменты (например, [миграций] [ 1] команд) требуют производный `DbContext` экземпляра во время разработки для сбора подробных сведений о приложении типы сущностей и их сопоставлении схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="c1746-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="c1746-104">В большинстве случаев желательно, `DbContext` тем самым созданный настраивается так же, как в том, как было бы [настроен во время выполнения][2].</span><span class="sxs-lookup"><span data-stu-id="c1746-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="c1746-105">Существует несколько способов, попытайтесь создать средства `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="c1746-105">There are various ways the tools try to create the `DbContext`:</span></span>

<a name="from-application-services"></a><span data-ttu-id="c1746-106">Из служб приложений</span><span class="sxs-lookup"><span data-stu-id="c1746-106">From application services</span></span>
-------------------------
<span data-ttu-id="c1746-107">Если автоматически запускаемый проект приложения ASP.NET Core, средства попытайтесь получить объект DbContext приложения поставщика услуг.</span><span class="sxs-lookup"><span data-stu-id="c1746-107">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="c1746-108">Средство сначала пытаются получить доступ к службе с помощью вызова `Program.BuildWebHost()` и доступ к `IWebHost.Services` свойство.</span><span class="sxs-lookup"><span data-stu-id="c1746-108">The tool first try to obtain the service provider by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span>

> [!NOTE]
> <span data-ttu-id="c1746-109">При создании нового приложения ASP.NET Core 2.0, этот обработчик включается по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="c1746-109">When you create a new ASP.NET Core 2.0 application, this hook is included by default.</span></span> <span data-ttu-id="c1746-110">В предыдущих версиях EF Core и ASP.NET Core, попытка вызова средства `Startup.ConfigureServices` напрямую для получения поставщика службы приложения, но этот шаблон больше не работает правильно в приложениях ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="c1746-110">In previous versions of EF Core and ASP.NET Core, the tools try to invoke `Startup.ConfigureServices` directly in order to obtain the application's service provider, but this pattern no longer works correctly in ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="c1746-111">Если вы обновляете приложение ASP.NET Core 1.x 2.0, вы можете [изменить вашей `Program` класс, чтобы использовать этот новый подход][3].</span><span class="sxs-lookup"><span data-stu-id="c1746-111">If you are upgrading an ASP.NET Core 1.x application to 2.0, you can [modify your `Program` class to follow the new pattern][3].</span></span>

<span data-ttu-id="c1746-112">`DbContext` Себя и все зависимости в конструкторе должны быть зарегистрированы как службы в приложение поставщика услуг.</span><span class="sxs-lookup"><span data-stu-id="c1746-112">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="c1746-113">Это легко сделать, задав [конструктор `DbContext` , принимающий экземпляр `DbContextOptions<TContext>` как аргумент] [ 4] и с помощью [ `AddDbContext<TContext>` метод] [5].</span><span class="sxs-lookup"><span data-stu-id="c1746-113">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as a argument][4] and using the [`AddDbContext<TContext>` method][5].</span></span>

<a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="c1746-114">С помощью конструктора без параметров</span><span class="sxs-lookup"><span data-stu-id="c1746-114">Using a constructor with no parameters</span></span>
--------------------------------------
<span data-ttu-id="c1746-115">Если класс DbContext нельзя получить от поставщика службы приложений, средства поиска производного `DbContext` типа внутри проекта.</span><span class="sxs-lookup"><span data-stu-id="c1746-115">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="c1746-116">Затем они пытаются создать экземпляр с помощью конструктора без параметров.</span><span class="sxs-lookup"><span data-stu-id="c1746-116">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="c1746-117">Это может быть конструктор по умолчанию, если `DbContext` настраивается с помощью [ `OnConfiguring` ] [ 6] метод.</span><span class="sxs-lookup"><span data-stu-id="c1746-117">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][6] method.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="c1746-118">Из фабрики во время разработки</span><span class="sxs-lookup"><span data-stu-id="c1746-118">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="c1746-119">Можно также сказать, средства Создание вашей DbContext путем реализации `IDesignTimeDbContextFactory<TContext>` интерфейса: Если класс, реализующий этот интерфейс находится в любом проекте, производным `DbContext` или в проекте при запуске приложения, обходить средства другие способы вместо создания DbContext и использование фабрики во время разработки.</span><span class="sxs-lookup"><span data-stu-id="c1746-119">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

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
> <span data-ttu-id="c1746-120">`args` Параметр не в настоящее время используется.</span><span class="sxs-lookup"><span data-stu-id="c1746-120">The `args` parameter is currently unused.</span></span> <span data-ttu-id="c1746-121">Отсутствует [проблему] [ 7] возможность указывать аргументы во время разработки с помощью средств отслеживания.</span><span class="sxs-lookup"><span data-stu-id="c1746-121">There is [an issue][7] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="c1746-122">Фабрика разработки может быть особенно полезно, если необходимо настроить класс DbContext по-разному для времени разработки, чем во время выполнения, если `DbContext` конструктор принимает дополнительные параметры не зарегистрированы в DI, если вы не используете DI вообще или, если для некоторых причины, не должны иметь `BuildWebHost` метода в приложении ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1746-122">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's</span></span>  
<span data-ttu-id="c1746-123">Класс `Main`.</span><span class="sxs-lookup"><span data-stu-id="c1746-123">`Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
