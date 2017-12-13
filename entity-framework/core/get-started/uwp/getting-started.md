---
title: "Начало работы в UWP — новая база данных — Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.topic: get-started-article
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
ms.technology: entity-framework-core
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: f743ff5392d1f30283a13d2e7fb8029be88387aa
ms.sourcegitcommit: 96324e58c02b97277395ed43173bf13ac80d2012
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/01/2017
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="86fd3-102">Начало работы с EF Core на универсальной платформе Windows (UWP) с новой базой данных</span><span class="sxs-lookup"><span data-stu-id="86fd3-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

> [!NOTE]
> <span data-ttu-id="86fd3-103">В этом руководстве используется EF Core 2.0.1 (выпущен вместе с ASP.NET Core и пакетом SDK для .NET Core 2.0.3).</span><span class="sxs-lookup"><span data-stu-id="86fd3-103">This tutorial uses EF Core 2.0.1 (released alongside ASP.NET Core and .NET Core SDK 2.0.3).</span></span> <span data-ttu-id="86fd3-104">В EF Core 2.0.0 еще не исправлены некоторые ошибки, оказывающие важное влияние на работу с UWP.</span><span class="sxs-lookup"><span data-stu-id="86fd3-104">EF Core 2.0.0 lacks some crucial bug fixes required for a good UWP experience.</span></span>

<span data-ttu-id="86fd3-105">В этом пошаговом руководстве вы создадите приложение UWP, которое осуществляет базовые операции доступа к локальной базе данных SQLite с помощью Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="86fd3-105">In this walkthrough, you will build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="86fd3-106">**Старайтесь не использовать анонимные типы в запросах LINQ на UWP**.</span><span class="sxs-lookup"><span data-stu-id="86fd3-106">**Consider avoiding anonymous types in LINQ queries on UWP**.</span></span> <span data-ttu-id="86fd3-107">Чтобы развернуть приложение UWP в магазине приложений, оно должно быть скомпилировано в .NET Native.</span><span class="sxs-lookup"><span data-stu-id="86fd3-107">Deploying a UWP application to the app store requires your application to be compiled with .NET Native.</span></span> <span data-ttu-id="86fd3-108">Запросы с анонимными типами плохо работают в среде .NET Native.</span><span class="sxs-lookup"><span data-stu-id="86fd3-108">Queries with anonymous types have worse performance on .NET Native.</span></span>

> [!TIP]
> <span data-ttu-id="86fd3-109">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="86fd3-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="86fd3-110">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="86fd3-110">Prerequisites</span></span>

<span data-ttu-id="86fd3-111">Для выполнения этого пошагового руководства необходимы следующие элементы:</span><span class="sxs-lookup"><span data-stu-id="86fd3-111">The following items are required to complete this walkthrough:</span></span>

* <span data-ttu-id="86fd3-112">[Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0);</span><span class="sxs-lookup"><span data-stu-id="86fd3-112">[Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)</span></span>

