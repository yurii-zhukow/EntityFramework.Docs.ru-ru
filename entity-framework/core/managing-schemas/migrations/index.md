---
title: Миграции — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 99bb420d95cb86443b63ba05ce9e6b4ab838eff9
ms.sourcegitcommit: 79e460f76b6664e1da5886d102bd97f651d2ffff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82538457"
---
# <a name="migrations"></a><span data-ttu-id="5b668-102">Миграции</span><span class="sxs-lookup"><span data-stu-id="5b668-102">Migrations</span></span>

<span data-ttu-id="5b668-103">Модель данных в процессе разработки может измениться и перестанет соответствовать базе данных.</span><span class="sxs-lookup"><span data-stu-id="5b668-103">A data model changes during development and gets out of sync with the database.</span></span> <span data-ttu-id="5b668-104">Вы всегда можете удалить базу данных, и EF создаст для вас новую версию, в точности соответствующую модели, но такая процедура приводит к потере текущих данных.</span><span class="sxs-lookup"><span data-stu-id="5b668-104">You can drop the database and let EF create a new one that matches the model, but this procedure results in the loss of data.</span></span> <span data-ttu-id="5b668-105">Функция миграции в EF Core позволяет последовательно применять изменения схемы к базе данных, чтобы синхронизировать ее с моделью данных в приложении без потери существующих данных.</span><span class="sxs-lookup"><span data-stu-id="5b668-105">The migrations feature in EF Core provides a way to incrementally update the database schema to keep it in sync with the application's data model while preserving existing data in the database.</span></span>

<span data-ttu-id="5b668-106">Миграции включают средства командной строки и API-интерфейсы, которые помогают в решении следующих задач:</span><span class="sxs-lookup"><span data-stu-id="5b668-106">Migrations includes command-line tools and APIs that help with the following tasks:</span></span>

