---
title: Обзор миграций — EF Core
description: Общие сведения об использовании миграций для управления схемами базы данных с помощью Entity Framework Core
author: bricelam
ms.date: 10/28/2020
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: b9547298714af59453aeae6d05742a03c067708b
ms.sourcegitcommit: 788a56c2248523967b846bcca0e98c2ed7ef0d6b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2020
ms.locfileid: "95003399"
---
# <a name="migrations-overview"></a><span data-ttu-id="c764d-103">Обзор миграций</span><span class="sxs-lookup"><span data-stu-id="c764d-103">Migrations Overview</span></span>

<span data-ttu-id="c764d-104">В реальных проектах модели данных изменяются по мере реализации функций. При добавлении или изменении новых сущностей или свойств схемы базы данных должны быть соответствующим образом изменены для синхронизации с приложением.</span><span class="sxs-lookup"><span data-stu-id="c764d-104">In real world projects, data models change as features get implemented: new entities or properties are added and removed, and database schemas needs to be changed accordingly to be kept in sync with the application.</span></span> <span data-ttu-id="c764d-105">Функция миграции в EF Core позволяет последовательно применять изменения схемы к базе данных, чтобы синхронизировать ее с моделью данных в приложении без потери существующих данных.</span><span class="sxs-lookup"><span data-stu-id="c764d-105">The migrations feature in EF Core provides a way to incrementally update the database schema to keep it in sync with the application's data model while preserving existing data in the database.</span></span>

<span data-ttu-id="c764d-106">В общем миграции работают следующим образом.</span><span class="sxs-lookup"><span data-stu-id="c764d-106">At a high level, migrations function in the following way:</span></span>

* <span data-ttu-id="c764d-107">При появлении изменения модели данных разработчик использует средства EF Core, чтобы добавить соответствующую миграцию с описанием обновлений, необходимых для синхронизации схемы базы данных. EF Core сравнивает текущую модель с моментальным снимком старой модели для определения различий и создает исходные файлы миграции. Файлы можно отслеживать в системе управления версиями проекта, как и любые другие исходные файлы.</span><span class="sxs-lookup"><span data-stu-id="c764d-107">When a data model change is introduced, the developer uses EF Core tools to add a corresponding migration describing the updates necessary to keep the database schema in sync. EF Core compares the current model against a snapshot of the old model to determine the differences, and generates migration source files; the files can be tracked in your project's source control like any other source file.</span></span>
* <span data-ttu-id="c764d-108">Созданную миграцию можно применять к базе данных различными способами.</span><span class="sxs-lookup"><span data-stu-id="c764d-108">Once a new migration has been generated, it can be applied to a database in various ways.</span></span> <span data-ttu-id="c764d-109">EF Core записывает все примененные миграции в специальную таблицу журнала, из которой будет ясно, какие миграции были применены, а какие нет.</span><span class="sxs-lookup"><span data-stu-id="c764d-109">EF Core records all applied migrations in a special history table, allowing it to know which migrations have been applied and which haven't.</span></span>

<span data-ttu-id="c764d-110">Оставшаяся часть этой страницы представляет собой пошаговое руководство для начинающих по использованию миграций.</span><span class="sxs-lookup"><span data-stu-id="c764d-110">The rest of this page is a step-by-step beginner's guide for using migrations.</span></span> <span data-ttu-id="c764d-111">Дополнительные сведения см. на других страницах этого раздела.</span><span class="sxs-lookup"><span data-stu-id="c764d-111">Consult the other pages in this section for more in-depth information.</span></span>

## <a name="getting-started"></a><span data-ttu-id="c764d-112">Начало работы</span><span class="sxs-lookup"><span data-stu-id="c764d-112">Getting started</span></span>

<span data-ttu-id="c764d-113">Предположим, что вы только что завершили создание первого приложения EF Core, которое содержит следующую простую модель:</span><span class="sxs-lookup"><span data-stu-id="c764d-113">Let's assume you've just completed your first EF Core application, which contains the following simple model:</span></span>

```csharp
public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```

