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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Оптимизированные для памяти таблицы поддерживают в поставщик базы данных SQL Server EF Core

> [!NOTE]  
>
> Эта возможность появилась в EF Core 1.1.

[Оптимизированные для памяти таблиц](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) — это компонент SQL Server, где вся таблица находится в памяти. Второй копии данных таблицы сохраняется на диске, но только с целью увеличения устойчивости. Данные в таблицах, оптимизированных для памяти только считываются с диска во время восстановления базы данных. Например после перезапуска сервера.

## <a name="configuring-a-memory-optimized-table"></a>Настройка оптимизированной для памяти таблицы

Вы можете указать, что сопоставленная с таблицей сущность оптимизирована для памяти. Если вы используете EF Core для создания и обслуживания базы данных на основе используемой модели (с помощью миграций или `Database.EnsureCreated()`), то для таких сущностей будет создана оптимизированная для памяти таблица.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
