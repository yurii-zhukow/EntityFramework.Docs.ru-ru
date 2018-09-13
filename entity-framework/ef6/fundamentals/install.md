---
title: Получение Entity Framework — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 7f840a4f9e437ec12f699184339e386976e1528b
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490662"
---
# <a name="get-entity-framework"></a><span data-ttu-id="61b27-102">Получение платформы Entity Framework</span><span class="sxs-lookup"><span data-stu-id="61b27-102">Get Entity Framework</span></span>
<span data-ttu-id="61b27-103">Платформа Entity Framework состоит из средства EF для Visual Studio и среды выполнения EF.</span><span class="sxs-lookup"><span data-stu-id="61b27-103">Entity Framework is made up of the EF Tools for Visual Studio and the EF Runtime.</span></span>

## <a name="ef-tools-for-visual-studio"></a><span data-ttu-id="61b27-104">Средства EF для Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61b27-104">EF Tools for Visual Studio</span></span>

<span data-ttu-id="61b27-105">Инструменты Entity Framework для Visual Studio включают конструктор EF и мастера моделей EF и являются обязательными для базы данных сначала модели первый рабочих процессов.</span><span class="sxs-lookup"><span data-stu-id="61b27-105">The Entity Framework Tools for Visual Studio include the EF Designer and the EF Model Wizard and are required for the database first and model first workflows.</span></span> <span data-ttu-id="61b27-106">Средства EF включены во всех последних версиях Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="61b27-106">EF Tools are included in all recent versions of Visual Studio.</span></span> <span data-ttu-id="61b27-107">При выполнении настраиваемой установки Visual Studio, вам потребуется убедиться, что элемент «Инструменты Entity Framework 6» выбирается путем либо рабочая нагрузка, которая включает его или выбрав его в качестве отдельного компонента.</span><span class="sxs-lookup"><span data-stu-id="61b27-107">If you perform a custom install of Visual Studio you will need to ensure that the item "Entity Framework 6 Tools" is selected by either choosing a workload that includes it or by selecting it as an individual component.</span></span>

<span data-ttu-id="61b27-108">Для некоторых предыдущих версий Visual Studio обновленные средства EF доступны для загрузки.</span><span class="sxs-lookup"><span data-stu-id="61b27-108">For some past versions of Visual Studio, updated EF Tools are available as a download.</span></span> <span data-ttu-id="61b27-109">См. в разделе [версий Visual Studio](~/ef6/what-is-new/visual-studio.md) рекомендации о том, как получить последнюю версию средства EF, доступные для вашей версии Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="61b27-109">See [Visual Studio Versions](~/ef6/what-is-new/visual-studio.md) for guidance on how to get the latest version of EF Tools available for your version of Visual Studio.</span></span>

## <a name="ef-runtime"></a><span data-ttu-id="61b27-110">Исполняющая среда EF</span><span class="sxs-lookup"><span data-stu-id="61b27-110">EF Runtime</span></span>

