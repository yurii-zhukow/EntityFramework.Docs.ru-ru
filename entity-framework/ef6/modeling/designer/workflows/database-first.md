---
title: Database First EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: d40cff4ddccf43a394ef4f244653372a5a89b05a
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182454"
---
# <a name="database-first"></a><span data-ttu-id="6e109-102">База данных как основа</span><span class="sxs-lookup"><span data-stu-id="6e109-102">Database First</span></span>
<span data-ttu-id="6e109-103">Следующее видео и пошаговое руководство познакомят вас с разработкой на основе базы данных в Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6e109-103">This video and step-by-step walkthrough provide an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="6e109-104">Такой подход дает возможность реконструировать модель по существующей базе.</span><span class="sxs-lookup"><span data-stu-id="6e109-104">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="6e109-105">Модель хранится в EDMX-файле (расширение .emdx), и её можно просмотреть и изменить в Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="6e109-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="6e109-106">Классы, с которыми вы взаимодействуете в приложении, автоматически создаются из файла EDMX.</span><span class="sxs-lookup"><span data-stu-id="6e109-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="6e109-107">Просмотрите видео</span><span class="sxs-lookup"><span data-stu-id="6e109-107">Watch the video</span></span>
<span data-ttu-id="6e109-108">Это видео предоставляет общие сведения о разработке на основе базы данных в Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6e109-108">This video provides an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="6e109-109">Такой подход дает возможность реконструировать модель по существующей базе.</span><span class="sxs-lookup"><span data-stu-id="6e109-109">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="6e109-110">Модель хранится в EDMX-файле (расширение .emdx), и её можно просмотреть и изменить в Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="6e109-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="6e109-111">Классы, с которыми вы взаимодействуете в приложении, автоматически создаются из файла EDMX.</span><span class="sxs-lookup"><span data-stu-id="6e109-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="6e109-112">**Представляет**: [Роуэн Миллер (Rowan Miller)](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="6e109-112">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="6e109-113">**Видео**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="6e109-113">**Video**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="6e109-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="6e109-114">Pre-Requisites</span></span>

<span data-ttu-id="6e109-115">Для выполнения инструкций этого пошагового руководства необходимо установить Visual Studio 2010 или Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="6e109-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="6e109-116">Если вы используете Visual Studio 2010, также необходимо будет установить [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c).</span><span class="sxs-lookup"><span data-stu-id="6e109-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

 

## <a name="1-create-an-existing-database"></a><span data-ttu-id="6e109-117">1. Создание существующей базы данных</span><span class="sxs-lookup"><span data-stu-id="6e109-117">1. Create an Existing Database</span></span>

