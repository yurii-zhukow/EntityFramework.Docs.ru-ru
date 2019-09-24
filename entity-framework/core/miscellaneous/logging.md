---
title: Ведение журнала — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 6a8499f9f0220087e76f2e0b3a75ce551c4ddb80
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197507"
---
# <a name="logging"></a><span data-ttu-id="91871-102">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="91871-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="91871-103">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="91871-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="91871-104">ASP.NET Core приложения</span><span class="sxs-lookup"><span data-stu-id="91871-104">ASP.NET Core applications</span></span>

<span data-ttu-id="91871-105">EF Core автоматически интегрируется с механизмами ведения журналов ASP.NET Core при `AddDbContext` каждом `AddDbContextPool` использовании или.</span><span class="sxs-lookup"><span data-stu-id="91871-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="91871-106">Поэтому при использовании ASP.NET Core необходимо настроить ведение журнала, как описано в документации по [ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="91871-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="91871-107">Другие приложения</span><span class="sxs-lookup"><span data-stu-id="91871-107">Other applications</span></span>

<span data-ttu-id="91871-108">Для ведения журнала EF Core требуется Илогжерфактори, настроенный с одним или несколькими регистраторами.</span><span class="sxs-lookup"><span data-stu-id="91871-108">EF Core logging requires an ILoggerFactory which is itself configured with one or more logging providers.</span></span> <span data-ttu-id="91871-109">Общие поставщики поставляются в следующие пакеты:</span><span class="sxs-lookup"><span data-stu-id="91871-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="91871-110">[Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): Простое средство ведения журнала консоли.</span><span class="sxs-lookup"><span data-stu-id="91871-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="91871-111">[Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Поддерживает журналы диагностики служб приложений Azure и функции "поток журнала".</span><span class="sxs-lookup"><span data-stu-id="91871-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="91871-112">[Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Ведет журналы в монитор отладчика с помощью System. Diagnostics. Debug. WriteLine ().</span><span class="sxs-lookup"><span data-stu-id="91871-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="91871-113">[Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Записывает в журнал событий Windows.</span><span class="sxs-lookup"><span data-stu-id="91871-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="91871-114">[Microsoft. Extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Поддерживает EventSource/EventListener.</span><span class="sxs-lookup"><span data-stu-id="91871-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="91871-115">[Microsoft. Extensions. Logging. TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Ведет журналы в прослушиватель трассировки `System.Diagnostics.TraceSource.TraceEvent()`с помощью.</span><span class="sxs-lookup"><span data-stu-id="91871-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using `System.Diagnostics.TraceSource.TraceEvent()`.</span></span>

<span data-ttu-id="91871-116">После установки соответствующих пакетов приложение должно создать одноэлементный или глобальный экземпляр LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="91871-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="91871-117">Например, с помощью средства ведения журнала консоли:</span><span class="sxs-lookup"><span data-stu-id="91871-117">For example, using the console logger:</span></span>

# <a name="version-30tabv3"></a>[<span data-ttu-id="91871-118">Версия 3,0</span><span class="sxs-lookup"><span data-stu-id="91871-118">Version 3.0</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

# <a name="version-2xtabv2"></a>[<span data-ttu-id="91871-119">Версия 2. x</span><span class="sxs-lookup"><span data-stu-id="91871-119">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="91871-120">В следующем примере кода используется `ConsoleLoggerProvider` конструктор, который является устаревшим в версии 2,2 и заменен в 3,0.</span><span class="sxs-lookup"><span data-stu-id="91871-120">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="91871-121">Можно спокойно пропускать и подавлять предупреждения при использовании 2,2.</span><span class="sxs-lookup"><span data-stu-id="91871-121">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

***

<span data-ttu-id="91871-122">Этот одноэлементный или глобальный экземпляр затем должен быть зарегистрирован в EF Core `DbContextOptionsBuilder`в.</span><span class="sxs-lookup"><span data-stu-id="91871-122">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="91871-123">Например:</span><span class="sxs-lookup"><span data-stu-id="91871-123">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="91871-124">Очень важно, чтобы приложения не создали новый экземпляр Илогжерфактори для каждого экземпляра контекста.</span><span class="sxs-lookup"><span data-stu-id="91871-124">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="91871-125">Это приведет к утечке памяти и снижению производительности.</span><span class="sxs-lookup"><span data-stu-id="91871-125">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="91871-126">Фильтрация регистрируемых</span><span class="sxs-lookup"><span data-stu-id="91871-126">Filtering what is logged</span></span>

<span data-ttu-id="91871-127">Приложение может контролировать, что регистрируется, настроив фильтр на Илогжерпровидер.</span><span class="sxs-lookup"><span data-stu-id="91871-127">The application can control what is logged by configuring a filter on the ILoggerProvider.</span></span> <span data-ttu-id="91871-128">Например:</span><span class="sxs-lookup"><span data-stu-id="91871-128">For example:</span></span>

# <a name="version-30tabv3"></a>[<span data-ttu-id="91871-129">Версия 3,0</span><span class="sxs-lookup"><span data-stu-id="91871-129">Version 3.0</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

# <a name="version-2xtabv2"></a>[<span data-ttu-id="91871-130">Версия 2. x</span><span class="sxs-lookup"><span data-stu-id="91871-130">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="91871-131">В следующем примере кода используется `ConsoleLoggerProvider` конструктор, который является устаревшим в версии 2,2 и заменен в 3,0.</span><span class="sxs-lookup"><span data-stu-id="91871-131">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="91871-132">Можно спокойно пропускать и подавлять предупреждения при использовании 2,2.</span><span class="sxs-lookup"><span data-stu-id="91871-132">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

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

<span data-ttu-id="91871-133">В этом примере журнал фильтруется так, чтобы возвращались только сообщения:</span><span class="sxs-lookup"><span data-stu-id="91871-133">In this example, the log is filtered to only return messages:</span></span>
 * <span data-ttu-id="91871-134">в категории Microsoft. EntityFrameworkCore. Database. Command</span><span class="sxs-lookup"><span data-stu-id="91871-134">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="91871-135">на уровне Information</span><span class="sxs-lookup"><span data-stu-id="91871-135">at the 'Information' level</span></span>

<span data-ttu-id="91871-136">Для EF Core категории средств ведения журнала определяются в `DbLoggerCategory` классе, чтобы упростить поиск категорий, но они разрешаются в простые строки.</span><span class="sxs-lookup"><span data-stu-id="91871-136">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="91871-137">Дополнительные сведения о базовой инфраструктуре ведения журнала можно найти в [документации по ASP.NET Core ведению журналов](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="91871-137">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
