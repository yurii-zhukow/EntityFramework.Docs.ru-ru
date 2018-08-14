---
title: Начало работы в .NET Framework — существующая база данных — EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/06/2018
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: d5c548927b736199c7d6fddc9c74139ca5f6614e
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614419"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="7d6fc-102">Начало работы с EF Core в .NET Framework с существующей базой данных</span><span class="sxs-lookup"><span data-stu-id="7d6fc-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="7d6fc-103">В этом руководстве вы создадите консольное приложение, которое реализует простейший доступ к базе данных Microsoft SQL Server с помощью Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="7d6fc-104">Для создания модели Entity Framework используется реконструирование существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-104">You create an Entity Framework model by reverse engineering an existing database.</span></span>

<span data-ttu-id="7d6fc-105">[Пример для этой статьи на GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="7d6fc-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7d6fc-106">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="7d6fc-106">Prerequisites</span></span>

* [<span data-ttu-id="7d6fc-107">Visual Studio 2017 версии 15.7 или выше</span><span class="sxs-lookup"><span data-stu-id="7d6fc-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-blogging-database"></a><span data-ttu-id="7d6fc-108">Создание базы данных Blogging</span><span class="sxs-lookup"><span data-stu-id="7d6fc-108">Create Blogging database</span></span>

<span data-ttu-id="7d6fc-109">В этом руководстве в качестве существующей базы данных используется база **Blogging** для ведения блогов, размещенная на локальном экземпляре LocalDb.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-109">This tutorial uses a **Blogging** database on the LocalDb instance as the existing database.</span></span> <span data-ttu-id="7d6fc-110">Если вы уже создали базу данных **Blogging**, работая с другим руководством, пропустите эти шаги.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-110">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="7d6fc-111">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7d6fc-111">Open Visual Studio</span></span>

* <span data-ttu-id="7d6fc-112">**"Сервис" > "Подключение к базе данных"…**</span><span class="sxs-lookup"><span data-stu-id="7d6fc-112">**Tools > Connect to Database...**</span></span>

* <span data-ttu-id="7d6fc-113">Выберите **Microsoft SQL Server** и щелкните **Продолжить**.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-113">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="7d6fc-114">Введите значение **(localdb)\mssqllocaldb** для параметра **Имя сервера**.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-114">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="7d6fc-115">Введите значение **master** для параметра **Имя базы данных**, затем щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-115">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="7d6fc-116">Теперь база данных master появится в разделе **Подключения к данным** в **обозревателе сервера**.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-116">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="7d6fc-117">Щелкните правой кнопкой мыши базу данных в **обозревателе сервера** и выберите действие **Создать запрос**.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-117">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="7d6fc-118">Скопируйте представленный ниже скрипт и вставьте его в редактор запросов</span><span class="sxs-lookup"><span data-stu-id="7d6fc-118">Copy the script listed below into the query editor</span></span>

* <span data-ttu-id="7d6fc-119">Щелкните область редактора запросов правой кнопкой мыши и выберите действие **Выполнить**.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-119">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="7d6fc-120">Создание нового проекта</span><span class="sxs-lookup"><span data-stu-id="7d6fc-120">Create a new project</span></span>

* <span data-ttu-id="7d6fc-121">Откройте Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-121">Open Visual Studio 2017</span></span>

* <span data-ttu-id="7d6fc-122">**"Файл" > "Создать" > "Проект"…**</span><span class="sxs-lookup"><span data-stu-id="7d6fc-122">**File > New > Project...**</span></span>

* <span data-ttu-id="7d6fc-123">В меню слева выберите **"Установленные" > Visual C# > Классическое приложение Windows**</span><span class="sxs-lookup"><span data-stu-id="7d6fc-123">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="7d6fc-124">Выберите шаблон проекта **Консольное приложение (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-124">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="7d6fc-125">Задайте **.NET Framework 4.6.1** или более позднюю версию в качестве целевой платформы проекта</span><span class="sxs-lookup"><span data-stu-id="7d6fc-125">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="7d6fc-126">Назовите проект *ConsoleApp.ExistingDb* и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="7d6fc-126">Name the project *ConsoleApp.ExistingDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="7d6fc-127">Установка Entity Framework</span><span class="sxs-lookup"><span data-stu-id="7d6fc-127">Install Entity Framework</span></span>

<span data-ttu-id="7d6fc-128">Чтобы использовать EF Core, установите пакеты для поставщиков базы данных, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-128">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="7d6fc-129">В этом руководстве используется SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-129">This tutorial uses SQL Server.</span></span> <span data-ttu-id="7d6fc-130">Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).</span><span class="sxs-lookup"><span data-stu-id="7d6fc-130">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="7d6fc-131">Последовательно выберите пункты **Средства > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-131">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="7d6fc-132">Запуск `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="7d6fc-132">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="7d6fc-133">На следующем этапе вы воспользуетесь рядом инструментов Entity Framework Tools для реконструирования базы данных.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-133">In the next step, you use some Entity Framework Tools to reverse engineer the database.</span></span> <span data-ttu-id="7d6fc-134">Поэтому также установите пакет инструментов.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-134">So install the tools package as well.</span></span>

* <span data-ttu-id="7d6fc-135">Запуск `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="7d6fc-135">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-the-model"></a><span data-ttu-id="7d6fc-136">Реконструирование модели</span><span class="sxs-lookup"><span data-stu-id="7d6fc-136">Reverse engineer the model</span></span>

<span data-ttu-id="7d6fc-137">Теперь пора создать модель EF на основе существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-137">Now it's time to create the EF model based on an existing database.</span></span>

* <span data-ttu-id="7d6fc-138">Последовательно выберите пункты **Средства -> Диспетчер пакетов NuGet -> Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-138">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>

* <span data-ttu-id="7d6fc-139">Выполните следующую команду, чтобы создать модель на основе существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-139">Run the following command to create a model from the existing database</span></span>

  ``` powershell
  Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
  ```

> [!TIP]  
> <span data-ttu-id="7d6fc-140">Вы можете выбрать, для каких таблиц создавать сущности, указав в команде выше аргумент `-Tables`.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-140">You can specify the tables to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="7d6fc-141">Например, `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-141">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="7d6fc-142">Процесс реконструирования создает классы сущностей (`Blog` и `Post`) и производный контекст (`BloggingContext`) на основе схемы существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-142">The reverse engineer process created entity classes (`Blog` and `Post`) and a derived context (`BloggingContext`) based on the schema of the existing database.</span></span>

<span data-ttu-id="7d6fc-143">Классы сущностей — это простые объекты C#, которые представляют данные для использования в запросах и командах сохранения.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-143">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="7d6fc-144">Ниже приведены классы сущностей `Blog` и `Post`:</span><span class="sxs-lookup"><span data-stu-id="7d6fc-144">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Post.cs)]

> [!TIP]  
> <span data-ttu-id="7d6fc-145">Чтобы включить отложенную загрузку, можно присвоить свойствам навигации (Blog.Post и Post.Blog) значение `virtual`.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-145">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

<span data-ttu-id="7d6fc-146">Контекст представляет сеанс работы с базой данных.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-146">The context represents a session with the database.</span></span> <span data-ttu-id="7d6fc-147">Он содержит методы, которые можно использовать для запроса и сохранения экземпляров классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-147">It has methods that you can use to query and save instances of the entity classes.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)]