<span data-ttu-id="6e109-118">Обычно при ориентировании на существующую базу данных она уже будет создана, но в этом пошаговом руководстве нам нужно создать базу данных для доступа.</span><span class="sxs-lookup"><span data-stu-id="6e109-118">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="6e109-119">Сервер базы данных, который устанавливается вместе с Visual Studio, отличается в зависимости от версии Visual Studio, которую вы установили:</span><span class="sxs-lookup"><span data-stu-id="6e109-119">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="6e109-120">Если вы используете Visual Studio 2010, вы создадите базу данных SQL Express.</span><span class="sxs-lookup"><span data-stu-id="6e109-120">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="6e109-121">Если вы используете Visual Studio 2012, вы создадите базу данных [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="6e109-121">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

 

<span data-ttu-id="6e109-122">Перейдем дальше и создадим базу данных.</span><span class="sxs-lookup"><span data-stu-id="6e109-122">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="6e109-123">Запустите Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6e109-123">Open Visual Studio</span></span>
-   <span data-ttu-id="6e109-124">**Вид" —&gt; "Обозреватель сервера**</span><span class="sxs-lookup"><span data-stu-id="6e109-124">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="6e109-125">Щелкните правой кнопкой мыши на **Подключения к данным -&gt; Добавить подключение…**</span><span class="sxs-lookup"><span data-stu-id="6e109-125">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="6e109-126">Если вы не подключались к базе данных с помощью "Обозревателя сервера" ранее, потребуется выбрать Microsoft SQL Server в качестве источника данных</span><span class="sxs-lookup"><span data-stu-id="6e109-126">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Выберите источник данных](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="6e109-128">Подключитесь к LocalDB или SQL Express, в зависимости от того, какую из них вы установили, и введите имя базы данных **DatabaseFirst.Blogging**</span><span class="sxs-lookup"><span data-stu-id="6e109-128">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **DatabaseFirst.Blogging** as the database name</span></span>

    ![DF подключения к SQL Express](~/ef6/media/sqlexpressconnectiondf.png)

    ![DF подключения LocalDB](~/ef6/media/localdbconnectiondf.png)

-   <span data-ttu-id="6e109-131">Выберите **ОК**, и вам будет задан вопрос, хотите ли вы создать новую базу данных. Выберите **Да**</span><span class="sxs-lookup"><span data-stu-id="6e109-131">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Диалоговое окно создания базы данных](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="6e109-133">Новая база данных появится в Обозревателе сервера. Щелкните ее правой кнопкой мыши и выберите **Новый запрос**</span><span class="sxs-lookup"><span data-stu-id="6e109-133">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="6e109-134">Скопируйте следующий код SQL в новый запрос, а затем щелкните запрос правой кнопкой мыши и выберите **Выполнить**</span><span class="sxs-lookup"><span data-stu-id="6e109-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

## <a name="2-create-the-application"></a><span data-ttu-id="6e109-135">2. Создание приложения</span><span class="sxs-lookup"><span data-stu-id="6e109-135">2. Create the Application</span></span>

<span data-ttu-id="6e109-136">Для простоты мы создадим простое консольное приложение, использующее подход, основывающийся на базе данных, для выполнения доступа к данным:</span><span class="sxs-lookup"><span data-stu-id="6e109-136">To keep things simple we’re going to build a basic console application that uses the Database First to perform data access:</span></span>

-   <span data-ttu-id="6e109-137">Запустите Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6e109-137">Open Visual Studio</span></span>
-   <span data-ttu-id="6e109-138">**Файл —&gt; Создать —&gt; Проект…**</span><span class="sxs-lookup"><span data-stu-id="6e109-138">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="6e109-139">В меню слева выберите **Windows** и **Консольное приложение**</span><span class="sxs-lookup"><span data-stu-id="6e109-139">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="6e109-140">Введите **DatabaseFirstSample** в поле "Имя"</span><span class="sxs-lookup"><span data-stu-id="6e109-140">Enter **DatabaseFirstSample** as the name</span></span>
-   <span data-ttu-id="6e109-141">Нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="6e109-141">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="6e109-142">3. реконструирование модели</span><span class="sxs-lookup"><span data-stu-id="6e109-142">3. Reverse Engineer Model</span></span>

<span data-ttu-id="6e109-143">Для создания нашей модели мы собираемся использовать конструктор Entity Framework Designer, который входит в состав Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6e109-143">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="6e109-144">**Проект -&gt; Добавить новый элемент…**</span><span class="sxs-lookup"><span data-stu-id="6e109-144">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="6e109-145">Выберите **Данные** в меню слева и затем **Модель ADO.NET EDM**</span><span class="sxs-lookup"><span data-stu-id="6e109-145">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="6e109-146">Введите имя **BloggingModel** и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="6e109-146">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="6e109-147">Это откроет **Мастер моделей EDM**</span><span class="sxs-lookup"><span data-stu-id="6e109-147">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="6e109-148">Выберите **Создать из базы данных** и нажмите кнопку **Далее**</span><span class="sxs-lookup"><span data-stu-id="6e109-148">Select **Generate from Database** and click **Next**</span></span>

    ![Шаг 1 мастера](~/ef6/media/wizardstep1.png)

-   <span data-ttu-id="6e109-150">Выберите подключение к базе данных, созданной в первом разделе, введите **BloggingContext** в качестве имени строки подключения и нажмите кнопку **Далее** .</span><span class="sxs-lookup"><span data-stu-id="6e109-150">Select the connection to the database you created in the first section, enter **BloggingContext** as the name of the connection string and click **Next**</span></span>

    ![Шаг 2 мастера](~/ef6/media/wizardstep2.png)

-   <span data-ttu-id="6e109-152">Установите флажок рядом с пунктом "таблицы", чтобы импортировать все таблицы, и нажмите кнопку "Готово".</span><span class="sxs-lookup"><span data-stu-id="6e109-152">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Шаг 3 мастера](~/ef6/media/wizardstep3.png)

 

<span data-ttu-id="6e109-154">После завершения процесса реконструирования новая модель будет добавлена в проект и откроется для просмотра в Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="6e109-154">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="6e109-155">В проект также будет добавлен файл App.config со сведениями о подключении к базе данных.</span><span class="sxs-lookup"><span data-stu-id="6e109-155">An App.config file has also been added to your project with the connection details for the database.</span></span>

