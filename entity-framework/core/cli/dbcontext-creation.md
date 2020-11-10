---
title: Создание DbContext во время разработки — EF Core
description: Стратегии создания DbContext времени разработки с Entity Framework Core
author: bricelam
ms.date: 10/27/2020
uid: core/cli/dbcontext-creation
ms.openlocfilehash: 144ed26dcf605dc29d53519ad2ea9cea58fb4e44
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94431360"
---
# <a name="design-time-dbcontext-creation"></a><span data-ttu-id="c76cc-103">Создание DbContext во время разработки</span><span class="sxs-lookup"><span data-stu-id="c76cc-103">Design-time DbContext Creation</span></span>

<span data-ttu-id="c76cc-104">Для некоторых команд EF Core инструментов (например, для команд [миграции][1] ) `DbContext` во время разработки необходимо создать производный экземпляр, чтобы получить сведения о типах сущностей приложения и их сопоставлении со схемой базы данных.</span><span class="sxs-lookup"><span data-stu-id="c76cc-104">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="c76cc-105">В большинстве случаев желательно, `DbContext` чтобы созданный таким образом объект был настроен аналогично тому, как он будет [настроен во время выполнения][2].</span><span class="sxs-lookup"><span data-stu-id="c76cc-105">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="c76cc-106">Существует несколько способов, с помощью которых средства пытаются создать `DbContext` :</span><span class="sxs-lookup"><span data-stu-id="c76cc-106">There are various ways the tools try to create the `DbContext`:</span></span>

## <a name="from-application-services"></a><span data-ttu-id="c76cc-107">Из служб приложений</span><span class="sxs-lookup"><span data-stu-id="c76cc-107">From application services</span></span>

<span data-ttu-id="c76cc-108">Если запускаемый проект использует [ASP.NET Core веб-узел][3] или [универсальный узел .NET Core][4], средства пытаются получить объект DbContext от поставщика услуг приложения.</span><span class="sxs-lookup"><span data-stu-id="c76cc-108">If your startup project uses the [ASP.NET Core Web Host][3] or [.NET Core Generic Host][4], the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="c76cc-109">Сначала средства пытаются получить поставщик услуг путем вызова `Program.CreateHostBuilder()` , вызова `Build()` и доступа к `Services` свойству.</span><span class="sxs-lookup"><span data-stu-id="c76cc-109">The tools first try to obtain the service provider by invoking `Program.CreateHostBuilder()`, calling `Build()`, then accessing the `Services` property.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/CommandLine/ApplicationService.cs#ApplicationService)]

> [!NOTE]
> <span data-ttu-id="c76cc-110">При создании нового ASP.NET Core приложения этот обработчик включается по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="c76cc-110">When you create a new ASP.NET Core application, this hook is included by default.</span></span>

<span data-ttu-id="c76cc-111">`DbContext`Сам по себе и все зависимости в конструкторе должны быть зарегистрированы в качестве служб в поставщике услуг приложения.</span><span class="sxs-lookup"><span data-stu-id="c76cc-111">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="c76cc-112">Это можно легко сделать, используя [конструктор `DbContext` , который принимает экземпляр `DbContextOptions<TContext>` в качестве аргумента][5] и использует [ `AddDbContext<TContext>` метод][6].</span><span class="sxs-lookup"><span data-stu-id="c76cc-112">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as an argument][5] and using the [`AddDbContext<TContext>` method][6].</span></span>

## <a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="c76cc-113">Использование конструктора без параметров</span><span class="sxs-lookup"><span data-stu-id="c76cc-113">Using a constructor with no parameters</span></span>

<span data-ttu-id="c76cc-114">Если DbContext невозможно получить от поставщика службы приложений, средства ищут производный `DbContext` тип внутри проекта.</span><span class="sxs-lookup"><span data-stu-id="c76cc-114">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="c76cc-115">Затем они пытаются создать экземпляр с помощью конструктора без параметров.</span><span class="sxs-lookup"><span data-stu-id="c76cc-115">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="c76cc-116">Это может быть конструктор по умолчанию, если `DbContext` настроен с помощью [`OnConfiguring`][7] метода.</span><span class="sxs-lookup"><span data-stu-id="c76cc-116">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][7] method.</span></span>

