---
title: Поддерживаемые реализации .NET — EF Core
description: Сведения о поддерживаемых платформах в разных версиях Entity Framework Core
author: bricelam
ms.date: 06/26/2020
uid: core/miscellaneous/platforms
ms.openlocfilehash: 6b888843d491805e735fd903253d7f10c028dd9f
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94431281"
---
# <a name="net-implementations-supported-by-ef-core"></a>Реализации .NET, поддерживаемые EF Core

Мы стремимся обеспечить поддержку EF Core во всех современных реализациях .NET и продолжаем работу над этим. Поддержка EF Core в .NET Core подтверждена результатами автоматизированного тестирования и нормальной работой многих приложений, тогда как с использованием этого решения с Mono, Xamarin и UWP возникают некоторые проблемы.

## <a name="overview"></a>Обзор

В следующей таблице приводятся рекомендации для каждой реализации .NET:

| EF Core                       | 2.1 и 3.1 | 5.0             |
|:------------------------------|:------------|:----------------|
| .NET Standard                 | 2.0         | 2.1             |
| .NET Core                     | 2.0         | 3.0             |
| .NET Framework<sup>(1)</sup>  | 4.7.2       | (Не поддерживается) |
| Mono                          | 5,4         | 6.4             |
| Xamarin.iOS<sup>(2)</sup>     | 10.14       | 12.16           |
| Xamarin.Mac<sup>(2)</sup>     | 3.8         | 5.16            |
| Xamarin.Android<sup>(2)</sup> | 8.0         | 10.0            |
| UWP<sup>(3)</sup>             | 10.0.16299  | TBD             |
| Unity<sup>(4)</sup>           | 2018.1      | TBD             |

<sup>(1)</sup> См. раздел [.NET Framework](#net-framework) далее.

<sup>(2)</sup> При использовании с Xamarin возникают некоторые проблемы и ограничения, которые могут препятствовать правильной работе некоторых приложений, разработанных с использованием EF Core. Обходные решения см. в списке [активных проблем](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin).

<sup>(3)</sup> Рекомендуется использовать EF Core 2.0.1 и последующих версий. Установите [пакет .NET Core UWP 6.x](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/). См. раздел [Универсальная платформа Windows](#universal-windows-platform) далее.

<sup>(4)</sup> В Unity существуют проблемы и известные ограничения. См. список [актуальных проблем](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-unity).

## <a name="net-framework"></a>.NET Framework

Приложения, предназначенные для .NET Framework, могут потребовать внесения изменений для работы с библиотеками .NET Standard:

Измените файл проекта и включите в группу начальной свойств следующую запись.

```xml
<AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
```

Для тестовых проектов также включите следующую запись.

```xml
<GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
```

Если вы хотите использовать старую версию Visual Studio, [обновите клиент NuGet до версии 3.6.0](https://www.nuget.org/downloads) для работы с библиотеками .NET Standard 2.0.

Мы также рекомендуем вместо NuGet packages.config по возможности использовать PackageReference. Добавьте в файл проекта следующее свойство:

```xml
<RestoreProjectStyle>PackageReference</RestoreProjectStyle>
```

## <a name="universal-windows-platform"></a>Универсальная платформа Windows

В ранних версиях EF Core и .NET UWP присутствовали некоторые проблемы с совместимостью, особенно в отношении приложений, скомпилированных с использованием цепочки инструментов .NET Native. В новой версии универсальной платформы Windows .NET добавлена поддержка платформы .NET Standard 2.0 и содержится .NET Native 2.0, где исправлено большинство ранее выявленных проблем с совместимостью. Было проведено более тщательное тестирование EF Core 2.0.1 с универсальной платформой Windows, однако эти тесты не были автоматизированными.

При использовании EF Core в UWP:

* Чтобы оптимизировать производительность запросов, не используйте анонимные типы в запросах LINQ. Чтобы развернуть приложение UWP в магазине приложений, оно должно быть скомпилировано в .NET Native. Запросы с анонимными типами плохо работают в среде .NET Native.

* Чтобы оптимизировать производительность `SaveChanges()`, используйте [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) и реализуйте [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging ](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx) и [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) в типах сущностей.

## <a name="report-issues"></a>Сообщить о проблеме

Если какое-либо сочетание компонентов не работает должным образом, рекомендуем создать проблему на [трекере EF Core](https://github.com/dotnet/efcore/issues/new). Для проблем, относящихся к Xamarin, используйте средство отслеживания проблем [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) или [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).
