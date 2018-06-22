---
title: Microsoft SQL Server базы данных поставщика - оптимизированные для памяти таблицы — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 83a0decffc5946d036903b8b8add59f0ea31b21f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052644"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="3fd4b-102">Оптимизированные для памяти таблицы поддерживают поставщик EF основной базы данных для SQL Server</span><span class="sxs-lookup"><span data-stu-id="3fd4b-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

> [!NOTE]  
>
> <span data-ttu-id="3fd4b-103">Эта функция появилась в версии 1.1 EF Core.</span><span class="sxs-lookup"><span data-stu-id="3fd4b-103">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="3fd4b-104">[Оптимизированные для памяти таблиц](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) являются компонентом SQL Server, где вся таблица находится в памяти.</span><span class="sxs-lookup"><span data-stu-id="3fd4b-104">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="3fd4b-105">Вторая копия табличных данных хранится на диске, но только с целью увеличения устойчивости.</span><span class="sxs-lookup"><span data-stu-id="3fd4b-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="3fd4b-106">Данные в таблицах, оптимизированных для памяти, считываются с диска только в ходе восстановления базы данных.</span><span class="sxs-lookup"><span data-stu-id="3fd4b-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="3fd4b-107">Например, после перезапуска сервера.</span><span class="sxs-lookup"><span data-stu-id="3fd4b-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="3fd4b-108">Настройка оптимизированной для памяти таблицы</span><span class="sxs-lookup"><span data-stu-id="3fd4b-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="3fd4b-109">Можно указать таблица сопоставлена сущность — оптимизированных для памяти.</span><span class="sxs-lookup"><span data-stu-id="3fd4b-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="3fd4b-110">Если использование EF основных компонентов для создания и настройки базы данных на основе используемой модели (с помощью миграций или `Database.EnsureCreated()`), оптимизированной для памяти таблицы будут созданы для этих сущностей.</span><span class="sxs-lookup"><span data-stu-id="3fd4b-110">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
