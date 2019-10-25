---
title: Миграция с несколькими поставщиками — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/08/2017
uid: core/managing-schemas/migrations/providers
ms.openlocfilehash: c9b1a2563ef548e592374f90a6242b0bd851bc98
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811958"
---
# <a name="migrations-with-multiple-providers"></a>Миграция с несколькими поставщиками

[Средства EF Core][1] только миграция шаблонов для активного поставщика. Однако иногда может потребоваться использовать более одного поставщика (например Microsoft SQL Server и SQLite) с DbContext. Существует два способа справиться с миграцией. Можно поддерживать два набора миграций — по одному для каждого поставщика, или объединять их в один набор, который может работать с обоими.

## <a name="two-migration-sets"></a>Два набора миграции

При первом подходе создается две миграции для каждого изменения модели.

Один из способов сделать это — разместить каждый набор миграции [в отдельной сборке][2] и вручную переключить активный поставщик (и сборку миграции) между добавлением двух миграций.

Другим подходом, который упрощает работу с инструментами, является создание нового типа, производного от DbContext, и переопределение активного поставщика. Этот тип используется во время разработки при добавлении или применении миграций.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Поскольку каждый набор миграции использует собственные типы DbContext, этот подход не требует использования отдельной сборки миграции.

При добавлении новой миграции укажите типы контекста.

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```

``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> Не нужно указывать выходной каталог для последующих миграций, так как они создаются в качестве элементов того же уровня, что и последний.

## <a name="one-migration-set"></a>Один набор миграции

Если вы не хотите использовать два набора миграций, можно вручную объединить их в один набор, который можно применить к обоим поставщикам.

Заметки могут сосуществовать, так как поставщик игнорирует любые заметки, которые он не понимает. Например, первичный ключевой столбец, который работает с Microsoft SQL Server и SQLite, может выглядеть следующим образом.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

Если операции могут применяться только к одному поставщику (или они отличаются между поставщиками), используйте свойство `ActiveProvider`, чтобы определить, какой поставщик является активным.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```

  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
