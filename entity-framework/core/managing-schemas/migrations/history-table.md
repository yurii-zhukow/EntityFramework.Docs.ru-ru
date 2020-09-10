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
# <a name="custom-migrations-history-table"></a><span data-ttu-id="72b2f-103">Таблица журнала настраиваемых миграций</span><span class="sxs-lookup"><span data-stu-id="72b2f-103">Custom Migrations History Table</span></span>

<span data-ttu-id="72b2f-104">По умолчанию EF Core отслеживает, какие миграции были применены к базе данных, записывая их в таблицу с именем `__EFMigrationsHistory` .</span><span class="sxs-lookup"><span data-stu-id="72b2f-104">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="72b2f-105">По различным причинам может потребоваться настроить эту таблицу в соответствии со своими потребностями.</span><span class="sxs-lookup"><span data-stu-id="72b2f-105">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="72b2f-106">При настройке таблицы журнала миграции *после* применения миграции вы несете ответственность за обновление существующей таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="72b2f-106">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

## <a name="schema-and-table-name"></a><span data-ttu-id="72b2f-107">Имя схемы и таблицы</span><span class="sxs-lookup"><span data-stu-id="72b2f-107">Schema and table name</span></span>

<span data-ttu-id="72b2f-108">Изменить схему и имя таблицы можно с помощью `MigrationsHistoryTable()` метода в `OnConfiguring()` (или `ConfigureServices()` ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="72b2f-108">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="72b2f-109">Ниже приведен пример использования поставщика SQL Server EF Core.</span><span class="sxs-lookup"><span data-stu-id="72b2f-109">Here is an example using the SQL Server EF Core provider.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MigrationTableNameContext.cs#TableNameContext)]

## <a name="other-changes"></a><span data-ttu-id="72b2f-110">Другие изменения</span><span class="sxs-lookup"><span data-stu-id="72b2f-110">Other changes</span></span>

<span data-ttu-id="72b2f-111">Чтобы настроить дополнительные аспекты таблицы, переопределите и замените службу, зависящую от поставщика `IHistoryRepository` .</span><span class="sxs-lookup"><span data-stu-id="72b2f-111">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="72b2f-112">Ниже приведен пример изменения имени столбца Мигратионид на *идентификатор* SQL Server.</span><span class="sxs-lookup"><span data-stu-id="72b2f-112">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepositoryContext)]

> [!WARNING]
> <span data-ttu-id="72b2f-113">`SqlServerHistoryRepository` находится внутри внутреннего пространства имен и может измениться в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="72b2f-113">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepository)]