* <span data-ttu-id="86fd3-113">[пакет SDK .NET Core 2.0.0](https://www.microsoft.com/net/core) или более поздней версии;</span><span class="sxs-lookup"><span data-stu-id="86fd3-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

* <span data-ttu-id="86fd3-114">[Visual Studio 2017](https://www.visualstudio.com/downloads/) версии 15.4 или более поздней версии с рабочей нагрузкой **Разработка с помощью универсальной платформы Windows**.</span><span class="sxs-lookup"><span data-stu-id="86fd3-114">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.4 or later with the **Universal Windows Platform Development** workload.</span></span>

## <a name="create-a-new-model-project"></a><span data-ttu-id="86fd3-115">Создание нового проекта модели</span><span class="sxs-lookup"><span data-stu-id="86fd3-115">Create a new model project</span></span>

> [!WARNING]
> <span data-ttu-id="86fd3-116">Из-за ограничений в методах взаимодействия между средствами .NET Core и проектами UWP, модель нельзя помещать в проект UWP, если для нее нужно выполнять команды миграции в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="86fd3-116">Due to limitations in the way .NET Core tools interact with UWP projects the model needs to be placed in a non-UWP project to be able to run migrations commands in the Package Manager Console</span></span>

* <span data-ttu-id="86fd3-117">Откройте Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="86fd3-117">Open Visual Studio</span></span>

* <span data-ttu-id="86fd3-118">Последовательно выберите "Файл" > "Создать" > "Проект".</span><span class="sxs-lookup"><span data-stu-id="86fd3-118">File > New > Project...</span></span>

* <span data-ttu-id="86fd3-119">В меню слева выберите "Шаблоны" -> Visual C#.</span><span class="sxs-lookup"><span data-stu-id="86fd3-119">From the left menu select Templates > Visual C#</span></span>

* <span data-ttu-id="86fd3-120">Выберите шаблон проекта **Библиотека классов (.NET Standard)**.</span><span class="sxs-lookup"><span data-stu-id="86fd3-120">Select the **Class Library (.NET Standard)** project template</span></span>

* <span data-ttu-id="86fd3-121">Присвойте проекту имя и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="86fd3-121">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="86fd3-122">Установка Entity Framework</span><span class="sxs-lookup"><span data-stu-id="86fd3-122">Install Entity Framework</span></span>

<span data-ttu-id="86fd3-123">Чтобы использовать EF Core, установите пакеты для поставщиков базы данных, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="86fd3-123">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="86fd3-124">В этом пошаговом руководстве используется SQLite.</span><span class="sxs-lookup"><span data-stu-id="86fd3-124">This walkthrough uses SQLite.</span></span> <span data-ttu-id="86fd3-125">Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).</span><span class="sxs-lookup"><span data-stu-id="86fd3-125">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="86fd3-126">Последовательно выберите пункты "Средства" -> "Диспетчер пакетов NuGet" -> "Консоль диспетчера пакетов".</span><span class="sxs-lookup"><span data-stu-id="86fd3-126">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="86fd3-127">Выполните `Install-Package Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="86fd3-127">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

<span data-ttu-id="86fd3-128">Далее в этом пошаговом руководстве мы будем использовать некоторые средства платформы Entity Framework для обслуживания базы данных.</span><span class="sxs-lookup"><span data-stu-id="86fd3-128">Later in this walkthrough we will also be using some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="86fd3-129">Поэтому мы установим пакет средств:</span><span class="sxs-lookup"><span data-stu-id="86fd3-129">So we will install the tools package as well.</span></span>

* <span data-ttu-id="86fd3-130">Выполните `Install-Package Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="86fd3-130">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

* <span data-ttu-id="86fd3-131">Измените CSPROJ-файл, заменив в нем `<TargetFramework>netstandard2.0</TargetFramework>` на `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`.</span><span class="sxs-lookup"><span data-stu-id="86fd3-131">Edit the .csproj file and replace `<TargetFramework>netstandard2.0</TargetFramework>` with `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`</span></span>

## <a name="create-your-model"></a><span data-ttu-id="86fd3-132">Создание модели</span><span class="sxs-lookup"><span data-stu-id="86fd3-132">Create your model</span></span>

<span data-ttu-id="86fd3-133">Теперь нам нужно определить контекст и классы сущности, которые входят в модель:</span><span class="sxs-lookup"><span data-stu-id="86fd3-133">Now it's time to define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="86fd3-134">Выберите элементы "Проект" > "Добавить класс".</span><span class="sxs-lookup"><span data-stu-id="86fd3-134">Project > Add Class...</span></span>

* <span data-ttu-id="86fd3-135">Введите имя класса *Model.cs* и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="86fd3-135">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="86fd3-136">Замените все содержимое этого файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="86fd3-136">Replace the contents of the file with the following code</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a><span data-ttu-id="86fd3-137">Создание нового проекта UWP</span><span class="sxs-lookup"><span data-stu-id="86fd3-137">Create a new UWP project</span></span>

* <span data-ttu-id="86fd3-138">Откройте Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="86fd3-138">Open Visual Studio</span></span>

* <span data-ttu-id="86fd3-139">Последовательно выберите "Файл" > "Создать" > "Проект".</span><span class="sxs-lookup"><span data-stu-id="86fd3-139">File > New > Project...</span></span>

* <span data-ttu-id="86fd3-140">В меню слева выберите элементы "Шаблоны" > "Visual C#" > "Универсальные приложения Windows".</span><span class="sxs-lookup"><span data-stu-id="86fd3-140">From the left menu select Templates > Visual C# > Windows Universal</span></span>

* <span data-ttu-id="86fd3-141">Выберите шаблон проекта **Пустое приложение (универсальное приложение Windows)**.</span><span class="sxs-lookup"><span data-stu-id="86fd3-141">Select the **Blank App (Universal Windows)** project template</span></span>

* <span data-ttu-id="86fd3-142">Присвойте проекту имя и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="86fd3-142">Give the project a name and click **OK**</span></span>

* <span data-ttu-id="86fd3-143">В качестве целевой и минимальной версий укажите по меньшей мере `Windows 10 Fall Creators Update (10.0; build 16299.0)`.</span><span class="sxs-lookup"><span data-stu-id="86fd3-143">Set the target and minimum versions to at least `Windows 10 Fall Creators Update (10.0; build 16299.0)`</span></span>

## <a name="create-your-database"></a><span data-ttu-id="86fd3-144">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="86fd3-144">Create your database</span></span>

<span data-ttu-id="86fd3-145">Итак, модель будет готова, и вы можете создать из нее базу данных с помощью миграций:</span><span class="sxs-lookup"><span data-stu-id="86fd3-145">Now that you have a model, you can use migrations to create a database for you.</span></span>

* <span data-ttu-id="86fd3-146">Последовательно выберите пункты "Средства" -> "Диспетчер пакетов NuGet" -> "Консоль диспетчера пакетов".</span><span class="sxs-lookup"><span data-stu-id="86fd3-146">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="86fd3-147">Выберите проект модели в качестве проекта по умолчанию и укажите его в качестве запускаемого проекта.</span><span class="sxs-lookup"><span data-stu-id="86fd3-147">Select the model project as the Default project and set it as the startup project</span></span>

* <span data-ttu-id="86fd3-148">Запустите `Add-Migration MyFirstMigration`, чтобы сформировать шаблон миграции для создания начального набора таблиц для модели.</span><span class="sxs-lookup"><span data-stu-id="86fd3-148">Run `Add-Migration MyFirstMigration` to scaffold a migration to create the initial set of tables for your model.</span></span>

<span data-ttu-id="86fd3-149">Поскольку мы хотим создать базу данных на устройстве, где работает приложение, нам нужно добавить код, который при запуске приложения применит все незавершенные миграции к локальной базе данных.</span><span class="sxs-lookup"><span data-stu-id="86fd3-149">Since we want the database to be created on the device that the app runs on, we will add some code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="86fd3-150">Таким образом, при первом запуске приложения локальная база данных будет создана автоматически.</span><span class="sxs-lookup"><span data-stu-id="86fd3-150">The first time that the app runs, this will take care of creating the local database for us.</span></span>

* <span data-ttu-id="86fd3-151">В **обозревателе решений** щелкните правой кнопкой мыши файл **App.xaml** и выберите команду **Просмотреть код**.</span><span class="sxs-lookup"><span data-stu-id="86fd3-151">Right-click on **App.xaml** in **Solution Explorer** and select **View Code**</span></span>

* <span data-ttu-id="86fd3-152">Добавьте в начало файла выделенные инструкции using.</span><span class="sxs-lookup"><span data-stu-id="86fd3-152">Add the highlighted using to the start of the file</span></span>

* <span data-ttu-id="86fd3-153">Добавьте выделенный код, который применяет незавершенные миграции:</span><span class="sxs-lookup"><span data-stu-id="86fd3-153">Add the highlighted code to apply any pending migrations</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/App.xaml.cs?highlight=1,25-28)]

