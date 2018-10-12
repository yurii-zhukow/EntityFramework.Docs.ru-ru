---
title: Миграции — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 5ae06a4342a556936dc44c5bf6622814eaad4733
ms.sourcegitcommit: 7a7da65404c9338e1e3df42576a13be536a6f95f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/06/2018
ms.locfileid: "48834751"
---
<a name="migrations"></a><span data-ttu-id="a4570-102">Миграции</span><span class="sxs-lookup"><span data-stu-id="a4570-102">Migrations</span></span>
==========

<span data-ttu-id="a4570-103">Модель данных в процессе разработки может измениться и перестанет соответствовать базе данных.</span><span class="sxs-lookup"><span data-stu-id="a4570-103">A data model changes during development and gets out of sync with the database.</span></span> <span data-ttu-id="a4570-104">Вы всегда можете удалить базу данных, и EF создаст для вас новую версию, в точности соответствующую модели, но такая процедура приводит к потере текущих данных.</span><span class="sxs-lookup"><span data-stu-id="a4570-104">You can drop the database and let EF create a new one that matches the model, but this procedure results in the loss of data.</span></span> <span data-ttu-id="a4570-105">Функция миграции в EF Core позволяет последовательно применять изменения схемы к базе данных, чтобы синхронизировать ее с моделью данных в приложении без потери существующих данных.</span><span class="sxs-lookup"><span data-stu-id="a4570-105">The migrations feature in EF Core provides a way to incrementally update the database schema to keep it in sync with the application's data model while preserving existing data in the database.</span></span>

<span data-ttu-id="a4570-106">Миграции включают средства командной строки и API-интерфейсы, которые помогают в решении следующих задач:</span><span class="sxs-lookup"><span data-stu-id="a4570-106">Migrations includes command-line tools and APIs that help with the following tasks:</span></span>

