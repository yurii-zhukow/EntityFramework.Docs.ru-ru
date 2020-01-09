---
title: Ведение журнала — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 1a3863ee5f508c1fd393d4ec2c25c46ab8634f00
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502101"
---
# <a name="logging"></a><span data-ttu-id="e0961-102">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="e0961-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="e0961-103">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="e0961-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="e0961-104">ASP.NET Core приложения</span><span class="sxs-lookup"><span data-stu-id="e0961-104">ASP.NET Core applications</span></span>

<span data-ttu-id="e0961-105">EF Core автоматически интегрируется с механизмами ведения журналов ASP.NET Core каждый раз, когда используется `AddDbContext` или `AddDbContextPool`.</span><span class="sxs-lookup"><span data-stu-id="e0961-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="e0961-106">Поэтому при использовании ASP.NET Core необходимо настроить ведение журнала, как описано в документации по [ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="e0961-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="e0961-107">Другие приложения</span><span class="sxs-lookup"><span data-stu-id="e0961-107">Other applications</span></span>

<span data-ttu-id="e0961-108">Для ведения журнала EF Core требуется Илогжерфактори, настроенный с одним или несколькими регистраторами.</span><span class="sxs-lookup"><span data-stu-id="e0961-108">EF Core logging requires an ILoggerFactory which is itself configured with one or more logging providers.</span></span> <span data-ttu-id="e0961-109">Общие поставщики поставляются в следующие пакеты:</span><span class="sxs-lookup"><span data-stu-id="e0961-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="e0961-110">[Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): простое средство ведения журнала консоли.</span><span class="sxs-lookup"><span data-stu-id="e0961-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="e0961-111">[Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): поддерживает журналы диагностики служб приложений Azure и функции потока журнала.</span><span class="sxs-lookup"><span data-stu-id="e0961-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="e0961-112">[Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): журналы в мониторе отладчика с помощью System. Diagnostics. Debug. WriteLine ().</span><span class="sxs-lookup"><span data-stu-id="e0961-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="e0961-113">[Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): ведет журнал событий Windows.</span><span class="sxs-lookup"><span data-stu-id="e0961-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="e0961-114">[Microsoft. Extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): поддерживает EventSource/EventListener.</span><span class="sxs-lookup"><span data-stu-id="e0961-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="e0961-115">[Microsoft. Extensions. Logging. TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): записывается в прослушиватель трассировки с помощью `System.Diagnostics.TraceSource.TraceEvent()`.</span><span class="sxs-lookup"><span data-stu-id="e0961-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using `System.Diagnostics.TraceSource.TraceEvent()`.</span></span>

<span data-ttu-id="e0961-116">После установки соответствующих пакетов приложение должно создать одноэлементный или глобальный экземпляр LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="e0961-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="e0961-117">Например, с помощью средства ведения журнала консоли:</span><span class="sxs-lookup"><span data-stu-id="e0961-117">For example, using the console logger:</span></span>

### <a name="version-30tabv3"></a>[<span data-ttu-id="e0961-118">Версия 3,0</span><span class="sxs-lookup"><span data-stu-id="e0961-118">Version 3.0</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

### <a name="version-2xtabv2"></a>[<span data-ttu-id="e0961-119">Версия 2.x</span><span class="sxs-lookup"><span data-stu-id="e0961-119">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="e0961-120">В следующем примере кода используется конструктор `ConsoleLoggerProvider`, который является устаревшим в версии 2,2 и заменен в 3,0.</span><span class="sxs-lookup"><span data-stu-id="e0961-120">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="e0961-121">Можно спокойно пропускать и подавлять предупреждения при использовании 2,2.</span><span class="sxs-lookup"><span data-stu-id="e0961-121">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

***

<span data-ttu-id="e0961-122">Затем этот одноэлементный или глобальный экземпляр должен быть зарегистрирован в EF Core на `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="e0961-122">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="e0961-123">Например:</span><span class="sxs-lookup"><span data-stu-id="e0961-123">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="e0961-124">Очень важно, чтобы приложения не создали новый экземпляр Илогжерфактори для каждого экземпляра контекста.</span><span class="sxs-lookup"><span data-stu-id="e0961-124">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="e0961-125">Это приведет к утечке памяти и снижению производительности.</span><span class="sxs-lookup"><span data-stu-id="e0961-125">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="e0961-126">Фильтрация регистрируемых</span><span class="sxs-lookup"><span data-stu-id="e0961-126">Filtering what is logged</span></span>

<span data-ttu-id="e0961-127">Приложение может контролировать, что регистрируется, настроив фильтр на Илогжерпровидер.</span><span class="sxs-lookup"><span data-stu-id="e0961-127">The application can control what is logged by configuring a filter on the ILoggerProvider.</span></span> <span data-ttu-id="e0961-128">Например:</span><span class="sxs-lookup"><span data-stu-id="e0961-128">For example:</span></span>

### <a name="version-30tabv3"></a>[<span data-ttu-id="e0961-129">Версия 3,0</span><span class="sxs-lookup"><span data-stu-id="e0961-129">Version 3.0</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

### <a name="version-2xtabv2"></a>[<span data-ttu-id="e0961-130">Версия 2.x</span><span class="sxs-lookup"><span data-stu-id="e0961-130">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="e0961-131">В следующем примере кода используется конструктор `ConsoleLoggerProvider`, который является устаревшим в версии 2,2 и заменен в 3,0.</span><span class="sxs-lookup"><span data-stu-id="e0961-131">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="e0961-132">Можно спокойно пропускать и подавлять предупреждения при использовании 2,2.</span><span class="sxs-lookup"><span data-stu-id="e0961-132">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

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

<span data-ttu-id="e0961-133">В этом примере журнал фильтруется так, чтобы возвращались только сообщения:</span><span class="sxs-lookup"><span data-stu-id="e0961-133">In this example, the log is filtered to only return messages:</span></span>

* <span data-ttu-id="e0961-134">в категории Microsoft. EntityFrameworkCore. Database. Command</span><span class="sxs-lookup"><span data-stu-id="e0961-134">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
* <span data-ttu-id="e0961-135">на уровне Information</span><span class="sxs-lookup"><span data-stu-id="e0961-135">at the 'Information' level</span></span>

<span data-ttu-id="e0961-136">Для EF Core категории средств ведения журнала определяются в классе `DbLoggerCategory`, чтобы упростить поиск категорий, но они разрешаются в простые строки.</span><span class="sxs-lookup"><span data-stu-id="e0961-136">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="e0961-137">Дополнительные сведения о базовой инфраструктуре ведения журнала можно найти в [документации по ASP.NET Core ведению журналов](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="e0961-137">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
