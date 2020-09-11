---
title: Обзор миграций — EF Core
description: Общие сведения об использовании миграций для управления схемами базы данных с помощью Entity Framework Core
author: bricelam
ms.author: bricelam
ms.date: 05/06/2020
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: f1197fb869c33c34672d20e9b727cd187c9c5601
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89619478"
---
# <a name="migrations-overview"></a><span data-ttu-id="6f45e-103">Обзор миграций</span><span class="sxs-lookup"><span data-stu-id="6f45e-103">Migrations Overview</span></span>

<span data-ttu-id="6f45e-104">В реальных проектах модели данных изменяются по мере реализации функций. При добавлении или изменении новых сущностей или свойств схемы базы данных должны быть соответствующим образом изменены для синхронизации с приложением.</span><span class="sxs-lookup"><span data-stu-id="6f45e-104">In real world projects, data models change as features get implemented: new entities or properties are added and removed, and database schemas needs to be changed accordingly to be kept in sync with the application.</span></span> <span data-ttu-id="6f45e-105">Функция миграции в EF Core позволяет последовательно применять изменения схемы к базе данных, чтобы синхронизировать ее с моделью данных в приложении без потери существующих данных.</span><span class="sxs-lookup"><span data-stu-id="6f45e-105">The migrations feature in EF Core provides a way to incrementally update the database schema to keep it in sync with the application's data model while preserving existing data in the database.</span></span>

<span data-ttu-id="6f45e-106">В общем миграции работают следующим образом.</span><span class="sxs-lookup"><span data-stu-id="6f45e-106">At a high level, migrations function in the following way:</span></span>

* <span data-ttu-id="6f45e-107">При появлении изменения модели данных разработчик использует средства EF Core, чтобы добавить соответствующую миграцию с описанием обновлений, необходимых для синхронизации схемы базы данных. EF Core сравнивает текущую модель с моментальным снимком старой модели для определения различий и создает исходные файлы миграции. Файлы можно отслеживать в системе управления версиями проекта, как и любые другие исходные файлы.</span><span class="sxs-lookup"><span data-stu-id="6f45e-107">When a data model change is introduced, the developer uses EF Core tools to add a corresponding migration describing the updates necessary to keep the database schema in sync. EF Core compares the current model against a snapshot of the old model to determine the differences, and generates migration source files; the files can be tracked in your project's source control like any other source file.</span></span>
* <span data-ttu-id="6f45e-108">Созданную миграцию можно применять к базе данных различными способами.</span><span class="sxs-lookup"><span data-stu-id="6f45e-108">Once a new migration has been generated, it can be applied to a database in various ways.</span></span> <span data-ttu-id="6f45e-109">EF Core записывает все примененные миграции в специальную таблицу журнала, из которой будет ясно, какие миграции были применены, а какие нет.</span><span class="sxs-lookup"><span data-stu-id="6f45e-109">EF Core records all applied migrations in a special history table, allowing it to know which migrations have been applied and which haven't.</span></span>

<span data-ttu-id="6f45e-110">Оставшаяся часть этой страницы представляет собой пошаговое руководство для начинающих по использованию миграций.</span><span class="sxs-lookup"><span data-stu-id="6f45e-110">The rest of this page is a step-by-step beginner's guide for using migrations.</span></span> <span data-ttu-id="6f45e-111">Дополнительные сведения см. на других страницах этого раздела.</span><span class="sxs-lookup"><span data-stu-id="6f45e-111">Consult the other pages in this section for more in-depth information.</span></span>

## <a name="getting-started"></a><span data-ttu-id="6f45e-112">Начало работы</span><span class="sxs-lookup"><span data-stu-id="6f45e-112">Getting started</span></span>

<span data-ttu-id="6f45e-113">Предположим, что вы только что завершили создание первого приложения EF Core, которое содержит следующую простую модель:</span><span class="sxs-lookup"><span data-stu-id="6f45e-113">Let's assume you've just completed your first EF Core application, which contains the following simple model:</span></span>

```c#
public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```

<span data-ttu-id="6f45e-114">Во время разработки вы, возможно, использовали [API-интерфейсы создания и удаления](xref:core/managing-schemas/ensure-created) для быстрой итерации и необходимого изменения модели. Но теперь, когда приложение будет выпущено в рабочую среду, вам нужен способ безопасного развертывания схемы без удаления всей базы данных.</span><span class="sxs-lookup"><span data-stu-id="6f45e-114">During development, you may have used the [Create and Drop APIs](xref:core/managing-schemas/ensure-created) to iterate quickly, changing your model as needed; but now that your application is going to production, you need a way to safely evolve the schema without dropping the entire database.</span></span>

