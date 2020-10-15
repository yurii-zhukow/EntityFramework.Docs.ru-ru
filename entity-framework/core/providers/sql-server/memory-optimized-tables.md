---
title: Поставщик базы данных Microsoft SQL Server — Memory-Optimized таблицы EF Core
description: Использование Memory-Optimized таблиц с поставщиком базы данных SQL Server Entity Framework Core
author: AndriySvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 42b2a2ffafb2234e1ce7a6d0844234509a4b5b94
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92063924"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Поддержка Memory-Optimized таблиц в поставщике базы данных SQL Server EF Core

[Оптимизированные для памяти таблицы](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) — это функция SQL Server, где вся таблица находится в памяти. Вторая копия табличных данных хранится на диске, но только с целью увеличения устойчивости. Данные в таблицах, оптимизированных для памяти, считываются с диска только в ходе восстановления базы данных. Например, после перезапуска сервера.

## <a name="configuring-a-memory-optimized-table"></a>Настройка оптимизированной для памяти таблицы

Вы можете указать, что сопоставленная с таблицей сущность оптимизирована для памяти. При использовании EF Core для создания и обслуживания базы данных на основе модели (с [миграцией](xref:core/managing-schemas/migrations/index) или [енсурекреатед](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)) для этих сущностей будет создана таблица, оптимизированная для памяти.

[!code-csharp[IsMemoryOptimized](../../../../samples/core/SqlServer/InMemory/InMemoryContext.cs?name=IsMemoryOptimized)]
