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
# <a name="custom-migrations-history-table"></a>Таблица журнала настраиваемых миграций

По умолчанию EF Core отслеживает, какие миграции были применены к базе данных, записывая их в таблицу с именем `__EFMigrationsHistory`. По различным причинам может потребоваться настроить эту таблицу в соответствии со своими потребностями.

> [!IMPORTANT]
> При настройке таблицы журнала миграции *после* применения миграции вы несете ответственность за обновление существующей таблицы в базе данных.

## <a name="schema-and-table-name"></a>Имя схемы и таблицы

Изменить схему и имя таблицы можно с помощью метода `MigrationsHistoryTable()` в `OnConfiguring()` (или `ConfigureServices()` в ASP.NET Core). Ниже приведен пример использования поставщика SQL Server EF Core.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

## <a name="other-changes"></a>Прочие изменения

Чтобы настроить дополнительные аспекты таблицы, переопределите и замените службу `IHistoryRepository`, зависящую от поставщика. Ниже приведен пример изменения имени столбца Мигратионид на *идентификатор* SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository` находится внутри внутреннего пространства имен и может измениться в будущих выпусках.

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