## <a name="use-the-model"></a><span data-ttu-id="7d6fc-148">Использование модели</span><span class="sxs-lookup"><span data-stu-id="7d6fc-148">Use the model</span></span>

<span data-ttu-id="7d6fc-149">Теперь вы можете использовать созданную модель для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-149">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="7d6fc-150">Откройте файл *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-150">Open *Program.cs*</span></span>

* <span data-ttu-id="7d6fc-151">Замените все содержимое этого файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="7d6fc-151">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] 

* <span data-ttu-id="7d6fc-152">Выберите "Отладка" -> "Запустить без отладки".</span><span class="sxs-lookup"><span data-stu-id="7d6fc-152">Debug > Start Without Debugging</span></span>

  <span data-ttu-id="7d6fc-153">Вы увидите, как один блог сохраняется в базе данных, а затем сведения обо всех блогах выводятся в консоль.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-153">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![изображение](_static/output-existing-db.png)

## <a name="additional-resources"></a><span data-ttu-id="7d6fc-155">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="7d6fc-155">Additional Resources</span></span>

* [<span data-ttu-id="7d6fc-156">EF Core в .NET Framework с новой базой данных</span><span class="sxs-lookup"><span data-stu-id="7d6fc-156">EF Core on .NET Framework with a new database</span></span>](xref:core/get-started/full-dotnet/new-db)
* <span data-ttu-id="7d6fc-157">[EF Core в .NET Core с новой базой данных — SQLite](xref:core/get-started/netcore/new-db-sqlite). Руководство по кроссплатформенной консоли EF.</span><span class="sxs-lookup"><span data-stu-id="7d6fc-157">[EF Core on .NET Core with a new database - SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
