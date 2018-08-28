---
title: Миграция с несколькими провайдерами — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.openlocfilehash: 7ae695037992323337a780cda29d8c8ed8a13458
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997977"
---
<a name="migrations-with-multiple-providers"></a><span data-ttu-id="8242e-102">Миграция с несколькими поставщиками</span><span class="sxs-lookup"><span data-stu-id="8242e-102">Migrations with Multiple Providers</span></span>
==================================
<span data-ttu-id="8242e-103">[Инструменты EF Core] [ 1] только сформировать шаблон миграции для поставщика active Directory.</span><span class="sxs-lookup"><span data-stu-id="8242e-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="8242e-104">Иногда тем не менее, можно использовать более одного поставщика (например, Microsoft SQL Server и SQLite) с DbContext.</span><span class="sxs-lookup"><span data-stu-id="8242e-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="8242e-105">Существует два способа решения этой с Миграциями.</span><span class="sxs-lookup"><span data-stu-id="8242e-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="8242e-106">Вы можете поддерживать две миграции — один для каждого поставщика--или объединения их в один набор, можно работать с обеими.</span><span class="sxs-lookup"><span data-stu-id="8242e-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

<a name="two-migration-sets"></a><span data-ttu-id="8242e-107">Два набора миграции</span><span class="sxs-lookup"><span data-stu-id="8242e-107">Two migration sets</span></span>
------------------
<span data-ttu-id="8242e-108">Первый способ вы создаете две операции миграции для каждого изменения модели.</span><span class="sxs-lookup"><span data-stu-id="8242e-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="8242e-109">Один из способов сделать это для размещения каждого набора миграции [в отдельной сборке] [ 2] и переключения между добавлением две операции миграции поставщика активной (и миграции сборки).</span><span class="sxs-lookup"><span data-stu-id="8242e-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="8242e-110">Другой подход, который упрощает работу со средствами — создать новый тип, который является производным от DbContext и переопределяет активный поставщик.</span><span class="sxs-lookup"><span data-stu-id="8242e-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="8242e-111">Этот тип используется на этапе разработки времени при добавлении или применение миграций.</span><span class="sxs-lookup"><span data-stu-id="8242e-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="8242e-112">Так как каждый набор миграции использует собственные типы DbContext, этот подход не требует использования отдельных миграций сборки.</span><span class="sxs-lookup"><span data-stu-id="8242e-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="8242e-113">При добавлении новой миграции, укажите типы контекстов.</span><span class="sxs-lookup"><span data-stu-id="8242e-113">When adding new migration, specify the context types.</span></span>

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> <span data-ttu-id="8242e-114">Не нужно указать выходной каталог для последующей миграции, так как они создаются как элементы одного уровня до последнего.</span><span class="sxs-lookup"><span data-stu-id="8242e-114">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

<a name="one-migration-set"></a><span data-ttu-id="8242e-115">Набор одной миграции</span><span class="sxs-lookup"><span data-stu-id="8242e-115">One migration set</span></span>
-----------------
<span data-ttu-id="8242e-116">Если вас не устраивает, наличие двух наборов миграций, их можно вручную объединить в один набор, который может применяться для обоих поставщиков.</span><span class="sxs-lookup"><span data-stu-id="8242e-116">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="8242e-117">Заметки могут сосуществовать, так как поставщик не учитывает любые заметки, которые не понимает.</span><span class="sxs-lookup"><span data-stu-id="8242e-117">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="8242e-118">Например столбец первичного ключа, который работает с Microsoft SQL Server и SQLite может выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="8242e-118">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="8242e-119">Если операции могут применяться только на один поставщик (или если они по-разному между поставщиками), используйте `ActiveProvider` свойство, чтобы определить, какой поставщик является активным.</span><span class="sxs-lookup"><span data-stu-id="8242e-119">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
