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
# <a name="get-entity-framework"></a><span data-ttu-id="379b4-103">Установка Entity Framework</span><span class="sxs-lookup"><span data-stu-id="379b4-103">Get Entity Framework</span></span>
<span data-ttu-id="379b4-104">Entity Framework состоит из средств EF для Visual Studio и среды выполнения EF.</span><span class="sxs-lookup"><span data-stu-id="379b4-104">Entity Framework is made up of the EF Tools for Visual Studio and the EF Runtime.</span></span>

## <a name="ef-tools-for-visual-studio"></a><span data-ttu-id="379b4-105">Средства EF для Visual Studio</span><span class="sxs-lookup"><span data-stu-id="379b4-105">EF Tools for Visual Studio</span></span>

<span data-ttu-id="379b4-106">[Entity Framework Tools для Visual Studio](/visualstudio/data-tools/entity-data-model-tools-in-visual-studio) включает конструктор EF и мастер модели EF и необходимы для первой базы данных и моделирования первых рабочих процессов.</span><span class="sxs-lookup"><span data-stu-id="379b4-106">The [Entity Framework Tools for Visual Studio](/visualstudio/data-tools/entity-data-model-tools-in-visual-studio) include the EF Designer and the EF Model Wizard and are required for the database first and model first workflows.</span></span> <span data-ttu-id="379b4-107">Средства EF включены во все последние версии Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="379b4-107">EF Tools are included in all recent versions of Visual Studio.</span></span> <span data-ttu-id="379b4-108">При выполнении выборочной установки Visual Studio необходимо убедиться, что выбран пункт "Entity Framework 6 инструментов", выбрав рабочую нагрузку, содержащую ее, или выбрав ее в качестве отдельного компонента.</span><span class="sxs-lookup"><span data-stu-id="379b4-108">If you perform a custom install of Visual Studio you will need to ensure that the item "Entity Framework 6 Tools" is selected by either choosing a workload that includes it or by selecting it as an individual component.</span></span>

<span data-ttu-id="379b4-109">В некоторых прошлых версиях Visual Studio обновленные средства EF доступны для загрузки.</span><span class="sxs-lookup"><span data-stu-id="379b4-109">For some past versions of Visual Studio, updated EF Tools are available as a download.</span></span> <span data-ttu-id="379b4-110">Сведения о том, как получить последнюю версию средств EF, доступных для вашей версии Visual Studio, см. в статье [версии Visual Studio](xref:ef6/what-is-new/visual-studio) .</span><span class="sxs-lookup"><span data-stu-id="379b4-110">See [Visual Studio Versions](xref:ef6/what-is-new/visual-studio) for guidance on how to get the latest version of EF Tools available for your version of Visual Studio.</span></span>

## <a name="ef-runtime"></a><span data-ttu-id="379b4-111">Среда выполнения EF</span><span class="sxs-lookup"><span data-stu-id="379b4-111">EF Runtime</span></span>

