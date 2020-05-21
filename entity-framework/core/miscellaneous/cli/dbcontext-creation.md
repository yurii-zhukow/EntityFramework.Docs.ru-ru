---
title: Создание DbContext во время разработки — EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 0b0271dcabea63a2529c091cc14cb9059d56ac8d
ms.sourcegitcommit: 59e3d5ce7dfb284457cf1c991091683b2d1afe9d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2020
ms.locfileid: "83672957"
---
# <a name="design-time-dbcontext-creation"></a><span data-ttu-id="90118-102">Создание DbContext во время разработки</span><span class="sxs-lookup"><span data-stu-id="90118-102">Design-time DbContext Creation</span></span>

<span data-ttu-id="90118-103">Для некоторых команд EF Core инструментов (например, для команд [миграции][1] ) `DbContext` во время разработки необходимо создать производный экземпляр, чтобы получить сведения о типах сущностей приложения и их сопоставлении со схемой базы данных.</span><span class="sxs-lookup"><span data-stu-id="90118-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="90118-104">В большинстве случаев желательно, `DbContext` чтобы созданный таким образом объект был настроен аналогично тому, как он будет [настроен во время выполнения][2].</span><span class="sxs-lookup"><span data-stu-id="90118-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="90118-105">Существует несколько способов, с помощью которых средства пытаются создать `DbContext` :</span><span class="sxs-lookup"><span data-stu-id="90118-105">There are various ways the tools try to create the `DbContext`:</span></span>

## <a name="from-application-services"></a><span data-ttu-id="90118-106">Из служб приложений</span><span class="sxs-lookup"><span data-stu-id="90118-106">From application services</span></span>

<span data-ttu-id="90118-107">Если запускаемый проект использует [ASP.NET Core веб-узел][3] или [универсальный узел .NET Core][4], средства пытаются получить объект DbContext от поставщика услуг приложения.</span><span class="sxs-lookup"><span data-stu-id="90118-107">If your startup project uses the [ASP.NET Core Web Host][3] or [.NET Core Generic Host][4], the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="90118-108">Сначала средства пытаются получить поставщик услуг путем вызова `Program.CreateHostBuilder()` , вызова `Build()` и доступа к `Services` свойству.</span><span class="sxs-lookup"><span data-stu-id="90118-108">The tools first try to obtain the service provider by invoking `Program.CreateHostBuilder()`, calling `Build()`, then accessing the `Services` property.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/ApplicationService.cs)]

> [!NOTE]
> <span data-ttu-id="90118-109">При создании нового ASP.NET Core приложения этот обработчик включается по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="90118-109">When you create a new ASP.NET Core application, this hook is included by default.</span></span>

<span data-ttu-id="90118-110">`DbContext`Сам по себе и все зависимости в конструкторе должны быть зарегистрированы в качестве служб в поставщике услуг приложения.</span><span class="sxs-lookup"><span data-stu-id="90118-110">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="90118-111">Это можно легко сделать, используя [конструктор `DbContext` , который принимает экземпляр `DbContextOptions<TContext>` в качестве аргумента][5] и использует [ `AddDbContext<TContext>` метод][6].</span><span class="sxs-lookup"><span data-stu-id="90118-111">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as an argument][5] and using the [`AddDbContext<TContext>` method][6].</span></span>

## <a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="90118-112">Использование конструктора без параметров</span><span class="sxs-lookup"><span data-stu-id="90118-112">Using a constructor with no parameters</span></span>

<span data-ttu-id="90118-113">Если DbContext невозможно получить от поставщика службы приложений, средства ищут производный `DbContext` тип внутри проекта.</span><span class="sxs-lookup"><span data-stu-id="90118-113">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="90118-114">Затем они пытаются создать экземпляр с помощью конструктора без параметров.</span><span class="sxs-lookup"><span data-stu-id="90118-114">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="90118-115">Это может быть конструктор по умолчанию, если `DbContext` настроен с помощью [`OnConfiguring`][7] метода.</span><span class="sxs-lookup"><span data-stu-id="90118-115">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][7] method.</span></span>

## <a name="from-a-design-time-factory"></a><span data-ttu-id="90118-116">Из фабрики времени разработки</span><span class="sxs-lookup"><span data-stu-id="90118-116">From a design-time factory</span></span>

<span data-ttu-id="90118-117">Вы также можете сообщить средствам, как создать DbContext, реализовав `IDesignTimeDbContextFactory<TContext>` интерфейс: Если класс, реализующий этот интерфейс, находится в том же проекте, что и производный, `DbContext` или в проекте запуска приложения, средства обходят другие способы создания DbContext и используют вместо этого фабрику времени разработки.</span><span class="sxs-lookup"><span data-stu-id="90118-117">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/BloggingContextFactory.cs)]

> [!NOTE]
> <span data-ttu-id="90118-118">До Ефкоре 5,0 `args` параметр был неиспользован (см. [эту ошибку][8]).</span><span class="sxs-lookup"><span data-stu-id="90118-118">Prior to EFCore 5.0 the `args` parameter was unused (see [this issue][8]).</span></span>
> <span data-ttu-id="90118-119">Это исправлено в Ефкоре 5,0, а все дополнительные аргументы времени разработки передаются в приложение через этот параметр.</span><span class="sxs-lookup"><span data-stu-id="90118-119">This is fixed in EFCore 5.0 and any additional design-time arguments are passed into the application through that parameter.</span></span>

<span data-ttu-id="90118-120">Фабрика времени разработки может быть особенно полезной, если необходимо настроить DbContext по-разному для времени разработки, чем во время выполнения, если `DbContext` конструктор принимает дополнительные параметры, не зарегистрированные в di, если вы не используете функцию Onon или по какой-либо причине не хотите использовать `BuildWebHost` метод в классе ASP.NET Core приложения `Main` .</span><span class="sxs-lookup"><span data-stu-id="90118-120">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's `Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