* <span data-ttu-id="5b668-107">[Создание миграции](#create-a-migration).</span><span class="sxs-lookup"><span data-stu-id="5b668-107">[Create a migration](#create-a-migration).</span></span> <span data-ttu-id="5b668-108">Создайте код, который обновляет базу данных для синхронизации с набором изменений модели данных.</span><span class="sxs-lookup"><span data-stu-id="5b668-108">Generate code that can update the database to sync it with a set of model changes.</span></span>
* <span data-ttu-id="5b668-109">[Обновление базы данных](#update-the-database).</span><span class="sxs-lookup"><span data-stu-id="5b668-109">[Update the database](#update-the-database).</span></span> <span data-ttu-id="5b668-110">Примените ожидающие миграции, чтобы обновить схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="5b668-110">Apply pending migrations to update the database schema.</span></span>
* <span data-ttu-id="5b668-111">[Настройка кода миграции](#customize-migration-code).</span><span class="sxs-lookup"><span data-stu-id="5b668-111">[Customize migration code](#customize-migration-code).</span></span> <span data-ttu-id="5b668-112">Иногда созданный код приходится изменять или дополнять.</span><span class="sxs-lookup"><span data-stu-id="5b668-112">Sometimes the generated code needs to be modified or supplemented.</span></span>
* <span data-ttu-id="5b668-113">[Удаление миграции](#remove-a-migration).</span><span class="sxs-lookup"><span data-stu-id="5b668-113">[Remove a migration](#remove-a-migration).</span></span> <span data-ttu-id="5b668-114">Удалите созданный код.</span><span class="sxs-lookup"><span data-stu-id="5b668-114">Delete the generated code.</span></span>
* <span data-ttu-id="5b668-115">[Отмена миграции](#revert-a-migration).</span><span class="sxs-lookup"><span data-stu-id="5b668-115">[Revert a migration](#revert-a-migration).</span></span> <span data-ttu-id="5b668-116">Отмените примененные к базе данных изменения.</span><span class="sxs-lookup"><span data-stu-id="5b668-116">Undo the database changes.</span></span>
* <span data-ttu-id="5b668-117">[Создание скриптов SQL](#generate-sql-scripts).</span><span class="sxs-lookup"><span data-stu-id="5b668-117">[Generate SQL scripts](#generate-sql-scripts).</span></span> <span data-ttu-id="5b668-118">Иногда бывает нужно создать скрипт для обновления рабочей базы данных или для устранения неполадок в коде миграции.</span><span class="sxs-lookup"><span data-stu-id="5b668-118">You might need a script to update a production database or to troubleshoot migration code.</span></span>
* <span data-ttu-id="5b668-119">[Применение миграции во время выполнения](#apply-migrations-at-runtime).</span><span class="sxs-lookup"><span data-stu-id="5b668-119">[Apply migrations at runtime](#apply-migrations-at-runtime).</span></span> <span data-ttu-id="5b668-120">Если обновления во время разработки и выполнение скриптов в вашем сценарии работы подходят плохо, вызовите метод `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="5b668-120">When design-time updates and running scripts aren't the best options, call the `Migrate()` method.</span></span>

> [!TIP]
> <span data-ttu-id="5b668-121">Если `DbContext` находится не в той же сборке, что и начальный проект, можно явным образом указать целевой и начальный проекты в [средствах консоли диспетчера пакетов](xref:core/miscellaneous/cli/powershell#target-and-startup-project) или в [средствах .NET Core CLI](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).</span><span class="sxs-lookup"><span data-stu-id="5b668-121">If the `DbContext` is in a different assembly than the startup project, you can explicitly specify the target and startup projects in either the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell#target-and-startup-project) or the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).</span></span>

## <a name="install-the-tools"></a><span data-ttu-id="5b668-122">Установка инструментов</span><span class="sxs-lookup"><span data-stu-id="5b668-122">Install the tools</span></span>

<span data-ttu-id="5b668-123">Установите [средства командной строки](xref:core/miscellaneous/cli/index).</span><span class="sxs-lookup"><span data-stu-id="5b668-123">Install the [command-line tools](xref:core/miscellaneous/cli/index):</span></span>

* <span data-ttu-id="5b668-124">Для Visual Studio мы рекомендуем [Инструменты консоли диспетчера пакетов](xref:core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="5b668-124">For Visual Studio, we recommend the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="5b668-125">Для других сред разработки выберите [Средства интерфейса командной строки .NET Core](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="5b668-125">For other development environments, choose the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span>

## <a name="create-a-migration"></a><span data-ttu-id="5b668-126">Создание миграции</span><span class="sxs-lookup"><span data-stu-id="5b668-126">Create a migration</span></span>

<span data-ttu-id="5b668-127">После того, как вы [определите начальную модель](xref:core/modeling/index), можно переходить к созданию базы данных.</span><span class="sxs-lookup"><span data-stu-id="5b668-127">After you've [defined your initial model](xref:core/modeling/index), it's time to create the database.</span></span> <span data-ttu-id="5b668-128">Чтобы добавить первоначальную миграцию, выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="5b668-128">To add an initial migration, run the following command.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="5b668-129">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="5b668-129">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate
```

### <a name="visual-studio"></a>[<span data-ttu-id="5b668-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b668-130">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration InitialCreate
```

***

<span data-ttu-id="5b668-131">В каталог **Migrations** проекта добавляются три файла.</span><span class="sxs-lookup"><span data-stu-id="5b668-131">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="5b668-132">**XXXXXXXXXXXXXX_InitialCreate.cs** — главный файл миграций.</span><span class="sxs-lookup"><span data-stu-id="5b668-132">**XXXXXXXXXXXXXX_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="5b668-133">Содержит операции, необходимые для применения миграции (в `Up()`) и ее отмены (в `Down()`).</span><span class="sxs-lookup"><span data-stu-id="5b668-133">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="5b668-134">**XXXXXXXXXXXXXX_InitialCreate.Designer.cs** — файл метаданных миграций.</span><span class="sxs-lookup"><span data-stu-id="5b668-134">**XXXXXXXXXXXXXX_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="5b668-135">Содержит сведения, используемые EF.</span><span class="sxs-lookup"><span data-stu-id="5b668-135">Contains information used by EF.</span></span>
* <span data-ttu-id="5b668-136">**MyContextModelSnapshot.cs** — моментальный снимок текущей модели.</span><span class="sxs-lookup"><span data-stu-id="5b668-136">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="5b668-137">Используется для определения появившихся изменений при добавлении следующей миграции.</span><span class="sxs-lookup"><span data-stu-id="5b668-137">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="5b668-138">Метка времени в именах файлов позволяет расположить их в хронологическом порядке, чтобы вы могли отслеживать ход изменений.</span><span class="sxs-lookup"><span data-stu-id="5b668-138">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="5b668-139">Вы можете свободно перемещать файлы миграций и изменять их пространство имен вручную.</span><span class="sxs-lookup"><span data-stu-id="5b668-139">You are free to move Migrations files and change their namespace manually.</span></span> <span data-ttu-id="5b668-140">Новые миграции создаются в виде элементов того же уровня, что и последняя миграция.</span><span class="sxs-lookup"><span data-stu-id="5b668-140">New migrations are created as siblings of the last migration.</span></span>
> 
> <span data-ttu-id="5b668-141">Кроме того, можно использовать параметр `-Namespace` (консоль диспетчера пакетов) или `--namespace` (.NET Core CLI), чтобы указать пространство имен во время создания.</span><span class="sxs-lookup"><span data-stu-id="5b668-141">Alternatively you can use `-Namespace` (Package Manager Console) or `--namespace` (.NET Core CLI) to specify the namespace at generation time.</span></span>
> ### <a name="net-core-cli"></a>[<span data-ttu-id="5b668-142">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="5b668-142">.NET Core CLI</span></span>](#tab/dotnet-core-cli)
> 
> ```dotnetcli
> dotnet ef migrations add InitialCreate --namespace Your.Namespace
> ```
> 
> ### <a name="visual-studio"></a>[<span data-ttu-id="5b668-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b668-143">Visual Studio</span></span>](#tab/vs)
> 
> ``` powershell
> Add-Migration InitialCreate -Namespace Your.Namespace
> ```

## <a name="update-the-database"></a><span data-ttu-id="5b668-144">Обновление базы данных</span><span class="sxs-lookup"><span data-stu-id="5b668-144">Update the database</span></span>

<span data-ttu-id="5b668-145">После этого примените миграцию к базе данных, чтобы создать схему.</span><span class="sxs-lookup"><span data-stu-id="5b668-145">Next, apply the migration to the database to create the schema.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="5b668-146">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="5b668-146">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studio"></a>[<span data-ttu-id="5b668-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b668-147">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database
```

***

## <a name="customize-migration-code"></a><span data-ttu-id="5b668-148">Настройка кода миграции</span><span class="sxs-lookup"><span data-stu-id="5b668-148">Customize migration code</span></span>

<span data-ttu-id="5b668-149">После внесения изменений в модель EF Core может нарушиться синхронизация схемы базы данных. Чтобы сделать ее актуальной, добавьте еще одну миграцию.</span><span class="sxs-lookup"><span data-stu-id="5b668-149">After making changes to your EF Core model, the database schema might be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="5b668-150">Имя миграции можно использовать как сообщение фиксации в системе управления версиями.</span><span class="sxs-lookup"><span data-stu-id="5b668-150">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="5b668-151">Например, вы можете выбрать имя *AddProductReviews*, если суть изменений заключается в создании нового класса сущностей для обзоров.</span><span class="sxs-lookup"><span data-stu-id="5b668-151">For example, you might choose a name like *AddProductReviews* if the change is a new entity class for reviews.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="5b668-152">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="5b668-152">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add AddProductReviews
```

### <a name="visual-studio"></a>[<span data-ttu-id="5b668-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b668-153">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration AddProductReviews
```

***

<span data-ttu-id="5b668-154">После создания кода миграции проверьте, правильно ли он создан. Если потребуется, добавьте, удалите или измените любые операции для правильного применения изменений.</span><span class="sxs-lookup"><span data-stu-id="5b668-154">Once the migration is scaffolded (code generated for it), review the code for accuracy and add, remove or modify any operations required to apply it correctly.</span></span>

<span data-ttu-id="5b668-155">Например, миграция может содержать следующие операции.</span><span class="sxs-lookup"><span data-stu-id="5b668-155">For example, a migration might contain the following operations:</span></span>

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

<span data-ttu-id="5b668-156">Хотя эти операции обеспечивают совместимость схемы базы данных, они не сохраняют существующие имена клиентов.</span><span class="sxs-lookup"><span data-stu-id="5b668-156">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="5b668-157">Чтобы улучшить миграцию, перепишите ее следующим образом.</span><span class="sxs-lookup"><span data-stu-id="5b668-157">To make it better, rewrite it as follows.</span></span>

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
> <span data-ttu-id="5b668-158">Процесс создания новой миграции предупреждает, если операция может привести к потере данных (например, содержит удаление столбца).</span><span class="sxs-lookup"><span data-stu-id="5b668-158">The migration scaffolding process warns when an operation might result in data loss (like dropping a column).</span></span> <span data-ttu-id="5b668-159">Увидев это предупреждение, проверьте точность кода миграции с особой тщательностью.</span><span class="sxs-lookup"><span data-stu-id="5b668-159">If you see that warning, be especially sure to review the migrations code for accuracy.</span></span>

<span data-ttu-id="5b668-160">Примените миграцию к базе данных с помощью соответствующей команды.</span><span class="sxs-lookup"><span data-stu-id="5b668-160">Apply the migration to the database using the appropriate command.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="5b668-161">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="5b668-161">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studio"></a>[<span data-ttu-id="5b668-162">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b668-162">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database
```

***

### <a name="empty-migrations"></a><span data-ttu-id="5b668-163">Пустые миграции</span><span class="sxs-lookup"><span data-stu-id="5b668-163">Empty migrations</span></span>

<span data-ttu-id="5b668-164">Иногда бывает удобно добавить миграцию, не внося изменения в модель.</span><span class="sxs-lookup"><span data-stu-id="5b668-164">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="5b668-165">В этом случае при добавлении новой миграции создаются файлы кода с пустыми классами.</span><span class="sxs-lookup"><span data-stu-id="5b668-165">In this case, adding a new migration creates code files with empty classes.</span></span> <span data-ttu-id="5b668-166">Вы можете настроить ее для выполнения операций, которые не связаны напрямую с моделью EF Core.</span><span class="sxs-lookup"><span data-stu-id="5b668-166">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span> <span data-ttu-id="5b668-167">Ниже приведен ряд вещей, которыми вам может потребоваться управлять.</span><span class="sxs-lookup"><span data-stu-id="5b668-167">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="5b668-168">Полнотекстовый поиск</span><span class="sxs-lookup"><span data-stu-id="5b668-168">Full-Text Search</span></span>
* <span data-ttu-id="5b668-169">Функции</span><span class="sxs-lookup"><span data-stu-id="5b668-169">Functions</span></span>
* <span data-ttu-id="5b668-170">Хранимые процедуры</span><span class="sxs-lookup"><span data-stu-id="5b668-170">Stored procedures</span></span>
* <span data-ttu-id="5b668-171">Триггеры</span><span class="sxs-lookup"><span data-stu-id="5b668-171">Triggers</span></span>
* <span data-ttu-id="5b668-172">Представления</span><span class="sxs-lookup"><span data-stu-id="5b668-172">Views</span></span>

## <a name="remove-a-migration"></a><span data-ttu-id="5b668-173">Удаление миграции</span><span class="sxs-lookup"><span data-stu-id="5b668-173">Remove a migration</span></span>

<span data-ttu-id="5b668-174">Иногда, добавив миграцию, вы понимаем, что нужно внести дополнительные изменения в модель EF Core перед ее применением.</span><span class="sxs-lookup"><span data-stu-id="5b668-174">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span> <span data-ttu-id="5b668-175">Для удаления последней миграции используйте приведенную ниже команду.</span><span class="sxs-lookup"><span data-stu-id="5b668-175">To remove the last migration, use this command.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="5b668-176">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="5b668-176">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations remove
```

### <a name="visual-studio"></a>[<span data-ttu-id="5b668-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b668-177">Visual Studio</span></span>](#tab/vs)

``` powershell
Remove-Migration
```

***

<span data-ttu-id="5b668-178">Удалив миграцию, вы можете внести в модель дополнительные изменения и снова добавить ее.</span><span class="sxs-lookup"><span data-stu-id="5b668-178">After removing the migration, you can make the additional model changes and add it again.</span></span>

## <a name="revert-a-migration"></a><span data-ttu-id="5b668-179">Отмена миграции</span><span class="sxs-lookup"><span data-stu-id="5b668-179">Revert a migration</span></span>

<span data-ttu-id="5b668-180">Если вы уже применили одну миграцию для базы данных или несколько и хотите отменить их, можно использовать ту же команду, что и для применения миграций, но указав имя миграции, к которой надо откатить изменения.</span><span class="sxs-lookup"><span data-stu-id="5b668-180">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="5b668-181">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="5b668-181">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update LastGoodMigration
```

### <a name="visual-studio"></a>[<span data-ttu-id="5b668-182">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b668-182">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database LastGoodMigration
```

***

## <a name="generate-sql-scripts"></a><span data-ttu-id="5b668-183">Создание скриптов SQL</span><span class="sxs-lookup"><span data-stu-id="5b668-183">Generate SQL scripts</span></span>

<span data-ttu-id="5b668-184">При отладке миграций или их развертывании в рабочей базе данных бывает полезно создать скрипт SQL.</span><span class="sxs-lookup"><span data-stu-id="5b668-184">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="5b668-185">Такой скрипт можно дополнительно проверить на точность и настроить в соответствии с потребностями рабочей базы данных.</span><span class="sxs-lookup"><span data-stu-id="5b668-185">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="5b668-186">Кроме того, его можно использовать в сочетании с технологией развертывания.</span><span class="sxs-lookup"><span data-stu-id="5b668-186">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="5b668-187">Базовая команда имеет следующий вид.</span><span class="sxs-lookup"><span data-stu-id="5b668-187">The basic command is as follows.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="5b668-188">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="5b668-188">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

#### <a name="basic-usage"></a><span data-ttu-id="5b668-189">Основное использование</span><span class="sxs-lookup"><span data-stu-id="5b668-189">Basic Usage</span></span>
```dotnetcli
dotnet ef migrations script
```

#### <a name="with-from-to-implied"></a><span data-ttu-id="5b668-190">С from (в подразумеваемую миграцию)</span><span class="sxs-lookup"><span data-stu-id="5b668-190">With From (to implied)</span></span>
<span data-ttu-id="5b668-191">Будет создан скрипт SQL из этой миграции в последнюю миграцию.</span><span class="sxs-lookup"><span data-stu-id="5b668-191">This will generate a SQL script from this migration to the latest migration.</span></span>
```dotnetcli
dotnet ef migrations script 20190725054716_Add_new_tables
```

#### <a name="with-from-and-to"></a><span data-ttu-id="5b668-192">С from и to</span><span class="sxs-lookup"><span data-stu-id="5b668-192">With From and To</span></span>
<span data-ttu-id="5b668-193">Будет создан скрипт SQL из миграции `from`в указанную миграцию `to`.</span><span class="sxs-lookup"><span data-stu-id="5b668-193">This will generate a SQL script from the `from` migration to the specified `to` migration.</span></span>
```dotnetcli
dotnet ef migrations script 20190725054716_Add_new_tables 20190829031257_Add_audit_table
```
<span data-ttu-id="5b668-194">Для создания скрипта отката можно использовать значение `from`, более новое, чем `to`.</span><span class="sxs-lookup"><span data-stu-id="5b668-194">You can use a `from` that is newer than the `to` in order to generate a rollback script.</span></span> <span data-ttu-id="5b668-195">*Обязательно учитывайте возможные сценарии потери данных.*</span><span class="sxs-lookup"><span data-stu-id="5b668-195">*Please take note of potential data loss scenarios.*</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="5b668-196">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b668-196">Visual Studio</span></span>](#tab/vs)

#### <a name="basic-usage"></a><span data-ttu-id="5b668-197">Основное использование</span><span class="sxs-lookup"><span data-stu-id="5b668-197">Basic Usage</span></span>
``` powershell
Script-Migration
```

#### <a name="with-from-to-implied"></a><span data-ttu-id="5b668-198">С from (в подразумеваемую миграцию)</span><span class="sxs-lookup"><span data-stu-id="5b668-198">With From (to implied)</span></span>
<span data-ttu-id="5b668-199">Будет создан скрипт SQL из этой миграции в последнюю миграцию.</span><span class="sxs-lookup"><span data-stu-id="5b668-199">This will generate a SQL script from this migration to the latest migration.</span></span>
```powershell
Script-Migration 20190725054716_Add_new_tables
```

#### <a name="with-from-and-to"></a><span data-ttu-id="5b668-200">С from и to</span><span class="sxs-lookup"><span data-stu-id="5b668-200">With From and To</span></span>
<span data-ttu-id="5b668-201">Будет создан скрипт SQL из миграции `from`в указанную миграцию `to`.</span><span class="sxs-lookup"><span data-stu-id="5b668-201">This will generate a SQL script from the `from` migration to the specified `to` migration.</span></span>
```powershell
Script-Migration 20190725054716_Add_new_tables 20190829031257_Add_audit_table
```
<span data-ttu-id="5b668-202">Для создания скрипта отката можно использовать значение `from`, более новое, чем `to`.</span><span class="sxs-lookup"><span data-stu-id="5b668-202">You can use a `from` that is newer than the `to` in order to generate a rollback script.</span></span> <span data-ttu-id="5b668-203">*Обязательно учитывайте возможные сценарии потери данных.*</span><span class="sxs-lookup"><span data-stu-id="5b668-203">*Please take note of potential data loss scenarios.*</span></span>

***

<span data-ttu-id="5b668-204">Для этой команды доступно несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="5b668-204">There are several options to this command.</span></span>

<span data-ttu-id="5b668-205">Миграция **from** должна быть последней миграцией, применяемой к базе данных перед выполнением скрипта.</span><span class="sxs-lookup"><span data-stu-id="5b668-205">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="5b668-206">Если не было применено ни одной миграции, укажите `0` (это значение по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="5b668-206">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="5b668-207">Миграция **to** является последней миграцией, применяемой к базе данных после выполнения скрипта.</span><span class="sxs-lookup"><span data-stu-id="5b668-207">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="5b668-208">По умолчанию она является последней миграцией в проекте.</span><span class="sxs-lookup"><span data-stu-id="5b668-208">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="5b668-209">При необходимости можно создать **идемпотентный** скрипт.</span><span class="sxs-lookup"><span data-stu-id="5b668-209">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="5b668-210">Он применяет миграции только в том случае, если они еще не были применены к базе данных.</span><span class="sxs-lookup"><span data-stu-id="5b668-210">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="5b668-211">Это удобно, если точно неизвестно, какая последняя миграция была применена к базе данных, или вы развертываете несколько баз данных, каждая из которых может иметь отдельную миграцию.</span><span class="sxs-lookup"><span data-stu-id="5b668-211">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

## <a name="apply-migrations-at-runtime"></a><span data-ttu-id="5b668-212">Применение миграции во время выполнения</span><span class="sxs-lookup"><span data-stu-id="5b668-212">Apply migrations at runtime</span></span>

<span data-ttu-id="5b668-213">Некоторым приложениям может потребоваться применить миграции во время выполнения — при запуске или первом выполнении.</span><span class="sxs-lookup"><span data-stu-id="5b668-213">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="5b668-214">Для этого можно использовать метод `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="5b668-214">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="5b668-215">Этот метод основан на службе `IMigrator`, которую можно применять в более сложных сценариях.</span><span class="sxs-lookup"><span data-stu-id="5b668-215">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="5b668-216">Для доступа к нему используйте `myDbContext.GetInfrastructure().GetService<IMigrator>()`.</span><span class="sxs-lookup"><span data-stu-id="5b668-216">Use `myDbContext.GetInfrastructure().GetService<IMigrator>()` to access it.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
>
> * <span data-ttu-id="5b668-217">Такой подход не является универсальным.</span><span class="sxs-lookup"><span data-stu-id="5b668-217">This approach isn't for everyone.</span></span> <span data-ttu-id="5b668-218">Хотя он отлично подходит для приложений с локальной базой данных, большинству приложений требуется более надежная стратегия развертывания, такая как создание скриптов SQL.</span><span class="sxs-lookup"><span data-stu-id="5b668-218">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>
> * <span data-ttu-id="5b668-219">Не вызывайте `EnsureCreated()` перед `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="5b668-219">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="5b668-220">`EnsureCreated()` обходит миграции, чтобы создать схему, что приводит к сбою `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="5b668-220">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5b668-221">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="5b668-221">Next steps</span></span>

<span data-ttu-id="5b668-222">Для получения дополнительной информации см. <xref:core/miscellaneous/cli/index>.</span><span class="sxs-lookup"><span data-stu-id="5b668-222">For more information, see <xref:core/miscellaneous/cli/index>.</span></span>
