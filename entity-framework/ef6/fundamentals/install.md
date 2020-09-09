---
title: Получение Entity Framework EF6
description: Получение Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
uid: ef6/fundamentals/install
ms.openlocfilehash: bfd016a93de73ada4487a454ec7abd9251aeecfe
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89616251"
---
# <a name="get-entity-framework"></a>Установка Entity Framework
Entity Framework состоит из средств EF для Visual Studio и среды выполнения EF.

## <a name="ef-tools-for-visual-studio"></a>Средства EF для Visual Studio

Entity Framework Tools для Visual Studio включает конструктор EF и мастер модели EF и необходимы для первой базы данных и моделирования первых рабочих процессов. Средства EF включены во все последние версии Visual Studio. При выполнении выборочной установки Visual Studio необходимо убедиться, что выбран пункт "Entity Framework 6 инструментов", выбрав рабочую нагрузку, содержащую ее, или выбрав ее в качестве отдельного компонента.

В некоторых прошлых версиях Visual Studio обновленные средства EF доступны для загрузки. Сведения о том, как получить последнюю версию средств EF, доступных для вашей версии Visual Studio, см. в статье [версии Visual Studio](xref:ef6/what-is-new/visual-studio) .

## <a name="ef-runtime"></a>Среда выполнения EF

Последняя версия Entity Framework доступна в качестве [пакета NuGet EntityFramework](https://nuget.org/packages/EntityFramework/). Если вы не знакомы с диспетчером пакетов NuGet, рекомендуем ознакомиться с [обзором NuGet](/nuget/consume-packages/overview-and-workflow).

### <a name="installing-the-ef-nuget-package"></a>Установка пакета EF NuGet

Вы можете установить пакет EntityFramework, щелкнув правой кнопкой мыши папку **References** проекта и выбрав пункт **Управление пакетами NuGet...**

![Управление пакетами NuGet](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a>Установка из консоли диспетчера пакетов

Кроме того, можно установить EntityFramework, выполнив следующую команду в [консоли диспетчера пакетов](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a>Установка определенной версии EF

Начиная с EF 4,1, в качестве [пакета NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/)были выпущены новые версии среды выполнения EF. Любую из этих версий можно добавить в проект на основе .NET Framework, выполнив следующую команду в [консоли диспетчера пакетов](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)Visual Studio:

``` powershell
Install-Package EntityFramework -Version <number>
```

Обратите внимание, что `<number>` представляет конкретную версию EF для установки. Например, 6.2.0 — это версия Number для EF 6,2.   

Среды выполнения EF до 4,1 были частью .NET Framework и не могут быть установлены отдельно.

### <a name="installing-the-latest-preview"></a>Установка последней предварительной версии

Приведенные выше методы предоставляют последнюю полностью поддерживаемую версию Entity Framework. Часто доступны предварительные версии Entity Framework, которые мы будем рады получить, и поделитесь с нами.

Чтобы установить последнюю предварительную версию EntityFramework, можно выбрать параметр включить предварительные **выпуски** в окне Управление пакетами NuGet. Если предварительные версии недоступны, вы автоматически получите последнюю версию Entity Framework, полностью поддерживаемую.

![Включить предварительные выпуски](~/ef6/media/includeprerelease.png)

Кроме того, можно выполнить следующую команду в [консоли диспетчера пакетов](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework -Pre
```
