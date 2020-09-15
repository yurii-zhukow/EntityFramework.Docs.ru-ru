---
title: Начало работы — EF Core
description: Руководство. Начало работы с Entity Framework Core
author: rick-anderson
ms.date: 09/17/2019
uid: core/get-started/index
ms.openlocfilehash: 9f0bb1eb99cb7f4cb7542c444ad86480917bdd0f
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90071983"
---
# <a name="getting-started-with-ef-core"></a><span data-ttu-id="b9a25-103">Начало работы с EF Core</span><span class="sxs-lookup"><span data-stu-id="b9a25-103">Getting Started with EF Core</span></span>

<span data-ttu-id="b9a25-104">В этом руководстве вы создадите консольное приложение .NET Core, которое осуществляет доступ к базе данных SQLite с помощью Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b9a25-104">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="b9a25-105">Инструкции из этого руководства можно выполнять с помощью Visual Studio 2017 в Windows, а также .NET Core CLI в Windows, macOS или Linux.</span><span class="sxs-lookup"><span data-stu-id="b9a25-105">You can follow the tutorial by using Visual Studio on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="b9a25-106">[Пример для этой статьи на GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span><span class="sxs-lookup"><span data-stu-id="b9a25-106">[View this article's sample on GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b9a25-107">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="b9a25-107">Prerequisites</span></span>

<span data-ttu-id="b9a25-108">Установите следующее программное обеспечение:</span><span class="sxs-lookup"><span data-stu-id="b9a25-108">Install the following software:</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="b9a25-109">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="b9a25-109">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="b9a25-110">[пакет SDK для .NET Core](https://www.microsoft.com/net/download/core);</span><span class="sxs-lookup"><span data-stu-id="b9a25-110">[.NET Core SDK](https://www.microsoft.com/net/download/core).</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="b9a25-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b9a25-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b9a25-112">[Visual Studio 2019 16.3 или последующей версии](https://www.visualstudio.com/downloads/) с этой рабочей нагрузкой:</span><span class="sxs-lookup"><span data-stu-id="b9a25-112">[Visual Studio 2019 version 16.3 or later](https://www.visualstudio.com/downloads/) with this  workload:</span></span>
  * <span data-ttu-id="b9a25-113">**Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)</span><span class="sxs-lookup"><span data-stu-id="b9a25-113">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="b9a25-114">Создание нового проекта</span><span class="sxs-lookup"><span data-stu-id="b9a25-114">Create a new project</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="b9a25-115">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="b9a25-115">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new console -o EFGetStarted
cd EFGetStarted
```

### <a name="visual-studio"></a>[<span data-ttu-id="b9a25-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b9a25-116">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b9a25-117">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b9a25-117">Open Visual Studio</span></span>
* <span data-ttu-id="b9a25-118">Щелкните **Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="b9a25-118">Click **Create a new project**</span></span>
* <span data-ttu-id="b9a25-119">Выберите **Консольное приложение (.NET Core )** с тегом **C#** и щелкните **Далее**.</span><span class="sxs-lookup"><span data-stu-id="b9a25-119">Select **Console App (.NET Core)** with the **C#** tag and click **Next**</span></span>
* <span data-ttu-id="b9a25-120">Задайте проекту имя **EFGetStarted** и щелкните **Создать**.</span><span class="sxs-lookup"><span data-stu-id="b9a25-120">Enter **EFGetStarted** for the name and click **Create**</span></span>

---

## <a name="install-entity-framework-core"></a><span data-ttu-id="b9a25-121">Установка Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="b9a25-121">Install Entity Framework Core</span></span>

<span data-ttu-id="b9a25-122">Чтобы установить EF Core, установите пакеты целевых поставщиков базы данных EF Core, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="b9a25-122">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="b9a25-123">В этом руководстве используется SQLite, так как он работает на всех платформах, поддерживаемых .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b9a25-123">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span> <span data-ttu-id="b9a25-124">Список доступных поставщиков см. в разделе [Database Providers](xref:core/providers/index) (Поставщики базы данных).</span><span class="sxs-lookup"><span data-stu-id="b9a25-124">For a list of available providers, see [Database Providers](xref:core/providers/index).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="b9a25-125">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="b9a25-125">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[<span data-ttu-id="b9a25-126">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b9a25-126">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b9a25-127">Последовательно выберите пункты **Средства > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="b9a25-127">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="b9a25-128">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="b9a25-128">Run the following commands:</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Sqlite
  ```