### <a name="install-the-tools"></a><span data-ttu-id="6f45e-115">Установка инструментов</span><span class="sxs-lookup"><span data-stu-id="6f45e-115">Install the tools</span></span>

<span data-ttu-id="6f45e-116">Во-первых, необходимо установить [средства командной строки EF Core](xref:core/miscellaneous/cli/index):</span><span class="sxs-lookup"><span data-stu-id="6f45e-116">First, you'll have to install the [EF Core command-line tools](xref:core/miscellaneous/cli/index):</span></span>

* <span data-ttu-id="6f45e-117">Обычно рекомендуется использовать [средства интерфейса командной строки .NET](xref:core/miscellaneous/cli/dotnet), которые работают на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="6f45e-117">We generally recommend using the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet), which work on all platforms.</span></span>
* <span data-ttu-id="6f45e-118">Если вы привыкли работать в Visual Studio или знакомы с миграциями EF6, можно также использовать [средства консоли диспетчера пакетов](xref:core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="6f45e-118">If you're more comfortable working inside Visual Studio or have experience with EF6 migrations, you can also use the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell).</span></span>

### <a name="create-your-first-migration"></a><span data-ttu-id="6f45e-119">Создание первой миграции</span><span class="sxs-lookup"><span data-stu-id="6f45e-119">Create your first migration</span></span>

<span data-ttu-id="6f45e-120">Теперь все готово к добавлению первой миграции.</span><span class="sxs-lookup"><span data-stu-id="6f45e-120">You're now ready to add your first migration!</span></span> <span data-ttu-id="6f45e-121">Укажите EF Core создать миграцию с именем **InitialCreate**:</span><span class="sxs-lookup"><span data-stu-id="6f45e-121">Instruct EF Core to create a migration named **InitialCreate**:</span></span>

#### <a name="net-core-cli"></a>[<span data-ttu-id="6f45e-122">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="6f45e-122">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate
```

#### <a name="visual-studio"></a>[<span data-ttu-id="6f45e-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6f45e-123">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration InitialCreate
```

***

<span data-ttu-id="6f45e-124">EF Core создаст в проекте каталог с именем **Migrations** и добавит несколько файлов.</span><span class="sxs-lookup"><span data-stu-id="6f45e-124">EF Core will create a directory called **Migrations** in your project, and generate some files.</span></span> <span data-ttu-id="6f45e-125">Рекомендуется проверить, какие именно файлы созданы в EF Core, и, возможно, исправить их, но сейчас мы это делать не будем.</span><span class="sxs-lookup"><span data-stu-id="6f45e-125">It's a good idea to inspect what exactly EF Core generated - and possibly amend it - but we'll skip over that for now.</span></span>

### <a name="create-your-database-and-schema"></a><span data-ttu-id="6f45e-126">Создание базы данных и схемы</span><span class="sxs-lookup"><span data-stu-id="6f45e-126">Create your database and schema</span></span>

<span data-ttu-id="6f45e-127">На этом этапе в EF можно создать базу данных и схему из миграции.</span><span class="sxs-lookup"><span data-stu-id="6f45e-127">At this point you can have EF create your database and create your schema from the migration.</span></span> <span data-ttu-id="6f45e-128">Это можно сделать с помощью следующих средств:</span><span class="sxs-lookup"><span data-stu-id="6f45e-128">This can be done via the following:</span></span>

#### <a name="net-core-cli"></a>[<span data-ttu-id="6f45e-129">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="6f45e-129">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```
#### <a name="visual-studio"></a>[<span data-ttu-id="6f45e-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6f45e-130">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database
```

***

<span data-ttu-id="6f45e-131">Вот и все — ваше приложение готово к работе в новой базе данных, и вам не пришлось писать ни одной строки кода SQL.</span><span class="sxs-lookup"><span data-stu-id="6f45e-131">That's all there is to it - your application is ready to run on your new database, and you didn't need to write a single line of SQL.</span></span> <span data-ttu-id="6f45e-132">Обратите внимание, что такой способ применения миграций идеально подходит для локальной разработки и хуже подходит для рабочих сред. Дополнительные сведения см. на странице [применения миграций](xref:core/managing-schemas/migrations/applying).</span><span class="sxs-lookup"><span data-stu-id="6f45e-132">Note that this way of applying migrations is ideal for local development, but is less suitable for production environments - see the [Applying Migrations page](xref:core/managing-schemas/migrations/applying) for more info.</span></span>

### <a name="evolving-your-model"></a><span data-ttu-id="6f45e-133">Развитие модели</span><span class="sxs-lookup"><span data-stu-id="6f45e-133">Evolving your model</span></span>

<span data-ttu-id="6f45e-134">Прошло несколько дней. Вам дали задание добавить в блоги метку времени создания.</span><span class="sxs-lookup"><span data-stu-id="6f45e-134">A few days have passed, and you're asked to add a creation timestamp to your blogs.</span></span> <span data-ttu-id="6f45e-135">Вы внесли необходимые изменения в приложения, и теперь модель выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="6f45e-135">You've done the necessary changes to your application, and your model now looks like this:</span></span>

```c#
public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }
    public DateTime CreatedTimestamp { get; set; }
}
```

<span data-ttu-id="6f45e-136">Но модель и рабочая база данных не синхронизированы, поэтому необходимо добавить новый столбец в схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="6f45e-136">Your model and your production database are now out of sync - we must add a new column to your database schema.</span></span> <span data-ttu-id="6f45e-137">Создадим новую миграцию для этой задачи:</span><span class="sxs-lookup"><span data-stu-id="6f45e-137">Let's create a new migration for this:</span></span>

#### <a name="net-core-cli"></a>[<span data-ttu-id="6f45e-138">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="6f45e-138">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add AddBlogCreatedTimestamp
```

