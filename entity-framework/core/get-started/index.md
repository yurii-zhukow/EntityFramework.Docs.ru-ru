---
title: Начало работы — EF Core
author: rick-anderson
ms.date: 09/17/2019
ms.assetid: 3c88427c-20c6-42ec-a736-22d3eccd5071
uid: core/get-started/index
ms.openlocfilehash: b45a7e5010dd3ef37ea952aaf483847ae95ea4ef
ms.sourcegitcommit: 1f0f93c66b2b50e03fcbed90260e94faa0279c46
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2020
ms.locfileid: "84418952"
---
# <a name="getting-started-with-ef-core"></a><span data-ttu-id="e1ac9-102">Начало работы с EF Core</span><span class="sxs-lookup"><span data-stu-id="e1ac9-102">Getting Started with EF Core</span></span>

<span data-ttu-id="e1ac9-103">В этом руководстве вы создадите консольное приложение .NET Core, которое осуществляет доступ к базе данных SQLite с помощью Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-103">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="e1ac9-104">Инструкции из этого руководства можно выполнять с помощью Visual Studio 2017 в Windows, а также .NET Core CLI в Windows, macOS или Linux.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-104">You can follow the tutorial by using Visual Studio on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="e1ac9-105">[Пример для этой статьи на GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span><span class="sxs-lookup"><span data-stu-id="e1ac9-105">[View this article's sample on GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e1ac9-106">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="e1ac9-106">Prerequisites</span></span>

<span data-ttu-id="e1ac9-107">Установите следующее программное обеспечение:</span><span class="sxs-lookup"><span data-stu-id="e1ac9-107">Install the following software:</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e1ac9-108">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="e1ac9-108">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="e1ac9-109">[пакет SDK для .NET Core](https://www.microsoft.com/net/download/core);</span><span class="sxs-lookup"><span data-stu-id="e1ac9-109">[.NET Core SDK](https://www.microsoft.com/net/download/core).</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="e1ac9-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1ac9-110">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e1ac9-111">[Visual Studio 2019 16.3 или последующей версии](https://www.visualstudio.com/downloads/) с этой рабочей нагрузкой:</span><span class="sxs-lookup"><span data-stu-id="e1ac9-111">[Visual Studio 2019 version 16.3 or later](https://www.visualstudio.com/downloads/) with this  workload:</span></span>
  * <span data-ttu-id="e1ac9-112">**Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)</span><span class="sxs-lookup"><span data-stu-id="e1ac9-112">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="e1ac9-113">Создание нового проекта</span><span class="sxs-lookup"><span data-stu-id="e1ac9-113">Create a new project</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e1ac9-114">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="e1ac9-114">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new console -o EFGetStarted
cd EFGetStarted
```

### <a name="visual-studio"></a>[<span data-ttu-id="e1ac9-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1ac9-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e1ac9-116">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1ac9-116">Open Visual Studio</span></span>
* <span data-ttu-id="e1ac9-117">Щелкните **Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-117">Click **Create a new project**</span></span>
* <span data-ttu-id="e1ac9-118">Выберите **Консольное приложение (.NET Core )** с тегом **C#** и щелкните **Далее**.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-118">Select **Console App (.NET Core)** with the **C#** tag and click **Next**</span></span>
* <span data-ttu-id="e1ac9-119">Задайте проекту имя **EFGetStarted** и щелкните **Создать**.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-119">Enter **EFGetStarted** for the name and click **Create**</span></span>

---

## <a name="install-entity-framework-core"></a><span data-ttu-id="e1ac9-120">Установка Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e1ac9-120">Install Entity Framework Core</span></span>

<span data-ttu-id="e1ac9-121">Чтобы установить EF Core, установите пакеты целевых поставщиков базы данных EF Core, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-121">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="e1ac9-122">В этом руководстве используется SQLite, так как он работает на всех платформах, поддерживаемых .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-122">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span> <span data-ttu-id="e1ac9-123">Список доступных поставщиков см. в разделе [Database Providers](../providers/index.md) (Поставщики базы данных).</span><span class="sxs-lookup"><span data-stu-id="e1ac9-123">For a list of available providers, see [Database Providers](../providers/index.md).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e1ac9-124">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="e1ac9-124">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[<span data-ttu-id="e1ac9-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1ac9-125">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e1ac9-126">Последовательно выберите пункты **Средства > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-126">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="e1ac9-127">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="e1ac9-127">Run the following commands:</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Sqlite
  ```

<span data-ttu-id="e1ac9-128">Совет. Можно также установить пакеты, щелкнув проект правой кнопкой мыши и выбрав **Управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-128">Tip: You can also install packages by right-clicking on the project and selecting **Manage NuGet Packages**</span></span>

---

## <a name="create-the-model"></a><span data-ttu-id="e1ac9-129">Создание модели</span><span class="sxs-lookup"><span data-stu-id="e1ac9-129">Create the model</span></span>

<span data-ttu-id="e1ac9-130">Задайте класс контекста и классы сущностей, составляющие модель.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-130">Define a context class and entity classes that make up the model.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e1ac9-131">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="e1ac9-131">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="e1ac9-132">В каталоге проекта создайте файл **Model.cs** с таким кодом:</span><span class="sxs-lookup"><span data-stu-id="e1ac9-132">In the project directory, create **Model.cs** with the following code</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="e1ac9-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1ac9-133">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e1ac9-134">Щелкните проект правой кнопкой мыши и выберите **Добавить > Класс**.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-134">Right-click on the project and select **Add > Class**</span></span>
* <span data-ttu-id="e1ac9-135">Задайте имя **Model.cs** и щелкните **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-135">Enter **Model.cs** as the name and click **Add**</span></span>
* <span data-ttu-id="e1ac9-136">Замените все содержимое этого файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="e1ac9-136">Replace the contents of the file with the following code</span></span>

---

[!code-csharp[Main](../../../samples/core/GetStarted/Model.cs)]

<span data-ttu-id="e1ac9-137">В EF Core также можно [реконструировать](../managing-schemas/scaffolding.md) модель из существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-137">EF Core can also [reverse engineer](../managing-schemas/scaffolding.md) a model from an existing database.</span></span>

<span data-ttu-id="e1ac9-138">Совет. Эта программа намеренно упрощена для наглядности.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-138">Tip: This application intentionally keeps things simple for clarity.</span></span> <span data-ttu-id="e1ac9-139">В приложениях для рабочей среды не следует хранить [строки подключений](../miscellaneous/connection-strings.md) в коде.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-139">[Connection strings](../miscellaneous/connection-strings.md) should not be stored in the code for production applications.</span></span> <span data-ttu-id="e1ac9-140">Возможно, также будет лучше поместить каждый класс C# в отдельный файл.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-140">You may also want to split each C# class into its own file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="e1ac9-141">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="e1ac9-141">Create the database</span></span>

<span data-ttu-id="e1ac9-142">В следующих действиях используются [миграции](xref:core/managing-schemas/migrations/index) для создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-142">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e1ac9-143">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="e1ac9-143">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="e1ac9-144">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="e1ac9-144">Run the following commands:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="e1ac9-145">При этом устанавливается [dotnet ef](../miscellaneous/cli/dotnet.md) и пакет конструктора, требуемый для выполнения команды в проекте.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-145">This installs [dotnet ef](../miscellaneous/cli/dotnet.md) and the design package which is required to run the command on a project.</span></span> <span data-ttu-id="e1ac9-146">Команда `migrations` формирует шаблон миграции для создания начального набора таблиц в модели.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-146">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="e1ac9-147">Команда `database update` создает базу данных и применяет к ней созданную миграцию.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-147">The `database update` command creates the database and applies the new migration to it.</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="e1ac9-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1ac9-148">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e1ac9-149">В **консоли диспетчера пакетов** выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="e1ac9-149">Run the following commands in **Package Manager Console**</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Tools
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="e1ac9-150">При этом устанавливаются [средства консоли диспетчера пакетов для EF Core](../miscellaneous/cli/powershell.md).</span><span class="sxs-lookup"><span data-stu-id="e1ac9-150">This installs the [PMC tools for EF Core](../miscellaneous/cli/powershell.md).</span></span> <span data-ttu-id="e1ac9-151">Команда `Add-Migration` формирует шаблон миграции для создания начального набора таблиц в модели.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-151">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="e1ac9-152">Команда `Update-Database` создает базу данных и применяет к ней созданную миграцию.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-152">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-read-update--delete"></a><span data-ttu-id="e1ac9-153">Создание, чтение, обновление и удаление</span><span class="sxs-lookup"><span data-stu-id="e1ac9-153">Create, read, update & delete</span></span>

* <span data-ttu-id="e1ac9-154">Откройте файл *Program.cs* и замените его содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="e1ac9-154">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../samples/core/GetStarted/Program.cs)]

