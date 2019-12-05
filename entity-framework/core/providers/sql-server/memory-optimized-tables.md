---
title: Поставщик базы данных Microsoft SQL Server — оптимизированные для памяти таблицы — EF Core
description: Использование таблиц, оптимизированных для памяти, с поставщиком базы данных SQL Server Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: a504fb3487aea6dd36abf204a7427095e3d29118
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824650"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="0a4ed-103">Поддержка таблиц, оптимизированных для памяти, в поставщике базы данных SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="0a4ed-103">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="0a4ed-104">[Оптимизированные для памяти таблицы](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) — это функция SQL Server, где вся таблица находится в памяти.</span><span class="sxs-lookup"><span data-stu-id="0a4ed-104">[Memory-Optimized Tables](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="0a4ed-105">Вторая копия табличных данных хранится на диске, но только с целью увеличения устойчивости.</span><span class="sxs-lookup"><span data-stu-id="0a4ed-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="0a4ed-106">Данные в таблицах, оптимизированных для памяти, считываются с диска только в ходе восстановления базы данных.</span><span class="sxs-lookup"><span data-stu-id="0a4ed-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="0a4ed-107">Например, после перезапуска сервера.</span><span class="sxs-lookup"><span data-stu-id="0a4ed-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="0a4ed-108">Настройка оптимизированной для памяти таблицы</span><span class="sxs-lookup"><span data-stu-id="0a4ed-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="0a4ed-109">Вы можете указать, что сопоставленная с таблицей сущность оптимизирована для памяти.</span><span class="sxs-lookup"><span data-stu-id="0a4ed-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="0a4ed-110">При использовании EF Core для создания и обслуживания базы данных на основе модели (с [миграцией](xref:core/managing-schemas/migrations/index) или [енсурекреатед](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)) для этих сущностей будет создана таблица, оптимизированная для памяти.</span><span class="sxs-lookup"><span data-stu-id="0a4ed-110">When using EF Core to create and maintain a database based on your model (either with [migrations](xref:core/managing-schemas/migrations/index) or [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)), a memory-optimized table will be created for these entities.</span></span>

[!code-csharp[IsMemoryOptimized](../../../../samples/core/SqlServer/InMemory/InMemoryContext.cs?name=IsMemoryOptimized)]
