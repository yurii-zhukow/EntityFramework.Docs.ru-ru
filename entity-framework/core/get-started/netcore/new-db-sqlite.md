---
title: Начало работы в .NET Core — новая база данных — Core EF
author: rick-anderson
ms.author: riande
description: Начало работы с .NET Core и Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: ec20040917a2bca8177924b6905b1cd79e5cd9da
ms.sourcegitcommit: 7a7da65404c9338e1e3df42576a13be536a6f95f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/06/2018
ms.locfileid: "48834739"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="c2428-103">Начало работы с EF Core в консольном приложении .NET Core с новой базой данных</span><span class="sxs-lookup"><span data-stu-id="c2428-103">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="c2428-104">В этом руководстве вы создадите консольное приложение .NET Core, которое осуществляет доступ к базе данных SQLite с помощью Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c2428-104">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="c2428-105">Вы примените процесс миграций, чтобы создать базу данных из модели.</span><span class="sxs-lookup"><span data-stu-id="c2428-105">You use migrations to create the database from the model.</span></span> <span data-ttu-id="c2428-106">В разделе [ASP.NET Core - новая база данных](xref:core/get-started/aspnetcore/new-db) описана версия для Visual Studio с использованием ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="c2428-106">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

<span data-ttu-id="c2428-107">Пример для этой статьи на GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span><span class="sxs-lookup"><span data-stu-id="c2428-107">View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c2428-108">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="c2428-108">Prerequisites</span></span>

