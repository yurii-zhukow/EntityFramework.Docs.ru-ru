---
title: Ведение журнала — EF Core
description: Настройка ведения журнала с помощью Entity Framework Core
author: ajcvickers
ms.date: 10/06/2020
uid: core/miscellaneous/logging
ms.openlocfilehash: 389834b3822aeeaefb8c085538bc6359ccfa7094
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92063014"
---
# <a name="logging"></a><span data-ttu-id="a44f7-103">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="a44f7-103">Logging</span></span>

> [!TIP]
> <span data-ttu-id="a44f7-104">Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="a44f7-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="simple-logging"></a><span data-ttu-id="a44f7-105">Простое ведение журнала</span><span class="sxs-lookup"><span data-stu-id="a44f7-105">Simple logging</span></span>

> [!NOTE]
> <span data-ttu-id="a44f7-106">Эта функция была добавлена в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="a44f7-106">This feature was added in EF Core 5.0.</span></span>

<span data-ttu-id="a44f7-107">Entity Framework Core (EF Core) создает сообщения журнала для таких операций, как выполнение запроса или сохранение изменений в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a44f7-107">Entity Framework Core (EF Core) generates log messages for operations such as executing a query or saving changes to the database.</span></span> <span data-ttu-id="a44f7-108">Доступ к ним можно получить из любого типа приложений с помощью [логто](https://github.com/dotnet/efcore/blob/ec3df8fd7e4ea4ebeebfa747619cef37b23ab2c6/src/EFCore/DbContextOptionsBuilder.cs#L135)</span><span class="sxs-lookup"><span data-stu-id="a44f7-108">These can be accessed from any type of application through use of [LogTo](https://github.com/dotnet/efcore/blob/ec3df8fd7e4ea4ebeebfa747619cef37b23ab2c6/src/EFCore/DbContextOptionsBuilder.cs#L135)</span></span> <!-- Issue #2748 <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.LogTo%2A> --> <span data-ttu-id="a44f7-109">При [настройке экземпляра DbContext](xref:core/miscellaneous/configuring-dbcontext).</span><span class="sxs-lookup"><span data-stu-id="a44f7-109">when [configuring a DbContext instance](xref:core/miscellaneous/configuring-dbcontext).</span></span> <span data-ttu-id="a44f7-110">Эта конфигурация обычно выполняется при переопределении <xref:Microsoft.EntityFrameworkCore.DbContext.OnConfiguring%2A?displayProperty=nameWithType> .</span><span class="sxs-lookup"><span data-stu-id="a44f7-110">This configuration is commonly done in an override of <xref:Microsoft.EntityFrameworkCore.DbContext.OnConfiguring%2A?displayProperty=nameWithType>.</span></span> <span data-ttu-id="a44f7-111">Например.</span><span class="sxs-lookup"><span data-stu-id="a44f7-111">For example:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(Console.WriteLine);
-->
[!code-csharp[LogToConsole](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=LogToConsole)]

<span data-ttu-id="a44f7-112">Эта концепция аналогична <xref:System.Data.Entity.Database.Log?displayProperty=nameWithType> в EF6.</span><span class="sxs-lookup"><span data-stu-id="a44f7-112">This concept is similar to <xref:System.Data.Entity.Database.Log?displayProperty=nameWithType> in EF6.</span></span>

<span data-ttu-id="a44f7-113">Дополнительные сведения см. в разделе [Простое ведение журнала](xref:core/miscellaneous/events/simple-logging) .</span><span class="sxs-lookup"><span data-stu-id="a44f7-113">See [Simple Logging](xref:core/miscellaneous/events/simple-logging) for more information.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="a44f7-114">ASP.NET Core приложения</span><span class="sxs-lookup"><span data-stu-id="a44f7-114">ASP.NET Core applications</span></span>

