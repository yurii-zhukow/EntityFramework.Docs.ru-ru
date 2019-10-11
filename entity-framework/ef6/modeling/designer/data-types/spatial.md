---
title: Конструктор пространственного-EF-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: a9c54fbc14dd02ce5d4d91449a0d5f9e72f7f0f7
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182502"
---
# <a name="spatial---ef-designer"></a><span data-ttu-id="065a0-102">Конструктор пространственного-EF</span><span class="sxs-lookup"><span data-stu-id="065a0-102">Spatial - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="065a0-103">**Только EF5** — функции, API и т. д., обсуждаемые на этой странице, появились в Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="065a0-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="065a0-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="065a0-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="065a0-105">В видеоролике и пошаговом руководстве показано, как сопоставлять пространственные типы с Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="065a0-105">The video and step-by-step walkthrough shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="065a0-106">В нем также показано, как использовать запрос LINQ для поиска расстояния между двумя расположениями.</span><span class="sxs-lookup"><span data-stu-id="065a0-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="065a0-107">В этом пошаговом руководстве будет использоваться Model First для создания новой базы данных, но конструктор EF также можно использовать с рабочим процессом [Database First](~/ef6/modeling/designer/workflows/database-first.md) для соотнесения с существующей базой данных.</span><span class="sxs-lookup"><span data-stu-id="065a0-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="065a0-108">Поддержка пространственного типа появилась в Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="065a0-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="065a0-109">Обратите внимание, что для использования новых функций, таких как пространственный тип, перечисления и функции с табличным значением, необходимо выбрать .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="065a0-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="065a0-110">Visual Studio 2012 нацелена на .NET 4,5 по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="065a0-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="065a0-111">Чтобы использовать пространственные типы данных, необходимо также использовать поставщик Entity Framework с поддержкой пространственного доступа.</span><span class="sxs-lookup"><span data-stu-id="065a0-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="065a0-112">Дополнительные сведения см. в разделе [Поддержка пространственных типов в поставщике](~/ef6/fundamentals/providers/spatial-support.md) .</span><span class="sxs-lookup"><span data-stu-id="065a0-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="065a0-113">Существует два основных типа пространственных данных: geography и geometry.</span><span class="sxs-lookup"><span data-stu-id="065a0-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="065a0-114">Тип данных geography хранит данные эллиптических (например, координаты широты и долготы GPS).</span><span class="sxs-lookup"><span data-stu-id="065a0-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="065a0-115">Тип данных geometry представляет евклидово (плоскую) систему координат.</span><span class="sxs-lookup"><span data-stu-id="065a0-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="065a0-116">Просмотрите видео</span><span class="sxs-lookup"><span data-stu-id="065a0-116">Watch the video</span></span>
<span data-ttu-id="065a0-117">В этом видео показано, как сопоставлять пространственные типы с Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="065a0-117">This video shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="065a0-118">В нем также показано, как использовать запрос LINQ для поиска расстояния между двумя расположениями.</span><span class="sxs-lookup"><span data-stu-id="065a0-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="065a0-119">**Представлено**: Julia Корнич</span><span class="sxs-lookup"><span data-stu-id="065a0-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="065a0-120">**Видео**: [WMV](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span><span class="sxs-lookup"><span data-stu-id="065a0-120">**Video**: [WMV](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="065a0-121">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="065a0-121">Pre-Requisites</span></span>

<span data-ttu-id="065a0-122">Для выполнения инструкций этого пошагового руководства необходимо установить Visual Studio 2012, Ultimate, Premium, Professional или Web Express Edition.</span><span class="sxs-lookup"><span data-stu-id="065a0-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="065a0-123">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="065a0-123">Set up the Project</span></span>

1.  <span data-ttu-id="065a0-124">Откройте Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="065a0-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="065a0-125">В меню **файл** наведите указатель мыши на пункт **создать**и выберите **проект** .</span><span class="sxs-lookup"><span data-stu-id="065a0-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="065a0-126">В левой области щелкните **Visual C @ no__t-1**, а затем выберите шаблон **консоли** .</span><span class="sxs-lookup"><span data-stu-id="065a0-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="065a0-127">Введите **спатиалефдесигнер** в качестве имени проекта и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="065a0-127">Enter **SpatialEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="065a0-128">Создание новой модели с помощью конструктора EF</span><span class="sxs-lookup"><span data-stu-id="065a0-128">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="065a0-129">Щелкните правой кнопкой мыши имя проекта в обозреватель решений, наведите указатель на пункт **Добавить**и выберите пункт **новый элемент** .</span><span class="sxs-lookup"><span data-stu-id="065a0-129">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="065a0-130">Выберите **данные** в меню слева, а затем выберите **ADO.NET EDM** в области Шаблоны.</span><span class="sxs-lookup"><span data-stu-id="065a0-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="065a0-131">Введите **университимодел. EDMX** в поле имя файла и нажмите кнопку **Добавить** .</span><span class="sxs-lookup"><span data-stu-id="065a0-131">Enter **UniversityModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="065a0-132">На странице "Мастер EDM" в диалоговом окне Выбор содержимого модели выберите **Пустая модель** .</span><span class="sxs-lookup"><span data-stu-id="065a0-132">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="065a0-133">Нажмите кнопку **Готово** .</span><span class="sxs-lookup"><span data-stu-id="065a0-133">Click **Finish**</span></span>

<span data-ttu-id="065a0-134">Отобразится Entity Designer, предоставляющий область конструктора для редактирования модели.</span><span class="sxs-lookup"><span data-stu-id="065a0-134">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="065a0-135">Мастер выполняет следующие действия.</span><span class="sxs-lookup"><span data-stu-id="065a0-135">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="065a0-136">Создает файл Енумтестмодел. EDMX, который определяет концептуальную модель, модель хранения и сопоставление между ними.</span><span class="sxs-lookup"><span data-stu-id="065a0-136">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="065a0-137">Задает свойство обработки артефакта метаданных файла EDMX, которое будет внедрено в выходную сборку, чтобы созданные файлы метаданных были внедрены в сборку.</span><span class="sxs-lookup"><span data-stu-id="065a0-137">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="065a0-138">Добавляет ссылку на следующие сборки: EntityFramework, System. ComponentModel. Аннотация и System. Data. Entity.</span><span class="sxs-lookup"><span data-stu-id="065a0-138">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="065a0-139">Создает файлы UniversityModel.tt и UniversityModel.Context.tt и добавляет их в EDMX файл.</span><span class="sxs-lookup"><span data-stu-id="065a0-139">Creates UniversityModel.tt and UniversityModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="065a0-140">Эти файлы шаблонов T4 создают код, определяющий производный тип DbContext и типы POCO, которые сопоставляются с сущностями в модели EDMX.</span><span class="sxs-lookup"><span data-stu-id="065a0-140">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="065a0-141">Добавить новый тип сущности</span><span class="sxs-lookup"><span data-stu-id="065a0-141">Add a New Entity Type</span></span>

1.  <span data-ttu-id="065a0-142">Щелкните правой кнопкой мыши пустую область в области конструктора и выберите **Добавить-&gt; сущность**, откроется диалоговое окно Новая сущность.</span><span class="sxs-lookup"><span data-stu-id="065a0-142">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="065a0-143">Укажите название типа **университет** и укажите **университид** в качестве имени свойства ключа, оставьте тип **Int32** .</span><span class="sxs-lookup"><span data-stu-id="065a0-143">Specify **University** for the type name and specify **UniversityID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="065a0-144">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="065a0-144">Click **OK**</span></span>
4.  <span data-ttu-id="065a0-145">Щелкните сущность правой кнопкой мыши и выберите пункт **Добавить New-&gt; скалярное свойство** .</span><span class="sxs-lookup"><span data-stu-id="065a0-145">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="065a0-146">Переименовать новое свойство в **имя**</span><span class="sxs-lookup"><span data-stu-id="065a0-146">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="065a0-147">Добавьте еще одно скалярное свойство и переименуйте его в **Расположение** . Откройте окно свойств и измените тип нового свойства на **Geography** .</span><span class="sxs-lookup"><span data-stu-id="065a0-147">Add another scalar property and rename it to **Location** Open the Properties window and change the type of the new property to **Geography**</span></span>
7.  <span data-ttu-id="065a0-148">Сохранение модели и сборка проекта</span><span class="sxs-lookup"><span data-stu-id="065a0-148">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="065a0-149">При сборке в Список ошибок могут отображаться предупреждения о несопоставленных сущностях и ассоциациях.</span><span class="sxs-lookup"><span data-stu-id="065a0-149">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="065a0-150">Эти предупреждения можно игнорировать, так как после выбора создания базы данных из модели ошибки будут исчезнуть.</span><span class="sxs-lookup"><span data-stu-id="065a0-150">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="065a0-151">Создать базу данных из модели</span><span class="sxs-lookup"><span data-stu-id="065a0-151">Generate Database from Model</span></span>

<span data-ttu-id="065a0-152">Теперь можно создать базу данных, основанную на модели.</span><span class="sxs-lookup"><span data-stu-id="065a0-152">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="065a0-153">Щелкните правой кнопкой мыши пустое пространство на поверхности Entity Designer и выберите пункт **создать базу данных из модели** .</span><span class="sxs-lookup"><span data-stu-id="065a0-153">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="065a0-154">Откроется диалоговое окно Выбор подключения к данным мастера создания базы данных. Нажмите кнопку **создать подключение** , укажите **(LocalDB) \\mssqllocaldb** для имени сервера и **университета** для базы данных и нажмите кнопку **ОК.**</span><span class="sxs-lookup"><span data-stu-id="065a0-154">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **University** for the database and click **OK**</span></span>
3.  <span data-ttu-id="065a0-155">Появится диалоговое окно с вопросом о том, нужно ли создать новую базу данных, и нажать кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="065a0-155">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="065a0-156">Нажмите кнопку **Далее** , и мастер создания базы данных создаст язык описания данных (DDL) для создания базы данных. в диалоговом окне "Сводка и параметры" отобразится созданная DDL-Библиотека, которая не содержит определения для таблицы, сопоставленной с тип перечисления</span><span class="sxs-lookup"><span data-stu-id="065a0-156">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="065a0-157">Нажмите кнопку **Готово** , чтобы не выполнять скрипт DDL.</span><span class="sxs-lookup"><span data-stu-id="065a0-157">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="065a0-158">Мастер создания базы данных выполняет следующие действия. Открывает файл **университимодел. edmx. SQL** в редакторе T-SQL. создает схему хранилища и разделы СОПОСТАВЛЕНИЯ файла EDMX, добавляя сведения строки подключения к файлу app. config.</span><span class="sxs-lookup"><span data-stu-id="065a0-158">The Create Database Wizard does the following: Opens the **UniversityModel.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="065a0-159">Щелкните правой кнопкой мыши в редакторе T-SQL и выберите пункт **выполнить** диалоговое окно соединение с сервером, введите сведения о подключении на шаге 2 и нажмите кнопку **подключить** .</span><span class="sxs-lookup"><span data-stu-id="065a0-159">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="065a0-160">Чтобы просмотреть созданную схему, щелкните правой кнопкой мыши имя базы данных в обозреватель объектов SQL Server и выберите **Обновить** .</span><span class="sxs-lookup"><span data-stu-id="065a0-160">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="065a0-161">Сохранение и получение данных</span><span class="sxs-lookup"><span data-stu-id="065a0-161">Persist and Retrieve Data</span></span>

<span data-ttu-id="065a0-162">Откройте файл Program.cs, в котором определен метод Main.</span><span class="sxs-lookup"><span data-stu-id="065a0-162">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="065a0-163">Добавьте следующий код в функцию main.</span><span class="sxs-lookup"><span data-stu-id="065a0-163">Add the following code into the Main function.</span></span>

<span data-ttu-id="065a0-164">Код добавляет в контекст два новых объекта университета.</span><span class="sxs-lookup"><span data-stu-id="065a0-164">The code adds two new University objects to the context.</span></span> <span data-ttu-id="065a0-165">Пространственные свойства инициализируются с помощью метода заданное DbGeography. FromText.</span><span class="sxs-lookup"><span data-stu-id="065a0-165">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="065a0-166">Географическая точка, представленная как Веллкновнтекст, передается в метод.</span><span class="sxs-lookup"><span data-stu-id="065a0-166">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="065a0-167">Затем код сохраняет данные.</span><span class="sxs-lookup"><span data-stu-id="065a0-167">The code then saves the data.</span></span> <span data-ttu-id="065a0-168">Затем выполняется построение и выполнение запроса LINQ, который возвращает объект университета, где его расположение ближе всего к указанному расположению.</span><span class="sxs-lookup"><span data-stu-id="065a0-168">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

``` csharp
using (var context = new UniversityModelContainer())
{
    context.Universities.Add(new University()
    {
        Name = "Graphic Design Institute",
        Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
    });

    context.Universities.Add(new University()
    {
        Name = "School of Fine Art",
        Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
    });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                                orderby u.Location.Distance(myLocation)
                                select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

<span data-ttu-id="065a0-169">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="065a0-169">Compile and run the application.</span></span> <span data-ttu-id="065a0-170">Программа выдает следующие результаты.</span><span class="sxs-lookup"><span data-stu-id="065a0-170">The program produces the following output:</span></span>

```console
The closest University to you is: School of Fine Art.
```

<span data-ttu-id="065a0-171">Чтобы просмотреть данные в базе данных, щелкните правой кнопкой мыши имя базы данных в обозреватель объектов SQL Server и выберите **Обновить**.</span><span class="sxs-lookup"><span data-stu-id="065a0-171">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="065a0-172">Затем щелкните правой кнопкой мыши таблицу и выберите **Просмотр данных**.</span><span class="sxs-lookup"><span data-stu-id="065a0-172">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="065a0-173">Сводка</span><span class="sxs-lookup"><span data-stu-id="065a0-173">Summary</span></span>

<span data-ttu-id="065a0-174">В этом пошаговом руководстве мы рассмотрели, как сопоставлять пространственные типы с помощью Entity Framework Designer и как использовать пространственные типы в коде.</span><span class="sxs-lookup"><span data-stu-id="065a0-174">In this walkthrough we looked at how to map spatial types using the Entity Framework Designer and how to use spatial types in code.</span></span> 