<span data-ttu-id="b9a25-129">Совет. Можно также установить пакеты, щелкнув проект правой кнопкой мыши и выбрав **Управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b9a25-129">Tip: You can also install packages by right-clicking on the project and selecting **Manage NuGet Packages**</span></span>

---

## <a name="create-the-model"></a><span data-ttu-id="b9a25-130">Создание модели</span><span class="sxs-lookup"><span data-stu-id="b9a25-130">Create the model</span></span>

<span data-ttu-id="b9a25-131">Задайте класс контекста и классы сущностей, составляющие модель.</span><span class="sxs-lookup"><span data-stu-id="b9a25-131">Define a context class and entity classes that make up the model.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="b9a25-132">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="b9a25-132">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="b9a25-133">В каталоге проекта создайте файл **Model.cs** с таким кодом:</span><span class="sxs-lookup"><span data-stu-id="b9a25-133">In the project directory, create **Model.cs** with the following code</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="b9a25-134">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b9a25-134">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b9a25-135">Щелкните проект правой кнопкой мыши и выберите **Добавить > Класс**.</span><span class="sxs-lookup"><span data-stu-id="b9a25-135">Right-click on the project and select **Add > Class**</span></span>
* <span data-ttu-id="b9a25-136">Задайте имя **Model.cs** и щелкните **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="b9a25-136">Enter **Model.cs** as the name and click **Add**</span></span>
* <span data-ttu-id="b9a25-137">Замените все содержимое этого файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="b9a25-137">Replace the contents of the file with the following code</span></span>

---

[!code-csharp[Main](../../../samples/core/GetStarted/Model.cs)]

<span data-ttu-id="b9a25-138">В EF Core также можно [реконструировать](xref:core/managing-schemas/scaffolding) модель из существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="b9a25-138">EF Core can also [reverse engineer](xref:core/managing-schemas/scaffolding) a model from an existing database.</span></span>

<span data-ttu-id="b9a25-139">Совет. Эта программа намеренно упрощена для наглядности.</span><span class="sxs-lookup"><span data-stu-id="b9a25-139">Tip: This application intentionally keeps things simple for clarity.</span></span> <span data-ttu-id="b9a25-140">В приложениях для рабочей среды не следует хранить [строки подключений](xref:core/miscellaneous/connection-strings) в коде.</span><span class="sxs-lookup"><span data-stu-id="b9a25-140">[Connection strings](xref:core/miscellaneous/connection-strings) should not be stored in the code for production applications.</span></span> <span data-ttu-id="b9a25-141">Возможно, также будет лучше поместить каждый класс C# в отдельный файл.</span><span class="sxs-lookup"><span data-stu-id="b9a25-141">You may also want to split each C# class into its own file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="b9a25-142">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="b9a25-142">Create the database</span></span>

<span data-ttu-id="b9a25-143">В следующих действиях используются [миграции](xref:core/managing-schemas/migrations/index) для создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="b9a25-143">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="b9a25-144">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="b9a25-144">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="b9a25-145">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="b9a25-145">Run the following commands:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="b9a25-146">При этом устанавливается [dotnet ef](xref:core/miscellaneous/cli/dotnet) и пакет конструктора, требуемый для выполнения команды в проекте.</span><span class="sxs-lookup"><span data-stu-id="b9a25-146">This installs [dotnet ef](xref:core/miscellaneous/cli/dotnet) and the design package which is required to run the command on a project.</span></span> <span data-ttu-id="b9a25-147">Команда `migrations` формирует шаблон миграции для создания начального набора таблиц в модели.</span><span class="sxs-lookup"><span data-stu-id="b9a25-147">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="b9a25-148">Команда `database update` создает базу данных и применяет к ней созданную миграцию.</span><span class="sxs-lookup"><span data-stu-id="b9a25-148">The `database update` command creates the database and applies the new migration to it.</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="b9a25-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b9a25-149">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b9a25-150">В **консоли диспетчера пакетов (PMC)** выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="b9a25-150">Run the following commands in **Package Manager Console (PMC)**</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Tools
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="b9a25-151">При этом устанавливаются [средства консоли диспетчера пакетов для EF Core](xref:core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="b9a25-151">This installs the [PMC tools for EF Core](xref:core/miscellaneous/cli/powershell).</span></span> <span data-ttu-id="b9a25-152">Команда `Add-Migration` формирует шаблон миграции для создания начального набора таблиц в модели.</span><span class="sxs-lookup"><span data-stu-id="b9a25-152">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="b9a25-153">Команда `Update-Database` создает базу данных и применяет к ней созданную миграцию.</span><span class="sxs-lookup"><span data-stu-id="b9a25-153">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-read-update--delete"></a><span data-ttu-id="b9a25-154">Создание, чтение, обновление и удаление</span><span class="sxs-lookup"><span data-stu-id="b9a25-154">Create, read, update & delete</span></span>

* <span data-ttu-id="b9a25-155">Откройте файл *Program.cs* и замените его содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="b9a25-155">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../samples/core/GetStarted/Program.cs)]

