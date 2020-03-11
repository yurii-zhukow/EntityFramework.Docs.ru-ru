---
title: Code First существующей базы данных — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: 0a51f826422d7e2bff33b968605eace1e754c425
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415601"
---
# <a name="code-first-to-an-existing-database"></a><span data-ttu-id="3cf07-102">Code First существующей базы данных</span><span class="sxs-lookup"><span data-stu-id="3cf07-102">Code First to an Existing Database</span></span>
<span data-ttu-id="3cf07-103">В этом видео и пошаговом руководстве представлены общие сведения о Code First разработке, предназначенной для существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="3cf07-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting an existing database.</span></span> <span data-ttu-id="3cf07-104">Code First позволяет определить модель с помощью классов C\# или VB.Net.</span><span class="sxs-lookup"><span data-stu-id="3cf07-104">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="3cf07-105">Дополнительно можно выполнить дополнительную настройку с помощью атрибутов в классах и свойствах или с помощью API-интерфейса Fluent.</span><span class="sxs-lookup"><span data-stu-id="3cf07-105">Optionally additional configuration can be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="3cf07-106">Просмотреть видео</span><span class="sxs-lookup"><span data-stu-id="3cf07-106">Watch the video</span></span>
<span data-ttu-id="3cf07-107">Это видео [теперь доступно на канале Channel 9](https://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span><span class="sxs-lookup"><span data-stu-id="3cf07-107">This video is [now available on Channel 9](https://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="3cf07-108">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="3cf07-108">Pre-Requisites</span></span>

<span data-ttu-id="3cf07-109">Для выполнения инструкций этого пошагового руководства необходимо установить **Visual Studio 2012** или **Visual Studio 2013** .</span><span class="sxs-lookup"><span data-stu-id="3cf07-109">You will need to have **Visual Studio 2012** or **Visual Studio 2013** installed to complete this walkthrough.</span></span>

<span data-ttu-id="3cf07-110">Также потребуется версия **6,1** (или более поздняя) **Entity Framework Tools для Visual Studio** .</span><span class="sxs-lookup"><span data-stu-id="3cf07-110">You will also need version **6.1** (or later) of the **Entity Framework Tools for Visual Studio** installed.</span></span> <span data-ttu-id="3cf07-111">Сведения об установке последней версии Entity Framework Tools см. в статье [получение Entity Framework](~/ef6/fundamentals/install.md) .</span><span class="sxs-lookup"><span data-stu-id="3cf07-111">See [Get Entity Framework](~/ef6/fundamentals/install.md) for information on installing the latest version of the Entity Framework Tools.</span></span>

## <a name="1-create-an-existing-database"></a><span data-ttu-id="3cf07-112">1. Создание существующей базы данных</span><span class="sxs-lookup"><span data-stu-id="3cf07-112">1. Create an Existing Database</span></span>

<span data-ttu-id="3cf07-113">Обычно при ориентировании на существующую базу данных она уже будет создана, но в этом пошаговом руководстве нам нужно создать базу данных для доступа.</span><span class="sxs-lookup"><span data-stu-id="3cf07-113">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="3cf07-114">Перейдем дальше и создадим базу данных.</span><span class="sxs-lookup"><span data-stu-id="3cf07-114">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="3cf07-115">Запустите Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3cf07-115">Open Visual Studio</span></span>
-   <span data-ttu-id="3cf07-116">**Просмотр —&gt; обозреватель сервера**</span><span class="sxs-lookup"><span data-stu-id="3cf07-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="3cf07-117">Щелкните правой кнопкой мыши **подключения к данным —&gt; добавить подключение...**</span><span class="sxs-lookup"><span data-stu-id="3cf07-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="3cf07-118">Если вы не подключились к базе данных из **Обозреватель сервера** , прежде чем нужно будет выбрать **Microsoft SQL Server** в качестве источника данных</span><span class="sxs-lookup"><span data-stu-id="3cf07-118">If you haven’t connected to a database from **Server Explorer** before you’ll need to select **Microsoft SQL Server** as the data source</span></span>

    ![Выберите источник данных](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="3cf07-120">Подключитесь к экземпляру LocalDB и введите в качестве имени базы данных **блог**</span><span class="sxs-lookup"><span data-stu-id="3cf07-120">Connect to your LocalDB instance, and enter **Blogging** as the database name</span></span>

    ![Соединение LocalDB](~/ef6/media/localdbconnection.png)

-   <span data-ttu-id="3cf07-122">Нажмите кнопку **ОК** , и появится запрос на создание новой базы данных, выберите **Да** .</span><span class="sxs-lookup"><span data-stu-id="3cf07-122">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Диалоговое окно создания базы данных](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="3cf07-124">Новая база данных появится в обозреватель сервера, щелкните ее правой кнопкой мыши и выберите пункт **создать запрос** .</span><span class="sxs-lookup"><span data-stu-id="3cf07-124">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="3cf07-125">Скопируйте следующий SQL в новый запрос, щелкните запрос правой кнопкой мыши и выберите команду **выполнить** .</span><span class="sxs-lookup"><span data-stu-id="3cf07-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Blogs] (
    [BlogId] INT IDENTITY (1, 1) NOT NULL,
    [Name] NVARCHAR (200) NULL,
    [Url]  NVARCHAR (200) NULL,
    CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
);

CREATE TABLE [dbo].[Posts] (
    [PostId] INT IDENTITY (1, 1) NOT NULL,
    [Title] NVARCHAR (200) NULL,
    [Content] NTEXT NULL,
    [BlogId] INT NOT NULL,
    CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
    CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
);

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('The Visual Studio Blog', 'http://blogs.msdn.com/visualstudio/')

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('.NET Framework Blog', 'http://blogs.msdn.com/dotnet/')
```

## <a name="2-create-the-application"></a><span data-ttu-id="3cf07-126">2. Создание приложения</span><span class="sxs-lookup"><span data-stu-id="3cf07-126">2. Create the Application</span></span>

<span data-ttu-id="3cf07-127">Для простоты мы создадим простое консольное приложение, которое использует Code First для доступа к данным:</span><span class="sxs-lookup"><span data-stu-id="3cf07-127">To keep things simple we will build a basic console application that uses Code First to do the data access:</span></span>

-   <span data-ttu-id="3cf07-128">Запустите Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3cf07-128">Open Visual Studio</span></span>
-   <span data-ttu-id="3cf07-129">**Файл-&gt; проект New-&gt;...**</span><span class="sxs-lookup"><span data-stu-id="3cf07-129">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="3cf07-130">Выбор **окон** в меню слева и **консольное приложение**</span><span class="sxs-lookup"><span data-stu-id="3cf07-130">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="3cf07-131">Введите **кодефирстексистингдатабасесампле** в качестве имени</span><span class="sxs-lookup"><span data-stu-id="3cf07-131">Enter **CodeFirstExistingDatabaseSample** as the name</span></span>
-   <span data-ttu-id="3cf07-132">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="3cf07-132">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="3cf07-133">3. реконструирование модели</span><span class="sxs-lookup"><span data-stu-id="3cf07-133">3. Reverse Engineer Model</span></span>

<span data-ttu-id="3cf07-134">Мы будем использовать Entity Framework Tools для Visual Studio, чтобы помочь нам создать начальный код для сопоставлений с базой данных.</span><span class="sxs-lookup"><span data-stu-id="3cf07-134">We will use the Entity Framework Tools for Visual Studio to help us generate some initial code to map to the database.</span></span> <span data-ttu-id="3cf07-135">Эти средства просто создают код, который можно также ввести вручную при желании.</span><span class="sxs-lookup"><span data-stu-id="3cf07-135">These tools are just generating code that you could also type by hand if you prefer.</span></span>

-   <span data-ttu-id="3cf07-136">**Проект-&gt; добавить новый элемент...**</span><span class="sxs-lookup"><span data-stu-id="3cf07-136">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="3cf07-137">Выберите **данные** в меню слева, а затем **ADO.NET EDM**</span><span class="sxs-lookup"><span data-stu-id="3cf07-137">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="3cf07-138">Введите **BloggingContext** в качестве имени и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="3cf07-138">Enter **BloggingContext** as the name and click **OK**</span></span>
-   <span data-ttu-id="3cf07-139">Запустится **мастер EDM**</span><span class="sxs-lookup"><span data-stu-id="3cf07-139">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="3cf07-140">Выберите **Code First из базы данных** и нажмите кнопку **Далее** .</span><span class="sxs-lookup"><span data-stu-id="3cf07-140">Select **Code First from Database** and click **Next**</span></span>

    ![Мастер 1 CFE](~/ef6/media/wizardonecfe.png)

-   <span data-ttu-id="3cf07-142">Выберите подключение к базе данных, созданной в первом разделе, и нажмите кнопку **Далее** .</span><span class="sxs-lookup"><span data-stu-id="3cf07-142">Select the connection to the database you created in the first section and click **Next**</span></span>

    ![Мастер 2 CFE](~/ef6/media/wizardtwocfe.png)

-   <span data-ttu-id="3cf07-144">Установите флажок рядом с **таблицами** , чтобы импортировать все таблицы, и нажмите кнопку **Готово** .</span><span class="sxs-lookup"><span data-stu-id="3cf07-144">Click the checkbox next to **Tables** to import all tables and click **Finish**</span></span>

    ![Мастер 3 CFE](~/ef6/media/wizardthreecfe.png)

<span data-ttu-id="3cf07-146">После завершения процесса реконструирования в проект будет добавлено несколько элементов, давайте посмотрим, что было добавлено.</span><span class="sxs-lookup"><span data-stu-id="3cf07-146">Once the reverse engineer process completes a number of items will have been added to the project, let's take a look at what's been added.</span></span>

### <a name="configuration-file"></a><span data-ttu-id="3cf07-147">Файл конфигурации</span><span class="sxs-lookup"><span data-stu-id="3cf07-147">Configuration file</span></span>

<span data-ttu-id="3cf07-148">Файл App. config добавлен в проект, этот файл содержит строку подключения к существующей базе данных.</span><span class="sxs-lookup"><span data-stu-id="3cf07-148">An App.config file has been added to the project, this file contains the connection string to the existing database.</span></span>

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

<span data-ttu-id="3cf07-149">*В файле конфигурации также можно заметить некоторые другие параметры. это параметры по умолчанию EF, которые указывают Code First, где создавать базы данных. Так как мы сопоставлены с существующей базой данных, этот параметр будет пропущен в нашем приложении.*</span><span class="sxs-lookup"><span data-stu-id="3cf07-149">*You’ll notice some other settings in the configuration file too, these are default EF settings that tell Code First where to create databases. Since we are mapping to an existing database these setting will be ignored in our application.*</span></span>

### <a name="derived-context"></a><span data-ttu-id="3cf07-150">Производный контекст</span><span class="sxs-lookup"><span data-stu-id="3cf07-150">Derived Context</span></span>

<span data-ttu-id="3cf07-151">В проект был добавлен класс **BloggingContext** .</span><span class="sxs-lookup"><span data-stu-id="3cf07-151">A **BloggingContext** class has been added to the project.</span></span> <span data-ttu-id="3cf07-152">Контекст представляет сеанс с базой данных, что позволяет нам запрашивать и сохранять данные.</span><span class="sxs-lookup"><span data-stu-id="3cf07-152">The context represents a session with the database, allowing us to query and save data.</span></span>
<span data-ttu-id="3cf07-153">Контекст предоставляет **&gt;DbSet&lt;** для каждого типа в нашей модели.</span><span class="sxs-lookup"><span data-stu-id="3cf07-153">The context exposes a **DbSet&lt;TEntity&gt;** for each type in our model.</span></span> <span data-ttu-id="3cf07-154">Также обратите внимание, что конструктор по умолчанию вызывает базовый конструктор, используя синтаксис **Name =** .</span><span class="sxs-lookup"><span data-stu-id="3cf07-154">You’ll also notice that the default constructor calls a base constructor using the **name=** syntax.</span></span> <span data-ttu-id="3cf07-155">Это говорит Code First, что строка подключения, используемая для этого контекста, должна быть загружена из файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="3cf07-155">This tells Code First that the connection string to use for this context should be loaded from the configuration file.</span></span>

``` csharp
public partial class BloggingContext : DbContext
    {
        public BloggingContext()
            : base("name=BloggingContext")
        {
        }

        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
        }
    }
