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
# <a name="logging"></a>Ведение журнала

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) из репозитория GitHub.

## <a name="aspnet-core-applications"></a>Приложения ASP.NET Core

EF Core автоматически интегрируется с механизмы ведения журнала ASP.NET Core всякий раз, когда `AddDbContext` или `AddDbContextPool` используется. Таким образом, при использовании ASP.NET Core, ведение журнала следует настроить как описано в разделе [документации по ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Другие приложения

Ведение журнала в данный момент EF Core требуется ILoggerFactory, который сам настроены ILoggerProvider один или несколько. Общих поставщиков поставляются в следующие пакеты:

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): Простое консольное средство ведения журнала.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Поддерживает службы приложений «Журналы диагностики» и «Вход потока» компоненты.
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Журналы, чтобы отладчик монитор, используя System.Diagnostics.Debug.WriteLine().
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Записывает журнал событий Windows.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Поддерживает EventSource/EventListener.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Журналы, чтобы прослушиватель трассировки с помощью System.Diagnostics.TraceSource.TraceEvent().

> [!NOTE]
> В следующем коде используется пример `ConsoleLoggerProvider` конструктор, который были устарел в версии 2.2. Правильное замены для устаревших API регистрации будут доступны в версии 3.0. В то же время можно безопасно пропускать и отключать предупреждения.

После установки соответствующих пакетов, в приложении необходимо создать одноэлементный/глобальный экземпляр LoggerFactory. Например используя средство ведения журнала консоли:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

Этот одноэлементный или глобальный экземпляр затем должен быть зарегистрирован в EF Core на `DbContextOptionsBuilder`. Пример:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> Очень важно, приложения не создают новый экземпляр ILoggerFactory для каждого экземпляра контекста. Это приведет к утечке памяти и снижению производительности.

## <a name="filtering-what-is-logged"></a>Фильтрация, регистрируемых

> [!NOTE]
> В следующем коде используется пример `ConsoleLoggerProvider` конструктор, который были устарел в версии 2.2. Правильное замены для устаревших API регистрации будут доступны в версии 3.0. В то же время можно безопасно пропускать и отключать предупреждения.

Для фильтрации, регистрируемых проще всего настроить его при регистрации ILoggerProvider. Пример:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

В этом примере для возвращения только сообщений фильтруется журнала:
 * в категории «Microsoft.EntityFrameworkCore.Database.Command»
 * на уровне «Сведения»

Категории средства ведения журнала для EF Core, определенных в `DbLoggerCategory` класса, чтобы его легко найти в категории, но эти разрешения в простых строк.

Дополнительные сведения о базовой инфраструктуре ведения журнала можно найти в [документации ведения журнала ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).
