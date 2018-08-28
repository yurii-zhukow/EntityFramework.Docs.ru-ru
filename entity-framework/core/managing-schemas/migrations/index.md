---
title: Миграции — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 4a5d6f3798c7af7597f95cebea1aeb9e5e58d277
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996526"
---
<a name="migrations"></a><span data-ttu-id="e46e0-102">Миграции</span><span class="sxs-lookup"><span data-stu-id="e46e0-102">Migrations</span></span>
==========
<span data-ttu-id="e46e0-103">Миграции позволяет последовательно применять изменения схемы к базе данных, чтобы синхронизировать ее с моделью EF Core, сохраняя существующие в базе данные.</span><span class="sxs-lookup"><span data-stu-id="e46e0-103">Migrations provide a way to incrementally apply schema changes to the database to keep it in sync with your EF Core model while preserving existing data in the database.</span></span>

<a name="creating-the-database"></a><span data-ttu-id="e46e0-104">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="e46e0-104">Creating the database</span></span>
---------------------
<span data-ttu-id="e46e0-105">После [определения начальной модели][1] можно переходить к созданию базы данных.</span><span class="sxs-lookup"><span data-stu-id="e46e0-105">After you've [defined your initial model][1], it's time to create the database.</span></span> <span data-ttu-id="e46e0-106">Для этого нужно добавить первоначальную миграцию.</span><span class="sxs-lookup"><span data-stu-id="e46e0-106">To do this, add an initial migration.</span></span>
<span data-ttu-id="e46e0-107">Установите [инструменты EF Core][2] и выполните соответствующую команду.</span><span class="sxs-lookup"><span data-stu-id="e46e0-107">Install the [EF Core Tools][2] and run the appropriate command.</span></span>

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="e46e0-108">В каталог **Migrations** проекта добавляются три файла.</span><span class="sxs-lookup"><span data-stu-id="e46e0-108">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="e46e0-109">**00000000000000_InitialCreate.cs** — главный файл миграций.</span><span class="sxs-lookup"><span data-stu-id="e46e0-109">**00000000000000_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="e46e0-110">Содержит операции, необходимые для применения миграции (в `Up()`) и ее отмены (в `Down()`).</span><span class="sxs-lookup"><span data-stu-id="e46e0-110">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="e46e0-111">**00000000000000_InitialCreate.Designer.cs** — файл метаданных миграций.</span><span class="sxs-lookup"><span data-stu-id="e46e0-111">**00000000000000_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="e46e0-112">Содержит сведения, используемые EF.</span><span class="sxs-lookup"><span data-stu-id="e46e0-112">Contains information used by EF.</span></span>
* <span data-ttu-id="e46e0-113">**MyContextModelSnapshot.cs** — моментальный снимок текущей модели.</span><span class="sxs-lookup"><span data-stu-id="e46e0-113">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="e46e0-114">Используется для определения появившихся изменений при добавлении следующей миграции.</span><span class="sxs-lookup"><span data-stu-id="e46e0-114">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="e46e0-115">Метка времени в именах файлов позволяет расположить их в хронологическом порядке, чтобы вы могли отслеживать ход изменений.</span><span class="sxs-lookup"><span data-stu-id="e46e0-115">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="e46e0-116">Вы можете свободно перемещать файлы миграций и изменять их пространство имен.</span><span class="sxs-lookup"><span data-stu-id="e46e0-116">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="e46e0-117">Новые миграции создаются в виде элементов того же уровня, что и последняя миграция.</span><span class="sxs-lookup"><span data-stu-id="e46e0-117">New migrations are created as siblings of the last migration.</span></span>

<span data-ttu-id="e46e0-118">После этого примените миграцию к базе данных, чтобы создать схему.</span><span class="sxs-lookup"><span data-stu-id="e46e0-118">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a><span data-ttu-id="e46e0-119">Добавление другой миграции</span><span class="sxs-lookup"><span data-stu-id="e46e0-119">Adding another migration</span></span>
------------------------
<span data-ttu-id="e46e0-120">После внесения изменений в модель EF Core синхронизация схемы базы данных нарушается. Чтобы сделать ее актуальной, добавьте еще одну миграцию.</span><span class="sxs-lookup"><span data-stu-id="e46e0-120">After making changes to your EF Core model, the database schema will be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="e46e0-121">Имя миграции можно использовать как сообщение фиксации в системе управления версиями.</span><span class="sxs-lookup"><span data-stu-id="e46e0-121">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="e46e0-122">Например, если вы вносите изменения для сохранения отзывов клиентов о продуктах, можно выбрать имя *AddProductReviews*.</span><span class="sxs-lookup"><span data-stu-id="e46e0-122">For example, if I made changes to save customer reviews of products, I might choose something like *AddProductReviews*.</span></span>

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="e46e0-123">После формирования миграции нужно проверить ее точность и добавить все операции, необходимые для ее правильного применения.</span><span class="sxs-lookup"><span data-stu-id="e46e0-123">Once the migration is scaffolded, you should review it for accuracy and add any additional operations required to apply it correctly.</span></span> <span data-ttu-id="e46e0-124">Например, миграция может содержать следующие операции.</span><span class="sxs-lookup"><span data-stu-id="e46e0-124">For example, your migration might contain the following operations:</span></span>

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