<span data-ttu-id="c764d-114">Во время разработки вы, возможно, использовали [API-интерфейсы создания и удаления](xref:core/managing-schemas/ensure-created) для быстрой итерации и необходимого изменения модели. Но теперь, когда приложение будет выпущено в рабочую среду, вам нужен способ безопасного развертывания схемы без удаления всей базы данных.</span><span class="sxs-lookup"><span data-stu-id="c764d-114">During development, you may have used the [Create and Drop APIs](xref:core/managing-schemas/ensure-created) to iterate quickly, changing your model as needed; but now that your application is going to production, you need a way to safely evolve the schema without dropping the entire database.</span></span>

### <a name="install-the-tools"></a><span data-ttu-id="c764d-115">Установка инструментов</span><span class="sxs-lookup"><span data-stu-id="c764d-115">Install the tools</span></span>

<span data-ttu-id="c764d-116">Во-первых, необходимо установить [средства командной строки EF Core](xref:core/cli/index):</span><span class="sxs-lookup"><span data-stu-id="c764d-116">First, you'll have to install the [EF Core command-line tools](xref:core/cli/index):</span></span>

* <span data-ttu-id="c764d-117">Обычно рекомендуется использовать [средства интерфейса командной строки .NET](xref:core/cli/dotnet), которые работают на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="c764d-117">We generally recommend using the [.NET Core CLI tools](xref:core/cli/dotnet), which work on all platforms.</span></span>
* <span data-ttu-id="c764d-118">Если вы привыкли работать в Visual Studio или знакомы с миграциями EF6, можно также использовать [средства консоли диспетчера пакетов](xref:core/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="c764d-118">If you're more comfortable working inside Visual Studio or have experience with EF6 migrations, you can also use the [Package Manager Console tools](xref:core/cli/powershell).</span></span>

### <a name="create-your-first-migration"></a><span data-ttu-id="c764d-119">Создание первой миграции</span><span class="sxs-lookup"><span data-stu-id="c764d-119">Create your first migration</span></span>

<span data-ttu-id="c764d-120">Теперь все готово к добавлению первой миграции.</span><span class="sxs-lookup"><span data-stu-id="c764d-120">You're now ready to add your first migration!</span></span> <span data-ttu-id="c764d-121">Укажите EF Core создать миграцию с именем **InitialCreate**:</span><span class="sxs-lookup"><span data-stu-id="c764d-121">Instruct EF Core to create a migration named **InitialCreate**:</span></span>

#### <a name="net-core-cli"></a>[<span data-ttu-id="c764d-122">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="c764d-122">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate
```

#### <a name="visual-studio"></a>[<span data-ttu-id="c764d-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c764d-123">Visual Studio</span></span>](#tab/vs)

```powershell
Add-Migration InitialCreate
```

<span data-ttu-id="c764d-124">\*\*_</span><span class="sxs-lookup"><span data-stu-id="c764d-124">\*\*_</span></span>

<span data-ttu-id="c764d-125">EF Core создаст в проекте каталог с именем _ *Migrations*\* и добавит несколько файлов.</span><span class="sxs-lookup"><span data-stu-id="c764d-125">EF Core will create a directory called _ *Migrations*\* in your project, and generate some files.</span></span> <span data-ttu-id="c764d-126">Рекомендуется проверить, какие именно файлы созданы в EF Core, и, возможно, исправить их, но сейчас мы это делать не будем.</span><span class="sxs-lookup"><span data-stu-id="c764d-126">It's a good idea to inspect what exactly EF Core generated - and possibly amend it - but we'll skip over that for now.</span></span>

### <a name="create-your-database-and-schema"></a><span data-ttu-id="c764d-127">Создание базы данных и схемы</span><span class="sxs-lookup"><span data-stu-id="c764d-127">Create your database and schema</span></span>

<span data-ttu-id="c764d-128">На этом этапе в EF можно создать базу данных и схему из миграции.</span><span class="sxs-lookup"><span data-stu-id="c764d-128">At this point you can have EF create your database and create your schema from the migration.</span></span> <span data-ttu-id="c764d-129">Это можно сделать с помощью следующих средств:</span><span class="sxs-lookup"><span data-stu-id="c764d-129">This can be done via the following:</span></span>

#### <a name="net-core-cli"></a>[<span data-ttu-id="c764d-130">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="c764d-130">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

#### <a name="visual-studio"></a>[<span data-ttu-id="c764d-131">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c764d-131">Visual Studio</span></span>](#tab/vs)

```powershell
Update-Database
```

<span data-ttu-id="c764d-132">\*\*_</span><span class="sxs-lookup"><span data-stu-id="c764d-132">\*\*_</span></span>

<span data-ttu-id="c764d-133">Вот и все — ваше приложение готово к работе в новой базе данных, и вам не пришлось писать ни одной строки кода SQL.</span><span class="sxs-lookup"><span data-stu-id="c764d-133">That's all there is to it - your application is ready to run on your new database, and you didn't need to write a single line of SQL.</span></span> <span data-ttu-id="c764d-134">Обратите внимание, что такой способ применения миграций идеально подходит для локальной разработки и хуже подходит для рабочих сред. Дополнительные сведения см. на странице [применения миграций](xref:core/managing-schemas/migrations/applying).</span><span class="sxs-lookup"><span data-stu-id="c764d-134">Note that this way of applying migrations is ideal for local development, but is less suitable for production environments - see the [Applying Migrations page](xref:core/managing-schemas/migrations/applying) for more info.</span></span>

### <a name="evolving-your-model"></a><span data-ttu-id="c764d-135">Развитие модели</span><span class="sxs-lookup"><span data-stu-id="c764d-135">Evolving your model</span></span>

<span data-ttu-id="c764d-136">Прошло несколько дней. Вам дали задание добавить в блоги метку времени создания.</span><span class="sxs-lookup"><span data-stu-id="c764d-136">A few days have passed, and you're asked to add a creation timestamp to your blogs.</span></span> <span data-ttu-id="c764d-137">Вы внесли необходимые изменения в приложения, и теперь модель выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="c764d-137">You've done the necessary changes to your application, and your model now looks like this:</span></span>

```csharp
public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }
    public DateTime CreatedTimestamp { get; set; }
}
```

<span data-ttu-id="c764d-138">Но модель и рабочая база данных не синхронизированы, поэтому необходимо добавить новый столбец в схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="c764d-138">Your model and your production database are now out of sync - we must add a new column to your database schema.</span></span> <span data-ttu-id="c764d-139">Создадим новую миграцию для этой задачи:</span><span class="sxs-lookup"><span data-stu-id="c764d-139">Let's create a new migration for this:</span></span>

#### <a name="net-core-cli"></a>[<span data-ttu-id="c764d-140">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="c764d-140">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add AddBlogCreatedTimestamp
```