* [<span data-ttu-id="c2428-109">Пакет SDK для NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="c2428-109">The .NET Core 2.1 SDK</span></span>](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a><span data-ttu-id="c2428-110">Создание нового проекта</span><span class="sxs-lookup"><span data-stu-id="c2428-110">Create a new project</span></span>

* <span data-ttu-id="c2428-111">Создание консольного проекта:</span><span class="sxs-lookup"><span data-stu-id="c2428-111">Create a new console project:</span></span>

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  ```
## <a name="change-the-current-directory"></a><span data-ttu-id="c2428-112">Изменение текущего каталога</span><span class="sxs-lookup"><span data-stu-id="c2428-112">Change the current directory</span></span>

<span data-ttu-id="c2428-113">В последующих шагах нам нужно выдавать команды `dotnet` для приложения.</span><span class="sxs-lookup"><span data-stu-id="c2428-113">In subsequent steps, we need to issue `dotnet` commands against the application.</span></span>

* <span data-ttu-id="c2428-114">Мы изменяем текущий каталог на каталог приложения следующим образом:</span><span class="sxs-lookup"><span data-stu-id="c2428-114">We change the current directory to the application's directory like this:</span></span>

  ``` Console
  cd ConsoleApp.SQLite/
  ```
## <a name="install-entity-framework-core"></a><span data-ttu-id="c2428-115">Установка Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c2428-115">Install Entity Framework Core</span></span>

<span data-ttu-id="c2428-116">Чтобы использовать EF Core, установите пакеты для поставщиков базы данных, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="c2428-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="c2428-117">В этом пошаговом руководстве используется SQLite.</span><span class="sxs-lookup"><span data-stu-id="c2428-117">This walkthrough uses SQLite.</span></span> <span data-ttu-id="c2428-118">Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).</span><span class="sxs-lookup"><span data-stu-id="c2428-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="c2428-119">Установите Microsoft.EntityFrameworkCore.Sqlite и Microsoft.EntityFrameworkCore.Design:</span><span class="sxs-lookup"><span data-stu-id="c2428-119">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* <span data-ttu-id="c2428-120">Выполните `dotnet restore`, чтобы установить новые пакеты.</span><span class="sxs-lookup"><span data-stu-id="c2428-120">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="c2428-121">Создание модели</span><span class="sxs-lookup"><span data-stu-id="c2428-121">Create the model</span></span>

<span data-ttu-id="c2428-122">Определите контекст и классы сущности, которые должны входить в модель:</span><span class="sxs-lookup"><span data-stu-id="c2428-122">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="c2428-123">Создайте новый файл *Model.cs* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="c2428-123">Create a new *Model.cs* file with the following contents.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="c2428-124">Совет. В реальном приложении каждый класс помещается в отдельный файл, а строка подключения — в файл конфигурации или переменную среды.</span><span class="sxs-lookup"><span data-stu-id="c2428-124">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="c2428-125">Для упрощения в этом руководстве все данные содержатся в одном файле.</span><span class="sxs-lookup"><span data-stu-id="c2428-125">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="c2428-126">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="c2428-126">Create the database</span></span>

<span data-ttu-id="c2428-127">Когда модель будет готова, создайте базу данных с помощью [миграций](xref:core/managing-schemas/migrations/index).</span><span class="sxs-lookup"><span data-stu-id="c2428-127">Once you have a model, you use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

* <span data-ttu-id="c2428-128">Запустите `dotnet ef migrations add InitialCreate`, чтобы сформировать шаблон миграции и создать начальный набор таблиц для модели.</span><span class="sxs-lookup"><span data-stu-id="c2428-128">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="c2428-129">Запустите `dotnet ef database update`, чтобы применить созданную миграцию к базе данных.</span><span class="sxs-lookup"><span data-stu-id="c2428-129">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="c2428-130">Эта команда создает базу данных и применяет к ней миграции.</span><span class="sxs-lookup"><span data-stu-id="c2428-130">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="c2428-131">База данных SQLite *blogging.db* \* находится в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="c2428-131">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="c2428-132">Использование модели</span><span class="sxs-lookup"><span data-stu-id="c2428-132">Use the model</span></span>

* <span data-ttu-id="c2428-133">Откройте файл *Program.cs* и замените его содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="c2428-133">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="c2428-134">Тестирование приложения из консоли.</span><span class="sxs-lookup"><span data-stu-id="c2428-134">Test the app from the console.</span></span> <span data-ttu-id="c2428-135">См. раздел [Примечание Visual Studio](#vs) для запуска приложения из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c2428-135">See the [Visual Studio note](#vs) to run the app from Visual Studio.</span></span>

  `dotnet run`

  <span data-ttu-id="c2428-136">В базе данных будет создан один блог, а затем в консоли появятся сведения обо всех блогах.</span><span class="sxs-lookup"><span data-stu-id="c2428-136">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="c2428-137">Изменение модели</span><span class="sxs-lookup"><span data-stu-id="c2428-137">Changing the model:</span></span>

- <span data-ttu-id="c2428-138">Если вы внесете в модель изменения, с помощью команды `dotnet ef migrations add` вы можете сформировать новую [миграцию](xref:core/managing-schemas/migrations/index).</span><span class="sxs-lookup"><span data-stu-id="c2428-138">If you make changes to the model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](xref:core/managing-schemas/migrations/index).</span></span> <span data-ttu-id="c2428-139">Проверьте сформированный код (и внесите необходимые правки), а затем используйте команду `dotnet ef database update`, чтобы применить изменения схемы к базе данных.</span><span class="sxs-lookup"><span data-stu-id="c2428-139">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the schema changes to the database.</span></span>
- <span data-ttu-id="c2428-140">EF Core использует таблицу `__EFMigrationsHistory` в базе данных для отслеживания уже выполнявшихся в ней миграций.</span><span class="sxs-lookup"><span data-stu-id="c2428-140">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="c2428-141">Ядро СУБД SQLite не поддерживает некоторые изменения схем, поддерживаемые большинством других реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="c2428-141">The SQLite database engine doesn't support certain schema changes that are supported by most other relational databases.</span></span> <span data-ttu-id="c2428-142">Например, не поддерживается операция `DropColumn`.</span><span class="sxs-lookup"><span data-stu-id="c2428-142">For example, the `DropColumn` operation is not supported.</span></span> <span data-ttu-id="c2428-143">При миграциях EF Core будет создан код для этих операций.</span><span class="sxs-lookup"><span data-stu-id="c2428-143">EF Core Migrations will generate code for these operations.</span></span> <span data-ttu-id="c2428-144">Но если вы попробуете применить их к базе данных или сгенерировать скрипт, EF Core будет выдавать исключения.</span><span class="sxs-lookup"><span data-stu-id="c2428-144">But if you try to apply them to a database or generate a script, EF Core throws exceptions.</span></span> <span data-ttu-id="c2428-145">Подробнее о них можно узнать [в этой статье](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="c2428-145">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="c2428-146">Если вы разрабатываете новое приложение, при изменении модели лучше не использовать миграции, а просто отказаться от старой базы данных и создать новую.</span><span class="sxs-lookup"><span data-stu-id="c2428-146">For new development, consider dropping the database and creating a new one rather than using migrations when the model changes.</span></span>

<a name="vs"></a>
### <a name="run-from-visual-studio"></a><span data-ttu-id="c2428-147">Запуск из Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c2428-147">Run from Visual Studio</span></span>

<span data-ttu-id="c2428-148">Чтобы запустить этот пример из Visual Studio, необходимо вручную задать рабочий каталог, который должен быть корнем проекта.</span><span class="sxs-lookup"><span data-stu-id="c2428-148">To run this sample from Visual Studio, you must set the working directory manually to be the root of the project.</span></span> <span data-ttu-id="c2428-149">Если рабочий каталог не задан, возникает следующее исключение `Microsoft.Data.Sqlite.SqliteException`: `SQLite Error 1: 'no such table: Blogs'`.</span><span class="sxs-lookup"><span data-stu-id="c2428-149">If  you don't set the working directory, the following `Microsoft.Data.Sqlite.SqliteException` is thrown: `SQLite Error 1: 'no such table: Blogs'`.</span></span>

<span data-ttu-id="c2428-150">Чтобы задать рабочий каталог, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="c2428-150">To set the working directory:</span></span>

* <span data-ttu-id="c2428-151">В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите пункт **Свойства**.</span><span class="sxs-lookup"><span data-stu-id="c2428-151">In **Solution Explorer**, right click the project and then select **Properties**.</span></span>
* <span data-ttu-id="c2428-152">Выберите вкладку **Отладка** в левой панели.</span><span class="sxs-lookup"><span data-stu-id="c2428-152">Select the **Debug** tab in the left pane.</span></span>
* <span data-ttu-id="c2428-153">Задайте каталог проекта в качестве **рабочего каталога**.</span><span class="sxs-lookup"><span data-stu-id="c2428-153">Set **Working directory** to the project directory.</span></span>
* <span data-ttu-id="c2428-154">Сохраните изменения.</span><span class="sxs-lookup"><span data-stu-id="c2428-154">Save the changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c2428-155">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c2428-155">Additional Resources</span></span>

* [<span data-ttu-id="c2428-156">Руководство. Начало работы с EF Core в ASP.NET Core с новой базой данных с использованием SQLite</span><span class="sxs-lookup"><span data-stu-id="c2428-156">Tutorial: Get started with EF Core on ASP.NET Core with a new database using SQLite</span></span>](xref:core/get-started/aspnetcore/new-db)
* [<span data-ttu-id="c2428-157">Руководство. Начало работы с Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c2428-157">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="c2428-158">Руководство. Razor Pages с Entity Framework Core в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c2428-158">Tutorial: Razor Pages with Entity Framework Core in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