<span data-ttu-id="e46e0-125">Хотя эти операции обеспечивают совместимость схемы базы данных, они не сохраняют существующие имена клиентов.</span><span class="sxs-lookup"><span data-stu-id="e46e0-125">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="e46e0-126">Чтобы улучшить миграцию, перепишите ее следующим образом.</span><span class="sxs-lookup"><span data-stu-id="e46e0-126">To make it better, rewrite it as follows.</span></span>

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

> [!TIP]
> <span data-ttu-id="e46e0-127">При добавлении новой миграции появляется предупреждение о том, что при формировании операции могут быть потеряны данные (например, удален столбец).</span><span class="sxs-lookup"><span data-stu-id="e46e0-127">Adding a new migration warns when an operation is scaffolded that may result in data loss (like dropping a column).</span></span> <span data-ttu-id="e46e0-128">Обязательно проверьте точность этих миграций.</span><span class="sxs-lookup"><span data-stu-id="e46e0-128">Be sure to especially review these migrations for accuracy.</span></span>

<span data-ttu-id="e46e0-129">Примените миграцию к базе данных с помощью соответствующей команды.</span><span class="sxs-lookup"><span data-stu-id="e46e0-129">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a><span data-ttu-id="e46e0-130">Удаление миграции</span><span class="sxs-lookup"><span data-stu-id="e46e0-130">Removing a migration</span></span>
--------------------
<span data-ttu-id="e46e0-131">Иногда, добавив миграцию, вы понимаем, что нужно внести дополнительные изменения в модель EF Core перед ее применением.</span><span class="sxs-lookup"><span data-stu-id="e46e0-131">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span>
<span data-ttu-id="e46e0-132">Для удаления последней миграции используйте приведенную ниже команду.</span><span class="sxs-lookup"><span data-stu-id="e46e0-132">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

<span data-ttu-id="e46e0-133">Удалив миграцию, вы можете внести в модель дополнительные изменения и снова добавить ее.</span><span class="sxs-lookup"><span data-stu-id="e46e0-133">After removing it, you can make the additional model changes and add it again.</span></span>

<a name="reverting-a-migration"></a><span data-ttu-id="e46e0-134">Отмена миграции</span><span class="sxs-lookup"><span data-stu-id="e46e0-134">Reverting a migration</span></span>
---------------------
<span data-ttu-id="e46e0-135">Если вы уже применили одну миграцию для базы данных или несколько и хотите отменить их, можно использовать ту же команду, что и для применения миграций, но указав имя отменяемой миграции.</span><span class="sxs-lookup"><span data-stu-id="e46e0-135">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a><span data-ttu-id="e46e0-136">Пустые миграции</span><span class="sxs-lookup"><span data-stu-id="e46e0-136">Empty migrations</span></span>
----------------
<span data-ttu-id="e46e0-137">Иногда бывает удобно добавить миграцию, не внося изменения в модель.</span><span class="sxs-lookup"><span data-stu-id="e46e0-137">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="e46e0-138">В этом случае при добавлении новой миграции создается пустая миграция.</span><span class="sxs-lookup"><span data-stu-id="e46e0-138">In this case, adding a new migration creates an empty one.</span></span> <span data-ttu-id="e46e0-139">Вы можете настроить ее для выполнения операций, которые не связаны напрямую с моделью EF Core.</span><span class="sxs-lookup"><span data-stu-id="e46e0-139">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span>
<span data-ttu-id="e46e0-140">Ниже приведен ряд вещей, которыми вам может потребоваться управлять.</span><span class="sxs-lookup"><span data-stu-id="e46e0-140">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="e46e0-141">Полнотекстовый поиск</span><span class="sxs-lookup"><span data-stu-id="e46e0-141">Full-Text Search</span></span>
* <span data-ttu-id="e46e0-142">Функции</span><span class="sxs-lookup"><span data-stu-id="e46e0-142">Functions</span></span>
* <span data-ttu-id="e46e0-143">Хранимые процедуры</span><span class="sxs-lookup"><span data-stu-id="e46e0-143">Stored procedures</span></span>
* <span data-ttu-id="e46e0-144">Триггеры</span><span class="sxs-lookup"><span data-stu-id="e46e0-144">Triggers</span></span>
* <span data-ttu-id="e46e0-145">Представления</span><span class="sxs-lookup"><span data-stu-id="e46e0-145">Views</span></span>
* <span data-ttu-id="e46e0-146">и т. д.</span><span class="sxs-lookup"><span data-stu-id="e46e0-146">etc.</span></span>

