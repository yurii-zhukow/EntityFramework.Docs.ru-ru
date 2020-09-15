---
title: Code First в новую базу данных — EF6
description: Code First в новую базу данных в Entity Framework 6
author: divega
ms.date: 10/23/2016
uid: ef6/modeling/code-first/workflows/new-database
ms.openlocfilehash: 96d76162383149c045178954196e1294331918a8
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90070371"
---
# <a name="code-first-to-a-new-database"></a><span data-ttu-id="3b110-103">Создание базы данных с помощью Code First</span><span class="sxs-lookup"><span data-stu-id="3b110-103">Code First to a New Database</span></span>
<span data-ttu-id="3b110-104">В этом видео и пошаговом руководстве представлены общие сведения о Code First разработке, предназначенной для новой базы данных.</span><span class="sxs-lookup"><span data-stu-id="3b110-104">This video and step-by-step walkthrough provide an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="3b110-105">Этот сценарий включает в себя целевую базу данных, которая не существует и Code First создаст, или пустую базу данных, в которую Code First добавит новые таблицы.</span><span class="sxs-lookup"><span data-stu-id="3b110-105">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="3b110-106">Code First позволяет определить модель с помощью \# классов C или VB.NET.</span><span class="sxs-lookup"><span data-stu-id="3b110-106">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="3b110-107">Дополнительную конфигурацию можно также выполнить с помощью атрибутов в классах и свойствах или с помощью API Fluent.</span><span class="sxs-lookup"><span data-stu-id="3b110-107">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="3b110-108">Просмотреть видео</span><span class="sxs-lookup"><span data-stu-id="3b110-108">Watch the video</span></span>
<span data-ttu-id="3b110-109">В этом видео представлены общие сведения о Code First разработке, предназначенной для новой базы данных.</span><span class="sxs-lookup"><span data-stu-id="3b110-109">This video provides an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="3b110-110">Этот сценарий включает в себя целевую базу данных, которая не существует и Code First создаст, или пустую базу данных, в которую Code First добавит новые таблицы.</span><span class="sxs-lookup"><span data-stu-id="3b110-110">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="3b110-111">Code First позволяет определить модель с помощью классов C# или VB.Net.</span><span class="sxs-lookup"><span data-stu-id="3b110-111">Code First allows you to define your model using C# or VB.Net classes.</span></span> <span data-ttu-id="3b110-112">Дополнительную конфигурацию можно также выполнить с помощью атрибутов в классах и свойствах или с помощью API Fluent.</span><span class="sxs-lookup"><span data-stu-id="3b110-112">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

