---
title: Начало работы в .NET Core — новая база данных — Core EF
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Начало работы с .NET Core и Entity Framework Core
keywords: .NET Core, Entity Framework Core, Visual Studio Code, VS Code, Mac, Linux
ms.date: 04/05/2017
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 2511dfa3f3262bb12c2058dc1c402b7dcc4c670d
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2018
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="ed6fc-104">Начало работы с EF Core в консольном приложении .NET Core с новой базой данных</span><span class="sxs-lookup"><span data-stu-id="ed6fc-104">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="ed6fc-105">В этом пошаговом руководстве вы создадите консольное приложение .NET Core, которое осуществляет базовые операции доступа к базе данных SQLite с помощью Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ed6fc-105">In this walkthrough, you will create a .NET Core console app that performs basic data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="ed6fc-106">Вы примените процесс миграций, чтобы создать базу данных из модели.</span><span class="sxs-lookup"><span data-stu-id="ed6fc-106">You will use migrations to create the database from your model.</span></span> <span data-ttu-id="ed6fc-107">В разделе [ASP.NET Core - новая база данных](xref:core/get-started/aspnetcore/new-db) описана версия для Visual Studio с использованием ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="ed6fc-107">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

> [!TIP]  
> <span data-ttu-id="ed6fc-108">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="ed6fc-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ed6fc-109">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="ed6fc-109">Prerequisites</span></span>

