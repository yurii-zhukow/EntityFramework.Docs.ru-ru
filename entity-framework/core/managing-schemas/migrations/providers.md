---
title: Миграция с несколькими поставщиками — EF Core
description: Использование миграций для управления схемами баз данных при использовании нескольких поставщиков баз данных с Entity Framework Core
author: bricelam
ms.author: bricelam
ms.date: 11/08/2017
uid: core/managing-schemas/migrations/providers
ms.openlocfilehash: df38af6ac700a530894b98e1f29bbe804831bad5
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89619185"
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

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```

***

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

Если операции можно применить только к одному поставщику или они отличаются между поставщиками, используйте свойство, `ActiveProvider` чтобы определить, какой именно поставщик является активным:

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```

  [1]: xref:core/miscellaneous/cli/index
  [2]: xref:core/managing-schemas/migrations/projects
