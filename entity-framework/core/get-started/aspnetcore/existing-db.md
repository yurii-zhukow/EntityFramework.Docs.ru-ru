---
title: "Начало работы в ASP.NET Core — существующая база данных — Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: afd99d68d2ba25ce58a21dc48d2c7ce27f208807
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="40092-102">Начало работы с EF Core в ASP.NET Core с существующей базой данных</span><span class="sxs-lookup"><span data-stu-id="40092-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="40092-103">[Пакет SDK для .NET Core](https://www.microsoft.com/net/download/core) больше не поддерживает `project.json` и Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="40092-103">The [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or Visual Studio 2015.</span></span> <span data-ttu-id="40092-104">Всем, кто работает с .NET Core, мы рекомендуем выполнить [миграцию из project.json на csproj](https://docs.microsoft.com/dotnet/articles/core/migration/) и [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="40092-104">Everyone doing .NET Core development is encouraged to [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/) and [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

<span data-ttu-id="40092-105">В этом пошаговом руководстве вы создадите приложение ASP.NET Core MVC, которое выполняет базовые операции доступа к данным через платформу Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="40092-105">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework.</span></span> <span data-ttu-id="40092-106">Чтобы создать модель Entity Framework для существующей базы данных, мы применим метод реконструирования.</span><span class="sxs-lookup"><span data-stu-id="40092-106">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="40092-107">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="40092-107">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="40092-108">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="40092-108">Prerequisites</span></span>

