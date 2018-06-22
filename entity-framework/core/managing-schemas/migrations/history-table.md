---
title: Таблицы журнала миграций пользовательских - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: cb9892241f3d7f1fae6293bd60a8a5c3e7120969
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
ms.locfileid: "26053814"
---
<a name="custom-migrations-history-table"></a>Таблицы журнала миграций, пользовательские
===============================
По умолчанию EF ядра отслеживает отслеживания объекта какие миграции были применены к базе данных путем их записи в таблице с именем `__EFMigrationsHistory`. По различным причинам может потребоваться настроить эту таблицу для удовлетворения ваших потребностей.

> [!IMPORTANT]
> При настройке таблицы журнала миграций *после* применение миграции, вы несете ответственность за обновление существующей таблицы в базе данных.

<a name="schema-and-table-name"></a>Имя схемы и таблицы
----------------------
Можно изменить схему и имя таблицы с помощью `MigrationsHistoryTable()` метод в `OnConfiguring()` (или `ConfigureServices()` в ASP.NET Core). Ниже приведен пример с помощью поставщика SQL Server EF Core.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a>Прочие изменения
-------------
Чтобы настроить дополнительные аспекты таблицы, переопределить и заменить поставщика `IHistoryRepository` службы. Ниже приведен пример изменить имя столбца MigrationId *идентификатор* на сервере SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository`внутри внутреннего пространства имен и может измениться в будущих версиях.

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
