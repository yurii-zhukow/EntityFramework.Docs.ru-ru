---
title: Ведение журнала — EF Core
description: Настройка ведения журнала с помощью Entity Framework Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 56d0e3f8c5ecf56ee6bd6ddf0c9668bb8e3ac12f
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89617747"
---
# <a name="logging"></a><span data-ttu-id="ec26c-103">Logging</span><span class="sxs-lookup"><span data-stu-id="ec26c-103">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="ec26c-104">Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="ec26c-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="ec26c-105">ASP.NET Core приложения</span><span class="sxs-lookup"><span data-stu-id="ec26c-105">ASP.NET Core applications</span></span>

<span data-ttu-id="ec26c-106">EF Core автоматически интегрируется с механизмами ведения журналов ASP.NET Core при `AddDbContext` каждом `AddDbContextPool` использовании или.</span><span class="sxs-lookup"><span data-stu-id="ec26c-106">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="ec26c-107">Поэтому при использовании ASP.NET Core необходимо настроить ведение журнала, как описано в документации по [ASP.NET Core](/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="ec26c-107">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="ec26c-108">Другие приложения</span><span class="sxs-lookup"><span data-stu-id="ec26c-108">Other applications</span></span>

<span data-ttu-id="ec26c-109">Для ведения журнала EF Core требуется Илогжерфактори, настроенный с одним или несколькими регистраторами.</span><span class="sxs-lookup"><span data-stu-id="ec26c-109">EF Core logging requires an ILoggerFactory which is itself configured with one or more logging providers.</span></span> <span data-ttu-id="ec26c-110">Общие поставщики поставляются в следующие пакеты:</span><span class="sxs-lookup"><span data-stu-id="ec26c-110">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="ec26c-111">[Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): простое средство ведения журнала консоли.</span><span class="sxs-lookup"><span data-stu-id="ec26c-111">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="ec26c-112">[Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): поддерживает журналы диагностики служб приложений Azure и функции потока журнала.</span><span class="sxs-lookup"><span data-stu-id="ec26c-112">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="ec26c-113">[Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): журналы в мониторе отладчика с помощью System. Diagnostics. Debug. WriteLine ().</span><span class="sxs-lookup"><span data-stu-id="ec26c-113">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="ec26c-114">[Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): ведет журнал событий Windows.</span><span class="sxs-lookup"><span data-stu-id="ec26c-114">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="ec26c-115">[Microsoft. Extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): поддерживает EventSource/EventListener.</span><span class="sxs-lookup"><span data-stu-id="ec26c-115">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="ec26c-116">[Microsoft. Extensions. Logging. TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): ведет журналы в прослушиватель трассировки с помощью `System.Diagnostics.TraceSource.TraceEvent()` .</span><span class="sxs-lookup"><span data-stu-id="ec26c-116">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using `System.Diagnostics.TraceSource.TraceEvent()`.</span></span>

<span data-ttu-id="ec26c-117">После установки соответствующих пакетов приложение должно создать одноэлементный или глобальный экземпляр LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="ec26c-117">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="ec26c-118">Например, с помощью средства ведения журнала консоли:</span><span class="sxs-lookup"><span data-stu-id="ec26c-118">For example, using the console logger:</span></span>

### <a name="version-3x"></a>[<span data-ttu-id="ec26c-119">Версия 3. x</span><span class="sxs-lookup"><span data-stu-id="ec26c-119">Version 3.x</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

### <a name="version-2x"></a>[<span data-ttu-id="ec26c-120">Версия 2. x</span><span class="sxs-lookup"><span data-stu-id="ec26c-120">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="ec26c-121">В следующем примере кода используется `ConsoleLoggerProvider` конструктор, который является устаревшим в версии 2,2 и заменен в 3,0.</span><span class="sxs-lookup"><span data-stu-id="ec26c-121">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="ec26c-122">Можно спокойно пропускать и подавлять предупреждения при использовании 2,2.</span><span class="sxs-lookup"><span data-stu-id="ec26c-122">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

***

<span data-ttu-id="ec26c-123">Этот одноэлементный или глобальный экземпляр затем должен быть зарегистрирован в EF Core в `DbContextOptionsBuilder` .</span><span class="sxs-lookup"><span data-stu-id="ec26c-123">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="ec26c-124">Пример:</span><span class="sxs-lookup"><span data-stu-id="ec26c-124">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="ec26c-125">Очень важно, чтобы приложения не создали новый экземпляр Илогжерфактори для каждого экземпляра контекста.</span><span class="sxs-lookup"><span data-stu-id="ec26c-125">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="ec26c-126">Это приведет к утечке памяти и снижению производительности.</span><span class="sxs-lookup"><span data-stu-id="ec26c-126">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="ec26c-127">Фильтрация регистрируемых</span><span class="sxs-lookup"><span data-stu-id="ec26c-127">Filtering what is logged</span></span>

<span data-ttu-id="ec26c-128">Приложение может контролировать, что регистрируется, настроив фильтр на Илогжерпровидер.</span><span class="sxs-lookup"><span data-stu-id="ec26c-128">The application can control what is logged by configuring a filter on the ILoggerProvider.</span></span> <span data-ttu-id="ec26c-129">Пример:</span><span class="sxs-lookup"><span data-stu-id="ec26c-129">For example:</span></span>

### <a name="version-3x"></a>[<span data-ttu-id="ec26c-130">Версия 3. x</span><span class="sxs-lookup"><span data-stu-id="ec26c-130">Version 3.x</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

### <a name="version-2x"></a>[<span data-ttu-id="ec26c-131">Версия 2. x</span><span class="sxs-lookup"><span data-stu-id="ec26c-131">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="ec26c-132">В следующем примере кода используется `ConsoleLoggerProvider` конструктор, который является устаревшим в версии 2,2 и заменен в 3,0.</span><span class="sxs-lookup"><span data-stu-id="ec26c-132">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="ec26c-133">Можно спокойно пропускать и подавлять предупреждения при использовании 2,2.</span><span class="sxs-lookup"><span data-stu-id="ec26c-133">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[]
    {
        new ConsoleLoggerProvider((category, level)
            => category == DbLoggerCategory.Database.Command.Name
               && level == LogLevel.Information, true)
    });
```

***

<span data-ttu-id="ec26c-134">В этом примере журнал фильтруется так, чтобы возвращались только сообщения:</span><span class="sxs-lookup"><span data-stu-id="ec26c-134">In this example, the log is filtered to only return messages:</span></span>

* <span data-ttu-id="ec26c-135">в категории Microsoft. EntityFrameworkCore. Database. Command</span><span class="sxs-lookup"><span data-stu-id="ec26c-135">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
* <span data-ttu-id="ec26c-136">на уровне Information</span><span class="sxs-lookup"><span data-stu-id="ec26c-136">at the 'Information' level</span></span>

<span data-ttu-id="ec26c-137">Для EF Core категории средств ведения журнала определяются в `DbLoggerCategory` классе, чтобы упростить поиск категорий, но они разрешаются в простые строки.</span><span class="sxs-lookup"><span data-stu-id="ec26c-137">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="ec26c-138">Дополнительные сведения о базовой инфраструктуре ведения журнала можно найти в [документации по ASP.NET Core ведению журналов](/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="ec26c-138">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
