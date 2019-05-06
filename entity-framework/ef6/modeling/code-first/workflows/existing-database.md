---
title: Code First для существующей базы данных - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: f05420beb3dff2d632151fcbf48986b0d9cd18ff
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490614"
---
# <a name="code-first-to-an-existing-database"></a><span data-ttu-id="a13dc-102">Code First для существующей базы данных</span><span class="sxs-lookup"><span data-stu-id="a13dc-102">Code First to an Existing Database</span></span>
<span data-ttu-id="a13dc-103">В этом пошаговом руководстве видео и пошаговые познакомят вас с разработки Code First, предназначенных для существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="a13dc-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting an existing database.</span></span> <span data-ttu-id="a13dc-104">Во-первых, код позволяет определить модель с помощью C#\# или классам VB.Net.</span><span class="sxs-lookup"><span data-stu-id="a13dc-104">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="a13dc-105">При необходимости дополнительной настройки могут выполняться с помощью атрибутов для классов и свойств или с помощью текучего API.</span><span class="sxs-lookup"><span data-stu-id="a13dc-105">Optionally additional configuration can be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="a13dc-106">Просмотреть видео</span><span class="sxs-lookup"><span data-stu-id="a13dc-106">Watch the video</span></span>
<span data-ttu-id="a13dc-107">Этот видеоролик — [теперь доступна на канале Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span><span class="sxs-lookup"><span data-stu-id="a13dc-107">This video is [now available on Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="a13dc-108">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="a13dc-108">Pre-Requisites</span></span>

<span data-ttu-id="a13dc-109">Необходимо иметь **Visual Studio 2012** или **Visual Studio 2013** для выполнения этого пошагового руководства.</span><span class="sxs-lookup"><span data-stu-id="a13dc-109">You will need to have **Visual Studio 2012** or **Visual Studio 2013** installed to complete this walkthrough.</span></span>

<span data-ttu-id="a13dc-110">Вам также потребуется версии **6.1** (или более поздней версии) из **средства платформы Entity Framework для Visual Studio** установлен.</span><span class="sxs-lookup"><span data-stu-id="a13dc-110">You will also need version **6.1** (or later) of the **Entity Framework Tools for Visual Studio** installed.</span></span> <span data-ttu-id="a13dc-111">См. в разделе [получение Entity Framework](~/ef6/fundamentals/install.md) сведения об установке последней версии средства платформы Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a13dc-111">See [Get Entity Framework](~/ef6/fundamentals/install.md) for information on installing the latest version of the Entity Framework Tools.</span></span>

## <a name="1-create-an-existing-database"></a><span data-ttu-id="a13dc-112">1. Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="a13dc-112">1. Create an Existing Database</span></span>

<span data-ttu-id="a13dc-113">Обычно при ориентировании существующей базы данных, он будет уже создан, но для этого пошагового руководства необходимо создать базу данных для доступа к.</span><span class="sxs-lookup"><span data-stu-id="a13dc-113">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="a13dc-114">Перейдем дальше и создать базу данных.</span><span class="sxs-lookup"><span data-stu-id="a13dc-114">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="a13dc-115">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a13dc-115">Open Visual Studio</span></span>
-   <span data-ttu-id="a13dc-116">**Представление —&gt; обозревателя серверов**</span><span class="sxs-lookup"><span data-stu-id="a13dc-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="a13dc-117">Щелкните правой кнопкой мыши **подключения к данным -&gt; добавить соединение...**</span><span class="sxs-lookup"><span data-stu-id="a13dc-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="a13dc-118">Если вы не подключились к базе данных из **обозревателя серверов** прежде, чем вам нужно будет выбрать **Microsoft SQL Server** как источник данных</span><span class="sxs-lookup"><span data-stu-id="a13dc-118">If you haven’t connected to a database from **Server Explorer** before you’ll need to select **Microsoft SQL Server** as the data source</span></span>

    ![Выберите источник данных](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="a13dc-120">Подключитесь к экземпляру LocalDB и введите **ведения блогов** имя базы данных</span><span class="sxs-lookup"><span data-stu-id="a13dc-120">Connect to your LocalDB instance, and enter **Blogging** as the database name</span></span>

    ![Подключение LocalDB](~/ef6/media/localdbconnection.png)

-   <span data-ttu-id="a13dc-122">Выберите **ОК** и вам нужно будет Если вы хотите создать новую базу данных, выберите **Да**</span><span class="sxs-lookup"><span data-stu-id="a13dc-122">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Создание диалогового окна базы данных](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="a13dc-124">Новой базы данных будут отображаться в обозревателе сервера щелкните его правой кнопкой мыши и выберите **новый запрос**</span><span class="sxs-lookup"><span data-stu-id="a13dc-124">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="a13dc-125">Скопируйте следующий запрос SQL в новый запрос, а затем щелкните правой кнопкой мыши запрос и выберите **Execute**</span><span class="sxs-lookup"><span data-stu-id="a13dc-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

## <a name="2-create-the-application"></a><span data-ttu-id="a13dc-126">2. Создание приложения</span><span class="sxs-lookup"><span data-stu-id="a13dc-126">2. Create the Application</span></span>

<span data-ttu-id="a13dc-127">Для простоты мы собираемся создать простое консольное приложение, которое использует Code First для осуществления доступа к данным:</span><span class="sxs-lookup"><span data-stu-id="a13dc-127">To keep things simple we’re going to build a basic console application that uses Code First to perform data access:</span></span>

-   <span data-ttu-id="a13dc-128">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a13dc-128">Open Visual Studio</span></span>
-   <span data-ttu-id="a13dc-129">**Файл —&gt; Новинка —&gt; проекта...**</span><span class="sxs-lookup"><span data-stu-id="a13dc-129">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="a13dc-130">Выберите **Windows** в меню слева и **консольного приложения**</span><span class="sxs-lookup"><span data-stu-id="a13dc-130">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="a13dc-131">Введите **CodeFirstExistingDatabaseSample** как имя</span><span class="sxs-lookup"><span data-stu-id="a13dc-131">Enter **CodeFirstExistingDatabaseSample** as the name</span></span>
-   <span data-ttu-id="a13dc-132">Нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="a13dc-132">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="a13dc-133">3. Реконструирование модели</span><span class="sxs-lookup"><span data-stu-id="a13dc-133">3. Reverse Engineer Model</span></span>

<span data-ttu-id="a13dc-134">Мы будем использовать инструменты Entity Framework для Visual Studio позволяет нам создать некоторый исходный код для сопоставления с базой данных.</span><span class="sxs-lookup"><span data-stu-id="a13dc-134">We’re going to make use of the Entity Framework Tools for Visual Studio to help us generate some initial code to map to the database.</span></span> <span data-ttu-id="a13dc-135">Эти средства просто генерирует код, который также можно ввести вручную при необходимости.</span><span class="sxs-lookup"><span data-stu-id="a13dc-135">These tools are just generating code that you could also type by hand if you prefer.</span></span>

-   <span data-ttu-id="a13dc-136">**Проект -&gt; добавить новый элемент...**</span><span class="sxs-lookup"><span data-stu-id="a13dc-136">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="a13dc-137">Выберите **данных** в меню слева и затем **модель EDM ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="a13dc-137">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="a13dc-138">Введите **BloggingContext** имя и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="a13dc-138">Enter **BloggingContext** as the name and click **OK**</span></span>
-   <span data-ttu-id="a13dc-139">Это откроет **мастер моделей EDM**</span><span class="sxs-lookup"><span data-stu-id="a13dc-139">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="a13dc-140">Выберите **Code First, из базы данных** и нажмите кнопку **Далее**</span><span class="sxs-lookup"><span data-stu-id="a13dc-140">Select **Code First from Database** and click **Next**</span></span>

    ![Мастер один CFE](~/ef6/media/wizardonecfe.png)

-   <span data-ttu-id="a13dc-142">Выберите соединение с базой данных, созданной в первом разделе и нажмите кнопку **Далее**</span><span class="sxs-lookup"><span data-stu-id="a13dc-142">Select the connection to the database you created in the first section and click **Next**</span></span>

    ![Мастер два CFE](~/ef6/media/wizardtwocfe.png)

-   <span data-ttu-id="a13dc-144">Установите флажок рядом с полем **таблиц** для импорта всех таблиц и нажмите кнопку **Готово**</span><span class="sxs-lookup"><span data-stu-id="a13dc-144">Click the checkbox next to **Tables** to import all tables and click **Finish**</span></span>

    ![Мастер три CFE](~/ef6/media/wizardthreecfe.png)

<span data-ttu-id="a13dc-146">После завершения процесса реконструирования с числом элементов будут добавлены в проект, давайте взглянем на то, что добавляется.</span><span class="sxs-lookup"><span data-stu-id="a13dc-146">Once the reverse engineer process completes a number of items will have been added to the project, let's take a look at what's been added.</span></span>

### <a name="configuration-file"></a><span data-ttu-id="a13dc-147">файл конфигурации</span><span class="sxs-lookup"><span data-stu-id="a13dc-147">Configuration file</span></span>

<span data-ttu-id="a13dc-148">Файл App.config был добавлен в проект, в этот файл содержит строку подключения к существующей базе данных.</span><span class="sxs-lookup"><span data-stu-id="a13dc-148">An App.config file has been added to the project, this file contains the connection string to the existing database.</span></span>

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

<span data-ttu-id="a13dc-149">*Вы заметите некоторые параметры в файле конфигурации слишком, это параметры по умолчанию EF, которые сообщают, Code First для создания баз данных. Так как мы сопоставляется существующей базы данных, эти параметры будут игнорироваться в нашем приложении.*</span><span class="sxs-lookup"><span data-stu-id="a13dc-149">*You’ll notice some other settings in the configuration file too, these are default EF settings that tell Code First where to create databases. Since we are mapping to an existing database these setting will be ignored in our application.*</span></span>

### <a name="derived-context"></a><span data-ttu-id="a13dc-150">Производного контекста</span><span class="sxs-lookup"><span data-stu-id="a13dc-150">Derived Context</span></span>

<span data-ttu-id="a13dc-151">Объект **BloggingContext** класс будет добавлен в проект.</span><span class="sxs-lookup"><span data-stu-id="a13dc-151">A **BloggingContext** class has been added to the project.</span></span> <span data-ttu-id="a13dc-152">Контекст представляет собой сеанс с базой данных, позволяет запрашивать и сохранять данные.</span><span class="sxs-lookup"><span data-stu-id="a13dc-152">The context represents a session with the database, allowing us to query and save data.</span></span>
<span data-ttu-id="a13dc-153">Предоставляет контекст **DbSet&lt;TEntity&gt;**  для каждого типа в нашей модели.</span><span class="sxs-lookup"><span data-stu-id="a13dc-153">The context exposes a **DbSet&lt;TEntity&gt;** for each type in our model.</span></span> <span data-ttu-id="a13dc-154">Вы также заметите, что конструктор по умолчанию вызывает конструктор базового класса с помощью **имя =** синтаксис.</span><span class="sxs-lookup"><span data-stu-id="a13dc-154">You’ll also notice that the default constructor calls a base constructor using the **name=** syntax.</span></span> <span data-ttu-id="a13dc-155">Это сообщает Code First, что строка подключения, необходимо использовать в этом контексте должна быть загружена из файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="a13dc-155">This tells Code First that the connection string to use for this context should be loaded from the configuration file.</span></span>

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

<span data-ttu-id="a13dc-156">*Следует всегда использовать **имя =** синтаксиса при использовании строки подключения в файле конфигурации. Это гарантирует, что если строке подключения отсутствует затем Entity Framework будет генерировать вместо создания новой базы данных по соглашению.*</span><span class="sxs-lookup"><span data-stu-id="a13dc-156">*You should always use the **name=** syntax when you are using a connection string in the config file. This ensures that if the connection string is not present then Entity Framework will throw rather than creating a new database by convention.*</span></span>

### <a name="model-classes"></a><span data-ttu-id="a13dc-157">Классы модели</span><span class="sxs-lookup"><span data-stu-id="a13dc-157">Model classes</span></span>

<span data-ttu-id="a13dc-158">Наконец **блог** и **Post** класс также были добавлены в проект.</span><span class="sxs-lookup"><span data-stu-id="a13dc-158">Finally, a **Blog** and **Post** class have also been added to the project.</span></span> <span data-ttu-id="a13dc-159">Это доменных классов, входящих в состав нашей модели.</span><span class="sxs-lookup"><span data-stu-id="a13dc-159">These are the domain classes that make up our model.</span></span> <span data-ttu-id="a13dc-160">Вы увидите заметки данных применяются к классам для указания конфигурации где соглашения Code First бы не согласуются со структурой существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="a13dc-160">You'll see Data Annotations applied to the classes to specify configuration where the Code First conventions would not align with the structure of the existing database.</span></span> <span data-ttu-id="a13dc-161">Например, вы увидите **StringLength** заметки на **Blog.Name** и **Blog.Url** так, как они имеют длину не более **200** в базы данных (Code First по умолчанию разрешено использовать максимальное длину, поддерживаемую поставщиком баз данных - **nvarchar(max)** в SQL Server).</span><span class="sxs-lookup"><span data-stu-id="a13dc-161">For example, you'll see the **StringLength** annotation on **Blog.Name** and **Blog.Url** since they have a maximum length of **200** in the database (the Code First default is to use the maximun length supported by the database provider - **nvarchar(max)** in SQL Server).</span></span>

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

## <a name="4-reading--writing-data"></a><span data-ttu-id="a13dc-162">4. Чтение и запись данных</span><span class="sxs-lookup"><span data-stu-id="a13dc-162">4. Reading & Writing Data</span></span>

<span data-ttu-id="a13dc-163">Теперь, когда у нас есть модель, настала пора использовать ее для доступа к некоторые данные.</span><span class="sxs-lookup"><span data-stu-id="a13dc-163">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="a13dc-164">Реализуйте **Main** метод в **Program.cs** как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="a13dc-164">Implement the **Main** method in **Program.cs** as shown below.</span></span> <span data-ttu-id="a13dc-165">Этот код создает новый экземпляр класса наш контекст и затем используется, чтобы вставить новый **блог**.</span><span class="sxs-lookup"><span data-stu-id="a13dc-165">This code creates a new instance of our context and then uses it to insert a new **Blog**.</span></span> <span data-ttu-id="a13dc-166">Затем она использует запрос LINQ для извлечения всех **блоги** из базы данных, в алфавитном порядке по **Title**.</span><span class="sxs-lookup"><span data-stu-id="a13dc-166">Then it uses a LINQ query to retrieve all **Blogs** from the database ordered alphabetically by **Title**.</span></span>

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

<span data-ttu-id="a13dc-167">Теперь можно запустить приложение и протестировать его.</span><span class="sxs-lookup"><span data-stu-id="a13dc-167">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a><span data-ttu-id="a13dc-168">Что делать, если Моя база данных изменения?</span><span class="sxs-lookup"><span data-stu-id="a13dc-168">What if My Database Changes?</span></span>

<span data-ttu-id="a13dc-169">Code First для базы данных мастер предназначен для создания начальной точки набор классов, которые можно настроить и изменить.</span><span class="sxs-lookup"><span data-stu-id="a13dc-169">The Code First to Database wizard is designed to generate a starting point set of classes that you can then tweak and modify.</span></span> <span data-ttu-id="a13dc-170">При изменении схемы базы данных можно вручную изменить классы или выполнить другую реконструирования перезаписать классы.</span><span class="sxs-lookup"><span data-stu-id="a13dc-170">If your database schema changes you can either manually edit the classes or perform another reverse engineer to overwrite the classes.</span></span>

## <a name="using-code-first-migrations-to-an-existing-database"></a><span data-ttu-id="a13dc-171">С помощью Code First Migrations для существующей базы данных</span><span class="sxs-lookup"><span data-stu-id="a13dc-171">Using Code First Migrations to an Existing Database</span></span>

<span data-ttu-id="a13dc-172">Если вы хотите использовать Code First Migrations с существующей базы данных, см. в разделе [Code First Migrations для существующей базы данных](~/ef6/modeling/code-first/migrations/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="a13dc-172">If you want to use Code First Migrations with an existing database, see [Code First Migrations to an existing database](~/ef6/modeling/code-first/migrations/existing-database.md).</span></span>

## <a name="summary"></a><span data-ttu-id="a13dc-173">Сводка</span><span class="sxs-lookup"><span data-stu-id="a13dc-173">Summary</span></span>

<span data-ttu-id="a13dc-174">В этом пошаговом руководстве мы рассмотрели шаблона разработки Code First с помощью существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="a13dc-174">In this walkthrough we looked at Code First development using an existing database.</span></span> <span data-ttu-id="a13dc-175">Мы использовали средства платформы Entity Framework для Visual Studio, чтобы реконструировать набор классов, которые сопоставляются с базой данных и может использоваться для хранения и извлечения данных.</span><span class="sxs-lookup"><span data-stu-id="a13dc-175">We used the Entity Framework Tools for Visual Studio to reverse engineer a set of classes that mapped to the database and could be used to store and retrieve data.</span></span>
