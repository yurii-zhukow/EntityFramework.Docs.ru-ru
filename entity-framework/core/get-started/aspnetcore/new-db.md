---
title: "Начало работы в ASP.NET Core — новая база данных — Core EF"
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: f6ed19d3c5d2ae8d1f5756558e50c1f0dddd2f07
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2018
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="71c08-102">Начало работы с EF Core в ASP.NET Core с новой базой данных</span><span class="sxs-lookup"><span data-stu-id="71c08-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="71c08-103">В этом пошаговом руководстве вы создадите приложение ASP.NET Core MVC, которое выполняет базовые операции доступа к данным через платформу Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="71c08-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="71c08-104">Вы примените процесс миграций, чтобы создать базу данных из модели EF Core.</span><span class="sxs-lookup"><span data-stu-id="71c08-104">You will use migrations to create the database from your EF Core model.</span></span> <span data-ttu-id="71c08-105">В разделе [Дополнительные ресурсы](#additional-resources) вы найдете еще несколько руководств по платформе Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="71c08-105">See [Additional Resources](#additional-resources) for more Entity Framework Core tutorials.</span></span>

<span data-ttu-id="71c08-106">Для работы с этим руководством вам потребуется следующее:</span><span class="sxs-lookup"><span data-stu-id="71c08-106">This tutorial requires:</span></span>
* <span data-ttu-id="71c08-107">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) с такими рабочими нагрузками:</span><span class="sxs-lookup"><span data-stu-id="71c08-107">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="71c08-108">**ASP.NET и веб-разработка** в разделе **Интернет и облако**)</span><span class="sxs-lookup"><span data-stu-id="71c08-108">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="71c08-109">**Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)</span><span class="sxs-lookup"><span data-stu-id="71c08-109">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="71c08-110">[пакет SDK для .NET Core 2.0](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="71c08-110">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>

> [!TIP]  
> <span data-ttu-id="71c08-111">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="71c08-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) on GitHub.</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="71c08-112">Создание нового проекта в Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="71c08-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="71c08-113">Последовательно выберите **Файл > Создать > Проект**.</span><span class="sxs-lookup"><span data-stu-id="71c08-113">**File > New > Project**</span></span>
* <span data-ttu-id="71c08-114">В меню слева выберите **Установленные > Шаблоны > Visual C# -> .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="71c08-114">From the left menu select **Installed > Templates > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="71c08-115">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="71c08-115">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="71c08-116">Введите имя проекта **EFGetStarted.AspNetCore.NewDb** и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="71c08-116">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="71c08-117">В диалоговом окне **Создание веб-приложения ASP.NET Core** сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="71c08-117">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="71c08-118">Убедитесь, что в раскрывающихся списках выбраны варианты **.NET Core** и **ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="71c08-118">Ensure the options **.NET Core** and **ASP.NET Core 2.0** are selected in the drop down lists</span></span>
  * <span data-ttu-id="71c08-119">Выберите **Web Application (Model-View-Controller)** (Веб-приложение "модель — представление — контроллер").</span><span class="sxs-lookup"><span data-stu-id="71c08-119">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="71c08-120">Убедитесь, что для параметра **Проверка подлинности** задано значение **Без проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="71c08-120">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="71c08-121">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="71c08-121">Click **OK**</span></span>

<span data-ttu-id="71c08-122">Предупреждение. Если для параметра **Проверка подлинности** вы установите значение **Учетные записи отдельных пользователей** вместо **Без проверки подлинности**, в ваш проект будет добавлен файл `Models\IdentityModel.cs` с моделью Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="71c08-122">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="71c08-123">Используя методы, которые представлены в этом пошаговом руководстве, вы можете добавить еще одну модель или включить ваши классы сущностей в уже созданную модель.</span><span class="sxs-lookup"><span data-stu-id="71c08-123">Using the techniques you will learn in this walkthrough, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="71c08-124">Установка Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="71c08-124">Install Entity Framework Core</span></span>

<span data-ttu-id="71c08-125">Установите пакеты для поставщиков базы данных EF Core, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="71c08-125">Install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="71c08-126">В этом пошаговом руководстве используется SQL Server.</span><span class="sxs-lookup"><span data-stu-id="71c08-126">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="71c08-127">Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).</span><span class="sxs-lookup"><span data-stu-id="71c08-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="71c08-128">Последовательно выберите пункты **Средства > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="71c08-128">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="71c08-129">Запуск `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="71c08-129">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="71c08-130">Нам понадобятся некоторые средства платформы Entity Framework, чтобы создать базу данных из модели EF Core.</span><span class="sxs-lookup"><span data-stu-id="71c08-130">We will be using some Entity Framework Core Tools to create a database from your EF Core model.</span></span> <span data-ttu-id="71c08-131">Поэтому мы установим пакет средств:</span><span class="sxs-lookup"><span data-stu-id="71c08-131">So we will install the tools package as well:</span></span>

