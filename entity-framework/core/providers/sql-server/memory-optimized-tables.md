---
title: Поставщик базы данных Microsoft SQL Server — оптимизированные для памяти таблицы — EF Core
description: Использование таблиц, оптимизированных для памяти, с поставщиком базы данных SQL Server Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: a504fb3487aea6dd36abf204a7427095e3d29118
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414767"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Поддержка таблиц, оптимизированных для памяти, в поставщике базы данных SQL Server EF Core

[Оптимизированные для памяти таблицы](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) — это функция SQL Server, где вся таблица находится в памяти. Вторая копия табличных данных хранится на диске, но только с целью увеличения устойчивости. Данные в таблицах, оптимизированных для памяти, считываются с диска только в ходе восстановления базы данных. Например, после перезапуска сервера.

## <a name="configuring-a-memory-optimized-table"></a>Настройка оптимизированной для памяти таблицы

Вы можете указать, что сопоставленная с таблицей сущность оптимизирована для памяти. При использовании EF Core для создания и обслуживания базы данных на основе модели (с [миграцией](xref:core/managing-schemas/migrations/index) или [енсурекреатед](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)) для этих сущностей будет создана таблица, оптимизированная для памяти.

[!code-csharp[IsMemoryOptimized](../../../../samples/core/SqlServer/InMemory/InMemoryContext.cs?name=IsMemoryOptimized)]
