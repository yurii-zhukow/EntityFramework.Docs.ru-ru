---
title: Управление миграцией — EF Core
author: bricelam
ms.author: bricelam
ms.date: 05/06/2020
uid: core/managing-schemas/migrations/managing
ms.openlocfilehash: e52d3680360a1e83e05f04650c735c5a67680094
ms.sourcegitcommit: 31536e52b838a84680d2e93e5bb52fb16df72a97
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2020
ms.locfileid: "86238738"
---
# <a name="managing-migrations"></a><span data-ttu-id="6f519-102">Управление миграцией</span><span class="sxs-lookup"><span data-stu-id="6f519-102">Managing Migrations</span></span>

<span data-ttu-id="6f519-103">При изменении модели миграция добавляется и удаляется в рамках обычной разработки, а файлы миграции возвращаются в систему управления версиями проекта.</span><span class="sxs-lookup"><span data-stu-id="6f519-103">As your model changes, migrations are added and removed as part of normal development, and the migration files are checked into your project's source control.</span></span> <span data-ttu-id="6f519-104">Для управления миграцией необходимо сначала установить [EF Core программ командной строки](xref:core/miscellaneous/cli/index).</span><span class="sxs-lookup"><span data-stu-id="6f519-104">To manage migrations, you must first install the [EF Core command-line tools](xref:core/miscellaneous/cli/index).</span></span>

