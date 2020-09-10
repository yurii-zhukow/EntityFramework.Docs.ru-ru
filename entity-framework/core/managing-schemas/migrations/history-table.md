---
title: Таблица журналов настраиваемых миграций — EF Core
description: Настройка таблицы журнала для миграции с помощью Entity Framework Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: deed86b2023557036eb14ca5a66f04ab5bb4139f
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89617975"
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
