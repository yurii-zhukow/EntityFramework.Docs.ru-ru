---
title: Поддерживаемые реализации .NET — EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/30/2017
ms.technology: entity-framework-core
uid: core/platforms/index
ms.openlocfilehash: 790628c407cc4374fee4ebde8201783955afdcc3
ms.sourcegitcommit: fd50ac53b93a03825dcbb42ed2e7ca95ca858d5f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900334"
---
# <a name="net-implementations-supported-by-ef-core"></a>Реализации .NET, поддерживаемые EF Core

Мы стремимся сделать EF Core доступным везде, где вы можете писать код .NET, и продолжаем работать в этом направлении. Поддержка EF Core в .NET Core и .NET Framework проверена в рамках автоматизированного тестирования и множества случаев успешного применения, однако в Mono, Xamarin и на универсальной платформе Windows по-прежнему присутствуют некоторые проблемы.

В следующей таблице приводятся рекомендации для каждой реализации .NET:

| Реализация .NET                                                                                                  | Status                                                             | Требования для EF Core 1.x                                                                                | Требования для EF Core 2.x <sup>(1)</sup>                                                                 |
|:---------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|
| **.NET Core** ([ASP.NET Core](../get-started/aspnetcore/index.md), [консоль](../get-started/netcore/index.md) и т. д.) | Полностью поддерживается и рекомендуется                                    | [Пакет SDK 1.x для .NET Core](https://www.microsoft.com/net/core/)                                                | [Пакет SDK 2.x для .NET Core](https://www.microsoft.com/net/core/)                                                |
| **.NET Framework** (WinForms, WPF, ASP.NET, [консоль](../get-started/full-dotnet/index.md) и т.  .)                    | Полностью поддерживается и рекомендуется. Также доступна версия EF6 <sup>(2)</sup> | .NET Framework 4.5.1                                                                                    | .NET Framework 4.6.1                                                                                    |
| **Mono и Xamarin**                                                                                                   | В работе <sup>(3)</sup>                                         | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7                               | Mono 5.4 <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5                        |
| [**Универсальная платформа Windows**](../get-started/uwp/index.md)                                                        | Рекомендуется версия EF Core 2.0.1 <sup>(4)</sup>                           | [Пакет .NET Core UWP 5.x](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) | [Пакет .NET Core UWP 6.x](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) |

<sup>(1)</sup> Версия EF Core 2.0 ориентируется на платформу [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard) и поэтому нуждается в поддерживающих ее реализациях .NET.

<sup>(2)</sup> Рекомендации по выбору правильной технологии см. в разделе [Сравнение EF Core и EF6](../../efcore-and-ef6/index.md).

<sup>(3)</sup> В Xamarin присутствуют некоторые проблемы и ограничения, которые могут препятствовать правильной работе некоторых приложений, разработанных с использованием EF Core 2.0. Обходные решения см. в списке [активных проблем](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin).

<sup>(4)</sup> В ранних версиях EF Core и универсальной платформы Windows .NET присутствовали некоторые проблемы с совместимостью, особенно в отношении приложений, скомпилированных с использованием цепочки инструментов .NET Native. В новой версии универсальной платформы Windows .NET добавлена поддержка платформы .NET Standard 2.0 и содержится .NET Native 2.0, где исправлено большинство ранее выявленных проблем с совместимостью. Было проведено более тщательное тестирование EF Core 2.0.1 с универсальной платформой Windows, однако эти тесты не были автоматизированными.

Если какое-либо сочетание работает неправильно, мы рекомендуем добавить новые вопросы в [средство отслеживания вопросов EF Core](https://github.com/aspnet/entityframeworkcore/issues/new). Для проблем, относящихся к Xamarin, используйте средство отслеживания проблем [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) или [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).