#### <a name="visual-studio"></a>[<span data-ttu-id="c764d-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c764d-141">Visual Studio</span></span>](#tab/vs)

```powershell
Add-Migration AddBlogCreatedTimestamp
```

_*_

<span data-ttu-id="c764d-142">Обратите внимание, что миграциям задается описательное имя, чтобы в дальнейшем упростить чтение и понимание журнала проекта.</span><span class="sxs-lookup"><span data-stu-id="c764d-142">Note that we give migrations a descriptive name, to make it easier to understand the project history later.</span></span>

<span data-ttu-id="c764d-143">Так как это не первая миграция проекта, теперь перед добавлением столбца EF Core сравнивает обновленную модель с моментальным снимком старой модели. Моментальный снимок модели — это один из файлов, созданных EF Core при добавлении миграции и возвращенных в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="c764d-143">Since this isn't the project's first migration, EF Core now compares your updated model against a snapshot of the old model, before the column was added; the model snapshot is one of the files generated by EF Core when you add a migration, and is checked into source control.</span></span> <span data-ttu-id="c764d-144">На основе этого сравнения EF Core обнаруживает, что был добавлен столбец, и добавляет соответствующую миграцию.</span><span class="sxs-lookup"><span data-stu-id="c764d-144">Based on that comparison, EF Core detects that a column has been added, and adds the appropriate migration.</span></span>

<span data-ttu-id="c764d-145">Теперь можно применить миграцию, как и раньше:</span><span class="sxs-lookup"><span data-stu-id="c764d-145">You can now apply your migration as before:</span></span>

