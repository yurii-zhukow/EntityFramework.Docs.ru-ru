---
title: Миграция с несколькими провайдерами — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/08/2017
ms.openlocfilehash: 75c055221609679db3f00016b9cb44c6c8c6e473
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488781"
---
<a name="migrations-with-multiple-providers"></a>Миграция с несколькими поставщиками
==================================
[Инструменты EF Core] [ 1] только сформировать шаблон миграции для поставщика active Directory. Иногда тем не менее, можно использовать более одного поставщика (например, Microsoft SQL Server и SQLite) с DbContext. Существует два способа решения этой с Миграциями. Вы можете поддерживать две миграции — один для каждого поставщика--или объединения их в один набор, можно работать с обеими.

<a name="two-migration-sets"></a>Два набора миграции
------------------
Первый способ вы создаете две операции миграции для каждого изменения модели.

Один из способов сделать это для размещения каждого набора миграции [в отдельной сборке] [ 2] и переключения между добавлением две операции миграции поставщика активной (и миграции сборки).

Другой подход, который упрощает работу со средствами — создать новый тип, который является производным от DbContext и переопределяет активный поставщик. Этот тип используется на этапе разработки времени при добавлении или применение миграций.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Так как каждый набор миграции использует собственные типы DbContext, этот подход не требует использования отдельных миграций сборки.

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
> Не нужно указать выходной каталог для последующей миграции, так как они создаются как элементы одного уровня до последнего.

<a name="one-migration-set"></a>Набор одной миграции
-----------------
Если вас не устраивает, наличие двух наборов миграций, их можно вручную объединить в один набор, который может применяться для обоих поставщиков.

Заметки могут сосуществовать, так как поставщик не учитывает любые заметки, которые не понимает. Например столбец первичного ключа, который работает с Microsoft SQL Server и SQLite может выглядеть следующим образом.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

Если операции могут применяться только на один поставщик (или если они по-разному между поставщиками), используйте `ActiveProvider` свойство, чтобы определить, какой поставщик является активным.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
