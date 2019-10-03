---
title: Поставщик базы данных Microsoft SQL Server — оптимизированные для памяти таблицы — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 7383e74b3f83172f9b8e0eaf9bd09d4e187e87f8
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813498"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="c0321-102">Поддержка таблиц, оптимизированных для памяти, в поставщике базы данных SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="c0321-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="c0321-103">[Оптимизированные для памяти таблицы](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) — это функция SQL Server, где вся таблица находится в памяти.</span><span class="sxs-lookup"><span data-stu-id="c0321-103">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="c0321-104">Вторая копия табличных данных сохраняется на диске, но только в целях устойчивости.</span><span class="sxs-lookup"><span data-stu-id="c0321-104">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="c0321-105">Данные в таблицах, оптимизированных для памяти, считываются с диска только во время восстановления базы данных.</span><span class="sxs-lookup"><span data-stu-id="c0321-105">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="c0321-106">Например, после перезапуска сервера.</span><span class="sxs-lookup"><span data-stu-id="c0321-106">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="c0321-107">Настройка оптимизированной для памяти таблицы</span><span class="sxs-lookup"><span data-stu-id="c0321-107">Configuring a memory-optimized table</span></span>

<span data-ttu-id="c0321-108">Вы можете указать, что сопоставленная с таблицей сущность оптимизирована для памяти.</span><span class="sxs-lookup"><span data-stu-id="c0321-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="c0321-109">Если вы используете EF Core для создания и обслуживания базы данных на основе используемой модели (с помощью миграций или `Database.EnsureCreated()`), то для таких сущностей будет создана оптимизированная для памяти таблица.</span><span class="sxs-lookup"><span data-stu-id="c0321-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