<a name="generating-a-sql-script"></a><span data-ttu-id="e46e0-147">Формирование скрипта SQL</span><span class="sxs-lookup"><span data-stu-id="e46e0-147">Generating a SQL script</span></span>
-----------------------
<span data-ttu-id="e46e0-148">При отладке миграций или их развертывании в рабочей базе данных бывает полезно создать скрипт SQL.</span><span class="sxs-lookup"><span data-stu-id="e46e0-148">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="e46e0-149">Такой скрипт можно дополнительно проверить на точность и настроить в соответствии с потребностями рабочей базы данных.</span><span class="sxs-lookup"><span data-stu-id="e46e0-149">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="e46e0-150">Кроме того, его можно использовать в сочетании с технологией развертывания.</span><span class="sxs-lookup"><span data-stu-id="e46e0-150">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="e46e0-151">Базовая команда имеет следующий вид.</span><span class="sxs-lookup"><span data-stu-id="e46e0-151">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

<span data-ttu-id="e46e0-152">Для этой команды доступно несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="e46e0-152">There are several options to this command.</span></span>

<span data-ttu-id="e46e0-153">Миграция **from** должна быть последней миграцией, применяемой к базе данных перед выполнением скрипта.</span><span class="sxs-lookup"><span data-stu-id="e46e0-153">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="e46e0-154">Если не было применено ни одной миграции, укажите `0` (это значение по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="e46e0-154">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="e46e0-155">Миграция **to** является последней миграцией, применяемой к базе данных после выполнения скрипта.</span><span class="sxs-lookup"><span data-stu-id="e46e0-155">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="e46e0-156">По умолчанию она является последней миграцией в проекте.</span><span class="sxs-lookup"><span data-stu-id="e46e0-156">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="e46e0-157">При необходимости можно создать **идемпотентный** скрипт.</span><span class="sxs-lookup"><span data-stu-id="e46e0-157">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="e46e0-158">Он применяет миграции только в том случае, если они еще не были применены к базе данных.</span><span class="sxs-lookup"><span data-stu-id="e46e0-158">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="e46e0-159">Это удобно, если точно неизвестно, какая последняя миграция была применена к базе данных, или вы развертываете несколько баз данных, каждая из которых может иметь отдельную миграцию.</span><span class="sxs-lookup"><span data-stu-id="e46e0-159">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<a name="applying-migrations-at-runtime"></a><span data-ttu-id="e46e0-160">Применение миграции во время выполнения</span><span class="sxs-lookup"><span data-stu-id="e46e0-160">Applying migrations at runtime</span></span>
------------------------------
<span data-ttu-id="e46e0-161">Некоторым приложениям может потребоваться применить миграции во время выполнения — при запуске или первом выполнении.</span><span class="sxs-lookup"><span data-stu-id="e46e0-161">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="e46e0-162">Для этого можно использовать метод `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="e46e0-162">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="e46e0-163">Обратите внимание, что такой подход не является универсальным.</span><span class="sxs-lookup"><span data-stu-id="e46e0-163">Caution, this approach isn't for everyone.</span></span> <span data-ttu-id="e46e0-164">Хотя он отлично подходит для приложений с локальной базой данных, большинству приложений требуется более надежная стратегия развертывания, такая как создание скриптов SQL.</span><span class="sxs-lookup"><span data-stu-id="e46e0-164">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> <span data-ttu-id="e46e0-165">Не вызывайте `EnsureCreated()` перед `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="e46e0-165">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="e46e0-166">`EnsureCreated()` обходит миграции, чтобы создать схему, что приводит к сбою `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="e46e0-166">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

> [!NOTE]
> <span data-ttu-id="e46e0-167">Этот метод основан на службе `IMigrator`, которую можно применять в более сложных сценариях.</span><span class="sxs-lookup"><span data-stu-id="e46e0-167">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="e46e0-168">Для доступа к нему используйте `DbContext.GetService<IMigrator>()`.</span><span class="sxs-lookup"><span data-stu-id="e46e0-168">Use `DbContext.GetService<IMigrator>()` to access it.</span></span>


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
