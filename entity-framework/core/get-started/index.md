---
title: Начало работы — EF Core
author: rick-anderson
ms.date: 09/17/2019
ms.assetid: 3c88427c-20c6-42ec-a736-22d3eccd5071
uid: core/get-started/index
ms.openlocfilehash: 8aea40b9caaf454bc06d4991fd403e1ad0ff9b93
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502023"
---
# <a name="getting-started-with-ef-core"></a><span data-ttu-id="0a754-102">Начало работы с EF Core</span><span class="sxs-lookup"><span data-stu-id="0a754-102">Getting Started with EF Core</span></span>

<span data-ttu-id="0a754-103">В этом руководстве вы создадите консольное приложение .NET Core, которое осуществляет доступ к базе данных SQLite с помощью Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0a754-103">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="0a754-104">Инструкции из этого руководства можно выполнять с помощью Visual Studio 2017 в Windows, а также .NET Core CLI в Windows, macOS или Linux.</span><span class="sxs-lookup"><span data-stu-id="0a754-104">You can follow the tutorial by using Visual Studio on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="0a754-105">[Пример для этой статьи на GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span><span class="sxs-lookup"><span data-stu-id="0a754-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0a754-106">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="0a754-106">Prerequisites</span></span>

<span data-ttu-id="0a754-107">Установите следующее программное обеспечение:</span><span class="sxs-lookup"><span data-stu-id="0a754-107">Install the following software:</span></span>

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0a754-108">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="0a754-108">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="0a754-109">[пакет SDK для .NET Core 3.0](https://www.microsoft.com/net/download/core);</span><span class="sxs-lookup"><span data-stu-id="0a754-109">[.NET Core 3.0 SDK](https://www.microsoft.com/net/download/core).</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a754-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a754-110">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0a754-111">[Visual Studio 2019 16.3 или последующей версии](https://www.visualstudio.com/downloads/) с этой рабочей нагрузкой:</span><span class="sxs-lookup"><span data-stu-id="0a754-111">[Visual Studio 2019 version 16.3 or later](https://www.visualstudio.com/downloads/) with this  workload:</span></span>
  * <span data-ttu-id="0a754-112">**Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)</span><span class="sxs-lookup"><span data-stu-id="0a754-112">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="0a754-113">Создание нового проекта</span><span class="sxs-lookup"><span data-stu-id="0a754-113">Create a new project</span></span>

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0a754-114">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="0a754-114">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new console -o EFGetStarted
cd EFGetStarted
```

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a754-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a754-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0a754-116">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a754-116">Open Visual Studio</span></span>
* <span data-ttu-id="0a754-117">Щелкните **Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="0a754-117">Click **Create a new project**</span></span>
* <span data-ttu-id="0a754-118">Выберите **Консольное приложение (.NET Core )** с тегом **C#** и щелкните **Далее**.</span><span class="sxs-lookup"><span data-stu-id="0a754-118">Select **Console App (.NET Core)** with the **C#** tag and click **Next**</span></span>
* <span data-ttu-id="0a754-119">Задайте проекту имя **EFGetStarted** и щелкните **Создать**.</span><span class="sxs-lookup"><span data-stu-id="0a754-119">Enter **EFGetStarted** for the name and click **Create**</span></span>

---

## <a name="install-entity-framework-core"></a><span data-ttu-id="0a754-120">Установка Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="0a754-120">Install Entity Framework Core</span></span>

<span data-ttu-id="0a754-121">Чтобы установить EF Core, установите пакеты целевых поставщиков базы данных EF Core, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="0a754-121">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="0a754-122">В этом руководстве используется SQLite, так как он работает на всех платформах, поддерживаемых .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0a754-122">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span> <span data-ttu-id="0a754-123">Список доступных поставщиков см. в разделе [Database Providers](../providers/index.md) (Поставщики базы данных).</span><span class="sxs-lookup"><span data-stu-id="0a754-123">For a list of available providers, see [Database Providers](../providers/index.md).</span></span>

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0a754-124">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="0a754-124">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a754-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a754-125">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0a754-126">Последовательно выберите пункты **Средства > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="0a754-126">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="0a754-127">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="0a754-127">Run the following commands:</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Sqlite
  ```

