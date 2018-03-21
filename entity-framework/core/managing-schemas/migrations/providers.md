---
title: "Миграция с несколькими поставщиками - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d950e74ed4cef7d4274aabcf3eda7b0b735574c6
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2018
---
<a name="migrations-with-multiple-providers"></a><span data-ttu-id="bdf47-102">Миграция с несколькими поставщиками</span><span class="sxs-lookup"><span data-stu-id="bdf47-102">Migrations with Multiple Providers</span></span>
==================================
<span data-ttu-id="bdf47-103">[EF основные инструменты] [ 1] только формировать миграции для поставщика active Directory.</span><span class="sxs-lookup"><span data-stu-id="bdf47-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="bdf47-104">Иногда тем не менее, вы можете использовать больше одного поставщика (например, Microsoft SQL Server и SQLite) с вашей DbContext.</span><span class="sxs-lookup"><span data-stu-id="bdf47-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="bdf47-105">Для обработки в данном миграции двумя способами.</span><span class="sxs-lookup"><span data-stu-id="bdf47-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="bdf47-106">Можно сохранить двумя наборами миграций — один для каждого поставщика--или объединить их в один набор, можно работать с обеими.</span><span class="sxs-lookup"><span data-stu-id="bdf47-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

<a name="two-migration-sets"></a><span data-ttu-id="bdf47-107">Два набора миграции</span><span class="sxs-lookup"><span data-stu-id="bdf47-107">Two migration sets</span></span>
------------------
<span data-ttu-id="bdf47-108">Первый способ создания двух новых проектах для каждого изменения модели.</span><span class="sxs-lookup"><span data-stu-id="bdf47-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="bdf47-109">Один из способов сделать это для размещения каждого набора миграции [в отдельной сборке] [ 2] и вручную переключаться между добавление двух новых проектах поставщика активной (и миграция сборки).</span><span class="sxs-lookup"><span data-stu-id="bdf47-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="bdf47-110">Другой подход, который упрощает работу со средствами является создание новый тип, который является производным от вашего DbContext и переопределяет активный поставщик.</span><span class="sxs-lookup"><span data-stu-id="bdf47-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="bdf47-111">Этот тип используется на этапе разработки времени при добавлении или применения миграции.</span><span class="sxs-lookup"><span data-stu-id="bdf47-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="bdf47-112">Поскольку каждый набор миграции используются собственные типы DbContext, такой подход не требует использования сборки отдельных миграции.</span><span class="sxs-lookup"><span data-stu-id="bdf47-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="bdf47-113">При добавлении новой миграции, укажите типы контекстов.</span><span class="sxs-lookup"><span data-stu-id="bdf47-113">When adding new migration, specify the context types.</span></span>

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> <span data-ttu-id="bdf47-114">Нет необходимости указывать выходной каталог для последующей миграции, поскольку они создаются общего родителя последним.</span><span class="sxs-lookup"><span data-stu-id="bdf47-114">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

<a name="one-migration-set"></a><span data-ttu-id="bdf47-115">Набор одной миграции</span><span class="sxs-lookup"><span data-stu-id="bdf47-115">One migration set</span></span>
-----------------
<span data-ttu-id="bdf47-116">Если вас не устраивает, наличие двух наборов миграций, можно вручную объединить их в один набор, который может применяться для обоих поставщиков.</span><span class="sxs-lookup"><span data-stu-id="bdf47-116">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="bdf47-117">Заметки могут сосуществовать, поскольку поставщик не учитывает любые заметки, которые он не распознает.</span><span class="sxs-lookup"><span data-stu-id="bdf47-117">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="bdf47-118">Например столбец первичного ключа, который работает с Microsoft SQL Server и SQLite может выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="bdf47-118">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="bdf47-119">Если операции могут применяться только на один поставщик (или если они по-разному между поставщиками), используйте `ActiveProvider` свойство, чтобы определить, какой поставщик active.</span><span class="sxs-lookup"><span data-stu-id="bdf47-119">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