<span data-ttu-id="a44f7-115">EF Core автоматически интегрируется с механизмами ведения журналов ASP.NET Core при `AddDbContext` каждом `AddDbContextPool` использовании или.</span><span class="sxs-lookup"><span data-stu-id="a44f7-115">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="a44f7-116">Поэтому при использовании ASP.NET Core необходимо настроить ведение журнала, как описано в документации по [ASP.NET Core](/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="a44f7-116">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="a44f7-117">Другие приложения</span><span class="sxs-lookup"><span data-stu-id="a44f7-117">Other applications</span></span>

<span data-ttu-id="a44f7-118">Для ведения журнала EF Core требуется Илогжерфактори, настроенный с одним или несколькими регистраторами.</span><span class="sxs-lookup"><span data-stu-id="a44f7-118">EF Core logging requires an ILoggerFactory which is itself configured with one or more logging providers.</span></span> <span data-ttu-id="a44f7-119">Общие поставщики поставляются в следующие пакеты:</span><span class="sxs-lookup"><span data-stu-id="a44f7-119">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="a44f7-120">[Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): простое средство ведения журнала консоли.</span><span class="sxs-lookup"><span data-stu-id="a44f7-120">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="a44f7-121">[Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): поддерживает журналы диагностики служб приложений Azure и функции потока журнала.</span><span class="sxs-lookup"><span data-stu-id="a44f7-121">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="a44f7-122">[Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): журналы в мониторе отладчика с помощью System. Diagnostics. Debug. WriteLine ().</span><span class="sxs-lookup"><span data-stu-id="a44f7-122">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="a44f7-123">[Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): ведет журнал событий Windows.</span><span class="sxs-lookup"><span data-stu-id="a44f7-123">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="a44f7-124">[Microsoft. Extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): поддерживает EventSource/EventListener.</span><span class="sxs-lookup"><span data-stu-id="a44f7-124">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="a44f7-125">[Microsoft. Extensions. Logging. TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): ведет журналы в прослушиватель трассировки с помощью `System.Diagnostics.TraceSource.TraceEvent()` .</span><span class="sxs-lookup"><span data-stu-id="a44f7-125">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using `System.Diagnostics.TraceSource.TraceEvent()`.</span></span>

<span data-ttu-id="a44f7-126">После установки соответствующих пакетов приложение должно создать одноэлементный или глобальный экземпляр LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="a44f7-126">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="a44f7-127">Например, с помощью средства ведения журнала консоли:</span><span class="sxs-lookup"><span data-stu-id="a44f7-127">For example, using the console logger:</span></span>

### <a name="version-3x"></a>[<span data-ttu-id="a44f7-128">Версия 3. x</span><span class="sxs-lookup"><span data-stu-id="a44f7-128">Version 3.x</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

### <a name="version-2x"></a>[<span data-ttu-id="a44f7-129">Версия 2. x</span><span class="sxs-lookup"><span data-stu-id="a44f7-129">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="a44f7-130">В следующем примере кода используется `ConsoleLoggerProvider` конструктор, который является устаревшим в версии 2,2 и заменен в 3,0.</span><span class="sxs-lookup"><span data-stu-id="a44f7-130">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="a44f7-131">Можно спокойно пропускать и подавлять предупреждения при использовании 2,2.</span><span class="sxs-lookup"><span data-stu-id="a44f7-131">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

```csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

***

<span data-ttu-id="a44f7-132">Этот одноэлементный или глобальный экземпляр затем должен быть зарегистрирован в EF Core в `DbContextOptionsBuilder` .</span><span class="sxs-lookup"><span data-stu-id="a44f7-132">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="a44f7-133">Например.</span><span class="sxs-lookup"><span data-stu-id="a44f7-133">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="a44f7-134">Очень важно, чтобы приложения не создали новый экземпляр Илогжерфактори для каждого экземпляра контекста.</span><span class="sxs-lookup"><span data-stu-id="a44f7-134">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="a44f7-135">Это приведет к утечке памяти и снижению производительности.</span><span class="sxs-lookup"><span data-stu-id="a44f7-135">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="a44f7-136">Фильтрация регистрируемых</span><span class="sxs-lookup"><span data-stu-id="a44f7-136">Filtering what is logged</span></span>

<span data-ttu-id="a44f7-137">Приложение может контролировать, что регистрируется, настроив фильтр на Илогжерпровидер.</span><span class="sxs-lookup"><span data-stu-id="a44f7-137">The application can control what is logged by configuring a filter on the ILoggerProvider.</span></span> <span data-ttu-id="a44f7-138">Например.</span><span class="sxs-lookup"><span data-stu-id="a44f7-138">For example:</span></span>

### <a name="version-3x"></a>[<span data-ttu-id="a44f7-139">Версия 3. x</span><span class="sxs-lookup"><span data-stu-id="a44f7-139">Version 3.x</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

### <a name="version-2x"></a>[<span data-ttu-id="a44f7-140">Версия 2. x</span><span class="sxs-lookup"><span data-stu-id="a44f7-140">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="a44f7-141">В следующем примере кода используется `ConsoleLoggerProvider` конструктор, который является устаревшим в версии 2,2 и заменен в 3,0.</span><span class="sxs-lookup"><span data-stu-id="a44f7-141">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="a44f7-142">Можно спокойно пропускать и подавлять предупреждения при использовании 2,2.</span><span class="sxs-lookup"><span data-stu-id="a44f7-142">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

```csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[]
    {
        new ConsoleLoggerProvider((category, level)
            => category == DbLoggerCategory.Database.Command.Name
               && level == LogLevel.Information, true)
    });
```

***

<span data-ttu-id="a44f7-143">В этом примере журнал фильтруется так, чтобы возвращались только сообщения:</span><span class="sxs-lookup"><span data-stu-id="a44f7-143">In this example, the log is filtered to only return messages:</span></span>

* <span data-ttu-id="a44f7-144">в категории Microsoft. EntityFrameworkCore. Database. Command</span><span class="sxs-lookup"><span data-stu-id="a44f7-144">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
* <span data-ttu-id="a44f7-145">на уровне Information</span><span class="sxs-lookup"><span data-stu-id="a44f7-145">at the 'Information' level</span></span>

<span data-ttu-id="a44f7-146">Для EF Core категории средств ведения журнала определяются в `DbLoggerCategory` классе, чтобы упростить поиск категорий, но они разрешаются в простые строки.</span><span class="sxs-lookup"><span data-stu-id="a44f7-146">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="a44f7-147">Дополнительные сведения о базовой инфраструктуре ведения журнала можно найти в [документации по ASP.NET Core ведению журналов](/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="a44f7-147">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