## <a name="run-the-app"></a><span data-ttu-id="e1ac9-155">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="e1ac9-155">Run the app</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="e1ac9-156">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="e1ac9-156">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet run
```

### <a name="visual-studio"></a>[<span data-ttu-id="e1ac9-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1ac9-157">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e1ac9-158">В Visual Studio используется неподходящий рабочий каталог при запуске консольных приложений .NET Core</span><span class="sxs-lookup"><span data-stu-id="e1ac9-158">Visual Studio uses an inconsistent working directory when running .NET Core console apps.</span></span> <span data-ttu-id="e1ac9-159">(см. [dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619)). Это вызывает исключение, информирующее об *отсутствии таблицы (блоги)* .</span><span class="sxs-lookup"><span data-stu-id="e1ac9-159">(see [dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619)) This results in an exception being thrown: *no such table: Blogs*.</span></span> <span data-ttu-id="e1ac9-160">Чтобы обновить рабочий каталог, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="e1ac9-160">To update the working directory:</span></span>

* <span data-ttu-id="e1ac9-161">Щелкните проект правой кнопкой мыши и выберите **Изменить файл проекта**.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-161">Right-click on the project and select **Edit Project File**</span></span>
* <span data-ttu-id="e1ac9-162">Непосредственно под свойством *TargetFramework* добавьте следующее:</span><span class="sxs-lookup"><span data-stu-id="e1ac9-162">Just below the *TargetFramework* property, add the following:</span></span>

  ``` XML
  <StartWorkingDirectory>$(MSBuildProjectDirectory)</StartWorkingDirectory>
  ```

* <span data-ttu-id="e1ac9-163">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-163">Save the file</span></span>

<span data-ttu-id="e1ac9-164">Теперь можно запустить приложение:</span><span class="sxs-lookup"><span data-stu-id="e1ac9-164">Now you can run the app:</span></span>

* <span data-ttu-id="e1ac9-165">**"Отладка" > "Запустить без отладки"**</span><span class="sxs-lookup"><span data-stu-id="e1ac9-165">**Debug > Start Without Debugging**</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="e1ac9-166">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="e1ac9-166">Next steps</span></span>

* <span data-ttu-id="e1ac9-167">Следуйте инструкциям из [руководства по ASP.NET Core](/aspnet/core/data/ef-rp/intro), чтобы использовать EF Core в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-167">Follow the [ASP.NET Core Tutorial](/aspnet/core/data/ef-rp/intro) to use EF Core in a web app</span></span>
* <span data-ttu-id="e1ac9-168">См. сведения о [выражениях запросов LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations).</span><span class="sxs-lookup"><span data-stu-id="e1ac9-168">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
* <span data-ttu-id="e1ac9-169">[Настройте модель](xref:core/modeling/index), указав [требуемую](xref:core/modeling/entity-properties#required-and-optional-properties) и [максимальную длину](xref:core/modeling/entity-properties#maximum-length).</span><span class="sxs-lookup"><span data-stu-id="e1ac9-169">[Configure your model](xref:core/modeling/index) to specify things like [required](xref:core/modeling/entity-properties#required-and-optional-properties) and [maximum length](xref:core/modeling/entity-properties#maximum-length)</span></span>
* <span data-ttu-id="e1ac9-170">Используйте [миграции](xref:core/managing-schemas/migrations/index) для обновления схемы базы данных после изменения модели.</span><span class="sxs-lookup"><span data-stu-id="e1ac9-170">Use [Migrations](xref:core/managing-schemas/migrations/index) to update the database schema after changing your model</span></span>
