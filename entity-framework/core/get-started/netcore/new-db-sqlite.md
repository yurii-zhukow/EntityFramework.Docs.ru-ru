---
title: Начало работы в .NET Core — новая база данных — Core EF
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Начало работы с .NET Core и Entity Framework Core
keywords: .NET Core, Entity Framework Core, Visual Studio Code, VS Code, Mac, Linux
ms.date: 06/05/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: e4eafed037325237345efbc3d7d42b32270a54e3
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911506"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="82c65-104">Начало работы с EF Core в консольном приложении .NET Core с новой базой данных</span><span class="sxs-lookup"><span data-stu-id="82c65-104">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="82c65-105">В этом пошаговом руководстве вы создадите консольное приложение .NET Core, которое осуществляет операции доступа к базе данных SQLite с помощью Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="82c65-105">In this walkthrough, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="82c65-106">Вы примените процесс миграций, чтобы создать базу данных из модели.</span><span class="sxs-lookup"><span data-stu-id="82c65-106">You use migrations to create the database from the model.</span></span> <span data-ttu-id="82c65-107">В разделе [ASP.NET Core - новая база данных](xref:core/get-started/aspnetcore/new-db) описана версия для Visual Studio с использованием ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="82c65-107">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

> [!TIP]  
> <span data-ttu-id="82c65-108">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="82c65-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="82c65-109">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="82c65-109">Prerequisites</span></span>

<span data-ttu-id="82c65-110">[Пакет SDK для .NET Core](https://www.microsoft.com/net/core) 2.1</span><span class="sxs-lookup"><span data-stu-id="82c65-110">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.1</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="82c65-111">Создание нового проекта</span><span class="sxs-lookup"><span data-stu-id="82c65-111">Create a new project</span></span>

* <span data-ttu-id="82c65-112">Создание консольного проекта:</span><span class="sxs-lookup"><span data-stu-id="82c65-112">Create a new console project:</span></span>

``` Console
dotnet new console -o ConsoleApp.SQLite
cd ConsoleApp.SQLite/
```

## <a name="install-entity-framework-core"></a><span data-ttu-id="82c65-113">Установка Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="82c65-113">Install Entity Framework Core</span></span>

<span data-ttu-id="82c65-114">Чтобы использовать EF Core, установите пакеты для поставщиков базы данных, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="82c65-114">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="82c65-115">В этом пошаговом руководстве используется SQLite.</span><span class="sxs-lookup"><span data-stu-id="82c65-115">This walkthrough uses SQLite.</span></span> <span data-ttu-id="82c65-116">Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).</span><span class="sxs-lookup"><span data-stu-id="82c65-116">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="82c65-117">Установите Microsoft.EntityFrameworkCore.Sqlite и Microsoft.EntityFrameworkCore.Design:</span><span class="sxs-lookup"><span data-stu-id="82c65-117">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* <span data-ttu-id="82c65-118">Выполните `dotnet restore`, чтобы установить новые пакеты.</span><span class="sxs-lookup"><span data-stu-id="82c65-118">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="82c65-119">Создание модели</span><span class="sxs-lookup"><span data-stu-id="82c65-119">Create the model</span></span>

<span data-ttu-id="82c65-120">Определите контекст и классы сущности, которые должны входить в модель:</span><span class="sxs-lookup"><span data-stu-id="82c65-120">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="82c65-121">Создайте новый файл *Model.cs* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="82c65-121">Create a new *Model.cs* file with the following contents.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="82c65-122">Подсказка. В реальном приложении каждый класс помещается в отдельный файл, а строка подключения — в файл конфигурации.</span><span class="sxs-lookup"><span data-stu-id="82c65-122">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file.</span></span> <span data-ttu-id="82c65-123">Для упрощения в этом руководстве все данные содержатся в одном файле.</span><span class="sxs-lookup"><span data-stu-id="82c65-123">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="82c65-124">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="82c65-124">Create the database</span></span>

<span data-ttu-id="82c65-125">Когда модель будет готова, создайте базу данных с помощью [миграций](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="82c65-125">Once you have a model, you use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="82c65-126">Запустите `dotnet ef migrations add InitialCreate`, чтобы сформировать шаблон миграции и создать начальный набор таблиц для модели.</span><span class="sxs-lookup"><span data-stu-id="82c65-126">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="82c65-127">Запустите `dotnet ef database update`, чтобы применить созданную миграцию к базе данных.</span><span class="sxs-lookup"><span data-stu-id="82c65-127">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="82c65-128">Эта команда создает базу данных и применяет к ней миграции.</span><span class="sxs-lookup"><span data-stu-id="82c65-128">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="82c65-129">База данных SQLite *blogging.db* \* находится в каталоге проекта.</span><span class="sxs-lookup"><span data-stu-id="82c65-129">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="82c65-130">Использование модели</span><span class="sxs-lookup"><span data-stu-id="82c65-130">Use your model</span></span>

* <span data-ttu-id="82c65-131">Откройте файл *Program.cs* и замените его содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="82c65-131">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="82c65-132">Проверьте работу приложения:</span><span class="sxs-lookup"><span data-stu-id="82c65-132">Test the app:</span></span>

  `dotnet run`

  <span data-ttu-id="82c65-133">В базе данных будет создан один блог, а затем в консоли появятся сведения обо всех блогах.</span><span class="sxs-lookup"><span data-stu-id="82c65-133">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="82c65-134">Изменение модели</span><span class="sxs-lookup"><span data-stu-id="82c65-134">Changing the model:</span></span>

- <span data-ttu-id="82c65-135">Если вы внесете изменения в эту модель, с помощью команды `dotnet ef migrations add` вы сможете сформировать новую [миграцию](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations), которая вносит необходимые изменения в схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="82c65-135">If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)  to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="82c65-136">Проверьте сформированный код шаблона (и внесите правки, если потребуется), а затем выполните команду `dotnet ef database update`, чтобы применить изменения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="82c65-136">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.</span></span>
- <span data-ttu-id="82c65-137">EF использует таблицу `__EFMigrationsHistory` в базе данных, чтобы отслеживать уже примененные к базе данных миграции.</span><span class="sxs-lookup"><span data-stu-id="82c65-137">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="82c65-138">SQLite поддерживает не все миграции (изменения схемы) из-за собственных ограничений SQLite.</span><span class="sxs-lookup"><span data-stu-id="82c65-138">SQLite does not support all migrations (schema changes) due to limitations in SQLite.</span></span> <span data-ttu-id="82c65-139">Подробнее о них можно узнать [в этой статье](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="82c65-139">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="82c65-140">Если вы разрабатываете новое приложение, лучше не использовать миграции для изменения модели, а просто удалить старую базу данных и создать новую.</span><span class="sxs-lookup"><span data-stu-id="82c65-140">For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="82c65-141">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="82c65-141">Additional Resources</span></span>

* <span data-ttu-id="82c65-142">[Руководство по применению кроссплатформенной консоли EF с .NET Core и новой базой данных SQLite](xref:core/get-started/netcore/new-db-sqlite)</span><span class="sxs-lookup"><span data-stu-id="82c65-142">[.NET Core - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="82c65-143">Введение в ASP.NET Core MVC для Mac и Linux</span><span class="sxs-lookup"><span data-stu-id="82c65-143">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="82c65-144">Введение в ASP.NET Core MVC для Visual Studio</span><span class="sxs-lookup"><span data-stu-id="82c65-144">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="82c65-145">Начало работы с ASP.NET Core и Entity Framework Core с использованием Visual Studio</span><span class="sxs-lookup"><span data-stu-id="82c65-145">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
