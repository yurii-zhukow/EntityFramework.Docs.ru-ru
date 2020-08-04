---
title: Справочник по инструментам Entity Framework Core — EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/19/2018
uid: core/miscellaneous/cli/index
ms.openlocfilehash: 2821263f1e9cd962569e8050b01d704cf858e6c8
ms.sourcegitcommit: 949faaba02e07e44359e77d7935f540af5c32093
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2020
ms.locfileid: "87526490"
---
# <a name="entity-framework-core-tools-reference"></a><span data-ttu-id="c02d0-102">Справочник по инструментам Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c02d0-102">Entity Framework Core tools reference</span></span>

<span data-ttu-id="c02d0-103">Инструменты Entity Framework Core используются для задач разработки.</span><span class="sxs-lookup"><span data-stu-id="c02d0-103">The Entity Framework Core tools help with design-time development tasks.</span></span> <span data-ttu-id="c02d0-104">Эти инструменты используются в основном для управления миграциями и формирования `DbContext` и типов сущностей путем реконструирования схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="c02d0-104">They're primarily used to manage Migrations and to scaffold a `DbContext` and entity types by reverse engineering the schema of a database.</span></span>

* <span data-ttu-id="c02d0-105">[Инструменты консоли диспетчера пакетов EF Core](powershell.md) запускаются в [консоли диспетчера пакетов](/nuget/tools/package-manager-console) в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c02d0-105">The [EF Core Package Manager Console tools](powershell.md) run in the [Package Manager Console](/nuget/tools/package-manager-console) in Visual Studio.</span></span>

* <span data-ttu-id="c02d0-106">[Инструменты интерфейса командной строки EF Core .NET](dotnet.md) представляют собой расширение для кроссплатформенных [инструментов .NET Core CLI](/dotnet/core/tools/).</span><span class="sxs-lookup"><span data-stu-id="c02d0-106">The [EF Core .NET command-line interface (CLI) tools](dotnet.md) are an extension to the cross-platform [.NET Core CLI tools](/dotnet/core/tools/).</span></span> <span data-ttu-id="c02d0-107">Этим инструментам нужен проект пакета SDK .NET Core (с `Sdk="Microsoft.NET.Sdk"` или аналогичным объектом в файле проекта).</span><span class="sxs-lookup"><span data-stu-id="c02d0-107">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="c02d0-108">Оба варианта инструментов предоставляют одинаковую функциональность.</span><span class="sxs-lookup"><span data-stu-id="c02d0-108">Both tools expose the same functionality.</span></span> <span data-ttu-id="c02d0-109">При разработке в Visual Studio мы рекомендуем использовать инструменты **консоли диспетчера пакетов**, так как они лучше интегрированы.</span><span class="sxs-lookup"><span data-stu-id="c02d0-109">If you're developing in Visual Studio, we recommend using the **Package Manager Console** tools since they provide a more integrated experience.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c02d0-110">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="c02d0-110">Next steps</span></span>

* [<span data-ttu-id="c02d0-111">Справочник по инструментам консоли диспетчера пакетов EF Core</span><span class="sxs-lookup"><span data-stu-id="c02d0-111">EF Core Package Manager Console tools reference</span></span>](powershell.md)
* [<span data-ttu-id="c02d0-112">Справочник по инструментам EF Core — .NET CLI</span><span class="sxs-lookup"><span data-stu-id="c02d0-112">EF Core .NET CLI tools reference</span></span>](dotnet.md)
