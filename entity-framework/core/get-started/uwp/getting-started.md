---
title: Начало работы в UWP — новая база данных — Core EF
author: rowanmiller
ms.date: 08/08/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: c243ef2a1940af9bf4f4b32f17acfcce7f972862
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996914"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="ff513-102">Начало работы с EF Core на универсальной платформе Windows (UWP) с новой базой данных</span><span class="sxs-lookup"><span data-stu-id="ff513-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="ff513-103">В этом руководстве вы создадите приложение универсальной платформы Windows (UWP), которое осуществляет простейший доступ к локальной базе данных SQLite с помощью Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ff513-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="ff513-104">[Пример для этой статьи на GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span><span class="sxs-lookup"><span data-stu-id="ff513-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff513-105">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="ff513-105">Prerequisites</span></span>

* <span data-ttu-id="ff513-106">[Windows 10 Fall Creators Update (10.0, сборка 16299) или более поздняя версия](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span><span class="sxs-lookup"><span data-stu-id="ff513-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="ff513-107">[Visual Studio 2017 версии 15.7 или более поздняя версия](https://www.visualstudio.com/downloads/) с рабочей нагрузкой **Разработка приложений для универсальной платформы Windows**.</span><span class="sxs-lookup"><span data-stu-id="ff513-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="ff513-108">[Пакет SDK для .NET Core 2.1](https://www.microsoft.com/net/core) или более поздняя версия.</span><span class="sxs-lookup"><span data-stu-id="ff513-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

## <a name="create-a-model-project"></a><span data-ttu-id="ff513-109">Создание проекта модели</span><span class="sxs-lookup"><span data-stu-id="ff513-109">Create a model project</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ff513-110">Из-за ограниченных возможностей взаимодействия инструментов .NET Core с проектами UWP, если вы хотите запускать команды миграции в **консоли диспетчера пакетов** (PMC), модель следует поместить в проект не для UWP</span><span class="sxs-lookup"><span data-stu-id="ff513-110">Due to limitations in the way .NET Core tools interact with UWP projects the model needs to be placed in a non-UWP project to be able to run migrations commands in the **Package Manager Console** (PMC)</span></span>

* <span data-ttu-id="ff513-111">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ff513-111">Open Visual Studio</span></span>

* <span data-ttu-id="ff513-112">Последовательно выберите **Файл > Создать > Проект**.</span><span class="sxs-lookup"><span data-stu-id="ff513-112">**File > New > Project**</span></span>

* <span data-ttu-id="ff513-113">В меню слева выберите **"Установленные" > Visual C# > .NET Standard**.</span><span class="sxs-lookup"><span data-stu-id="ff513-113">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="ff513-114">Выберите шаблон **Библиотека классов (.NET Standard)**.</span><span class="sxs-lookup"><span data-stu-id="ff513-114">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="ff513-115">Назовите проект *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="ff513-115">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="ff513-116">Назовите решение *Blogging*.</span><span class="sxs-lookup"><span data-stu-id="ff513-116">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="ff513-117">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ff513-117">Click **OK**.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="ff513-118">Установка Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="ff513-118">Install Entity Framework Core</span></span>

<span data-ttu-id="ff513-119">Чтобы использовать EF Core, установите пакеты для поставщиков базы данных, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="ff513-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="ff513-120">В этом руководстве используется SQLite.</span><span class="sxs-lookup"><span data-stu-id="ff513-120">This tutorial uses SQLite.</span></span> <span data-ttu-id="ff513-121">Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).</span><span class="sxs-lookup"><span data-stu-id="ff513-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="ff513-122">Выберите **"Сервис" > "Диспетчер пакетов NuGet" > "Консоль диспетчера пакетов"**.</span><span class="sxs-lookup"><span data-stu-id="ff513-122">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="ff513-123">Запуск `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="ff513-123">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

<span data-ttu-id="ff513-124">Далее в этом руководстве вы воспользуетесь рядом инструментов Entity Framework Core для обслуживания базы данных.</span><span class="sxs-lookup"><span data-stu-id="ff513-124">Later in this tutorial you will be using some Entity Framework Core tools to maintain the database.</span></span> <span data-ttu-id="ff513-125">Поэтому также установите пакет инструментов.</span><span class="sxs-lookup"><span data-stu-id="ff513-125">So install the tools package as well.</span></span>

* <span data-ttu-id="ff513-126">Запуск `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="ff513-126">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="ff513-127">Создание модели</span><span class="sxs-lookup"><span data-stu-id="ff513-127">Create the model</span></span>

<span data-ttu-id="ff513-128">Теперь нужно задать контекст и классы сущностей, которые составляют модель.</span><span class="sxs-lookup"><span data-stu-id="ff513-128">Now it's time to define a context and entity classes that make up the model.</span></span>

* <span data-ttu-id="ff513-129">Удалите *Class1.cs*.</span><span class="sxs-lookup"><span data-stu-id="ff513-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="ff513-130">Создайте файл *Model.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="ff513-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a><span data-ttu-id="ff513-131">Создание нового проекта UWP</span><span class="sxs-lookup"><span data-stu-id="ff513-131">Create a new UWP project</span></span>

* <span data-ttu-id="ff513-132">В **обозревателе решений** щелкните решение правой кнопкой мыши и выберите **"Добавить" > "Новый проект"**.</span><span class="sxs-lookup"><span data-stu-id="ff513-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="ff513-133">В меню слева выберите **"Установленные" > Visual C# > "Универсальные приложения для Windows"**.</span><span class="sxs-lookup"><span data-stu-id="ff513-133">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="ff513-134">Выберите шаблон проекта **Пустое приложение (универсальное приложение для Windows)**.</span><span class="sxs-lookup"><span data-stu-id="ff513-134">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="ff513-135">Назовите проект *Blogging.UWP* и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="ff513-135">Name the project *Blogging.UWP*, and click **OK**</span></span>

* <span data-ttu-id="ff513-136">Задайте в качестве целевой и минимальной версий как минимум **Windows 10 Fall Creators Update (10.0, сборка 16299.0)**.</span><span class="sxs-lookup"><span data-stu-id="ff513-136">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="ff513-137">Создание первоначальной миграции</span><span class="sxs-lookup"><span data-stu-id="ff513-137">Create the initial migration</span></span>

<span data-ttu-id="ff513-138">Теперь, когда у вас есть модель, настройте приложение на создание базы данных при его первом запуске.</span><span class="sxs-lookup"><span data-stu-id="ff513-138">Now that you have a model, set up the app to create a database the first time it runs.</span></span> <span data-ttu-id="ff513-139">В этом разделе вы создадите начальную миграцию.</span><span class="sxs-lookup"><span data-stu-id="ff513-139">In this section, you create the initial migration.</span></span> <span data-ttu-id="ff513-140">В следующем разделе вы добавите код, который выполняет эту миграцию при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="ff513-140">In the following section, you add code that applies this migration when the app starts.</span></span>

<span data-ttu-id="ff513-141">Инструментам миграции требуется автозагружаемый проект, не являющийся проектом UWP, поэтому сначала создайте его.</span><span class="sxs-lookup"><span data-stu-id="ff513-141">Migrations tools require a non-UWP startup project, so create that first.</span></span>

* <span data-ttu-id="ff513-142">В **обозревателе решений** щелкните решение правой кнопкой мыши и выберите **"Добавить" > "Новый проект"**.</span><span class="sxs-lookup"><span data-stu-id="ff513-142">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="ff513-143">В меню слева выберите **"Установленные" > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ff513-143">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="ff513-144">Выберите шаблон проекта **Консольное приложение (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="ff513-144">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="ff513-145">Назовите проект *Blogging.Migrations.Startup* и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ff513-145">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="ff513-146">Добавьте ссылку на проект из *Blogging.Migrations.Startup* в *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="ff513-146">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

<span data-ttu-id="ff513-147">Теперь вы можете создать начальную миграцию.</span><span class="sxs-lookup"><span data-stu-id="ff513-147">Now you can create your initial migration.</span></span>

* <span data-ttu-id="ff513-148">Последовательно выберите пункты **Средства > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="ff513-148">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="ff513-149">Выберите проект *Blogging.Model* в качестве **проекта по умолчанию**.</span><span class="sxs-lookup"><span data-stu-id="ff513-149">Select the *Blogging.Model* project as the **Default project**.</span></span>

* <span data-ttu-id="ff513-150">В **обозревателе решений** задайте проект *Blogging.Migrations.Startup* как автозагружаемый.</span><span class="sxs-lookup"><span data-stu-id="ff513-150">In **Solution Explorer**, set the *Blogging.Migrations.Startup* project as the startup project.</span></span>

* <span data-ttu-id="ff513-151">Запустите `Add-Migration InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="ff513-151">Run `Add-Migration InitialCreate`.</span></span>

  <span data-ttu-id="ff513-152">Эта команда формирует миграцию, которая создает начальный набор таблиц для вашей модели.</span><span class="sxs-lookup"><span data-stu-id="ff513-152">This command scaffolds a migration that creates the initial set of tables for your model.</span></span>

## <a name="create-the-database-on-app-startup"></a><span data-ttu-id="ff513-153">Создание базы данных при запуске приложения</span><span class="sxs-lookup"><span data-stu-id="ff513-153">Create the database on app startup</span></span>

<span data-ttu-id="ff513-154">Так как вы хотите создать базу данных на устройстве, где работает приложение, добавьте код, который при запуске приложения выполнит все незавершенные миграции для локальной базы данных.</span><span class="sxs-lookup"><span data-stu-id="ff513-154">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="ff513-155">Таким образом локальная база данных будет создана автоматически при первом запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="ff513-155">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="ff513-156">Добавьте ссылку на проект из *Blogging.UWP* в *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="ff513-156">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="ff513-157">Откройте файл *App.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="ff513-157">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="ff513-158">Добавьте выделенный код, который выполняет незавершенные миграции.</span><span class="sxs-lookup"><span data-stu-id="ff513-158">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="ff513-159">Если вы внесете в модель изменения, с помощью команды `Add-Migration` вы сможете сформировать новую миграцию, которая внесет соответствующие изменения в базу данных.</span><span class="sxs-lookup"><span data-stu-id="ff513-159">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="ff513-160">Теперь при каждом запуске приложения к локальной базе данных на каждом устройстве будут применяться все незавершенные миграции.</span><span class="sxs-lookup"><span data-stu-id="ff513-160">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="ff513-161">EF использует таблицу `__EFMigrationsHistory` в базе данных, чтобы отслеживать уже примененные к базе данных миграции.</span><span class="sxs-lookup"><span data-stu-id="ff513-161">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="ff513-162">Использование модели</span><span class="sxs-lookup"><span data-stu-id="ff513-162">Use the model</span></span>

<span data-ttu-id="ff513-163">Теперь вы можете использовать созданную модель для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="ff513-163">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="ff513-164">Откройте файл *MainPage.xaml*.</span><span class="sxs-lookup"><span data-stu-id="ff513-164">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="ff513-165">Добавьте обработчик загрузки страницы и содержимое пользовательского интерфейса, которые выделены ниже.</span><span class="sxs-lookup"><span data-stu-id="ff513-165">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="ff513-166">Теперь добавьте код, который свяжет пользовательский интерфейс с базой данных</span><span class="sxs-lookup"><span data-stu-id="ff513-166">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="ff513-167">Откройте файл *MainPage.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="ff513-167">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="ff513-168">Добавьте в него код, выделенный в следующем листинге:</span><span class="sxs-lookup"><span data-stu-id="ff513-168">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="ff513-169">Теперь вы можете запустить приложение и увидеть, как оно работает:</span><span class="sxs-lookup"><span data-stu-id="ff513-169">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="ff513-170">В **обозревателе решений** щелкните правой кнопкой мыши проект *Blogging.UWP* и выберите **Развернуть**.</span><span class="sxs-lookup"><span data-stu-id="ff513-170">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="ff513-171">Задайте *Blogging.UWP* в качестве автозагружаемого проекта.</span><span class="sxs-lookup"><span data-stu-id="ff513-171">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="ff513-172">**"Отладка" > "Запустить без отладки"**</span><span class="sxs-lookup"><span data-stu-id="ff513-172">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="ff513-173">Произойдет сборка и запуск приложения.</span><span class="sxs-lookup"><span data-stu-id="ff513-173">The app builds and runs.</span></span>

* <span data-ttu-id="ff513-174">Введите URL-адрес и нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="ff513-174">Enter a URL and click the **Add** button</span></span>

  ![изображение](_static/create.png)

  ![изображение](_static/list.png)

  <span data-ttu-id="ff513-177">Готово!</span><span class="sxs-lookup"><span data-stu-id="ff513-177">Tada!</span></span> <span data-ttu-id="ff513-178">Теперь у вас есть простое приложение UWP, в котором работает Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ff513-178">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ff513-179">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="ff513-179">Next steps</span></span>

<span data-ttu-id="ff513-180">Сведения о совместимости и производительности, которые нужно знать при использовании EF Core с UWP, см. в разделе [Реализации .NET, поддерживаемые EF Core](../../platforms/index.md#universal-windows-platform).</span><span class="sxs-lookup"><span data-stu-id="ff513-180">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="ff513-181">Другие статьи этой документации помогут вам расширить свои знания о возможностях Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="ff513-181">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