<span data-ttu-id="0a754-128">Совет. Можно также установить пакеты, щелкнув проект правой кнопкой мыши и выбрав **Управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="0a754-128">Tip: You can also install packages by right-clicking on the project and selecting **Manage NuGet Packages**</span></span>

---

## <a name="create-the-model"></a><span data-ttu-id="0a754-129">Создание модели</span><span class="sxs-lookup"><span data-stu-id="0a754-129">Create the model</span></span>

<span data-ttu-id="0a754-130">Задайте класс контекста и классы сущностей, составляющие модель.</span><span class="sxs-lookup"><span data-stu-id="0a754-130">Define a context class and entity classes that make up the model.</span></span>

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0a754-131">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="0a754-131">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="0a754-132">В каталоге проекта создайте файл **Model.cs** с таким кодом:</span><span class="sxs-lookup"><span data-stu-id="0a754-132">In the project directory, create **Model.cs** with the following code</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a754-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a754-133">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0a754-134">Щелкните проект правой кнопкой мыши и выберите **Добавить > Класс**.</span><span class="sxs-lookup"><span data-stu-id="0a754-134">Right-click on the project and select **Add > Class**</span></span>
* <span data-ttu-id="0a754-135">Задайте имя **Model.cs** и щелкните **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="0a754-135">Enter **Model.cs** as the name and click **Add**</span></span>
* <span data-ttu-id="0a754-136">Замените все содержимое этого файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="0a754-136">Replace the contents of the file with the following code</span></span>

---

[!code-csharp[Main](../../../samples/core/GetStarted/Model.cs)]

<span data-ttu-id="0a754-137">В EF Core также можно [реконструировать](../managing-schemas/scaffolding.md) модель из существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="0a754-137">EF Core can also [reverse engineer](../managing-schemas/scaffolding.md) a model from an existing database.</span></span>

<span data-ttu-id="0a754-138">Совет. В реальном приложении каждый класс помещается в отдельный файл, а [строка подключения](../miscellaneous/connection-strings.md) — в файл конфигурации или переменную среды.</span><span class="sxs-lookup"><span data-stu-id="0a754-138">Tip: In a real app, you put each class in a separate file and put the [connection string](../miscellaneous/connection-strings.md) in a configuration file or environment variable.</span></span> <span data-ttu-id="0a754-139">Для упрощения в этом руководстве все данные содержатся в одном файле.</span><span class="sxs-lookup"><span data-stu-id="0a754-139">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="0a754-140">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="0a754-140">Create the database</span></span>

<span data-ttu-id="0a754-141">В следующих действиях используются [миграции](xref:core/managing-schemas/migrations/index) для создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="0a754-141">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0a754-142">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="0a754-142">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="0a754-143">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="0a754-143">Run the following commands:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="0a754-144">При этом устанавливается [dotnet ef](../miscellaneous/cli/dotnet.md) и пакет конструктора, требуемый для выполнения команды в проекте.</span><span class="sxs-lookup"><span data-stu-id="0a754-144">This installs [dotnet ef](../miscellaneous/cli/dotnet.md) and the design package which is required to run the command on a project.</span></span> <span data-ttu-id="0a754-145">Команда `migrations` формирует шаблон миграции для создания начального набора таблиц в модели.</span><span class="sxs-lookup"><span data-stu-id="0a754-145">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="0a754-146">Команда `database update` создает базу данных и применяет к ней созданную миграцию.</span><span class="sxs-lookup"><span data-stu-id="0a754-146">The `database update` command creates the database and applies the new migration to it.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a754-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a754-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0a754-148">В **консоли диспетчера пакетов** выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="0a754-148">Run the following commands in **Package Manager Console**</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Tools
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="0a754-149">При этом устанавливаются [средства консоли диспетчера пакетов для EF Core](../miscellaneous/cli/powershell.md).</span><span class="sxs-lookup"><span data-stu-id="0a754-149">This installs the [PMC tools for EF Core](../miscellaneous/cli/powershell.md).</span></span> <span data-ttu-id="0a754-150">Команда `Add-Migration` формирует шаблон миграции для создания начального набора таблиц в модели.</span><span class="sxs-lookup"><span data-stu-id="0a754-150">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="0a754-151">Команда `Update-Database` создает базу данных и применяет к ней созданную миграцию.</span><span class="sxs-lookup"><span data-stu-id="0a754-151">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-read-update--delete"></a><span data-ttu-id="0a754-152">Создание, чтение, обновление и удаление</span><span class="sxs-lookup"><span data-stu-id="0a754-152">Create, read, update & delete</span></span>

