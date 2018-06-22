---
title: Начало работы в ASP.NET Core — существующая база данных — Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: db2469d0badd428734425c1f568667f00bef2f4f
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2018
ms.locfileid: "30151018"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="6b3d9-102">Начало работы с EF Core в ASP.NET Core с существующей базой данных</span><span class="sxs-lookup"><span data-stu-id="6b3d9-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

<span data-ttu-id="6b3d9-103">В этом пошаговом руководстве вы создадите приложение ASP.NET Core MVC, которое выполняет базовые операции доступа к данным через платформу Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework.</span></span> <span data-ttu-id="6b3d9-104">Чтобы создать модель Entity Framework для существующей базы данных, мы применим метод реконструирования.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="6b3d9-105">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6b3d9-106">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="6b3d9-106">Prerequisites</span></span>

<span data-ttu-id="6b3d9-107">Для прохождения этого пошагового руководства нужны следующие элементы:</span><span class="sxs-lookup"><span data-stu-id="6b3d9-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="6b3d9-108">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) с такими рабочими нагрузками:</span><span class="sxs-lookup"><span data-stu-id="6b3d9-108">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="6b3d9-109">**ASP.NET и веб-разработка** в разделе **Интернет и облако**)</span><span class="sxs-lookup"><span data-stu-id="6b3d9-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="6b3d9-110">**Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)</span><span class="sxs-lookup"><span data-stu-id="6b3d9-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="6b3d9-111">[пакет SDK для .NET Core 2.0](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="6b3d9-111">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>
* <span data-ttu-id="6b3d9-112">[база данных для ведения блогов](#blogging-database).</span><span class="sxs-lookup"><span data-stu-id="6b3d9-112">[Blogging database](#blogging-database)</span></span>

### <a name="blogging-database"></a><span data-ttu-id="6b3d9-113">База данных для ведения блогов</span><span class="sxs-lookup"><span data-stu-id="6b3d9-113">Blogging database</span></span>

<span data-ttu-id="6b3d9-114">В этом руководстве в качестве существующей базы данных используется база данных **для ведения блогов**, размещенная на локальном экземпляре LocalDb.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-114">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="6b3d9-115">Если вы уже создали эту базу данных **для ведения блогов**, работая с другим руководством, можете пропустить эти шаги.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-115">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="6b3d9-116">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6b3d9-116">Open Visual Studio</span></span>
* <span data-ttu-id="6b3d9-117">Выберите **Инструменты -> Подключиться к базе данных...**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-117">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="6b3d9-118">Выберите **Microsoft SQL Server** и щелкните **Продолжить**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-118">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="6b3d9-119">Введите значение **(localdb)\mssqllocaldb** для параметра **Имя сервера**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-119">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="6b3d9-120">Введите значение **master** для параметра **Имя базы данных**, затем щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-120">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="6b3d9-121">Теперь база данных master появится в разделе **Подключения к данным** в **обозревателе сервера**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-121">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="6b3d9-122">Щелкните правой кнопкой мыши базу данных в **обозревателе сервера** и выберите действие **Создать запрос**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-122">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="6b3d9-123">Скопируйте представленный ниже скрипт и вставьте его в редактор запросов.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-123">Copy the script, listed below, into the query editor</span></span>
* <span data-ttu-id="6b3d9-124">Щелкните область редактора запросов правой кнопкой мыши и выберите действие **Выполнить**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-124">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="6b3d9-125">Создание нового проекта</span><span class="sxs-lookup"><span data-stu-id="6b3d9-125">Create a new project</span></span>

* <span data-ttu-id="6b3d9-126">Откройте Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-126">Open Visual Studio 2017</span></span>
* <span data-ttu-id="6b3d9-127">Последовательно выберите **Файл > Создать > Проект**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-127">**File -> New -> Project...**</span></span>
* <span data-ttu-id="6b3d9-128">В меню слева выберите **Установленные -> Шаблоны -> Visual C# -> Веб**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-128">From the left menu select **Installed -> Templates -> Visual C# -> Web**</span></span>
* <span data-ttu-id="6b3d9-129">Выберите шаблон проекта **Веб-приложение ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-129">Select the **ASP.NET Core Web Application (.NET Core)** project template</span></span>
* <span data-ttu-id="6b3d9-130">Введите имя проекта **EFGetStarted.AspNetCore.ExistingDb** и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="6b3d9-131">Дождитесь появления диалогового окна **Создание веб-приложения ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-131">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="6b3d9-132">В разделе **Шаблоны ASP.NET Core 2.0** выберите **Web Application (Model-View-Controller)** (Веб-приложение "модель — представление — контроллер").</span><span class="sxs-lookup"><span data-stu-id="6b3d9-132">Under **ASP.NET Core Templates 2.0** select the **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="6b3d9-133">Убедитесь, что для параметра **Проверка подлинности** задано значение **Без проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-133">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="6b3d9-134">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-134">Click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="6b3d9-135">Установка Entity Framework</span><span class="sxs-lookup"><span data-stu-id="6b3d9-135">Install Entity Framework</span></span>

<span data-ttu-id="6b3d9-136">Чтобы использовать EF Core, установите пакеты для поставщиков базы данных, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-136">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="6b3d9-137">В этом пошаговом руководстве используется SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-137">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="6b3d9-138">Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).</span><span class="sxs-lookup"><span data-stu-id="6b3d9-138">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="6b3d9-139">Последовательно выберите пункты **Средства > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="6b3d9-140">Запуск `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="6b3d9-140">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="6b3d9-141">Нам понадобятся некоторые средства платформы Entity Framework, чтобы создать модель на основе базы данных.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-141">We will be using some Entity Framework Tools to create a model from the database.</span></span> <span data-ttu-id="6b3d9-142">Поэтому мы установим пакет средств:</span><span class="sxs-lookup"><span data-stu-id="6b3d9-142">So we will install the tools package as well:</span></span>

* <span data-ttu-id="6b3d9-143">Запуск `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="6b3d9-143">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="6b3d9-144">Позднее мы применим средства формирования шаблонов для ASP.NET Core, чтобы создать контроллеры и представления.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-144">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="6b3d9-145">Поэтому мы установим этот пакет разработки:</span><span class="sxs-lookup"><span data-stu-id="6b3d9-145">So we will install this design package as well:</span></span>

