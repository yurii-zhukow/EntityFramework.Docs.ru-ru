---
title: Начало работы в .NET Core — новая база данных — Core EF
author: rick-anderson
ms.author: riande
description: Начало работы с .NET Core и Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 51f5752eebce5603c663072f7b36dfecd4ddf227
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993696"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="d06a0-103">Начало работы с EF Core в консольном приложении .NET Core с новой базой данных</span><span class="sxs-lookup"><span data-stu-id="d06a0-103">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="d06a0-104">В этом руководстве вы создадите консольное приложение .NET Core, которое осуществляет доступ к базе данных SQLite с помощью Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="d06a0-104">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="d06a0-105">Вы примените процесс миграций, чтобы создать базу данных из модели.</span><span class="sxs-lookup"><span data-stu-id="d06a0-105">You use migrations to create the database from the model.</span></span> <span data-ttu-id="d06a0-106">В разделе [ASP.NET Core - новая база данных](xref:core/get-started/aspnetcore/new-db) описана версия для Visual Studio с использованием ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="d06a0-106">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

<span data-ttu-id="d06a0-107">Пример для этой статьи на GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span><span class="sxs-lookup"><span data-stu-id="d06a0-107">View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d06a0-108">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="d06a0-108">Prerequisites</span></span>

* [<span data-ttu-id="d06a0-109">Пакет SDK для NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="d06a0-109">The .NET Core 2.1 SDK</span></span>](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a><span data-ttu-id="d06a0-110">Создание нового проекта</span><span class="sxs-lookup"><span data-stu-id="d06a0-110">Create a new project</span></span>

* <span data-ttu-id="d06a0-111">Создание консольного проекта:</span><span class="sxs-lookup"><span data-stu-id="d06a0-111">Create a new console project:</span></span>

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  cd ConsoleApp.SQLite/
  ```

## <a name="install-entity-framework-core"></a><span data-ttu-id="d06a0-112">Установка Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d06a0-112">Install Entity Framework Core</span></span>

<span data-ttu-id="d06a0-113">Чтобы использовать EF Core, установите пакеты для поставщиков базы данных, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="d06a0-113">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="d06a0-114">В этом пошаговом руководстве используется SQLite.</span><span class="sxs-lookup"><span data-stu-id="d06a0-114">This walkthrough uses SQLite.</span></span> <span data-ttu-id="d06a0-115">Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).</span><span class="sxs-lookup"><span data-stu-id="d06a0-115">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="d06a0-116">Установите Microsoft.EntityFrameworkCore.Sqlite и Microsoft.EntityFrameworkCore.Design:</span><span class="sxs-lookup"><span data-stu-id="d06a0-116">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* <span data-ttu-id="d06a0-117">Выполните `dotnet restore`, чтобы установить новые пакеты.</span><span class="sxs-lookup"><span data-stu-id="d06a0-117">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="d06a0-118">Создание модели</span><span class="sxs-lookup"><span data-stu-id="d06a0-118">Create the model</span></span>

<span data-ttu-id="d06a0-119">Определите контекст и классы сущности, которые должны входить в модель:</span><span class="sxs-lookup"><span data-stu-id="d06a0-119">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="d06a0-120">Создайте новый файл *Model.cs* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="d06a0-120">Create a new *Model.cs* file with the following contents.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="d06a0-121">Совет. В реальном приложении каждый класс помещается в отдельный файл, а строка подключения — в файл конфигурации или переменную среды.</span><span class="sxs-lookup"><span data-stu-id="d06a0-121">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="d06a0-122">Для упрощения в этом руководстве все данные содержатся в одном файле.</span><span class="sxs-lookup"><span data-stu-id="d06a0-122">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="d06a0-123">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="d06a0-123">Create the database</span></span>

<span data-ttu-id="d06a0-124">Когда модель будет готова, создайте базу данных с помощью [миграций](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="d06a0-124">Once you have a model, you use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="d06a0-125">Запустите `dotnet ef migrations add InitialCreate`, чтобы сформировать шаблон миграции и создать начальный набор таблиц для модели.</span><span class="sxs-lookup"><span data-stu-id="d06a0-125">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="d06a0-126">Запустите `dotnet ef database update`, чтобы применить созданную миграцию к базе данных.</span><span class="sxs-lookup"><span data-stu-id="d06a0-126">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="d06a0-127">Эта команда создает базу данных и применяет к ней миграции.</span><span class="sxs-lookup"><span data-stu-id="d06a0-127">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="d06a0-128">База данных SQLite *blogging.db* \* находится в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="d06a0-128">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="d06a0-129">Использование модели</span><span class="sxs-lookup"><span data-stu-id="d06a0-129">Use the model</span></span>

* <span data-ttu-id="d06a0-130">Откройте файл *Program.cs* и замените его содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="d06a0-130">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="d06a0-131">Проверьте работу приложения:</span><span class="sxs-lookup"><span data-stu-id="d06a0-131">Test the app:</span></span>

  `dotnet run`

  <span data-ttu-id="d06a0-132">В базе данных будет создан один блог, а затем в консоли появятся сведения обо всех блогах.</span><span class="sxs-lookup"><span data-stu-id="d06a0-132">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="d06a0-133">Изменение модели</span><span class="sxs-lookup"><span data-stu-id="d06a0-133">Changing the model:</span></span>

- <span data-ttu-id="d06a0-134">Если вы внесете в модель изменения, с помощью команды `dotnet ef migrations add` вы можете сформировать новую [миграцию](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="d06a0-134">If you make changes to the model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations).</span></span> <span data-ttu-id="d06a0-135">Проверьте сформированный код (и внесите необходимые правки), а затем используйте команду `dotnet ef database update`, чтобы применить изменения схемы к базе данных.</span><span class="sxs-lookup"><span data-stu-id="d06a0-135">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the schema changes to the database.</span></span>
- <span data-ttu-id="d06a0-136">EF Core использует таблицу `__EFMigrationsHistory` в базе данных для отслеживания уже выполнявшихся в ней миграций.</span><span class="sxs-lookup"><span data-stu-id="d06a0-136">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="d06a0-137">Ядро СУБД SQLite не поддерживает некоторые изменения схем, поддерживаемые большинством других реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="d06a0-137">The SQLite database engine doesn't support certain schema changes that are supported by most other relational databases.</span></span> <span data-ttu-id="d06a0-138">Например, не поддерживается операция `DropColumn`.</span><span class="sxs-lookup"><span data-stu-id="d06a0-138">For example, the `DropColumn` operation is not supported.</span></span> <span data-ttu-id="d06a0-139">При миграциях EF Core будет создан код для этих операций.</span><span class="sxs-lookup"><span data-stu-id="d06a0-139">EF Core Migrations will generate code for these operations.</span></span> <span data-ttu-id="d06a0-140">Но если вы попробуете применить их к базе данных или сгенерировать скрипт, EF Core будет выдавать исключения.</span><span class="sxs-lookup"><span data-stu-id="d06a0-140">But if you try to apply them to a database or generate a script, EF Core throws exceptions.</span></span> <span data-ttu-id="d06a0-141">Подробнее о них можно узнать [в этой статье](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="d06a0-141">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="d06a0-142">Если вы разрабатываете новое приложение, при изменении модели лучше не использовать миграции, а просто отказаться от старой базы данных и создать новую.</span><span class="sxs-lookup"><span data-stu-id="d06a0-142">For new development, consider dropping the database and creating a new one rather than using migrations when the model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d06a0-143">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d06a0-143">Additional Resources</span></span>

* [<span data-ttu-id="d06a0-144">Введение в ASP.NET Core MVC для Mac и Linux</span><span class="sxs-lookup"><span data-stu-id="d06a0-144">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="d06a0-145">Введение в ASP.NET Core MVC для Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d06a0-145">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="d06a0-146">Начало работы с ASP.NET Core и Entity Framework Core с использованием Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d06a0-146">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
