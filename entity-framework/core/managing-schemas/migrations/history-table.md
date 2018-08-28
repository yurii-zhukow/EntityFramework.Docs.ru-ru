---
title: Таблица журнала пользовательских миграции — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.openlocfilehash: 7ee76cadd6fac4ec403918e88460e43067ae5815
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995700"
---
<a name="custom-migrations-history-table"></a><span data-ttu-id="529ca-102">Таблица миграции пользовательского журнала</span><span class="sxs-lookup"><span data-stu-id="529ca-102">Custom Migrations History Table</span></span>
===============================
<span data-ttu-id="529ca-103">По умолчанию EF Core следит за миграций, которые были применены к базе данных, записав их в таблицу с именем `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="529ca-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="529ca-104">По различным причинам можно настроить эту таблицу для удовлетворения ваших потребностей.</span><span class="sxs-lookup"><span data-stu-id="529ca-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="529ca-105">Если вы настраиваете таблице журнала миграции *после* применение миграций, вы несете ответственность за обновление существующей таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="529ca-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

<a name="schema-and-table-name"></a><span data-ttu-id="529ca-106">Имя схемы и таблицы</span><span class="sxs-lookup"><span data-stu-id="529ca-106">Schema and table name</span></span>
----------------------
<span data-ttu-id="529ca-107">Можно изменить схему и имя таблицы с помощью `MigrationsHistoryTable()` метод в `OnConfiguring()` (или `ConfigureServices()` на ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="529ca-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="529ca-108">Ниже приведен пример с помощью поставщика SQL Server EF Core.</span><span class="sxs-lookup"><span data-stu-id="529ca-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a><span data-ttu-id="529ca-109">Прочие изменения</span><span class="sxs-lookup"><span data-stu-id="529ca-109">Other changes</span></span>
-------------
<span data-ttu-id="529ca-110">Чтобы настроить дополнительные аспекты класса таблицы, переопределение и замените специфический для поставщика `IHistoryRepository` службы.</span><span class="sxs-lookup"><span data-stu-id="529ca-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="529ca-111">Ниже приведен пример изменения имени столбца MigrationId для *идентификатор* на сервере SQL Server.</span><span class="sxs-lookup"><span data-stu-id="529ca-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="529ca-112">`SqlServerHistoryRepository` находится внутри внутреннего пространства имен и может измениться в будущих версиях.</span><span class="sxs-lookup"><span data-stu-id="529ca-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

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
