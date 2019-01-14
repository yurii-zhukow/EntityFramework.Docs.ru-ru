---
title: Начало работы в ASP.NET Core — новая база данных — Core EF
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 4734586adc89e9c1d866a1b4accd8b5e51fe2bb0
ms.sourcegitcommit: ebf661025d2ad2b62466fa7bf0e0772a7811cbe7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2019
ms.locfileid: "54211170"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="6a7d2-102">Начало работы с EF Core в ASP.NET Core с новой базой данных</span><span class="sxs-lookup"><span data-stu-id="6a7d2-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="6a7d2-103">В этом руководстве вы создадите приложение ASP.NET Core MVC, которое выполняет базовые операции доступа к данным через платформу Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="6a7d2-104">В этом руководстве используются миграции для создания базы данных из модели данных.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-104">The tutorial uses migrations to create the database from the data model.</span></span>

<span data-ttu-id="6a7d2-105">Вы можете выполнять это руководство с помощью Visual Studio 2017 в Windows или с помощью интерфейса командной строки .NET Core CLI в Windows, macOS или Linux.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-105">You can follow the tutorial by using Visual Studio 2017 on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="6a7d2-106">Пример для этой статьи на GitHub:</span><span class="sxs-lookup"><span data-stu-id="6a7d2-106">View this article's sample on GitHub:</span></span>
* [<span data-ttu-id="6a7d2-107">Visual Studio 2017 с SQL Server</span><span class="sxs-lookup"><span data-stu-id="6a7d2-107">Visual Studio 2017 with SQL Server</span></span>](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* <span data-ttu-id="6a7d2-108">[.NET Core CLI с SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span><span class="sxs-lookup"><span data-stu-id="6a7d2-108">[.NET Core CLI with SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6a7d2-109">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="6a7d2-109">Prerequisites</span></span>

<span data-ttu-id="6a7d2-110">Установите следующее программное обеспечение:</span><span class="sxs-lookup"><span data-stu-id="6a7d2-110">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a7d2-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a7d2-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6a7d2-112">[Visual Studio 2017 версии 15.7 или более поздней](https://www.visualstudio.com/downloads/) со следующими рабочими нагрузками:</span><span class="sxs-lookup"><span data-stu-id="6a7d2-112">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="6a7d2-113">**ASP.NET и веб-разработка** в разделе **Интернет и облако**)</span><span class="sxs-lookup"><span data-stu-id="6a7d2-113">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="6a7d2-114">**Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)</span><span class="sxs-lookup"><span data-stu-id="6a7d2-114">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="6a7d2-115">[Пакет SDK для NET Core 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="6a7d2-115">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6a7d2-116">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="6a7d2-116">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="6a7d2-117">[Пакет SDK для NET Core 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="6a7d2-117">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="6a7d2-118">Создание нового проекта</span><span class="sxs-lookup"><span data-stu-id="6a7d2-118">Create a new project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a7d2-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a7d2-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6a7d2-120">Откройте Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-120">Open Visual Studio 2017</span></span>
* <span data-ttu-id="6a7d2-121">Последовательно выберите **Файл > Создать > Проект**.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-121">**File > New > Project**</span></span>
* <span data-ttu-id="6a7d2-122">В меню слева выберите **Установленные > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-122">From the left menu, select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="6a7d2-123">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-123">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="6a7d2-124">Введите имя проекта **EFGetStarted.AspNetCore.NewDb** и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-124">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="6a7d2-125">В диалоговом окне **Создание веб-приложения ASP.NET Core** сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-125">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="6a7d2-126">Выберите **.NET Core** и **ASP.NET Core 2.1** в раскрывающихся списках.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-126">Make sure that **.NET Core** and **ASP.NET Core 2.1** are selected in the drop-down lists</span></span>
  * <span data-ttu-id="6a7d2-127">Выберите **Web Application (Model-View-Controller)** (Веб-приложение "модель — представление — контроллер").</span><span class="sxs-lookup"><span data-stu-id="6a7d2-127">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="6a7d2-128">Убедитесь, что для параметра **Проверка подлинности** задано значение **Без проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-128">Make sure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="6a7d2-129">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-129">Click **OK**</span></span>

<span data-ttu-id="6a7d2-130">Предупреждение. Если для параметра **Проверка подлинности** установить значение **Учетные записи отдельных пользователей** вместо **Без проверки подлинности**, в проект будет добавлен файл `Models\IdentityModel.cs` с моделью Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-130">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to the project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="6a7d2-131">Используя методы, представленные в этом руководстве, вы можете добавить еще одну модель или расширить уже созданную, включив в нее ваши классы сущностей.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-131">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6a7d2-132">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="6a7d2-132">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="6a7d2-133">Выполните следующую команду для создания проекта MVC.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-133">Run the following command to create an MVC project:</span></span>

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* <span data-ttu-id="6a7d2-134">Перейдите в каталог проекта.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-134">Change to the project directory.</span></span> <span data-ttu-id="6a7d2-135">Следующие команды нужно выполнять для нового проекта.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-135">The next commands you enter need to run against the new project.</span></span>

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a><span data-ttu-id="6a7d2-136">Установка Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="6a7d2-136">Install Entity Framework Core</span></span>

<span data-ttu-id="6a7d2-137">Чтобы установить EF Core, установите пакеты целевых поставщиков базы данных EF Core, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="6a7d2-138">Список доступных поставщиков см. в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).</span><span class="sxs-lookup"><span data-stu-id="6a7d2-138">For a list of available providers, see [Database Providers](../../providers/index.md).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a7d2-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a7d2-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6a7d2-140">В этом руководстве используется SQL Server, поэтому устанавливать пакет поставщиков не требуется.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-140">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="6a7d2-141">Пакет поставщиков SQL Server включен в [метапакет Microsoft.AspnetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="6a7d2-141">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6a7d2-142">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="6a7d2-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="6a7d2-143">В этом руководстве используется SQLite, так как он работает на всех платформах, поддерживаемых .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-143">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span>

* <span data-ttu-id="6a7d2-144">Чтобы установить поставщика SQLite, выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6a7d2-144">Run the following command to install the SQLite provider:</span></span>

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a><span data-ttu-id="6a7d2-145">Создание модели</span><span class="sxs-lookup"><span data-stu-id="6a7d2-145">Create the model</span></span>

<span data-ttu-id="6a7d2-146">Задайте класс контекста и классы сущностей, составляющие модель.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-146">Define a context class and entity classes that make up the model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a7d2-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a7d2-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6a7d2-148">Щелкните папку **Models** (Модели) правой кнопкой мыши и выберите **Добавить > Класс**.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-148">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="6a7d2-149">Введите имя класса **Model.cs** и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-149">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="6a7d2-150">Замените все содержимое этого файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6a7d2-150">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6a7d2-151">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="6a7d2-151">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="6a7d2-152">В папке **Models** создайте файл **Model.cs**, содержащий следующий код:</span><span class="sxs-lookup"><span data-stu-id="6a7d2-152">In the **Models** folder create **Model.cs** with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

<span data-ttu-id="6a7d2-153">Рабочее приложение обычно помещает каждый класс в отдельный файл.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-153">A production app would typically put each class in a separate file.</span></span> <span data-ttu-id="6a7d2-154">В этом руководстве для простоты все классы помещаются в один файл.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-154">For the sake of simplicity, this tutorial puts these classes in one file.</span></span>

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="6a7d2-155">Регистрация контекста с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="6a7d2-155">Register the context with dependency injection</span></span>

<span data-ttu-id="6a7d2-156">С помощью [внедрения зависимостей](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) службы (например, `BloggingContext`) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-156">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="6a7d2-157">Компоненты, которым требуются эти службы (например, контроллеры MVC), обращаются к ним через свойства или параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-157">Components that require these services (such as MVC controllers) are provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="6a7d2-158">Чтобы сделать контекст `BloggingContext` доступным MVC-контроллерам, зарегистрируйте его как службу.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-158">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a7d2-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a7d2-159">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6a7d2-160">Добавьте в файл **Startup.cs** следующие инструкции `using`:</span><span class="sxs-lookup"><span data-stu-id="6a7d2-160">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* <span data-ttu-id="6a7d2-161">Добавьте следующий выделенный код в метод `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6a7d2-161">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6a7d2-162">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="6a7d2-162">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="6a7d2-163">Добавьте в файл **Startup.cs** следующие инструкции `using`:</span><span class="sxs-lookup"><span data-stu-id="6a7d2-163">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* <span data-ttu-id="6a7d2-164">Добавьте следующий выделенный код в метод `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6a7d2-164">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

<span data-ttu-id="6a7d2-165">В реальном рабочем приложении строка подключения обычно помещается в файл конфигурации или в переменную среды.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-165">A production app would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="6a7d2-166">В этом руководстве для простоты она задана в коде.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-166">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="6a7d2-167">Дополнительные сведения вы найдете в статье [Connection Strings](../../miscellaneous/connection-strings.md) (Строки подключения).</span><span class="sxs-lookup"><span data-stu-id="6a7d2-167">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="6a7d2-168">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="6a7d2-168">Create the database</span></span>

<span data-ttu-id="6a7d2-169">В следующих действиях используются [миграции](xref:core/managing-schemas/migrations/index) для создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-169">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a7d2-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a7d2-170">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6a7d2-171">Последовательно выберите пункты **Средства > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-171">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="6a7d2-172">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="6a7d2-172">Run the following commands:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="6a7d2-173">Если появляется сообщение об ошибке `The term 'add-migration' is not recognized as the name of a cmdlet`, закройте и снова откройте Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-173">If you get an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>

  <span data-ttu-id="6a7d2-174">Команда `Add-Migration` формирует шаблон миграции для создания начального набора таблиц в модели.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-174">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="6a7d2-175">Команда `Update-Database` создает базу данных и применяет к ней созданную миграцию.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-175">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6a7d2-176">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="6a7d2-176">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="6a7d2-177">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="6a7d2-177">Run the following commands:</span></span>

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="6a7d2-178">Команда `migrations` формирует шаблон миграции для создания начального набора таблиц в модели.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-178">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="6a7d2-179">Команда `database update` создает базу данных и применяет к ней созданную миграцию.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-179">The `database update` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-a-controller"></a><span data-ttu-id="6a7d2-180">Создание контроллера</span><span class="sxs-lookup"><span data-stu-id="6a7d2-180">Create a controller</span></span>

<span data-ttu-id="6a7d2-181">Добавьте шаблон контроллера и представлений для сущности `Blog`.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-181">Scaffold a controller and views for the `Blog` entity.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a7d2-182">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a7d2-182">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6a7d2-183">В **обозревателе решений** щелкните папку **Контроллеры** правой кнопкой мыши и выберите пункт **Добавить -> Контроллер**.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-183">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="6a7d2-184">Выберите **Контроллер MVC с представлениями, использующий Entity Framework** и щелкните **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-184">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="6a7d2-185">Для параметра **Класс модели** установите значение **Блог**, а для параметра **Класс контекста данных** — **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-185">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="6a7d2-186">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-186">Click **Add**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6a7d2-187">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="6a7d2-187">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="6a7d2-188">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="6a7d2-188">Run the following commands:</span></span>

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  <span data-ttu-id="6a7d2-189">Команды `tool install` и `add package` устанавливают средства, которые могут формировать контроллеры и представления.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-189">The `tool install` and `add package` commands install the tooling that can scaffold controllers and views.</span></span> <span data-ttu-id="6a7d2-190">Команда `restore` удостоверяется, что все пакеты проекта загружены, а команда `aspnet-codegenerator` выполняет формирование.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-190">The `restore` command ensures that all of the project's packages are downloaded, and the `aspnet-codegenerator` command does the scaffolding.</span></span>
---

<span data-ttu-id="6a7d2-191">Подсистема формирования шаблонов создает следующие файлы:</span><span class="sxs-lookup"><span data-stu-id="6a7d2-191">The scaffolding engine creates the following files:</span></span>

* <span data-ttu-id="6a7d2-192">файл контроллера (*Controllers/MoviesController.cs*);</span><span class="sxs-lookup"><span data-stu-id="6a7d2-192">A controller (*Controllers/BlogsController.cs*)</span></span>
* <span data-ttu-id="6a7d2-193">файлы представления Razor для страниц Create, Delete, Details, Edit и Index (_Views/Blogs/\*.cshtml_).</span><span class="sxs-lookup"><span data-stu-id="6a7d2-193">Razor views for Create, Delete, Details, Edit, and Index pages (_Views/Blogs/\*.cshtml_)</span></span>

## <a name="run-the-application"></a><span data-ttu-id="6a7d2-194">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="6a7d2-194">Run the application</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a7d2-195">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a7d2-195">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6a7d2-196">**Отладка** > **Запуск без отладки**</span><span class="sxs-lookup"><span data-stu-id="6a7d2-196">**Debug** > **Start Without Debugging**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6a7d2-197">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="6a7d2-197">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet run
```
---

* <span data-ttu-id="6a7d2-198">Перейдите к папке `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-198">Navigate to `/Blogs`</span></span>

* <span data-ttu-id="6a7d2-199">С помощью ссылки **Create New** (Создать новую запись) создайте несколько записей в блоге.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-199">Use the **Create New** link to create some blog entries.</span></span>

  ![Страница "Создать"](_static/create.png)

* <span data-ttu-id="6a7d2-201">Проверьте работу ссылок **Details**, **Edit** и **Delete**.</span><span class="sxs-lookup"><span data-stu-id="6a7d2-201">Test the **Details**, **Edit**, and **Delete** links.</span></span>

  ![Страница индексов](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="6a7d2-203">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6a7d2-203">Additional Resources</span></span>

* <span data-ttu-id="6a7d2-204">Руководство [ Начало работы с EF Core в консольном приложении .NET Core с новой базой данных](xref:core/get-started/netcore/new-db-sqlite)</span><span class="sxs-lookup"><span data-stu-id="6a7d2-204">[Tutorial: Get started with EF Core on .NET Core with a new database using SQLite](xref:core/get-started/netcore/new-db-sqlite)</span></span>
* [<span data-ttu-id="6a7d2-205">Учебник. Начало работы с Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6a7d2-205">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="6a7d2-206">Учебник. Razor Pages с Entity Framework Core в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6a7d2-206">Tutorial: Razor Pages with Entity Framework Core in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
