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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Поддержка таблиц, оптимизированных для памяти, в поставщике базы данных SQL Server EF Core

[Оптимизированные для памяти таблицы](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) — это функция SQL Server, где вся таблица находится в памяти. Вторая копия табличных данных сохраняется на диске, но только в целях устойчивости. Данные в таблицах, оптимизированных для памяти, считываются с диска только во время восстановления базы данных. Например, после перезапуска сервера.

## <a name="configuring-a-memory-optimized-table"></a>Настройка оптимизированной для памяти таблицы

Вы можете указать, что сопоставленная с таблицей сущность оптимизирована для памяти. Если вы используете EF Core для создания и обслуживания базы данных на основе используемой модели (с помощью миграций или `Database.EnsureCreated()`), то для таких сущностей будет создана оптимизированная для памяти таблица.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
