---
title: Справочник по инструментам Entity Framework Core — EF Core
description: Справочное руководство по средству CLI Entity Framework Core и консоли диспетчера пакетов Visual Studio
author: bricelam
ms.author: bricelam
ms.date: 09/19/2018
uid: core/miscellaneous/cli/index
ms.openlocfilehash: 7d5fb984aafed2cf45efa9e5b83bf4bc6c18a44a
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89619420"
---
# <a name="entity-framework-core-tools-reference"></a><span data-ttu-id="c7d07-103">Справочник по инструментам Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c7d07-103">Entity Framework Core tools reference</span></span>

<span data-ttu-id="c7d07-104">Инструменты Entity Framework Core используются для задач разработки.</span><span class="sxs-lookup"><span data-stu-id="c7d07-104">The Entity Framework Core tools help with design-time development tasks.</span></span> <span data-ttu-id="c7d07-105">Эти инструменты используются в основном для управления миграциями и формирования `DbContext` и типов сущностей путем реконструирования схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="c7d07-105">They're primarily used to manage Migrations and to scaffold a `DbContext` and entity types by reverse engineering the schema of a database.</span></span>

* <span data-ttu-id="c7d07-106">[Инструменты консоли диспетчера пакетов EF Core](xref:core/miscellaneous/cli/powershell) запускаются в [консоли диспетчера пакетов](/nuget/tools/package-manager-console) в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c7d07-106">The [EF Core Package Manager Console tools](xref:core/miscellaneous/cli/powershell) run in the [Package Manager Console](/nuget/tools/package-manager-console) in Visual Studio.</span></span>

* <span data-ttu-id="c7d07-107">[Инструменты интерфейса командной строки EF Core .NET](xref:core/miscellaneous/cli/dotnet) представляют собой расширение для кроссплатформенных [инструментов .NET Core CLI](/dotnet/core/tools/).</span><span class="sxs-lookup"><span data-stu-id="c7d07-107">The [EF Core .NET command-line interface (CLI) tools](xref:core/miscellaneous/cli/dotnet) are an extension to the cross-platform [.NET Core CLI tools](/dotnet/core/tools/).</span></span> <span data-ttu-id="c7d07-108">Этим инструментам нужен проект пакета SDK .NET Core (с `Sdk="Microsoft.NET.Sdk"` или аналогичным объектом в файле проекта).</span><span class="sxs-lookup"><span data-stu-id="c7d07-108">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="c7d07-109">Оба варианта инструментов предоставляют одинаковую функциональность.</span><span class="sxs-lookup"><span data-stu-id="c7d07-109">Both tools expose the same functionality.</span></span> <span data-ttu-id="c7d07-110">При разработке в Visual Studio мы рекомендуем использовать инструменты **консоли диспетчера пакетов**, так как они лучше интегрированы.</span><span class="sxs-lookup"><span data-stu-id="c7d07-110">If you're developing in Visual Studio, we recommend using the **Package Manager Console** tools since they provide a more integrated experience.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c7d07-111">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="c7d07-111">Next steps</span></span>

* [<span data-ttu-id="c7d07-112">Справочник по инструментам консоли диспетчера пакетов EF Core</span><span class="sxs-lookup"><span data-stu-id="c7d07-112">EF Core Package Manager Console tools reference</span></span>](xref:core/miscellaneous/cli/powershell)
* [<span data-ttu-id="c7d07-113">Справочник по инструментам EF Core — .NET CLI</span><span class="sxs-lookup"><span data-stu-id="c7d07-113">EF Core .NET CLI tools reference</span></span>](xref:core/miscellaneous/cli/dotnet)
