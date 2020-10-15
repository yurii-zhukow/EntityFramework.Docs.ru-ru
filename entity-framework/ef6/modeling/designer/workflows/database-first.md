---
title: Database First EF6
description: Database First в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/designer/workflows/database-first
ms.openlocfilehash: 4046144ff3f1ecb4a3c9e7d4cb5d6c462c3782c7
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92066113"
---
# <a name="database-first"></a><span data-ttu-id="30790-103">Database First</span><span class="sxs-lookup"><span data-stu-id="30790-103">Database First</span></span>
<span data-ttu-id="30790-104">В этом видео и пошаговом руководстве представлены общие сведения о Database First разработке с помощью Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="30790-104">This video and step-by-step walkthrough provide an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="30790-105">Database First позволяет реконструировать модель из существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="30790-105">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="30790-106">Модель хранится в EDMX-файле (расширение EDMX) и может быть просмотрена и изменена в Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="30790-106">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="30790-107">Классы, с которыми вы взаимодействуете в приложении, автоматически создаются из файла EDMX.</span><span class="sxs-lookup"><span data-stu-id="30790-107">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="30790-108">Просмотреть видео</span><span class="sxs-lookup"><span data-stu-id="30790-108">Watch the video</span></span>
<span data-ttu-id="30790-109">В этом видеоролике приводятся общие сведения о Database First разработке с помощью Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="30790-109">This video provides an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="30790-110">Database First позволяет реконструировать модель из существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="30790-110">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="30790-111">Модель хранится в EDMX-файле (расширение EDMX) и может быть просмотрена и изменена в Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="30790-111">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="30790-112">Классы, с которыми вы взаимодействуете в приложении, автоматически создаются из файла EDMX.</span><span class="sxs-lookup"><span data-stu-id="30790-112">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="30790-113">**Представляет**: [Роуэн Миллер (Rowan Miller)](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="30790-113">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="30790-114">**Видео**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv)  |  [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v)  |  [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="30790-114">**Video**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="30790-115">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="30790-115">Pre-Requisites</span></span>

<span data-ttu-id="30790-116">Для выполнения инструкций этого пошагового руководства необходимо установить Visual Studio 2010 или Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="30790-116">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="30790-117">Если вы используете Visual Studio 2010, вам также потребуется установить [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .</span><span class="sxs-lookup"><span data-stu-id="30790-117">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

 

## <a name="1-create-an-existing-database"></a><span data-ttu-id="30790-118">1. Создание существующей базы данных</span><span class="sxs-lookup"><span data-stu-id="30790-118">1. Create an Existing Database</span></span>

<span data-ttu-id="30790-119">Обычно при использовании существующей базы данных она будет создана, но в этом пошаговом руководстве необходимо создать базу данных для доступа к ней.</span><span class="sxs-lookup"><span data-stu-id="30790-119">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="30790-120">Сервер базы данных, устанавливаемый с Visual Studio, отличается в зависимости от установленной версии Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="30790-120">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="30790-121">При использовании Visual Studio 2010 будет создаваться база данных SQL Express.</span><span class="sxs-lookup"><span data-stu-id="30790-121">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="30790-122">Если вы используете Visual Studio 2012, то будете создавать базу данных [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) .</span><span class="sxs-lookup"><span data-stu-id="30790-122">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

 

<span data-ttu-id="30790-123">Давайте создадим базу данных.</span><span class="sxs-lookup"><span data-stu-id="30790-123">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="30790-124">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="30790-124">Open Visual Studio</span></span>
-   <span data-ttu-id="30790-125">**Просмотр — &gt; Обозреватель сервера**</span><span class="sxs-lookup"><span data-stu-id="30790-125">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="30790-126">Щелкните правой кнопкой мыши **подключения к данным — &gt; Добавить подключение...**</span><span class="sxs-lookup"><span data-stu-id="30790-126">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="30790-127">Если вы не подключились к базе данных из обозреватель сервера, прежде чем нужно будет выбрать Microsoft SQL Server в качестве источника данных</span><span class="sxs-lookup"><span data-stu-id="30790-127">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Выберите источник данных](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="30790-129">Подключитесь к LocalDB или SQL Express, в зависимости от того, какой из установленных служб, и введите **датабасефирст. блога** в качестве имени базы данных.</span><span class="sxs-lookup"><span data-stu-id="30790-129">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **DatabaseFirst.Blogging** as the database name</span></span>

    ![DF подключения к SQL Express](~/ef6/media/sqlexpressconnectiondf.png)

    ![DF подключения LocalDB](~/ef6/media/localdbconnectiondf.png)

-   <span data-ttu-id="30790-132">Нажмите кнопку **ОК** , и появится запрос на создание новой базы данных, выберите **Да** .</span><span class="sxs-lookup"><span data-stu-id="30790-132">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Диалоговое окно создания базы данных](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="30790-134">Новая база данных появится в обозреватель сервера, щелкните ее правой кнопкой мыши и выберите пункт **создать запрос** .</span><span class="sxs-lookup"><span data-stu-id="30790-134">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="30790-135">Скопируйте следующий SQL в новый запрос, щелкните запрос правой кнопкой мыши и выберите команду **выполнить** .</span><span class="sxs-lookup"><span data-stu-id="30790-135">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

## <a name="2-create-the-application"></a><span data-ttu-id="30790-136">2. Создание приложения</span><span class="sxs-lookup"><span data-stu-id="30790-136">2. Create the Application</span></span>

<span data-ttu-id="30790-137">Чтобы не усложнять, мы создадим простое консольное приложение, которое использует Database First для доступа к данным:</span><span class="sxs-lookup"><span data-stu-id="30790-137">To keep things simple we’re going to build a basic console application that uses the Database First to perform data access:</span></span>

-   <span data-ttu-id="30790-138">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="30790-138">Open Visual Studio</span></span>
-   <span data-ttu-id="30790-139">**Файл- &gt; создать &gt; проект...**</span><span class="sxs-lookup"><span data-stu-id="30790-139">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="30790-140">Выбор **окон** в меню слева и **консольное приложение**</span><span class="sxs-lookup"><span data-stu-id="30790-140">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="30790-141">Введите **датабасефирстсампле** в качестве имени</span><span class="sxs-lookup"><span data-stu-id="30790-141">Enter **DatabaseFirstSample** as the name</span></span>
-   <span data-ttu-id="30790-142">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="30790-142">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="30790-143">3. реконструирование модели</span><span class="sxs-lookup"><span data-stu-id="30790-143">3. Reverse Engineer Model</span></span>

<span data-ttu-id="30790-144">Мы будем использовать Entity Framework Designer, который входит в состав Visual Studio, для создания нашей модели.</span><span class="sxs-lookup"><span data-stu-id="30790-144">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="30790-145">**Проект- &gt; Добавить новый элемент...**</span><span class="sxs-lookup"><span data-stu-id="30790-145">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="30790-146">Выберите **данные** в меню слева, а затем **ADO.NET EDM**</span><span class="sxs-lookup"><span data-stu-id="30790-146">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="30790-147">Введите **блоггингмодел** в качестве имени и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="30790-147">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="30790-148">Запустится **мастер EDM**</span><span class="sxs-lookup"><span data-stu-id="30790-148">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="30790-149">Выберите **создать из базы данных** и нажмите кнопку **Далее** .</span><span class="sxs-lookup"><span data-stu-id="30790-149">Select **Generate from Database** and click **Next**</span></span>

    ![Шаг 1 мастера](~/ef6/media/wizardstep1.png)

-   <span data-ttu-id="30790-151">Выберите подключение к базе данных, созданной в первом разделе, введите **BloggingContext** в качестве имени строки подключения и нажмите кнопку **Далее** .</span><span class="sxs-lookup"><span data-stu-id="30790-151">Select the connection to the database you created in the first section, enter **BloggingContext** as the name of the connection string and click **Next**</span></span>

    ![Шаг 2 мастера](~/ef6/media/wizardstep2.png)

-   <span data-ttu-id="30790-153">Установите флажок рядом с пунктом "таблицы", чтобы импортировать все таблицы, и нажмите кнопку "Готово".</span><span class="sxs-lookup"><span data-stu-id="30790-153">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Шаг 3 мастера](~/ef6/media/wizardstep3.png)

 

<span data-ttu-id="30790-155">После завершения процесса реконструирования новая модель добавляется в проект и открывается для просмотра в Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="30790-155">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="30790-156">В проект также добавлен файл App.config с подробными сведениями о соединении для базы данных.</span><span class="sxs-lookup"><span data-stu-id="30790-156">An App.config file has also been added to your project with the connection details for the database.</span></span>

![Начальная модель](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="30790-158">Дополнительные действия в Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="30790-158">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="30790-159">При работе в Visual Studio 2010 необходимо выполнить некоторые дополнительные действия по обновлению до последней версии Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="30790-159">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="30790-160">Обновление важно, так как оно предоставляет доступ к улучшенной поверхности API, которая гораздо проще в использовании, а также последние исправления ошибок.</span><span class="sxs-lookup"><span data-stu-id="30790-160">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="30790-161">Сначала необходимо получить последнюю версию Entity Framework из NuGet.</span><span class="sxs-lookup"><span data-stu-id="30790-161">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="30790-162">**Проект — &gt; Управление пакетами NuGet...** 
     *Если у вас нет параметра **Управление пакетами NuGet** , следует установить [последнюю версию NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .*</span><span class="sxs-lookup"><span data-stu-id="30790-162">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="30790-163">Выбор вкладки "в **сети** "</span><span class="sxs-lookup"><span data-stu-id="30790-163">Select the **Online** tab</span></span>
-   <span data-ttu-id="30790-164">Выберите пакет **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="30790-164">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="30790-165">Щелкните **Установить**.</span><span class="sxs-lookup"><span data-stu-id="30790-165">Click **Install**</span></span>

<span data-ttu-id="30790-166">Далее необходимо поменять модель, чтобы создать код, который использует API DbContext, который появился в более поздних версиях Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="30790-166">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="30790-167">Щелкните правой кнопкой мыши пустое место в модели в конструкторе EF и выберите пункт **Добавить элемент создания кода...**</span><span class="sxs-lookup"><span data-stu-id="30790-167">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="30790-168">В меню слева выберите **шаблоны в Интернете** и выполните поиск по запросу **DbContext**</span><span class="sxs-lookup"><span data-stu-id="30790-168">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="30790-169">Выберите генератор EF \*\*5. x DbContext для C \# \*\*, введите **блоггингмодел** в качестве имени и нажмите кнопку **Добавить** .</span><span class="sxs-lookup"><span data-stu-id="30790-169">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![Шаблон DbContext](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a><span data-ttu-id="30790-171">4. чтение & запись данных</span><span class="sxs-lookup"><span data-stu-id="30790-171">4. Reading & Writing Data</span></span>

<span data-ttu-id="30790-172">Теперь, когда у нас есть модель, пришло время использовать ее для доступа к некоторым данным.</span><span class="sxs-lookup"><span data-stu-id="30790-172">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="30790-173">Классы, которые мы будем использовать для доступа к данным, создаются автоматически на основе файла EDMX.</span><span class="sxs-lookup"><span data-stu-id="30790-173">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="30790-174">*Этот снимок экрана находится в Visual Studio 2012, если вы используете Visual Studio 2010 файлы BloggingModel.tt и BloggingModel.Context.tt будут находиться непосредственно в проекте, а не вложены в EDMX-файл.*</span><span class="sxs-lookup"><span data-stu-id="30790-174">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Создаваемые классы DF](~/ef6/media/generatedclassesdf.png)

 

<span data-ttu-id="30790-176">Реализуйте метод Main в Program.cs, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="30790-176">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="30790-177">Этот код создает новый экземпляр нашего контекста, а затем использует его для вставки нового блога.</span><span class="sxs-lookup"><span data-stu-id="30790-177">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="30790-178">Затем он использует запрос LINQ для получения всех блогов из базы данных, упорядоченного в алфавитном порядке по названию.</span><span class="sxs-lookup"><span data-stu-id="30790-178">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="30790-179">Теперь можно запустить приложение и протестировать его.</span><span class="sxs-lookup"><span data-stu-id="30790-179">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a><span data-ttu-id="30790-180">5. Работа с изменениями базы данных</span><span class="sxs-lookup"><span data-stu-id="30790-180">5. Dealing with Database Changes</span></span>

<span data-ttu-id="30790-181">Пришло время внести некоторые изменения в схему базы данных, при внесении этих изменений нам также потребуется обновить нашу модель, чтобы отразить эти изменения.</span><span class="sxs-lookup"><span data-stu-id="30790-181">Now it’s time to make some changes to our database schema, when we make these changes we also need to update our model to reflect those changes.</span></span>

<span data-ttu-id="30790-182">Первым шагом является внесение некоторых изменений в схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="30790-182">The first step is to make some changes to the database schema.</span></span> <span data-ttu-id="30790-183">В схему будет добавлена таблица пользователей.</span><span class="sxs-lookup"><span data-stu-id="30790-183">We’re going to add a Users table to the schema.</span></span>

-   <span data-ttu-id="30790-184">Щелкните правой кнопкой мыши базу данных **датабасефирст. блоги** в обозреватель сервера и выберите **создать запрос** .</span><span class="sxs-lookup"><span data-stu-id="30790-184">Right-click on the **DatabaseFirst.Blogging** database in Server Explorer and select **New Query**</span></span>
-   <span data-ttu-id="30790-185">Скопируйте следующий SQL в новый запрос, щелкните запрос правой кнопкой мыши и выберите команду **выполнить** .</span><span class="sxs-lookup"><span data-stu-id="30790-185">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

<span data-ttu-id="30790-186">Теперь, когда схема обновлена, пришло время обновить модель с помощью этих изменений.</span><span class="sxs-lookup"><span data-stu-id="30790-186">Now that the schema is updated, it’s time to update the model with those changes.</span></span>

-   <span data-ttu-id="30790-187">Щелкните правой кнопкой мыши пустое место в модели в конструкторе EF и выберите "обновить модель из базы данных...", после чего запустится мастер обновления.</span><span class="sxs-lookup"><span data-stu-id="30790-187">Right-click on an empty spot of your model in the EF Designer and select ‘Update Model from Database…’, this will launch the Update Wizard</span></span>
-   <span data-ttu-id="30790-188">На вкладке Добавить мастера обновления установите флажок рядом с таблицами. Это означает, что нам нужно добавить новые таблицы из схемы.</span><span class="sxs-lookup"><span data-stu-id="30790-188">On the Add tab of the Update Wizard check the box next to Tables, this indicates that we want to add any new tables from the schema.</span></span>
    <span data-ttu-id="30790-189">*На вкладке обновление отображаются все имеющиеся в модели таблицы, которые будут проверяться на наличие изменений во время обновления. На вкладках удаление отображаются все таблицы, которые были удалены из схемы и также будут удалены из модели в рамках обновления. Информация на этих двух вкладках автоматически обнаруживается и предоставляется только в информационных целях, поэтому изменить параметры нельзя.*</span><span class="sxs-lookup"><span data-stu-id="30790-189">*The Refresh tab shows any existing tables in the model that will be checked for changes during the update. The Delete tabs show any tables that have been removed from the schema and will also be removed from the model as part of the update. The information on these two tabs is automatically detected and is provided for informational purposes only, you cannot change any settings.*</span></span>

    ![Мастер обновления](~/ef6/media/refreshwizard.png)

-   <span data-ttu-id="30790-191">Нажмите кнопку "Готово" в мастере обновления.</span><span class="sxs-lookup"><span data-stu-id="30790-191">Click Finish on the Update Wizard</span></span>

 

<span data-ttu-id="30790-192">Теперь модель обновлена, чтобы включить новую сущность пользователя, сопоставленную с таблицей Users, добавленной в базу данных.</span><span class="sxs-lookup"><span data-stu-id="30790-192">The model is now updated to include a new User entity that maps to the Users table we added to the database.</span></span>

![Модель обновлена](~/ef6/media/modelupdated.png)

## <a name="summary"></a><span data-ttu-id="30790-194">Итоги</span><span class="sxs-lookup"><span data-stu-id="30790-194">Summary</span></span>

<span data-ttu-id="30790-195">В этом пошаговом руководстве мы рассматривали Database Firstную разработку, которая позволила нам создать модель в конструкторе EF на основе существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="30790-195">In this walkthrough we looked at Database First development, which allowed us to create a model in the EF Designer based on an existing database.</span></span> <span data-ttu-id="30790-196">Затем мы использовали эту модель для чтения и записи данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="30790-196">We then used that model to read and write some data from the database.</span></span> <span data-ttu-id="30790-197">Наконец, мы обновили модель, чтобы отразить изменения, внесенные в схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="30790-197">Finally, we updated the model to reflect changes we made to the database schema.</span></span>
