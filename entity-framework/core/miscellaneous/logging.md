---
title: Ведение журналов - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
ms.technology: entity-framework-core
uid: core/miscellaneous/logging
ms.openlocfilehash: 60d76bf3360eb47cdd9836494c1f135d1005a215
ms.sourcegitcommit: 3adf1267be92effc3c9daa893906a7f36834204f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35232140"
---
# <a name="logging"></a>Ведение журнала

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) из репозитория GitHub.

## <a name="aspnet-core-applications"></a>Приложения ASP.NET Core

EF Core автоматически интегрируется с механизма ведения журнала для ASP.NET Core всякий раз, когда `AddDbContext` или `AddDbContextPool` используется. Таким образом, при использовании ASP.NET Core, ведения журналов должен настраиваться как описано в [документации ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Другие приложения

Core EF, ведение журнала в настоящее время требует ILoggerFactory, которое, в свою настроены ILoggerProvider один или несколько. В следующих пакетах поставляются общих поставщиков:

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): простое консольное средство ведения журнала.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): службы приложений Azure поддерживает «Журналы диагностики» и «Журнал поток» компоненты.
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): журналы, чтобы отладчик монитор, используя System.Diagnostics.Debug.WriteLine().
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): журналы в журнал событий Windows.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): поддерживает EventSource/EventListener.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): журналы, чтобы прослушиватель трассировки с помощью System.Diagnostics.TraceSource.TraceEvent().

После установки соответствующих пакетов, в приложении необходимо создать одноэлементный/глобальный экземпляр класса LoggerFactory. Например с помощью средства ведения журнала консоли:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

Это одноэлементный/глобальный экземпляр затем должна быть зарегистрирована с основными EF на `DbContextOptionsBuilder`. Пример:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> Очень важно, приложения не создают новый экземпляр ILoggerFactory для каждого экземпляра контекста. Это приведет к утечке памяти и снижению производительности.

## <a name="filtering-what-is-logged"></a>Фильтрация регистрируемых сведений

Чтобы отфильтровать регистрируемых проще всего настроить его при регистрации ILoggerProvider. Пример:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

В этом примере для возвращения только сообщений фильтруется журнала:
 * в категории «Microsoft.EntityFrameworkCore.Database.Command»
 * на уровне «Сведения»

Для основных EF категории ведения журнала определены в `DbLoggerCategory` класса, чтобы облегчить поиск категорий, но эти решения для простых строк.

Дополнительные сведения о базовой инфраструктурой ведения журнала можно найти в [документации ведения журнала ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).
