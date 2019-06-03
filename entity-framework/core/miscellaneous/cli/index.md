---
title: Справочник по инструментам Entity Framework Core — EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/19/2018
uid: core/miscellaneous/cli/index
ms.openlocfilehash: 13e80f740bc5ce3404e8dba40b65ec872c5e3f90
ms.sourcegitcommit: ea1cdec0b982b922a59b9d9301d3ed2b94baca0f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452256"
---
# <a name="entity-framework-core-tools-reference"></a><span data-ttu-id="27b19-102">Справочник по инструментам Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="27b19-102">Entity Framework Core tools reference</span></span>

<span data-ttu-id="27b19-103">Инструменты Entity Framework Core используются для задач разработки.</span><span class="sxs-lookup"><span data-stu-id="27b19-103">The Entity Framework Core tools help with design-time development tasks.</span></span> <span data-ttu-id="27b19-104">Эти инструменты используются в основном для управления миграциями и формирования `DbContext` и типов сущностей путем реконструирования схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="27b19-104">They're primarily used to manage Migrations and to scaffold a `DbContext` and entity types by reverse engineering the schema of a database.</span></span>

* <span data-ttu-id="27b19-105">[Инструменты консоли диспетчера пакетов EF Core](powershell.md) запускаются в [консоли диспетчера пакетов](https://docs.microsoft.com/nuget/tools/package-manager-console) в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="27b19-105">The [EF Core Package Manager Console tools](powershell.md) run in the [Package Manager Console](https://docs.microsoft.com/nuget/tools/package-manager-console) in Visual Studio.</span></span> <span data-ttu-id="27b19-106">Эти инструменты совместимы с проектами .NET Framework и .NET Core.</span><span class="sxs-lookup"><span data-stu-id="27b19-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

* <span data-ttu-id="27b19-107">[Инструменты интерфейса командной строки EF Core .NET](dotnet.md) представляют собой расширение для кроссплатформенных [инструментов .NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/).</span><span class="sxs-lookup"><span data-stu-id="27b19-107">The [EF Core .NET command-line interface (CLI) tools](dotnet.md) are an extension to the cross-platform [.NET Core CLI tools](https://docs.microsoft.com/dotnet/core/tools/).</span></span> <span data-ttu-id="27b19-108">Этим инструментам нужен проект пакета SDK .NET Core (с `Sdk="Microsoft.NET.Sdk"` или аналогичным объектом в файле проекта).</span><span class="sxs-lookup"><span data-stu-id="27b19-108">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="27b19-109">Оба варианта инструментов предоставляют одинаковую функциональность.</span><span class="sxs-lookup"><span data-stu-id="27b19-109">Both tools expose the same functionality.</span></span> <span data-ttu-id="27b19-110">При разработке в Visual Studio мы рекомендуем использовать инструменты **консоли диспетчера пакетов**, так как они лучше интегрированы.</span><span class="sxs-lookup"><span data-stu-id="27b19-110">If you're developing in Visual Studio, we recommend using the **Package Manager Console** tools since they provide a more integrated experience.</span></span>

## <a name="next-steps"></a><span data-ttu-id="27b19-111">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="27b19-111">Next steps</span></span>

* [<span data-ttu-id="27b19-112">Справочник по инструментам консоли диспетчера пакетов EF Core</span><span class="sxs-lookup"><span data-stu-id="27b19-112">EF Core Package Manager Console tools reference</span></span>](powershell.md)
* [<span data-ttu-id="27b19-113">Справочник по инструментам EF Core — .NET CLI</span><span class="sxs-lookup"><span data-stu-id="27b19-113">EF Core .NET CLI tools reference</span></span>](dotnet.md)