<span data-ttu-id="379b4-112">Последняя версия Entity Framework доступна в качестве [пакета NuGet EntityFramework](https://nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="379b4-112">The latest version of Entity Framework is available as the [EntityFramework NuGet package](https://nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="379b4-113">Если вы не знакомы с диспетчером пакетов NuGet, рекомендуем ознакомиться с [обзором NuGet](/nuget/consume-packages/overview-and-workflow).</span><span class="sxs-lookup"><span data-stu-id="379b4-113">If you are not familiar with the NuGet Package Manager, we encourage you to read the [NuGet Overview](/nuget/consume-packages/overview-and-workflow).</span></span>

### <a name="installing-the-ef-nuget-package"></a><span data-ttu-id="379b4-114">Установка пакета EF NuGet</span><span class="sxs-lookup"><span data-stu-id="379b4-114">Installing the EF NuGet Package</span></span>

<span data-ttu-id="379b4-115">Вы можете установить пакет EntityFramework, щелкнув правой кнопкой мыши папку **References** проекта и выбрав пункт **Управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="379b4-115">You can install the EntityFramework package by right-clicking on the **References** folder of your project and selecting **Manage NuGet Packages…**</span></span>

![Управление пакетами NuGet](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a><span data-ttu-id="379b4-117">Установка из консоли диспетчера пакетов</span><span class="sxs-lookup"><span data-stu-id="379b4-117">Installing from Package Manager Console</span></span>

<span data-ttu-id="379b4-118">Кроме того, можно установить EntityFramework, выполнив следующую команду в [консоли диспетчера пакетов](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="379b4-118">Alternatively, you can install EntityFramework by running the following command in the [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a><span data-ttu-id="379b4-119">Установка определенной версии EF</span><span class="sxs-lookup"><span data-stu-id="379b4-119">Installing a specific version of EF</span></span>

<span data-ttu-id="379b4-120">Начиная с EF 4,1, в качестве [пакета NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/)были выпущены новые версии среды выполнения EF.</span><span class="sxs-lookup"><span data-stu-id="379b4-120">From EF 4.1 onwards, new versions of the EF runtime have been released as the [EntityFramework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="379b4-121">Любую из этих версий можно добавить в проект на основе .NET Framework, выполнив следующую команду в [консоли диспетчера пакетов](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="379b4-121">Any of those versions can be added to a .NET Framework-based project by running the following command in Visual Studio's [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span></span>

``` powershell
Install-Package EntityFramework -Version <number>
```

<span data-ttu-id="379b4-122">Обратите внимание, что `<number>` представляет конкретную версию EF для установки.</span><span class="sxs-lookup"><span data-stu-id="379b4-122">Note that `<number>` represents the specific version of EF to install.</span></span> <span data-ttu-id="379b4-123">Например, 6.2.0 — это версия Number для EF 6,2.</span><span class="sxs-lookup"><span data-stu-id="379b4-123">For example, 6.2.0 is the version of number for EF 6.2.</span></span>   

<span data-ttu-id="379b4-124">Среды выполнения EF до 4,1 были частью .NET Framework и не могут быть установлены отдельно.</span><span class="sxs-lookup"><span data-stu-id="379b4-124">EF runtimes before 4.1 were part of .NET Framework and cannot be installed separately.</span></span>

### <a name="installing-the-latest-preview"></a><span data-ttu-id="379b4-125">Установка последней предварительной версии</span><span class="sxs-lookup"><span data-stu-id="379b4-125">Installing the Latest Preview</span></span>

<span data-ttu-id="379b4-126">Приведенные выше методы предоставляют последнюю полностью поддерживаемую версию Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="379b4-126">The above methods will give you the latest fully supported release of Entity Framework.</span></span> <span data-ttu-id="379b4-127">Часто доступны предварительные версии Entity Framework, которые мы будем рады получить, и поделитесь с нами.</span><span class="sxs-lookup"><span data-stu-id="379b4-127">There are often prerelease versions of Entity Framework available that we would love you to try out and give us feedback on.</span></span>

<span data-ttu-id="379b4-128">Чтобы установить последнюю предварительную версию EntityFramework, можно выбрать параметр включить предварительные **выпуски** в окне Управление пакетами NuGet.</span><span class="sxs-lookup"><span data-stu-id="379b4-128">To install the latest preview of EntityFramework you can select **Include Prerelease** in the Manage NuGet Packages window.</span></span> <span data-ttu-id="379b4-129">Если предварительные версии недоступны, вы автоматически получите последнюю версию Entity Framework, полностью поддерживаемую.</span><span class="sxs-lookup"><span data-stu-id="379b4-129">If no prerelease versions are available you will automatically get the latest fully supported version of Entity Framework.</span></span>

![Включить предварительные выпуски](~/ef6/media/includeprerelease.png)

<span data-ttu-id="379b4-131">Кроме того, можно выполнить следующую команду в [консоли диспетчера пакетов](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="379b4-131">Alternatively, you can run the following command in the [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework -Pre
```