<span data-ttu-id="ed6fc-110">Для прохождения этого пошагового руководства нужны следующие элементы:</span><span class="sxs-lookup"><span data-stu-id="ed6fc-110">The following prerequisites are needed to complete this walkthrough:</span></span>
* <span data-ttu-id="ed6fc-111">операционная система, которая поддерживает .NET Core;</span><span class="sxs-lookup"><span data-stu-id="ed6fc-111">An operating system that supports .NET Core.</span></span>
* <span data-ttu-id="ed6fc-112">[пакет SDK для .NET Core](https://www.microsoft.com/net/core) 2.0 (но эти же инструкции с очень небольшими изменениями позволят создать приложение и в предыдущей версии).</span><span class="sxs-lookup"><span data-stu-id="ed6fc-112">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.0 (although the instructions can be used to create an application with a previous version with very few modifications).</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="ed6fc-113">Создание нового проекта</span><span class="sxs-lookup"><span data-stu-id="ed6fc-113">Create a new project</span></span>

* <span data-ttu-id="ed6fc-114">Создайте для проекта новую папку `ConsoleApp.SQLite` и с помощью команды `dotnet` заполните ее приложением .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ed6fc-114">Create a new `ConsoleApp.SQLite` folder for your project and use the `dotnet` command to populate it with a .NET Core app.</span></span>

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a><span data-ttu-id="ed6fc-115">Установка Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="ed6fc-115">Install Entity Framework Core</span></span>

<span data-ttu-id="ed6fc-116">Чтобы использовать EF Core, установите пакеты для поставщиков базы данных, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="ed6fc-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="ed6fc-117">В этом пошаговом руководстве используется SQLite.</span><span class="sxs-lookup"><span data-stu-id="ed6fc-117">This walkthrough uses SQLite.</span></span> <span data-ttu-id="ed6fc-118">Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).</span><span class="sxs-lookup"><span data-stu-id="ed6fc-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="ed6fc-119">Установите Microsoft.EntityFrameworkCore.Sqlite и Microsoft.EntityFrameworkCore.Design:</span><span class="sxs-lookup"><span data-stu-id="ed6fc-119">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* <span data-ttu-id="ed6fc-120">Вручную измените `ConsoleApp.SQLite.csproj`, добавив значение DotNetCliToolReference в Microsoft.EntityFrameworkCore.Tools.DotNet:</span><span class="sxs-lookup"><span data-stu-id="ed6fc-120">Manually edit `ConsoleApp.SQLite.csproj` to add a DotNetCliToolReference to Microsoft.EntityFrameworkCore.Tools.DotNet:</span></span>

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

<span data-ttu-id="ed6fc-121">Теперь `ConsoleApp.SQLite.csproj` должен содержать следующее:</span><span class="sxs-lookup"><span data-stu-id="ed6fc-121">`ConsoleApp.SQLite.csproj` should now contain the following:</span></span>

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 <span data-ttu-id="ed6fc-122">Примечание. Приведенные выше номера версий были правильными на момент публикации.</span><span class="sxs-lookup"><span data-stu-id="ed6fc-122">Note: The version numbers used above were correct at the time of publishing.</span></span>

*  <span data-ttu-id="ed6fc-123">Выполните `dotnet restore`, чтобы установить новые пакеты.</span><span class="sxs-lookup"><span data-stu-id="ed6fc-123">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="ed6fc-124">Создание модели</span><span class="sxs-lookup"><span data-stu-id="ed6fc-124">Create the model</span></span>

<span data-ttu-id="ed6fc-125">Определите контекст и классы сущности, которые должны входить в модель:</span><span class="sxs-lookup"><span data-stu-id="ed6fc-125">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="ed6fc-126">Создайте новый файл *Model.cs* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="ed6fc-126">Create a new *Model.cs* file with the following contents.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="ed6fc-127">Подсказка. В реальном приложении каждый класс помещается в отдельный файл, а строка подключения — в файл конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ed6fc-127">Tip: In a real application you would put each class in a separate file and put the connection string in a configuration file.</span></span> <span data-ttu-id="ed6fc-128">Для упрощения в этом руководстве мы заносим все данные в один файл.</span><span class="sxs-lookup"><span data-stu-id="ed6fc-128">To keep the tutorial simple, we are putting everything in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="ed6fc-129">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="ed6fc-129">Create the database</span></span>

<span data-ttu-id="ed6fc-130">Когда модель будет готова, из нее можно создать базу данных с помощью [миграций](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations):</span><span class="sxs-lookup"><span data-stu-id="ed6fc-130">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="ed6fc-131">Запустите `dotnet ef migrations add InitialCreate`, чтобы сформировать шаблон миграции и создать начальный набор таблиц для модели.</span><span class="sxs-lookup"><span data-stu-id="ed6fc-131">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="ed6fc-132">Запустите `dotnet ef database update`, чтобы применить созданную миграцию к базе данных.</span><span class="sxs-lookup"><span data-stu-id="ed6fc-132">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="ed6fc-133">Эта команда создает базу данных и применяет к ней миграции.</span><span class="sxs-lookup"><span data-stu-id="ed6fc-133">This command creates the database before applying migrations.</span></span>

> [!NOTE]  
> <span data-ttu-id="ed6fc-134">Если для SQLite применяются относительные пути, они учитываются относительно главной сборки приложения.</span><span class="sxs-lookup"><span data-stu-id="ed6fc-134">When using relative paths with SQLite, the path will be relative to the application's main assembly.</span></span> <span data-ttu-id="ed6fc-135">В этом примере основным двоичным файлом является `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, а значит база данных SQLite расположена в папке `bin/Debug/netcoreapp2.0/blogging.db`.</span><span class="sxs-lookup"><span data-stu-id="ed6fc-135">In this sample, the main binary is `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, so the SQLite database will be in `bin/Debug/netcoreapp2.0/blogging.db`.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="ed6fc-136">Использование модели</span><span class="sxs-lookup"><span data-stu-id="ed6fc-136">Use your model</span></span>

* <span data-ttu-id="ed6fc-137">Откройте файл *Program.cs* и замените его содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="ed6fc-137">Open *Program.cs* and replace the contents with the following code:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="ed6fc-138">Проверьте работу приложения:</span><span class="sxs-lookup"><span data-stu-id="ed6fc-138">Test the app:</span></span>

 `dotnet run`

 <span data-ttu-id="ed6fc-139">В базе данных будет создан один блог, а затем в консоли появятся сведения обо всех блогах.</span><span class="sxs-lookup"><span data-stu-id="ed6fc-139">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="ed6fc-140">Изменение модели</span><span class="sxs-lookup"><span data-stu-id="ed6fc-140">Changing the model:</span></span>

- <span data-ttu-id="ed6fc-141">Если вы внесете изменения в эту модель, с помощью команды `dotnet ef migrations add` вы сможете сформировать новую [миграцию](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations), которая вносит необходимые изменения в схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="ed6fc-141">If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)  to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="ed6fc-142">Проверьте сформированный код шаблона (и внесите правки, если потребуется), а затем выполните команду `dotnet ef database update`, чтобы применить изменения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="ed6fc-142">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.</span></span>
- <span data-ttu-id="ed6fc-143">EF использует таблицу `__EFMigrationsHistory` в базе данных, чтобы отслеживать уже примененные к базе данных миграции.</span><span class="sxs-lookup"><span data-stu-id="ed6fc-143">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="ed6fc-144">SQLite поддерживает не все миграции (изменения схемы) из-за собственных ограничений SQLite.</span><span class="sxs-lookup"><span data-stu-id="ed6fc-144">SQLite does not support all migrations (schema changes) due to limitations in SQLite.</span></span> <span data-ttu-id="ed6fc-145">Подробнее о них можно узнать [в этой статье](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="ed6fc-145">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="ed6fc-146">Если вы разрабатываете новое приложение, лучше не использовать миграции для изменения модели, а просто удалить старую базу данных и создать новую.</span><span class="sxs-lookup"><span data-stu-id="ed6fc-146">For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ed6fc-147">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ed6fc-147">Additional Resources</span></span>

* <span data-ttu-id="ed6fc-148">[Руководство по применению кроссплатформенной консоли EF с .NET Core и новой базой данных SQLite](xref:core/get-started/netcore/new-db-sqlite)</span><span class="sxs-lookup"><span data-stu-id="ed6fc-148">[.NET Core - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="ed6fc-149">Введение в ASP.NET Core MVC для Mac и Linux</span><span class="sxs-lookup"><span data-stu-id="ed6fc-149">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="ed6fc-150">Введение в ASP.NET Core MVC для Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ed6fc-150">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="ed6fc-151">Начало работы с ASP.NET Core и Entity Framework Core с использованием Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ed6fc-151">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
