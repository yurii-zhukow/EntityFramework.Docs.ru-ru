---
title: Миграция с несколькими поставщиками — EF Core
description: Использование миграций для управления схемами баз данных при использовании нескольких поставщиков баз данных с Entity Framework Core
author: bricelam
ms.date: 11/08/2017
uid: core/managing-schemas/migrations/providers
ms.openlocfilehash: f44abb5156ea3a175c68c1a0ec23ff41a9d13452
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92061987"
---
# <a name="migrations-with-multiple-providers"></a><span data-ttu-id="73260-103">Миграция с несколькими поставщиками</span><span class="sxs-lookup"><span data-stu-id="73260-103">Migrations with Multiple Providers</span></span>

<span data-ttu-id="73260-104">[Средства EF Core][1] только миграция шаблонов для активного поставщика.</span><span class="sxs-lookup"><span data-stu-id="73260-104">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="73260-105">Однако иногда может потребоваться использовать более одного поставщика (например Microsoft SQL Server и SQLite) с DbContext.</span><span class="sxs-lookup"><span data-stu-id="73260-105">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="73260-106">Существует два способа справиться с миграцией.</span><span class="sxs-lookup"><span data-stu-id="73260-106">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="73260-107">Можно поддерживать два набора миграций — по одному для каждого поставщика, или объединять их в один набор, который может работать с обоими.</span><span class="sxs-lookup"><span data-stu-id="73260-107">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

## <a name="two-migration-sets"></a><span data-ttu-id="73260-108">Два набора миграции</span><span class="sxs-lookup"><span data-stu-id="73260-108">Two migration sets</span></span>

<span data-ttu-id="73260-109">При первом подходе создается две миграции для каждого изменения модели.</span><span class="sxs-lookup"><span data-stu-id="73260-109">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="73260-110">Один из способов сделать это — разместить каждый набор миграции [в отдельной сборке][2] и вручную переключить активный поставщик (и сборку миграции) между добавлением двух миграций.</span><span class="sxs-lookup"><span data-stu-id="73260-110">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="73260-111">Другим подходом, который упрощает работу с инструментами, является создание нового типа, производного от DbContext, и переопределение активного поставщика.</span><span class="sxs-lookup"><span data-stu-id="73260-111">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="73260-112">Этот тип используется во время разработки при добавлении или применении миграций.</span><span class="sxs-lookup"><span data-stu-id="73260-112">This type is used at design time when adding or applying migrations.</span></span>

```csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="73260-113">Поскольку каждый набор миграции использует собственные типы DbContext, этот подход не требует использования отдельной сборки миграции.</span><span class="sxs-lookup"><span data-stu-id="73260-113">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="73260-114">При добавлении новой миграции укажите типы контекста.</span><span class="sxs-lookup"><span data-stu-id="73260-114">When adding new migration, specify the context types.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="73260-115">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="73260-115">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

### <a name="visual-studio"></a>[<span data-ttu-id="73260-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73260-116">Visual Studio</span></span>](#tab/vs)

```powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```

***

> [!TIP]
> <span data-ttu-id="73260-117">Не нужно указывать выходной каталог для последующих миграций, так как они создаются в качестве элементов того же уровня, что и последний.</span><span class="sxs-lookup"><span data-stu-id="73260-117">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

## <a name="one-migration-set"></a><span data-ttu-id="73260-118">Один набор миграции</span><span class="sxs-lookup"><span data-stu-id="73260-118">One migration set</span></span>

<span data-ttu-id="73260-119">Если вы не хотите использовать два набора миграций, можно вручную объединить их в один набор, который можно применить к обоим поставщикам.</span><span class="sxs-lookup"><span data-stu-id="73260-119">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="73260-120">Заметки могут сосуществовать, так как поставщик игнорирует любые заметки, которые он не понимает.</span><span class="sxs-lookup"><span data-stu-id="73260-120">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="73260-121">Например, первичный ключевой столбец, который работает с Microsoft SQL Server и SQLite, может выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="73260-121">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

```csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="73260-122">Если операции можно применить только к одному поставщику или они отличаются между поставщиками, используйте свойство, `ActiveProvider` чтобы определить, какой именно поставщик является активным:</span><span class="sxs-lookup"><span data-stu-id="73260-122">If operations can be applied only for one provider, or they're different between providers, use the `ActiveProvider` property to determine which provider is active:</span></span>

```csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```

  [1]: xref:core/miscellaneous/cli/index
  [2]: xref:core/managing-schemas/migrations/projects
