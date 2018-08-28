---
title: Microsoft SQL Server базы данных поставщика — оптимизированных для памяти таблицы — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 63d2cbf8b69e4f1945ad60914e284fb42c48e8db
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995806"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="67c85-102">Оптимизированные для памяти таблицы поддерживают в поставщик базы данных SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="67c85-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

> [!NOTE]  
>
> <span data-ttu-id="67c85-103">Эта возможность появилась в EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="67c85-103">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="67c85-104">[Оптимизированные для памяти таблиц](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) — это компонент SQL Server, где вся таблица находится в памяти.</span><span class="sxs-lookup"><span data-stu-id="67c85-104">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="67c85-105">Второй копии данных таблицы сохраняется на диске, но только с целью увеличения устойчивости.</span><span class="sxs-lookup"><span data-stu-id="67c85-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="67c85-106">Данные в таблицах, оптимизированных для памяти только считываются с диска во время восстановления базы данных.</span><span class="sxs-lookup"><span data-stu-id="67c85-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="67c85-107">Например после перезапуска сервера.</span><span class="sxs-lookup"><span data-stu-id="67c85-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="67c85-108">Настройка оптимизированной для памяти таблицы</span><span class="sxs-lookup"><span data-stu-id="67c85-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="67c85-109">Вы можете указать, что сопоставленная с таблицей сущность оптимизирована для памяти.</span><span class="sxs-lookup"><span data-stu-id="67c85-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="67c85-110">Если вы используете EF Core для создания и обслуживания базы данных на основе используемой модели (с помощью миграций или `Database.EnsureCreated()`), то для таких сущностей будет создана оптимизированная для памяти таблица.</span><span class="sxs-lookup"><span data-stu-id="67c85-110">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
