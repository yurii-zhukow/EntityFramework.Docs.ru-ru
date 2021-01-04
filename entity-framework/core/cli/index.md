---
title: Справочник по инструментам Entity Framework Core — EF Core
description: Справочное руководство по средству CLI Entity Framework Core и консоли диспетчера пакетов Visual Studio
author: bricelam
ms.date: 09/19/2018
uid: core/cli/index
ms.openlocfilehash: 1ffc773cb8ed30516d682b90bbd9accef634ae6a
ms.sourcegitcommit: 4860d036ea0fb392c28799907bcc924c987d2d7b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97635385"
---
# <a name="entity-framework-core-tools-reference"></a><span data-ttu-id="3d3d2-103">Справочник по инструментам Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="3d3d2-103">Entity Framework Core tools reference</span></span>

<span data-ttu-id="3d3d2-104">Инструменты Entity Framework Core используются для задач разработки.</span><span class="sxs-lookup"><span data-stu-id="3d3d2-104">The Entity Framework Core tools help with design-time development tasks.</span></span> <span data-ttu-id="3d3d2-105">Эти инструменты используются в основном для управления миграциями и формирования `DbContext` и типов сущностей путем реконструирования схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="3d3d2-105">They're primarily used to manage Migrations and to scaffold a `DbContext` and entity types by reverse engineering the schema of a database.</span></span>

<span data-ttu-id="3d3d2-106">Можно установить любое из следующих средств, так как оба средства предоставляют одинаковую функциональность:</span><span class="sxs-lookup"><span data-stu-id="3d3d2-106">Either of the following tools can be installed, as both tools expose the same functionality:</span></span>

* <span data-ttu-id="3d3d2-107">[Инструменты консоли диспетчера пакетов EF Core](xref:core/cli/powershell) запускаются в [консоли диспетчера пакетов](/nuget/tools/package-manager-console) в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3d3d2-107">The [EF Core Package Manager Console tools](xref:core/cli/powershell) run in the [Package Manager Console](/nuget/tools/package-manager-console) in Visual Studio.</span></span> <span data-ttu-id="3d3d2-108">Мы рекомендуем использовать эти средства при разработке в Visual Studio, так как они лучше интегрированы.</span><span class="sxs-lookup"><span data-stu-id="3d3d2-108">We recommend using these tools if you are developing in Visual Studio as they provide a more integrated experience.</span></span>

* <span data-ttu-id="3d3d2-109">[Инструменты интерфейса командной строки EF Core .NET](xref:core/cli/dotnet) представляют собой расширение для кроссплатформенных [инструментов .NET Core CLI](/dotnet/core/tools/).</span><span class="sxs-lookup"><span data-stu-id="3d3d2-109">The [EF Core .NET command-line interface (CLI) tools](xref:core/cli/dotnet) are an extension to the cross-platform [.NET Core CLI tools](/dotnet/core/tools/).</span></span> <span data-ttu-id="3d3d2-110">Этим инструментам нужен проект пакета SDK .NET Core (с `Sdk="Microsoft.NET.Sdk"` или аналогичным объектом в файле проекта).</span><span class="sxs-lookup"><span data-stu-id="3d3d2-110">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3d3d2-111">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="3d3d2-111">Next steps</span></span>

* [<span data-ttu-id="3d3d2-112">Справочник по инструментам консоли диспетчера пакетов EF Core</span><span class="sxs-lookup"><span data-stu-id="3d3d2-112">EF Core Package Manager Console tools reference</span></span>](xref:core/cli/powershell)
* [<span data-ttu-id="3d3d2-113">Справочник по инструментам EF Core — .NET CLI</span><span class="sxs-lookup"><span data-stu-id="3d3d2-113">EF Core .NET CLI tools reference</span></span>](xref:core/cli/dotnet)
