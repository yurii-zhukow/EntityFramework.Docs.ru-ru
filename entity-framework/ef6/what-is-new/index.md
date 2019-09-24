---
title: Новые возможности EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 568790d9c9bb7dd2213907bef8fa090710cd3ba0
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149118"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="72f20-102">Новые возможности EF6</span><span class="sxs-lookup"><span data-stu-id="72f20-102">What's New in EF6</span></span>

<span data-ttu-id="72f20-103">Корпорация Майкрософт настоятельно рекомендует использовать последнюю выпущенную версию Entity Framework, чтобы вы могли использовать новые функции с высочайшим уровнем надежности.</span><span class="sxs-lookup"><span data-stu-id="72f20-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="72f20-104">Тем не менее мы понимаем, что вам может потребоваться предыдущая версия или вы захотите поэкспериментировать с новыми усовершенствованиями в последнем предварительном выпуске.</span><span class="sxs-lookup"><span data-stu-id="72f20-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="72f20-105">Чтобы установить определенные версии EF, см. сведения в разделе о [получении Entity Framework](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="72f20-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-630"></a><span data-ttu-id="72f20-106">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="72f20-106">EF 6.3.0</span></span>

<span data-ttu-id="72f20-107">Среда выполнения EF 6.3.0 выпущена в NuGet в октябре 2019 г.</span><span class="sxs-lookup"><span data-stu-id="72f20-107">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="72f20-108">Основной целью этого выпуска было упрощение переноса существующих приложений, использующих EF 6, в .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="72f20-108">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="72f20-109">Сообществом также было внесено несколько исправлений и усовершенствований.</span><span class="sxs-lookup"><span data-stu-id="72f20-109">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="72f20-110">См. сведения о проблемах, устраненных в каждой [контрольной точке](https://github.com/aspnet/EntityFramework6/milestones?state=closed) версии 6.3.0.</span><span class="sxs-lookup"><span data-stu-id="72f20-110">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="72f20-111">Ниже перечислены некоторые основные моменты:</span><span class="sxs-lookup"><span data-stu-id="72f20-111">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="72f20-112">Включена поддержка .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="72f20-112">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="72f20-113">Теперь пакет EntityFramework можно использовать с .NET Standard 2.1 наряду с .NET Framework 4.x.</span><span class="sxs-lookup"><span data-stu-id="72f20-113">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x</span></span>
  - <span data-ttu-id="72f20-114">Команды миграции были переписаны для внепроцессного выполнения и поддержки проектов на основе пакета SDK.</span><span class="sxs-lookup"><span data-stu-id="72f20-114">The migrations commands have been rewritten to execute out of process and work with SDK-style projects</span></span>
- <span data-ttu-id="72f20-115">Включена поддержка SQL Server HierarchyId.</span><span class="sxs-lookup"><span data-stu-id="72f20-115">Support for SQL Server HierarchyId</span></span>
- <span data-ttu-id="72f20-116">Улучшена совместимость с Roslyn и NuGet PackageReference.</span><span class="sxs-lookup"><span data-stu-id="72f20-116">Improved compatibility with Roslyn and NuGet PackageReference</span></span>
- <span data-ttu-id="72f20-117">Добавлен файл Ef6.exe для включения, добавления, создания скриптов и применения миграций из сборок.</span><span class="sxs-lookup"><span data-stu-id="72f20-117">Added ef6.exe for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="72f20-118">Он заменяет файл migrate.exe.</span><span class="sxs-lookup"><span data-stu-id="72f20-118">This replaces migrate.exe</span></span>

## <a name="past-releases"></a><span data-ttu-id="72f20-119">Прошлые выпуски</span><span class="sxs-lookup"><span data-stu-id="72f20-119">Past Releases</span></span>

<span data-ttu-id="72f20-120">На странице [прошлых выпусков](past-releases.md) содержится архив всех предыдущих версий EF и основных функций, которые были представлены в каждом выпуске.</span><span class="sxs-lookup"><span data-stu-id="72f20-120">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
