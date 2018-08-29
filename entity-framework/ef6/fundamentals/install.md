---
title: Получение Entity Framework — EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 91b78e56f60edf7ebc8769b1c385f8547f63cd3d
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152392"
---
# <a name="get-entity-framework"></a>Получение платформы Entity Framework
Платформа Entity Framework состоит из средства EF для Visual Studio и среды выполнения EF.

## <a name="ef-tools-for-visual-studio"></a>Средства EF для Visual Studio

Инструменты Entity Framework для Visual Studio включают конструктор EF и мастера моделей EF и являются обязательными для базы данных сначала модели первый рабочих процессов. Средства EF включены во всех последних версиях Visual Studio. При выполнении настраиваемой установки Visual Studio, вам потребуется убедиться, что элемент «Инструменты Entity Framework 6» выбирается путем либо рабочая нагрузка, которая включает его или выбрав его в качестве отдельного компонента.

Для некоторых предыдущих версий Visual Studio обновленные средства EF доступны для загрузки. См. в разделе [версий Visual Studio](~/ef6/what-is-new/visual-studio.md) рекомендации о том, как получить последнюю версию средства EF, доступные для вашей версии Visual Studio.

## <a name="ef-runtime"></a>Исполняющая среда EF

Последнюю версию Entity Framework доступен в виде [пакета EntityFramework NuGet](http://nuget.org/packages/EntityFramework/). Если вы не знакомы с помощью диспетчера пакетов NuGet, мы рекомендуем вам ознакомиться [Обзор NuGet](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).

### <a name="installing-the-ef-nuget-package"></a>Установка пакета NuGet EF

Можно установить пакет EntityFramework щелкните правой кнопкой мыши **ссылки** папку проекта и выбрав **управление пакетами NuGet...**

![ManageNuGetPackages](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a>Установка консоли диспетчера пакетов

Кроме того, можно установить, выполнив следующую команду в EntityFramework [консоль диспетчера пакетов](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a>Установка определенной версии EF

Новые версии среды выполнения EF из EF версии 4.1 и более поздних версий были выпущены как [пакета NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/). Любой из этих версий можно добавить в проект на основе .NET Framework, выполнив следующую команду в Visual Studio [консоль диспетчера пакетов](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):

``` powershell
Install-Package EntityFramework -Version <number>
```

Обратите внимание, что `<number>` представляет определенную версию EF для установки. Например 6.2.0 — версия номер для EF 6.2.   

EF сред выполнения, прежде чем 4.1 были частью .NET Framework и не может устанавливаться отдельно.

### <a name="installing-the-latest-preview"></a>Установка последней предварительной версии

Указанные выше методы позволит получить последнюю версию полностью поддерживается версия Entity Framework. Часто возникают предварительных версиях Entity Framework, что мы будем рады вас опробовать и Присылайте ваши отзывы о доступных.

Чтобы установить последнюю предварительную версию можно выбрать EntityFramework **включить предварительный выпуск** в окне «Управление пакетами NuGet». Если доступны без предварительной версии вы автоматически получите последнюю версию полностью поддерживаемую версию Entity Framework.

![IncludePreRelease](~/ef6/media/includeprerelease.png)

Также можно запустить следующую команду [консоль диспетчера пакетов](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework -Pre
```