* <span data-ttu-id="71c08-132">Запуск `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="71c08-132">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="71c08-133">Позднее мы применим средства формирования шаблонов для ASP.NET Core, чтобы создать контроллеры и представления.</span><span class="sxs-lookup"><span data-stu-id="71c08-133">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="71c08-134">Поэтому мы установим этот пакет разработки:</span><span class="sxs-lookup"><span data-stu-id="71c08-134">So we will install this design package as well:</span></span>

* <span data-ttu-id="71c08-135">Запуск `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span><span class="sxs-lookup"><span data-stu-id="71c08-135">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="71c08-136">Создание модели</span><span class="sxs-lookup"><span data-stu-id="71c08-136">Create the model</span></span>

<span data-ttu-id="71c08-137">Определите контекст и классы сущности, которые входят в модель:</span><span class="sxs-lookup"><span data-stu-id="71c08-137">Define a context and entity classes that make up the model:</span></span>

* <span data-ttu-id="71c08-138">Щелкните папку **Models** (Модели) правой кнопкой мыши и выберите **Добавить > Класс**.</span><span class="sxs-lookup"><span data-stu-id="71c08-138">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="71c08-139">Введите имя класса **Model.cs** и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="71c08-139">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="71c08-140">Замените все содержимое этого файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="71c08-140">Replace the contents of the file with the following code:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="71c08-141">Примечание. В реальном приложении каждый класс из модели обычно размещается в отдельном файле.</span><span class="sxs-lookup"><span data-stu-id="71c08-141">Note: In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="71c08-142">В этом руководстве мы для простоты заносим все классы в один файл.</span><span class="sxs-lookup"><span data-stu-id="71c08-142">For the sake of simplicity, we are putting all the classes in one file for this tutorial.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="71c08-143">Регистрация контекста с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="71c08-143">Register your context with dependency injection</span></span>

<span data-ttu-id="71c08-144">С помощью [внедрения зависимостей](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) службы (например, `BloggingContext`) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="71c08-144">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="71c08-145">Затем компоненты, которые используют эти службы (например, контроллеры MVC), обращаются к ним через параметры или свойства конструктора.</span><span class="sxs-lookup"><span data-stu-id="71c08-145">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="71c08-146">Чтобы контроллеры MVC могли использовать службу `BloggingContext`, мы должны ее зарегистрировать:</span><span class="sxs-lookup"><span data-stu-id="71c08-146">In order for our MVC controllers to make use of `BloggingContext` we will register it as a service.</span></span>

* <span data-ttu-id="71c08-147">Откройте файл **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="71c08-147">Open **Startup.cs**</span></span>
* <span data-ttu-id="71c08-148">Добавьте в него следующие инструкции `using`:</span><span class="sxs-lookup"><span data-stu-id="71c08-148">Add the following `using` statements:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="71c08-149">Добавьте метод `AddDbContext`, чтобы зарегистрировать нашу службу:</span><span class="sxs-lookup"><span data-stu-id="71c08-149">Add the `AddDbContext` method to register it as a service:</span></span>

