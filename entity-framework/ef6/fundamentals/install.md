---
title: Получение Entity Framework EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 2bdec6a9be228fbe934d0f46aa1bfafdfb2c971c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416099"
---
# <a name="get-entity-framework"></a><span data-ttu-id="0c72a-102">Установка Entity Framework</span><span class="sxs-lookup"><span data-stu-id="0c72a-102">Get Entity Framework</span></span>
<span data-ttu-id="0c72a-103">Entity Framework состоит из средств EF для Visual Studio и среды выполнения EF.</span><span class="sxs-lookup"><span data-stu-id="0c72a-103">Entity Framework is made up of the EF Tools for Visual Studio and the EF Runtime.</span></span>

## <a name="ef-tools-for-visual-studio"></a><span data-ttu-id="0c72a-104">Средства EF для Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c72a-104">EF Tools for Visual Studio</span></span>

<span data-ttu-id="0c72a-105">Entity Framework Tools для Visual Studio включает конструктор EF и мастер модели EF и необходимы для первой базы данных и моделирования первых рабочих процессов.</span><span class="sxs-lookup"><span data-stu-id="0c72a-105">The Entity Framework Tools for Visual Studio include the EF Designer and the EF Model Wizard and are required for the database first and model first workflows.</span></span> <span data-ttu-id="0c72a-106">Средства EF включены во все последние версии Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0c72a-106">EF Tools are included in all recent versions of Visual Studio.</span></span> <span data-ttu-id="0c72a-107">При выполнении выборочной установки Visual Studio необходимо убедиться, что выбран пункт "Entity Framework 6 инструментов", выбрав рабочую нагрузку, содержащую ее, или выбрав ее в качестве отдельного компонента.</span><span class="sxs-lookup"><span data-stu-id="0c72a-107">If you perform a custom install of Visual Studio you will need to ensure that the item "Entity Framework 6 Tools" is selected by either choosing a workload that includes it or by selecting it as an individual component.</span></span>

<span data-ttu-id="0c72a-108">В некоторых прошлых версиях Visual Studio обновленные средства EF доступны для загрузки.</span><span class="sxs-lookup"><span data-stu-id="0c72a-108">For some past versions of Visual Studio, updated EF Tools are available as a download.</span></span> <span data-ttu-id="0c72a-109">Сведения о том, как получить последнюю версию средств EF, доступных для вашей версии Visual Studio, см. в статье [версии Visual Studio](~/ef6/what-is-new/visual-studio.md) .</span><span class="sxs-lookup"><span data-stu-id="0c72a-109">See [Visual Studio Versions](~/ef6/what-is-new/visual-studio.md) for guidance on how to get the latest version of EF Tools available for your version of Visual Studio.</span></span>

## <a name="ef-runtime"></a><span data-ttu-id="0c72a-110">Среда выполнения EF</span><span class="sxs-lookup"><span data-stu-id="0c72a-110">EF Runtime</span></span>