```

<span data-ttu-id="3cf07-156">*При использовании строки подключения в файле конфигурации всегда следует использовать синтаксис **Name =** . Это гарантирует, что если строка подключения отсутствует, Entity Framework выдаст исключение, а не создает новую базу данных по соглашению.*</span><span class="sxs-lookup"><span data-stu-id="3cf07-156">*You should always use the **name=** syntax when you are using a connection string in the config file. This ensures that if the connection string is not present then Entity Framework will throw rather than creating a new database by convention.*</span></span>

### <a name="model-classes"></a><span data-ttu-id="3cf07-157">Классы моделей</span><span class="sxs-lookup"><span data-stu-id="3cf07-157">Model classes</span></span>

<span data-ttu-id="3cf07-158">Наконец, в проект также добавлены **блог** и класс **POST** .</span><span class="sxs-lookup"><span data-stu-id="3cf07-158">Finally, a **Blog** and **Post** class have also been added to the project.</span></span> <span data-ttu-id="3cf07-159">Это классы домена, составляющие модель.</span><span class="sxs-lookup"><span data-stu-id="3cf07-159">These are the domain classes that make up our model.</span></span> <span data-ttu-id="3cf07-160">Вы увидите заметки к данным, примененные к классам, чтобы указать конфигурацию, в которой Code First соглашения не будут согласованы с структурой существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="3cf07-160">You'll see Data Annotations applied to the classes to specify configuration where the Code First conventions would not align with the structure of the existing database.</span></span> <span data-ttu-id="3cf07-161">Например, вы увидите заметку **StringLength** для **Blog.Name** и **Blog. URL** , так как в базе данных максимальная длина **200** (Code First по умолчанию — использование длины максимун, поддерживаемой поставщиком базы данных- **nvarchar (max)** в SQL Server).</span><span class="sxs-lookup"><span data-stu-id="3cf07-161">For example, you'll see the **StringLength** annotation on **Blog.Name** and **Blog.Url** since they have a maximum length of **200** in the database (the Code First default is to use the maximun length supported by the database provider - **nvarchar(max)** in SQL Server).</span></span>

``` csharp
public partial class Blog
{
    public Blog()
    {
        Posts = new HashSet<Post>();
    }

