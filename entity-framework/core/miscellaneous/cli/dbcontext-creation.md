---
title: Создание DbContext во время разработки — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 66fec7605b6ac2da0af1e801f8a1dca0789aea35
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993722"
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="b12a7-102">Создание DbContext во время разработки</span><span class="sxs-lookup"><span data-stu-id="b12a7-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="b12a7-103">Некоторые команды Инструменты EF Core (например, [миграций][1] команды) требуют производный `DbContext` экземпляра во время разработки для сбора сведений о приложении типы сущностей и их сопоставлении схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="b12a7-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="b12a7-104">В большинстве случаев желательно, `DbContext` тем самым созданный настраивается в том, как было бы так же, как [настроен во время выполнения][2].</span><span class="sxs-lookup"><span data-stu-id="b12a7-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="b12a7-105">Существует несколько способов, попробуйте создать средства `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="b12a7-105">There are various ways the tools try to create the `DbContext`:</span></span>

<a name="from-application-services"></a><span data-ttu-id="b12a7-106">Из служб приложений</span><span class="sxs-lookup"><span data-stu-id="b12a7-106">From application services</span></span>
-------------------------
<span data-ttu-id="b12a7-107">Если ваш запускаемый проект приложения ASP.NET Core, инструменты попытаться получить объект DbContext у поставщика услуг приложения.</span><span class="sxs-lookup"><span data-stu-id="b12a7-107">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="b12a7-108">Средства сначала пытаются получить доступ к службе, вызвав `Program.BuildWebHost()` и доступ к `IWebHost.Services` свойство.</span><span class="sxs-lookup"><span data-stu-id="b12a7-108">The tools first try to obtain the service provider by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span>

> [!NOTE]
> <span data-ttu-id="b12a7-109">При создании нового приложения ASP.NET Core 2.0, этот обработчик включается по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b12a7-109">When you create a new ASP.NET Core 2.0 application, this hook is included by default.</span></span> <span data-ttu-id="b12a7-110">В предыдущих версиях EF Core и ASP.NET Core, инструменты попытке вызвать `Startup.ConfigureServices` напрямую, чтобы получить поставщик службы приложения, но этот шаблон больше не работает правильно в приложениях ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="b12a7-110">In previous versions of EF Core and ASP.NET Core, the tools try to invoke `Startup.ConfigureServices` directly in order to obtain the application's service provider, but this pattern no longer works correctly in ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="b12a7-111">Если вы обновляете приложение ASP.NET Core 1.x на 2.0, вы можете [изменить ваш `Program` класс стоит следовать этому шаблону новый][3].</span><span class="sxs-lookup"><span data-stu-id="b12a7-111">If you are upgrading an ASP.NET Core 1.x application to 2.0, you can [modify your `Program` class to follow the new pattern][3].</span></span>

<span data-ttu-id="b12a7-112">`DbContext` Сам и все зависимости в своем конструкторе должны быть зарегистрированы как службы в поставщике служб приложения.</span><span class="sxs-lookup"><span data-stu-id="b12a7-112">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="b12a7-113">Это легко достигается за счет [конструктор в `DbContext` , принимающий экземпляр `DbContextOptions<TContext>` как аргумент][4] и с помощью [`AddDbContext<TContext>` метод][5].</span><span class="sxs-lookup"><span data-stu-id="b12a7-113">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as an argument][4] and using the [`AddDbContext<TContext>` method][5].</span></span>

<a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="b12a7-114">С помощью конструктора без параметров</span><span class="sxs-lookup"><span data-stu-id="b12a7-114">Using a constructor with no parameters</span></span>
--------------------------------------
<span data-ttu-id="b12a7-115">DbContext не может быть получен от поставщика службы приложения, средства поиска производные `DbContext` тип внутри проекта.</span><span class="sxs-lookup"><span data-stu-id="b12a7-115">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="b12a7-116">Затем они пытаются создать экземпляр с помощью конструктора без параметров.</span><span class="sxs-lookup"><span data-stu-id="b12a7-116">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="b12a7-117">Это может быть конструктор по умолчанию, если `DbContext` настраивается с помощью [`OnConfiguring`][6] метод.</span><span class="sxs-lookup"><span data-stu-id="b12a7-117">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][6] method.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="b12a7-118">Из фабрики во время разработки</span><span class="sxs-lookup"><span data-stu-id="b12a7-118">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="b12a7-119">Вы также можете перенаправить средства Создание DbContext, реализовав `IDesignTimeDbContextFactory<TContext>` интерфейса: Если класс, реализующий этот интерфейс находится в любом проекте, производные `DbContext` или в проекте запуска приложения, средства обхода другие способы вместо создания DbContext и использование разработки фабрики.</span><span class="sxs-lookup"><span data-stu-id="b12a7-119">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

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
> <span data-ttu-id="b12a7-120">`args` Параметра, используемого в данный момент.</span><span class="sxs-lookup"><span data-stu-id="b12a7-120">The `args` parameter is currently unused.</span></span> <span data-ttu-id="b12a7-121">Существует [проблема][7] отслеживания возможность указывать аргументы во время разработки с помощью средств.</span><span class="sxs-lookup"><span data-stu-id="b12a7-121">There is [an issue][7] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="b12a7-122">Во время разработки фабрика может быть особенно полезно в том случае, если вам нужно по-разному настроить DbContext для времени разработки, чем во время выполнения, если `DbContext` конструктор принимает дополнительные параметры не зарегистрированы в функции внедрения Зависимостей, если вы не используете DI вообще или, если для некоторых -либо причине вы не хотите иметь `BuildWebHost` метод в приложении ASP.NET Core `Main` класса.</span><span class="sxs-lookup"><span data-stu-id="b12a7-122">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's `Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