<span data-ttu-id="61b27-111">Последнюю версию Entity Framework доступен в виде [пакета EntityFramework NuGet](http://nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="61b27-111">The latest version of Entity Framework is available as the [EntityFramework NuGet package](http://nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="61b27-112">Если вы не знакомы с помощью диспетчера пакетов NuGet, мы рекомендуем вам ознакомиться [Обзор NuGet](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span><span class="sxs-lookup"><span data-stu-id="61b27-112">If you are not familiar with the NuGet Package Manager, we encourage you to read the [NuGet Overview](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span></span>

### <a name="installing-the-ef-nuget-package"></a><span data-ttu-id="61b27-113">Установка пакета NuGet EF</span><span class="sxs-lookup"><span data-stu-id="61b27-113">Installing the EF NuGet Package</span></span>

<span data-ttu-id="61b27-114">Можно установить пакет EntityFramework щелкните правой кнопкой мыши **ссылки** папку проекта и выбрав **управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="61b27-114">You can install the EntityFramework package by right-clicking on the **References** folder of your project and selecting **Manage NuGet Packages…**</span></span>

![Управление пакетами NuGet](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a><span data-ttu-id="61b27-116">Установка консоли диспетчера пакетов</span><span class="sxs-lookup"><span data-stu-id="61b27-116">Installing from Package Manager Console</span></span>

<span data-ttu-id="61b27-117">Кроме того, можно установить, выполнив следующую команду в EntityFramework [консоль диспетчера пакетов](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="61b27-117">Alternatively, you can install EntityFramework by running the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a><span data-ttu-id="61b27-118">Установка определенной версии EF</span><span class="sxs-lookup"><span data-stu-id="61b27-118">Installing a specific version of EF</span></span>

<span data-ttu-id="61b27-119">Новые версии среды выполнения EF из EF версии 4.1 и более поздних версий были выпущены как [пакета NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="61b27-119">From EF 4.1 onwards, new versions of the EF runtime have been released as the [EntityFramework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="61b27-120">Любой из этих версий можно добавить в проект на основе .NET Framework, выполнив следующую команду в Visual Studio [консоль диспетчера пакетов](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span><span class="sxs-lookup"><span data-stu-id="61b27-120">Any of those versions can be added to a .NET Framework-based project by running the following command in Visual Studio's [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span></span>

``` powershell
Install-Package EntityFramework -Version <number>
```

<span data-ttu-id="61b27-121">Обратите внимание, что `<number>` представляет определенную версию EF для установки.</span><span class="sxs-lookup"><span data-stu-id="61b27-121">Note that `<number>` represents the specific version of EF to install.</span></span> <span data-ttu-id="61b27-122">Например 6.2.0 — версия номер для EF 6.2.</span><span class="sxs-lookup"><span data-stu-id="61b27-122">For example, 6.2.0 is the version of number for EF 6.2.</span></span>   

<span data-ttu-id="61b27-123">EF сред выполнения, прежде чем 4.1 были частью .NET Framework и не может устанавливаться отдельно.</span><span class="sxs-lookup"><span data-stu-id="61b27-123">EF runtimes before 4.1 were part of .NET Framework and cannot be installed separately.</span></span>

### <a name="installing-the-latest-preview"></a><span data-ttu-id="61b27-124">Установка последней предварительной версии</span><span class="sxs-lookup"><span data-stu-id="61b27-124">Installing the Latest Preview</span></span>

<span data-ttu-id="61b27-125">Указанные выше методы позволит получить последнюю версию полностью поддерживается версия Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="61b27-125">The above methods will give you the latest fully supported release of Entity Framework.</span></span> <span data-ttu-id="61b27-126">Часто возникают предварительных версиях Entity Framework, что мы будем рады вас опробовать и Присылайте ваши отзывы о доступных.</span><span class="sxs-lookup"><span data-stu-id="61b27-126">There are often prerelease versions of Entity Framework available that we would love you to try out and give us feedback on.</span></span>

<span data-ttu-id="61b27-127">Чтобы установить последнюю предварительную версию можно выбрать EntityFramework **включить предварительный выпуск** в окне «Управление пакетами NuGet».</span><span class="sxs-lookup"><span data-stu-id="61b27-127">To install the latest preview of EntityFramework you can select **Include Prerelease** in the Manage NuGet Packages window.</span></span> <span data-ttu-id="61b27-128">Если доступны без предварительной версии вы автоматически получите последнюю версию полностью поддерживаемую версию Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="61b27-128">If no prerelease versions are available you will automatically get the latest fully supported version of Entity Framework.</span></span>

![Включить предварительные выпуски](~/ef6/media/includeprerelease.png)

<span data-ttu-id="61b27-130">Также можно запустить следующую команду [консоль диспетчера пакетов](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="61b27-130">Alternatively, you can run the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework -Pre
```
