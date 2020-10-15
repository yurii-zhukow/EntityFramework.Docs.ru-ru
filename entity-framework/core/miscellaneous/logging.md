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
# <a name="logging"></a>Ведение журнала

> [!TIP]
> Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) из репозитория GitHub.

## <a name="simple-logging"></a>Простое ведение журнала

> [!NOTE]
> Эта функция была добавлена в EF Core 5,0.

Entity Framework Core (EF Core) создает сообщения журнала для таких операций, как выполнение запроса или сохранение изменений в базе данных. Доступ к ним можно получить из любого типа приложений с помощью [логто](https://github.com/dotnet/efcore/blob/ec3df8fd7e4ea4ebeebfa747619cef37b23ab2c6/src/EFCore/DbContextOptionsBuilder.cs#L135) <!-- Issue #2748 <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.LogTo%2A> --> При [настройке экземпляра DbContext](xref:core/miscellaneous/configuring-dbcontext). Эта конфигурация обычно выполняется при переопределении <xref:Microsoft.EntityFrameworkCore.DbContext.OnConfiguring%2A?displayProperty=nameWithType> . Например.

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(Console.WriteLine);
-->
[!code-csharp[LogToConsole](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=LogToConsole)]

Эта концепция аналогична <xref:System.Data.Entity.Database.Log?displayProperty=nameWithType> в EF6.

Дополнительные сведения см. в разделе [Простое ведение журнала](xref:core/miscellaneous/events/simple-logging) .

## <a name="aspnet-core-applications"></a>ASP.NET Core приложения

EF Core автоматически интегрируется с механизмами ведения журналов ASP.NET Core при `AddDbContext` каждом `AddDbContextPool` использовании или. Поэтому при использовании ASP.NET Core необходимо настроить ведение журнала, как описано в документации по [ASP.NET Core](/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Другие приложения

Для ведения журнала EF Core требуется Илогжерфактори, настроенный с одним или несколькими регистраторами. Общие поставщики поставляются в следующие пакеты:

* [Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): простое средство ведения журнала консоли.
* [Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): поддерживает журналы диагностики служб приложений Azure и функции потока журнала.
* [Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): журналы в мониторе отладчика с помощью System. Diagnostics. Debug. WriteLine ().
* [Microsoft. Extensions. Logging. EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): ведет журнал событий Windows.
* [Microsoft. Extensions. Logging. EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): поддерживает EventSource/EventListener.
* [Microsoft. Extensions. Logging. TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): ведет журналы в прослушиватель трассировки с помощью `System.Diagnostics.TraceSource.TraceEvent()` .

После установки соответствующих пакетов приложение должно создать одноэлементный или глобальный экземпляр LoggerFactory. Например, с помощью средства ведения журнала консоли:

### <a name="version-3x"></a>[Версия 3. x](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

### <a name="version-2x"></a>[Версия 2. x](#tab/v2)

> [!NOTE]
> В следующем примере кода используется `ConsoleLoggerProvider` конструктор, который является устаревшим в версии 2,2 и заменен в 3,0. Можно спокойно пропускать и подавлять предупреждения при использовании 2,2.

```csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

***

Этот одноэлементный или глобальный экземпляр затем должен быть зарегистрирован в EF Core в `DbContextOptionsBuilder` . Например.

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> Очень важно, чтобы приложения не создали новый экземпляр Илогжерфактори для каждого экземпляра контекста. Это приведет к утечке памяти и снижению производительности.

## <a name="filtering-what-is-logged"></a>Фильтрация регистрируемых

Приложение может контролировать, что регистрируется, настроив фильтр на Илогжерпровидер. Например.

### <a name="version-3x"></a>[Версия 3. x](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

### <a name="version-2x"></a>[Версия 2. x](#tab/v2)

> [!NOTE]
> В следующем примере кода используется `ConsoleLoggerProvider` конструктор, который является устаревшим в версии 2,2 и заменен в 3,0. Можно спокойно пропускать и подавлять предупреждения при использовании 2,2.

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

В этом примере журнал фильтруется так, чтобы возвращались только сообщения:

* в категории Microsoft. EntityFrameworkCore. Database. Command
* на уровне Information

Для EF Core категории средств ведения журнала определяются в `DbLoggerCategory` классе, чтобы упростить поиск категорий, но они разрешаются в простые строки.

Дополнительные сведения о базовой инфраструктуре ведения журнала можно найти в [документации по ASP.NET Core ведению журналов](/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).
