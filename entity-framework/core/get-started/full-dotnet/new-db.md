---
title: Начало работы в .NET Framework — новая база данных — Core EF
author: rowanmiller
ms.date: 08/06/2018
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: 66d9011a5978fc3d8253a963bdb9df27848e9ff9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997591"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a><span data-ttu-id="78eba-102">Начало работы с EF Core в .NET Framework с новой базой данных</span><span class="sxs-lookup"><span data-stu-id="78eba-102">Getting started with EF Core on .NET Framework with a New Database</span></span>

<span data-ttu-id="78eba-103">В этом руководстве вы создадите консольное приложение, которое реализует простейший доступ к базе данных Microsoft SQL Server с помощью Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="78eba-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="78eba-104">Вы создадите из модели базу данных при помощи миграций.</span><span class="sxs-lookup"><span data-stu-id="78eba-104">You use migrations to create the database from a model.</span></span>

<span data-ttu-id="78eba-105">[Пример для этой статьи на GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).</span><span class="sxs-lookup"><span data-stu-id="78eba-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="78eba-106">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="78eba-106">Prerequisites</span></span>

* [<span data-ttu-id="78eba-107">Visual Studio 2017 версии 15.7 или выше</span><span class="sxs-lookup"><span data-stu-id="78eba-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-a-new-project"></a><span data-ttu-id="78eba-108">Создание нового проекта</span><span class="sxs-lookup"><span data-stu-id="78eba-108">Create a new project</span></span>

* <span data-ttu-id="78eba-109">Откройте Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="78eba-109">Open Visual Studio 2017</span></span>

* <span data-ttu-id="78eba-110">**"Файл" > "Создать" > "Проект"…**</span><span class="sxs-lookup"><span data-stu-id="78eba-110">**File > New > Project...**</span></span>

* <span data-ttu-id="78eba-111">В меню слева выберите **"Установленные" > Visual C# > Классическое приложение Windows**</span><span class="sxs-lookup"><span data-stu-id="78eba-111">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="78eba-112">Выберите шаблон проекта **Консольное приложение (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="78eba-112">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="78eba-113">Задайте **.NET Framework 4.6.1** или более позднюю версию в качестве целевой платформы проекта</span><span class="sxs-lookup"><span data-stu-id="78eba-113">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="78eba-114">Назовите проект *ConsoleApp.NewDb* и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="78eba-114">Name the project *ConsoleApp.NewDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="78eba-115">Установка Entity Framework</span><span class="sxs-lookup"><span data-stu-id="78eba-115">Install Entity Framework</span></span>

<span data-ttu-id="78eba-116">Чтобы использовать EF Core, установите пакеты для поставщиков базы данных, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="78eba-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="78eba-117">В этом руководстве используется SQL Server.</span><span class="sxs-lookup"><span data-stu-id="78eba-117">This tutorial uses SQL Server.</span></span> <span data-ttu-id="78eba-118">Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).</span><span class="sxs-lookup"><span data-stu-id="78eba-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="78eba-119">Последовательно выберите пункты "Средства" -> "Диспетчер пакетов NuGet" -> "Консоль диспетчера пакетов".</span><span class="sxs-lookup"><span data-stu-id="78eba-119">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="78eba-120">Запуск `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="78eba-120">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="78eba-121">Далее в этом руководстве вы воспользуетесь рядом инструментов Entity Framework Tools для обслуживания базы данных.</span><span class="sxs-lookup"><span data-stu-id="78eba-121">Later in this tutorial you use some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="78eba-122">Поэтому также установите пакет инструментов.</span><span class="sxs-lookup"><span data-stu-id="78eba-122">So install the tools package as well.</span></span>

* <span data-ttu-id="78eba-123">Запуск `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="78eba-123">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="78eba-124">Создание модели</span><span class="sxs-lookup"><span data-stu-id="78eba-124">Create the model</span></span>

<span data-ttu-id="78eba-125">Теперь нужно задать контекст и классы сущностей, которые составляют модель.</span><span class="sxs-lookup"><span data-stu-id="78eba-125">Now it's time to define a context and entity classes that make up the model.</span></span>

* <span data-ttu-id="78eba-126">**"Проект" > "Добавить класс"…**</span><span class="sxs-lookup"><span data-stu-id="78eba-126">**Project > Add Class...**</span></span>

* <span data-ttu-id="78eba-127">Введите имя класса *Model.cs* и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="78eba-127">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="78eba-128">Замените все содержимое этого файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="78eba-128">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] 

