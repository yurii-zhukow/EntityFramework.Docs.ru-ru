---
title: "Миграция с несколькими поставщиками - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 6b278a5ae270b6a84269dffd72eeca609b168cdd
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2017
---
<a name="migrations-with-multiple-providers"></a>Миграция с несколькими поставщиками
==================================
[EF основные инструменты] [ 1] только формировать миграции для поставщика active Directory. Иногда тем не менее, вы можете использовать больше одного поставщика (например, Microsoft SQL Server и SQLite) с вашей DbContext. Для обработки в данном миграции двумя способами. Можно сохранить двумя наборами миграций — один для каждого поставщика--или объединить их в один набор, можно работать с обеими.

<a name="two-migration-sets"></a>Два набора миграции
------------------
Первый способ создания двух новых проектах для каждого изменения модели.

Один из способов сделать это для размещения каждого набора миграции [в отдельной сборке] [ 2] и вручную переключаться между добавление двух новых проектах поставщика активной (и миграция сборки).

Другой подход, что упрощает работу со средствами проще будет создать новый тип является производным от вашего DbContext и переопределяет активный поставщик. Этот тип используется на этапе разработки времени при добавлении или применения миграции.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Поскольку каждый набор миграции используются собственные типы DbContext, такой подход не требует использования сборки отдельных миграции.

При добавлении новой миграции, укажите типы контекстов.

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> Нет необходимости указывать выходной каталог для последующей миграции, поскольку они создаются общего родителя последним.

<a name="one-migration-set"></a>Набор одной миграции
-----------------
Если вас не устраивает, наличие двух наборов миграций, можно вручную объединить их в один набор, который может применяться для обоих поставщиков.

Заметки могут сосуществовать, поскольку поставщик не учитывает любые заметки, которые он не распознает. Например столбец первичного ключа, который работает с Microsoft SQL Server и SQLite может выглядеть следующим образом.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

Если операции могут применяться только на один поставщик (или если они по-разному между поставщиками), используйте `ActiveProvider` свойство, чтобы определить, какой поставщик active.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
