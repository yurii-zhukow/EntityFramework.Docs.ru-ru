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
# <a name="logging"></a>Ведение журнала

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) из репозитория GitHub.

## <a name="aspnet-core-applications"></a>ASP.NET Core приложения

EF Core автоматически интегрируется с механизмами ведения журналов ASP.NET Core каждый раз, когда используется `AddDbContext` или `AddDbContextPool`. Поэтому при использовании ASP.NET Core необходимо настроить ведение журнала, как описано в документации по [ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Другие приложения

Для ведения журнала EF Core требуется Илогжерфактори, настроенный с одним или несколькими регистраторами. Общие поставщики поставляются в следующие пакеты:

* [Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): простое средство ведения журнала консоли.
* [Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): поддерживает журналы диагностики служб приложений Azure и функции потока журнала.
* [Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): журналы в мониторе отладчика с помощью System. Diagnostics. Debug. WriteLine ().
* [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): ведет журнал событий Windows.
* [Microsoft. Extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): поддерживает EventSource/EventListener.
* [Microsoft. Extensions. Logging. TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): записывается в прослушиватель трассировки с помощью `System.Diagnostics.TraceSource.TraceEvent()`.

После установки соответствующих пакетов приложение должно создать одноэлементный или глобальный экземпляр LoggerFactory. Например, с помощью средства ведения журнала консоли:

### <a name="version-30tabv3"></a>[Версия 3,0](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

### <a name="version-2xtabv2"></a>[Версия 2.x](#tab/v2)

> [!NOTE]
> В следующем примере кода используется конструктор `ConsoleLoggerProvider`, который является устаревшим в версии 2,2 и заменен в 3,0. Можно спокойно пропускать и подавлять предупреждения при использовании 2,2.

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

***

Затем этот одноэлементный или глобальный экземпляр должен быть зарегистрирован в EF Core на `DbContextOptionsBuilder`. Например:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> Очень важно, чтобы приложения не создали новый экземпляр Илогжерфактори для каждого экземпляра контекста. Это приведет к утечке памяти и снижению производительности.

## <a name="filtering-what-is-logged"></a>Фильтрация регистрируемых

Приложение может контролировать, что регистрируется, настроив фильтр на Илогжерпровидер. Например:

### <a name="version-30tabv3"></a>[Версия 3,0](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

### <a name="version-2xtabv2"></a>[Версия 2.x](#tab/v2)

> [!NOTE]
> В следующем примере кода используется конструктор `ConsoleLoggerProvider`, который является устаревшим в версии 2,2 и заменен в 3,0. Можно спокойно пропускать и подавлять предупреждения при использовании 2,2.

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

В этом примере журнал фильтруется так, чтобы возвращались только сообщения:

* в категории Microsoft. EntityFrameworkCore. Database. Command
* на уровне Information

Для EF Core категории средств ведения журнала определяются в классе `DbLoggerCategory`, чтобы упростить поиск категорий, но они разрешаются в простые строки.

Дополнительные сведения о базовой инфраструктуре ведения журнала можно найти в [документации по ASP.NET Core ведению журналов](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).