<span data-ttu-id="3b110-113">**Представляет**: [Роуэн Миллер (Rowan Miller)](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="3b110-113">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="3b110-114">**Видео**: [WMV](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv)  |  [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v)  |  [WMV (ZIP)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span><span class="sxs-lookup"><span data-stu-id="3b110-114">**Video**: [WMV](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="3b110-115">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="3b110-115">Pre-Requisites</span></span>

<span data-ttu-id="3b110-116">Для выполнения инструкций этого пошагового руководства необходимо установить Visual Studio 2010 или Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="3b110-116">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="3b110-117">Если вы используете Visual Studio 2010, вам также потребуется установить [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .</span><span class="sxs-lookup"><span data-stu-id="3b110-117">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="3b110-118">1. Создание приложения</span><span class="sxs-lookup"><span data-stu-id="3b110-118">1. Create the Application</span></span>

<span data-ttu-id="3b110-119">Чтобы не усложнять, мы создадим простое консольное приложение, которое использует Code First для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="3b110-119">To keep things simple we’re going to build a basic console application that uses Code First to perform data access.</span></span>

-   <span data-ttu-id="3b110-120">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3b110-120">Open Visual Studio</span></span>
-   <span data-ttu-id="3b110-121">**Файл- &gt; создать &gt; проект...**</span><span class="sxs-lookup"><span data-stu-id="3b110-121">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="3b110-122">Выбор **окон** в меню слева и **консольное приложение**</span><span class="sxs-lookup"><span data-stu-id="3b110-122">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="3b110-123">Введите **кодефирстневдатабасесампле** в качестве имени</span><span class="sxs-lookup"><span data-stu-id="3b110-123">Enter **CodeFirstNewDatabaseSample** as the name</span></span>
-   <span data-ttu-id="3b110-124">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="3b110-124">Select **OK**</span></span>

## <a name="2-create-the-model"></a><span data-ttu-id="3b110-125">2. Создание модели</span><span class="sxs-lookup"><span data-stu-id="3b110-125">2. Create the Model</span></span>

<span data-ttu-id="3b110-126">Давайте определим очень простую модель с помощью классов.</span><span class="sxs-lookup"><span data-stu-id="3b110-126">Let’s define a very simple model using classes.</span></span> <span data-ttu-id="3b110-127">Мы просто определяем их в файле Program.cs, но в реальном мире вы разбиваете классы на отдельные файлы и, возможно, на отдельный проект.</span><span class="sxs-lookup"><span data-stu-id="3b110-127">We’re just defining them in the Program.cs file but in a real world application you would split your classes out into separate files and potentially a separate project.</span></span>

<span data-ttu-id="3b110-128">Под определением класса программы в Program.cs добавьте следующие два класса.</span><span class="sxs-lookup"><span data-stu-id="3b110-128">Below the Program class definition in Program.cs add the following two classes.</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public virtual Blog Blog { get; set; }
}
```

<span data-ttu-id="3b110-129">Вы заметите, что два свойства навигации (blog. posts и POST. blog) являются виртуальными.</span><span class="sxs-lookup"><span data-stu-id="3b110-129">You’ll notice that we’re making the two navigation properties (Blog.Posts and Post.Blog) virtual.</span></span> <span data-ttu-id="3b110-130">Это включает функцию отложенной загрузки Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3b110-130">This enables the Lazy Loading feature of Entity Framework.</span></span> <span data-ttu-id="3b110-131">Отложенная загрузка означает, что содержимое этих свойств будет автоматически загружаться из базы данных при попытке доступа к ним.</span><span class="sxs-lookup"><span data-stu-id="3b110-131">Lazy Loading means that the contents of these properties will be automatically loaded from the database when you try to access them.</span></span>

## <a name="3-create-a-context"></a><span data-ttu-id="3b110-132">3. Создание контекста</span><span class="sxs-lookup"><span data-stu-id="3b110-132">3. Create a Context</span></span>

<span data-ttu-id="3b110-133">Теперь пора определить производный контекст, который представляет сеанс с базой данных, что позволит нам запрашивать и сохранять данные.</span><span class="sxs-lookup"><span data-stu-id="3b110-133">Now it’s time to define a derived context, which represents a session with the database, allowing us to query and save data.</span></span> <span data-ttu-id="3b110-134">Мы определяем контекст, производный от System. Data. Entity. DbContext, и предоставляет типизированный DbSetный &lt; домик &gt; для каждого класса в нашей модели.</span><span class="sxs-lookup"><span data-stu-id="3b110-134">We define a context that derives from System.Data.Entity.DbContext and exposes a typed DbSet&lt;TEntity&gt; for each class in our model.</span></span>

<span data-ttu-id="3b110-135">Теперь мы начали использовать типы из Entity Framework, поэтому нам нужно добавить пакет NuGet EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="3b110-135">We’re now starting to use types from the Entity Framework so we need to add the EntityFramework NuGet package.</span></span>

-   <span data-ttu-id="3b110-136">**Проект — &gt; Управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="3b110-136">**Project –&gt; Manage NuGet Packages…**</span></span>
    <span data-ttu-id="3b110-137">Примечание. Если у вас нет **пакетов NuGet для управления...**</span><span class="sxs-lookup"><span data-stu-id="3b110-137">Note: If you don’t have the **Manage NuGet Packages…**</span></span> <span data-ttu-id="3b110-138">параметр следует установить [последнюю версию NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span><span class="sxs-lookup"><span data-stu-id="3b110-138">option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span></span>
-   <span data-ttu-id="3b110-139">Выбор вкладки "в **сети** "</span><span class="sxs-lookup"><span data-stu-id="3b110-139">Select the **Online** tab</span></span>
-   <span data-ttu-id="3b110-140">Выберите пакет **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="3b110-140">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="3b110-141">Щелкните **Установить**.</span><span class="sxs-lookup"><span data-stu-id="3b110-141">Click **Install**</span></span>

<span data-ttu-id="3b110-142">Добавьте инструкцию using для System. Data. Entity в верхнюю часть Program.cs.</span><span class="sxs-lookup"><span data-stu-id="3b110-142">Add a using statement for System.Data.Entity at the top of Program.cs.</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="3b110-143">Под классом POST в Program.cs добавьте следующий производный контекст.</span><span class="sxs-lookup"><span data-stu-id="3b110-143">Below the Post class in Program.cs add the following derived context.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

<span data-ttu-id="3b110-144">Ниже приведен полный список элементов, которые теперь должны содержаться в Program.cs.</span><span class="sxs-lookup"><span data-stu-id="3b110-144">Here is a complete listing of what Program.cs should now contain.</span></span>

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Entity;

namespace CodeFirstNewDatabaseSample
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }

    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }
}
```

<span data-ttu-id="3b110-145">Это весь код, необходимый для начала хранения и извлечения данных.</span><span class="sxs-lookup"><span data-stu-id="3b110-145">That is all the code we need to start storing and retrieving data.</span></span> <span data-ttu-id="3b110-146">Очевидно, что в фоновом режиме происходит довольно много времени, и мы рассмотрим его в действии.</span><span class="sxs-lookup"><span data-stu-id="3b110-146">Obviously there is quite a bit going on behind the scenes and we’ll take a look at that in a moment but first let’s see it in action.</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="3b110-147">4. чтение & запись данных</span><span class="sxs-lookup"><span data-stu-id="3b110-147">4. Reading & Writing Data</span></span>

<span data-ttu-id="3b110-148">Реализуйте метод Main в Program.cs, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="3b110-148">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="3b110-149">Этот код создает новый экземпляр нашего контекста, а затем использует его для вставки нового блога.</span><span class="sxs-lookup"><span data-stu-id="3b110-149">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="3b110-150">Затем он использует запрос LINQ для получения всех блогов из базы данных, упорядоченного в алфавитном порядке по названию.</span><span class="sxs-lookup"><span data-stu-id="3b110-150">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="3b110-151">Теперь можно запустить приложение и протестировать его.</span><span class="sxs-lookup"><span data-stu-id="3b110-151">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a><span data-ttu-id="3b110-152">Где мои данные?</span><span class="sxs-lookup"><span data-stu-id="3b110-152">Where’s My Data?</span></span>

<span data-ttu-id="3b110-153">По соглашению DbContext создала базу данных.</span><span class="sxs-lookup"><span data-stu-id="3b110-153">By convention DbContext has created a database for you.</span></span>

-   <span data-ttu-id="3b110-154">Если локальный экземпляр SQL Express доступен (устанавливается по умолчанию в Visual Studio 2010), Code First создал базу данных на этом экземпляре</span><span class="sxs-lookup"><span data-stu-id="3b110-154">If a local SQL Express instance is available (installed by default with Visual Studio 2010) then Code First has created the database on that instance</span></span>
-   <span data-ttu-id="3b110-155">Если экспресс-выпуск SQL Server недоступен, Code First попытается использовать [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (устанавливается по умолчанию в Visual Studio 2012).</span><span class="sxs-lookup"><span data-stu-id="3b110-155">If SQL Express isn’t available then Code First will try and use [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installed by default with Visual Studio 2012)</span></span>
-   <span data-ttu-id="3b110-156">База данных именуется после полного имени производного контекста, в нашем случае это **кодефирстневдатабасесампле. BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="3b110-156">The database is named after the fully qualified name of the derived context, in our case that is **CodeFirstNewDatabaseSample.BloggingContext**</span></span>

<span data-ttu-id="3b110-157">Это только соглашения по умолчанию, и существуют различные способы изменения базы данных, которую Code First использует. Дополнительные сведения см. в разделе **DbContext обнаружение модели и подключения к базе данных** .</span><span class="sxs-lookup"><span data-stu-id="3b110-157">These are just the default conventions and there are various ways to change the database that Code First uses, more information is available in the **How DbContext Discovers the Model and Database Connection** topic.</span></span>
<span data-ttu-id="3b110-158">Вы можете подключиться к этой базе данных с помощью обозреватель сервера в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3b110-158">You can connect to this database using Server Explorer in Visual Studio</span></span>

-   <span data-ttu-id="3b110-159">**Просмотр — &gt; Обозреватель сервера**</span><span class="sxs-lookup"><span data-stu-id="3b110-159">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="3b110-160">Щелкните правой кнопкой мыши **подключения к данным** и выберите **Добавить подключение...**</span><span class="sxs-lookup"><span data-stu-id="3b110-160">Right click on **Data Connections** and select **Add Connection…**</span></span>
-   <span data-ttu-id="3b110-161">Если вы не подключились к базе данных из обозреватель сервера, прежде чем нужно будет выбрать Microsoft SQL Server в качестве источника данных</span><span class="sxs-lookup"><span data-stu-id="3b110-161">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Выберите источник данных](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="3b110-163">Подключение к LocalDB или SQL Express в зависимости от того, какой из установленных служб</span><span class="sxs-lookup"><span data-stu-id="3b110-163">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>

<span data-ttu-id="3b110-164">Теперь можно проверить схему, созданную Code First.</span><span class="sxs-lookup"><span data-stu-id="3b110-164">We can now inspect the schema that Code First created.</span></span>

![Начальная схема](~/ef6/media/schemainitial.png)

<span data-ttu-id="3b110-166">DbContext выработала, какие классы следует включить в модель, взглянув на определяемые нами свойства DbSet.</span><span class="sxs-lookup"><span data-stu-id="3b110-166">DbContext worked out what classes to include in the model by looking at the DbSet properties that we defined.</span></span> <span data-ttu-id="3b110-167">Затем он использует набор соглашений по умолчанию Code First для определения имен таблиц и столбцов, определения типов данных, поиска первичных ключей и т. д.</span><span class="sxs-lookup"><span data-stu-id="3b110-167">It then uses the default set of Code First conventions to determine table and column names, determine data types, find primary keys, etc.</span></span> <span data-ttu-id="3b110-168">Далее в этом пошаговом руководстве мы рассмотрим, как можно переопределить эти соглашения.</span><span class="sxs-lookup"><span data-stu-id="3b110-168">Later in this walkthrough we’ll look at how you can override these conventions.</span></span>

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="3b110-169">5. Работа с изменениями модели</span><span class="sxs-lookup"><span data-stu-id="3b110-169">5. Dealing with Model Changes</span></span>

<span data-ttu-id="3b110-170">Пришло время внести некоторые изменения в нашу модель, при внесении этих изменений нам также потребуется обновить схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="3b110-170">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span> <span data-ttu-id="3b110-171">Для этого мы будем использовать функцию, именуемую Code First Migrations, или миграцию для коротких.</span><span class="sxs-lookup"><span data-stu-id="3b110-171">To do this we are going to use a feature called Code First Migrations, or Migrations for short.</span></span>

<span data-ttu-id="3b110-172">Миграция позволяет нам выполнить упорядоченный набор шагов, описывающих порядок обновления схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="3b110-172">Migrations allows us to have an ordered set of steps that describe how to upgrade (and downgrade) our database schema.</span></span> <span data-ttu-id="3b110-173">Каждый из этих шагов, называемый миграцией, содержит код, описывающий применяемые изменения.</span><span class="sxs-lookup"><span data-stu-id="3b110-173">Each of these steps, known as a migration, contains some code that describes the changes to be applied.</span></span> 

<span data-ttu-id="3b110-174">Первым шагом является включение Code First Migrations для нашего BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="3b110-174">The first step is to enable Code First Migrations for our BloggingContext.</span></span>

-   <span data-ttu-id="3b110-175">**Средства — &gt; Диспетчер пакетов библиотек — &gt; консоль диспетчера пакетов**</span><span class="sxs-lookup"><span data-stu-id="3b110-175">**Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
-   <span data-ttu-id="3b110-176">Запустите команду **Enable-Migrations** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="3b110-176">Run the **Enable-Migrations** command in Package Manager Console</span></span>
-   <span data-ttu-id="3b110-177">В наш проект добавлена новая папка миграции, содержащая два элемента:</span><span class="sxs-lookup"><span data-stu-id="3b110-177">A new Migrations folder has been added to our project that contains two items:</span></span>
    -   <span data-ttu-id="3b110-178">**Configuration.CS** — этот файл содержит параметры, которые будут использоваться миграцией для миграции BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="3b110-178">**Configuration.cs** – This file contains the settings that Migrations will use for migrating BloggingContext.</span></span> <span data-ttu-id="3b110-179">Нам не нужно ничего менять в этом пошаговом руководстве, но здесь можно указать начальные данные, зарегистрировать поставщики для других баз данных, изменить пространство имен, в котором создаются миграции, и т. д.</span><span class="sxs-lookup"><span data-stu-id="3b110-179">We don’t need to change anything for this walkthrough, but here is where you can specify seed data, register providers for other databases, changes the namespace that migrations are generated in etc.</span></span>
    -   <span data-ttu-id="3b110-180">\*\* &lt; timestamp &gt; \_ InitialCreate.CS\*\* — это первая миграция, которая представляет изменения, уже примененные к базе данных для того, чтобы сделать ее пустой базой данных, которая включает таблицы блогов и записей.</span><span class="sxs-lookup"><span data-stu-id="3b110-180">**&lt;timestamp&gt;\_InitialCreate.cs** – This is your first migration, it represents the changes that have already been applied to the database to take it from being an empty database to one that includes the Blogs and Posts tables.</span></span> <span data-ttu-id="3b110-181">Хотя мы позволяем Code First автоматически создавать эти таблицы для нас, теперь, когда мы участвуем в миграции, они были преобразованы в миграцию.</span><span class="sxs-lookup"><span data-stu-id="3b110-181">Although we let Code First automatically create these tables for us, now that we have opted in to Migrations they have been converted into a Migration.</span></span> <span data-ttu-id="3b110-182">Code First также записана в локальную базу данных, к которой уже применена эта миграция.</span><span class="sxs-lookup"><span data-stu-id="3b110-182">Code First has also recorded in our local database that this Migration has already been applied.</span></span> <span data-ttu-id="3b110-183">Метка времени для имени файла используется для упорядочения.</span><span class="sxs-lookup"><span data-stu-id="3b110-183">The timestamp on the filename is used for ordering purposes.</span></span>

    <span data-ttu-id="3b110-184">Теперь изменим нашу модель, добавив свойство URL в класс Blog:</span><span class="sxs-lookup"><span data-stu-id="3b110-184">Now let’s make a change to our model, add a Url property to the Blog class:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   <span data-ttu-id="3b110-185">Выполните команду **Add-Migration аддурл** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="3b110-185">Run the **Add-Migration AddUrl** command in Package Manager Console.</span></span>
    <span data-ttu-id="3b110-186">Команда Add-Migration проверяет наличие изменений с момента последней миграции и формирует новую миграцию с любыми найденными изменениями.</span><span class="sxs-lookup"><span data-stu-id="3b110-186">The Add-Migration command checks for changes since your last migration and scaffolds a new migration with any changes that are found.</span></span> <span data-ttu-id="3b110-187">Мы можем присвоить имя миграции. в этом случае вызывается миграция "Аддурл".</span><span class="sxs-lookup"><span data-stu-id="3b110-187">We can give migrations a name; in this case we are calling the migration ‘AddUrl’.</span></span>
    <span data-ttu-id="3b110-188">Шаблонный код говорит, что нам нужно добавить столбец URL-адреса, который может содержать строковые данные, в dbo. Таблица блогов.</span><span class="sxs-lookup"><span data-stu-id="3b110-188">The scaffolded code is saying that we need to add a Url column, that can hold string data, to the dbo.Blogs table.</span></span> <span data-ttu-id="3b110-189">При необходимости можно изменить шаблонный код, но это не является обязательным в этом случае.</span><span class="sxs-lookup"><span data-stu-id="3b110-189">If needed, we could edit the scaffolded code but that’s not required in this case.</span></span>

``` csharp
namespace CodeFirstNewDatabaseSample.Migrations
{
    using System;
    using System.Data.Entity.Migrations;

    public partial class AddUrl : DbMigration
    {
        public override void Up()
        {
            AddColumn("dbo.Blogs", "Url", c => c.String());
        }

        public override void Down()
        {
            DropColumn("dbo.Blogs", "Url");
        }
    }
}
```

-   <span data-ttu-id="3b110-190">Выполните команду **обновления базы данных** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="3b110-190">Run the **Update-Database** command in Package Manager Console.</span></span> <span data-ttu-id="3b110-191">Эта команда применит все незавершенные миграции к базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b110-191">This command will apply any pending migrations to the database.</span></span> <span data-ttu-id="3b110-192">Наша InitialCreate миграция уже применена, поэтому миграция будет просто применена к нашей новой Аддурл миграции.</span><span class="sxs-lookup"><span data-stu-id="3b110-192">Our InitialCreate migration has already been applied so migrations will just apply our new AddUrl migration.</span></span>
    <span data-ttu-id="3b110-193">Совет. при вызове Update-Database можно использовать параметр **– verbose** для просмотра SQL, выполняемого в базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b110-193">Tip: You can use the **–Verbose** switch when calling Update-Database to see the SQL that is being executed against the database.</span></span>

<span data-ttu-id="3b110-194">Новый столбец URL-адреса теперь добавляется в таблицу блоги базы данных:</span><span class="sxs-lookup"><span data-stu-id="3b110-194">The new Url column is now added to the Blogs table in the database:</span></span>

![Схема с URL-адресом](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a><span data-ttu-id="3b110-196">6. заметки к данным</span><span class="sxs-lookup"><span data-stu-id="3b110-196">6. Data Annotations</span></span>

<span data-ttu-id="3b110-197">До сих пор мы просто добавим EF к модели, используя соглашения по умолчанию, но бывают случаи, когда наши классы не соответствуют соглашениям, и нам нужно иметь возможность выполнить дальнейшую настройку.</span><span class="sxs-lookup"><span data-stu-id="3b110-197">So far we’ve just let EF discover the model using its default conventions, but there are going to be times when our classes don’t follow the conventions and we need to be able to perform further configuration.</span></span> <span data-ttu-id="3b110-198">Для этого есть два варианта. Мы рассмотрим примечания к данным в этом разделе, а затем API Fluent в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="3b110-198">There are two options for this; we’ll look at Data Annotations in this section and then the fluent API in the next section.</span></span>

-   <span data-ttu-id="3b110-199">Давайте добавим в нашу модель класс User.</span><span class="sxs-lookup"><span data-stu-id="3b110-199">Let’s add a User class to our model</span></span>

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="3b110-200">Также необходимо добавить набор в производный контекст</span><span class="sxs-lookup"><span data-stu-id="3b110-200">We also need to add a set to our derived context</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   <span data-ttu-id="3b110-201">Если мы попытались добавить миграцию, возникнет ошибка, говорящая, что*ключ EntityType "User" не определен. Определите ключ для этого типа EntityType ".*</span><span class="sxs-lookup"><span data-stu-id="3b110-201">If we tried to add a migration we’d get an error saying “*EntityType ‘User’ has no key defined. Define the key for this EntityType.”*</span></span> <span data-ttu-id="3b110-202">Поскольку EF не имеет возможности знать, что имя пользователя должно быть первичным ключом для пользователя.</span><span class="sxs-lookup"><span data-stu-id="3b110-202">because EF has no way of knowing that Username should be the primary key for User.</span></span>
-   <span data-ttu-id="3b110-203">Теперь мы будем использовать заметки к данным, поэтому нам нужно добавить инструкцию using в начало Program.cs</span><span class="sxs-lookup"><span data-stu-id="3b110-203">We’re going to use Data Annotations now so we need to add a using statement at the top of Program.cs</span></span>

```csharp
using System.ComponentModel.DataAnnotations;
```

-   <span data-ttu-id="3b110-204">Теперь добавьте аннотацию для свойства UserName, чтобы обозначить, что это первичный ключ</span><span class="sxs-lookup"><span data-stu-id="3b110-204">Now annotate the Username property to identify that it is the primary key</span></span>

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="3b110-205">Используйте команду **adduser для добавления и миграции** , чтобы сформировать шаблон миграции, чтобы применить эти изменения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b110-205">Use the **Add-Migration AddUser** command to scaffold a migration to apply these changes to the database</span></span>
-   <span data-ttu-id="3b110-206">Выполните команду **обновления базы данных** , чтобы применить новую миграцию к базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b110-206">Run the **Update-Database** command to apply the new migration to the database</span></span>

<span data-ttu-id="3b110-207">Новая таблица будет добавлена в базу данных:</span><span class="sxs-lookup"><span data-stu-id="3b110-207">The new table is now added to the database:</span></span>

![Схема с пользователями](~/ef6/media/schemawithusers.png)

<span data-ttu-id="3b110-209">Полный список заметок, поддерживаемых EF,:</span><span class="sxs-lookup"><span data-stu-id="3b110-209">The full list of annotations supported by EF is:</span></span>

-   [<span data-ttu-id="3b110-210">кэйаттрибуте</span><span class="sxs-lookup"><span data-stu-id="3b110-210">KeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [<span data-ttu-id="3b110-211">стрингленгсаттрибуте</span><span class="sxs-lookup"><span data-stu-id="3b110-211">StringLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [<span data-ttu-id="3b110-212">MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="3b110-212">MaxLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [<span data-ttu-id="3b110-213">конкурренцичеккаттрибуте</span><span class="sxs-lookup"><span data-stu-id="3b110-213">ConcurrencyCheckAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [<span data-ttu-id="3b110-214">RequiredAttribute</span><span class="sxs-lookup"><span data-stu-id="3b110-214">RequiredAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [<span data-ttu-id="3b110-215">тиместампаттрибуте</span><span class="sxs-lookup"><span data-stu-id="3b110-215">TimestampAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [<span data-ttu-id="3b110-216">комплекстипеаттрибуте</span><span class="sxs-lookup"><span data-stu-id="3b110-216">ComplexTypeAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [<span data-ttu-id="3b110-217">колумнаттрибуте</span><span class="sxs-lookup"><span data-stu-id="3b110-217">ColumnAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [<span data-ttu-id="3b110-218">TableAttribute</span><span class="sxs-lookup"><span data-stu-id="3b110-218">TableAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [<span data-ttu-id="3b110-219">инверсепропертяттрибуте</span><span class="sxs-lookup"><span data-stu-id="3b110-219">InversePropertyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [<span data-ttu-id="3b110-220">фореигнкэйаттрибуте</span><span class="sxs-lookup"><span data-stu-id="3b110-220">ForeignKeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [<span data-ttu-id="3b110-221">датабасеженератедаттрибуте</span><span class="sxs-lookup"><span data-stu-id="3b110-221">DatabaseGeneratedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [<span data-ttu-id="3b110-222">нотмаппедаттрибуте</span><span class="sxs-lookup"><span data-stu-id="3b110-222">NotMappedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a><span data-ttu-id="3b110-223">7. fluent API</span><span class="sxs-lookup"><span data-stu-id="3b110-223">7. Fluent API</span></span>

<span data-ttu-id="3b110-224">В предыдущем разделе мы рассматривали использование заметок к данным для дополнения или переопределения того, что было обнаружено соглашением.</span><span class="sxs-lookup"><span data-stu-id="3b110-224">In the previous section we looked at using Data Annotations to supplement or override what was detected by convention.</span></span> <span data-ttu-id="3b110-225">Другой способ настройки модели — через API-интерфейс Code First Fluent.</span><span class="sxs-lookup"><span data-stu-id="3b110-225">The other way to configure the model is via the Code First fluent API.</span></span>

<span data-ttu-id="3b110-226">Большую часть конфигурации модели можно выполнить с помощью простых заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="3b110-226">Most model configuration can be done using simple data annotations.</span></span> <span data-ttu-id="3b110-227">API-интерфейс Fluent — это более сложный способ определения конфигурации модели, охватывающий все возможности, которые могут делать заметки к данным в дополнение к более сложной конфигурации с заметками данных.</span><span class="sxs-lookup"><span data-stu-id="3b110-227">The fluent API is a more advanced way of specifying model configuration that covers everything that data annotations can do in addition to some more advanced configuration not possible with data annotations.</span></span> <span data-ttu-id="3b110-228">Заметки к данным и API Fluent можно использовать вместе.</span><span class="sxs-lookup"><span data-stu-id="3b110-228">Data annotations and the fluent API can be used together.</span></span>

<span data-ttu-id="3b110-229">Чтобы получить доступ к API Fluent, переопределите метод OnModelCreating в DbContext.</span><span class="sxs-lookup"><span data-stu-id="3b110-229">To access the fluent API you override the OnModelCreating method in DbContext.</span></span> <span data-ttu-id="3b110-230">Предположим, требуется переименовать столбец, в котором хранится User. DisplayName, в для отображаемого \_ имени.</span><span class="sxs-lookup"><span data-stu-id="3b110-230">Let’s say we wanted to rename the column that User.DisplayName is stored in to display\_name.</span></span>

-   <span data-ttu-id="3b110-231">Переопределите метод OnModelCreating в BloggingContext с помощью следующего кода</span><span class="sxs-lookup"><span data-stu-id="3b110-231">Override the OnModelCreating method on BloggingContext with the following code</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .Property(u => u.DisplayName)
            .HasColumnName("display_name");
    }
}
```

-   <span data-ttu-id="3b110-232">Используйте команду **Add-Migration чанжедисплайнаме** , чтобы сформировать шаблон миграции, чтобы применить эти изменения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b110-232">Use the **Add-Migration ChangeDisplayName** command to scaffold a migration to apply these changes to the database.</span></span>
-   <span data-ttu-id="3b110-233">Выполните команду **обновления базы данных** , чтобы применить новую миграцию к базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b110-233">Run the **Update-Database** command to apply the new migration to the database.</span></span>

<span data-ttu-id="3b110-234">Теперь столбец DisplayName переименован в отображаемое \_ Имя:</span><span class="sxs-lookup"><span data-stu-id="3b110-234">The DisplayName column is now renamed to display\_name:</span></span>

![Схема с переименованным отображаемым именем](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a><span data-ttu-id="3b110-236">Сводка</span><span class="sxs-lookup"><span data-stu-id="3b110-236">Summary</span></span>

<span data-ttu-id="3b110-237">В этом пошаговом руководстве мы рассматривали Code Firstную разработку с помощью новой базы данных.</span><span class="sxs-lookup"><span data-stu-id="3b110-237">In this walkthrough we looked at Code First development using a new database.</span></span> <span data-ttu-id="3b110-238">Мы определили модель, используя классы, которые затем использовали эту модель для создания базы данных, хранения и извлечения данных.</span><span class="sxs-lookup"><span data-stu-id="3b110-238">We defined a model using classes then used that model to create a database and store and retrieve data.</span></span> <span data-ttu-id="3b110-239">После создания базы данных мы использовали Code First Migrations, чтобы изменить схему по мере развития нашей модели.</span><span class="sxs-lookup"><span data-stu-id="3b110-239">Once the database was created we used Code First Migrations to change the schema as our model evolved.</span></span> <span data-ttu-id="3b110-240">Мы также увидели, как настроить модель с помощью заметок к данным и API Fluent.</span><span class="sxs-lookup"><span data-stu-id="3b110-240">We also saw how to configure a model using Data Annotations and the Fluent API.</span></span>