## <a name="from-a-design-time-factory"></a><span data-ttu-id="c76cc-117">Из фабрики времени разработки</span><span class="sxs-lookup"><span data-stu-id="c76cc-117">From a design-time factory</span></span>

<span data-ttu-id="c76cc-118">Вы также можете сообщить средствам, как создать DbContext, реализовав `IDesignTimeDbContextFactory<TContext>` интерфейс: Если класс, реализующий этот интерфейс, находится в том же проекте, что и производный, `DbContext` или в проекте запуска приложения, средства обходят другие способы создания DbContext и используют вместо этого фабрику времени разработки.</span><span class="sxs-lookup"><span data-stu-id="c76cc-118">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/CommandLine/BloggingContextFactory.cs#BloggingContextFactory)]

> [!NOTE]
> <span data-ttu-id="c76cc-119">До Ефкоре 5,0 `args` параметр был неиспользован (см. [эту ошибку][8]).</span><span class="sxs-lookup"><span data-stu-id="c76cc-119">Prior to EFCore 5.0 the `args` parameter was unused (see [this issue][8]).</span></span>
> <span data-ttu-id="c76cc-120">Это исправлено в Ефкоре 5,0, а все дополнительные аргументы времени разработки передаются в приложение через этот параметр.</span><span class="sxs-lookup"><span data-stu-id="c76cc-120">This is fixed in EFCore 5.0 and any additional design-time arguments are passed into the application through that parameter.</span></span>

<span data-ttu-id="c76cc-121">Фабрика времени разработки может быть особенно полезной, если необходимо настроить DbContext по-разному для времени разработки, чем во время выполнения, если `DbContext` конструктор принимает дополнительные параметры, не зарегистрированные в di, если вы не используете функцию Onon или по какой-либо причине не хотите использовать `CreateHostBuilder` метод в классе ASP.NET Core приложения `Main` .</span><span class="sxs-lookup"><span data-stu-id="c76cc-121">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `CreateHostBuilder` method in your ASP.NET Core application's `Main` class.</span></span>

## <a name="args"></a><span data-ttu-id="c76cc-122">Args</span><span class="sxs-lookup"><span data-stu-id="c76cc-122">Args</span></span>

<span data-ttu-id="c76cc-123">И Идесигнтимедбконтекстфактори. Креатедбконтекст, и Program. Креатехостбуилдер принимают аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="c76cc-123">Both IDesignTimeDbContextFactory.CreateDbContext and Program.CreateHostBuilder accept command line arguments.</span></span>

<span data-ttu-id="c76cc-124">Начиная с EF Core 5,0, можно указать следующие аргументы в средствах:</span><span class="sxs-lookup"><span data-stu-id="c76cc-124">Starting in EF Core 5.0, you can specify these arguments from the tools:</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="c76cc-125">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="c76cc-125">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update -- --environment Production
```

<span data-ttu-id="c76cc-126">`--`Токен направляет `dotnet ef` все, что следует за аргументом, и не пытается проанализировать их как параметры.</span><span class="sxs-lookup"><span data-stu-id="c76cc-126">The `--` token directs `dotnet ef` to treat everything that follows as an argument and not try to parse them as options.</span></span> <span data-ttu-id="c76cc-127">Все дополнительные аргументы, не используемые, `dotnet ef` пересылаются в приложение.</span><span class="sxs-lookup"><span data-stu-id="c76cc-127">Any extra arguments not used by `dotnet ef` are forwarded to the app.</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="c76cc-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c76cc-128">Visual Studio</span></span>](#tab/vs)

```powershell
Update-Database -Args '--environment Production'
```

***

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/dbcontext-configuration/index
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/dbcontext-configuration/index#constructor-argument
  [6]: xref:core/dbcontext-configuration/index#using-dbcontext-with-dependency-injection
  [7]: xref:core/dbcontext-configuration/index#onconfiguring
  [8]: https://github.com/dotnet/efcore/issues/8332
