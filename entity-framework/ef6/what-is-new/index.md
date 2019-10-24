---
title: Новые возможности EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: 9daae787d0cec0ca536413e6263bb363ba76ff2c
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812134"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="79099-102">Новые возможности EF6</span><span class="sxs-lookup"><span data-stu-id="79099-102">What's new in EF6</span></span>

<span data-ttu-id="79099-103">Корпорация Майкрософт настоятельно рекомендует использовать последнюю выпущенную версию Entity Framework, чтобы вы могли использовать новые функции с высочайшим уровнем надежности.</span><span class="sxs-lookup"><span data-stu-id="79099-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="79099-104">Тем не менее мы понимаем, что вам может потребоваться предыдущая версия или вы захотите поэкспериментировать с новыми усовершенствованиями в последнем предварительном выпуске.</span><span class="sxs-lookup"><span data-stu-id="79099-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="79099-105">Чтобы установить определенные версии EF, см. сведения в разделе о [получении Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="79099-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-630"></a><span data-ttu-id="79099-106">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="79099-106">EF 6.3.0</span></span>

<span data-ttu-id="79099-107">Среда выполнения EF 6.3.0 выпущена в NuGet в октябре 2019 г.</span><span class="sxs-lookup"><span data-stu-id="79099-107">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="79099-108">Основной целью этого выпуска было упрощение переноса существующих приложений, использующих EF 6, в .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="79099-108">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="79099-109">Сообществом также было внесено несколько исправлений и усовершенствований.</span><span class="sxs-lookup"><span data-stu-id="79099-109">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="79099-110">См. сведения о проблемах, устраненных в каждой [контрольной точке](https://github.com/aspnet/EntityFramework6/milestones?state=closed) версии 6.3.0.</span><span class="sxs-lookup"><span data-stu-id="79099-110">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="79099-111">Ниже перечислены некоторые основные моменты:</span><span class="sxs-lookup"><span data-stu-id="79099-111">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="79099-112">Включена поддержка .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="79099-112">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="79099-113">Теперь пакет Entity Framework можно использовать для .NET Standard 2.1 наряду с .NET Framework 4.x.</span><span class="sxs-lookup"><span data-stu-id="79099-113">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x.</span></span>
  - <span data-ttu-id="79099-114">Это означает, что платформа EF 6.3 является кроссплатформенной и поддерживается не только в Windows, но и в других операционных системах, таких как Linux и macOS.</span><span class="sxs-lookup"><span data-stu-id="79099-114">This means that EF 6.3 is cross-platform and supported on other operating systems besides Windows, like Linux and macOS.</span></span>
  - <span data-ttu-id="79099-115">Команды миграции были переписаны: теперь они поддерживают внепроцессное выполнение и проекты на базе пакетов SDK.</span><span class="sxs-lookup"><span data-stu-id="79099-115">The migrations commands have been rewritten to execute out of process and work with SDK-style projects.</span></span>
- <span data-ttu-id="79099-116">Поддержка HierarchyId SQL Server.</span><span class="sxs-lookup"><span data-stu-id="79099-116">Support for SQL Server HierarchyId.</span></span>
- <span data-ttu-id="79099-117">Улучшена совместимость с Roslyn и NuGet PackageReference.</span><span class="sxs-lookup"><span data-stu-id="79099-117">Improved compatibility with Roslyn and NuGet PackageReference.</span></span>
- <span data-ttu-id="79099-118">Добавлена служебная программа `ef6.exe` для включения, добавления, создания скриптов и применения миграций из сборок.</span><span class="sxs-lookup"><span data-stu-id="79099-118">Added `ef6.exe` utility for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="79099-119">Это замена для `migrate.exe`.</span><span class="sxs-lookup"><span data-stu-id="79099-119">This replaces `migrate.exe`.</span></span>

### <a name="ef-designer-support"></a><span data-ttu-id="79099-120">Поддержка конструктора EF</span><span class="sxs-lookup"><span data-stu-id="79099-120">EF designer support</span></span>

<span data-ttu-id="79099-121">Сейчас использование конструктора EF непосредственно в проектах .NET Core или .NET Standard не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="79099-121">There's currently no support for using the EF designer directly on .NET Core or .NET Standard projects.</span></span> 

<span data-ttu-id="79099-122">Это ограничение можно обойти, добавив EDMX-файл и созданные классы для сущностей, а также DbContext в качестве связанных файлов в проект .NET Core 3.0 или .NET Standard 2.1 в одном решении.</span><span class="sxs-lookup"><span data-stu-id="79099-122">You can work around this limitation by adding the EDMX file and the generated classes for the entities and the DbContext as linked files to a .NET Core 3.0 or .NET Standard 2.1 project in the same solution.</span></span>

<span data-ttu-id="79099-123">В файле проекта связанные файлы будут выглядеть так:</span><span class="sxs-lookup"><span data-stu-id="79099-123">The linked files will look like this in the project file:</span></span>

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

<span data-ttu-id="79099-124">Обратите внимание, что EDMX-файл связан с действием сборки EntityDeploy.</span><span class="sxs-lookup"><span data-stu-id="79099-124">Note that the EDMX file is linked with the EntityDeploy build action.</span></span> <span data-ttu-id="79099-125">Это специальная задача MSBuild (теперь включена в пакет EF 6.3), которая добавляет модель EF в целевую сборку в качестве внедренных ресурсов (или копирует ее как файлы в выходную папку в зависимости от параметра обработки артефакта метаданных в EDMX).</span><span class="sxs-lookup"><span data-stu-id="79099-125">This is a special MSBuild task (now included in the EF 6.3 package) that takes care of adding the EF model into the target assembly as embedded resources (or copying it as files in the output folder, depending on the Metadata Artifact Processing setting in the EDMX).</span></span> <span data-ttu-id="79099-126">См. сведения об этом параметре в [примере EDMX .NET Core](https://aka.ms/EdmxDotNetCoreSample).</span><span class="sxs-lookup"><span data-stu-id="79099-126">For more details on how to get this set up, see our [EDMX .NET Core sample](https://aka.ms/EdmxDotNetCoreSample).</span></span>

## <a name="past-releases"></a><span data-ttu-id="79099-127">Прошлые выпуски</span><span class="sxs-lookup"><span data-stu-id="79099-127">Past Releases</span></span>

<span data-ttu-id="79099-128">На странице [прошлых выпусков](past-releases.md) содержится архив всех предыдущих версий EF и основных функций, которые были представлены в каждом выпуске.</span><span class="sxs-lookup"><span data-stu-id="79099-128">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