* <span data-ttu-id="0a754-153">Откройте файл *Program.cs* и замените его содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="0a754-153">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../samples/core/GetStarted/Program.cs)]

## <a name="run-the-app"></a><span data-ttu-id="0a754-154">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="0a754-154">Run the app</span></span>

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0a754-155">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="0a754-155">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet run
```

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0a754-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a754-156">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0a754-157">В Visual Studio используется неподходящий рабочий каталог при запуске консольных приложений .NET Core</span><span class="sxs-lookup"><span data-stu-id="0a754-157">Visual Studio uses an inconsistent working directory when running .NET Core console apps.</span></span> <span data-ttu-id="0a754-158">(см. [dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619)). Это вызывает исключение, информирующее об *отсутствии таблицы (блоги)* .</span><span class="sxs-lookup"><span data-stu-id="0a754-158">(see [dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619)) This results in an exception being thrown: *no such table: Blogs*.</span></span> <span data-ttu-id="0a754-159">Чтобы обновить рабочий каталог, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="0a754-159">To update the working directory:</span></span>

* <span data-ttu-id="0a754-160">Щелкните проект правой кнопкой мыши и выберите **Изменить файл проекта**.</span><span class="sxs-lookup"><span data-stu-id="0a754-160">Right-click on the project and select **Edit Project File**</span></span>
* <span data-ttu-id="0a754-161">Непосредственно под свойством *TargetFramework* добавьте следующее:</span><span class="sxs-lookup"><span data-stu-id="0a754-161">Just below the *TargetFramework* property, add the following:</span></span>

  ``` XML
  <StartWorkingDirectory>$(MSBuildProjectDirectory)</StartWorkingDirectory>
  ```

* <span data-ttu-id="0a754-162">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="0a754-162">Save the file</span></span>

<span data-ttu-id="0a754-163">Теперь можно запустить приложение:</span><span class="sxs-lookup"><span data-stu-id="0a754-163">Now you can run the app:</span></span>

* <span data-ttu-id="0a754-164">**"Отладка" > "Запустить без отладки"**</span><span class="sxs-lookup"><span data-stu-id="0a754-164">**Debug > Start Without Debugging**</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="0a754-165">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="0a754-165">Next steps</span></span>

* <span data-ttu-id="0a754-166">Следуйте инструкциям из [руководства по ASP.NET Core](/aspnet/core/data/ef-rp/intro), чтобы использовать EF Core в веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="0a754-166">Follow the [ASP.NET Core Tutorial](/aspnet/core/data/ef-rp/intro) to use EF Core in a web app</span></span>
* <span data-ttu-id="0a754-167">См. сведения о [выражениях запросов LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations).</span><span class="sxs-lookup"><span data-stu-id="0a754-167">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
* <span data-ttu-id="0a754-168">[Настройте модель](xref:core/modeling/index), указав [требуемую](xref:core/modeling/entity-properties#required-and-optional-properties) и [максимальную длину](xref:core/modeling/entity-properties#maximum-length).</span><span class="sxs-lookup"><span data-stu-id="0a754-168">[Configure your model](xref:core/modeling/index) to specify things like [required](xref:core/modeling/entity-properties#required-and-optional-properties) and [maximum length](xref:core/modeling/entity-properties#maximum-length)</span></span>
* <span data-ttu-id="0a754-169">Используйте [миграции](xref:core/managing-schemas/migrations/index) для обновления схемы базы данных после изменения модели.</span><span class="sxs-lookup"><span data-stu-id="0a754-169">Use [Migrations](xref:core/managing-schemas/migrations/index) to update the database schema after changing your model</span></span>