![Начальная модель](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="6e109-157">Дополнительные действия в Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="6e109-157">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="6e109-158">При работе в Visual Studio 2010 необходимо выполнить некоторые дополнительные действия по обновлению до последней версии Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6e109-158">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="6e109-159">Обновление важно, так как оно предоставляет вам доступ к улучшенным API, которые гораздо проще в использовании, а также последние исправления ошибок.</span><span class="sxs-lookup"><span data-stu-id="6e109-159">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="6e109-160">Сначала необходимо получить последнюю версию Entity Framework из NuGet.</span><span class="sxs-lookup"><span data-stu-id="6e109-160">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="6e109-161">\*\*Проект —&gt; Управление пакетами NuGet... \*\* 
     \*При отсутствии пункта \**Управление пакетами NuGet… \*\* следует установить [последнюю версию NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span><span class="sxs-lookup"><span data-stu-id="6e109-161">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="6e109-162">Выберите вкладку **В сети**</span><span class="sxs-lookup"><span data-stu-id="6e109-162">Select the **Online** tab</span></span>
-   <span data-ttu-id="6e109-163">Выберите пакет **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="6e109-163">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="6e109-164">Нажмите кнопку **Установить**</span><span class="sxs-lookup"><span data-stu-id="6e109-164">Click **Install**</span></span>

<span data-ttu-id="6e109-165">Далее нам нужно переключить нашу модель, чтобы сгенерировать код, который использует API DbContext, появившийся в более поздних версиях Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6e109-165">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="6e109-166">Щелкните правой кнопкой мыши на пустом месте модели в EF Designer и выберите **Добавить элемент создания кода…**</span><span class="sxs-lookup"><span data-stu-id="6e109-166">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="6e109-167">Выберите **Шаблоны в Интернете** из меню слева и выполните поиск **DbContext**</span><span class="sxs-lookup"><span data-stu-id="6e109-167">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="6e109-168">Выберите генератор EF **5. x DbContext для C\#** , введите **блоггингмодел** в качестве имени и нажмите кнопку **добавить** .</span><span class="sxs-lookup"><span data-stu-id="6e109-168">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![Шаблон DbContext](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a><span data-ttu-id="6e109-170">4. Чтение & запись данных</span><span class="sxs-lookup"><span data-stu-id="6e109-170">4. Reading & Writing Data</span></span>

<span data-ttu-id="6e109-171">Теперь, когда у нас есть модель, настала пора использовать ее для доступа к каким-нибудь данным.</span><span class="sxs-lookup"><span data-stu-id="6e109-171">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="6e109-172">Классы, которые мы будем использовать для доступа к данным, автоматически создаются в зависимости от EDMX-файла.</span><span class="sxs-lookup"><span data-stu-id="6e109-172">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="6e109-173">*Этот снимок экрана из Visual Studio 2012. Если вы используете Visual Studio 2010, файлы BloggingModel.tt и BloggingModel.Context.tt будут лежать непосредственно в проекте, а не вложены в узел EDMX-файла.*</span><span class="sxs-lookup"><span data-stu-id="6e109-173">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Создаваемые классы DF](~/ef6/media/generatedclassesdf.png)

 

<span data-ttu-id="6e109-175">Реализуйте метод Main в файле Program.cs так, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="6e109-175">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="6e109-176">Этот код создает новый экземпляр нашего контекста, а затем использует его для вставки новой записи блога.</span><span class="sxs-lookup"><span data-stu-id="6e109-176">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="6e109-177">Затем он использует запрос LINQ для извлечения из базы данных всех записей блога, упорядоченных в алфавитном порядке по названию.</span><span class="sxs-lookup"><span data-stu-id="6e109-177">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="6e109-178">Теперь можно запустить приложение и протестировать его.</span><span class="sxs-lookup"><span data-stu-id="6e109-178">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a><span data-ttu-id="6e109-179">5. Работа с изменениями базы данных</span><span class="sxs-lookup"><span data-stu-id="6e109-179">5. Dealing with Database Changes</span></span>

<span data-ttu-id="6e109-180">Теперь пора внести некоторые изменения в схему нашей базы данных. Когда мы внесём эти изменения, необходимо также обновить нашу модель, чтобы отразить эти изменения.</span><span class="sxs-lookup"><span data-stu-id="6e109-180">Now it’s time to make some changes to our database schema, when we make these changes we also need to update our model to reflect those changes.</span></span>

<span data-ttu-id="6e109-181">Первый шаг — это внесение некоторых изменений в схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="6e109-181">The first step is to make some changes to the database schema.</span></span> <span data-ttu-id="6e109-182">В схему будет добавлена таблица пользователей.</span><span class="sxs-lookup"><span data-stu-id="6e109-182">We’re going to add a Users table to the schema.</span></span>

-   <span data-ttu-id="6e109-183">Щелкните правой кнопкой мыши базу данных **DatabaseFirst.Blogging** в Обозревателе сервера и выберите **Создать запрос**</span><span class="sxs-lookup"><span data-stu-id="6e109-183">Right-click on the **DatabaseFirst.Blogging** database in Server Explorer and select **New Query**</span></span>
-   <span data-ttu-id="6e109-184">Скопируйте следующий код SQL в новый запрос, а затем щелкните запрос правой кнопкой мыши и выберите **Выполнить**</span><span class="sxs-lookup"><span data-stu-id="6e109-184">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

<span data-ttu-id="6e109-185">Теперь, когда схема обновлена, пришло время внести изменения в модель.</span><span class="sxs-lookup"><span data-stu-id="6e109-185">Now that the schema is updated, it’s time to update the model with those changes.</span></span>

-   <span data-ttu-id="6e109-186">Щелкните правой кнопкой мыши пустое место в модели в конструкторе EF и выберите "обновить модель из базы данных...", после чего запустится мастер обновления.</span><span class="sxs-lookup"><span data-stu-id="6e109-186">Right-click on an empty spot of your model in the EF Designer and select ‘Update Model from Database…’, this will launch the Update Wizard</span></span>
-   <span data-ttu-id="6e109-187">На вкладке "Добавление" мастера обновления установите флажок рядом с таблицами. Это означает, что мы хотим добавить новые таблицы из схемы.</span><span class="sxs-lookup"><span data-stu-id="6e109-187">On the Add tab of the Update Wizard check the box next to Tables, this indicates that we want to add any new tables from the schema.</span></span>
    <span data-ttu-id="6e109-188">*На вкладке обновление отображаются все имеющиеся в модели таблицы, которые будут проверяться на наличие изменений во время обновления. На вкладках удаление отображаются все таблицы, которые были удалены из схемы и также будут удалены из модели в рамках обновления. Информация на этих двух вкладках автоматически обнаруживается и предоставляется только в информационных целях, поэтому изменить параметры нельзя.*</span><span class="sxs-lookup"><span data-stu-id="6e109-188">*The Refresh tab shows any existing tables in the model that will be checked for changes during the update. The Delete tabs show any tables that have been removed from the schema and will also be removed from the model as part of the update. The information on these two tabs is automatically detected and is provided for informational purposes only, you cannot change any settings.*</span></span>

    ![Мастер обновления](~/ef6/media/refreshwizard.png)

-   <span data-ttu-id="6e109-190">Нажмите кнопку "Готово" в мастере обновления.</span><span class="sxs-lookup"><span data-stu-id="6e109-190">Click Finish on the Update Wizard</span></span>

 

<span data-ttu-id="6e109-191">Теперь модель обновлена. Добавлена новая сущность "Пользователь", которая сопоставляется с таблицей пользователей, которую мы добавили в базу данных.</span><span class="sxs-lookup"><span data-stu-id="6e109-191">The model is now updated to include a new User entity that maps to the Users table we added to the database.</span></span>

![Модель обновлена](~/ef6/media/modelupdated.png)

## <a name="summary"></a><span data-ttu-id="6e109-193">Сводка</span><span class="sxs-lookup"><span data-stu-id="6e109-193">Summary</span></span>

<span data-ttu-id="6e109-194">В этом пошаговом руководстве мы рассмотрели разработку на основе базы данных, которая позволяет создать в EF Designer модель, основываясь на существующей базе.</span><span class="sxs-lookup"><span data-stu-id="6e109-194">In this walkthrough we looked at Database First development, which allowed us to create a model in the EF Designer based on an existing database.</span></span> <span data-ttu-id="6e109-195">Мы использовали эту модель для чтения и записи некоторых данных в базе.</span><span class="sxs-lookup"><span data-stu-id="6e109-195">We then used that model to read and write some data from the database.</span></span> <span data-ttu-id="6e109-196">Наконец, мы обновили модель для отражения изменений, внесенных в схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="6e109-196">Finally, we updated the model to reflect changes we made to the database schema.</span></span>