<span data-ttu-id="0c72a-111">Последняя версия Entity Framework доступна в качестве [пакета NuGet EntityFramework](https://nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="0c72a-111">The latest version of Entity Framework is available as the [EntityFramework NuGet package](https://nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="0c72a-112">Если вы не знакомы с диспетчером пакетов NuGet, рекомендуем ознакомиться с [обзором NuGet](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span><span class="sxs-lookup"><span data-stu-id="0c72a-112">If you are not familiar with the NuGet Package Manager, we encourage you to read the [NuGet Overview](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span></span>

### <a name="installing-the-ef-nuget-package"></a><span data-ttu-id="0c72a-113">Установка пакета EF NuGet</span><span class="sxs-lookup"><span data-stu-id="0c72a-113">Installing the EF NuGet Package</span></span>

<span data-ttu-id="0c72a-114">Вы можете установить пакет EntityFramework, щелкнув правой кнопкой мыши папку **References** проекта и выбрав пункт **Управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="0c72a-114">You can install the EntityFramework package by right-clicking on the **References** folder of your project and selecting **Manage NuGet Packages…**</span></span>

![Управление пакетами NuGet](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a><span data-ttu-id="0c72a-116">Установка из консоли диспетчера пакетов</span><span class="sxs-lookup"><span data-stu-id="0c72a-116">Installing from Package Manager Console</span></span>

<span data-ttu-id="0c72a-117">Кроме того, можно установить EntityFramework, выполнив следующую команду в [консоли диспетчера пакетов](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="0c72a-117">Alternatively, you can install EntityFramework by running the following command in the [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a><span data-ttu-id="0c72a-118">Установка определенной версии EF</span><span class="sxs-lookup"><span data-stu-id="0c72a-118">Installing a specific version of EF</span></span>

<span data-ttu-id="0c72a-119">Начиная с EF 4,1, в качестве [пакета NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/)были выпущены новые версии среды выполнения EF.</span><span class="sxs-lookup"><span data-stu-id="0c72a-119">From EF 4.1 onwards, new versions of the EF runtime have been released as the [EntityFramework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="0c72a-120">Любую из этих версий можно добавить в проект на основе .NET Framework, выполнив следующую команду в [консоли диспетчера пакетов](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="0c72a-120">Any of those versions can be added to a .NET Framework-based project by running the following command in Visual Studio's [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span></span>

``` powershell
Install-Package EntityFramework -Version <number>
```

<span data-ttu-id="0c72a-121">Обратите внимание, что `<number>` представляет определенную версию EF для установки.</span><span class="sxs-lookup"><span data-stu-id="0c72a-121">Note that `<number>` represents the specific version of EF to install.</span></span> <span data-ttu-id="0c72a-122">Например, 6.2.0 — это версия Number для EF 6,2.</span><span class="sxs-lookup"><span data-stu-id="0c72a-122">For example, 6.2.0 is the version of number for EF 6.2.</span></span>   

<span data-ttu-id="0c72a-123">Среды выполнения EF до 4,1 были частью .NET Framework и не могут быть установлены отдельно.</span><span class="sxs-lookup"><span data-stu-id="0c72a-123">EF runtimes before 4.1 were part of .NET Framework and cannot be installed separately.</span></span>

### <a name="installing-the-latest-preview"></a><span data-ttu-id="0c72a-124">Установка последней предварительной версии</span><span class="sxs-lookup"><span data-stu-id="0c72a-124">Installing the Latest Preview</span></span>

<span data-ttu-id="0c72a-125">Приведенные выше методы предоставляют последнюю полностью поддерживаемую версию Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0c72a-125">The above methods will give you the latest fully supported release of Entity Framework.</span></span> <span data-ttu-id="0c72a-126">Часто доступны предварительные версии Entity Framework, которые мы будем рады получить, и поделитесь с нами.</span><span class="sxs-lookup"><span data-stu-id="0c72a-126">There are often prerelease versions of Entity Framework available that we would love you to try out and give us feedback on.</span></span>

<span data-ttu-id="0c72a-127">Чтобы установить последнюю предварительную версию EntityFramework, можно выбрать параметр включить предварительные **выпуски** в окне Управление пакетами NuGet.</span><span class="sxs-lookup"><span data-stu-id="0c72a-127">To install the latest preview of EntityFramework you can select **Include Prerelease** in the Manage NuGet Packages window.</span></span> <span data-ttu-id="0c72a-128">Если предварительные версии недоступны, вы автоматически получите последнюю версию Entity Framework, полностью поддерживаемую.</span><span class="sxs-lookup"><span data-stu-id="0c72a-128">If no prerelease versions are available you will automatically get the latest fully supported version of Entity Framework.</span></span>

![Включить предварительные выпуски](~/ef6/media/includeprerelease.png)

<span data-ttu-id="0c72a-130">Кроме того, можно выполнить следующую команду в [консоли диспетчера пакетов](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="0c72a-130">Alternatively, you can run the following command in the [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework -Pre
```
