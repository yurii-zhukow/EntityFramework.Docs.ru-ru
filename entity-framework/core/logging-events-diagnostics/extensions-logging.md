---
title: Использование Microsoft. Extensions. Logging-EF Core
description: Ведение журнала EF Core с помощью Microsoft. Extensions. Logging в ASP.NET Core и других типах приложений
author: ajcvickers
ms.date: 10/15/2020
uid: core/logging-events-diagnostics/extensions-logging
ms.openlocfilehash: 3732bda0ef29b05672a2dfb83405da802f899191
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94431293"
---
# <a name="using-microsoftextensionslogging-in-ef-core"></a><span data-ttu-id="d54e5-103">Использование Microsoft. Extensions. Logging в EF Core</span><span class="sxs-lookup"><span data-stu-id="d54e5-103">Using Microsoft.Extensions.Logging in EF Core</span></span>

<span data-ttu-id="d54e5-104">[Microsoft. Extensions. Logging](/dotnet/core/extensions/logging) — это расширяемый механизм ведения журналов с поставщиками подключаемых модулей для многих распространенных систем ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="d54e5-104">[Microsoft.Extensions.Logging](/dotnet/core/extensions/logging) is an extensible logging mechanism with plug-in providers for many common logging systems.</span></span> <span data-ttu-id="d54e5-105">В качестве пакетов NuGet доступны оба подключаемых модуля, предоставляемые корпорацией Майкрософт (например, [Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/)) и сторонние подключаемые модули (например, [Serilog. Extensions. Logging](https://www.nuget.org/packages/Serilog.Extensions.Logging/)).</span><span class="sxs-lookup"><span data-stu-id="d54e5-105">Both Microsoft-supplied plug-ins (e.g [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/)) and third-party plug-ins (e.g. [Serilog.Extensions.Logging](https://www.nuget.org/packages/Serilog.Extensions.Logging/)) are available as NuGet packages.</span></span>

<span data-ttu-id="d54e5-106">Entity Framework Core (EF Core) полностью интегрируется с `Microsoft.Extensions.Logging` .</span><span class="sxs-lookup"><span data-stu-id="d54e5-106">Entity Framework Core (EF Core) fully integrates with `Microsoft.Extensions.Logging`.</span></span> <span data-ttu-id="d54e5-107">Однако рекомендуется использовать [Простое ведение](xref:core/logging-events-diagnostics/simple-logging) журнала для упрощения ведения журнала, особенно для приложений, которые не используют внедрение зависимостей.</span><span class="sxs-lookup"><span data-stu-id="d54e5-107">However, consider using [simple logging](xref:core/logging-events-diagnostics/simple-logging) for a simpler way to log, especially for applications that don't use dependency injection.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="d54e5-108">ASP.NET Core приложения</span><span class="sxs-lookup"><span data-stu-id="d54e5-108">ASP.NET Core applications</span></span>

<span data-ttu-id="d54e5-109">`Microsoft.Extensions.Logging`[используется по умолчанию в ASP.NET Core приложениях](/aspnet/core/fundamentals/logging).</span><span class="sxs-lookup"><span data-stu-id="d54e5-109">`Microsoft.Extensions.Logging` is [used by default in ASP.NET Core applications](/aspnet/core/fundamentals/logging).</span></span> <span data-ttu-id="d54e5-110">Вызов <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext%2A> или <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContextPool%2A> .</span><span class="sxs-lookup"><span data-stu-id="d54e5-110">Calling <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext%2A> or <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContextPool%2A>.</span></span>

## <a name="other-application-types"></a><span data-ttu-id="d54e5-111">Другие типы приложений</span><span class="sxs-lookup"><span data-stu-id="d54e5-111">Other application types</span></span>

<span data-ttu-id="d54e5-112">Другие типы приложений могут использовать [женеричост](/dotnet/core/extensions/generic-host) для получения тех же шаблонов внедрения зависимостей, которые используются в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d54e5-112">Other application types can use the [GenericHost](/dotnet/core/extensions/generic-host) to get the same dependency injection patterns as are used in ASP.NET Core.</span></span> <span data-ttu-id="d54e5-113">AddDbContext или Адддбконтекстпул можно использовать так же, как и в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d54e5-113">AddDbContext or AddDbContextPool can then be used just like in ASP.NET Core applications.</span></span>

<span data-ttu-id="d54e5-114">`Microsoft.Extensions.Logging` также может использоваться для приложений, не использующих внедрение зависимостей, однако проще настроить [Простое ведение журнала](xref:core/logging-events-diagnostics/simple-logging) .</span><span class="sxs-lookup"><span data-stu-id="d54e5-114">`Microsoft.Extensions.Logging` can also be used for applications that don't use dependency injection, although [simple logging](xref:core/logging-events-diagnostics/simple-logging) can be easier to set up.</span></span>

<span data-ttu-id="d54e5-115">`Microsoft.Extensions.Logging` требуется создание <xref:Microsoft.Extensions.Logging.LoggerFactory> .</span><span class="sxs-lookup"><span data-stu-id="d54e5-115">`Microsoft.Extensions.Logging` requires creation of a <xref:Microsoft.Extensions.Logging.LoggerFactory>.</span></span> <span data-ttu-id="d54e5-116">Эта фабрика должна храниться как статический или глобальный экземпляр в любом месте и использоваться каждый раз при создании DbContext.</span><span class="sxs-lookup"><span data-stu-id="d54e5-116">This factory should be stored as a static/global instance somewhere and used each time a DbContext is created.</span></span> <span data-ttu-id="d54e5-117">Например, фабрика средств ведения журнала часто хранится как статическое свойство в DbContext.</span><span class="sxs-lookup"><span data-stu-id="d54e5-117">For example, it is common to store the logger factory as a static property on the DbContext.</span></span>

### <a name="ef-core-30-and-above"></a>[<span data-ttu-id="d54e5-118">EF Core 3,0 и выше</span><span class="sxs-lookup"><span data-stu-id="d54e5-118">EF Core 3.0 and above</span></span>](#tab/v3)

<!--
        public static readonly ILoggerFactory MyLoggerFactory
            = LoggerFactory.Create(builder => { builder.AddConsole(); });
-->
[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

### <a name="ef-core-21"></a>[<span data-ttu-id="d54e5-119">EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="d54e5-119">EF Core 2.1</span></span>](#tab/v2)

```csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

> [!WARNING]
> <span data-ttu-id="d54e5-120">В EF Core 2,1 очень важно, чтобы приложения не создали новый экземпляр LoggerFactory для каждого экземпляра DbContext.</span><span class="sxs-lookup"><span data-stu-id="d54e5-120">In EF Core 2.1, It is very important that applications do not create a new LoggerFactory instance for each DbContext instance.</span></span> <span data-ttu-id="d54e5-121">Это приведет к утечке памяти и снижению производительности.</span><span class="sxs-lookup"><span data-stu-id="d54e5-121">Doing so will result in a memory leak and poor performance.</span></span> <span data-ttu-id="d54e5-122">Это было исправлено в EF Core 3,0 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="d54e5-122">This has been fixed in EF Core 3.0 and above.</span></span>

***

<span data-ttu-id="d54e5-123">Этот одноэлементный или глобальный экземпляр затем должен быть зарегистрирован в EF Core в <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder> .</span><span class="sxs-lookup"><span data-stu-id="d54e5-123">This singleton/global instance should then be registered with EF Core on the <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>.</span></span> <span data-ttu-id="d54e5-124">Пример:</span><span class="sxs-lookup"><span data-stu-id="d54e5-124">For example:</span></span>

<!--
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            => optionsBuilder
                .UseLoggerFactory(MyLoggerFactory)
                .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFLogging;ConnectRetryCount=0");
-->
[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

## <a name="getting-detailed-messages"></a><span data-ttu-id="d54e5-125">Получение подробных сообщений</span><span class="sxs-lookup"><span data-stu-id="d54e5-125">Getting detailed messages</span></span>

> [!TIP]
> <span data-ttu-id="d54e5-126">Onconfiguring по-прежнему вызывается при использовании AddDbContext или при передаче экземпляра DbContextOptions в конструктор DbContext.</span><span class="sxs-lookup"><span data-stu-id="d54e5-126">OnConfiguring is still called when AddDbContext is used or a DbContextOptions instance is passed to the DbContext constructor.</span></span> <span data-ttu-id="d54e5-127">Это делает его идеальным местом для применения конфигурации контекста независимо от того, как создается DbContext.</span><span class="sxs-lookup"><span data-stu-id="d54e5-127">This makes it the ideal place to apply context configuration regardless of how the DbContext is constructed.</span></span>

### <a name="sensitive-data"></a><span data-ttu-id="d54e5-128">Конфиденциальные данные</span><span class="sxs-lookup"><span data-stu-id="d54e5-128">Sensitive data</span></span>

<span data-ttu-id="d54e5-129">По умолчанию EF Core не будет включать значения каких бы то ни было данных в сообщениях об исключениях.</span><span class="sxs-lookup"><span data-stu-id="d54e5-129">By default, EF Core will not include the values of any data in exception messages.</span></span> <span data-ttu-id="d54e5-130">Это связано с тем, что такие данные могут быть конфиденциальными и могут быть отображены в рабочей среде, если исключение не обработано.</span><span class="sxs-lookup"><span data-stu-id="d54e5-130">This is because such data may be confidential, and could be revealed in production use if an exception is not handled.</span></span>

<span data-ttu-id="d54e5-131">Однако знание значений данных, особенно для ключей, может быть очень полезным при отладке.</span><span class="sxs-lookup"><span data-stu-id="d54e5-131">However, knowing data values, especially for keys, can be very helpful when debugging.</span></span> <span data-ttu-id="d54e5-132">Это можно включить в EF Core путем вызова <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.EnableSensitiveDataLogging> .</span><span class="sxs-lookup"><span data-stu-id="d54e5-132">This can be enabled in EF Core by calling <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.EnableSensitiveDataLogging>.</span></span> <span data-ttu-id="d54e5-133">Пример:</span><span class="sxs-lookup"><span data-stu-id="d54e5-133">For example:</span></span>

<!--
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            => optionsBuilder.EnableSensitiveDataLogging();
-->
[!code-csharp[EnableSensitiveDataLogging](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs?name=EnableSensitiveDataLogging)]

### <a name="detailed-query-exceptions"></a><span data-ttu-id="d54e5-134">Подробные исключения запросов</span><span class="sxs-lookup"><span data-stu-id="d54e5-134">Detailed query exceptions</span></span>

<span data-ttu-id="d54e5-135">Из соображений производительности EF Core не заключает каждый вызов для считывания значения из поставщика базы данных в блок try-catch.</span><span class="sxs-lookup"><span data-stu-id="d54e5-135">For performance reasons, EF Core does not wrap each call to read a value from the database provider in a try-catch block.</span></span> <span data-ttu-id="d54e5-136">Однако это иногда приводит к возникновению исключений, которые трудно диагностировать, особенно если база данных возвращает значение NULL, если она не разрешена моделью.</span><span class="sxs-lookup"><span data-stu-id="d54e5-136">However, this sometimes results in exceptions that are hard to diagnose, especially when the database returns a NULL when not allowed by the model.</span></span>

<span data-ttu-id="d54e5-137">Включение приводит <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.EnableDetailedErrors%2A> к тому, что EF познакомит эти блоки try-catch и, таким образом, предоставит более подробные сведения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="d54e5-137">Turning on <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.EnableDetailedErrors%2A> will cause EF to introduce these try-catch blocks and thereby provide more detailed errors.</span></span> <span data-ttu-id="d54e5-138">Пример:</span><span class="sxs-lookup"><span data-stu-id="d54e5-138">For example:</span></span>

<!--
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            => optionsBuilder.EnableDetailedErrors();
-->
[!code-csharp[EnableDetailedErrors](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs?name=EnableDetailedErrors)]

## <a name="configuration-for-specific-messages"></a><span data-ttu-id="d54e5-139">Конфигурация для конкретных сообщений</span><span class="sxs-lookup"><span data-stu-id="d54e5-139">Configuration for specific messages</span></span>

<span data-ttu-id="d54e5-140">EF Core <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.ConfigureWarnings%2A> API позволяет приложениям изменять то, что происходит при обнаружении определенного события.</span><span class="sxs-lookup"><span data-stu-id="d54e5-140">The EF Core <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.ConfigureWarnings%2A> API allows applications to change what happens when a specific event is encountered.</span></span> <span data-ttu-id="d54e5-141">Это можно использовать для следующих действий:</span><span class="sxs-lookup"><span data-stu-id="d54e5-141">This can be used to:</span></span>

* <span data-ttu-id="d54e5-142">Изменение уровня ведения журнала, в котором регистрируется событие</span><span class="sxs-lookup"><span data-stu-id="d54e5-142">Change the log level at which the event is logged</span></span>
* <span data-ttu-id="d54e5-143">Пропустить ведение журнала события</span><span class="sxs-lookup"><span data-stu-id="d54e5-143">Skip logging the event altogether</span></span>
* <span data-ttu-id="d54e5-144">Создавать исключение при возникновении события</span><span class="sxs-lookup"><span data-stu-id="d54e5-144">Throw an exception when the event occurs</span></span>

### <a name="changing-the-log-level-for-an-event"></a><span data-ttu-id="d54e5-145">Изменение уровня ведения журнала для события</span><span class="sxs-lookup"><span data-stu-id="d54e5-145">Changing the log level for an event</span></span>

<span data-ttu-id="d54e5-146">Иногда может быть полезно изменить предварительно определенный уровень ведения журнала для события.</span><span class="sxs-lookup"><span data-stu-id="d54e5-146">Sometimes it can be useful to change the pre-defined log level for an event.</span></span> <span data-ttu-id="d54e5-147">Например, это можно использовать для продвижения двух дополнительных событий из `LogLevel.Debug` в `LogLevel.Information` :</span><span class="sxs-lookup"><span data-stu-id="d54e5-147">For example, this can be used to promote two additional events from `LogLevel.Debug` to `LogLevel.Information`:</span></span>

<!--
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            => optionsBuilder
                .ConfigureWarnings(b => b.Log(
                    (RelationalEventId.ConnectionOpened, LogLevel.Information),
                    (RelationalEventId.ConnectionClosed, LogLevel.Information)));
-->
[!code-csharp[ChangeLogLevel](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs?name=ChangeLogLevel)]

### <a name="suppress-logging-an-event"></a><span data-ttu-id="d54e5-148">Отключить ведение журнала событий</span><span class="sxs-lookup"><span data-stu-id="d54e5-148">Suppress logging an event</span></span>

<span data-ttu-id="d54e5-149">Аналогичным образом отдельное событие можно подавлять из журнала.</span><span class="sxs-lookup"><span data-stu-id="d54e5-149">In a similar way, an individual event can be suppressed from logging.</span></span> <span data-ttu-id="d54e5-150">Это особенно полезно для пропуска предупреждения, которое было проверено и понято.</span><span class="sxs-lookup"><span data-stu-id="d54e5-150">This is particularly useful for ignoring a warning that has been reviewed and understood.</span></span> <span data-ttu-id="d54e5-151">Пример:</span><span class="sxs-lookup"><span data-stu-id="d54e5-151">For example:</span></span>

<!--
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            => optionsBuilder
                .ConfigureWarnings(b => b.Ignore(CoreEventId.DetachedLazyLoadingWarning));
-->
[!code-csharp[SuppressMessage](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs?name=SuppressMessage)]

### <a name="throw-for-an-event"></a><span data-ttu-id="d54e5-152">Создать для события</span><span class="sxs-lookup"><span data-stu-id="d54e5-152">Throw for an event</span></span>

<span data-ttu-id="d54e5-153">Наконец, EF Core можно настроить для создания исключения для данного события.</span><span class="sxs-lookup"><span data-stu-id="d54e5-153">Finally, EF Core can be configured to throw for a given event.</span></span> <span data-ttu-id="d54e5-154">Это особенно полезно для изменения предупреждения в ошибку.</span><span class="sxs-lookup"><span data-stu-id="d54e5-154">This is particularly useful for changing a warning into an error.</span></span> <span data-ttu-id="d54e5-155">(Действительно, это была первоначальная цель `ConfigureWarnings` метода, и, следовательно, имя.) Например:</span><span class="sxs-lookup"><span data-stu-id="d54e5-155">(Indeed, this was the original purpose of `ConfigureWarnings` method, hence the name.) For example:</span></span>

<!--
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            => optionsBuilder
                .ConfigureWarnings(b => b.Throw(RelationalEventId.QueryPossibleUnintendedUseOfEqualsWarning));
-->
[!code-csharp[ThrowForEvent](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs?name=ThrowForEvent)]

## <a name="filtering-and-other-configuration"></a><span data-ttu-id="d54e5-156">Фильтрация и другая конфигурация</span><span class="sxs-lookup"><span data-stu-id="d54e5-156">Filtering and other configuration</span></span>

<span data-ttu-id="d54e5-157">Инструкции по фильтрации журналов и другим настройкам см. [в разделе Ведение журнала в .NET](/dotnet/core/extensions/logging) .</span><span class="sxs-lookup"><span data-stu-id="d54e5-157">See [Logging in .NET](/dotnet/core/extensions/logging) for guidance on log filtering and other configuration.</span></span>

<span data-ttu-id="d54e5-158">События ведения журнала EF Core определены в одном из следующих:</span><span class="sxs-lookup"><span data-stu-id="d54e5-158">EF Core logging events are defined in one of:</span></span>

* <span data-ttu-id="d54e5-159"><xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId> для событий, общих для всех поставщиков EF Core баз данных</span><span class="sxs-lookup"><span data-stu-id="d54e5-159"><xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId> for events common to all EF Core database providers</span></span>
* <span data-ttu-id="d54e5-160"><xref:Microsoft.EntityFrameworkCore.Diagnostics.RelationalEventId> для событий, общих для всех поставщиков реляционных баз данных</span><span class="sxs-lookup"><span data-stu-id="d54e5-160"><xref:Microsoft.EntityFrameworkCore.Diagnostics.RelationalEventId> for events common to all relational database providers</span></span>
* <span data-ttu-id="d54e5-161">Аналогичный класс для событий, относящихся к текущему поставщику базы данных.</span><span class="sxs-lookup"><span data-stu-id="d54e5-161">A similar class for events specific to the current database provider.</span></span> <span data-ttu-id="d54e5-162">Например, <xref:Microsoft.EntityFrameworkCore.Diagnostics.SqlServerEventId> для поставщика SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d54e5-162">For example, <xref:Microsoft.EntityFrameworkCore.Diagnostics.SqlServerEventId> for the SQL Server provider.</span></span>

<span data-ttu-id="d54e5-163">Эти определения содержат идентификаторы событий, уровень ведения журнала и категорию для каждого события, используемые в `Microsoft.Extensions.Logging` .</span><span class="sxs-lookup"><span data-stu-id="d54e5-163">These definitions contain the event IDs, log level, and category for each event, as used by `Microsoft.Extensions.Logging`.</span></span>