<!--markdownlint-disable MD024-->

#### <a name="net-core-cli"></a>[<span data-ttu-id="c764d-146">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="c764d-146">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

#### <a name="visual-studio"></a>[<span data-ttu-id="c764d-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c764d-147">Visual Studio</span></span>](#tab/vs)

```powershell
Update-Database
```

<!--markdownlint-enable MD024-->

_*_

<span data-ttu-id="c764d-148">Обратите внимание, что на этот раз EF обнаруживает, что база данных уже существует.</span><span class="sxs-lookup"><span data-stu-id="c764d-148">Note that this time, EF detects that the database already exists.</span></span> <span data-ttu-id="c764d-149">Кроме того, когда была применена первая миграция, этот факт был записан в специальную таблицу журнала миграций в базе данных, что позволяет EF автоматически применять только новую миграцию.</span><span class="sxs-lookup"><span data-stu-id="c764d-149">In addition, when our first migration was applied above, this fact was recorded in a special migrations history table in your database; this allows EF to automatically apply only the new migration.</span></span>

### <a name="excluding-parts-of-your-model"></a><span data-ttu-id="c764d-150">Исключение частей модели</span><span class="sxs-lookup"><span data-stu-id="c764d-150">Excluding parts of your model</span></span>

> [!NOTE]
> <span data-ttu-id="c764d-151">Эта возможность появилась в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="c764d-151">This feature was introduced EF in Core 5.0.</span></span>

<span data-ttu-id="c764d-152">Иногда может потребоваться создать ссылки на типы из другого экземпляра DbContext.</span><span class="sxs-lookup"><span data-stu-id="c764d-152">Sometimes you may want to reference types from another DbContext.</span></span> <span data-ttu-id="c764d-153">Это может привести к конфликтам миграции.</span><span class="sxs-lookup"><span data-stu-id="c764d-153">This can lead to migration conflicts.</span></span> <span data-ttu-id="c764d-154">Чтобы избежать этого, исключите тип из миграций одного из экземпляров DbContext.</span><span class="sxs-lookup"><span data-stu-id="c764d-154">To prevent this, exclude the type from the migrations of one of the DbContexts.</span></span>

[!code-csharp[](../../../../samples/core/Modeling/FluentAPI/TableExcludeFromMigrations.cs#TableExcludeFromMigrations)]

### <a name="next-steps"></a><span data-ttu-id="c764d-155">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="c764d-155">Next steps</span></span>

<span data-ttu-id="c764d-156">Приведенные выше сведения были лишь кратким описанием миграций.</span><span class="sxs-lookup"><span data-stu-id="c764d-156">The above was only a brief introduction to migrations.</span></span> <span data-ttu-id="c764d-157">Дополнительную информацию об [управлении миграциями](xref:core/managing-schemas/migrations/managing), [применении миграций](xref:core/managing-schemas/migrations/applying) и т. д. можно найти на других страницах документации.</span><span class="sxs-lookup"><span data-stu-id="c764d-157">Please consult the other documentation pages to learn more about [managing migrations](xref:core/managing-schemas/migrations/managing), [applying them](xref:core/managing-schemas/migrations/applying), and other aspects.</span></span> <span data-ttu-id="c764d-158">В [справочнике по инструментам .NET Core CLI](xref:core/cli/index) также содержатся полезные сведения о различных командах.</span><span class="sxs-lookup"><span data-stu-id="c764d-158">The [.NET Core CLI tool reference](xref:core/cli/index) also contains useful information on the different commands</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c764d-159">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c764d-159">Additional resources</span></span>

<span data-ttu-id="c764d-160">_ [Рабочее совещание сообщества EF Core](https://www.youtube.com/watch?v=mSsGERmrhnE&list=PLdo4fOcmZ0oX-DBuRG4u58ZTAJgBAeQ-t&index=20), посвященное новым функциями миграции в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="c764d-160">_ [EF Core Community Standup session](https://www.youtube.com/watch?v=mSsGERmrhnE&list=PLdo4fOcmZ0oX-DBuRG4u58ZTAJgBAeQ-t&index=20) going over new migration features in EF Core 5.0.</span></span>
