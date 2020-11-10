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
# <a name="using-microsoftextensionslogging-in-ef-core"></a>Использование Microsoft. Extensions. Logging в EF Core

[Microsoft. Extensions. Logging](/dotnet/core/extensions/logging) — это расширяемый механизм ведения журналов с поставщиками подключаемых модулей для многих распространенных систем ведения журналов. В качестве пакетов NuGet доступны оба подключаемых модуля, предоставляемые корпорацией Майкрософт (например, [Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/)) и сторонние подключаемые модули (например, [Serilog. Extensions. Logging](https://www.nuget.org/packages/Serilog.Extensions.Logging/)).

Entity Framework Core (EF Core) полностью интегрируется с `Microsoft.Extensions.Logging` . Однако рекомендуется использовать [Простое ведение](xref:core/logging-events-diagnostics/simple-logging) журнала для упрощения ведения журнала, особенно для приложений, которые не используют внедрение зависимостей.

## <a name="aspnet-core-applications"></a>ASP.NET Core приложения

`Microsoft.Extensions.Logging`[используется по умолчанию в ASP.NET Core приложениях](/aspnet/core/fundamentals/logging). Вызов <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext%2A> или <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContextPool%2A> .

## <a name="other-application-types"></a>Другие типы приложений

Другие типы приложений могут использовать [женеричост](/dotnet/core/extensions/generic-host) для получения тех же шаблонов внедрения зависимостей, которые используются в ASP.NET Core. AddDbContext или Адддбконтекстпул можно использовать так же, как и в приложениях ASP.NET Core.

`Microsoft.Extensions.Logging` также может использоваться для приложений, не использующих внедрение зависимостей, однако проще настроить [Простое ведение журнала](xref:core/logging-events-diagnostics/simple-logging) .

`Microsoft.Extensions.Logging` требуется создание <xref:Microsoft.Extensions.Logging.LoggerFactory> . Эта фабрика должна храниться как статический или глобальный экземпляр в любом месте и использоваться каждый раз при создании DbContext. Например, фабрика средств ведения журнала часто хранится как статическое свойство в DbContext.

### <a name="ef-core-30-and-above"></a>[EF Core 3,0 и выше](#tab/v3)

<!--
        public static readonly ILoggerFactory MyLoggerFactory
            = LoggerFactory.Create(builder => { builder.AddConsole(); });
-->
[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

### <a name="ef-core-21"></a>[EF Core 2.1](#tab/v2)

```csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

> [!WARNING]
> В EF Core 2,1 очень важно, чтобы приложения не создали новый экземпляр LoggerFactory для каждого экземпляра DbContext. Это приведет к утечке памяти и снижению производительности. Это было исправлено в EF Core 3,0 и более поздних версий.

***

Этот одноэлементный или глобальный экземпляр затем должен быть зарегистрирован в EF Core в <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder> . Пример:

<!--
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            => optionsBuilder
                .UseLoggerFactory(MyLoggerFactory)
                .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFLogging;ConnectRetryCount=0");
-->
[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

## <a name="getting-detailed-messages"></a>Получение подробных сообщений

> [!TIP]
> Onconfiguring по-прежнему вызывается при использовании AddDbContext или при передаче экземпляра DbContextOptions в конструктор DbContext. Это делает его идеальным местом для применения конфигурации контекста независимо от того, как создается DbContext.

### <a name="sensitive-data"></a>Конфиденциальные данные

По умолчанию EF Core не будет включать значения каких бы то ни было данных в сообщениях об исключениях. Это связано с тем, что такие данные могут быть конфиденциальными и могут быть отображены в рабочей среде, если исключение не обработано.

Однако знание значений данных, особенно для ключей, может быть очень полезным при отладке. Это можно включить в EF Core путем вызова <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.EnableSensitiveDataLogging> . Пример:

<!--
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            => optionsBuilder.EnableSensitiveDataLogging();
-->
[!code-csharp[EnableSensitiveDataLogging](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs?name=EnableSensitiveDataLogging)]

### <a name="detailed-query-exceptions"></a>Подробные исключения запросов

Из соображений производительности EF Core не заключает каждый вызов для считывания значения из поставщика базы данных в блок try-catch. Однако это иногда приводит к возникновению исключений, которые трудно диагностировать, особенно если база данных возвращает значение NULL, если она не разрешена моделью.

Включение приводит <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.EnableDetailedErrors%2A> к тому, что EF познакомит эти блоки try-catch и, таким образом, предоставит более подробные сведения об ошибках. Пример:

<!--
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            => optionsBuilder.EnableDetailedErrors();
-->
[!code-csharp[EnableDetailedErrors](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs?name=EnableDetailedErrors)]

## <a name="configuration-for-specific-messages"></a>Конфигурация для конкретных сообщений

EF Core <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.ConfigureWarnings%2A> API позволяет приложениям изменять то, что происходит при обнаружении определенного события. Это можно использовать для следующих действий:

* Изменение уровня ведения журнала, в котором регистрируется событие
* Пропустить ведение журнала события
* Создавать исключение при возникновении события

### <a name="changing-the-log-level-for-an-event"></a>Изменение уровня ведения журнала для события

Иногда может быть полезно изменить предварительно определенный уровень ведения журнала для события. Например, это можно использовать для продвижения двух дополнительных событий из `LogLevel.Debug` в `LogLevel.Information` :

<!--
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            => optionsBuilder
                .ConfigureWarnings(b => b.Log(
                    (RelationalEventId.ConnectionOpened, LogLevel.Information),
                    (RelationalEventId.ConnectionClosed, LogLevel.Information)));
-->
[!code-csharp[ChangeLogLevel](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs?name=ChangeLogLevel)]

### <a name="suppress-logging-an-event"></a>Отключить ведение журнала событий

Аналогичным образом отдельное событие можно подавлять из журнала. Это особенно полезно для пропуска предупреждения, которое было проверено и понято. Пример:

<!--
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            => optionsBuilder
                .ConfigureWarnings(b => b.Ignore(CoreEventId.DetachedLazyLoadingWarning));
-->
[!code-csharp[SuppressMessage](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs?name=SuppressMessage)]

### <a name="throw-for-an-event"></a>Создать для события

Наконец, EF Core можно настроить для создания исключения для данного события. Это особенно полезно для изменения предупреждения в ошибку. (Действительно, это была первоначальная цель `ConfigureWarnings` метода, и, следовательно, имя.) Например:

<!--
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            => optionsBuilder
                .ConfigureWarnings(b => b.Throw(RelationalEventId.QueryPossibleUnintendedUseOfEqualsWarning));
-->
[!code-csharp[ThrowForEvent](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs?name=ThrowForEvent)]

## <a name="filtering-and-other-configuration"></a>Фильтрация и другая конфигурация

Инструкции по фильтрации журналов и другим настройкам см. [в разделе Ведение журнала в .NET](/dotnet/core/extensions/logging) .

События ведения журнала EF Core определены в одном из следующих:

* <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId> для событий, общих для всех поставщиков EF Core баз данных
* <xref:Microsoft.EntityFrameworkCore.Diagnostics.RelationalEventId> для событий, общих для всех поставщиков реляционных баз данных
* Аналогичный класс для событий, относящихся к текущему поставщику базы данных. Например, <xref:Microsoft.EntityFrameworkCore.Diagnostics.SqlServerEventId> для поставщика SQL Server.

Эти определения содержат идентификаторы событий, уровень ведения журнала и категорию для каждого события, используемые в `Microsoft.Extensions.Logging` .
