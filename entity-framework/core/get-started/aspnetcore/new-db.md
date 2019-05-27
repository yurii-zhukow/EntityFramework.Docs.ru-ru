---
title: Начало работы в ASP.NET Core — новая база данных — Core EF
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: fbc1a00d6d6d0624bcbbfa1e51f4e21a915baaaa
ms.sourcegitcommit: f277883a5ed28eba57d14aaaf17405bc1ae9cf94
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/18/2019
ms.locfileid: "65874567"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="cc9a8-102">Начало работы с EF Core в ASP.NET Core с новой базой данных</span><span class="sxs-lookup"><span data-stu-id="cc9a8-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="cc9a8-103">В этом руководстве вы создадите приложение ASP.NET Core MVC, которое выполняет базовые операции доступа к данным через платформу Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="cc9a8-104">В этом руководстве используются миграции для создания базы данных из модели данных.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-104">The tutorial uses migrations to create the database from the data model.</span></span>

<span data-ttu-id="cc9a8-105">Вы можете выполнять это руководство с помощью Visual Studio 2017 в Windows или с помощью интерфейса командной строки .NET Core CLI в Windows, macOS или Linux.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-105">You can follow the tutorial by using Visual Studio 2017 on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="cc9a8-106">Пример для этой статьи на GitHub:</span><span class="sxs-lookup"><span data-stu-id="cc9a8-106">View this article's sample on GitHub:</span></span>
* [<span data-ttu-id="cc9a8-107">Visual Studio 2017 с SQL Server</span><span class="sxs-lookup"><span data-stu-id="cc9a8-107">Visual Studio 2017 with SQL Server</span></span>](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* <span data-ttu-id="cc9a8-108">[.NET Core CLI с SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span><span class="sxs-lookup"><span data-stu-id="cc9a8-108">[.NET Core CLI with SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cc9a8-109">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="cc9a8-109">Prerequisites</span></span>

<span data-ttu-id="cc9a8-110">Установите следующее программное обеспечение:</span><span class="sxs-lookup"><span data-stu-id="cc9a8-110">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cc9a8-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cc9a8-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cc9a8-112">[Visual Studio 2017 версии 15.7 или более поздней](https://www.visualstudio.com/downloads/) со следующими рабочими нагрузками:</span><span class="sxs-lookup"><span data-stu-id="cc9a8-112">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="cc9a8-113">**ASP.NET и веб-разработка** в разделе **Интернет и облако**)</span><span class="sxs-lookup"><span data-stu-id="cc9a8-113">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="cc9a8-114">**Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)</span><span class="sxs-lookup"><span data-stu-id="cc9a8-114">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="cc9a8-115">[Пакет SDK для NET Core 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="cc9a8-115">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cc9a8-116">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="cc9a8-116">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="cc9a8-117">[Пакет SDK для NET Core 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="cc9a8-117">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="cc9a8-118">Создание нового проекта</span><span class="sxs-lookup"><span data-stu-id="cc9a8-118">Create a new project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cc9a8-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cc9a8-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cc9a8-120">Откройте Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-120">Open Visual Studio 2017</span></span>
* <span data-ttu-id="cc9a8-121">Последовательно выберите **Файл > Создать > Проект**.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-121">**File > New > Project**</span></span>
* <span data-ttu-id="cc9a8-122">В меню слева выберите **Установленные > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-122">From the left menu, select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="cc9a8-123">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-123">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="cc9a8-124">Введите имя проекта **EFGetStarted.AspNetCore.NewDb** и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-124">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="cc9a8-125">В диалоговом окне **Создание веб-приложения ASP.NET Core** сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-125">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="cc9a8-126">Выберите **.NET Core** и **ASP.NET Core 2.1** в раскрывающихся списках.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-126">Make sure that **.NET Core** and **ASP.NET Core 2.1** are selected in the drop-down lists</span></span>
  * <span data-ttu-id="cc9a8-127">Выберите **Web Application (Model-View-Controller)** (Веб-приложение "модель — представление — контроллер").</span><span class="sxs-lookup"><span data-stu-id="cc9a8-127">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="cc9a8-128">Убедитесь, что для параметра **Проверка подлинности** задано значение **Без проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-128">Make sure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="cc9a8-129">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-129">Click **OK**</span></span>

<span data-ttu-id="cc9a8-130">Предупреждение: Если для параметра **Проверка подлинности** установить значение **Учетные записи отдельных пользователей** вместо **Без проверки подлинности**, в проект будет добавлен файл `Models\IdentityModel.cs` с моделью Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-130">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to the project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="cc9a8-131">Используя методы, представленные в этом руководстве, вы можете добавить еще одну модель или расширить уже созданную, включив в нее ваши классы сущностей.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-131">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cc9a8-132">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="cc9a8-132">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="cc9a8-133">Выполните следующую команду для создания проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-133">Run the following command to create an MVC project:</span></span>

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* <span data-ttu-id="cc9a8-134">Перейдите в каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-134">Change to the project directory.</span></span> <span data-ttu-id="cc9a8-135">Следующие команды нужно выполнять для нового проекта.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-135">The next commands you enter need to run against the new project.</span></span>

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a><span data-ttu-id="cc9a8-136">Установка Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="cc9a8-136">Install Entity Framework Core</span></span>

<span data-ttu-id="cc9a8-137">Чтобы установить EF Core, установите пакеты целевых поставщиков базы данных EF Core, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="cc9a8-138">Список доступных поставщиков см. в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).</span><span class="sxs-lookup"><span data-stu-id="cc9a8-138">For a list of available providers, see [Database Providers](../../providers/index.md).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cc9a8-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cc9a8-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="cc9a8-140">В этом руководстве используется SQL Server, поэтому устанавливать пакет поставщиков не требуется.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-140">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="cc9a8-141">Пакет поставщиков SQL Server включен в [метапакет Microsoft.AspnetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="cc9a8-141">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cc9a8-142">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="cc9a8-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="cc9a8-143">В этом руководстве используется SQLite, так как он работает на всех платформах, поддерживаемых .NET Core.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-143">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span>

* <span data-ttu-id="cc9a8-144">Чтобы установить поставщика SQLite, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="cc9a8-144">Run the following command to install the SQLite provider:</span></span>

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a><span data-ttu-id="cc9a8-145">Создание модели</span><span class="sxs-lookup"><span data-stu-id="cc9a8-145">Create the model</span></span>

<span data-ttu-id="cc9a8-146">Задайте класс контекста и классы сущностей, составляющие модель.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-146">Define a context class and entity classes that make up the model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cc9a8-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cc9a8-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cc9a8-148">Щелкните папку **Models** (Модели) правой кнопкой мыши и выберите **Добавить > Класс**.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-148">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="cc9a8-149">Введите имя класса **Model.cs** и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-149">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="cc9a8-150">Замените все содержимое этого файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="cc9a8-150">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cc9a8-151">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="cc9a8-151">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="cc9a8-152">В папке **Models** создайте файл **Model.cs**, содержащий следующий код:</span><span class="sxs-lookup"><span data-stu-id="cc9a8-152">In the **Models** folder create **Model.cs** with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

<span data-ttu-id="cc9a8-153">Рабочее приложение обычно помещает каждый класс в отдельный файл.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-153">A production app would typically put each class in a separate file.</span></span> <span data-ttu-id="cc9a8-154">В этом руководстве для простоты все классы помещаются в один файл.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-154">For the sake of simplicity, this tutorial puts these classes in one file.</span></span>

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="cc9a8-155">Регистрация контекста с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="cc9a8-155">Register the context with dependency injection</span></span>

<span data-ttu-id="cc9a8-156">Чтобы сделать контекст `BloggingContext` доступным контроллерам MVC, зарегистрируйте его как службу в `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-156">To make `BloggingContext` available to MVC controllers, register it as a service in `Startup.cs`.</span></span>

<span data-ttu-id="cc9a8-157">Службы (такие как `BloggingContext`) регистрируются с помощью [внедрения зависимостей](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) во время запуска приложения, чтобы компоненты, которым они требуются (например, контроллеры MVC), могли обращаться к ним автоматически через свойства и параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-157">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup so that they can be provided automatically to components that consume services (such as MVC controllers) via constructor parameters and properties.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cc9a8-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cc9a8-158">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cc9a8-159">Добавьте в файл **Startup.cs** следующие инструкции `using`:</span><span class="sxs-lookup"><span data-stu-id="cc9a8-159">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* <span data-ttu-id="cc9a8-160">Добавьте следующий выделенный код в метод `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="cc9a8-160">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cc9a8-161">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="cc9a8-161">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="cc9a8-162">Добавьте в файл **Startup.cs** следующие инструкции `using`:</span><span class="sxs-lookup"><span data-stu-id="cc9a8-162">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* <span data-ttu-id="cc9a8-163">Добавьте следующий выделенный код в метод `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="cc9a8-163">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

<span data-ttu-id="cc9a8-164">В реальном рабочем приложении строка подключения обычно помещается в файл конфигурации или в переменную среды.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-164">A production app would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="cc9a8-165">В этом руководстве для простоты она задана в коде.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-165">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="cc9a8-166">Дополнительные сведения вы найдете в статье [Connection Strings](../../miscellaneous/connection-strings.md) (Строки подключения).</span><span class="sxs-lookup"><span data-stu-id="cc9a8-166">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="cc9a8-167">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="cc9a8-167">Create the database</span></span>

<span data-ttu-id="cc9a8-168">В следующих действиях используются [миграции](xref:core/managing-schemas/migrations/index) для создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-168">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cc9a8-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cc9a8-169">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cc9a8-170">Последовательно выберите пункты **Средства > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-170">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="cc9a8-171">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="cc9a8-171">Run the following commands:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="cc9a8-172">Если появляется сообщение об ошибке `The term 'add-migration' is not recognized as the name of a cmdlet`, закройте и снова откройте Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-172">If you get an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>

  <span data-ttu-id="cc9a8-173">Команда `Add-Migration` формирует шаблон миграции для создания начального набора таблиц в модели.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-173">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="cc9a8-174">Команда `Update-Database` создает базу данных и применяет к ней созданную миграцию.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-174">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cc9a8-175">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="cc9a8-175">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="cc9a8-176">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="cc9a8-176">Run the following commands:</span></span>

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="cc9a8-177">Команда `migrations` формирует шаблон миграции для создания начального набора таблиц в модели.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-177">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="cc9a8-178">Команда `database update` создает базу данных и применяет к ней созданную миграцию.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-178">The `database update` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-a-controller"></a><span data-ttu-id="cc9a8-179">Создание контроллера</span><span class="sxs-lookup"><span data-stu-id="cc9a8-179">Create a controller</span></span>

<span data-ttu-id="cc9a8-180">Добавьте шаблон контроллера и представлений для сущности `Blog`.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-180">Scaffold a controller and views for the `Blog` entity.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cc9a8-181">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cc9a8-181">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cc9a8-182">В **обозревателе решений** щелкните папку **Контроллеры** правой кнопкой мыши и выберите пункт **Добавить -> Контроллер**.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-182">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="cc9a8-183">Выберите **Контроллер MVC с представлениями, использующий Entity Framework** и щелкните **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-183">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="cc9a8-184">Для параметра **Класс модели** установите значение **Блог**, а для параметра **Класс контекста данных** — **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-184">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="cc9a8-185">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-185">Click **Add**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cc9a8-186">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="cc9a8-186">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="cc9a8-187">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="cc9a8-187">Run the following commands:</span></span>

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  <span data-ttu-id="cc9a8-188">Команды `tool install` и `add package` устанавливают средства, которые могут формировать контроллеры и представления.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-188">The `tool install` and `add package` commands install the tooling that can scaffold controllers and views.</span></span> <span data-ttu-id="cc9a8-189">Команда `restore` удостоверяется, что все пакеты проекта загружены, а команда `aspnet-codegenerator` выполняет формирование.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-189">The `restore` command ensures that all of the project's packages are downloaded, and the `aspnet-codegenerator` command does the scaffolding.</span></span>
---

<span data-ttu-id="cc9a8-190">Подсистема формирования шаблонов создает следующие файлы:</span><span class="sxs-lookup"><span data-stu-id="cc9a8-190">The scaffolding engine creates the following files:</span></span>

* <span data-ttu-id="cc9a8-191">файл контроллера (*Controllers/MoviesController.cs*);</span><span class="sxs-lookup"><span data-stu-id="cc9a8-191">A controller (*Controllers/BlogsController.cs*)</span></span>
* <span data-ttu-id="cc9a8-192">файлы представления Razor для страниц Create, Delete, Details, Edit и Index (_Views/Blogs/\*.cshtml_).</span><span class="sxs-lookup"><span data-stu-id="cc9a8-192">Razor views for Create, Delete, Details, Edit, and Index pages (_Views/Blogs/\*.cshtml_)</span></span>

## <a name="run-the-application"></a><span data-ttu-id="cc9a8-193">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="cc9a8-193">Run the application</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cc9a8-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cc9a8-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cc9a8-195">**Отладка** > **Запуск без отладки**</span><span class="sxs-lookup"><span data-stu-id="cc9a8-195">**Debug** > **Start Without Debugging**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cc9a8-196">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="cc9a8-196">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet run
```
---

* <span data-ttu-id="cc9a8-197">Перейдите к папке `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-197">Navigate to `/Blogs`</span></span>

* <span data-ttu-id="cc9a8-198">С помощью ссылки **Create New** (Создать новую запись) создайте несколько записей в блоге.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-198">Use the **Create New** link to create some blog entries.</span></span>

  ![Страница "Создать"](_static/create.png)

* <span data-ttu-id="cc9a8-200">Проверьте работу ссылок **Details**, **Edit** и **Delete**.</span><span class="sxs-lookup"><span data-stu-id="cc9a8-200">Test the **Details**, **Edit**, and **Delete** links.</span></span>

  ![Страница индексов](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="cc9a8-202">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="cc9a8-202">Additional Resources</span></span>

* [<span data-ttu-id="cc9a8-203">Учебник. Начало работы с EF Core в консольном приложении .NET Core с новой базой данных</span><span class="sxs-lookup"><span data-stu-id="cc9a8-203">Tutorial: Get started with EF Core on .NET Core with a new database using SQLite</span></span>](xref:core/get-started/netcore/new-db-sqlite)
* <span data-ttu-id="cc9a8-204">[Начало работы с Razor Pages в ASP.NET Core](/aspnet/core/tutorials/razor-pages/razor-pages-start) или [Начало работы с ASP.NET Core MVC](/aspnet/core/tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="cc9a8-204">[Get started with Razor Pages in ASP.NET Core](/aspnet/core/tutorials/razor-pages/razor-pages-start) or [Get started with ASP.NET Core MVC ](/aspnet/core/tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="cc9a8-205">[Учебник. Razor Pages с Entity Framework Core в ASP.NET Core](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro) или [Учебник. Начало работы с EF Core в веб-приложении MVC ASP.NET](/aspnet/core/data/ef-mvc/intro)</span><span class="sxs-lookup"><span data-stu-id="cc9a8-205">[Tutorial: Razor Pages with Entity Framework Core in ASP.NET Core](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro) or [Get started with EF Core in an ASP.NET MVC web app](/aspnet/core/data/ef-mvc/intro)</span></span>
