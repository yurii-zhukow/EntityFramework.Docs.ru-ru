---
title: Ведение журнала — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 0a996403afdbe076b1690c98eeb305b40c4d1f4a
ms.sourcegitcommit: 109a16478de498b65717a6e09be243647e217fb3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2019
ms.locfileid: "55985578"
---
# <a name="logging"></a><span data-ttu-id="ab722-102">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="ab722-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="ab722-103">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="ab722-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="ab722-104">Приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ab722-104">ASP.NET Core applications</span></span>

<span data-ttu-id="ab722-105">EF Core автоматически интегрируется с механизмы ведения журнала ASP.NET Core всякий раз, когда `AddDbContext` или `AddDbContextPool` используется.</span><span class="sxs-lookup"><span data-stu-id="ab722-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="ab722-106">Таким образом, при использовании ASP.NET Core, ведение журнала следует настроить как описано в разделе [документации по ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="ab722-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="ab722-107">Другие приложения</span><span class="sxs-lookup"><span data-stu-id="ab722-107">Other applications</span></span>

<span data-ttu-id="ab722-108">Ведение журнала в данный момент EF Core требуется ILoggerFactory, который сам настроены ILoggerProvider один или несколько.</span><span class="sxs-lookup"><span data-stu-id="ab722-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="ab722-109">Общих поставщиков поставляются в следующие пакеты:</span><span class="sxs-lookup"><span data-stu-id="ab722-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="ab722-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): Простое консольное средство ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="ab722-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="ab722-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Поддерживает службы приложений «Журналы диагностики» и «Вход потока» компоненты.</span><span class="sxs-lookup"><span data-stu-id="ab722-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="ab722-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Журналы, чтобы отладчик монитор, используя System.Diagnostics.Debug.WriteLine().</span><span class="sxs-lookup"><span data-stu-id="ab722-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="ab722-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Записывает журнал событий Windows.</span><span class="sxs-lookup"><span data-stu-id="ab722-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="ab722-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Поддерживает EventSource/EventListener.</span><span class="sxs-lookup"><span data-stu-id="ab722-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="ab722-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Журналы, чтобы прослушиватель трассировки с помощью System.Diagnostics.TraceSource.TraceEvent().</span><span class="sxs-lookup"><span data-stu-id="ab722-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

> [!NOTE]
> <span data-ttu-id="ab722-116">В следующем коде используется пример `ConsoleLoggerProvider` конструктор, который были устарел в версии 2.2.</span><span class="sxs-lookup"><span data-stu-id="ab722-116">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2.</span></span> <span data-ttu-id="ab722-117">Правильное замены для устаревших API регистрации будут доступны в версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="ab722-117">Proper replacements for obsolete logging APIs will be available in version 3.0.</span></span> <span data-ttu-id="ab722-118">В то же время можно безопасно пропускать и отключать предупреждения.</span><span class="sxs-lookup"><span data-stu-id="ab722-118">In the meantime, it is safe to ignore and suppress the warnings.</span></span>

<span data-ttu-id="ab722-119">После установки соответствующих пакетов, в приложении необходимо создать одноэлементный/глобальный экземпляр LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="ab722-119">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="ab722-120">Например используя средство ведения журнала консоли:</span><span class="sxs-lookup"><span data-stu-id="ab722-120">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="ab722-121">Этот одноэлементный или глобальный экземпляр затем должен быть зарегистрирован в EF Core на `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ab722-121">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="ab722-122">Пример:</span><span class="sxs-lookup"><span data-stu-id="ab722-122">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="ab722-123">Очень важно, приложения не создают новый экземпляр ILoggerFactory для каждого экземпляра контекста.</span><span class="sxs-lookup"><span data-stu-id="ab722-123">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="ab722-124">Это приведет к утечке памяти и снижению производительности.</span><span class="sxs-lookup"><span data-stu-id="ab722-124">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="ab722-125">Фильтрация, регистрируемых</span><span class="sxs-lookup"><span data-stu-id="ab722-125">Filtering what is logged</span></span>

> [!NOTE]
> <span data-ttu-id="ab722-126">В следующем коде используется пример `ConsoleLoggerProvider` конструктор, который были устарел в версии 2.2.</span><span class="sxs-lookup"><span data-stu-id="ab722-126">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2.</span></span> <span data-ttu-id="ab722-127">Правильное замены для устаревших API регистрации будут доступны в версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="ab722-127">Proper replacements for obsolete logging APIs will be available in version 3.0.</span></span> <span data-ttu-id="ab722-128">В то же время можно безопасно пропускать и отключать предупреждения.</span><span class="sxs-lookup"><span data-stu-id="ab722-128">In the meantime, it is safe to ignore and suppress the warnings.</span></span>

<span data-ttu-id="ab722-129">Для фильтрации, регистрируемых проще всего настроить его при регистрации ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="ab722-129">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="ab722-130">Пример:</span><span class="sxs-lookup"><span data-stu-id="ab722-130">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="ab722-131">В этом примере для возвращения только сообщений фильтруется журнала:</span><span class="sxs-lookup"><span data-stu-id="ab722-131">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="ab722-132">в категории «Microsoft.EntityFrameworkCore.Database.Command»</span><span class="sxs-lookup"><span data-stu-id="ab722-132">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="ab722-133">на уровне «Сведения»</span><span class="sxs-lookup"><span data-stu-id="ab722-133">at the 'Information' level</span></span>

<span data-ttu-id="ab722-134">Категории средства ведения журнала для EF Core, определенных в `DbLoggerCategory` класса, чтобы его легко найти в категории, но эти разрешения в простых строк.</span><span class="sxs-lookup"><span data-stu-id="ab722-134">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="ab722-135">Дополнительные сведения о базовой инфраструктуре ведения журнала можно найти в [документации ведения журнала ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="ab722-135">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