> [!TIP]  
> <span data-ttu-id="86fd3-154">Если позднее вы внесете изменения в эту модель, с помощью команды `Add-Migration` вы сможете создать новую миграцию, которая применяет необходимые изменения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="86fd3-154">If you make future changes to your model, you can use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="86fd3-155">Теперь при каждом запуске приложения к локальной базе данных на каждом устройстве будут применяться все незавершенные миграции.</span><span class="sxs-lookup"><span data-stu-id="86fd3-155">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="86fd3-156">EF использует таблицу `__EFMigrationsHistory` в базе данных, чтобы отслеживать уже примененные к базе данных миграции.</span><span class="sxs-lookup"><span data-stu-id="86fd3-156">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="86fd3-157">Использование модели</span><span class="sxs-lookup"><span data-stu-id="86fd3-157">Use your model</span></span>

<span data-ttu-id="86fd3-158">Теперь вы можете использовать созданную модель для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="86fd3-158">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="86fd3-159">Откройте файл *MainPage.xaml*.</span><span class="sxs-lookup"><span data-stu-id="86fd3-159">Open *MainPage.xaml*</span></span>

* <span data-ttu-id="86fd3-160">Добавьте обработчик загрузки страницы и содержимое пользовательского интерфейса, которые выделены ниже.</span><span class="sxs-lookup"><span data-stu-id="86fd3-160">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="86fd3-161">Теперь мы добавим код, который подключает пользовательский интерфейс к базе данных:</span><span class="sxs-lookup"><span data-stu-id="86fd3-161">Now we'll add code to wire up the UI with the database</span></span>