    public int BlogId { get; set; }

    [StringLength(200)]
    public string Name { get; set; }

    [StringLength(200)]
    public string Url { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

## <a name="4-reading--writing-data"></a><span data-ttu-id="3cf07-162">4. Чтение & запись данных</span><span class="sxs-lookup"><span data-stu-id="3cf07-162">4. Reading & Writing Data</span></span>

<span data-ttu-id="3cf07-163">Теперь, когда у нас есть модель, настала пора использовать ее для доступа к каким-нибудь данным.</span><span class="sxs-lookup"><span data-stu-id="3cf07-163">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="3cf07-164">Реализуйте метод **Main** в **Program.CS** , как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="3cf07-164">Implement the **Main** method in **Program.cs** as shown below.</span></span> <span data-ttu-id="3cf07-165">Этот код создает новый экземпляр нашего контекста, а затем использует его для вставки нового **блога**.</span><span class="sxs-lookup"><span data-stu-id="3cf07-165">This code creates a new instance of our context and then uses it to insert a new **Blog**.</span></span> <span data-ttu-id="3cf07-166">Затем он использует запрос LINQ для получения всех **блогов** из базы данных, упорядоченного в алфавитном порядке по **названию**.</span><span class="sxs-lookup"><span data-stu-id="3cf07-166">Then it uses a LINQ query to retrieve all **Blogs** from the database ordered alphabetically by **Title**.</span></span>

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

<span data-ttu-id="3cf07-167">Теперь можно запустить приложение и протестировать его.</span><span class="sxs-lookup"><span data-stu-id="3cf07-167">You can now run the application and test it.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a><span data-ttu-id="3cf07-168">Что делать, если изменилась моя база данных?</span><span class="sxs-lookup"><span data-stu-id="3cf07-168">What if My Database Changes?</span></span>

<span data-ttu-id="3cf07-169">Мастер Code First к базе данных предназначен для создания начального набора классов, которые затем можно настраивать и изменять.</span><span class="sxs-lookup"><span data-stu-id="3cf07-169">The Code First to Database wizard is designed to generate a starting point set of classes that you can then tweak and modify.</span></span> <span data-ttu-id="3cf07-170">Если схема базы данных изменяется, можно либо вручную изменить классы, либо выполнить другой реконструирование, чтобы перезаписать классы.</span><span class="sxs-lookup"><span data-stu-id="3cf07-170">If your database schema changes you can either manually edit the classes or perform another reverse engineer to overwrite the classes.</span></span>

## <a name="using-code-first-migrations-to-an-existing-database"></a><span data-ttu-id="3cf07-171">Использование Code First Migrations к существующей базе данных</span><span class="sxs-lookup"><span data-stu-id="3cf07-171">Using Code First Migrations to an Existing Database</span></span>

<span data-ttu-id="3cf07-172">Если вы хотите использовать Code First Migrations с существующей базой данных, см. статью [Code First migrations к существующей базе данных](~/ef6/modeling/code-first/migrations/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="3cf07-172">If you want to use Code First Migrations with an existing database, see [Code First Migrations to an existing database](~/ef6/modeling/code-first/migrations/existing-database.md).</span></span>

## <a name="summary"></a><span data-ttu-id="3cf07-173">Сводка</span><span class="sxs-lookup"><span data-stu-id="3cf07-173">Summary</span></span>

<span data-ttu-id="3cf07-174">В этом пошаговом руководстве мы рассматривали Code Firstную разработку, используя существующую базу данных.</span><span class="sxs-lookup"><span data-stu-id="3cf07-174">In this walkthrough we looked at Code First development using an existing database.</span></span> <span data-ttu-id="3cf07-175">В Visual Studio используется Entity Framework Tools для реконструирования набора классов, сопоставленных с базой данных, которые можно использовать для хранения и извлечения данных.</span><span class="sxs-lookup"><span data-stu-id="3cf07-175">We used the Entity Framework Tools for Visual Studio to reverse engineer a set of classes that mapped to the database and could be used to store and retrieve data.</span></span>
