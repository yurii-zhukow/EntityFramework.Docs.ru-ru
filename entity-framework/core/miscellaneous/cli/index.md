---
title: "Справочник по командной строке — EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: db25ed55e3724ee71743e563f39a6e4b16c17589
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2018
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="b7de8-102">Инструменты Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="b7de8-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="b7de8-103">Инструменты Entity Framework Core помогают разрабатывать приложения EF Core.</span><span class="sxs-lookup"><span data-stu-id="b7de8-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="b7de8-104">Они используются в основном для формирования DbContext и типов сущностей посредством реконструирования схемы базы данных, а также для управления миграциями.</span><span class="sxs-lookup"><span data-stu-id="b7de8-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="b7de8-105">[Инструменты консоли диспетчера пакетов (PMC) EF Core][1] предоставляют самые широчайшие возможности внутри Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b7de8-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="b7de8-106">Чтобы запустить их, используйте [консоль диспетчера пакетов][2] NuGet.</span><span class="sxs-lookup"><span data-stu-id="b7de8-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="b7de8-107">Эти инструменты совместимы с проектами .NET Framework и .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b7de8-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="b7de8-108">[Инструменты командной строки EF Core .NET][3] представляют собой расширение для [инструментов интерфейса командной строки (CLI) .NET Core][4], которые являются кроссплатформенными и могут выполняться за пределами Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b7de8-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="b7de8-109">Этим инструментам нужен проект пакета SDK .NET Core (с `Sdk="Microsoft.NET.Sdk"` или аналогичным объектом в файле проекта).</span><span class="sxs-lookup"><span data-stu-id="b7de8-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="b7de8-110">Оба варианта инструментов предоставляют одинаковую функциональность.</span><span class="sxs-lookup"><span data-stu-id="b7de8-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="b7de8-111">При разработке в Visual Studio мы рекомендуем использовать инструменты PMC, так как они лучше интегрированы.</span><span class="sxs-lookup"><span data-stu-id="b7de8-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="b7de8-112">Инфраструктуры</span><span class="sxs-lookup"><span data-stu-id="b7de8-112">Frameworks</span></span>
----------
<span data-ttu-id="b7de8-113">Эти инструменты поддерживают проекты, предназначенные для .NET Framework или .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b7de8-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="b7de8-114">Если вам нужно использовать библиотеку классов, то по возможности используйте библиотеки классов .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b7de8-114">If you want to use a class library, then consider using a .NET Core or .NET Framework class library if possible.</span></span> <span data-ttu-id="b7de8-115">В этом случае количество проблем с инструментарием .NET. будет сведено к минимуму.</span><span class="sxs-lookup"><span data-stu-id="b7de8-115">This will result in the least issues with .NET tooling.</span></span> <span data-ttu-id="b7de8-116">Если же вы хотите использовать библиотеку классов .NET Standard, то необходимо будет использовать запускаемый проект с указанием конкретной целевой платформы (.NET Framework или .NET Core), в который можно будет загрузить вашу библиотеку классов.</span><span class="sxs-lookup"><span data-stu-id="b7de8-116">If instead you wish to use a .NET Standard class library, then you will need to use a startup project that targets .NET Framework or .NET Core so that the tooling has a conrete target platform into which it can load your class library.</span></span> <span data-ttu-id="b7de8-117">Этот запускаемый проект может быть пустым проектом, не содержащим реального кода, а используемым лишь как целевое назначение для инструментария.</span><span class="sxs-lookup"><span data-stu-id="b7de8-117">This startup project can be a dummy project with no real code--it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="b7de8-118">Если проект предназначен для другой платформы (например, для универсальной платформы Windows или Xamarin), необходимо будет создать отдельную библиотеку классов .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="b7de8-118">If your project targets another framework (for example, Universal Windows or Xamarin), then you will need to create a separate .NET Standard class library.</span></span> <span data-ttu-id="b7de8-119">В этом случае следуйте указаниям выше, чтобы создать запускаемый проект для использования с инструментарием.</span><span class="sxs-lookup"><span data-stu-id="b7de8-119">In this case, follow the guidance above to also create a startup project that can be used by the tooling.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="b7de8-120">Запускаемый и целевой проекты</span><span class="sxs-lookup"><span data-stu-id="b7de8-120">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="b7de8-121">С каждым вызовом команды связаны два проекта: целевой и запускаемый.</span><span class="sxs-lookup"><span data-stu-id="b7de8-121">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="b7de8-122">Целевой проект служит для добавления всех файлов (в некоторых случаях они удаляются из него).</span><span class="sxs-lookup"><span data-stu-id="b7de8-122">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="b7de8-123">Запускаемый проект эмулируется инструментами при выполнении кода проекта.</span><span class="sxs-lookup"><span data-stu-id="b7de8-123">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="b7de8-124">Целевой и запускаемый проекты могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="b7de8-124">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
