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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Оптимизированные для памяти таблицы поддерживают поставщик EF основной базы данных для SQL Server

> [!NOTE]  
>
> Эта функция появилась в версии 1.1 EF Core.

[Оптимизированные для памяти таблиц](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) являются компонентом SQL Server, где вся таблица находится в памяти. Вторая копия табличных данных хранится на диске, но только с целью увеличения устойчивости. Данные в таблицах, оптимизированных для памяти, считываются с диска только в ходе восстановления базы данных. Например, после перезапуска сервера.

## <a name="configuring-a-memory-optimized-table"></a>Настройка оптимизированной для памяти таблицы

Можно указать таблица сопоставлена сущность — оптимизированных для памяти. Если использование EF основных компонентов для создания и настройки базы данных на основе используемой модели (с помощью миграций или `Database.EnsureCreated()`), оптимизированной для памяти таблицы будут созданы для этих сущностей.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
