---
title: Таблица журналов настраиваемых миграций — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0007da7ce3d78fd8f17007ac79a395bb2e6efd86
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655707"
---
# <a name="custom-migrations-history-table"></a><span data-ttu-id="1d9eb-102">Таблица журнала настраиваемых миграций</span><span class="sxs-lookup"><span data-stu-id="1d9eb-102">Custom Migrations History Table</span></span>

<span data-ttu-id="1d9eb-103">По умолчанию EF Core отслеживает, какие миграции были применены к базе данных, записывая их в таблицу с именем `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="1d9eb-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="1d9eb-104">По различным причинам может потребоваться настроить эту таблицу в соответствии со своими потребностями.</span><span class="sxs-lookup"><span data-stu-id="1d9eb-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1d9eb-105">При настройке таблицы журнала миграции *после* применения миграции вы несете ответственность за обновление существующей таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1d9eb-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

## <a name="schema-and-table-name"></a><span data-ttu-id="1d9eb-106">Имя схемы и таблицы</span><span class="sxs-lookup"><span data-stu-id="1d9eb-106">Schema and table name</span></span>

<span data-ttu-id="1d9eb-107">Изменить схему и имя таблицы можно с помощью метода `MigrationsHistoryTable()` в `OnConfiguring()` (или `ConfigureServices()` в ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="1d9eb-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="1d9eb-108">Ниже приведен пример использования поставщика SQL Server EF Core.</span><span class="sxs-lookup"><span data-stu-id="1d9eb-108">Here is an example using the SQL Server EF Core provider.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MigrationTableNameContext.cs#TableNameContext)]

## <a name="other-changes"></a><span data-ttu-id="1d9eb-109">Прочие изменения</span><span class="sxs-lookup"><span data-stu-id="1d9eb-109">Other changes</span></span>

<span data-ttu-id="1d9eb-110">Чтобы настроить дополнительные аспекты таблицы, переопределите и замените службу `IHistoryRepository`, зависящую от поставщика.</span><span class="sxs-lookup"><span data-stu-id="1d9eb-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="1d9eb-111">Ниже приведен пример изменения имени столбца Мигратионид на *идентификатор* SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1d9eb-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepositoryContext)]

> [!WARNING]
> <span data-ttu-id="1d9eb-112">`SqlServerHistoryRepository` находится внутри внутреннего пространства имен и может измениться в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="1d9eb-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepository)]
