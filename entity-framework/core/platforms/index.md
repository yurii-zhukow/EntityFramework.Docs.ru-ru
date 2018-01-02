---
title: "Поддерживаемые реализации .NET — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 08/30/2017
ms.technology: entity-framework-core
uid: core/platforms/index
ms.openlocfilehash: 6b6ea9ca833810169d0d850f3bea8776b761c007
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="net-implementations-supported-by-ef-core"></a>Реализации .NET, поддерживаемые EF Core

Мы стремимся сделать EF Core доступным везде, где вы можете писать код .NET, и продолжаем работать в этом направлении. Приведенная ниже таблица содержит рекомендации для каждой реализации .NET, где требуется включить EF Core.

EF Core 2.0 ориентируется на платформу [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard) и поэтому нуждается в поддерживающих ее реализациях .NET.

| Реализация .NET | Status | Требуется для 1.x | Требуется для 2.x
|-|-|-|-
| **.NET Core** ([ASP.NET Core](../get-started/aspnetcore/index.md), [консоль](../get-started/netcore/index.md) и т. д.) | **Полностью поддерживается и рекомендуется:** охвачена автоматическим тестированием, а многие приложения успешно ее используют. | [Пакет SDK 1.x для .NET Core](https://www.microsoft.com/net/core/) | [Пакет SDK 2.x для .NET Core](https://www.microsoft.com/net/core/)
| **.NET Framework** (WinForms, WPF, ASP.NET, [консоль](../get-started/full-dotnet/index.md) и т.  .) | **Полностью поддерживается и рекомендуется:** охвачена автоматическим тестированием, а многие приложения успешно ее используют. На этой платформе также доступна EF 6 (для выбора правильной технологии см. раздел [Сравнение EF Core и EF6](../../efcore-and-ef6/index.md)). | .NET Framework 4.5.1 | .NET Framework 4.6.1
| **Mono и Xamarin** | **В разработке — могут возникнуть проблемы:** клиентами и командами EF Core был выполнен некоторый объем тестирования. Ранние последователи сообщили об определенном успехе, но [некоторые проблемы уже возникли](https://github.com/aspnet/entityframework/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) и, скорее всего, возникнут в ходе тестирования. В частности, в Xamarin.iOS имеются ограничения, которые могут препятствовать правильной работе некоторых приложений, разработанных с использованием EF Core 2.0. | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7 | Mono 5.4 <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5
| [**Универсальная платформа Windows**](../get-started/uwp/index.md) | **В разработке — могут возникнуть проблемы:** клиентами и командами EF Core был выполнен некоторый объем тестирования. [Проблемы](https://github.com/aspnet/entityframework/issues?utf8=%E2%9C%93&q=is%3Aopen%20is%3Aissue%20label%3Aarea-uwp%20) имели место при компиляции с помощью цепочки инструментов .NET Native, которая обычно используется при сборке выпуска и является обязательной для развертывания в Магазине Windows (если вы не используете .NET Native или просто хотите поэкспериментировать, многие проблемы вас не затронут). | [Последний пакет .NET UWP 5](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/5.4.1) | [Последний пакет .NET UWP 6](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) <sup>(1)</sup>

<sup>(1) </sup> Эта версия .NET UWP добавляет поддержку для .NET Standard 2.0 и содержит .NET Native 2.0, устраняющий большинство отмеченных ранее проблем совместимости, но тестирование выявило [несколько оставшихся проблем](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A2.0.1+label%3Aarea-uwp) с EF Core 2.0, которые мы планируем устранить в предстоящем выпуске исправления.

Если какое-либо сочетание работает неправильно, мы рекомендуем добавить новые вопросы в [средство отслеживания вопросов EF Core](https://github.com/aspnet/entityframeworkcore/issues/new), а вопросы, связанные с Xamarin, — в [средство отслеживания вопросов Xamarin](https://bugzilla.xamarin.com/newbug).