#### <a name="visual-studio"></a>[<span data-ttu-id="6f45e-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6f45e-139">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration AddBlogCreatedTimestamp
```

***

<span data-ttu-id="6f45e-140">Обратите внимание, что миграциям задается описательное имя, чтобы в дальнейшем упростить чтение и понимание журнала проекта.</span><span class="sxs-lookup"><span data-stu-id="6f45e-140">Note that we give migrations a descriptive name, to make it easier to understand the project history later.</span></span>

<span data-ttu-id="6f45e-141">Так как это не первая миграция проекта, теперь перед добавлением столбца EF Core сравнивает обновленную модель с моментальным снимком старой модели. Моментальный снимок модели — это один из файлов, созданных EF Core при добавлении миграции и возвращенных в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="6f45e-141">Since this isn't the project's first migration, EF Core now compares your updated model against a snapshot of the old model, before the column was added; the model snapshot is one of the files generated by EF Core when you add a migration, and is checked into source control.</span></span> <span data-ttu-id="6f45e-142">На основе этого сравнения EF Core обнаруживает, что был добавлен столбец, и добавляет соответствующую миграцию.</span><span class="sxs-lookup"><span data-stu-id="6f45e-142">Based on that comparison, EF Core detects that a column has been added, and adds the appropriate migration.</span></span>

<span data-ttu-id="6f45e-143">Теперь можно применить миграцию, как и раньше:</span><span class="sxs-lookup"><span data-stu-id="6f45e-143">You can now apply your migration as before:</span></span>

#### <a name="net-core-cli"></a>[<span data-ttu-id="6f45e-144">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="6f45e-144">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```
#### <a name="visual-studio"></a>[<span data-ttu-id="6f45e-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6f45e-145">Visual Studio</span></span>](#tab/vs)

``` powershell
Update-Database
```

***

<span data-ttu-id="6f45e-146">Обратите внимание, что на этот раз EF обнаруживает, что база данных уже существует.</span><span class="sxs-lookup"><span data-stu-id="6f45e-146">Note that this time, EF detects that the database already exists.</span></span> <span data-ttu-id="6f45e-147">Кроме того, когда была применена первая миграция, этот факт был записан в специальную таблицу журнала миграций в базе данных, что позволяет EF автоматически применять только новую миграцию.</span><span class="sxs-lookup"><span data-stu-id="6f45e-147">In addition, when our first migration was applied above, this fact was recorded in a special migrations history table in your database; this allows EF to automatically apply only the new migration.</span></span>

### <a name="next-steps"></a><span data-ttu-id="6f45e-148">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="6f45e-148">Next steps</span></span>

<span data-ttu-id="6f45e-149">Приведенные выше сведения были лишь кратким описанием миграций.</span><span class="sxs-lookup"><span data-stu-id="6f45e-149">The above was only a brief introduction to migrations.</span></span> <span data-ttu-id="6f45e-150">Дополнительную информацию об [управлении миграциями](xref:core/managing-schemas/migrations/managing), [применении миграций](xref:core/managing-schemas/migrations/applying) и т. д. можно найти на других страницах документации.</span><span class="sxs-lookup"><span data-stu-id="6f45e-150">Please consult the other documentation pages to learn more about [managing migrations](xref:core/managing-schemas/migrations/managing), [applying them](xref:core/managing-schemas/migrations/applying), and other aspects.</span></span> <span data-ttu-id="6f45e-151">В [справочнике по инструментам .NET Core CLI](xref:core/miscellaneous/cli/index) также содержатся полезные сведения о различных командах.</span><span class="sxs-lookup"><span data-stu-id="6f45e-151">The [.NET Core CLI tool reference](xref:core/miscellaneous/cli/index) also contains useful information on the different commands</span></span>
