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
# <a name="custom-migrations-history-table"></a><span data-ttu-id="323c2-103">Таблица журнала настраиваемых миграций</span><span class="sxs-lookup"><span data-stu-id="323c2-103">Custom Migrations History Table</span></span>

<span data-ttu-id="323c2-104">По умолчанию EF Core отслеживает, какие миграции были применены к базе данных, записывая их в таблицу с именем `__EFMigrationsHistory` .</span><span class="sxs-lookup"><span data-stu-id="323c2-104">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="323c2-105">По различным причинам может потребоваться настроить эту таблицу в соответствии со своими потребностями.</span><span class="sxs-lookup"><span data-stu-id="323c2-105">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="323c2-106">При настройке таблицы журнала миграции *после* применения миграции вы несете ответственность за обновление существующей таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="323c2-106">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

## <a name="schema-and-table-name"></a><span data-ttu-id="323c2-107">Имя схемы и таблицы</span><span class="sxs-lookup"><span data-stu-id="323c2-107">Schema and table name</span></span>

<span data-ttu-id="323c2-108">Изменить схему и имя таблицы можно с помощью `MigrationsHistoryTable()` метода в `OnConfiguring()` (или `ConfigureServices()` ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="323c2-108">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="323c2-109">Ниже приведен пример использования поставщика SQL Server EF Core.</span><span class="sxs-lookup"><span data-stu-id="323c2-109">Here is an example using the SQL Server EF Core provider.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MigrationTableNameContext.cs#TableNameContext)]

## <a name="other-changes"></a><span data-ttu-id="323c2-110">Другие изменения</span><span class="sxs-lookup"><span data-stu-id="323c2-110">Other changes</span></span>

<span data-ttu-id="323c2-111">Чтобы настроить дополнительные аспекты таблицы, переопределите и замените службу, зависящую от поставщика `IHistoryRepository` .</span><span class="sxs-lookup"><span data-stu-id="323c2-111">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="323c2-112">Ниже приведен пример изменения имени столбца Мигратионид на *идентификатор* SQL Server.</span><span class="sxs-lookup"><span data-stu-id="323c2-112">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepositoryContext)]

> [!WARNING]
> <span data-ttu-id="323c2-113">`SqlServerHistoryRepository` находится внутри внутреннего пространства имен и может измениться в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="323c2-113">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepository)]
