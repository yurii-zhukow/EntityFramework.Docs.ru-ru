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
<a name="custom-migrations-history-table"></a>Таблица миграции пользовательского журнала
===============================
По умолчанию EF Core следит за миграций, которые были применены к базе данных, записав их в таблицу с именем `__EFMigrationsHistory`. По различным причинам можно настроить эту таблицу для удовлетворения ваших потребностей.

> [!IMPORTANT]
> Если вы настраиваете таблице журнала миграции *после* применение миграций, вы несете ответственность за обновление существующей таблицы в базе данных.

<a name="schema-and-table-name"></a>Имя схемы и таблицы
----------------------
Можно изменить схему и имя таблицы с помощью `MigrationsHistoryTable()` метод в `OnConfiguring()` (или `ConfigureServices()` на ASP.NET Core). Ниже приведен пример с помощью поставщика SQL Server EF Core.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a>Прочие изменения
-------------
Чтобы настроить дополнительные аспекты класса таблицы, переопределение и замените специфический для поставщика `IHistoryRepository` службы. Ниже приведен пример изменения имени столбца MigrationId для *идентификатор* на сервере SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository` находится внутри внутреннего пространства имен и может измениться в будущих версиях.

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
