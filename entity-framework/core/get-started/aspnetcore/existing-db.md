---
title: Начало работы в ASP.NET Core — существующая база данных — Core EF
author: rowanmiller
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: bba2742c3f3b6da93dd4b4f170a3878fc0473bc8
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022201"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="cd4f8-102">Начало работы с EF Core в ASP.NET Core с существующей базой данных</span><span class="sxs-lookup"><span data-stu-id="cd4f8-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

<span data-ttu-id="cd4f8-103">В этом руководстве вы создадите приложение ASP.NET Core MVC, которое выполняет базовые операции доступа к данным через платформу Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="cd4f8-104">Для создания модели Entity Framework используется реконструирование существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-104">You reverse engineer an existing database to create an Entity Framework model.</span></span>

<span data-ttu-id="cd4f8-105">[Пример для этой статьи на GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="cd4f8-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cd4f8-106">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="cd4f8-106">Prerequisites</span></span>

<span data-ttu-id="cd4f8-107">Установите следующее программное обеспечение:</span><span class="sxs-lookup"><span data-stu-id="cd4f8-107">Install the following software:</span></span>

* <span data-ttu-id="cd4f8-108">[Visual Studio 2017 версии 15.7](https://www.visualstudio.com/downloads/) со следующими рабочими нагрузками:</span><span class="sxs-lookup"><span data-stu-id="cd4f8-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="cd4f8-109">**ASP.NET и веб-разработка** в разделе **Интернет и облако**)</span><span class="sxs-lookup"><span data-stu-id="cd4f8-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="cd4f8-110">**Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)</span><span class="sxs-lookup"><span data-stu-id="cd4f8-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="cd4f8-111">[Пакет SDK для NET Core 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="cd4f8-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-blogging-database"></a><span data-ttu-id="cd4f8-112">Создание базы данных Blogging</span><span class="sxs-lookup"><span data-stu-id="cd4f8-112">Create Blogging database</span></span>

<span data-ttu-id="cd4f8-113">В этом руководстве в качестве существующей базы данных используется база данных **для ведения блогов**, размещенная на локальном экземпляре LocalDb.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-113">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span> <span data-ttu-id="cd4f8-114">Если вы уже создали базу данных **Blogging**, работая с другим руководством, пропустите эти шаги.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-114">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="cd4f8-115">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cd4f8-115">Open Visual Studio</span></span>
* <span data-ttu-id="cd4f8-116">Выберите **Инструменты -> Подключиться к базе данных...**.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-116">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="cd4f8-117">Выберите **Microsoft SQL Server** и щелкните **Продолжить**.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-117">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="cd4f8-118">Введите значение **(localdb)\mssqllocaldb** для параметра **Имя сервера**.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-118">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="cd4f8-119">Введите значение **master** для параметра **Имя базы данных**, затем щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-119">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="cd4f8-120">Теперь база данных master появится в разделе **Подключения к данным** в **обозревателе сервера**.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-120">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="cd4f8-121">Щелкните правой кнопкой мыши базу данных в **обозревателе сервера** и выберите действие **Создать запрос**.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-121">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="cd4f8-122">Скопируйте представленный ниже скрипт и вставьте его в редактор запросов</span><span class="sxs-lookup"><span data-stu-id="cd4f8-122">Copy the script listed below into the query editor</span></span>
* <span data-ttu-id="cd4f8-123">Щелкните область редактора запросов правой кнопкой мыши и выберите действие **Выполнить**.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-123">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="cd4f8-124">Создание нового проекта</span><span class="sxs-lookup"><span data-stu-id="cd4f8-124">Create a new project</span></span>

* <span data-ttu-id="cd4f8-125">Откройте Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-125">Open Visual Studio 2017</span></span>
* <span data-ttu-id="cd4f8-126">**"Файл" > "Создать" > "Проект"…**</span><span class="sxs-lookup"><span data-stu-id="cd4f8-126">**File > New > Project...**</span></span>
* <span data-ttu-id="cd4f8-127">В меню слева выберите **"Установленные" > Visual C# > Интернет**</span><span class="sxs-lookup"><span data-stu-id="cd4f8-127">From the left menu select **Installed > Visual C# > Web**</span></span>
* <span data-ttu-id="cd4f8-128">Выберите шаблон проекта **Веб-приложение ASP.NET Core**</span><span class="sxs-lookup"><span data-stu-id="cd4f8-128">Select the **ASP.NET Core Web Application** project template</span></span>
* <span data-ttu-id="cd4f8-129">Введите имя проекта **EFGetStarted.AspNetCore.ExistingDb** и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-129">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="cd4f8-130">Дождитесь появления диалогового окна **Создание веб-приложения ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-130">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="cd4f8-131">В соответствующих раскрывающихся списках должна быть выбрана целевая платформа **.NET Core** и версия **ASP.NET Core 2.1**</span><span class="sxs-lookup"><span data-stu-id="cd4f8-131">Make sure that the target framework dropdown is set to **.NET Core**, and the version dropdown is set to **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="cd4f8-132">Выберите шаблон **Веб-приложение (модель-представление-контроллер)**</span><span class="sxs-lookup"><span data-stu-id="cd4f8-132">Select the **Web Application (Model-View-Controller)** template</span></span>
* <span data-ttu-id="cd4f8-133">Убедитесь, что для параметра **Проверка подлинности** задано значение **Без проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-133">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="cd4f8-134">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-134">Click **OK**</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="cd4f8-135">Установка Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="cd4f8-135">Install Entity Framework Core</span></span>

<span data-ttu-id="cd4f8-136">Чтобы установить EF Core, установите пакеты целевых поставщиков базы данных EF Core, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-136">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="cd4f8-137">Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).</span><span class="sxs-lookup"><span data-stu-id="cd4f8-137">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="cd4f8-138">В этом руководстве используется SQL Server, поэтому устанавливать пакет поставщиков не требуется.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-138">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="cd4f8-139">Пакет поставщиков SQL Server включен в [метапакет Microsoft.AspnetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="cd4f8-139">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="cd4f8-140">Реконструирование модели</span><span class="sxs-lookup"><span data-stu-id="cd4f8-140">Reverse engineer your model</span></span>

<span data-ttu-id="cd4f8-141">Теперь пора создать модель EF на основе существующей базы данных:</span><span class="sxs-lookup"><span data-stu-id="cd4f8-141">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="cd4f8-142">Последовательно выберите пункты **Средства -> Диспетчер пакетов NuGet -> Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-142">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="cd4f8-143">Выполните следующую команду, чтобы создать модель на основе существующей базы данных:</span><span class="sxs-lookup"><span data-stu-id="cd4f8-143">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="cd4f8-144">Если появляется сообщение об ошибке `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, закройте и снова откройте Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-144">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="cd4f8-145">Вы можете выбрать, для каких таблиц нужно создать сущности, указав в команде выше аргумент `-Tables`.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-145">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="cd4f8-146">Например, `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-146">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="cd4f8-147">Процесс реконструирования создает классы сущностей (`Blog.cs` & `Post.cs`) и производный контекст (`BloggingContext.cs`) на основе схемы существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-147">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="cd4f8-148">Классы сущностей — это простые объекты C#, которые представляют данные для использования в запросах и командах сохранения.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-148">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="cd4f8-149">Ниже приведены классы сущностей `Blog` и `Post`:</span><span class="sxs-lookup"><span data-stu-id="cd4f8-149">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> <span data-ttu-id="cd4f8-150">Чтобы включить отложенную загрузку, можно присвоить свойствам навигации (Blog.Post и Post.Blog) значение `virtual`.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-150">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

 <span data-ttu-id="cd4f8-151">Контекст соответствует сеансу взаимодействия с базой данных и позволяет запрашивать и сохранять экземпляры классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-151">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- Static code listing, rather than a linked file, because the tutorial modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public BloggingContext()
    {
    }

    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    {
    }

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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="cd4f8-152">Регистрация контекста с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="cd4f8-152">Register your context with dependency injection</span></span>

<span data-ttu-id="cd4f8-153">Внедрение зависимостей является ключевой концепцией для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-153">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="cd4f8-154">С помощью внедрения зависимостей службы (например, `BloggingContext`) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-154">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="cd4f8-155">Затем компоненты, которые используют эти службы (например, контроллеры MVC), обращаются к ним через параметры или свойства конструктора.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-155">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="cd4f8-156">Дополнительные сведения о внедрении зависимостей вы найдете в статье [Introduction to Dependency Injection in ASP.NET Core](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) (Знакомство с концепцией внедрения зависимостей в ASP.NET Core) на сайте документации по этой платформе.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-156">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="cd4f8-157">Регистрация и настройка контекста в файле Startup.cs</span><span class="sxs-lookup"><span data-stu-id="cd4f8-157">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="cd4f8-158">Чтобы сделать контекст `BloggingContext` доступным MVC-контроллерам, зарегистрируйте его как службу.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-158">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="cd4f8-159">Откройте файл **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-159">Open **Startup.cs**</span></span>
* <span data-ttu-id="cd4f8-160">Добавьте в начало этого файла следующие инструкции `using`.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-160">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="cd4f8-161">Теперь вы можете использовать метод `AddDbContext(...)`, чтобы зарегистрировать его как службу.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-161">Now you can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="cd4f8-162">Найдите в коде метод `ConfigureServices(...)`.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-162">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="cd4f8-163">Добавьте в него следующий выделенный код, который регистрирует контекст в качестве службы</span><span class="sxs-lookup"><span data-stu-id="cd4f8-163">Add the following highlighted code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> <span data-ttu-id="cd4f8-164">В реальном приложении строка подключения обычно помещается в файл конфигурации или переменную среды.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-164">In a real application you would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="cd4f8-165">Для простоты в этом руководстве она будет задана в коде.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-165">For the sake of simplicity, this tutorial has you define it in code.</span></span> <span data-ttu-id="cd4f8-166">Дополнительные сведения см. в статье [Connection Strings](../../miscellaneous/connection-strings.md) (Строки подключения).</span><span class="sxs-lookup"><span data-stu-id="cd4f8-166">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="cd4f8-167">Создание контроллера и представлений</span><span class="sxs-lookup"><span data-stu-id="cd4f8-167">Create a controller and views</span></span>

* <span data-ttu-id="cd4f8-168">В **обозревателе решений** щелкните папку **Контроллеры** правой кнопкой мыши и выберите пункт **Добавить -> Контроллер**.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-168">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="cd4f8-169">Выберите **Контроллер MVC с представлениями, использующий Entity Framework** и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-169">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="cd4f8-170">Для параметра **Класс модели** установите значение **Блог**, а для параметра **Класс контекста данных** — **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-170">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="cd4f8-171">Щелкните **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-171">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="cd4f8-172">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="cd4f8-172">Run the application</span></span>

<span data-ttu-id="cd4f8-173">Теперь вы можете запустить приложение и увидеть, как оно работает:</span><span class="sxs-lookup"><span data-stu-id="cd4f8-173">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="cd4f8-174">Выберите **Отладка -> Запустить без отладки**.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-174">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="cd4f8-175">Будет выполнена сборка приложения, и оно откроется в браузере</span><span class="sxs-lookup"><span data-stu-id="cd4f8-175">The application builds and opens in a web browser</span></span>
* <span data-ttu-id="cd4f8-176">Перейдите к папке `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-176">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="cd4f8-177">Щелкните **Создать**.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-177">Click **Create New**</span></span>
* <span data-ttu-id="cd4f8-178">Введите **URL-адрес** для нового блога и щелкните **Создать**.</span><span class="sxs-lookup"><span data-stu-id="cd4f8-178">Enter a **Url** for the new blog and click **Create**</span></span>

  ![Страница "Создать"](_static/create.png)

  ![Страница индексов](_static/index-existing-db.png)

## <a name="next-steps"></a><span data-ttu-id="cd4f8-181">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="cd4f8-181">Next steps</span></span>

<span data-ttu-id="cd4f8-182">Дополнительные сведения о формировании классов контекста и сущности см. в следующих статьях:</span><span class="sxs-lookup"><span data-stu-id="cd4f8-182">For more information about how to scaffold a context and entity classes, see the following articles:</span></span>
* <span data-ttu-id="cd4f8-183">[Entity Framework Core tools reference - .NET CLI](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold) (Справочник по основным инструментам Entity Framework Core — .NET CLI)</span><span class="sxs-lookup"><span data-stu-id="cd4f8-183">[Entity Framework Core tools reference - .NET CLI](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)</span></span>
* <span data-ttu-id="cd4f8-184">[Entity Framework Core tools reference - Package Manager Console](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext) (Справочник по основным инструментам Entity Framework Core — консоль диспетчера пакетов)</span><span class="sxs-lookup"><span data-stu-id="cd4f8-184">[Entity Framework Core tools reference - Package Manager Console](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)</span></span>