> [!TIP]
> <span data-ttu-id="6f519-105">Если `DbContext` находится не в той же сборке, что и начальный проект, можно явным образом указать целевой и начальный проекты в [средствах консоли диспетчера пакетов](xref:core/miscellaneous/cli/powershell#target-and-startup-project) или в [средствах .NET Core CLI](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).</span><span class="sxs-lookup"><span data-stu-id="6f519-105">If the `DbContext` is in a different assembly than the startup project, you can explicitly specify the target and startup projects in either the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell#target-and-startup-project) or the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="6f519-106">Добавление миграции</span><span class="sxs-lookup"><span data-stu-id="6f519-106">Add a migration</span></span>

<span data-ttu-id="6f519-107">После изменения модели можно добавить миграцию для этого изменения:</span><span class="sxs-lookup"><span data-stu-id="6f519-107">After your model has been changed, you can add a migration for that change:</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="6f519-108">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="6f519-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add AddBlogCreatedTimestamp
```

### <a name="visual-studio"></a>[<span data-ttu-id="6f519-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6f519-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration AddBlogCreatedTimestamp
```

***

<span data-ttu-id="6f519-110">Имя миграции можно использовать как сообщение фиксации в системе управления версиями.</span><span class="sxs-lookup"><span data-stu-id="6f519-110">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="6f519-111">Например, можно выбрать имя, например *аддблогкреатедтиместамп* , если изменение является новым `CreatedTimestamp` свойством `Blog` сущности.</span><span class="sxs-lookup"><span data-stu-id="6f519-111">For example, you might choose a name like *AddBlogCreatedTimestamp* if the change is a new `CreatedTimestamp` property on your `Blog` entity.</span></span>

<span data-ttu-id="6f519-112">В каталог **Migrations** проекта добавляются три файла.</span><span class="sxs-lookup"><span data-stu-id="6f519-112">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="6f519-113">**XXXXXXXXXXXXXX_AddCreatedTimestamp. CS**— основной файл миграции.</span><span class="sxs-lookup"><span data-stu-id="6f519-113">**XXXXXXXXXXXXXX_AddCreatedTimestamp.cs**--The main migrations file.</span></span> <span data-ttu-id="6f519-114">Содержит операции, необходимые для применения миграции (в `Up`) и ее отмены (в `Down`).</span><span class="sxs-lookup"><span data-stu-id="6f519-114">Contains the operations necessary to apply the migration (in `Up`) and to revert it (in `Down`).</span></span>
* <span data-ttu-id="6f519-115">**XXXXXXXXXXXXXX_AddCreatedTimestamp. Designer. CS**— файл метаданных миграции.</span><span class="sxs-lookup"><span data-stu-id="6f519-115">**XXXXXXXXXXXXXX_AddCreatedTimestamp.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="6f519-116">Содержит сведения, используемые EF.</span><span class="sxs-lookup"><span data-stu-id="6f519-116">Contains information used by EF.</span></span>
* <span data-ttu-id="6f519-117">**MyContextModelSnapshot.cs** — моментальный снимок текущей модели.</span><span class="sxs-lookup"><span data-stu-id="6f519-117">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="6f519-118">Используется для определения появившихся изменений при добавлении следующей миграции.</span><span class="sxs-lookup"><span data-stu-id="6f519-118">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="6f519-119">Метка времени в именах файлов позволяет расположить их в хронологическом порядке, чтобы вы могли отслеживать ход изменений.</span><span class="sxs-lookup"><span data-stu-id="6f519-119">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

### <a name="namespaces"></a><span data-ttu-id="6f519-120">Пространства имен</span><span class="sxs-lookup"><span data-stu-id="6f519-120">Namespaces</span></span>

<span data-ttu-id="6f519-121">Вы можете свободно перемещать файлы миграций и изменять их пространство имен вручную.</span><span class="sxs-lookup"><span data-stu-id="6f519-121">You are free to move Migrations files and change their namespace manually.</span></span> <span data-ttu-id="6f519-122">Новые миграции создаются в виде элементов того же уровня, что и последняя миграция.</span><span class="sxs-lookup"><span data-stu-id="6f519-122">New migrations are created as siblings of the last migration.</span></span> <span data-ttu-id="6f519-123">Кроме того, можно указать пространство имен во время формирования следующим образом:</span><span class="sxs-lookup"><span data-stu-id="6f519-123">Alternatively, you can specify the namespace at generation time as follows:</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="6f519-124">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="6f519-124">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate --namespace Your.Namespace
```

### <a name="visual-studio"></a>[<span data-ttu-id="6f519-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6f519-125">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration InitialCreate -Namespace Your.Namespace
```

***

## <a name="customize-migration-code"></a><span data-ttu-id="6f519-126">Настройка кода миграции</span><span class="sxs-lookup"><span data-stu-id="6f519-126">Customize migration code</span></span>

<span data-ttu-id="6f519-127">Хотя EF Core обычно создает точную миграцию, следует всегда проверять код и убедиться, что он соответствует требуемому изменению. в некоторых случаях это даже необходимо сделать.</span><span class="sxs-lookup"><span data-stu-id="6f519-127">While EF Core generally creates accurate migrations, you should always review the code and make sure it corresponds to the desired change; in some cases, it is even necessary to do so.</span></span>

### <a name="column-renames"></a><span data-ttu-id="6f519-128">Переименование столбцов</span><span class="sxs-lookup"><span data-stu-id="6f519-128">Column renames</span></span>

<span data-ttu-id="6f519-129">Одним из важных примеров, где требуется настройка миграции, является переименование свойства.</span><span class="sxs-lookup"><span data-stu-id="6f519-129">One notable example where customizing migrations is required is when renaming a property.</span></span> <span data-ttu-id="6f519-130">Например, если переименовать свойство из `Name` в `FullName` , EF Core создаст следующую миграцию:</span><span class="sxs-lookup"><span data-stu-id="6f519-130">For example, if you rename a property from `Name` to `FullName`, EF Core will generate the following migration:</span></span>

```c#
migrationBuilder.DropColumn(
    name: "Name",
    table: "Customers");

migrationBuilder.AddColumn<string>(
    name: "FullName",
    table: "Customers",
    nullable: true);
```

<span data-ttu-id="6f519-131">EF Core, как правило, не может быть уверенным в том, что нужно удалить столбец и создать новый (два отдельных изменения) и когда столбец должен быть переименован.</span><span class="sxs-lookup"><span data-stu-id="6f519-131">EF Core is generally unable to know when the intention is to drop a column and create a new one (two separate changes), and when a column should be renamed.</span></span> <span data-ttu-id="6f519-132">Если приведенная выше миграция применяется "как есть", все имена клиентов будут потеряны.</span><span class="sxs-lookup"><span data-stu-id="6f519-132">If the above migration is applied as-is, all your customer names will be lost.</span></span> <span data-ttu-id="6f519-133">Чтобы переименовать столбец, замените созданную выше миграцию следующей:</span><span class="sxs-lookup"><span data-stu-id="6f519-133">To rename a column, replace the above generated migration with the following:</span></span>

```c#
migrationBuilder.RenameColumn(
    name: "Name",
    table: "Customers",
    newName: "FullName");
```

> [!TIP]
> <span data-ttu-id="6f519-134">Процесс создания новой миграции предупреждает, если операция может привести к потере данных (например, содержит удаление столбца).</span><span class="sxs-lookup"><span data-stu-id="6f519-134">The migration scaffolding process warns when an operation might result in data loss (like dropping a column).</span></span> <span data-ttu-id="6f519-135">Увидев это предупреждение, проверьте точность кода миграции с особой тщательностью.</span><span class="sxs-lookup"><span data-stu-id="6f519-135">If you see that warning, be especially sure to review the migrations code for accuracy.</span></span>

### <a name="adding-raw-sql"></a><span data-ttu-id="6f519-136">Добавление необработанного SQL</span><span class="sxs-lookup"><span data-stu-id="6f519-136">Adding raw SQL</span></span>

<span data-ttu-id="6f519-137">В то время как переименование столбца может быть выполнено через встроенный API, во многих случаях это невозможно.</span><span class="sxs-lookup"><span data-stu-id="6f519-137">While renaming a column can be achieved via a built-in API, in many cases that is not possible.</span></span> <span data-ttu-id="6f519-138">Например, может потребоваться заменить существующие `FirstName` `LastColumn` Свойства и одним новым `FullName` свойством.</span><span class="sxs-lookup"><span data-stu-id="6f519-138">For example, we may want to replace existing `FirstName` and `LastColumn` properties with a single, new `FullName` property.</span></span> <span data-ttu-id="6f519-139">Процесс миграции, создаваемый EF Core, будет следующим:</span><span class="sxs-lookup"><span data-stu-id="6f519-139">The migration generated by EF Core will be the following:</span></span>

``` csharp
migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");

migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);
```

<span data-ttu-id="6f519-140">Как и раньше, это вызовет нежелательные потери данных.</span><span class="sxs-lookup"><span data-stu-id="6f519-140">As before, this would cause unwanted data loss.</span></span> <span data-ttu-id="6f519-141">Чтобы перенести данные из старых столбцов, мы переупорядочиваем миграцию и представляем необработанную операцию SQL следующим образом:</span><span class="sxs-lookup"><span data-stu-id="6f519-141">To transfer the data from the old columns, we rearrange the migrations and introduce a raw SQL operation as follows:</span></span>

``` csharp
migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);

migrationBuilder.Sql(
@"
    UPDATE Customer
    SET Name = FirstName + ' ' + LastName;
");

migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");
```

### <a name="arbitrary-changes-via-raw-sql"></a><span data-ttu-id="6f519-142">Произвольные изменения через необработанный SQL</span><span class="sxs-lookup"><span data-stu-id="6f519-142">Arbitrary changes via raw SQL</span></span>

<span data-ttu-id="6f519-143">Необработанный SQL можно также использовать для управления объектами базы данных, которые EF Core не знают.</span><span class="sxs-lookup"><span data-stu-id="6f519-143">Raw SQL can also be used to manage database objects that EF Core isn't aware of.</span></span> <span data-ttu-id="6f519-144">Для этого добавьте миграцию, не внося изменения в модель. будет создана пустая миграция, которая затем может быть заполнена необработанными операциями SQL.</span><span class="sxs-lookup"><span data-stu-id="6f519-144">To do this, add a migration without making any model change; an empty migration will be generated, which you can then populate with raw SQL operations.</span></span>

<span data-ttu-id="6f519-145">Например, следующая миграция создает SQL Server хранимую процедуру:</span><span class="sxs-lookup"><span data-stu-id="6f519-145">For example, the following migration creates a SQL Server stored procedure:</span></span>

```c#
migrationBuilder.Sql(
@"
    EXEC ('CREATE PROCEDURE getFullName
        @LastName nvarchar(50),
        @FirstName nvarchar(50)
    AS
        RETURN @LastName + @FirstName;')");
```

<span data-ttu-id="6f519-146">Это можно использовать для управления любым аспектом базы данных, в том числе:</span><span class="sxs-lookup"><span data-stu-id="6f519-146">This can be used to manage any aspect of your database, including:</span></span>

* <span data-ttu-id="6f519-147">Хранимые процедуры</span><span class="sxs-lookup"><span data-stu-id="6f519-147">Stored procedures</span></span>
* <span data-ttu-id="6f519-148">Полнотекстовый поиск</span><span class="sxs-lookup"><span data-stu-id="6f519-148">Full-Text Search</span></span>
* <span data-ttu-id="6f519-149">Функции</span><span class="sxs-lookup"><span data-stu-id="6f519-149">Functions</span></span>
* <span data-ttu-id="6f519-150">Триггеры</span><span class="sxs-lookup"><span data-stu-id="6f519-150">Triggers</span></span>
* <span data-ttu-id="6f519-151">Представления</span><span class="sxs-lookup"><span data-stu-id="6f519-151">Views</span></span>

<span data-ttu-id="6f519-152">В большинстве случаев EF Core будет автоматически переносить каждую миграцию в собственную транзакцию при применении миграции.</span><span class="sxs-lookup"><span data-stu-id="6f519-152">In most cases, EF Core will automatically wrap each migration in its own transaction when applying migrations.</span></span> <span data-ttu-id="6f519-153">К сожалению, некоторые операции миграции не могут быть выполнены внутри транзакции в некоторых базах данных. в таких случаях вы можете отказаться от транзакции, передав ее `suppressTransaction: true` `migrationBuilder.Sql` .</span><span class="sxs-lookup"><span data-stu-id="6f519-153">Unfortunately, some migrations operations cannot be performed within a transaction in some databases; for these cases, you may opt out of the transaction by passing `suppressTransaction: true` to `migrationBuilder.Sql`.</span></span>

<span data-ttu-id="6f519-154">Если `DbContext` находится не в той же сборке, что и начальный проект, можно явным образом указать целевой и начальный проекты в [средствах консоли диспетчера пакетов](xref:core/miscellaneous/cli/powershell#target-and-startup-project) или в [средствах .NET Core CLI](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).</span><span class="sxs-lookup"><span data-stu-id="6f519-154">If the `DbContext` is in a different assembly than the startup project, you can explicitly specify the target and startup projects in either the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell#target-and-startup-project) or the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).</span></span>

## <a name="remove-a-migration"></a><span data-ttu-id="6f519-155">Удаление миграции</span><span class="sxs-lookup"><span data-stu-id="6f519-155">Remove a migration</span></span>

<span data-ttu-id="6f519-156">Иногда, добавив миграцию, вы понимаем, что нужно внести дополнительные изменения в модель EF Core перед ее применением.</span><span class="sxs-lookup"><span data-stu-id="6f519-156">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span> <span data-ttu-id="6f519-157">Для удаления последней миграции используйте приведенную ниже команду.</span><span class="sxs-lookup"><span data-stu-id="6f519-157">To remove the last migration, use this command.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="6f519-158">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="6f519-158">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations remove
```

### <a name="visual-studio"></a>[<span data-ttu-id="6f519-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6f519-159">Visual Studio</span></span>](#tab/vs)

``` powershell
Remove-Migration
```

***

<span data-ttu-id="6f519-160">Удалив миграцию, вы можете внести в модель дополнительные изменения и снова добавить ее.</span><span class="sxs-lookup"><span data-stu-id="6f519-160">After removing the migration, you can make the additional model changes and add it again.</span></span>

> [!WARNING]
> <span data-ttu-id="6f519-161">Не следует удалять все миграции, которые уже применены к рабочим базам данных.</span><span class="sxs-lookup"><span data-stu-id="6f519-161">Take care not to remove any migrations which are already applied to production databases.</span></span> <span data-ttu-id="6f519-162">Если этого не сделать, вы не сможете отменить изменения и может привести к нарушению предположений, сделанных последующими миграциями.</span><span class="sxs-lookup"><span data-stu-id="6f519-162">Not doing so will prevent you from being able to revert it, and may break the assumptions made by subsequent migrations.</span></span>

## <a name="listing-migrations"></a><span data-ttu-id="6f519-163">Перечисление миграций</span><span class="sxs-lookup"><span data-stu-id="6f519-163">Listing migrations</span></span>

<span data-ttu-id="6f519-164">Список всех существующих миграций можно просмотреть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="6f519-164">You can list all existing migrations as follows:</span></span>

```dotnetcli
dotnet ef migrations list
```

## <a name="resetting-all-migrations"></a><span data-ttu-id="6f519-165">Сброс всех миграций</span><span class="sxs-lookup"><span data-stu-id="6f519-165">Resetting all migrations</span></span>

<span data-ttu-id="6f519-166">В некоторых крайних случаях может потребоваться удалить все миграции и начать заново.</span><span class="sxs-lookup"><span data-stu-id="6f519-166">In some extreme cases, it may be necessary to remove all migrations and start over.</span></span> <span data-ttu-id="6f519-167">Это можно легко сделать, удалив папку **миграций** и удалив базу данных. на этом этапе можно создать новую начальную миграцию, которая будет содержать всю текущую схему.</span><span class="sxs-lookup"><span data-stu-id="6f519-167">This can be easily done by deleting your **Migrations** folder and dropping your database; at that point you can create a new initial migration, which will contain you entire current schema.</span></span>

<span data-ttu-id="6f519-168">Кроме того, можно сбросить все миграции и создать одну из них без потери данных.</span><span class="sxs-lookup"><span data-stu-id="6f519-168">It's also possible to reset all migrations and create a single one without losing your data.</span></span> <span data-ttu-id="6f519-169">Это иногда называется "использование параметра squash" и требует выполнения некоторых действий вручную:</span><span class="sxs-lookup"><span data-stu-id="6f519-169">This is sometimes called "squashing", and involves some manual work:</span></span>

* <span data-ttu-id="6f519-170">Удаление папки **миграций**</span><span class="sxs-lookup"><span data-stu-id="6f519-170">Delete your **Migrations** folder</span></span>
* <span data-ttu-id="6f519-171">Создать новую миграцию и создать для нее скрипт SQL</span><span class="sxs-lookup"><span data-stu-id="6f519-171">Create a new migration and generate a SQL script for it</span></span>
* <span data-ttu-id="6f519-172">В базе данных удалите все строки из таблицы журнала миграции.</span><span class="sxs-lookup"><span data-stu-id="6f519-172">In your database, delete all rows from the migrations history table</span></span>
* <span data-ttu-id="6f519-173">Вставка одной строки в журнал миграции для записи того, что первая миграция уже применена, так как таблицы уже есть.</span><span class="sxs-lookup"><span data-stu-id="6f519-173">Insert a single row into the migrations history, to record that the first migration has already been applied, since your tables are already there.</span></span> <span data-ttu-id="6f519-174">СЕКЛ вставки — это последняя операция в созданном выше скрипте SQL.</span><span class="sxs-lookup"><span data-stu-id="6f519-174">The insert SEQL is the last operation in the SQL script generated above.</span></span>
