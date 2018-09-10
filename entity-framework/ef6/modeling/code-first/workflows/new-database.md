---
title: Code First в новой базе данных - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: 8ed1bfbc3536acc0d83b9c8ecdd180aeb44eff83
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251054"
---
# <a name="code-first-to-a-new-database"></a><span data-ttu-id="a0a30-102">Code First в новой базе данных</span><span class="sxs-lookup"><span data-stu-id="a0a30-102">Code First to a New Database</span></span>
<span data-ttu-id="a0a30-103">В этом пошаговом руководстве видео и пошаговые познакомят вас с разработки Code First, предназначенные для новой базы данных.</span><span class="sxs-lookup"><span data-stu-id="a0a30-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="a0a30-104">Этот сценарий включает предназначенные для базы данных, которая не существует и Code First создаст пустую базу данных, Code First добавит новые таблицы для.</span><span class="sxs-lookup"><span data-stu-id="a0a30-104">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="a0a30-105">Во-первых, код позволяет определить модель с помощью c#\# или классам VB.Net.</span><span class="sxs-lookup"><span data-stu-id="a0a30-105">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="a0a30-106">Дополнительная настройка при необходимости выполняются с помощью атрибутов для классов и свойств или с помощью текучего API.</span><span class="sxs-lookup"><span data-stu-id="a0a30-106">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="a0a30-107">Просмотреть видео</span><span class="sxs-lookup"><span data-stu-id="a0a30-107">Watch the video</span></span>
<span data-ttu-id="a0a30-108">Этот видеоролик представляет собой введение разработки Code First, предназначенные для новой базы данных.</span><span class="sxs-lookup"><span data-stu-id="a0a30-108">This video provides an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="a0a30-109">Этот сценарий включает предназначенные для базы данных, которая не существует и Code First создаст пустую базу данных, Code First добавит новые таблицы для.</span><span class="sxs-lookup"><span data-stu-id="a0a30-109">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="a0a30-110">Во-первых, код позволяет определить модель с помощью классов C# или VB.Net.</span><span class="sxs-lookup"><span data-stu-id="a0a30-110">Code First allows you to define your model using C# or VB.Net classes.</span></span> <span data-ttu-id="a0a30-111">Дополнительная настройка при необходимости выполняются с помощью атрибутов для классов и свойств или с помощью текучего API.</span><span class="sxs-lookup"><span data-stu-id="a0a30-111">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

