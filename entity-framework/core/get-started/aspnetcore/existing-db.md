---
title: Приступая к работе с ASP.NET Core — существующая база данных — EF Core
author: rowanmiller
description: Приступая к работе с EF Core в ASP.NET Core с существующей базой данных
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: 6b0ed0a9222644bee31d23234aa27b2084137f4a
ms.sourcegitcommit: 755a15a789631cc4ea581e2262a2dcc49c219eef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2019
ms.locfileid: "68497516"
---
# <a name="get-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="88c82-103">Приступая к работе с EF Core в ASP.NET Core с существующей базой данных</span><span class="sxs-lookup"><span data-stu-id="88c82-103">Get started with EF Core on ASP.NET Core with an existing database</span></span>

<span data-ttu-id="88c82-104">В этом руководстве вы создадите приложение ASP.NET Core MVC, которое выполняет базовые операции доступа к данным через платформу Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="88c82-104">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="88c82-105">Для создания модели Entity Framework используется реконструирование существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="88c82-105">You reverse engineer an existing database to create an Entity Framework model.</span></span>

<span data-ttu-id="88c82-106">[Пример для этой статьи на GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="88c82-106">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="88c82-107">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="88c82-107">Prerequisites</span></span>

<span data-ttu-id="88c82-108">Установите следующее программное обеспечение:</span><span class="sxs-lookup"><span data-stu-id="88c82-108">Install the following software:</span></span>

* <span data-ttu-id="88c82-109">[Visual Studio 2017 версии 15.7](https://www.visualstudio.com/downloads/) со следующими рабочими нагрузками:</span><span class="sxs-lookup"><span data-stu-id="88c82-109">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="88c82-110">**ASP.NET и веб-разработка** в разделе **Интернет и облако**)</span><span class="sxs-lookup"><span data-stu-id="88c82-110">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="88c82-111">**Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)</span><span class="sxs-lookup"><span data-stu-id="88c82-111">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="88c82-112">[Пакет SDK для NET Core 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="88c82-112">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-blogging-database"></a><span data-ttu-id="88c82-113">Создание базы данных Blogging</span><span class="sxs-lookup"><span data-stu-id="88c82-113">Create Blogging database</span></span>

<span data-ttu-id="88c82-114">В этом руководстве в качестве существующей базы данных используется база данных **для ведения блогов**, размещенная на локальном экземпляре LocalDb.</span><span class="sxs-lookup"><span data-stu-id="88c82-114">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span> <span data-ttu-id="88c82-115">Если вы уже создали базу данных **Blogging**, работая с другим руководством, пропустите эти шаги.</span><span class="sxs-lookup"><span data-stu-id="88c82-115">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="88c82-116">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="88c82-116">Open Visual Studio</span></span>
* <span data-ttu-id="88c82-117">Выберите **Инструменты -> Подключиться к базе данных...** .</span><span class="sxs-lookup"><span data-stu-id="88c82-117">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="88c82-118">Выберите **Microsoft SQL Server** и щелкните **Продолжить**.</span><span class="sxs-lookup"><span data-stu-id="88c82-118">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="88c82-119">Введите значение **(localdb)\mssqllocaldb** для параметра **Имя сервера**.</span><span class="sxs-lookup"><span data-stu-id="88c82-119">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="88c82-120">Введите значение **master** для параметра **Имя базы данных**, затем щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="88c82-120">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="88c82-121">Теперь база данных master появится в разделе **Подключения к данным** в **обозревателе сервера**.</span><span class="sxs-lookup"><span data-stu-id="88c82-121">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="88c82-122">Щелкните правой кнопкой мыши базу данных в **обозревателе сервера** и выберите действие **Создать запрос**.</span><span class="sxs-lookup"><span data-stu-id="88c82-122">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="88c82-123">Скопируйте представленный ниже скрипт и вставьте его в редактор запросов</span><span class="sxs-lookup"><span data-stu-id="88c82-123">Copy the script listed below into the query editor</span></span>
* <span data-ttu-id="88c82-124">Щелкните область редактора запросов правой кнопкой мыши и выберите действие **Выполнить**.</span><span class="sxs-lookup"><span data-stu-id="88c82-124">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="88c82-125">Создание нового проекта</span><span class="sxs-lookup"><span data-stu-id="88c82-125">Create a new project</span></span>

* <span data-ttu-id="88c82-126">Откройте Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="88c82-126">Open Visual Studio 2017</span></span>
* <span data-ttu-id="88c82-127">**"Файл" > "Создать" > "Проект"…**</span><span class="sxs-lookup"><span data-stu-id="88c82-127">**File > New > Project...**</span></span>
* <span data-ttu-id="88c82-128">В меню слева выберите **"Установленные" > Visual C# > Интернет**</span><span class="sxs-lookup"><span data-stu-id="88c82-128">From the left menu select **Installed > Visual C# > Web**</span></span>
* <span data-ttu-id="88c82-129">Выберите шаблон проекта **Веб-приложение ASP.NET Core**</span><span class="sxs-lookup"><span data-stu-id="88c82-129">Select the **ASP.NET Core Web Application** project template</span></span>
* <span data-ttu-id="88c82-130">Введите имя **EFGetStarted.AspNetCore.ExistingDb** (требуется точное соответствие пространству имен, используемому позже) и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="88c82-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name (it has to match exactly the namespace later used in the code) and click **OK**</span></span> 
* <span data-ttu-id="88c82-131">Дождитесь появления диалогового окна **Создание веб-приложения ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="88c82-131">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="88c82-132">В соответствующих раскрывающихся списках должна быть выбрана целевая платформа **.NET Core** и версия **ASP.NET Core 2.1**</span><span class="sxs-lookup"><span data-stu-id="88c82-132">Make sure that the target framework dropdown is set to **.NET Core**, and the version dropdown is set to **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="88c82-133">Выберите шаблон **Веб-приложение (модель-представление-контроллер)**</span><span class="sxs-lookup"><span data-stu-id="88c82-133">Select the **Web Application (Model-View-Controller)** template</span></span>
* <span data-ttu-id="88c82-134">Убедитесь, что для параметра **Проверка подлинности** задано значение **Без проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="88c82-134">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="88c82-135">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="88c82-135">Click **OK**</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="88c82-136">Установка Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="88c82-136">Install Entity Framework Core</span></span>

<span data-ttu-id="88c82-137">Чтобы установить EF Core, установите пакеты целевых поставщиков базы данных EF Core, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="88c82-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="88c82-138">Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).</span><span class="sxs-lookup"><span data-stu-id="88c82-138">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="88c82-139">В этом руководстве используется SQL Server, поэтому устанавливать пакет поставщиков не требуется.</span><span class="sxs-lookup"><span data-stu-id="88c82-139">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="88c82-140">Пакет поставщиков SQL Server включен в [метапакет Microsoft.AspnetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="88c82-140">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="88c82-141">Реконструирование модели</span><span class="sxs-lookup"><span data-stu-id="88c82-141">Reverse engineer your model</span></span>

<span data-ttu-id="88c82-142">Теперь пора создать модель EF на основе существующей базы данных:</span><span class="sxs-lookup"><span data-stu-id="88c82-142">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="88c82-143">Последовательно выберите пункты **Средства -> Диспетчер пакетов NuGet -> Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="88c82-143">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="88c82-144">Выполните следующую команду, чтобы создать модель на основе существующей базы данных:</span><span class="sxs-lookup"><span data-stu-id="88c82-144">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="88c82-145">Если появляется сообщение об ошибке `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, закройте и снова откройте Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="88c82-145">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="88c82-146">Вы можете выбрать, для каких таблиц нужно создать сущности, указав в команде выше аргумент `-Tables`.</span><span class="sxs-lookup"><span data-stu-id="88c82-146">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="88c82-147">Например, `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="88c82-147">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="88c82-148">Процесс реконструирования создает классы сущностей (`Blog.cs` & `Post.cs`) и производный контекст (`BloggingContext.cs`) на основе схемы существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="88c82-148">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="88c82-149">Классы сущностей — это простые объекты C#, которые представляют данные для использования в запросах и командах сохранения.</span><span class="sxs-lookup"><span data-stu-id="88c82-149">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="88c82-150">Ниже приведены классы сущностей `Blog` и `Post`:</span><span class="sxs-lookup"><span data-stu-id="88c82-150">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> <span data-ttu-id="88c82-151">Чтобы включить отложенную загрузку, можно присвоить свойствам навигации (Blog.Post и Post.Blog) значение `virtual`.</span><span class="sxs-lookup"><span data-stu-id="88c82-151">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

 <span data-ttu-id="88c82-152">Контекст соответствует сеансу взаимодействия с базой данных и позволяет запрашивать и сохранять экземпляры классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="88c82-152">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="88c82-153">Регистрация контекста с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="88c82-153">Register your context with dependency injection</span></span>

<span data-ttu-id="88c82-154">Внедрение зависимостей является ключевой концепцией для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="88c82-154">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="88c82-155">С помощью внедрения зависимостей службы (например, `BloggingContext`) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="88c82-155">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="88c82-156">Затем компоненты, которые используют эти службы (например, контроллеры MVC), обращаются к ним через параметры или свойства конструктора.</span><span class="sxs-lookup"><span data-stu-id="88c82-156">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="88c82-157">Дополнительные сведения о внедрении зависимостей вы найдете в статье [Introduction to Dependency Injection in ASP.NET Core](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) (Знакомство с концепцией внедрения зависимостей в ASP.NET Core) на сайте документации по этой платформе.</span><span class="sxs-lookup"><span data-stu-id="88c82-157">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="88c82-158">Регистрация и настройка контекста в файле Startup.cs</span><span class="sxs-lookup"><span data-stu-id="88c82-158">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="88c82-159">Чтобы сделать контекст `BloggingContext` доступным MVC-контроллерам, зарегистрируйте его как службу.</span><span class="sxs-lookup"><span data-stu-id="88c82-159">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="88c82-160">Откройте файл **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="88c82-160">Open **Startup.cs**</span></span>
* <span data-ttu-id="88c82-161">Добавьте в начало этого файла следующие инструкции `using`.</span><span class="sxs-lookup"><span data-stu-id="88c82-161">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="88c82-162">Теперь вы можете использовать метод `AddDbContext(...)`, чтобы зарегистрировать его как службу.</span><span class="sxs-lookup"><span data-stu-id="88c82-162">Now you can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="88c82-163">Найдите в коде метод `ConfigureServices(...)`.</span><span class="sxs-lookup"><span data-stu-id="88c82-163">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="88c82-164">Добавьте в него следующий выделенный код, который регистрирует контекст в качестве службы</span><span class="sxs-lookup"><span data-stu-id="88c82-164">Add the following highlighted code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> <span data-ttu-id="88c82-165">В реальном приложении строка подключения обычно помещается в файл конфигурации или переменную среды.</span><span class="sxs-lookup"><span data-stu-id="88c82-165">In a real application you would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="88c82-166">Для простоты в этом руководстве она будет задана в коде.</span><span class="sxs-lookup"><span data-stu-id="88c82-166">For the sake of simplicity, this tutorial has you define it in code.</span></span> <span data-ttu-id="88c82-167">Дополнительные сведения см. в статье [Connection Strings](../../miscellaneous/connection-strings.md) (Строки подключения).</span><span class="sxs-lookup"><span data-stu-id="88c82-167">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="88c82-168">Создание контроллера и представлений</span><span class="sxs-lookup"><span data-stu-id="88c82-168">Create a controller and views</span></span>

* <span data-ttu-id="88c82-169">В **обозревателе решений** щелкните папку **Контроллеры** правой кнопкой мыши и выберите пункт **Добавить -> Контроллер**.</span><span class="sxs-lookup"><span data-stu-id="88c82-169">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="88c82-170">Выберите **Контроллер MVC с представлениями, использующий Entity Framework** и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="88c82-170">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="88c82-171">Для параметра **Класс модели** установите значение **Блог**, а для параметра **Класс контекста данных** — **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="88c82-171">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="88c82-172">Щелкните **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="88c82-172">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="88c82-173">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="88c82-173">Run the application</span></span>

<span data-ttu-id="88c82-174">Теперь вы можете запустить приложение и увидеть, как оно работает:</span><span class="sxs-lookup"><span data-stu-id="88c82-174">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="88c82-175">Выберите **Отладка -> Запустить без отладки**.</span><span class="sxs-lookup"><span data-stu-id="88c82-175">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="88c82-176">Будет выполнена сборка приложения, и оно откроется в браузере</span><span class="sxs-lookup"><span data-stu-id="88c82-176">The application builds and opens in a web browser</span></span>
* <span data-ttu-id="88c82-177">Перейдите к папке `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="88c82-177">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="88c82-178">Щелкните **Создать**.</span><span class="sxs-lookup"><span data-stu-id="88c82-178">Click **Create New**</span></span>
* <span data-ttu-id="88c82-179">Введите **URL-адрес** для нового блога и щелкните **Создать**.</span><span class="sxs-lookup"><span data-stu-id="88c82-179">Enter a **Url** for the new blog and click **Create**</span></span>

  ![Страница "Создать"](_static/create.png)

  ![Страница индексов](_static/index-existing-db.png)

## <a name="next-steps"></a><span data-ttu-id="88c82-182">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="88c82-182">Next steps</span></span>

<span data-ttu-id="88c82-183">Дополнительные сведения о формировании классов контекста и сущности см. в следующих статьях:</span><span class="sxs-lookup"><span data-stu-id="88c82-183">For more information about how to scaffold a context and entity classes, see the following articles:</span></span>
* [<span data-ttu-id="88c82-184">Реконструирование</span><span class="sxs-lookup"><span data-stu-id="88c82-184">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
* <span data-ttu-id="88c82-185">[Entity Framework Core tools reference - .NET CLI](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold) (Справочник по основным инструментам Entity Framework Core — .NET CLI)</span><span class="sxs-lookup"><span data-stu-id="88c82-185">[Entity Framework Core tools reference - .NET CLI](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)</span></span>
* <span data-ttu-id="88c82-186">[Entity Framework Core tools reference - Package Manager Console](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext) (Справочник по основным инструментам Entity Framework Core — консоль диспетчера пакетов)</span><span class="sxs-lookup"><span data-stu-id="88c82-186">[Entity Framework Core tools reference - Package Manager Console](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)</span></span>
