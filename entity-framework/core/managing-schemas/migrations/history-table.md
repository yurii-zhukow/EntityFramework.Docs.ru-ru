---
title: Таблица журналов настраиваемых миграций — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0db393ff3101564f8d8081d0a57b264c2c459df7
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812068"
---
# <a name="custom-migrations-history-table"></a><span data-ttu-id="34063-102">Таблица журнала настраиваемых миграций</span><span class="sxs-lookup"><span data-stu-id="34063-102">Custom Migrations History Table</span></span>

<span data-ttu-id="34063-103">По умолчанию EF Core отслеживает, какие миграции были применены к базе данных, записывая их в таблицу с именем `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="34063-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="34063-104">По различным причинам может потребоваться настроить эту таблицу в соответствии со своими потребностями.</span><span class="sxs-lookup"><span data-stu-id="34063-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="34063-105">При настройке таблицы журнала миграции *после* применения миграции вы несете ответственность за обновление существующей таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="34063-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

## <a name="schema-and-table-name"></a><span data-ttu-id="34063-106">Имя схемы и таблицы</span><span class="sxs-lookup"><span data-stu-id="34063-106">Schema and table name</span></span>

<span data-ttu-id="34063-107">Изменить схему и имя таблицы можно с помощью метода `MigrationsHistoryTable()` в `OnConfiguring()` (или `ConfigureServices()` в ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="34063-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="34063-108">Ниже приведен пример использования поставщика SQL Server EF Core.</span><span class="sxs-lookup"><span data-stu-id="34063-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

## <a name="other-changes"></a><span data-ttu-id="34063-109">Прочие изменения</span><span class="sxs-lookup"><span data-stu-id="34063-109">Other changes</span></span>

<span data-ttu-id="34063-110">Чтобы настроить дополнительные аспекты таблицы, переопределите и замените службу `IHistoryRepository`, зависящую от поставщика.</span><span class="sxs-lookup"><span data-stu-id="34063-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="34063-111">Ниже приведен пример изменения имени столбца Мигратионид на *идентификатор* SQL Server.</span><span class="sxs-lookup"><span data-stu-id="34063-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="34063-112">`SqlServerHistoryRepository` находится внутри внутреннего пространства имен и может измениться в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="34063-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

``` csharp
class MyHistoryRepository : SqlServerHistoryRepository
{
    public MyHistoryRepository(HistoryRepositoryDependencies dependencies)
        : base(dependencies)
    {
    }

    protected override void ConfigureTable(EntityTypeBuilder<HistoryRow> history)
    {
        base.ConfigureTable(history);

        history.Property(h => h.MigrationId).HasColumnName("Id");
    }
}
```
