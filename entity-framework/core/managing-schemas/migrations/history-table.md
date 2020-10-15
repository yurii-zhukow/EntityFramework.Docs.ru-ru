---
title: Таблица журналов настраиваемых миграций — EF Core
description: Настройка таблицы журнала для миграции с помощью Entity Framework Core
author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 1f6dcb2fbb20e7dafed17160832ccec453839aa3
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92062312"
---
# <a name="custom-migrations-history-table"></a>Таблица журнала настраиваемых миграций

По умолчанию EF Core отслеживает, какие миграции были применены к базе данных, записывая их в таблицу с именем `__EFMigrationsHistory` . По различным причинам может потребоваться настроить эту таблицу в соответствии со своими потребностями.

> [!IMPORTANT]
> При настройке таблицы журнала миграции *после* применения миграции вы несете ответственность за обновление существующей таблицы в базе данных.

## <a name="schema-and-table-name"></a>Имя схемы и таблицы

Изменить схему и имя таблицы можно с помощью `MigrationsHistoryTable()` метода в `OnConfiguring()` (или `ConfigureServices()` ASP.NET Core). Ниже приведен пример использования поставщика SQL Server EF Core.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MigrationTableNameContext.cs#TableNameContext)]

## <a name="other-changes"></a>Другие изменения

Чтобы настроить дополнительные аспекты таблицы, переопределите и замените службу, зависящую от поставщика `IHistoryRepository` . Ниже приведен пример изменения имени столбца Мигратионид на *идентификатор* SQL Server.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepositoryContext)]

> [!WARNING]
> `SqlServerHistoryRepository` находится внутри внутреннего пространства имен и может измениться в будущих выпусках.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepository)]
