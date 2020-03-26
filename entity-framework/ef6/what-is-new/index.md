---
title: Новые возможности EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: e0367aeefd682434bf520301776bcff4f0e72e06
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136141"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="67453-102">Новые возможности EF6</span><span class="sxs-lookup"><span data-stu-id="67453-102">What's new in EF6</span></span>

<span data-ttu-id="67453-103">Корпорация Майкрософт настоятельно рекомендует использовать последнюю выпущенную версию Entity Framework, чтобы вы могли использовать новые функции с высочайшим уровнем надежности.</span><span class="sxs-lookup"><span data-stu-id="67453-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="67453-104">Тем не менее мы понимаем, что вам может потребоваться предыдущая версия или вы захотите поэкспериментировать с новыми усовершенствованиями в последнем предварительном выпуске.</span><span class="sxs-lookup"><span data-stu-id="67453-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="67453-105">Чтобы установить определенные версии EF, см. сведения в разделе о [получении Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="67453-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-640"></a><span data-ttu-id="67453-106">EF 6.4.0</span><span class="sxs-lookup"><span data-stu-id="67453-106">EF 6.4.0</span></span>

<span data-ttu-id="67453-107">Среда выполнения EF 6.4.0 была выпущена в NuGet в декабре 2019 г.</span><span class="sxs-lookup"><span data-stu-id="67453-107">The EF 6.4.0 runtime was released to NuGet in December  2019.</span></span> <span data-ttu-id="67453-108">Основной целью EF 6.4 является представление улучшенных функций и сценариев, которые были выпущены в EF 6.3.</span><span class="sxs-lookup"><span data-stu-id="67453-108">The primary goal of EF 6.4 is to polish the features and scenarios that was delivered in EF 6.3.</span></span> <span data-ttu-id="67453-109">См. [список важных исправлений](https://github.com/dotnet/ef6/milestone/14?closed=1) на сайте GitHub.</span><span class="sxs-lookup"><span data-stu-id="67453-109">See [list of important fixes](https://github.com/dotnet/ef6/milestone/14?closed=1) on Github.</span></span>

## <a name="ef-630"></a><span data-ttu-id="67453-110">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="67453-110">EF 6.3.0</span></span>

<span data-ttu-id="67453-111">Среда выполнения EF 6.3.0 выпущена в NuGet в октябре 2019 г.</span><span class="sxs-lookup"><span data-stu-id="67453-111">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="67453-112">Основной целью этого выпуска было упрощение переноса существующих приложений, использующих EF 6, в .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="67453-112">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="67453-113">Сообществом также было внесено несколько исправлений и усовершенствований.</span><span class="sxs-lookup"><span data-stu-id="67453-113">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="67453-114">См. сведения о проблемах, устраненных в каждой [контрольной точке](https://github.com/aspnet/EntityFramework6/milestones?state=closed) версии 6.3.0.</span><span class="sxs-lookup"><span data-stu-id="67453-114">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="67453-115">Ниже перечислены некоторые основные моменты:</span><span class="sxs-lookup"><span data-stu-id="67453-115">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="67453-116">Включена поддержка .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="67453-116">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="67453-117">Теперь пакет Entity Framework можно использовать для .NET Standard 2.1 наряду с .NET Framework 4.x.</span><span class="sxs-lookup"><span data-stu-id="67453-117">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x.</span></span>
  - <span data-ttu-id="67453-118">Это означает, что платформа EF 6.3 является кроссплатформенной и поддерживается не только в Windows, но и в других операционных системах, таких как Linux и macOS.</span><span class="sxs-lookup"><span data-stu-id="67453-118">This means that EF 6.3 is cross-platform and supported on other operating systems besides Windows, like Linux and macOS.</span></span>
  - <span data-ttu-id="67453-119">Команды миграции были переписаны: теперь они поддерживают внепроцессное выполнение и проекты на базе пакетов SDK.</span><span class="sxs-lookup"><span data-stu-id="67453-119">The migrations commands have been rewritten to execute out of process and work with SDK-style projects.</span></span>
- <span data-ttu-id="67453-120">Поддержка HierarchyId SQL Server.</span><span class="sxs-lookup"><span data-stu-id="67453-120">Support for SQL Server HierarchyId.</span></span>
- <span data-ttu-id="67453-121">Улучшена совместимость с Roslyn и NuGet PackageReference.</span><span class="sxs-lookup"><span data-stu-id="67453-121">Improved compatibility with Roslyn and NuGet PackageReference.</span></span>
- <span data-ttu-id="67453-122">Добавлена служебная программа `ef6.exe` для включения, добавления, создания скриптов и применения миграций из сборок.</span><span class="sxs-lookup"><span data-stu-id="67453-122">Added `ef6.exe` utility for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="67453-123">Это замена для `migrate.exe`.</span><span class="sxs-lookup"><span data-stu-id="67453-123">This replaces `migrate.exe`.</span></span>

### <a name="ef-designer-support"></a><span data-ttu-id="67453-124">Поддержка конструктора EF</span><span class="sxs-lookup"><span data-stu-id="67453-124">EF designer support</span></span>

<span data-ttu-id="67453-125">Сейчас использование конструктора EF непосредственно в проектах .NET Core, .NET Standard или проекте .NET Framework в стиле SDK не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="67453-125">There's currently no support for using the EF designer directly on .NET Core or .NET Standard projects or on an SDK-style .NET Framework project.</span></span> 

<span data-ttu-id="67453-126">Это ограничение можно обойти, добавив EDMX-файл и созданные классы для сущностей, а также DbContext в качестве связанных файлов в проект .NET Core 3.0 или .NET Standard 2.1 в одном решении.</span><span class="sxs-lookup"><span data-stu-id="67453-126">You can work around this limitation by adding the EDMX file and the generated classes for the entities and the DbContext as linked files to a .NET Core 3.0 or .NET Standard 2.1 project in the same solution.</span></span>

<span data-ttu-id="67453-127">В файле проекта связанные файлы будут выглядеть так:</span><span class="sxs-lookup"><span data-stu-id="67453-127">The linked files will look like this in the project file:</span></span>

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

<span data-ttu-id="67453-128">Обратите внимание, что EDMX-файл связан с действием сборки EntityDeploy.</span><span class="sxs-lookup"><span data-stu-id="67453-128">Note that the EDMX file is linked with the EntityDeploy build action.</span></span> <span data-ttu-id="67453-129">Это специальная задача MSBuild (теперь включена в пакет EF 6.3), которая добавляет модель EF в целевую сборку в качестве внедренных ресурсов (или копирует ее как файлы в выходную папку в зависимости от параметра обработки артефакта метаданных в EDMX).</span><span class="sxs-lookup"><span data-stu-id="67453-129">This is a special MSBuild task (now included in the EF 6.3 package) that takes care of adding the EF model into the target assembly as embedded resources (or copying it as files in the output folder, depending on the Metadata Artifact Processing setting in the EDMX).</span></span> <span data-ttu-id="67453-130">См. сведения об этом параметре в [примере EDMX .NET Core](https://aka.ms/EdmxDotNetCoreSample).</span><span class="sxs-lookup"><span data-stu-id="67453-130">For more details on how to get this set up, see our [EDMX .NET Core sample](https://aka.ms/EdmxDotNetCoreSample).</span></span>

<span data-ttu-id="67453-131">Внимание! Убедитесь, что проект .NET Framework в старом стиле (т. е. не в стиле SDK), определяющий реальный EDMX-файл, _предшествует_ проекту, определяющему ссылку в SLN-файле.</span><span class="sxs-lookup"><span data-stu-id="67453-131">Warning: make sure the old style (i.e. non-SDK-style) .NET Framework project defining the "real" .edmx file comes _before_ the project defining the link inside the .sln file.</span></span> <span data-ttu-id="67453-132">В противном случае при открытии EDMX-файла в конструкторе появится сообщение об ошибке: "The Entity Framework is not available in the target framework currently specified for the project.</span><span class="sxs-lookup"><span data-stu-id="67453-132">Otherwise, when you open the .edmx file in the designer, you see the error message "The Entity Framework is not available in the target framework currently specified for the project.</span></span> <span data-ttu-id="67453-133">You can change the target framework of the project or edit the model in the XmlEditor" (Платформа Entity Framework недоступна на целевой платформе, указанной в настоящий момент для проекта. Вы можете изменить требуемую версию .NET Framework для проекта или изменить модель в XmlEditor.)</span><span class="sxs-lookup"><span data-stu-id="67453-133">You can change the target framework of the project or edit the model in the XmlEditor".</span></span>

## <a name="past-releases"></a><span data-ttu-id="67453-134">Прошлые выпуски</span><span class="sxs-lookup"><span data-stu-id="67453-134">Past Releases</span></span>

<span data-ttu-id="67453-135">На странице [прошлых выпусков](past-releases.md) содержится архив всех предыдущих версий EF и основных функций, которые были представлены в каждом выпуске.</span><span class="sxs-lookup"><span data-stu-id="67453-135">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
