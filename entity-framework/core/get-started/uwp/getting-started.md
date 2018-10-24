---
title: Начало работы в UWP — новая база данных — Core EF
author: rowanmiller
ms.date: 10/13/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: 48d26adbe17e4734753a7ada547b9c13317bef0d
ms.sourcegitcommit: 8b42045cd21f80f425a92f5e4e9dd4972a31720b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2018
ms.locfileid: "49315624"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="4eee5-102">Начало работы с EF Core на универсальной платформе Windows (UWP) с новой базой данных</span><span class="sxs-lookup"><span data-stu-id="4eee5-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="4eee5-103">В этом руководстве вы создадите приложение универсальной платформы Windows (UWP), которое осуществляет простейший доступ к локальной базе данных SQLite с помощью Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="4eee5-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="4eee5-104">[Пример для этой статьи на GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span><span class="sxs-lookup"><span data-stu-id="4eee5-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4eee5-105">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="4eee5-105">Prerequisites</span></span>

* <span data-ttu-id="4eee5-106">[Windows 10 Fall Creators Update (10.0, сборка 16299) или более поздняя версия](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span><span class="sxs-lookup"><span data-stu-id="4eee5-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="4eee5-107">[Visual Studio 2017 версии 15.7 или более поздняя версия](https://www.visualstudio.com/downloads/) с рабочей нагрузкой **Разработка приложений для универсальной платформы Windows**.</span><span class="sxs-lookup"><span data-stu-id="4eee5-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="4eee5-108">[Пакет SDK для .NET Core 2.1](https://www.microsoft.com/net/core) или более поздняя версия.</span><span class="sxs-lookup"><span data-stu-id="4eee5-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4eee5-109">В этом руководстве используются команды [миграций](xref:core/managing-schemas/migrations/index) Entity Framework Core для создания и обновления схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="4eee5-109">This tutorial uses Entity Framework Core [migrations](xref:core/managing-schemas/migrations/index) commands to create and update the schema of the database.</span></span>
> <span data-ttu-id="4eee5-110">Эти команды не работают напрямую в проектах UWP.</span><span class="sxs-lookup"><span data-stu-id="4eee5-110">These commands don't work directly with UWP projects.</span></span>
> <span data-ttu-id="4eee5-111">По этой причине модель данных приложения помещается в общий проект библиотеки, и для выполнения команд используется отдельное консольное приложение .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4eee5-111">For this reason, the application's data model is placed in a shared library project, and a separate .NET Core console application is used to run the commands.</span></span>

## <a name="create-a-library-project-to-hold-the-data-model"></a><span data-ttu-id="4eee5-112">Создание проекта библиотеки для хранения модели данных</span><span class="sxs-lookup"><span data-stu-id="4eee5-112">Create a library project to hold the data model</span></span>

* <span data-ttu-id="4eee5-113">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4eee5-113">Open Visual Studio</span></span>

* <span data-ttu-id="4eee5-114">Последовательно выберите **Файл > Создать > Проект**.</span><span class="sxs-lookup"><span data-stu-id="4eee5-114">**File > New > Project**</span></span>

* <span data-ttu-id="4eee5-115">В меню слева выберите **"Установленные" > Visual C# > .NET Standard**.</span><span class="sxs-lookup"><span data-stu-id="4eee5-115">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="4eee5-116">Выберите шаблон **Библиотека классов (.NET Standard)**.</span><span class="sxs-lookup"><span data-stu-id="4eee5-116">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="4eee5-117">Назовите проект *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="4eee5-117">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="4eee5-118">Назовите решение *Blogging*.</span><span class="sxs-lookup"><span data-stu-id="4eee5-118">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="4eee5-119">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="4eee5-119">Click **OK**.</span></span>

## <a name="install-entity-framework-core-runtime-in-the-data-model-project"></a><span data-ttu-id="4eee5-120">Установка среды выполнения Entity Framework Core в проекте модели данных</span><span class="sxs-lookup"><span data-stu-id="4eee5-120">Install Entity Framework Core runtime in the data model project</span></span>

<span data-ttu-id="4eee5-121">Чтобы использовать EF Core, установите пакеты для поставщиков базы данных, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="4eee5-121">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="4eee5-122">В этом руководстве используется SQLite.</span><span class="sxs-lookup"><span data-stu-id="4eee5-122">This tutorial uses SQLite.</span></span> <span data-ttu-id="4eee5-123">Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).</span><span class="sxs-lookup"><span data-stu-id="4eee5-123">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="4eee5-124">Выберите **"Сервис" > "Диспетчер пакетов NuGet" > "Консоль диспетчера пакетов"**.</span><span class="sxs-lookup"><span data-stu-id="4eee5-124">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="4eee5-125">Убедитесь, что проект библиотеки *Blogging.Model* выбран в качестве **проекта по умолчанию** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="4eee5-125">Make sure that the library project *Blogging.Model* is selected as the **Default Project** in the Package Manager Console.</span></span>

* <span data-ttu-id="4eee5-126">Запуск `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="4eee5-126">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="4eee5-127">Создание модели данных</span><span class="sxs-lookup"><span data-stu-id="4eee5-127">Create the data model</span></span>

<span data-ttu-id="4eee5-128">Теперь нужно задать контекст *DbContext* и классы сущностей, которые составляют модель.</span><span class="sxs-lookup"><span data-stu-id="4eee5-128">Now it's time to define the *DbContext* and entity classes that make up the model.</span></span>

* <span data-ttu-id="4eee5-129">Удалите *Class1.cs*.</span><span class="sxs-lookup"><span data-stu-id="4eee5-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="4eee5-130">Создайте файл *Model.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="4eee5-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-console-project-to-run-migrations-commands"></a><span data-ttu-id="4eee5-131">Создание проекта консольного приложения для выполнения команд миграции</span><span class="sxs-lookup"><span data-stu-id="4eee5-131">Create a new console project to run migrations commands</span></span>

* <span data-ttu-id="4eee5-132">В **обозревателе решений** щелкните решение правой кнопкой мыши и выберите **"Добавить" > "Новый проект"**.</span><span class="sxs-lookup"><span data-stu-id="4eee5-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="4eee5-133">В меню слева выберите **"Установленные" > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4eee5-133">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="4eee5-134">Выберите шаблон проекта **Консольное приложение (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="4eee5-134">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="4eee5-135">Назовите проект *Blogging.Migrations.Startup* и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="4eee5-135">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="4eee5-136">Добавьте ссылку на проект из *Blogging.Migrations.Startup* в *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="4eee5-136">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

## <a name="install-entity-framework-core-tools-in-the-migrations-startup-project"></a><span data-ttu-id="4eee5-137">Установка инструментов Entity Framework Core в проекте запуска миграций</span><span class="sxs-lookup"><span data-stu-id="4eee5-137">Install Entity Framework Core tools in the migrations startup project</span></span>

<span data-ttu-id="4eee5-138">Чтобы включить команды миграции EF Core в консоли диспетчера пакетов, установите пакет средств EF Core в консольном приложении.</span><span class="sxs-lookup"><span data-stu-id="4eee5-138">To enable the EF Core migration commands in the Package Manager Console, install the EF Core tools package in the console application.</span></span>

* <span data-ttu-id="4eee5-139">Последовательно выберите пункты **Средства > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="4eee5-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="4eee5-140">Запуск `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`</span><span class="sxs-lookup"><span data-stu-id="4eee5-140">Run `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="4eee5-141">Создание первоначальной миграции</span><span class="sxs-lookup"><span data-stu-id="4eee5-141">Create the initial migration</span></span>

 <span data-ttu-id="4eee5-142">Создайте первоначальную миграцию, указав в качестве проекта запуска консольное приложение.</span><span class="sxs-lookup"><span data-stu-id="4eee5-142">Create the initial migration, specifying the console application as the startup project.</span></span>

* <span data-ttu-id="4eee5-143">Запуск `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`</span><span class="sxs-lookup"><span data-stu-id="4eee5-143">Run `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`</span></span>

<span data-ttu-id="4eee5-144">Эта команда формирует миграцию, которая создает начальный набор таблиц базы данных для вашей модели.</span><span class="sxs-lookup"><span data-stu-id="4eee5-144">This command scaffolds a migration that creates the initial set of database tables for your data model.</span></span>

## <a name="create-the-uwp-project"></a><span data-ttu-id="4eee5-145">Создание проекта UWP</span><span class="sxs-lookup"><span data-stu-id="4eee5-145">Create the UWP project</span></span>

* <span data-ttu-id="4eee5-146">В **обозревателе решений** щелкните решение правой кнопкой мыши и выберите **"Добавить" > "Новый проект"**.</span><span class="sxs-lookup"><span data-stu-id="4eee5-146">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="4eee5-147">В меню слева выберите **"Установленные" > Visual C# > "Универсальные приложения для Windows"**.</span><span class="sxs-lookup"><span data-stu-id="4eee5-147">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="4eee5-148">Выберите шаблон проекта **Пустое приложение (универсальное приложение для Windows)**.</span><span class="sxs-lookup"><span data-stu-id="4eee5-148">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="4eee5-149">Назовите проект *Blogging.UWP* и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="4eee5-149">Name the project *Blogging.UWP*, and click **OK**</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4eee5-150">Задайте в качестве целевой и минимальной версий как минимум **Windows 10 Fall Creators Update (10.0, сборка 16299.0)**.</span><span class="sxs-lookup"><span data-stu-id="4eee5-150">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>
> <span data-ttu-id="4eee5-151">Предыдущие версии Windows 10 не поддерживают .NET Standard 2.0, который необходим для Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="4eee5-151">Previous  versions of Windows 10 do not support .NET Standard 2.0, which is required by Entity Framework Core.</span></span>

## <a name="add-code-to-create-the-database-on-application-startup"></a><span data-ttu-id="4eee5-152">Добавление кода для создания базы данных при запуске приложения</span><span class="sxs-lookup"><span data-stu-id="4eee5-152">Add code to create the database on application startup</span></span>

<span data-ttu-id="4eee5-153">Так как вы хотите создать базу данных на устройстве, где работает приложение, добавьте код, который при запуске приложения выполнит все незавершенные миграции для локальной базы данных.</span><span class="sxs-lookup"><span data-stu-id="4eee5-153">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="4eee5-154">Таким образом локальная база данных будет создана автоматически при первом запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="4eee5-154">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="4eee5-155">Добавьте ссылку на проект из *Blogging.UWP* в *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="4eee5-155">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="4eee5-156">Откройте файл *App.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="4eee5-156">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="4eee5-157">Добавьте выделенный код, который выполняет незавершенные миграции.</span><span class="sxs-lookup"><span data-stu-id="4eee5-157">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="4eee5-158">Если вы внесете в модель изменения, с помощью команды `Add-Migration` вы сможете сформировать новую миграцию, которая внесет соответствующие изменения в базу данных.</span><span class="sxs-lookup"><span data-stu-id="4eee5-158">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="4eee5-159">Теперь при каждом запуске приложения к локальной базе данных на каждом устройстве будут применяться все незавершенные миграции.</span><span class="sxs-lookup"><span data-stu-id="4eee5-159">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="4eee5-160">EF Core использует таблицу `__EFMigrationsHistory` в базе данных для отслеживания уже выполнявшихся в ней миграций.</span><span class="sxs-lookup"><span data-stu-id="4eee5-160">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-data-model"></a><span data-ttu-id="4eee5-161">Использование модели данных</span><span class="sxs-lookup"><span data-stu-id="4eee5-161">Use the data model</span></span>

<span data-ttu-id="4eee5-162">Теперь вы можете использовать EF Core для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="4eee5-162">You can now use EF Core to perform data access.</span></span>

* <span data-ttu-id="4eee5-163">Откройте файл *MainPage.xaml*.</span><span class="sxs-lookup"><span data-stu-id="4eee5-163">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="4eee5-164">Добавьте обработчик загрузки страницы и содержимое пользовательского интерфейса, которые выделены ниже.</span><span class="sxs-lookup"><span data-stu-id="4eee5-164">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="4eee5-165">Теперь добавьте код, который свяжет пользовательский интерфейс с базой данных</span><span class="sxs-lookup"><span data-stu-id="4eee5-165">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="4eee5-166">Откройте файл *MainPage.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="4eee5-166">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="4eee5-167">Добавьте в него код, выделенный в следующем листинге:</span><span class="sxs-lookup"><span data-stu-id="4eee5-167">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="4eee5-168">Теперь вы можете запустить приложение и увидеть, как оно работает:</span><span class="sxs-lookup"><span data-stu-id="4eee5-168">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="4eee5-169">В **обозревателе решений** щелкните правой кнопкой мыши проект *Blogging.UWP* и выберите **Развернуть**.</span><span class="sxs-lookup"><span data-stu-id="4eee5-169">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="4eee5-170">Задайте *Blogging.UWP* в качестве автозагружаемого проекта.</span><span class="sxs-lookup"><span data-stu-id="4eee5-170">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="4eee5-171">**"Отладка" > "Запустить без отладки"**</span><span class="sxs-lookup"><span data-stu-id="4eee5-171">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="4eee5-172">Произойдет сборка и запуск приложения.</span><span class="sxs-lookup"><span data-stu-id="4eee5-172">The app builds and runs.</span></span>

* <span data-ttu-id="4eee5-173">Введите URL-адрес и нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="4eee5-173">Enter a URL and click the **Add** button</span></span>

  ![изображение](_static/create.png)

  ![изображение](_static/list.png)

  <span data-ttu-id="4eee5-176">Готово!</span><span class="sxs-lookup"><span data-stu-id="4eee5-176">Tada!</span></span> <span data-ttu-id="4eee5-177">Теперь у вас есть простое приложение UWP, в котором работает Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="4eee5-177">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4eee5-178">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="4eee5-178">Next steps</span></span>

<span data-ttu-id="4eee5-179">Сведения о совместимости и производительности, которые нужно знать при использовании EF Core с UWP, см. в разделе [Реализации .NET, поддерживаемые EF Core](../../platforms/index.md#universal-windows-platform).</span><span class="sxs-lookup"><span data-stu-id="4eee5-179">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="4eee5-180">Другие статьи этой документации помогут вам расширить свои знания о возможностях Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="4eee5-180">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
