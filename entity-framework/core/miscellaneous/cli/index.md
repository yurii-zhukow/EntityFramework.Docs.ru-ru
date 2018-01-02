---
title: "Справочник по командной строке — EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 076e9251850ba10df323cd25922aa8b95b3a5491
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="7a7c0-102">Инструменты Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="7a7c0-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="7a7c0-103">Инструменты Entity Framework Core помогают разрабатывать приложения EF Core.</span><span class="sxs-lookup"><span data-stu-id="7a7c0-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="7a7c0-104">Они используются в основном для формирования DbContext и типов сущностей посредством реконструирования схемы базы данных, а также для управления миграциями.</span><span class="sxs-lookup"><span data-stu-id="7a7c0-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="7a7c0-105">[Инструменты консоли диспетчера пакетов (PMC) EF Core][1] предоставляют самые широчайшие возможности внутри Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7a7c0-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="7a7c0-106">Чтобы запустить их, используйте [консоль диспетчера пакетов][2] NuGet.</span><span class="sxs-lookup"><span data-stu-id="7a7c0-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="7a7c0-107">Эти инструменты совместимы с проектами .NET Framework и .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7a7c0-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="7a7c0-108">[Инструменты командной строки EF Core .NET][3] представляют собой расширение для [инструментов интерфейса командной строки (CLI) .NET Core][4], которые являются кроссплатформенными и могут выполняться за пределами Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7a7c0-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="7a7c0-109">Этим инструментам нужен проект пакета SDK .NET Core (с `Sdk="Microsoft.NET.Sdk"` или аналогичным объектом в файле проекта).</span><span class="sxs-lookup"><span data-stu-id="7a7c0-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="7a7c0-110">Оба варианта инструментов предоставляют одинаковую функциональность.</span><span class="sxs-lookup"><span data-stu-id="7a7c0-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="7a7c0-111">При разработке в Visual Studio мы рекомендуем использовать инструменты PMC, так как они лучше интегрированы.</span><span class="sxs-lookup"><span data-stu-id="7a7c0-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="7a7c0-112">Инфраструктуры</span><span class="sxs-lookup"><span data-stu-id="7a7c0-112">Frameworks</span></span>
----------
<span data-ttu-id="7a7c0-113">Эти инструменты поддерживают проекты, предназначенные для .NET Framework или .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7a7c0-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="7a7c0-114">Если ваш проект предназначен для другой платформы (например, универсальной платформы Windows или Xamarin), рекомендуется создать отдельный проект .NET Standard и переориентироваться на одну из поддерживаемых платформ.</span><span class="sxs-lookup"><span data-stu-id="7a7c0-114">If your project targets another framework (for example, Universal Windows or Xamarin), we recommend creating a separate .NET Standard project and cross-targeting one of the supported frameworks.</span></span>

<span data-ttu-id="7a7c0-115">Например, чтобы переориентироваться на .NET Core, щелкните правой кнопкой мыши проект и выберите пункт **Изменить \*.csproj**.</span><span class="sxs-lookup"><span data-stu-id="7a7c0-115">To cross-target .NET Core, for example, right-click on the project and select **Edit \*.csproj**.</span></span> <span data-ttu-id="7a7c0-116">Обновите свойство `TargetFramework`, как указано ниже.</span><span class="sxs-lookup"><span data-stu-id="7a7c0-116">Update the `TargetFramework` property as follows.</span></span> <span data-ttu-id="7a7c0-117">(Обратите внимание, что имя свойства становится множественного числа.)</span><span class="sxs-lookup"><span data-stu-id="7a7c0-117">(Note, the property name becomes plural.)</span></span>

``` xml
<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>
```

<span data-ttu-id="7a7c0-118">Если вы используете библиотеку классов .NET Standard, переориентироваться не требуется, если ваш запускаемый проект предназначен для платформы .NET Framework или .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7a7c0-118">If you're using a .NET Standard class library, you don't need to cross-target if your startup project targets .NET Framework or .NET Core.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="7a7c0-119">Запускаемый и целевой проекты</span><span class="sxs-lookup"><span data-stu-id="7a7c0-119">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="7a7c0-120">С каждым вызовом команды связаны два проекта: целевой и запускаемый.</span><span class="sxs-lookup"><span data-stu-id="7a7c0-120">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="7a7c0-121">Целевой проект служит для добавления всех файлов (в некоторых случаях они удаляются из него).</span><span class="sxs-lookup"><span data-stu-id="7a7c0-121">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="7a7c0-122">Запускаемый проект эмулируется инструментами при выполнении кода проекта.</span><span class="sxs-lookup"><span data-stu-id="7a7c0-122">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="7a7c0-123">Целевой и запускаемый проекты могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="7a7c0-123">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
