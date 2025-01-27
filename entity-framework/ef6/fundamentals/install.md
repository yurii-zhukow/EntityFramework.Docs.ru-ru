---
title: Получение Entity Framework EF6
description: Получение Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/fundamentals/install
ms.openlocfilehash: 7dc4c560cc4eb24a1c5d5dac0c9e28883de471b0
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92065463"
---
# <a name="get-entity-framework"></a>Установка Entity Framework
Entity Framework состоит из средств EF для Visual Studio и среды выполнения EF.

## <a name="ef-tools-for-visual-studio"></a>Средства EF для Visual Studio

[Entity Framework Tools для Visual Studio](/visualstudio/data-tools/entity-data-model-tools-in-visual-studio) включает конструктор EF и мастер модели EF и необходимы для первой базы данных и моделирования первых рабочих процессов. Средства EF включены во все последние версии Visual Studio. При выполнении выборочной установки Visual Studio необходимо убедиться, что выбран пункт "Entity Framework 6 инструментов", выбрав рабочую нагрузку, содержащую ее, или выбрав ее в качестве отдельного компонента.

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
