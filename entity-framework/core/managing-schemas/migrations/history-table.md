---
title: "Таблицы журнала миграций пользовательских - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: cb9892241f3d7f1fae6293bd60a8a5c3e7120969
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
<a name="custom-migrations-history-table"></a><span data-ttu-id="06bdf-102">Таблицы журнала миграций, пользовательские</span><span class="sxs-lookup"><span data-stu-id="06bdf-102">Custom Migrations History Table</span></span>
===============================
<span data-ttu-id="06bdf-103">По умолчанию EF ядра отслеживает отслеживания объекта какие миграции были применены к базе данных путем их записи в таблице с именем `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="06bdf-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="06bdf-104">По различным причинам может потребоваться настроить эту таблицу для удовлетворения ваших потребностей.</span><span class="sxs-lookup"><span data-stu-id="06bdf-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="06bdf-105">При настройке таблицы журнала миграций *после* применение миграции, вы несете ответственность за обновление существующей таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="06bdf-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

<a name="schema-and-table-name"></a><span data-ttu-id="06bdf-106">Имя схемы и таблицы</span><span class="sxs-lookup"><span data-stu-id="06bdf-106">Schema and table name</span></span>
----------------------
<span data-ttu-id="06bdf-107">Можно изменить схему и имя таблицы с помощью `MigrationsHistoryTable()` метод в `OnConfiguring()` (или `ConfigureServices()` в ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="06bdf-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="06bdf-108">Ниже приведен пример с помощью поставщика SQL Server EF Core.</span><span class="sxs-lookup"><span data-stu-id="06bdf-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a><span data-ttu-id="06bdf-109">Прочие изменения</span><span class="sxs-lookup"><span data-stu-id="06bdf-109">Other changes</span></span>
-------------
<span data-ttu-id="06bdf-110">Чтобы настроить дополнительные аспекты таблицы, переопределить и заменить поставщика `IHistoryRepository` службы.</span><span class="sxs-lookup"><span data-stu-id="06bdf-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="06bdf-111">Ниже приведен пример изменить имя столбца MigrationId *идентификатор* на сервере SQL Server.</span><span class="sxs-lookup"><span data-stu-id="06bdf-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="06bdf-112">`SqlServerHistoryRepository`внутри внутреннего пространства имен и может измениться в будущих версиях.</span><span class="sxs-lookup"><span data-stu-id="06bdf-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

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
