---
title: Таблица журналов настраиваемых миграций — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0007da7ce3d78fd8f17007ac79a395bb2e6efd86
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414305"
---
# <a name="custom-migrations-history-table"></a>Таблица журнала настраиваемых миграций

По умолчанию EF Core отслеживает, какие миграции были применены к базе данных, записывая их в таблицу с именем `__EFMigrationsHistory`. По различным причинам может потребоваться настроить эту таблицу в соответствии со своими потребностями.

> [!IMPORTANT]
> При настройке таблицы журнала миграции *после* применения миграции вы несете ответственность за обновление существующей таблицы в базе данных.

## <a name="schema-and-table-name"></a>Имя схемы и таблицы

Изменить схему и имя таблицы можно с помощью метода `MigrationsHistoryTable()` в `OnConfiguring()` (или `ConfigureServices()` в ASP.NET Core). Ниже приведен пример использования поставщика SQL Server EF Core.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MigrationTableNameContext.cs#TableNameContext)]

## <a name="other-changes"></a>Другие изменения

Чтобы настроить дополнительные аспекты таблицы, переопределите и замените службу `IHistoryRepository`, зависящую от поставщика. Ниже приведен пример изменения имени столбца Мигратионид на *идентификатор* SQL Server.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepositoryContext)]

> [!WARNING]
> `SqlServerHistoryRepository` находится внутри внутреннего пространства имен и может измениться в будущих выпусках.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepository)]