* <span data-ttu-id="71c08-150">Добавьте следующий код в метод `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="71c08-150">Add the following code to the `ConfigureServices` method:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

<span data-ttu-id="71c08-151">Примечание. В реальном приложении строка подключения обычно помещается в файл конфигурации.</span><span class="sxs-lookup"><span data-stu-id="71c08-151">Note: A real app would generally put the connection string in a configuration file.</span></span> <span data-ttu-id="71c08-152">Здесь мы для простоты определяем ее прямо в коде.</span><span class="sxs-lookup"><span data-stu-id="71c08-152">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="71c08-153">Дополнительные сведения вы найдете в статье [Connection Strings](../../miscellaneous/connection-strings.md) (Строки подключения).</span><span class="sxs-lookup"><span data-stu-id="71c08-153">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-your-database"></a><span data-ttu-id="71c08-154">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="71c08-154">Create your database</span></span>

<span data-ttu-id="71c08-155">Когда модель будет готова, из нее можно создать базу данных с помощью [миграций](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations):</span><span class="sxs-lookup"><span data-stu-id="71c08-155">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="71c08-156">Откройте PMC:</span><span class="sxs-lookup"><span data-stu-id="71c08-156">Open the PMC:</span></span>

  <span data-ttu-id="71c08-157">Последовательно выберите пункты **Средства -> Диспетчер пакетов NuGet -> Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="71c08-157">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="71c08-158">Запустите `Add-Migration InitialCreate`, чтобы сформировать шаблон миграции для создания начального набора таблиц для модели.</span><span class="sxs-lookup"><span data-stu-id="71c08-158">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="71c08-159">Если появляется сообщение об ошибке `The term 'add-migration' is not recognized as the name of a cmdlet`, закройте и снова откройте Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="71c08-159">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="71c08-160">Запустите `Update-Database`, чтобы применить созданную миграцию к базе данных.</span><span class="sxs-lookup"><span data-stu-id="71c08-160">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="71c08-161">Эта команда создает базу данных и применяет к ней миграции.</span><span class="sxs-lookup"><span data-stu-id="71c08-161">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="71c08-162">Создание контроллера</span><span class="sxs-lookup"><span data-stu-id="71c08-162">Create a controller</span></span>

<span data-ttu-id="71c08-163">Включите в проект формирование шаблонов:</span><span class="sxs-lookup"><span data-stu-id="71c08-163">Enable scaffolding in the project:</span></span>

* <span data-ttu-id="71c08-164">В **обозревателе решений** щелкните папку **Контроллеры** правой кнопкой мыши и выберите пункт **Добавить -> Контроллер**.</span><span class="sxs-lookup"><span data-stu-id="71c08-164">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="71c08-165">Выберите **Минимальные зависимости** и щелкните **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="71c08-165">Select **Minimal Dependencies** and click **Add**.</span></span>
* <span data-ttu-id="71c08-166">Файл *ScaffoldingReadMe.txt* можно игнорировать или удалить.</span><span class="sxs-lookup"><span data-stu-id="71c08-166">You can ignore or delete the *ScaffoldingReadMe.txt* file.</span></span>

<span data-ttu-id="71c08-167">Теперь формирование шаблонов включено, и мы можем сформировать контроллер для сущности `Blog`:</span><span class="sxs-lookup"><span data-stu-id="71c08-167">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="71c08-168">В **обозревателе решений** щелкните папку **Контроллеры** правой кнопкой мыши и выберите пункт **Добавить -> Контроллер**.</span><span class="sxs-lookup"><span data-stu-id="71c08-168">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="71c08-169">Выберите **Контроллер MVC с представлениями, использующий Entity Framework** и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="71c08-169">Select **MVC Controller with views, using Entity Framework** and click **Ok**.</span></span>
* <span data-ttu-id="71c08-170">Для параметра **Класс модели** установите значение **Блог**, а для параметра **Класс контекста данных** — **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="71c08-170">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="71c08-171">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="71c08-171">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="71c08-172">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="71c08-172">Run the application</span></span>

<span data-ttu-id="71c08-173">Нажмите клавишу F5, чтобы запустить приложение для проверки.</span><span class="sxs-lookup"><span data-stu-id="71c08-173">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="71c08-174">Перейдите к папке `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="71c08-174">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="71c08-175">С помощью ссылки создания внесите в блог несколько записей.</span><span class="sxs-lookup"><span data-stu-id="71c08-175">Use the create link to create some blog entries.</span></span> <span data-ttu-id="71c08-176">Проверьте также работу ссылок для просмотра и удаления.</span><span class="sxs-lookup"><span data-stu-id="71c08-176">Test the details and delete links.</span></span>

![изображение](_static/create.png)

![изображение](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="71c08-179">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="71c08-179">Additional Resources</span></span>

* <span data-ttu-id="71c08-180">[Руководство по применению кроссплатформенной консоли EF с новой базой данных SQLite](xref:core/get-started/netcore/new-db-sqlite)</span><span class="sxs-lookup"><span data-stu-id="71c08-180">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="71c08-181">Введение в ASP.NET Core MVC для Mac и Linux</span><span class="sxs-lookup"><span data-stu-id="71c08-181">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="71c08-182">Введение в ASP.NET Core MVC для Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71c08-182">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="71c08-183">Начало работы с ASP.NET Core и Entity Framework Core с использованием Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71c08-183">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