<span data-ttu-id="a0a30-112">**Представляет**: [Роуэн Миллер (Rowan Miller)](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="a0a30-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="a0a30-113">**Видео**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span><span class="sxs-lookup"><span data-stu-id="a0a30-113">**Video**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span></span>

# <a name="pre-requisites"></a><span data-ttu-id="a0a30-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="a0a30-114">Pre-Requisites</span></span>

<span data-ttu-id="a0a30-115">Для выполнения этого пошагового руководства необходимо иметь по крайней мере Visual Studio 2010 или Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="a0a30-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="a0a30-116">Если вы используете Visual Studio 2010, также необходимо будет иметь [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) установлен.</span><span class="sxs-lookup"><span data-stu-id="a0a30-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="a0a30-117">1. Создание приложения</span><span class="sxs-lookup"><span data-stu-id="a0a30-117">1. Create the Application</span></span>

<span data-ttu-id="a0a30-118">Для простоты мы собираемся создать простое консольное приложение, которое использует Code First для осуществления доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="a0a30-118">To keep things simple we’re going to build a basic console application that uses Code First to perform data access.</span></span>

-   <span data-ttu-id="a0a30-119">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a0a30-119">Open Visual Studio</span></span>
-   <span data-ttu-id="a0a30-120">**Файл —&gt; Новинка —&gt; проекта...**</span><span class="sxs-lookup"><span data-stu-id="a0a30-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="a0a30-121">Выберите **Windows** в меню слева и **консольного приложения**</span><span class="sxs-lookup"><span data-stu-id="a0a30-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="a0a30-122">Введите **CodeFirstNewDatabaseSample** как имя</span><span class="sxs-lookup"><span data-stu-id="a0a30-122">Enter **CodeFirstNewDatabaseSample** as the name</span></span>
-   <span data-ttu-id="a0a30-123">Нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="a0a30-123">Select **OK**</span></span>

## <a name="2-create-the-model"></a><span data-ttu-id="a0a30-124">2. Создание модели</span><span class="sxs-lookup"><span data-stu-id="a0a30-124">2. Create the Model</span></span>

<span data-ttu-id="a0a30-125">Давайте определим очень простую модель с помощью классов.</span><span class="sxs-lookup"><span data-stu-id="a0a30-125">Let’s define a very simple model using classes.</span></span> <span data-ttu-id="a0a30-126">Мы просто определяем их в файл Program.cs, но в случае реального приложения должно разбивать классы out в отдельных файлах и, возможно, отдельный проект.</span><span class="sxs-lookup"><span data-stu-id="a0a30-126">We’re just defining them in the Program.cs file but in a real world application you would split your classes out into separate files and potentially a separate project.</span></span>

<span data-ttu-id="a0a30-127">Ниже определении класса программы в файле Program.cs добавьте следующие два класса.</span><span class="sxs-lookup"><span data-stu-id="a0a30-127">Below the Program class definition in Program.cs add the following two classes.</span></span>

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

<span data-ttu-id="a0a30-128">Вы заметите, что мы создаем два свойства навигации (Blog.Posts и Post.Blog) виртуальный.</span><span class="sxs-lookup"><span data-stu-id="a0a30-128">You’ll notice that we’re making the two navigation properties (Blog.Posts and Post.Blog) virtual.</span></span> <span data-ttu-id="a0a30-129">Это позволяет функции отложенной загрузки в Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a0a30-129">This enables the Lazy Loading feature of Entity Framework.</span></span> <span data-ttu-id="a0a30-130">Отложенная загрузка означает, что содержимое этих свойств будет автоматически загружаться из базы данных при попытке доступа к ним.</span><span class="sxs-lookup"><span data-stu-id="a0a30-130">Lazy Loading means that the contents of these properties will be automatically loaded from the database when you try to access them.</span></span>

## <a name="3-create-a-context"></a><span data-ttu-id="a0a30-131">3. Создайте контекст</span><span class="sxs-lookup"><span data-stu-id="a0a30-131">3. Create a Context</span></span>

<span data-ttu-id="a0a30-132">Пришло время для определения производного контекста, который представляет сеанс подключения с базой данных, позволяет запрашивать и сохранять данные.</span><span class="sxs-lookup"><span data-stu-id="a0a30-132">Now it’s time to define a derived context, which represents a session with the database, allowing us to query and save data.</span></span> <span data-ttu-id="a0a30-133">Мы определяем контекст, который является производным от System.Data.Entity.DbContext и предоставляет типизированный DbSet&lt;TEntity&gt; для каждого класса в нашей модели.</span><span class="sxs-lookup"><span data-stu-id="a0a30-133">We define a context that derives from System.Data.Entity.DbContext and exposes a typed DbSet&lt;TEntity&gt; for each class in our model.</span></span>

<span data-ttu-id="a0a30-134">Теперь мы запускаем следует использовать типы из Entity Framework, поэтому нам нужно добавить пакет EntityFramework NuGet.</span><span class="sxs-lookup"><span data-stu-id="a0a30-134">We’re now starting to use types from the Entity Framework so we need to add the EntityFramework NuGet package.</span></span>

-   <span data-ttu-id="a0a30-135">**Проект —&gt; управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="a0a30-135">**Project –&gt; Manage NuGet Packages…**</span></span>
    <span data-ttu-id="a0a30-136">Примечание: Если у вас нет **управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="a0a30-136">Note: If you don’t have the **Manage NuGet Packages…**</span></span> <span data-ttu-id="a0a30-137">параметр, необходимо установить [последнюю версию NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span><span class="sxs-lookup"><span data-stu-id="a0a30-137">option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span></span>
-   <span data-ttu-id="a0a30-138">Выберите **Online** вкладку</span><span class="sxs-lookup"><span data-stu-id="a0a30-138">Select the **Online** tab</span></span>
-   <span data-ttu-id="a0a30-139">Выберите **EntityFramework** пакета</span><span class="sxs-lookup"><span data-stu-id="a0a30-139">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="a0a30-140">Нажмите кнопку **установки**</span><span class="sxs-lookup"><span data-stu-id="a0a30-140">Click **Install**</span></span>

<span data-ttu-id="a0a30-141">Добавить с помощью инструкции для System.Data.Entity в верхней части Program.cs.</span><span class="sxs-lookup"><span data-stu-id="a0a30-141">Add a using statement for System.Data.Entity at the top of Program.cs.</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="a0a30-142">Под Post класса в файле Program.cs добавьте следующие производного контекста.</span><span class="sxs-lookup"><span data-stu-id="a0a30-142">Below the Post class in Program.cs add the following derived context.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

<span data-ttu-id="a0a30-143">Ниже приведен полный листинг что еще теперь должно содержать файл Program.cs.</span><span class="sxs-lookup"><span data-stu-id="a0a30-143">Here is a complete listing of what Program.cs should now contain.</span></span>

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

<span data-ttu-id="a0a30-144">Это весь код, который необходимо запустить, хранения и извлечения данных.</span><span class="sxs-lookup"><span data-stu-id="a0a30-144">That is all the code we need to start storing and retrieving data.</span></span> <span data-ttu-id="a0a30-145">Очевидно, что есть довольно много происходит за кулисами, и мы рассмотрим краткий обзор, что в момент, однако сначала посмотрим на это в действии.</span><span class="sxs-lookup"><span data-stu-id="a0a30-145">Obviously there is quite a bit going on behind the scenes and we’ll take a look at that in a moment but first let’s see it in action.</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="a0a30-146">4. Чтение и запись данных</span><span class="sxs-lookup"><span data-stu-id="a0a30-146">4. Reading & Writing Data</span></span>

<span data-ttu-id="a0a30-147">Реализуйте метод Main в файле Program.cs, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="a0a30-147">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="a0a30-148">Этот код создает новый экземпляр класса наш контекст, а затем использует его для вставки нового блога.</span><span class="sxs-lookup"><span data-stu-id="a0a30-148">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="a0a30-149">Затем он использует запрос LINQ для извлечения из базы данных, представлены в алфавитном порядке по названию все блоги.</span><span class="sxs-lookup"><span data-stu-id="a0a30-149">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="a0a30-150">Теперь можно запустить приложение и протестировать его.</span><span class="sxs-lookup"><span data-stu-id="a0a30-150">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a><span data-ttu-id="a0a30-151">Где мои данные?</span><span class="sxs-lookup"><span data-stu-id="a0a30-151">Where’s My Data?</span></span>

<span data-ttu-id="a0a30-152">По соглашению DbContext автоматически создала базу данных.</span><span class="sxs-lookup"><span data-stu-id="a0a30-152">By convention DbContext has created a database for you.</span></span>

-   <span data-ttu-id="a0a30-153">Если локальный экземпляр SQL Express (устанавливается по умолчанию вместе с Visual Studio 2010) затем Code First базы данных на этом экземпляре создается</span><span class="sxs-lookup"><span data-stu-id="a0a30-153">If a local SQL Express instance is available (installed by default with Visual Studio 2010) then Code First has created the database on that instance</span></span>
-   <span data-ttu-id="a0a30-154">Если SQL Express недоступен, то Code First будет попробуйте и использовать [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (устанавливается по умолчанию с помощью Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="a0a30-154">If SQL Express isn’t available then Code First will try and use [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installed by default with Visual Studio 2012)</span></span>
-   <span data-ttu-id="a0a30-155">База данных называется после полное имя производного контекста, в нашем случае это **CodeFirstNewDatabaseSample.BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="a0a30-155">The database is named after the fully qualified name of the derived context, in our case that is **CodeFirstNewDatabaseSample.BloggingContext**</span></span>

<span data-ttu-id="a0a30-156">Это просто соглашения по умолчанию существуют различные способы изменения базы данных, который использует Code First, Дополнительные сведения можно найти в **как DbContext обнаруживает модель и подключение к базе данных** раздела.</span><span class="sxs-lookup"><span data-stu-id="a0a30-156">These are just the default conventions and there are various ways to change the database that Code First uses, more information is available in the **How DbContext Discovers the Model and Database Connection** topic.</span></span>
<span data-ttu-id="a0a30-157">Можно подключиться к этой базе данных, с помощью обозревателя серверов в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a0a30-157">You can connect to this database using Server Explorer in Visual Studio</span></span>

-   <span data-ttu-id="a0a30-158">**Представление —&gt; обозревателя серверов**</span><span class="sxs-lookup"><span data-stu-id="a0a30-158">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="a0a30-159">Щелкните правой кнопкой мыши **подключения к данным** и выберите **добавить соединение...**</span><span class="sxs-lookup"><span data-stu-id="a0a30-159">Right click on **Data Connections** and select **Add Connection…**</span></span>
-   <span data-ttu-id="a0a30-160">Если вы не подключились к базе данных с помощью обозревателя сервера прежде, чем вам потребуется выбрать в качестве источника данных Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="a0a30-160">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Выберите источник данных](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="a0a30-162">Подключение к LocalDB или SQL Express, в зависимости от того, какой из них установки</span><span class="sxs-lookup"><span data-stu-id="a0a30-162">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>

<span data-ttu-id="a0a30-163">Теперь можно проверить, Code First создает схему.</span><span class="sxs-lookup"><span data-stu-id="a0a30-163">We can now inspect the schema that Code First created.</span></span>

![Начальный схемы](~/ef6/media/schemainitial.png)

<span data-ttu-id="a0a30-165">Классы для включения в модель, просмотрев свойства DbSet, которые мы определили работы DbContext.</span><span class="sxs-lookup"><span data-stu-id="a0a30-165">DbContext worked out what classes to include in the model by looking at the DbSet properties that we defined.</span></span> <span data-ttu-id="a0a30-166">Затем набор соглашения Code First, по умолчанию используется для определения имен таблиц и столбцов, определения типов данных, найти первичные ключи и т. д.</span><span class="sxs-lookup"><span data-stu-id="a0a30-166">It then uses the default set of Code First conventions to determine table and column names, determine data types, find primary keys, etc.</span></span> <span data-ttu-id="a0a30-167">Далее в этом пошаговом руководстве мы рассмотрим, как можно переопределить эти соглашения.</span><span class="sxs-lookup"><span data-stu-id="a0a30-167">Later in this walkthrough we’ll look at how you can override these conventions.</span></span>

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="a0a30-168">5. Изменения модели</span><span class="sxs-lookup"><span data-stu-id="a0a30-168">5. Dealing with Model Changes</span></span>

<span data-ttu-id="a0a30-169">Теперь пора внести некоторые изменения в нашей модели, когда мы внести эти изменения, необходимо также обновить схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="a0a30-169">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span> <span data-ttu-id="a0a30-170">Для этого мы собираемся использовать средство Code First Migrations и миграции для краткости.</span><span class="sxs-lookup"><span data-stu-id="a0a30-170">To do this we are going to use a feature called Code First Migrations, or Migrations for short.</span></span>

<span data-ttu-id="a0a30-171">Миграция позволяет нам упорядоченный набор шагов, которые описывают обновление (или понижение) нашей схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="a0a30-171">Migrations allows us to have an ordered set of steps that describe how to upgrade (and downgrade) our database schema.</span></span> <span data-ttu-id="a0a30-172">Каждая из этих действий, известных как миграцию, содержит код, который описывает изменения, применяемые.</span><span class="sxs-lookup"><span data-stu-id="a0a30-172">Each of these steps, known as a migration, contains some code that describes the changes to be applied.</span></span> 

<span data-ttu-id="a0a30-173">Первым шагом является включение Code First Migrations для наших BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="a0a30-173">The first step is to enable Code First Migrations for our BloggingContext.</span></span>

-   <span data-ttu-id="a0a30-174">**Средства —&gt; диспетчер пакетов библиотеки -&gt; консоль диспетчера пакетов**</span><span class="sxs-lookup"><span data-stu-id="a0a30-174">**Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
-   <span data-ttu-id="a0a30-175">Запустите команду **Enable-Migrations** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="a0a30-175">Run the **Enable-Migrations** command in Package Manager Console</span></span>
-   <span data-ttu-id="a0a30-176">Новая папка миграции был добавлен в наш проект, содержащий два элемента:</span><span class="sxs-lookup"><span data-stu-id="a0a30-176">A new Migrations folder has been added to our project that contains two items:</span></span>
    -   <span data-ttu-id="a0a30-177">**Configuration.cs** — этот файл содержит параметры, которые будут использовать миграции для переноса BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="a0a30-177">**Configuration.cs** – This file contains the settings that Migrations will use for migrating BloggingContext.</span></span> <span data-ttu-id="a0a30-178">Нет необходимости изменять что-либо в этом пошаговом руководстве, но Вот где можно указать начальное значение данных, поставщики register для других баз данных, изменяет пространство имен что миграции создаются в и т.д.</span><span class="sxs-lookup"><span data-stu-id="a0a30-178">We don’t need to change anything for this walkthrough, but here is where you can specify seed data, register providers for other databases, changes the namespace that migrations are generated in etc.</span></span>
    -   <span data-ttu-id="a0a30-179">**&lt;Метка времени&gt;\_InitialCreate.cs** – это первую миграцию, он представляет изменения, которые уже были применены к базы данных, пользоваться не пустую базу данных, который включает в себя блогов и записей таблицы .</span><span class="sxs-lookup"><span data-stu-id="a0a30-179">**&lt;timestamp&gt;\_InitialCreate.cs** – This is your first migration, it represents the changes that have already been applied to the database to take it from being an empty database to one that includes the Blogs and Posts tables.</span></span> <span data-ttu-id="a0a30-180">Несмотря на то, что мы позволим Code First автоматически создавать эти таблицы для нас, раз уж мы выбрали для миграций, они были преобразованы в миграции.</span><span class="sxs-lookup"><span data-stu-id="a0a30-180">Although we let Code First automatically create these tables for us, now that we have opted in to Migrations they have been converted into a Migration.</span></span> <span data-ttu-id="a0a30-181">Код сначала также записывается в нашей локальной базы данных, что уже был применен такой миграции.</span><span class="sxs-lookup"><span data-stu-id="a0a30-181">Code First has also recorded in our local database that this Migration has already been applied.</span></span> <span data-ttu-id="a0a30-182">Метка времени в имени файла используется для сортировки целей.</span><span class="sxs-lookup"><span data-stu-id="a0a30-182">The timestamp on the filename is used for ordering purposes.</span></span>

    <span data-ttu-id="a0a30-183">Теперь давайте внести изменения в нашей модели, добавьте в класс блог свойства URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="a0a30-183">Now let’s make a change to our model, add a Url property to the Blog class:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   <span data-ttu-id="a0a30-184">Запустите **AddUrl Add-Migration** команду в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="a0a30-184">Run the **Add-Migration AddUrl** command in Package Manager Console.</span></span>
    <span data-ttu-id="a0a30-185">Команда Add-Migration проверяет наличие изменений с момента последнего перехода и формирует шаблоны новой миграции с любыми изменениями, которые находятся.</span><span class="sxs-lookup"><span data-stu-id="a0a30-185">The Add-Migration command checks for changes since your last migration and scaffolds a new migration with any changes that are found.</span></span> <span data-ttu-id="a0a30-186">Мы можем дать миграций имени; в этом случае мы называем миграции «AddUrl».</span><span class="sxs-lookup"><span data-stu-id="a0a30-186">We can give migrations a name; in this case we are calling the migration ‘AddUrl’.</span></span>
    <span data-ttu-id="a0a30-187">Шаблонный код говорит, что нам нужно добавить столбец URL-адрес, который может содержать строковые данные, dbo. Таблица блоги.</span><span class="sxs-lookup"><span data-stu-id="a0a30-187">The scaffolded code is saying that we need to add a Url column, that can hold string data, to the dbo.Blogs table.</span></span> <span data-ttu-id="a0a30-188">При необходимости, мы может изменить сформированный код, но в данном случае это не обязательно.</span><span class="sxs-lookup"><span data-stu-id="a0a30-188">If needed, we could edit the scaffolded code but that’s not required in this case.</span></span>

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

-   <span data-ttu-id="a0a30-189">Запустите **Update-Database** команду в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="a0a30-189">Run the **Update-Database** command in Package Manager Console.</span></span> <span data-ttu-id="a0a30-190">Эта команда применит все незавершенные миграции к базе данных.</span><span class="sxs-lookup"><span data-stu-id="a0a30-190">This command will apply any pending migrations to the database.</span></span> <span data-ttu-id="a0a30-191">Миграция это InitialCreate уже был применен, поэтому миграция будет просто применить нашей новой миграции AddUrl.</span><span class="sxs-lookup"><span data-stu-id="a0a30-191">Our InitialCreate migration has already been applied so migrations will just apply our new AddUrl migration.</span></span>
    <span data-ttu-id="a0a30-192">Совет: Можно использовать **– Verbose** при вызове Update-Database, чтобы увидеть SQL, который выполняется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a0a30-192">Tip: You can use the **–Verbose** switch when calling Update-Database to see the SQL that is being executed against the database.</span></span>

<span data-ttu-id="a0a30-193">Теперь новый столбец URL-адрес добавляется в блоги таблицу в базе данных:</span><span class="sxs-lookup"><span data-stu-id="a0a30-193">The new Url column is now added to the Blogs table in the database:</span></span>

![Схема с URL-адрес](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a><span data-ttu-id="a0a30-195">6. Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="a0a30-195">6. Data Annotations</span></span>

<span data-ttu-id="a0a30-196">Пока мы просто позволили EF обнаруживать модели с помощью его соглашения по умолчанию, но будут ли раз, когда наших классов не соответствуют соглашениям об, и нам нужно иметь возможность дальнейшей настройки.</span><span class="sxs-lookup"><span data-stu-id="a0a30-196">So far we’ve just let EF discover the model using its default conventions, but there are going to be times when our classes don’t follow the conventions and we need to be able to perform further configuration.</span></span> <span data-ttu-id="a0a30-197">Существует два варианта. Мы рассмотрим заметок к данным в этом разделе, а затем текучего API в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="a0a30-197">There are two options for this; we’ll look at Data Annotations in this section and then the fluent API in the next section.</span></span>

-   <span data-ttu-id="a0a30-198">Добавим к нашей модели пользовательского класса</span><span class="sxs-lookup"><span data-stu-id="a0a30-198">Let’s add a User class to our model</span></span>

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="a0a30-199">Необходимо также добавить набор в нашей производного контекста</span><span class="sxs-lookup"><span data-stu-id="a0a30-199">We also need to add a set to our derived context</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   <span data-ttu-id="a0a30-200">Если мы попытались добавить миграцию, то получили бы ошибка "*EntityType «User» имеет ключ не определен. Определите ключ для этого типа EntityType.»*</span><span class="sxs-lookup"><span data-stu-id="a0a30-200">If we tried to add a migration we’d get an error saying “*EntityType ‘User’ has no key defined. Define the key for this EntityType.”*</span></span> <span data-ttu-id="a0a30-201">так как EF не имеет возможности узнать, что имя пользователя должно быть первичный ключ для пользователя.</span><span class="sxs-lookup"><span data-stu-id="a0a30-201">because EF has no way of knowing that Username should be the primary key for User.</span></span>
-   <span data-ttu-id="a0a30-202">Мы собираемся теперь использовать заметки к данным, поэтому нам нужно добавить с помощью оператора в верхней части Program.cs</span><span class="sxs-lookup"><span data-stu-id="a0a30-202">We’re going to use Data Annotations now so we need to add a using statement at the top of Program.cs</span></span>

```
using System.ComponentModel.DataAnnotations;
```

-   <span data-ttu-id="a0a30-203">Теперь Добавление заметок свойство Username, чтобы определить, что он является первичным ключом</span><span class="sxs-lookup"><span data-stu-id="a0a30-203">Now annotate the Username property to identify that it is the primary key</span></span>

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="a0a30-204">Используйте **AddUser Add-Migration** команду, чтобы сформировать шаблон миграции для применения этих изменений в базу данных</span><span class="sxs-lookup"><span data-stu-id="a0a30-204">Use the **Add-Migration AddUser** command to scaffold a migration to apply these changes to the database</span></span>
-   <span data-ttu-id="a0a30-205">Запустите **Update-Database** команду, чтобы применить созданную миграцию к базе данных</span><span class="sxs-lookup"><span data-stu-id="a0a30-205">Run the **Update-Database** command to apply the new migration to the database</span></span>

<span data-ttu-id="a0a30-206">Теперь новая таблица добавляется в базу данных:</span><span class="sxs-lookup"><span data-stu-id="a0a30-206">The new table is now added to the database:</span></span>

![Схема с пользователями](~/ef6/media/schemawithusers.png)

<span data-ttu-id="a0a30-208">Приведен полный список заметок, поддерживаемых EF.</span><span class="sxs-lookup"><span data-stu-id="a0a30-208">The full list of annotations supported by EF is:</span></span>

-   [<span data-ttu-id="a0a30-209">KeyAttribute</span><span class="sxs-lookup"><span data-stu-id="a0a30-209">KeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [<span data-ttu-id="a0a30-210">StringLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="a0a30-210">StringLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [<span data-ttu-id="a0a30-211">MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="a0a30-211">MaxLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [<span data-ttu-id="a0a30-212">ConcurrencyCheckAttribute</span><span class="sxs-lookup"><span data-stu-id="a0a30-212">ConcurrencyCheckAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [<span data-ttu-id="a0a30-213">RequiredAttribute</span><span class="sxs-lookup"><span data-stu-id="a0a30-213">RequiredAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [<span data-ttu-id="a0a30-214">TimestampAttribute</span><span class="sxs-lookup"><span data-stu-id="a0a30-214">TimestampAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [<span data-ttu-id="a0a30-215">ComplexTypeAttribute</span><span class="sxs-lookup"><span data-stu-id="a0a30-215">ComplexTypeAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [<span data-ttu-id="a0a30-216">ColumnAttribute</span><span class="sxs-lookup"><span data-stu-id="a0a30-216">ColumnAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [<span data-ttu-id="a0a30-217">TableAttribute</span><span class="sxs-lookup"><span data-stu-id="a0a30-217">TableAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [<span data-ttu-id="a0a30-218">InversePropertyAttribute</span><span class="sxs-lookup"><span data-stu-id="a0a30-218">InversePropertyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [<span data-ttu-id="a0a30-219">ForeignKeyAttribute</span><span class="sxs-lookup"><span data-stu-id="a0a30-219">ForeignKeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [<span data-ttu-id="a0a30-220">DatabaseGeneratedAttribute</span><span class="sxs-lookup"><span data-stu-id="a0a30-220">DatabaseGeneratedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [<span data-ttu-id="a0a30-221">NotMappedAttribute</span><span class="sxs-lookup"><span data-stu-id="a0a30-221">NotMappedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a><span data-ttu-id="a0a30-222">7. Текучий API</span><span class="sxs-lookup"><span data-stu-id="a0a30-222">7. Fluent API</span></span>

<span data-ttu-id="a0a30-223">В предыдущем разделе мы рассмотрели использование заметок к данным для дополнения или переопределить, что была обнаружена по соглашению.</span><span class="sxs-lookup"><span data-stu-id="a0a30-223">In the previous section we looked at using Data Annotations to supplement or override what was detected by convention.</span></span> <span data-ttu-id="a0a30-224">Другой способ настройки модели — с помощью Code First fluent API.</span><span class="sxs-lookup"><span data-stu-id="a0a30-224">The other way to configure the model is via the Code First fluent API.</span></span>

<span data-ttu-id="a0a30-225">Большинство конфигураций модели можно сделать с помощью простых данных заметок.</span><span class="sxs-lookup"><span data-stu-id="a0a30-225">Most model configuration can be done using simple data annotations.</span></span> <span data-ttu-id="a0a30-226">Текучий API — это более сложных способ указания модели конфигурации, который охватывает все, что заметки к данным можно делать в дополнение к некоторые более сложные конфигурации, недоступные в заметки к данным.</span><span class="sxs-lookup"><span data-stu-id="a0a30-226">The fluent API is a more advanced way of specifying model configuration that covers everything that data annotations can do in addition to some more advanced configuration not possible with data annotations.</span></span> <span data-ttu-id="a0a30-227">Заметки к данным и текучий API могут использоваться совместно.</span><span class="sxs-lookup"><span data-stu-id="a0a30-227">Data annotations and the fluent API can be used together.</span></span>

<span data-ttu-id="a0a30-228">Для доступа к fluent API переопределить метод OnModelCreating в DbContext.</span><span class="sxs-lookup"><span data-stu-id="a0a30-228">To access the fluent API you override the OnModelCreating method in DbContext.</span></span> <span data-ttu-id="a0a30-229">Предположим, мы хотели бы переименовать столбец, который User.DisplayName хранится в для отображения\_имя.</span><span class="sxs-lookup"><span data-stu-id="a0a30-229">Let’s say we wanted to rename the column that User.DisplayName is stored in to display\_name.</span></span>

-   <span data-ttu-id="a0a30-230">Переопределите метод OnModelCreating на BloggingContext следующим кодом</span><span class="sxs-lookup"><span data-stu-id="a0a30-230">Override the OnModelCreating method on BloggingContext with the following code</span></span>

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

-   <span data-ttu-id="a0a30-231">Используйте **ChangeDisplayName Add-Migration** команду, чтобы сформировать шаблон миграции для применения этих изменений в базу данных.</span><span class="sxs-lookup"><span data-stu-id="a0a30-231">Use the **Add-Migration ChangeDisplayName** command to scaffold a migration to apply these changes to the database.</span></span>
-   <span data-ttu-id="a0a30-232">Запустите **Update-Database** команду, чтобы применить созданную миграцию к базе данных.</span><span class="sxs-lookup"><span data-stu-id="a0a30-232">Run the **Update-Database** command to apply the new migration to the database.</span></span>

<span data-ttu-id="a0a30-233">Теперь в столбце DisplayName переименовывается для отображения\_имя:</span><span class="sxs-lookup"><span data-stu-id="a0a30-233">The DisplayName column is now renamed to display\_name:</span></span>

![Схема с отображаемым именем переименован](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a><span data-ttu-id="a0a30-235">Сводка</span><span class="sxs-lookup"><span data-stu-id="a0a30-235">Summary</span></span>

<span data-ttu-id="a0a30-236">В этом пошаговом руководстве мы рассмотрели шаблона разработки Code First с помощью новой базы данных.</span><span class="sxs-lookup"><span data-stu-id="a0a30-236">In this walkthrough we looked at Code First development using a new database.</span></span> <span data-ttu-id="a0a30-237">Мы определена модель, с помощью классов, затем использовать эту модель для создания базы данных и хранения и извлечения данных.</span><span class="sxs-lookup"><span data-stu-id="a0a30-237">We defined a model using classes then used that model to create a database and store and retrieve data.</span></span> <span data-ttu-id="a0a30-238">После создания базы данных мы использовали Code First Migrations для изменения схемы, как наша модель развитие.</span><span class="sxs-lookup"><span data-stu-id="a0a30-238">Once the database was created we used Code First Migrations to change the schema as our model evolved.</span></span> <span data-ttu-id="a0a30-239">Мы также увидели, как настроить модель с помощью заметок к данным и Fluent API.</span><span class="sxs-lookup"><span data-stu-id="a0a30-239">We also saw how to configure a model using Data Annotations and the Fluent API.</span></span>