## <a name="run-the-app"></a><span data-ttu-id="b9a25-156">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="b9a25-156">Run the app</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="b9a25-157">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="b9a25-157">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet run
```

### <a name="visual-studio"></a>[<span data-ttu-id="b9a25-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b9a25-158">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b9a25-159">В Visual Studio используется неподходящий рабочий каталог при запуске консольных приложений .NET Core</span><span class="sxs-lookup"><span data-stu-id="b9a25-159">Visual Studio uses an inconsistent working directory when running .NET Core console apps.</span></span> <span data-ttu-id="b9a25-160">(см. [dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619)). Это вызывает исключение, информирующее об *отсутствии таблицы (блоги)* .</span><span class="sxs-lookup"><span data-stu-id="b9a25-160">(see [dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619)) This results in an exception being thrown: *no such table: Blogs*.</span></span> <span data-ttu-id="b9a25-161">Чтобы обновить рабочий каталог, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="b9a25-161">To update the working directory:</span></span>

* <span data-ttu-id="b9a25-162">Щелкните проект правой кнопкой мыши и выберите **Изменить файл проекта**.</span><span class="sxs-lookup"><span data-stu-id="b9a25-162">Right-click on the project and select **Edit Project File**</span></span>
* <span data-ttu-id="b9a25-163">Непосредственно под свойством *TargetFramework* добавьте следующее:</span><span class="sxs-lookup"><span data-stu-id="b9a25-163">Just below the *TargetFramework* property, add the following:</span></span>

  ``` XML
  <StartWorkingDirectory>$(MSBuildProjectDirectory)</StartWorkingDirectory>
  ```

* <span data-ttu-id="b9a25-164">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="b9a25-164">Save the file</span></span>

<span data-ttu-id="b9a25-165">Теперь можно запустить приложение:</span><span class="sxs-lookup"><span data-stu-id="b9a25-165">Now you can run the app:</span></span>

* <span data-ttu-id="b9a25-166">**"Отладка" > "Запустить без отладки"**</span><span class="sxs-lookup"><span data-stu-id="b9a25-166">**Debug > Start Without Debugging**</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="b9a25-167">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="b9a25-167">Next steps</span></span>

* <span data-ttu-id="b9a25-168">Следуйте инструкциям из [руководства по ASP.NET Core](/aspnet/core/data/ef-rp/intro), чтобы использовать EF Core в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="b9a25-168">Follow the [ASP.NET Core Tutorial](/aspnet/core/data/ef-rp/intro) to use EF Core in a web app</span></span>
* <span data-ttu-id="b9a25-169">См. сведения о [выражениях запросов LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations).</span><span class="sxs-lookup"><span data-stu-id="b9a25-169">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
* <span data-ttu-id="b9a25-170">[Настройте модель](xref:core/modeling/index), указав [требуемую](xref:core/modeling/entity-properties#required-and-optional-properties) и [максимальную длину](xref:core/modeling/entity-properties#maximum-length).</span><span class="sxs-lookup"><span data-stu-id="b9a25-170">[Configure your model](xref:core/modeling/index) to specify things like [required](xref:core/modeling/entity-properties#required-and-optional-properties) and [maximum length](xref:core/modeling/entity-properties#maximum-length)</span></span>
* <span data-ttu-id="b9a25-171">Используйте [миграции](xref:core/managing-schemas/migrations/index) для обновления схемы базы данных после изменения модели.</span><span class="sxs-lookup"><span data-stu-id="b9a25-171">Use [Migrations](xref:core/managing-schemas/migrations/index) to update the database schema after changing your model</span></span>