* <span data-ttu-id="86fd3-162">В **обозревателе решений** щелкните правой кнопкой мыши файл **MainPage.xaml** и выберите команду **Просмотреть код**.</span><span class="sxs-lookup"><span data-stu-id="86fd3-162">Right-click **MainPage.xaml** in **Solution Explorer** and select **View Code**</span></span>

* <span data-ttu-id="86fd3-163">Добавьте в него код, выделенный в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="86fd3-163">Add the highlighted code from the following listing</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml.cs?highlight=30-48)]

<span data-ttu-id="86fd3-164">Теперь вы можете запустить приложение и увидеть, как оно работает:</span><span class="sxs-lookup"><span data-stu-id="86fd3-164">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="86fd3-165">Выберите "Отладка" -> "Запустить без отладки".</span><span class="sxs-lookup"><span data-stu-id="86fd3-165">Debug > Start Without Debugging</span></span>

* <span data-ttu-id="86fd3-166">Это действие компилирует и запускает приложение.</span><span class="sxs-lookup"><span data-stu-id="86fd3-166">The application will build and launch</span></span>

* <span data-ttu-id="86fd3-167">Введите URL-адрес и нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="86fd3-167">Enter a URL and click the **Add** button</span></span>

![image](_static/create.png)

![image](_static/list.png)

## <a name="next-steps"></a><span data-ttu-id="86fd3-170">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="86fd3-170">Next steps</span></span>

> [!TIP]
> <span data-ttu-id="86fd3-171">Производительность метода `SaveChanges()` можно повысить, реализовав в типах сущностей [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) и применив к ним `ChangeTrackingStrategy.ChangingAndChangedNotifications`.</span><span class="sxs-lookup"><span data-stu-id="86fd3-171">`SaveChanges()` performance can be improved by implementing [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) in your entity types and using `ChangeTrackingStrategy.ChangingAndChangedNotifications`.</span></span>

<span data-ttu-id="86fd3-172">Готово!</span><span class="sxs-lookup"><span data-stu-id="86fd3-172">Tada!</span></span> <span data-ttu-id="86fd3-173">Теперь у вас есть простое приложение UWP, на котором выполняется Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="86fd3-173">You now have a simple UWP app running Entity Framework.</span></span>

<span data-ttu-id="86fd3-174">Изучите другие статьи в этом пакете документации, чтобы расширить свои знания о возможностях платформы Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="86fd3-174">Check out other articles in this documentation to learn more about Entity Framework's features.</span></span>
