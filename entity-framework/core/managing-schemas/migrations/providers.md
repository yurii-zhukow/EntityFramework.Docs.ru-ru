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
# <a name="migrations-with-multiple-providers"></a><span data-ttu-id="c216e-102">Миграция с несколькими поставщиками</span><span class="sxs-lookup"><span data-stu-id="c216e-102">Migrations with Multiple Providers</span></span>

<span data-ttu-id="c216e-103">[Средства EF Core][1] только миграция шаблонов для активного поставщика.</span><span class="sxs-lookup"><span data-stu-id="c216e-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="c216e-104">Однако иногда может потребоваться использовать более одного поставщика (например Microsoft SQL Server и SQLite) с DbContext.</span><span class="sxs-lookup"><span data-stu-id="c216e-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="c216e-105">Существует два способа справиться с миграцией.</span><span class="sxs-lookup"><span data-stu-id="c216e-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="c216e-106">Можно поддерживать два набора миграций — по одному для каждого поставщика, или объединять их в один набор, который может работать с обоими.</span><span class="sxs-lookup"><span data-stu-id="c216e-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

## <a name="two-migration-sets"></a><span data-ttu-id="c216e-107">Два набора миграции</span><span class="sxs-lookup"><span data-stu-id="c216e-107">Two migration sets</span></span>

<span data-ttu-id="c216e-108">При первом подходе создается две миграции для каждого изменения модели.</span><span class="sxs-lookup"><span data-stu-id="c216e-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="c216e-109">Один из способов сделать это — разместить каждый набор миграции [в отдельной сборке][2] и вручную переключить активный поставщик (и сборку миграции) между добавлением двух миграций.</span><span class="sxs-lookup"><span data-stu-id="c216e-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="c216e-110">Другим подходом, который упрощает работу с инструментами, является создание нового типа, производного от DbContext, и переопределение активного поставщика.</span><span class="sxs-lookup"><span data-stu-id="c216e-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="c216e-111">Этот тип используется во время разработки при добавлении или применении миграций.</span><span class="sxs-lookup"><span data-stu-id="c216e-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="c216e-112">Поскольку каждый набор миграции использует собственные типы DbContext, этот подход не требует использования отдельной сборки миграции.</span><span class="sxs-lookup"><span data-stu-id="c216e-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="c216e-113">При добавлении новой миграции укажите типы контекста.</span><span class="sxs-lookup"><span data-stu-id="c216e-113">When adding new migration, specify the context types.</span></span>

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```

``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> <span data-ttu-id="c216e-114">Не нужно указывать выходной каталог для последующих миграций, так как они создаются в качестве элементов того же уровня, что и последний.</span><span class="sxs-lookup"><span data-stu-id="c216e-114">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

## <a name="one-migration-set"></a><span data-ttu-id="c216e-115">Один набор миграции</span><span class="sxs-lookup"><span data-stu-id="c216e-115">One migration set</span></span>

<span data-ttu-id="c216e-116">Если вы не хотите использовать два набора миграций, можно вручную объединить их в один набор, который можно применить к обоим поставщикам.</span><span class="sxs-lookup"><span data-stu-id="c216e-116">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="c216e-117">Заметки могут сосуществовать, так как поставщик игнорирует любые заметки, которые он не понимает.</span><span class="sxs-lookup"><span data-stu-id="c216e-117">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="c216e-118">Например, первичный ключевой столбец, который работает с Microsoft SQL Server и SQLite, может выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="c216e-118">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="c216e-119">Если операции могут применяться только к одному поставщику (или они отличаются между поставщиками), используйте свойство `ActiveProvider`, чтобы определить, какой поставщик является активным.</span><span class="sxs-lookup"><span data-stu-id="c216e-119">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```

  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