* <span data-ttu-id="6b3d9-146">Запуск `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span><span class="sxs-lookup"><span data-stu-id="6b3d9-146">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="6b3d9-147">Реконструирование модели</span><span class="sxs-lookup"><span data-stu-id="6b3d9-147">Reverse engineer your model</span></span>

<span data-ttu-id="6b3d9-148">Теперь пора создать модель EF на основе существующей базы данных:</span><span class="sxs-lookup"><span data-stu-id="6b3d9-148">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="6b3d9-149">Последовательно выберите пункты **Средства -> Диспетчер пакетов NuGet -> Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-149">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="6b3d9-150">Выполните следующую команду, чтобы создать модель на основе существующей базы данных:</span><span class="sxs-lookup"><span data-stu-id="6b3d9-150">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="6b3d9-151">Если появляется сообщение об ошибке `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, закройте и снова откройте Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-151">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="6b3d9-152">Вы можете выбрать, для каких таблиц нужно создать сущности, указав в команде выше аргумент `-Tables`.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-152">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="6b3d9-153">Например,</span><span class="sxs-lookup"><span data-stu-id="6b3d9-153">E.g.</span></span> <span data-ttu-id="6b3d9-154">`-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-154">`-Tables Blog,Post`.</span></span>

<span data-ttu-id="6b3d9-155">Процесс реконструирования создает классы сущностей (`Blog.cs` & `Post.cs`) и производный контекст (`BloggingContext.cs`) на основе схемы существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-155">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="6b3d9-156">Классы сущностей — это простые объекты C#, которые представляют данные для использования в запросах и командах сохранения.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-156">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 <span data-ttu-id="6b3d9-157">Контекст соответствует сеансу взаимодействия с базой данных и позволяет запрашивать и сохранять экземпляры классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-157">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="6b3d9-158">Регистрация контекста с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="6b3d9-158">Register your context with dependency injection</span></span>

<span data-ttu-id="6b3d9-159">Внедрение зависимостей является ключевой концепцией для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-159">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="6b3d9-160">С помощью внедрения зависимостей службы (например, `BloggingContext`) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-160">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="6b3d9-161">Затем компоненты, которые используют эти службы (например, контроллеры MVC), обращаются к ним через параметры или свойства конструктора.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-161">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="6b3d9-162">Дополнительные сведения о внедрении зависимостей вы найдете в статье [Introduction to Dependency Injection in ASP.NET Core](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) (Знакомство с концепцией внедрения зависимостей в ASP.NET Core) на сайте документации по этой платформе.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-162">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="remove-inline-context-configuration"></a><span data-ttu-id="6b3d9-163">Удаление встроенного кода для настройки контекста</span><span class="sxs-lookup"><span data-stu-id="6b3d9-163">Remove inline context configuration</span></span>

<span data-ttu-id="6b3d9-164">В ASP.NET Core настройка обычно выполняется в **файле Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-164">In ASP.NET Core, configuration is generally performed in **Startup.cs**.</span></span> <span data-ttu-id="6b3d9-165">Мы тоже применим этот вариант, а для этого перенесем в **Startup.cs** конфигурацию поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-165">To conform to this pattern, we will move configuration of the database provider to **Startup.cs**.</span></span>

* <span data-ttu-id="6b3d9-166">Откройте файл `Models\BloggingContext.cs`.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-166">Open `Models\BloggingContext.cs`</span></span>
* <span data-ttu-id="6b3d9-167">Удалите метод `OnConfiguring(...)`.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-167">Delete the `OnConfiguring(...)` method</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* <span data-ttu-id="6b3d9-168">Добавьте следующий конструктор, который позволяет передавать в контекст конфигурацию с помощью внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-168">Add the following constructor, which will allow configuration to be passed into the context by dependency injection</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="6b3d9-169">Регистрация и настройка контекста в файле Startup.cs</span><span class="sxs-lookup"><span data-stu-id="6b3d9-169">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="6b3d9-170">Чтобы контроллеры MVC могли использовать службу `BloggingContext`, ее нужно зарегистрировать:</span><span class="sxs-lookup"><span data-stu-id="6b3d9-170">In order for our MVC controllers to make use of `BloggingContext` we are going to register it as a service.</span></span>

* <span data-ttu-id="6b3d9-171">Откройте файл **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-171">Open **Startup.cs**</span></span>
* <span data-ttu-id="6b3d9-172">Добавьте в начало этого файла следующие инструкции `using`.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-172">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="6b3d9-173">Теперь мы можем использовать метод `AddDbContext(...)`, чтобы зарегистрировать нашу службу:</span><span class="sxs-lookup"><span data-stu-id="6b3d9-173">Now we can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="6b3d9-174">Найдите в коде метод `ConfigureServices(...)`.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-174">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="6b3d9-175">Добавьте в него следующий фрагмент, который регистрирует контекст в качестве службы.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-175">Add the following code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> <span data-ttu-id="6b3d9-176">В реальном приложении строка подключения обычно помещается в файл конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-176">In a real application you would typically put the connection string in a configuration file.</span></span> <span data-ttu-id="6b3d9-177">Здесь мы для простоты определяем ее прямо в коде.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-177">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="6b3d9-178">Дополнительные сведения см. в статье [Connection Strings](../../miscellaneous/connection-strings.md) (Строки подключения).</span><span class="sxs-lookup"><span data-stu-id="6b3d9-178">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="6b3d9-179">Создание контроллера</span><span class="sxs-lookup"><span data-stu-id="6b3d9-179">Create a controller</span></span>

<span data-ttu-id="6b3d9-180">Теперь мы добавим в проект возможность формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="6b3d9-180">Next, we'll enable scaffolding in our project.</span></span>

* <span data-ttu-id="6b3d9-181">В **обозревателе решений** щелкните папку **Контроллеры** правой кнопкой мыши и выберите пункт **Добавить -> Контроллер**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-181">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="6b3d9-182">Выберите **Полные зависимости** и щелкните **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-182">Select **Full Dependencies** and click **Add**</span></span>
* <span data-ttu-id="6b3d9-183">Откроется файл `ScaffoldingReadMe.txt` с инструкциями, которые вы можете проигнорировать.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-183">You can ignore the instructions in the `ScaffoldingReadMe.txt` file that opens</span></span>

<span data-ttu-id="6b3d9-184">Теперь формирование шаблонов включено, и мы можем сформировать контроллер для сущности `Blog`:</span><span class="sxs-lookup"><span data-stu-id="6b3d9-184">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="6b3d9-185">В **обозревателе решений** щелкните папку **Контроллеры** правой кнопкой мыши и выберите пункт **Добавить -> Контроллер**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-185">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="6b3d9-186">Выберите **Контроллер MVC с представлениями, использующий Entity Framework** и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-186">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="6b3d9-187">Для параметра **Класс модели** установите значение **Блог**, а для параметра **Класс контекста данных** — **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-187">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="6b3d9-188">Щелкните **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-188">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="6b3d9-189">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="6b3d9-189">Run the application</span></span>

<span data-ttu-id="6b3d9-190">Теперь вы можете запустить приложение и увидеть, как оно работает:</span><span class="sxs-lookup"><span data-stu-id="6b3d9-190">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="6b3d9-191">Выберите **Отладка -> Запустить без отладки**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-191">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="6b3d9-192">Это действие выполняет сборку приложения и открывает его в веб-браузере.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-192">The application will build and open in a web browser</span></span>
* <span data-ttu-id="6b3d9-193">Перейдите к папке `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-193">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="6b3d9-194">Щелкните **Создать**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-194">Click **Create New**</span></span>
* <span data-ttu-id="6b3d9-195">Введите **URL-адрес** для нового блога и щелкните **Создать**.</span><span class="sxs-lookup"><span data-stu-id="6b3d9-195">Enter a **Url** for the new blog and click **Create**</span></span>

![изображение](_static/create.png)

![изображение](_static/index-existing-db.png)
