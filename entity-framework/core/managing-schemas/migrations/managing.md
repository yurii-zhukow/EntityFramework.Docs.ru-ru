---
title: Управление миграцией — EF Core
description: Добавление, удаление и иным образом управление миграцией схемы базы данных с помощью Entity Framework Core
author: bricelam
ms.date: 10/27/2020
uid: core/managing-schemas/migrations/managing
ms.openlocfilehash: 81f7cec54510d95b1e2432d56ff95110224fd9bf
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94429860"
---
# <a name="managing-migrations"></a><span data-ttu-id="d404b-103">Управление миграцией</span><span class="sxs-lookup"><span data-stu-id="d404b-103">Managing Migrations</span></span>

<span data-ttu-id="d404b-104">При изменении модели миграция добавляется и удаляется в рамках обычной разработки, а файлы миграции возвращаются в систему управления версиями проекта.</span><span class="sxs-lookup"><span data-stu-id="d404b-104">As your model changes, migrations are added and removed as part of normal development, and the migration files are checked into your project's source control.</span></span> <span data-ttu-id="d404b-105">Для управления миграцией необходимо сначала установить [EF Core программ командной строки](xref:core/cli/index).</span><span class="sxs-lookup"><span data-stu-id="d404b-105">To manage migrations, you must first install the [EF Core command-line tools](xref:core/cli/index).</span></span>