* <span data-ttu-id="a4570-107">[Создание миграции](#create-a-migration).</span><span class="sxs-lookup"><span data-stu-id="a4570-107">[Create a migration](#create-a-migration).</span></span> <span data-ttu-id="a4570-108">Создайте код, который обновляет базу данных для синхронизации с набором изменений модели данных.</span><span class="sxs-lookup"><span data-stu-id="a4570-108">Generate code that can update the database to sync it with a set of model changes.</span></span>
* <span data-ttu-id="a4570-109">[Обновление базы данных](#update-the-database).</span><span class="sxs-lookup"><span data-stu-id="a4570-109">[Update the database](#update-the-database).</span></span> <span data-ttu-id="a4570-110">Примените ожидающие миграции, чтобы обновить схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="a4570-110">Apply pending migrations to update the database schema.</span></span>
* <span data-ttu-id="a4570-111">[Настройка кода миграции](#customize-migration-code).</span><span class="sxs-lookup"><span data-stu-id="a4570-111">[Customize migration code](#customize-migration-code).</span></span> <span data-ttu-id="a4570-112">Иногда созданный код приходится изменять или дополнять.</span><span class="sxs-lookup"><span data-stu-id="a4570-112">Sometimes the generated code needs to be modified or supplemented.</span></span>
* <span data-ttu-id="a4570-113">[Удаление миграции](#remove-a-migration).</span><span class="sxs-lookup"><span data-stu-id="a4570-113">[Remove a migration](#remove-a-migration).</span></span> <span data-ttu-id="a4570-114">Удалите созданный код.</span><span class="sxs-lookup"><span data-stu-id="a4570-114">Delete the generated code.</span></span>
* <span data-ttu-id="a4570-115">[Отмена миграции](#revert-a-migration).</span><span class="sxs-lookup"><span data-stu-id="a4570-115">[Revert a migration](#revert-a-migration).</span></span> <span data-ttu-id="a4570-116">Отмените примененные к базе данных изменения.</span><span class="sxs-lookup"><span data-stu-id="a4570-116">Undo the database changes.</span></span>
* <span data-ttu-id="a4570-117">[Создание скриптов SQL](#generate-sql-scripts).</span><span class="sxs-lookup"><span data-stu-id="a4570-117">[Generate SQL scripts](#generate-sql-scripts).</span></span> <span data-ttu-id="a4570-118">Иногда бывает нужно создать скрипт для обновления рабочей базы данных или для устранения неполадок в коде миграции.</span><span class="sxs-lookup"><span data-stu-id="a4570-118">You might need a script to update a production database or to troubleshoot migration code.</span></span>
* <span data-ttu-id="a4570-119">[Применение миграции во время выполнения](#apply-migrations-at-runtime).</span><span class="sxs-lookup"><span data-stu-id="a4570-119">[Apply migrations at runtime](#apply-migrations-at-runtime).</span></span> <span data-ttu-id="a4570-120">Если обновления во время разработки и выполнение скриптов в вашем сценарии работы подходят плохо, вызовите метод `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="a4570-120">When design-time updates and running scripts aren't the best options, call the `Migrate()` method.</span></span>

<a name="install-the-tools"></a><span data-ttu-id="a4570-121">Установка инструментов</span><span class="sxs-lookup"><span data-stu-id="a4570-121">Install the tools</span></span>
-----------------

<span data-ttu-id="a4570-122">Установите [средства командной строки](xref:core/miscellaneous/cli/index).</span><span class="sxs-lookup"><span data-stu-id="a4570-122">Install the [command-line tools](xref:core/miscellaneous/cli/index):</span></span>
* <span data-ttu-id="a4570-123">Для Visual Studio мы рекомендуем [Инструменты консоли диспетчера пакетов](xref:core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="a4570-123">For Visual Studio, we recommend the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="a4570-124">Для других сред разработки выберите [Средства интерфейса командной строки .NET Core](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="a4570-124">For other development environments, choose the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span>

<a name="create-a-migration"></a><span data-ttu-id="a4570-125">Создание миграции</span><span class="sxs-lookup"><span data-stu-id="a4570-125">Create a migration</span></span>
------------------

<span data-ttu-id="a4570-126">После того, как вы [определите начальную модель](xref:core/modeling/index), можно переходить к созданию базы данных.</span><span class="sxs-lookup"><span data-stu-id="a4570-126">After you've [defined your initial model](xref:core/modeling/index), it's time to create the database.</span></span> <span data-ttu-id="a4570-127">Чтобы добавить первоначальную миграцию, выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="a4570-127">To add an initial migration, run the following command.</span></span>

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="a4570-128">В каталог **Migrations** проекта добавляются три файла.</span><span class="sxs-lookup"><span data-stu-id="a4570-128">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="a4570-129">**00000000000000_InitialCreate.cs** — главный файл миграций.</span><span class="sxs-lookup"><span data-stu-id="a4570-129">**00000000000000_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="a4570-130">Содержит операции, необходимые для применения миграции (в `Up()`) и ее отмены (в `Down()`).</span><span class="sxs-lookup"><span data-stu-id="a4570-130">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="a4570-131">**00000000000000_InitialCreate.Designer.cs** — файл метаданных миграций.</span><span class="sxs-lookup"><span data-stu-id="a4570-131">**00000000000000_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="a4570-132">Содержит сведения, используемые EF.</span><span class="sxs-lookup"><span data-stu-id="a4570-132">Contains information used by EF.</span></span>
* <span data-ttu-id="a4570-133">**MyContextModelSnapshot.cs** — моментальный снимок текущей модели.</span><span class="sxs-lookup"><span data-stu-id="a4570-133">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="a4570-134">Используется для определения появившихся изменений при добавлении следующей миграции.</span><span class="sxs-lookup"><span data-stu-id="a4570-134">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="a4570-135">Метка времени в именах файлов позволяет расположить их в хронологическом порядке, чтобы вы могли отслеживать ход изменений.</span><span class="sxs-lookup"><span data-stu-id="a4570-135">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="a4570-136">Вы можете свободно перемещать файлы миграций и изменять их пространство имен.</span><span class="sxs-lookup"><span data-stu-id="a4570-136">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="a4570-137">Новые миграции создаются в виде элементов того же уровня, что и последняя миграция.</span><span class="sxs-lookup"><span data-stu-id="a4570-137">New migrations are created as siblings of the last migration.</span></span>

<a name="update-the-database"></a><span data-ttu-id="a4570-138">Обновление базы данных</span><span class="sxs-lookup"><span data-stu-id="a4570-138">Update the database</span></span>
-------------------

<span data-ttu-id="a4570-139">После этого примените миграцию к базе данных, чтобы создать схему.</span><span class="sxs-lookup"><span data-stu-id="a4570-139">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="customize-migration-code"></a><span data-ttu-id="a4570-140">Настройка кода миграции</span><span class="sxs-lookup"><span data-stu-id="a4570-140">Customize migration code</span></span>
------------------------

<span data-ttu-id="a4570-141">После внесения изменений в модель EF Core может нарушиться синхронизация схемы базы данных. Чтобы сделать ее актуальной, добавьте еще одну миграцию.</span><span class="sxs-lookup"><span data-stu-id="a4570-141">After making changes to your EF Core model, the database schema might be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="a4570-142">Имя миграции можно использовать как сообщение фиксации в системе управления версиями.</span><span class="sxs-lookup"><span data-stu-id="a4570-142">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="a4570-143">Например, вы можете выбрать имя *AddProductReviews*, если суть изменений заключается в создании нового класса сущностей для обзоров.</span><span class="sxs-lookup"><span data-stu-id="a4570-143">For example, you might choose a name like *AddProductReviews* if the change is a new entity class for reviews.</span></span>

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="a4570-144">После создания кода миграции проверьте, правильно ли он создан. Если потребуется, добавьте, удалите или измените любые операции для правильного применения изменений.</span><span class="sxs-lookup"><span data-stu-id="a4570-144">Once the migration is scaffolded (code generated for it), review the code for accuracy and add, remove or modify any operations required to apply it correctly.</span></span>

<span data-ttu-id="a4570-145">Например, миграция может содержать следующие операции.</span><span class="sxs-lookup"><span data-stu-id="a4570-145">For example, a migration might contain the following operations:</span></span>

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

<span data-ttu-id="a4570-146">Хотя эти операции обеспечивают совместимость схемы базы данных, они не сохраняют существующие имена клиентов.</span><span class="sxs-lookup"><span data-stu-id="a4570-146">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="a4570-147">Чтобы улучшить миграцию, перепишите ее следующим образом.</span><span class="sxs-lookup"><span data-stu-id="a4570-147">To make it better, rewrite it as follows.</span></span>

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
> <span data-ttu-id="a4570-148">Процесс создания новой миграции предупреждает, если операция может привести к потере данных (например, содержит удаление столбца).</span><span class="sxs-lookup"><span data-stu-id="a4570-148">The migration scaffolding process warns when an operation might result in data loss (like dropping a column).</span></span> <span data-ttu-id="a4570-149">Увидев это предупреждение, проверьте точность кода миграции с особой тщательностью.</span><span class="sxs-lookup"><span data-stu-id="a4570-149">If you see that warning, be especially sure to review the migrations code for accuracy.</span></span>

<span data-ttu-id="a4570-150">Примените миграцию к базе данных с помощью соответствующей команды.</span><span class="sxs-lookup"><span data-stu-id="a4570-150">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

### <a name="empty-migrations"></a><span data-ttu-id="a4570-151">Пустые миграции</span><span class="sxs-lookup"><span data-stu-id="a4570-151">Empty migrations</span></span>

<span data-ttu-id="a4570-152">Иногда бывает удобно добавить миграцию, не внося изменения в модель.</span><span class="sxs-lookup"><span data-stu-id="a4570-152">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="a4570-153">В этом случае при добавлении новой миграции создаются файлы кода с пустыми классами.</span><span class="sxs-lookup"><span data-stu-id="a4570-153">In this case, adding a new migration creates code files with empty classes.</span></span> <span data-ttu-id="a4570-154">Вы можете настроить ее для выполнения операций, которые не связаны напрямую с моделью EF Core.</span><span class="sxs-lookup"><span data-stu-id="a4570-154">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span> <span data-ttu-id="a4570-155">Ниже приведен ряд вещей, которыми вам может потребоваться управлять.</span><span class="sxs-lookup"><span data-stu-id="a4570-155">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="a4570-156">Полнотекстовый поиск</span><span class="sxs-lookup"><span data-stu-id="a4570-156">Full-Text Search</span></span>
* <span data-ttu-id="a4570-157">Функции</span><span class="sxs-lookup"><span data-stu-id="a4570-157">Functions</span></span>
* <span data-ttu-id="a4570-158">Хранимые процедуры</span><span class="sxs-lookup"><span data-stu-id="a4570-158">Stored procedures</span></span>
* <span data-ttu-id="a4570-159">Триггеры</span><span class="sxs-lookup"><span data-stu-id="a4570-159">Triggers</span></span>
* <span data-ttu-id="a4570-160">Представления</span><span class="sxs-lookup"><span data-stu-id="a4570-160">Views</span></span>

<a name="remove-a-migration"></a><span data-ttu-id="a4570-161">Удаление миграции</span><span class="sxs-lookup"><span data-stu-id="a4570-161">Remove a migration</span></span>
------------------
<span data-ttu-id="a4570-162">Иногда, добавив миграцию, вы понимаем, что нужно внести дополнительные изменения в модель EF Core перед ее применением.</span><span class="sxs-lookup"><span data-stu-id="a4570-162">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span> <span data-ttu-id="a4570-163">Для удаления последней миграции используйте приведенную ниже команду.</span><span class="sxs-lookup"><span data-stu-id="a4570-163">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

<span data-ttu-id="a4570-164">Удалив миграцию, вы можете внести в модель дополнительные изменения и снова добавить ее.</span><span class="sxs-lookup"><span data-stu-id="a4570-164">After removing the migration, you can make the additional model changes and add it again.</span></span>

<a name="revert-a-migration"></a><span data-ttu-id="a4570-165">Отмена миграции</span><span class="sxs-lookup"><span data-stu-id="a4570-165">Revert a migration</span></span>
------------------
<span data-ttu-id="a4570-166">Если вы уже применили одну миграцию для базы данных или несколько и хотите отменить их, можно использовать ту же команду, что и для применения миграций, но указав имя миграции, к которой надо откатить изменения.</span><span class="sxs-lookup"><span data-stu-id="a4570-166">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="generate-sql-scripts"></a><span data-ttu-id="a4570-167">Создание скриптов SQL</span><span class="sxs-lookup"><span data-stu-id="a4570-167">Generate SQL scripts</span></span>
--------------------
<span data-ttu-id="a4570-168">При отладке миграций или их развертывании в рабочей базе данных бывает полезно создать скрипт SQL.</span><span class="sxs-lookup"><span data-stu-id="a4570-168">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="a4570-169">Такой скрипт можно дополнительно проверить на точность и настроить в соответствии с потребностями рабочей базы данных.</span><span class="sxs-lookup"><span data-stu-id="a4570-169">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="a4570-170">Кроме того, его можно использовать в сочетании с технологией развертывания.</span><span class="sxs-lookup"><span data-stu-id="a4570-170">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="a4570-171">Базовая команда имеет следующий вид.</span><span class="sxs-lookup"><span data-stu-id="a4570-171">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

<span data-ttu-id="a4570-172">Для этой команды доступно несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="a4570-172">There are several options to this command.</span></span>

<span data-ttu-id="a4570-173">Миграция **from** должна быть последней миграцией, применяемой к базе данных перед выполнением скрипта.</span><span class="sxs-lookup"><span data-stu-id="a4570-173">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="a4570-174">Если не было применено ни одной миграции, укажите `0` (это значение по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="a4570-174">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="a4570-175">Миграция **to** является последней миграцией, применяемой к базе данных после выполнения скрипта.</span><span class="sxs-lookup"><span data-stu-id="a4570-175">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="a4570-176">По умолчанию она является последней миграцией в проекте.</span><span class="sxs-lookup"><span data-stu-id="a4570-176">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="a4570-177">При необходимости можно создать **идемпотентный** скрипт.</span><span class="sxs-lookup"><span data-stu-id="a4570-177">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="a4570-178">Он применяет миграции только в том случае, если они еще не были применены к базе данных.</span><span class="sxs-lookup"><span data-stu-id="a4570-178">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="a4570-179">Это удобно, если точно неизвестно, какая последняя миграция была применена к базе данных, или вы развертываете несколько баз данных, каждая из которых может иметь отдельную миграцию.</span><span class="sxs-lookup"><span data-stu-id="a4570-179">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<a name="apply-migrations-at-runtime"></a><span data-ttu-id="a4570-180">Применение миграции во время выполнения</span><span class="sxs-lookup"><span data-stu-id="a4570-180">Apply migrations at runtime</span></span>
---------------------------
<span data-ttu-id="a4570-181">Некоторым приложениям может потребоваться применить миграции во время выполнения — при запуске или первом выполнении.</span><span class="sxs-lookup"><span data-stu-id="a4570-181">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="a4570-182">Для этого можно использовать метод `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="a4570-182">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="a4570-183">Этот метод основан на службе `IMigrator`, которую можно применять в более сложных сценариях.</span><span class="sxs-lookup"><span data-stu-id="a4570-183">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="a4570-184">Для доступа к нему используйте `DbContext.GetService<IMigrator>()`.</span><span class="sxs-lookup"><span data-stu-id="a4570-184">Use `DbContext.GetService<IMigrator>()` to access it.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> * <span data-ttu-id="a4570-185">Такой подход не является универсальным.</span><span class="sxs-lookup"><span data-stu-id="a4570-185">This approach isn't for everyone.</span></span> <span data-ttu-id="a4570-186">Хотя он отлично подходит для приложений с локальной базой данных, большинству приложений требуется более надежная стратегия развертывания, такая как создание скриптов SQL.</span><span class="sxs-lookup"><span data-stu-id="a4570-186">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>
> * <span data-ttu-id="a4570-187">Не вызывайте `EnsureCreated()` перед `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="a4570-187">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="a4570-188">`EnsureCreated()` обходит миграции, чтобы создать схему, что приводит к сбою `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="a4570-188">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

<a name="next-steps"></a><span data-ttu-id="a4570-189">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="a4570-189">Next steps</span></span>
----------

<span data-ttu-id="a4570-190">Дополнительные сведения см. в разделе <xref:core/miscellaneous/cli/index>.</span><span class="sxs-lookup"><span data-stu-id="a4570-190">For more information, see <xref:core/miscellaneous/cli/index>.</span></span>