---
title: EF6 Database First.
author: divega
ms.date: 2016-10-23
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: 93ae5729e487ed9be3972ac78d599dbea19ed458
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251093"
---
# <a name="database-first"></a><span data-ttu-id="793bd-102">Сначала базы данных</span><span class="sxs-lookup"><span data-stu-id="793bd-102">Database First</span></span>
<span data-ttu-id="793bd-103">В этом пошаговом руководстве видео и пошаговые познакомят вас с первой базы данных разработки, использующий Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="793bd-103">This video and step-by-step walkthrough provide an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="793bd-104">Во-первых, базы данных дает возможность Реконструировать модель из существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="793bd-104">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="793bd-105">Модель хранится в EDMX-файла (расширение EDMX) и их можно просмотреть и изменить в конструкторе Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="793bd-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="793bd-106">Классы, которые взаимодействуют с в приложении автоматически создаются из файла EDMX.</span><span class="sxs-lookup"><span data-stu-id="793bd-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="793bd-107">Просмотреть видео</span><span class="sxs-lookup"><span data-stu-id="793bd-107">Watch the video</span></span>
<span data-ttu-id="793bd-108">В этом видео предоставляет общие сведения о первой базы данных разработки, использующий Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="793bd-108">This video provides an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="793bd-109">Во-первых, базы данных дает возможность Реконструировать модель из существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="793bd-109">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="793bd-110">Модель хранится в EDMX-файла (расширение EDMX) и их можно просмотреть и изменить в конструкторе Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="793bd-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="793bd-111">Классы, которые взаимодействуют с в приложении автоматически создаются из файла EDMX.</span><span class="sxs-lookup"><span data-stu-id="793bd-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="793bd-112">**Представляет**: [Роуэн Миллер (Rowan Miller)](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="793bd-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="793bd-113">**Видео**: [WMV](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="793bd-113">**Video**: [WMV](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="793bd-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="793bd-114">Pre-Requisites</span></span>

<span data-ttu-id="793bd-115">Для выполнения этого пошагового руководства необходимо иметь по крайней мере Visual Studio 2010 или Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="793bd-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="793bd-116">Если вы используете Visual Studio 2010, также необходимо будет иметь [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) установлен.</span><span class="sxs-lookup"><span data-stu-id="793bd-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

 

## <a name="1-create-an-existing-database"></a><span data-ttu-id="793bd-117">1. Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="793bd-117">1. Create an Existing Database</span></span>

<span data-ttu-id="793bd-118">Обычно при ориентировании существующей базы данных, он будет уже создан, но для этого пошагового руководства необходимо создать базу данных для доступа к.</span><span class="sxs-lookup"><span data-stu-id="793bd-118">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="793bd-119">Сервер базы данных, который устанавливается вместе с Visual Studio отличается в зависимости от версии Visual Studio, вы установили:</span><span class="sxs-lookup"><span data-stu-id="793bd-119">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="793bd-120">Если вы используете Visual Studio 2010 вы создадите базу данных SQL Express.</span><span class="sxs-lookup"><span data-stu-id="793bd-120">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="793bd-121">Если вы используете Visual Studio 2012, а затем вы создадите [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) базы данных.</span><span class="sxs-lookup"><span data-stu-id="793bd-121">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

 

<span data-ttu-id="793bd-122">Перейдем дальше и создать базу данных.</span><span class="sxs-lookup"><span data-stu-id="793bd-122">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="793bd-123">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="793bd-123">Open Visual Studio</span></span>
-   <span data-ttu-id="793bd-124">**Представление —&gt; обозревателя серверов**</span><span class="sxs-lookup"><span data-stu-id="793bd-124">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="793bd-125">Щелкните правой кнопкой мыши **подключения к данным -&gt; добавить соединение...**</span><span class="sxs-lookup"><span data-stu-id="793bd-125">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="793bd-126">Если вы не подключились к базе данных с помощью обозревателя сервера прежде, чем вам потребуется выбрать в качестве источника данных Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="793bd-126">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Выберите источник данных](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="793bd-128">Подключение к LocalDB или SQL Express, в зависимости от того, какой из них вы установили и введите **DatabaseFirst.Blogging** имя базы данных</span><span class="sxs-lookup"><span data-stu-id="793bd-128">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **DatabaseFirst.Blogging** as the database name</span></span>

    ![Подключение SQL Express DF](~/ef6/media/sqlexpressconnectiondf.png)

    ![Подключение LocalDB DF](~/ef6/media/localdbconnectiondf.png)

-   <span data-ttu-id="793bd-131">Выберите **ОК** и вам нужно будет Если вы хотите создать новую базу данных, выберите **Да**</span><span class="sxs-lookup"><span data-stu-id="793bd-131">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Создание диалогового окна базы данных](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="793bd-133">Новой базы данных будут отображаться в обозревателе сервера щелкните его правой кнопкой мыши и выберите **новый запрос**</span><span class="sxs-lookup"><span data-stu-id="793bd-133">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="793bd-134">Скопируйте следующий запрос SQL в новый запрос, а затем щелкните правой кнопкой мыши запрос и выберите **Execute**</span><span class="sxs-lookup"><span data-stu-id="793bd-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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
```

## <a name="2-create-the-application"></a><span data-ttu-id="793bd-135">2. Создание приложения</span><span class="sxs-lookup"><span data-stu-id="793bd-135">2. Create the Application</span></span>

<span data-ttu-id="793bd-136">Для простоты мы создадим простое консольное приложение, использующее первой базы данных для выполнения доступа к данным:</span><span class="sxs-lookup"><span data-stu-id="793bd-136">To keep things simple we’re going to build a basic console application that uses the Database First to perform data access:</span></span>

-   <span data-ttu-id="793bd-137">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="793bd-137">Open Visual Studio</span></span>
-   <span data-ttu-id="793bd-138">**Файл —&gt; Новинка —&gt; проекта...**</span><span class="sxs-lookup"><span data-stu-id="793bd-138">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="793bd-139">Выберите **Windows** в меню слева и **консольного приложения**</span><span class="sxs-lookup"><span data-stu-id="793bd-139">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="793bd-140">Введите **DatabaseFirstSample** как имя</span><span class="sxs-lookup"><span data-stu-id="793bd-140">Enter **DatabaseFirstSample** as the name</span></span>
-   <span data-ttu-id="793bd-141">Нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="793bd-141">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="793bd-142">3. Реконструирование модели</span><span class="sxs-lookup"><span data-stu-id="793bd-142">3. Reverse Engineer Model</span></span>

<span data-ttu-id="793bd-143">Мы собираемся использовать Entity Framework Designer, который входит в состав Visual Studio, для создания нашей модели.</span><span class="sxs-lookup"><span data-stu-id="793bd-143">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="793bd-144">**Проект -&gt; добавить новый элемент...**</span><span class="sxs-lookup"><span data-stu-id="793bd-144">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="793bd-145">Выберите **данных** в меню слева и затем **модель EDM ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="793bd-145">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="793bd-146">Введите **BloggingModel** имя и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="793bd-146">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="793bd-147">Это откроет **мастер моделей EDM**</span><span class="sxs-lookup"><span data-stu-id="793bd-147">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="793bd-148">Выберите **создать из базы данных** и нажмите кнопку **Далее**</span><span class="sxs-lookup"><span data-stu-id="793bd-148">Select **Generate from Database** and click **Next**</span></span>

    ![Мастер, шаг 1](~/ef6/media/wizardstep1.png)

-   <span data-ttu-id="793bd-150">Выберите соединение с базой данных, созданной в первом разделе, введите **BloggingContext** как имя строки подключения и нажмите кнопку **Далее**</span><span class="sxs-lookup"><span data-stu-id="793bd-150">Select the connection to the database you created in the first section, enter **BloggingContext** as the name of the connection string and click **Next**</span></span>

    ![Шаг мастера 2](~/ef6/media/wizardstep2.png)

-   <span data-ttu-id="793bd-152">Установите флажок рядом с «Таблицы», чтобы импортировать все таблицы и нажмите кнопку «Готово»</span><span class="sxs-lookup"><span data-stu-id="793bd-152">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Мастер шаг 3](~/ef6/media/wizardstep3.png)

 

<span data-ttu-id="793bd-154">После завершения процесса реконструирования новой модели добавлен в проект и открывается для просмотра в конструкторе Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="793bd-154">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="793bd-155">Файл App.config также был добавлен в проект со сведениями о подключении для базы данных.</span><span class="sxs-lookup"><span data-stu-id="793bd-155">An App.config file has also been added to your project with the connection details for the database.</span></span>

![Начальная модель](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="793bd-157">Дополнительные действия в Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="793bd-157">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="793bd-158">Если вы работаете в Visual Studio 2010, существуют некоторые дополнительные действия, которые необходимо выполнить для обновления до последней версии платформы Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="793bd-158">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="793bd-159">Обновление важно, так как он предоставляет вам доступ к Улучшенная поверхность API, то есть гораздо проще в использовании, а также последние исправления ошибок.</span><span class="sxs-lookup"><span data-stu-id="793bd-159">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="793bd-160">Во-первых, нам нужно получить последнюю версию Entity Framework из NuGet.</span><span class="sxs-lookup"><span data-stu-id="793bd-160">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="793bd-161">\*\*Проект —&gt; управление пакетами NuGet... \*\* 
     \*При отсутствии \**управление пакетами NuGet... \*\* следует установить параметр [последнюю версию NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span><span class="sxs-lookup"><span data-stu-id="793bd-161">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="793bd-162">Выберите **Online** вкладку</span><span class="sxs-lookup"><span data-stu-id="793bd-162">Select the **Online** tab</span></span>
-   <span data-ttu-id="793bd-163">Выберите **EntityFramework** пакета</span><span class="sxs-lookup"><span data-stu-id="793bd-163">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="793bd-164">Нажмите кнопку **установки**</span><span class="sxs-lookup"><span data-stu-id="793bd-164">Click **Install**</span></span>

<span data-ttu-id="793bd-165">Далее нам нужно переключить нашей модели, для которого создается код, который использует API DbContext, который появился в более поздних версиях Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="793bd-165">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="793bd-166">Щелкните правой кнопкой мыши пустое место модели в конструкторе EF и выберите **добавить элемент формирования кода...**</span><span class="sxs-lookup"><span data-stu-id="793bd-166">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="793bd-167">Выберите **шаблоны в Интернете** из меню слева и выполните поиск **DbContext**</span><span class="sxs-lookup"><span data-stu-id="793bd-167">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="793bd-168">Выберите EF **5.x генератор DbContext для C\#**, введите **BloggingModel** имя и нажмите кнопку **добавить**</span><span class="sxs-lookup"><span data-stu-id="793bd-168">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![Шаблон DbContext](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a><span data-ttu-id="793bd-170">4. Чтение и запись данных</span><span class="sxs-lookup"><span data-stu-id="793bd-170">4. Reading & Writing Data</span></span>

<span data-ttu-id="793bd-171">Теперь, когда у нас есть модель, настала пора использовать ее для доступа к некоторые данные.</span><span class="sxs-lookup"><span data-stu-id="793bd-171">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="793bd-172">Классы мы будем использовать для доступа к данным автоматически создаются зависимости в EDMX-файл.</span><span class="sxs-lookup"><span data-stu-id="793bd-172">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="793bd-173">*Этот снимок экрана из Visual Studio 2012, если вы используете Visual Studio 2010 BloggingModel.tt и BloggingModel.Context.tt файлов будет непосредственно в проекте, а не вложен в узел EDMX-файла.*</span><span class="sxs-lookup"><span data-stu-id="793bd-173">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Созданные классы DF](~/ef6/media/generatedclassesdf.png)

 

<span data-ttu-id="793bd-175">Реализуйте метод Main в файле Program.cs, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="793bd-175">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="793bd-176">Этот код создает новый экземпляр класса наш контекст, а затем использует его для вставки нового блога.</span><span class="sxs-lookup"><span data-stu-id="793bd-176">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="793bd-177">Затем он использует запрос LINQ для извлечения из базы данных, представлены в алфавитном порядке по названию все блоги.</span><span class="sxs-lookup"><span data-stu-id="793bd-177">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="793bd-178">Теперь можно запустить приложение и протестировать его.</span><span class="sxs-lookup"><span data-stu-id="793bd-178">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a><span data-ttu-id="793bd-179">5. Изменения базы данных</span><span class="sxs-lookup"><span data-stu-id="793bd-179">5. Dealing with Database Changes</span></span>

<span data-ttu-id="793bd-180">Теперь пора внести некоторые изменения в схему нашей базы данных, когда мы внести эти изменения, необходимо также обновить нашей модели, чтобы отразить эти изменения.</span><span class="sxs-lookup"><span data-stu-id="793bd-180">Now it’s time to make some changes to our database schema, when we make these changes we also need to update our model to reflect those changes.</span></span>

<span data-ttu-id="793bd-181">Первым делом для внесения некоторых изменений в схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="793bd-181">The first step is to make some changes to the database schema.</span></span> <span data-ttu-id="793bd-182">Мы собираемся добавить таблицу пользователей в схему.</span><span class="sxs-lookup"><span data-stu-id="793bd-182">We’re going to add a Users table to the schema.</span></span>

-   <span data-ttu-id="793bd-183">Щелкните правой кнопкой мыши **DatabaseFirst.Blogging** базы данных в обозревателе сервера и выберите **новый запрос**</span><span class="sxs-lookup"><span data-stu-id="793bd-183">Right-click on the **DatabaseFirst.Blogging** database in Server Explorer and select **New Query**</span></span>
-   <span data-ttu-id="793bd-184">Скопируйте следующий запрос SQL в новый запрос, а затем щелкните правой кнопкой мыши запрос и выберите **Execute**</span><span class="sxs-lookup"><span data-stu-id="793bd-184">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

<span data-ttu-id="793bd-185">Теперь, когда схема обновляется, пришло время для обновления модели с помощью этих изменений.</span><span class="sxs-lookup"><span data-stu-id="793bd-185">Now that the schema is updated, it’s time to update the model with those changes.</span></span>

-   <span data-ttu-id="793bd-186">Щелкните правой кнопкой мыши пустое место модели в конструкторе EF и выберите «Обновить модель из базы данных...», это приведет к запуску мастера обновления</span><span class="sxs-lookup"><span data-stu-id="793bd-186">Right-click on an empty spot of your model in the EF Designer and select ‘Update Model from Database…’, this will launch the Update Wizard</span></span>
-   <span data-ttu-id="793bd-187">На вкладке "Добавить" мастер обновления проверки рядом с таблицами это означает, что мы хотим добавить новые таблицы из схемы.</span><span class="sxs-lookup"><span data-stu-id="793bd-187">On the Add tab of the Update Wizard check the box next to Tables, this indicates that we want to add any new tables from the schema.</span></span>
    <span data-ttu-id="793bd-188">*На вкладке обновления отображается все имеющиеся таблицы в модели, которая будет выполнена проверка изменений во время обновления. На вкладках Delete отображаются все таблицы, которые были удалены из схемы, а также будут удалены из модели как часть обновления. Сведения об этих двух вкладках обнаруживается автоматически и предоставляется только в ознакомительных целях нельзя изменить какие-либо параметры.*</span><span class="sxs-lookup"><span data-stu-id="793bd-188">*The Refresh tab shows any existing tables in the model that will be checked for changes during the update. The Delete tabs show any tables that have been removed from the schema and will also be removed from the model as part of the update. The information on these two tabs is automatically detected and is provided for informational purposes only, you cannot change any settings.*</span></span>

    ![Обновление мастера](~/ef6/media/refreshwizard.png)

-   <span data-ttu-id="793bd-190">Щелкните "Готово" в мастере обновления</span><span class="sxs-lookup"><span data-stu-id="793bd-190">Click Finish on the Update Wizard</span></span>

 

<span data-ttu-id="793bd-191">Теперь модель обновляется для включения нового Пользовательская сущность, которая сопоставляется с таблицей пользователей, которые мы добавили в базу данных.</span><span class="sxs-lookup"><span data-stu-id="793bd-191">The model is now updated to include a new User entity that maps to the Users table we added to the database.</span></span>

![Модель обновлена](~/ef6/media/modelupdated.png)

## <a name="summary"></a><span data-ttu-id="793bd-193">Сводка</span><span class="sxs-lookup"><span data-stu-id="793bd-193">Summary</span></span>

<span data-ttu-id="793bd-194">В этом пошаговом руководстве мы рассмотрели Database First разработки, которое позволяет создать модель в конструкторе, EF, основываясь на существующую базу данных.</span><span class="sxs-lookup"><span data-stu-id="793bd-194">In this walkthrough we looked at Database First development, which allowed us to create a model in the EF Designer based on an existing database.</span></span> <span data-ttu-id="793bd-195">Затем мы использовали эту модель для чтения и записи некоторых данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="793bd-195">We then used that model to read and write some data from the database.</span></span> <span data-ttu-id="793bd-196">Наконец мы обновили модель для отражения изменений, внесенных в схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="793bd-196">Finally, we updated the model to reflect changes we made to the database schema.</span></span>