> [!TIP]  
> <span data-ttu-id="78eba-129">В реальном приложении каждый класс помещается в отдельный файл, а строка подключения — в файл конфигурации или переменную среды.</span><span class="sxs-lookup"><span data-stu-id="78eba-129">In a real application you would put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="78eba-130">В этом руководстве для простоты все занесено в один файл кода.</span><span class="sxs-lookup"><span data-stu-id="78eba-130">For the sake of simplicity, everything is in a single code file for this tutorial.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="78eba-131">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="78eba-131">Create the database</span></span>

<span data-ttu-id="78eba-132">Итак, модель готова, и вы можете создать из нее базу данных с помощью миграций.</span><span class="sxs-lookup"><span data-stu-id="78eba-132">Now that you have a model, you can use migrations to create a database.</span></span>

* <span data-ttu-id="78eba-133">Последовательно выберите пункты **Средства > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="78eba-133">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="78eba-134">Выполните `Add-Migration InitialCreate`, чтобы сформировать шаблон миграции и создать начальный набор таблиц для модели.</span><span class="sxs-lookup"><span data-stu-id="78eba-134">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for the model.</span></span>

* <span data-ttu-id="78eba-135">Запустите `Update-Database`, чтобы применить созданную миграцию к базе данных.</span><span class="sxs-lookup"><span data-stu-id="78eba-135">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="78eba-136">Так как база данных еще не существует, она будет создана перед выполнением миграции.</span><span class="sxs-lookup"><span data-stu-id="78eba-136">Because the database doesn't exist yet, it will be created before the migration is applied.</span></span>

> [!TIP]  
> <span data-ttu-id="78eba-137">Если вы внесете в модель изменения, с помощью команды `Add-Migration` вы сможете сформировать новый шаблон миграции, который вносит необходимые изменения в схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="78eba-137">If you make changes to the model, you can use the `Add-Migration` command to scaffold a new migration to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="78eba-138">Проверьте сформированный код шаблона (и внесите правки, если потребуется), а затем выполните команду `Update-Database`, чтобы применить изменения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="78eba-138">Once you have checked the scaffolded code (and made any required changes), you can use the `Update-Database` command to apply the changes to the database.</span></span>
>
> <span data-ttu-id="78eba-139">EF использует таблицу `__EFMigrationsHistory` в базе данных, чтобы отслеживать уже примененные к базе данных миграции.</span><span class="sxs-lookup"><span data-stu-id="78eba-139">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="78eba-140">Использование модели</span><span class="sxs-lookup"><span data-stu-id="78eba-140">Use the model</span></span>

<span data-ttu-id="78eba-141">Теперь вы можете использовать созданную модель для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="78eba-141">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="78eba-142">Откройте файл *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="78eba-142">Open *Program.cs*</span></span>

* <span data-ttu-id="78eba-143">Замените все содержимое этого файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="78eba-143">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)]

* <span data-ttu-id="78eba-144">**"Отладка" > "Запустить без отладки"**</span><span class="sxs-lookup"><span data-stu-id="78eba-144">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="78eba-145">Вы увидите, как один блог сохраняется в базе данных, а затем сведения обо всех блогах выводятся в консоль.</span><span class="sxs-lookup"><span data-stu-id="78eba-145">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![изображение](_static/output-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="78eba-147">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="78eba-147">Additional Resources</span></span>

* [<span data-ttu-id="78eba-148">EF Core в .NET Framework с существующей базой данных</span><span class="sxs-lookup"><span data-stu-id="78eba-148">EF Core on .NET Framework with an existing database</span></span>](xref:core/get-started/full-dotnet/existing-db)
* <span data-ttu-id="78eba-149">[EF Core в .NET Core с новой базой данных — SQLite](xref:core/get-started/netcore/new-db-sqlite). Руководство по кроссплатформенной консоли EF.</span><span class="sxs-lookup"><span data-stu-id="78eba-149">[EF Core on .NET Core with a new database - SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