> [!TIP]
> <span data-ttu-id="d404b-106">Если `DbContext` находится не в той же сборке, что и начальный проект, можно явным образом указать целевой и начальный проекты в [средствах консоли диспетчера пакетов](xref:core/cli/powershell#target-and-startup-project) или в [средствах .NET Core CLI](xref:core/cli/dotnet#target-project-and-startup-project).</span><span class="sxs-lookup"><span data-stu-id="d404b-106">If the `DbContext` is in a different assembly than the startup project, you can explicitly specify the target and startup projects in either the [Package Manager Console tools](xref:core/cli/powershell#target-and-startup-project) or the [.NET Core CLI tools](xref:core/cli/dotnet#target-project-and-startup-project).</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="d404b-107">Добавление миграции</span><span class="sxs-lookup"><span data-stu-id="d404b-107">Add a migration</span></span>

<span data-ttu-id="d404b-108">После изменения модели можно добавить миграцию для этого изменения:</span><span class="sxs-lookup"><span data-stu-id="d404b-108">After your model has been changed, you can add a migration for that change:</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="d404b-109">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="d404b-109">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add AddBlogCreatedTimestamp
```

### <a name="visual-studio"></a>[<span data-ttu-id="d404b-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d404b-110">Visual Studio</span></span>](#tab/vs)

```powershell
Add-Migration AddBlogCreatedTimestamp
```

***

<span data-ttu-id="d404b-111">Имя миграции можно использовать как сообщение фиксации в системе управления версиями.</span><span class="sxs-lookup"><span data-stu-id="d404b-111">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="d404b-112">Например, можно выбрать имя, например *аддблогкреатедтиместамп* , если изменение является новым `CreatedTimestamp` свойством `Blog` сущности.</span><span class="sxs-lookup"><span data-stu-id="d404b-112">For example, you might choose a name like *AddBlogCreatedTimestamp* if the change is a new `CreatedTimestamp` property on your `Blog` entity.</span></span>

<span data-ttu-id="d404b-113">В каталог **Migrations** проекта добавляются три файла.</span><span class="sxs-lookup"><span data-stu-id="d404b-113">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="d404b-114">**XXXXXXXXXXXXXX_AddCreatedTimestamp. CS** — основной файл миграции.</span><span class="sxs-lookup"><span data-stu-id="d404b-114">**XXXXXXXXXXXXXX_AddCreatedTimestamp.cs** --The main migrations file.</span></span> <span data-ttu-id="d404b-115">Содержит операции, необходимые для применения миграции (в `Up`) и ее отмены (в `Down`).</span><span class="sxs-lookup"><span data-stu-id="d404b-115">Contains the operations necessary to apply the migration (in `Up`) and to revert it (in `Down`).</span></span>
* <span data-ttu-id="d404b-116">**XXXXXXXXXXXXXX_AddCreatedTimestamp. Designer. CS** — файл метаданных миграции.</span><span class="sxs-lookup"><span data-stu-id="d404b-116">**XXXXXXXXXXXXXX_AddCreatedTimestamp.Designer.cs** --The migrations metadata file.</span></span> <span data-ttu-id="d404b-117">Содержит сведения, используемые EF.</span><span class="sxs-lookup"><span data-stu-id="d404b-117">Contains information used by EF.</span></span>
* <span data-ttu-id="d404b-118">**MyContextModelSnapshot.cs**  — моментальный снимок текущей модели.</span><span class="sxs-lookup"><span data-stu-id="d404b-118">**MyContextModelSnapshot.cs** --A snapshot of your current model.</span></span> <span data-ttu-id="d404b-119">Используется для определения появившихся изменений при добавлении следующей миграции.</span><span class="sxs-lookup"><span data-stu-id="d404b-119">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="d404b-120">Метка времени в именах файлов позволяет расположить их в хронологическом порядке, чтобы вы могли отслеживать ход изменений.</span><span class="sxs-lookup"><span data-stu-id="d404b-120">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

### <a name="namespaces"></a><span data-ttu-id="d404b-121">Пространства имен</span><span class="sxs-lookup"><span data-stu-id="d404b-121">Namespaces</span></span>

<span data-ttu-id="d404b-122">Вы можете свободно перемещать файлы миграций и изменять их пространство имен вручную.</span><span class="sxs-lookup"><span data-stu-id="d404b-122">You are free to move Migrations files and change their namespace manually.</span></span> <span data-ttu-id="d404b-123">Новые миграции создаются в виде элементов того же уровня, что и последняя миграция.</span><span class="sxs-lookup"><span data-stu-id="d404b-123">New migrations are created as siblings of the last migration.</span></span> <span data-ttu-id="d404b-124">Кроме того, можно указать каталог во время создания следующим образом:</span><span class="sxs-lookup"><span data-stu-id="d404b-124">Alternatively, you can specify the directory at generation time as follows:</span></span>

#### <a name="net-core-cli"></a>[<span data-ttu-id="d404b-125">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="d404b-125">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate --output-dir Your/Directory
```

> [!NOTE]
> <span data-ttu-id="d404b-126">В EF Core 5,0 можно также изменить пространство имен независимо от каталога с помощью `--namespace` .</span><span class="sxs-lookup"><span data-stu-id="d404b-126">In EF Core 5.0, you can also change the namespace independently of the directory using `--namespace`.</span></span>

#### <a name="visual-studio"></a>[<span data-ttu-id="d404b-127">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d404b-127">Visual Studio</span></span>](#tab/vs)

```powershell
Add-Migration InitialCreate -OutputDir Your\Directory
```

> [!NOTE]
> <span data-ttu-id="d404b-128">В EF Core 5,0 можно также изменить пространство имен независимо от каталога с помощью `-Namespace` .</span><span class="sxs-lookup"><span data-stu-id="d404b-128">In EF Core 5.0, you can also change the namespace independently of the directory using `-Namespace`.</span></span>

<span data-ttu-id="d404b-129">\*\*_</span><span class="sxs-lookup"><span data-stu-id="d404b-129">\*\*_</span></span>

## <a name="customize-migration-code"></a><span data-ttu-id="d404b-130">Настройка кода миграции</span><span class="sxs-lookup"><span data-stu-id="d404b-130">Customize migration code</span></span>

<span data-ttu-id="d404b-131">Хотя EF Core обычно создает точную миграцию, следует всегда проверять код и убедиться, что он соответствует требуемому изменению. в некоторых случаях это даже необходимо сделать.</span><span class="sxs-lookup"><span data-stu-id="d404b-131">While EF Core generally creates accurate migrations, you should always review the code and make sure it corresponds to the desired change; in some cases, it is even necessary to do so.</span></span>

### <a name="column-renames"></a><span data-ttu-id="d404b-132">Переименование столбцов</span><span class="sxs-lookup"><span data-stu-id="d404b-132">Column renames</span></span>

<span data-ttu-id="d404b-133">Одним из важных примеров, где требуется настройка миграции, является переименование свойства.</span><span class="sxs-lookup"><span data-stu-id="d404b-133">One notable example where customizing migrations is required is when renaming a property.</span></span> <span data-ttu-id="d404b-134">Например, если переименовать свойство из `Name` в `FullName` , EF Core создаст следующую миграцию:</span><span class="sxs-lookup"><span data-stu-id="d404b-134">For example, if you rename a property from `Name` to `FullName`, EF Core will generate the following migration:</span></span>

```csharp
migrationBuilder.DropColumn(
    name: "Name",
    table: "Customers");

migrationBuilder.AddColumn<string>(
    name: "FullName",
    table: "Customers",
    nullable: true);
```

<span data-ttu-id="d404b-135">EF Core, как правило, не может быть уверенным в том, что нужно удалить столбец и создать новый (два отдельных изменения) и когда столбец должен быть переименован.</span><span class="sxs-lookup"><span data-stu-id="d404b-135">EF Core is generally unable to know when the intention is to drop a column and create a new one (two separate changes), and when a column should be renamed.</span></span> <span data-ttu-id="d404b-136">Если приведенная выше миграция применяется "как есть", все имена клиентов будут потеряны.</span><span class="sxs-lookup"><span data-stu-id="d404b-136">If the above migration is applied as-is, all your customer names will be lost.</span></span> <span data-ttu-id="d404b-137">Чтобы переименовать столбец, замените созданную выше миграцию следующей:</span><span class="sxs-lookup"><span data-stu-id="d404b-137">To rename a column, replace the above generated migration with the following:</span></span>

```csharp
migrationBuilder.RenameColumn(
    name: "Name",
    table: "Customers",
    newName: "FullName");
```

> [!TIP]
> <span data-ttu-id="d404b-138">Процесс создания новой миграции предупреждает, если операция может привести к потере данных (например, содержит удаление столбца).</span><span class="sxs-lookup"><span data-stu-id="d404b-138">The migration scaffolding process warns when an operation might result in data loss (like dropping a column).</span></span> <span data-ttu-id="d404b-139">Увидев это предупреждение, проверьте точность кода миграции с особой тщательностью.</span><span class="sxs-lookup"><span data-stu-id="d404b-139">If you see that warning, be especially sure to review the migrations code for accuracy.</span></span>

### <a name="adding-raw-sql"></a><span data-ttu-id="d404b-140">Добавление необработанного SQL</span><span class="sxs-lookup"><span data-stu-id="d404b-140">Adding raw SQL</span></span>

<span data-ttu-id="d404b-141">В то время как переименование столбца может быть выполнено через встроенный API, во многих случаях это невозможно.</span><span class="sxs-lookup"><span data-stu-id="d404b-141">While renaming a column can be achieved via a built-in API, in many cases that is not possible.</span></span> <span data-ttu-id="d404b-142">Например, может потребоваться заменить существующие `FirstName` `LastName` Свойства и одним новым `FullName` свойством.</span><span class="sxs-lookup"><span data-stu-id="d404b-142">For example, we may want to replace existing `FirstName` and `LastName` properties with a single, new `FullName` property.</span></span> <span data-ttu-id="d404b-143">Процесс миграции, создаваемый EF Core, будет следующим:</span><span class="sxs-lookup"><span data-stu-id="d404b-143">The migration generated by EF Core will be the following:</span></span>

```csharp
migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");

migrationBuilder.AddColumn<string>(
    name: "FullName",
    table: "Customer",
    nullable: true);
```

<span data-ttu-id="d404b-144">Как и раньше, это вызовет нежелательные потери данных.</span><span class="sxs-lookup"><span data-stu-id="d404b-144">As before, this would cause unwanted data loss.</span></span> <span data-ttu-id="d404b-145">Чтобы перенести данные из старых столбцов, мы переупорядочиваем миграцию и представляем необработанную операцию SQL следующим образом:</span><span class="sxs-lookup"><span data-stu-id="d404b-145">To transfer the data from the old columns, we rearrange the migrations and introduce a raw SQL operation as follows:</span></span>

```csharp
migrationBuilder.AddColumn<string>(
    name: "FullName",
    table: "Customer",
    nullable: true);

migrationBuilder.Sql(
@"
    UPDATE Customer
    SET FullName = FirstName + ' ' + LastName;
");

migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");
```

### <a name="arbitrary-changes-via-raw-sql"></a><span data-ttu-id="d404b-146">Произвольные изменения через необработанный SQL</span><span class="sxs-lookup"><span data-stu-id="d404b-146">Arbitrary changes via raw SQL</span></span>

<span data-ttu-id="d404b-147">Необработанный SQL можно также использовать для управления объектами базы данных, которые EF Core не знают.</span><span class="sxs-lookup"><span data-stu-id="d404b-147">Raw SQL can also be used to manage database objects that EF Core isn't aware of.</span></span> <span data-ttu-id="d404b-148">Для этого добавьте миграцию, не внося изменения в модель. будет создана пустая миграция, которая затем может быть заполнена необработанными операциями SQL.</span><span class="sxs-lookup"><span data-stu-id="d404b-148">To do this, add a migration without making any model change; an empty migration will be generated, which you can then populate with raw SQL operations.</span></span>

<span data-ttu-id="d404b-149">Например, следующая миграция создает SQL Server хранимую процедуру:</span><span class="sxs-lookup"><span data-stu-id="d404b-149">For example, the following migration creates a SQL Server stored procedure:</span></span>

```csharp
migrationBuilder.Sql(
@"
    EXEC ('CREATE PROCEDURE getFullName
        @LastName nvarchar(50),
        @FirstName nvarchar(50)
    AS
        RETURN @LastName + @FirstName;')");
```

> [!TIP]
> <span data-ttu-id="d404b-150">`EXEC` используется, если инструкция должна быть первой или единственной в пакете SQL.</span><span class="sxs-lookup"><span data-stu-id="d404b-150">`EXEC` is used when a statement must be the first or only one in a SQL batch.</span></span> <span data-ttu-id="d404b-151">Его также можно использовать для обхода ошибок синтаксического анализатора в сценариях миграции идемпотентными, которые могут возникать, если в таблице не существует столбцов, на которые имеются ссылки.</span><span class="sxs-lookup"><span data-stu-id="d404b-151">It can also be used to work around parser errors in idempotent migration scripts that can occur when referenced columns don't currently exist on a table.</span></span>

<span data-ttu-id="d404b-152">Это можно использовать для управления любым аспектом базы данных, в том числе:</span><span class="sxs-lookup"><span data-stu-id="d404b-152">This can be used to manage any aspect of your database, including:</span></span>

<span data-ttu-id="d404b-153">_ Хранимые процедуры</span><span class="sxs-lookup"><span data-stu-id="d404b-153">_ Stored procedures</span></span>
* <span data-ttu-id="d404b-154">Полнотекстовый поиск</span><span class="sxs-lookup"><span data-stu-id="d404b-154">Full-Text Search</span></span>
* <span data-ttu-id="d404b-155">Функции</span><span class="sxs-lookup"><span data-stu-id="d404b-155">Functions</span></span>
* <span data-ttu-id="d404b-156">Триггеры</span><span class="sxs-lookup"><span data-stu-id="d404b-156">Triggers</span></span>
* <span data-ttu-id="d404b-157">Представления</span><span class="sxs-lookup"><span data-stu-id="d404b-157">Views</span></span>

<span data-ttu-id="d404b-158">В большинстве случаев EF Core будет автоматически переносить каждую миграцию в собственную транзакцию при применении миграции.</span><span class="sxs-lookup"><span data-stu-id="d404b-158">In most cases, EF Core will automatically wrap each migration in its own transaction when applying migrations.</span></span> <span data-ttu-id="d404b-159">К сожалению, некоторые операции миграции не могут быть выполнены внутри транзакции в некоторых базах данных. в таких случаях вы можете отказаться от транзакции, передав ее `suppressTransaction: true` `migrationBuilder.Sql` .</span><span class="sxs-lookup"><span data-stu-id="d404b-159">Unfortunately, some migrations operations cannot be performed within a transaction in some databases; for these cases, you may opt out of the transaction by passing `suppressTransaction: true` to `migrationBuilder.Sql`.</span></span>

<span data-ttu-id="d404b-160">Если `DbContext` находится не в той же сборке, что и начальный проект, можно явным образом указать целевой и начальный проекты в [средствах консоли диспетчера пакетов](xref:core/cli/powershell#target-and-startup-project) или в [средствах .NET Core CLI](xref:core/cli/dotnet#target-project-and-startup-project).</span><span class="sxs-lookup"><span data-stu-id="d404b-160">If the `DbContext` is in a different assembly than the startup project, you can explicitly specify the target and startup projects in either the [Package Manager Console tools](xref:core/cli/powershell#target-and-startup-project) or the [.NET Core CLI tools](xref:core/cli/dotnet#target-project-and-startup-project).</span></span>

## <a name="remove-a-migration"></a><span data-ttu-id="d404b-161">Удаление миграции</span><span class="sxs-lookup"><span data-stu-id="d404b-161">Remove a migration</span></span>

<span data-ttu-id="d404b-162">Иногда, добавив миграцию, вы понимаем, что нужно внести дополнительные изменения в модель EF Core перед ее применением.</span><span class="sxs-lookup"><span data-stu-id="d404b-162">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span> <span data-ttu-id="d404b-163">Для удаления последней миграции используйте приведенную ниже команду.</span><span class="sxs-lookup"><span data-stu-id="d404b-163">To remove the last migration, use this command.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="d404b-164">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="d404b-164">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations remove
```

### <a name="visual-studio"></a>[<span data-ttu-id="d404b-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d404b-165">Visual Studio</span></span>](#tab/vs)

```powershell
Remove-Migration
```

***

<span data-ttu-id="d404b-166">Удалив миграцию, вы можете внести в модель дополнительные изменения и снова добавить ее.</span><span class="sxs-lookup"><span data-stu-id="d404b-166">After removing the migration, you can make the additional model changes and add it again.</span></span>

> [!WARNING]
> <span data-ttu-id="d404b-167">Не следует удалять все миграции, которые уже применены к рабочим базам данных.</span><span class="sxs-lookup"><span data-stu-id="d404b-167">Take care not to remove any migrations which are already applied to production databases.</span></span> <span data-ttu-id="d404b-168">Если этого не сделать, вы не сможете отменить изменения и может привести к нарушению предположений, сделанных последующими миграциями.</span><span class="sxs-lookup"><span data-stu-id="d404b-168">Not doing so will prevent you from being able to revert it, and may break the assumptions made by subsequent migrations.</span></span>

## <a name="listing-migrations"></a><span data-ttu-id="d404b-169">Перечисление миграций</span><span class="sxs-lookup"><span data-stu-id="d404b-169">Listing migrations</span></span>

<span data-ttu-id="d404b-170">Список всех существующих миграций можно просмотреть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="d404b-170">You can list all existing migrations as follows:</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="d404b-171">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="d404b-171">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations list
```

### <a name="visual-studio"></a>[<span data-ttu-id="d404b-172">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d404b-172">Visual Studio</span></span>](#tab/vs)

> [!NOTE]
> <span data-ttu-id="d404b-173">Эта команда была добавлена в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="d404b-173">This command was added in EF Core 5.0.</span></span>

```powershell
Get-Migration
```

<span data-ttu-id="d404b-174">\*\*_</span><span class="sxs-lookup"><span data-stu-id="d404b-174">\*\*_</span></span>

## <a name="resetting-all-migrations"></a><span data-ttu-id="d404b-175">Сброс всех миграций</span><span class="sxs-lookup"><span data-stu-id="d404b-175">Resetting all migrations</span></span>

<span data-ttu-id="d404b-176">В некоторых крайних случаях может потребоваться удалить все миграции и начать заново.</span><span class="sxs-lookup"><span data-stu-id="d404b-176">In some extreme cases, it may be necessary to remove all migrations and start over.</span></span> <span data-ttu-id="d404b-177">Это можно легко сделать, удалив папку _ *migrations* \* и удалив базу данных. на этом этапе можно создать новую начальную миграцию, которая будет содержать всю текущую схему.</span><span class="sxs-lookup"><span data-stu-id="d404b-177">This can be easily done by deleting your _ *Migrations* \* folder and dropping your database; at that point you can create a new initial migration, which will contain you entire current schema.</span></span>

<span data-ttu-id="d404b-178">Кроме того, можно сбросить все миграции и создать одну из них без потери данных.</span><span class="sxs-lookup"><span data-stu-id="d404b-178">It's also possible to reset all migrations and create a single one without losing your data.</span></span> <span data-ttu-id="d404b-179">Это иногда называется "использование параметра squash" и требует выполнения некоторых действий вручную:</span><span class="sxs-lookup"><span data-stu-id="d404b-179">This is sometimes called "squashing", and involves some manual work:</span></span>

* <span data-ttu-id="d404b-180">Удаление папки **миграций**</span><span class="sxs-lookup"><span data-stu-id="d404b-180">Delete your **Migrations** folder</span></span>
* <span data-ttu-id="d404b-181">Создать новую миграцию и создать для нее скрипт SQL</span><span class="sxs-lookup"><span data-stu-id="d404b-181">Create a new migration and generate a SQL script for it</span></span>
* <span data-ttu-id="d404b-182">В базе данных удалите все строки из таблицы журнала миграции.</span><span class="sxs-lookup"><span data-stu-id="d404b-182">In your database, delete all rows from the migrations history table</span></span>
* <span data-ttu-id="d404b-183">Вставка одной строки в журнал миграции для записи того, что первая миграция уже применена, так как таблицы уже есть.</span><span class="sxs-lookup"><span data-stu-id="d404b-183">Insert a single row into the migrations history, to record that the first migration has already been applied, since your tables are already there.</span></span> <span data-ttu-id="d404b-184">Инструкция INSERT SQL — это последняя операция в скрипте SQL, созданном выше.</span><span class="sxs-lookup"><span data-stu-id="d404b-184">The insert SQL is the last operation in the SQL script generated above.</span></span>

> [!WARNING]
> <span data-ttu-id="d404b-185">При удалении папки **миграции** любой [Пользовательский код миграции](#customize-migration-code) будет потерян.</span><span class="sxs-lookup"><span data-stu-id="d404b-185">Any [custom migration code](#customize-migration-code) will be lost when the **Migrations** folder is deleted.</span></span>  <span data-ttu-id="d404b-186">Все настройки должны быть применены к новой первоначальной миграции вручную, чтобы сохранить их.</span><span class="sxs-lookup"><span data-stu-id="d404b-186">Any customizations must be applied to the new initial migration manually in order to be preserved.</span></span>
