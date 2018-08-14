---
title: Начало работы в ASP.NET Core — новая база данных — Core EF
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 08/03/2018
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 9e86bc9cff028ad9791f23cbb45f0a93110c0064
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614354"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="1f0cf-102">Начало работы с EF Core в ASP.NET Core с новой базой данных</span><span class="sxs-lookup"><span data-stu-id="1f0cf-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="1f0cf-103">В этом руководстве вы создадите приложение ASP.NET Core MVC, которое выполняет базовые операции доступа к данным через платформу Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="1f0cf-104">Вы создадите из модели EF Core базу данных при помощи миграций.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-104">You use migrations to create the database from your EF Core model.</span></span>

<span data-ttu-id="1f0cf-105">[Пример для этой статьи на GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span><span class="sxs-lookup"><span data-stu-id="1f0cf-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1f0cf-106">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="1f0cf-106">Prerequisites</span></span>

<span data-ttu-id="1f0cf-107">Установите следующее программное обеспечение.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-107">Install the following software:</span></span>

* <span data-ttu-id="1f0cf-108">[Visual Studio 2017 версии 15.7](https://www.visualstudio.com/downloads/) со следующими рабочими нагрузками:</span><span class="sxs-lookup"><span data-stu-id="1f0cf-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="1f0cf-109">**ASP.NET и веб-разработка** в разделе **Интернет и облако**)</span><span class="sxs-lookup"><span data-stu-id="1f0cf-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="1f0cf-110">**Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)</span><span class="sxs-lookup"><span data-stu-id="1f0cf-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="1f0cf-111">[Пакет SDK для NET Core 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="1f0cf-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="1f0cf-112">Создание нового проекта в Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="1f0cf-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="1f0cf-113">Откройте Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-113">Open Visual Studio 2017</span></span>
* <span data-ttu-id="1f0cf-114">Последовательно выберите **Файл > Создать > Проект**.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-114">**File > New > Project**</span></span>
* <span data-ttu-id="1f0cf-115">В меню слева выберите **"Установленные" > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-115">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="1f0cf-116">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-116">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="1f0cf-117">Введите имя проекта **EFGetStarted.AspNetCore.NewDb** и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-117">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="1f0cf-118">В диалоговом окне **Создание веб-приложения ASP.NET Core** сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-118">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="1f0cf-119">Выберите пункты **.NET Core** и **ASP.NET Core 2.1** в раскрывающихся списках</span><span class="sxs-lookup"><span data-stu-id="1f0cf-119">Ensure the options **.NET Core** and **ASP.NET Core 2.1** are selected in the drop down lists</span></span>
  * <span data-ttu-id="1f0cf-120">Выберите **Web Application (Model-View-Controller)** (Веб-приложение "модель — представление — контроллер").</span><span class="sxs-lookup"><span data-stu-id="1f0cf-120">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="1f0cf-121">Убедитесь, что для параметра **Проверка подлинности** задано значение **Без проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-121">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="1f0cf-122">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-122">Click **OK**</span></span>

<span data-ttu-id="1f0cf-123">Предупреждение. Если для параметра **Проверка подлинности** вы установите значение **Учетные записи отдельных пользователей** вместо **Без проверки подлинности**, в ваш проект будет добавлен файл `Models\IdentityModel.cs` с моделью Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-123">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="1f0cf-124">Используя методы, представленные в этом руководстве, вы можете добавить еще одну модель или расширить уже созданную, включив в нее ваши классы сущностей.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-124">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="1f0cf-125">Установка Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="1f0cf-125">Install Entity Framework Core</span></span>

<span data-ttu-id="1f0cf-126">Чтобы установить EF Core, установите пакеты целевых поставщиков базы данных EF Core, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-126">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="1f0cf-127">Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).</span><span class="sxs-lookup"><span data-stu-id="1f0cf-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="1f0cf-128">В этом руководстве используется SQL Server, поэтому устанавливать пакет поставщиков не требуется.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-128">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="1f0cf-129">Пакет поставщиков SQL Server включен в [метапакет Microsoft.AspnetCore.App](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="1f0cf-129">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="create-the-model"></a><span data-ttu-id="1f0cf-130">Создание модели</span><span class="sxs-lookup"><span data-stu-id="1f0cf-130">Create the model</span></span>

<span data-ttu-id="1f0cf-131">Задайте класс контекста и классы сущностей, составляющие модель:</span><span class="sxs-lookup"><span data-stu-id="1f0cf-131">Define a context class and entity classes that make up the model:</span></span>

* <span data-ttu-id="1f0cf-132">Щелкните папку **Models** (Модели) правой кнопкой мыши и выберите **Добавить > Класс**.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-132">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="1f0cf-133">Введите имя класса **Model.cs** и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-133">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="1f0cf-134">Замените все содержимое этого файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="1f0cf-134">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="1f0cf-135">В реальном приложении каждый класс из модели обычно размещается в отдельном файле.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-135">In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="1f0cf-136">В этом руководстве для простоты все классы заносятся в один файл.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-136">For the sake of simplicity, this tutorial puts all the classes in one file.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="1f0cf-137">Регистрация контекста с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="1f0cf-137">Register your context with dependency injection</span></span>

<span data-ttu-id="1f0cf-138">С помощью [внедрения зависимостей](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) службы (например, `BloggingContext`) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-138">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="1f0cf-139">Затем компоненты, которые используют эти службы (например, контроллеры MVC), обращаются к ним через параметры или свойства конструктора.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-139">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="1f0cf-140">Чтобы сделать контекст `BloggingContext` доступным MVC-контроллерам, зарегистрируйте его как службу.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-140">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="1f0cf-141">Откройте файл **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-141">Open **Startup.cs**</span></span>
* <span data-ttu-id="1f0cf-142">Добавьте в него следующие инструкции `using`:</span><span class="sxs-lookup"><span data-stu-id="1f0cf-142">Add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="1f0cf-143">Вызовите метод `AddDbContext`, чтобы зарегистрировать контекст как службу.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-143">Call the `AddDbContext` method to register the context as a service.</span></span>

* <span data-ttu-id="1f0cf-144">Добавьте следующий выделенный код в метод `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1f0cf-144">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=13-14)]

<span data-ttu-id="1f0cf-145">Примечание. В реальном приложении строка подключения обычно помещается в файл конфигурации или переменную среды.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-145">Note: A real app would generally put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="1f0cf-146">В этом руководстве для простоты она задана в коде.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-146">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="1f0cf-147">Дополнительные сведения вы найдете в статье [Connection Strings](../../miscellaneous/connection-strings.md) (Строки подключения).</span><span class="sxs-lookup"><span data-stu-id="1f0cf-147">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="1f0cf-148">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="1f0cf-148">Create the database</span></span>

<span data-ttu-id="1f0cf-149">Когда модель будет готова, из нее можно создать базу данных с помощью [миграций](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations):</span><span class="sxs-lookup"><span data-stu-id="1f0cf-149">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="1f0cf-150">Последовательно выберите пункты **Средства > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-150">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="1f0cf-151">Запустите `Add-Migration InitialCreate`, чтобы сформировать шаблон миграции для создания начального набора таблиц для модели.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-151">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="1f0cf-152">Если появляется сообщение об ошибке `The term 'add-migration' is not recognized as the name of a cmdlet`, закройте и снова откройте Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-152">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="1f0cf-153">Запустите `Update-Database`, чтобы применить созданную миграцию к базе данных.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-153">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="1f0cf-154">Эта команда создает базу данных и применяет к ней миграции.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-154">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="1f0cf-155">Создание контроллера</span><span class="sxs-lookup"><span data-stu-id="1f0cf-155">Create a controller</span></span>

<span data-ttu-id="1f0cf-156">Добавьте шаблон контроллера и представлений для сущности `Blog`.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-156">Scaffold a controller and views for the `Blog` entity.</span></span>

* <span data-ttu-id="1f0cf-157">В **обозревателе решений** щелкните папку **Контроллеры** правой кнопкой мыши и выберите пункт **Добавить -> Контроллер**.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-157">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="1f0cf-158">Выберите **Контроллер MVC с представлениями, использующий Entity Framework** и щелкните **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-158">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="1f0cf-159">Для параметра **Класс модели** установите значение **Блог**, а для параметра **Класс контекста данных** — **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-159">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="1f0cf-160">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-160">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="1f0cf-161">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="1f0cf-161">Run the application</span></span>

<span data-ttu-id="1f0cf-162">Нажмите клавишу F5, чтобы запустить приложение для проверки.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-162">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="1f0cf-163">Перейдите к папке `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-163">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="1f0cf-164">С помощью ссылки создания внесите в блог несколько записей.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-164">Use the create link to create some blog entries.</span></span> <span data-ttu-id="1f0cf-165">Проверьте также работу ссылок для просмотра и удаления.</span><span class="sxs-lookup"><span data-stu-id="1f0cf-165">Test the details and delete links.</span></span>

![изображение](_static/create.png)

![изображение](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="1f0cf-168">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="1f0cf-168">Additional Resources</span></span>

* <span data-ttu-id="1f0cf-169">[Руководство по применению кроссплатформенной консоли EF с новой базой данных SQLite](xref:core/get-started/netcore/new-db-sqlite)</span><span class="sxs-lookup"><span data-stu-id="1f0cf-169">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="1f0cf-170">Введение в ASP.NET Core MVC для Mac и Linux</span><span class="sxs-lookup"><span data-stu-id="1f0cf-170">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="1f0cf-171">Введение в ASP.NET Core MVC для Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1f0cf-171">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="1f0cf-172">Начало работы с ASP.NET Core и Entity Framework Core с использованием Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1f0cf-172">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