<span data-ttu-id="40092-109">Для прохождения этого пошагового руководства нужны следующие элементы:</span><span class="sxs-lookup"><span data-stu-id="40092-109">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="40092-110">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) с такими рабочими нагрузками:</span><span class="sxs-lookup"><span data-stu-id="40092-110">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="40092-111">**ASP.NET и веб-разработка** в разделе **Интернет и облако**;</span><span class="sxs-lookup"><span data-stu-id="40092-111">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="40092-112">**кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**).</span><span class="sxs-lookup"><span data-stu-id="40092-112">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="40092-113">[пакет SDK для .NET Core 2.0](https://www.microsoft.com/net/download/core);</span><span class="sxs-lookup"><span data-stu-id="40092-113">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>
* <span data-ttu-id="40092-114">[база данных для ведения блогов](#blogging-database).</span><span class="sxs-lookup"><span data-stu-id="40092-114">[Blogging database](#blogging-database)</span></span>

### <a name="blogging-database"></a><span data-ttu-id="40092-115">База данных для ведения блогов</span><span class="sxs-lookup"><span data-stu-id="40092-115">Blogging database</span></span>

<span data-ttu-id="40092-116">В этом руководстве в качестве существующей базы данных используется база данных **для ведения блогов**, размещенная на локальном экземпляре LocalDb.</span><span class="sxs-lookup"><span data-stu-id="40092-116">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="40092-117">Если вы уже создали эту базу данных **для ведения блогов**, работая с другим руководством, можете пропустить эти шаги.</span><span class="sxs-lookup"><span data-stu-id="40092-117">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="40092-118">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="40092-118">Open Visual Studio</span></span>
* <span data-ttu-id="40092-119">Выберите **Инструменты -> Подключиться к базе данных...**.</span><span class="sxs-lookup"><span data-stu-id="40092-119">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="40092-120">Выберите **Microsoft SQL Server** и щелкните **Продолжить**.</span><span class="sxs-lookup"><span data-stu-id="40092-120">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="40092-121">Введите значение **(localdb)\mssqllocaldb** для параметра **Имя сервера**.</span><span class="sxs-lookup"><span data-stu-id="40092-121">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="40092-122">Введите значение **master** для параметра **Имя базы данных**, затем щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="40092-122">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="40092-123">Теперь база данных master появится в разделе **Подключения к данным** в **обозревателе сервера**.</span><span class="sxs-lookup"><span data-stu-id="40092-123">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="40092-124">Щелкните правой кнопкой мыши базу данных в **обозревателе сервера** и выберите действие **Создать запрос**.</span><span class="sxs-lookup"><span data-stu-id="40092-124">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="40092-125">Скопируйте представленный ниже скрипт и вставьте его в редактор запросов.</span><span class="sxs-lookup"><span data-stu-id="40092-125">Copy the script, listed below, into the query editor</span></span>
* <span data-ttu-id="40092-126">Щелкните область редактора запросов правой кнопкой мыши и выберите действие **Выполнить**.</span><span class="sxs-lookup"><span data-stu-id="40092-126">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="40092-127">Создание нового проекта</span><span class="sxs-lookup"><span data-stu-id="40092-127">Create a new project</span></span>

* <span data-ttu-id="40092-128">Откройте Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="40092-128">Open Visual Studio 2017</span></span>
* <span data-ttu-id="40092-129">Последовательно выберите **Файл > Создать > Проект**.</span><span class="sxs-lookup"><span data-stu-id="40092-129">**File -> New -> Project...**</span></span>
* <span data-ttu-id="40092-130">В меню слева выберите **Установленные -> Шаблоны -> Visual C# -> Веб**.</span><span class="sxs-lookup"><span data-stu-id="40092-130">From the left menu select **Installed -> Templates -> Visual C# -> Web**</span></span>
* <span data-ttu-id="40092-131">Выберите шаблон проекта **Веб-приложение ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="40092-131">Select the **ASP.NET Core Web Application (.NET Core)** project template</span></span>
* <span data-ttu-id="40092-132">Введите имя проекта **EFGetStarted.AspNetCore.ExistingDb** и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="40092-132">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="40092-133">Дождитесь появления диалогового окна **Создание веб-приложения ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="40092-133">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="40092-134">В разделе **Шаблоны ASP.NET Core 2.0** выберите **Web Application (Model-View-Controller)** (Веб-приложение "модель — представление — контроллер").</span><span class="sxs-lookup"><span data-stu-id="40092-134">Under **ASP.NET Core Templates 2.0** select the **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="40092-135">Убедитесь, что для параметра **Проверка подлинности** задано значение **Без проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="40092-135">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="40092-136">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="40092-136">Click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="40092-137">Установка Entity Framework</span><span class="sxs-lookup"><span data-stu-id="40092-137">Install Entity Framework</span></span>

<span data-ttu-id="40092-138">Чтобы использовать EF Core, установите пакеты для поставщиков базы данных, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="40092-138">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="40092-139">В этом пошаговом руководстве используется SQL Server.</span><span class="sxs-lookup"><span data-stu-id="40092-139">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="40092-140">Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).</span><span class="sxs-lookup"><span data-stu-id="40092-140">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="40092-141">Последовательно выберите пункты **Средства > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="40092-141">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="40092-142">Запуск `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="40092-142">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="40092-143">Нам понадобятся некоторые средства платформы Entity Framework, чтобы создать модель на основе базы данных.</span><span class="sxs-lookup"><span data-stu-id="40092-143">We will be using some Entity Framework Tools to create a model from the database.</span></span> <span data-ttu-id="40092-144">Поэтому мы установим пакет средств:</span><span class="sxs-lookup"><span data-stu-id="40092-144">So we will install the tools package as well:</span></span>

* <span data-ttu-id="40092-145">Запуск `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="40092-145">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="40092-146">Позднее мы применим средства формирования шаблонов для ASP.NET Core, чтобы создать контроллеры и представления.</span><span class="sxs-lookup"><span data-stu-id="40092-146">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="40092-147">Поэтому мы установим этот пакет разработки:</span><span class="sxs-lookup"><span data-stu-id="40092-147">So we will install this design package as well:</span></span>

* <span data-ttu-id="40092-148">Запуск `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span><span class="sxs-lookup"><span data-stu-id="40092-148">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="40092-149">Реконструирование модели</span><span class="sxs-lookup"><span data-stu-id="40092-149">Reverse engineer your model</span></span>

<span data-ttu-id="40092-150">Теперь пора создать модель EF на основе существующей базы данных:</span><span class="sxs-lookup"><span data-stu-id="40092-150">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="40092-151">Последовательно выберите пункты **Средства -> Диспетчер пакетов NuGet -> Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="40092-151">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="40092-152">Выполните следующую команду, чтобы создать модель на основе существующей базы данных:</span><span class="sxs-lookup"><span data-stu-id="40092-152">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="40092-153">Если появляется сообщение об ошибке `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, закройте и снова откройте Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="40092-153">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="40092-154">Вы можете выбрать, для каких таблиц нужно создать сущности, указав в команде выше аргумент `-Tables`.</span><span class="sxs-lookup"><span data-stu-id="40092-154">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="40092-155">Пример:</span><span class="sxs-lookup"><span data-stu-id="40092-155">E.g.</span></span> <span data-ttu-id="40092-156">`-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="40092-156">`-Tables Blog,Post`.</span></span>

<span data-ttu-id="40092-157">Процесс реконструирования создает классы сущностей (`Blog.cs` & `Post.cs`) и производный контекст (`BloggingContext.cs`) на основе схемы существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="40092-157">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="40092-158">Классы сущностей — это простые объекты C#, которые представляют данные для использования в запросах и командах сохранения.</span><span class="sxs-lookup"><span data-stu-id="40092-158">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 <span data-ttu-id="40092-159">Контекст соответствует сеансу взаимодействия с базой данных и позволяет запрашивать и сохранять экземпляры классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="40092-159">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- Static code listing, rather than a linked file, because the walkthrough modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>(entity =>
        {
            entity.Property(e => e.Url).IsRequired();
        });

        modelBuilder.Entity<Post>(entity =>
        {
            entity.HasOne(d => d.Blog)
                .WithMany(p => p.Post)
                .HasForeignKey(d => d.BlogId);
        });
    }
}
```

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="40092-160">Регистрация контекста с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="40092-160">Register your context with dependency injection</span></span>

<span data-ttu-id="40092-161">Внедрение зависимостей является ключевой концепцией для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="40092-161">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="40092-162">С помощью внедрения зависимостей службы (например, `BloggingContext`) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="40092-162">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="40092-163">Затем компоненты, которые используют эти службы (например, контроллеры MVC), обращаются к ним через параметры или свойства конструктора.</span><span class="sxs-lookup"><span data-stu-id="40092-163">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="40092-164">Дополнительные сведения о внедрении зависимостей вы найдете в статье [Introduction to Dependency Injection in ASP.NET Core](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) (Знакомство с концепцией внедрения зависимостей в ASP.NET Core) на сайте документации по этой платформе.</span><span class="sxs-lookup"><span data-stu-id="40092-164">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="remove-inline-context-configuration"></a><span data-ttu-id="40092-165">Удаление встроенного кода для настройки контекста</span><span class="sxs-lookup"><span data-stu-id="40092-165">Remove inline context configuration</span></span>

<span data-ttu-id="40092-166">В ASP.NET Core настройка обычно выполняется в **файле Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="40092-166">In ASP.NET Core, configuration is generally performed in **Startup.cs**.</span></span> <span data-ttu-id="40092-167">Мы тоже применим этот вариант, а для этого перенесем в **Startup.cs** конфигурацию поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="40092-167">To conform to this pattern, we will move configuration of the database provider to **Startup.cs**.</span></span>

* <span data-ttu-id="40092-168">Откройте `Models\BloggingContext.cs`.</span><span class="sxs-lookup"><span data-stu-id="40092-168">Open `Models\BloggingContext.cs`</span></span>
* <span data-ttu-id="40092-169">Удалите метод `OnConfiguring(...)`.</span><span class="sxs-lookup"><span data-stu-id="40092-169">Delete the `OnConfiguring(...)` method</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* <span data-ttu-id="40092-170">Добавьте следующий конструктор, который позволяет передавать в контекст конфигурацию с помощью внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="40092-170">Add the following constructor, which will allow configuration to be passed into the context by dependency injection</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="40092-171">Регистрация и настройка контекста в файле Startup.cs</span><span class="sxs-lookup"><span data-stu-id="40092-171">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="40092-172">Чтобы контроллеры MVC могли использовать службу `BloggingContext`, ее нужно зарегистрировать:</span><span class="sxs-lookup"><span data-stu-id="40092-172">In order for our MVC controllers to make use of `BloggingContext` we are going to register it as a service.</span></span>

* <span data-ttu-id="40092-173">Откройте файл **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="40092-173">Open **Startup.cs**</span></span>
* <span data-ttu-id="40092-174">Добавьте в начало этого файла следующие инструкции `using`.</span><span class="sxs-lookup"><span data-stu-id="40092-174">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="40092-175">Теперь мы можем использовать метод `AddDbContext(...)`, чтобы зарегистрировать нашу службу:</span><span class="sxs-lookup"><span data-stu-id="40092-175">Now we can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="40092-176">Найдите в коде метод `ConfigureServices(...)`.</span><span class="sxs-lookup"><span data-stu-id="40092-176">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="40092-177">Добавьте в него следующий фрагмент, который регистрирует контекст в качестве службы.</span><span class="sxs-lookup"><span data-stu-id="40092-177">Add the following code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> <span data-ttu-id="40092-178">В реальном приложении строка подключения обычно помещается в файл конфигурации.</span><span class="sxs-lookup"><span data-stu-id="40092-178">In a real application you would typically put the connection string in a configuration file.</span></span> <span data-ttu-id="40092-179">Здесь мы для простоты определяем ее прямо в коде.</span><span class="sxs-lookup"><span data-stu-id="40092-179">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="40092-180">Дополнительные сведения см. в статье [Connection Strings](../../miscellaneous/connection-strings.md) (Строки подключения).</span><span class="sxs-lookup"><span data-stu-id="40092-180">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="40092-181">Создание контроллера</span><span class="sxs-lookup"><span data-stu-id="40092-181">Create a controller</span></span>

<span data-ttu-id="40092-182">Теперь мы добавим в проект возможность формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="40092-182">Next, we'll enable scaffolding in our project.</span></span>

* <span data-ttu-id="40092-183">В **обозревателе решений** щелкните папку **Контроллеры** правой кнопкой мыши и выберите пункт **Добавить -> Контроллер**.</span><span class="sxs-lookup"><span data-stu-id="40092-183">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="40092-184">Выберите **Полные зависимости** и щелкните **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="40092-184">Select **Full Dependencies** and click **Add**</span></span>
* <span data-ttu-id="40092-185">Откроется файл `ScaffoldingReadMe.txt` с инструкциями, которые вы можете проигнорировать.</span><span class="sxs-lookup"><span data-stu-id="40092-185">You can ignore the instructions in the `ScaffoldingReadMe.txt` file that opens</span></span>

<span data-ttu-id="40092-186">Теперь формирование шаблонов включено, и мы можем сформировать контроллер для сущности `Blog`:</span><span class="sxs-lookup"><span data-stu-id="40092-186">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="40092-187">В **обозревателе решений** щелкните папку **Контроллеры** правой кнопкой мыши и выберите пункт **Добавить -> Контроллер**.</span><span class="sxs-lookup"><span data-stu-id="40092-187">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="40092-188">Выберите **Контроллер MVC с представлениями, использующий Entity Framework** и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="40092-188">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="40092-189">Для параметра **Класс модели** установите значение **Блог**, а для параметра **Класс контекста данных** — **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="40092-189">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="40092-190">Щелкните **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="40092-190">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="40092-191">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="40092-191">Run the application</span></span>

<span data-ttu-id="40092-192">Теперь вы можете запустить приложение и увидеть, как оно работает:</span><span class="sxs-lookup"><span data-stu-id="40092-192">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="40092-193">Выберите **Отладка -> Запустить без отладки**.</span><span class="sxs-lookup"><span data-stu-id="40092-193">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="40092-194">Это действие выполняет сборку приложения и открывает его в веб-браузере.</span><span class="sxs-lookup"><span data-stu-id="40092-194">The application will build and open in a web browser</span></span>
* <span data-ttu-id="40092-195">Перейдите к папке `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="40092-195">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="40092-196">Щелкните **Создать**.</span><span class="sxs-lookup"><span data-stu-id="40092-196">Click **Create New**</span></span>
* <span data-ttu-id="40092-197">Введите **URL-адрес** для нового блога и щелкните **Создать**.</span><span class="sxs-lookup"><span data-stu-id="40092-197">Enter a **Url** for the new blog and click **Create**</span></span>

![image](_static/create.png)

![image](_static/index-existing-db.png)
