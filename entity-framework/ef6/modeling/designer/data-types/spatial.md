---
title: Конструктор пространственного-EF-EF6
description: Конструктор пространственного-EF в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/designer/data-types/spatial
ms.openlocfilehash: 298666e558ad5bb531948e6a0bbcf3d71141df0b
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92064917"
---
# <a name="spatial---ef-designer"></a><span data-ttu-id="5d7a9-103">Конструктор пространственного-EF</span><span class="sxs-lookup"><span data-stu-id="5d7a9-103">Spatial - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="5d7a9-104">**Только EF5** — функции, API и т. д., обсуждаемые на этой странице, появились в Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-104">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="5d7a9-105">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-105">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="5d7a9-106">В видеоролике и пошаговом руководстве показано, как сопоставлять пространственные типы с Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-106">The video and step-by-step walkthrough shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="5d7a9-107">В нем также показано, как использовать запрос LINQ для поиска расстояния между двумя расположениями.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-107">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="5d7a9-108">В этом пошаговом руководстве будет использоваться Model First для создания новой базы данных, но конструктор EF также можно использовать с рабочим процессом [Database First](xref:ef6/modeling/designer/workflows/database-first) для соотнесения с существующей базой данных.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-108">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](xref:ef6/modeling/designer/workflows/database-first) workflow to map to an existing database.</span></span>

<span data-ttu-id="5d7a9-109">Поддержка пространственного типа появилась в Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-109">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="5d7a9-110">Обратите внимание, что для использования новых функций, таких как пространственный тип, перечисления и функции с табличным значением, необходимо выбрать .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-110">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="5d7a9-111">Visual Studio 2012 нацелена на .NET 4,5 по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-111">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="5d7a9-112">Чтобы использовать пространственные типы данных, необходимо также использовать поставщик Entity Framework с поддержкой пространственного доступа.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-112">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="5d7a9-113">Дополнительные сведения см. в разделе [Поддержка пространственных типов в поставщике](xref:ef6/fundamentals/providers/spatial-support) .</span><span class="sxs-lookup"><span data-stu-id="5d7a9-113">See [provider support for spatial types](xref:ef6/fundamentals/providers/spatial-support) for more information.</span></span>

<span data-ttu-id="5d7a9-114">Существует два основных типа пространственных данных: geography и geometry.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-114">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="5d7a9-115">Тип данных geography хранит данные эллиптических (например, координаты широты и долготы GPS).</span><span class="sxs-lookup"><span data-stu-id="5d7a9-115">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="5d7a9-116">Тип данных geometry представляет евклидово (плоскую) систему координат.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-116">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="5d7a9-117">Просмотреть видео</span><span class="sxs-lookup"><span data-stu-id="5d7a9-117">Watch the video</span></span>
<span data-ttu-id="5d7a9-118">В этом видео показано, как сопоставлять пространственные типы с Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-118">This video shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="5d7a9-119">В нем также показано, как использовать запрос LINQ для поиска расстояния между двумя расположениями.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-119">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="5d7a9-120">**Представлено**: Julia Корнич</span><span class="sxs-lookup"><span data-stu-id="5d7a9-120">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="5d7a9-121">**Видео**: [WMV](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv)  |  [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v)  |  [WMV (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span><span class="sxs-lookup"><span data-stu-id="5d7a9-121">**Video**: [WMV](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="5d7a9-122">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="5d7a9-122">Pre-Requisites</span></span>

<span data-ttu-id="5d7a9-123">Для выполнения инструкций этого пошагового руководства необходимо установить Visual Studio 2012, Ultimate, Premium, Professional или Web Express Edition.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-123">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="5d7a9-124">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="5d7a9-124">Set up the Project</span></span>

1.  <span data-ttu-id="5d7a9-125">Откройте Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="5d7a9-125">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="5d7a9-126">В меню **файл** наведите указатель мыши на пункт **создать**и выберите **проект** .</span><span class="sxs-lookup"><span data-stu-id="5d7a9-126">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="5d7a9-127">В левой области щелкните \*\*Visual C \# \*\*, а затем выберите шаблон **консоли** .</span><span class="sxs-lookup"><span data-stu-id="5d7a9-127">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="5d7a9-128">Введите **спатиалефдесигнер** в качестве имени проекта и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="5d7a9-128">Enter **SpatialEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="5d7a9-129">Создание новой модели с помощью конструктора EF</span><span class="sxs-lookup"><span data-stu-id="5d7a9-129">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="5d7a9-130">Щелкните правой кнопкой мыши имя проекта в обозреватель решений, наведите указатель на пункт **Добавить**и выберите пункт **новый элемент** .</span><span class="sxs-lookup"><span data-stu-id="5d7a9-130">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="5d7a9-131">Выберите **данные** в меню слева, а затем выберите **ADO.NET EDM** в области Шаблоны.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-131">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="5d7a9-132">Введите **университимодел. EDMX** в поле имя файла и нажмите кнопку **Добавить** .</span><span class="sxs-lookup"><span data-stu-id="5d7a9-132">Enter **UniversityModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="5d7a9-133">На странице "Мастер EDM" в диалоговом окне Выбор содержимого модели выберите **Пустая модель** .</span><span class="sxs-lookup"><span data-stu-id="5d7a9-133">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="5d7a9-134">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-134">Click **Finish**</span></span>

<span data-ttu-id="5d7a9-135">Отобразится Entity Designer, предоставляющий область конструктора для редактирования модели.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-135">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="5d7a9-136">Мастер выполняет следующие действия.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-136">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="5d7a9-137">Создает файл Енумтестмодел. EDMX, который определяет концептуальную модель, модель хранения и сопоставление между ними.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-137">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="5d7a9-138">Задает свойство обработки артефакта метаданных файла EDMX, которое будет внедрено в выходную сборку, чтобы созданные файлы метаданных были внедрены в сборку.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-138">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="5d7a9-139">Добавляет ссылку на следующие сборки: EntityFramework, System. ComponentModel. Аннотация и System. Data. Entity.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-139">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="5d7a9-140">Создает файлы UniversityModel.tt и UniversityModel.Context.tt и добавляет их в EDMX файл.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-140">Creates UniversityModel.tt and UniversityModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="5d7a9-141">Эти файлы шаблонов T4 создают код, определяющий производный тип DbContext и типы POCO, которые сопоставляются с сущностями в модели EDMX.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-141">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="5d7a9-142">Добавить новый тип сущности</span><span class="sxs-lookup"><span data-stu-id="5d7a9-142">Add a New Entity Type</span></span>

1.  <span data-ttu-id="5d7a9-143">Щелкните правой кнопкой мыши пустую область в области конструктора, выберите пункт **Добавить- &gt; сущность**, откроется диалоговое окно Новая сущность.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-143">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="5d7a9-144">Укажите название типа **университет** и укажите **университид** в качестве имени свойства ключа, оставьте тип **Int32** .</span><span class="sxs-lookup"><span data-stu-id="5d7a9-144">Specify **University** for the type name and specify **UniversityID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="5d7a9-145">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-145">Click **OK**</span></span>
4.  <span data-ttu-id="5d7a9-146">Щелкните сущность правой кнопкой мыши и выберите пункт **Добавить новое — &gt; скалярное свойство** .</span><span class="sxs-lookup"><span data-stu-id="5d7a9-146">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="5d7a9-147">Переименовать новое свойство в **имя**</span><span class="sxs-lookup"><span data-stu-id="5d7a9-147">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="5d7a9-148">Добавьте еще одно скалярное свойство и переименуйте его в **Расположение** . Откройте окно свойств и измените тип нового свойства на **Geography** .</span><span class="sxs-lookup"><span data-stu-id="5d7a9-148">Add another scalar property and rename it to **Location** Open the Properties window and change the type of the new property to **Geography**</span></span>
7.  <span data-ttu-id="5d7a9-149">Сохранение модели и сборка проекта</span><span class="sxs-lookup"><span data-stu-id="5d7a9-149">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="5d7a9-150">При сборке в Список ошибок могут отображаться предупреждения о несопоставленных сущностях и ассоциациях.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-150">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="5d7a9-151">Эти предупреждения можно игнорировать, так как после выбора создания базы данных из модели ошибки будут исчезнуть.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-151">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="5d7a9-152">Создать базу данных из модели</span><span class="sxs-lookup"><span data-stu-id="5d7a9-152">Generate Database from Model</span></span>

<span data-ttu-id="5d7a9-153">Теперь можно создать базу данных, основанную на модели.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-153">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="5d7a9-154">Щелкните правой кнопкой мыши пустое пространство на поверхности Entity Designer и выберите пункт **создать базу данных из модели** .</span><span class="sxs-lookup"><span data-stu-id="5d7a9-154">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="5d7a9-155">Откроется диалоговое окно Выбор подключения к данным мастера создания базы данных. Нажмите кнопку **создать подключение** , укажите **(LocalDB) \\ mssqllocaldb** в поле имя сервера и **университет** для базы данных и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="5d7a9-155">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **University** for the database and click **OK**</span></span>
3.  <span data-ttu-id="5d7a9-156">Появится диалоговое окно с вопросом о том, нужно ли создать новую базу данных, и нажать кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-156">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="5d7a9-157">Нажмите кнопку **Далее** , и мастер создания базы данных создаст язык описания данных (DDL) для создания базы данных. в диалоговом окне "Сводка и параметры" отображается созданная DDL-инструкция. Обратите внимание, что в DDL не содержится определение таблицы, сопоставленной с типом перечисления.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-157">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="5d7a9-158">Нажмите кнопку **Готово** , чтобы не выполнять скрипт DDL.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-158">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="5d7a9-159">Мастер создания базы данных выполняет следующие действия: открывает файл **университимодел. edmx. SQL** в редакторе T-SQL. создает схему хранилища и разделы СОПОСТАВЛЕНИЯ файла EDMX, добавляя сведения строки подключения к файлу App.config</span><span class="sxs-lookup"><span data-stu-id="5d7a9-159">The Create Database Wizard does the following: Opens the **UniversityModel.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="5d7a9-160">Щелкните правой кнопкой мыши в редакторе T-SQL и выберите пункт **выполнить** диалоговое окно соединение с сервером, введите сведения о подключении на шаге 2 и нажмите кнопку **подключить** .</span><span class="sxs-lookup"><span data-stu-id="5d7a9-160">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="5d7a9-161">Чтобы просмотреть созданную схему, щелкните правой кнопкой мыши имя базы данных в обозреватель объектов SQL Server и выберите **Обновить** .</span><span class="sxs-lookup"><span data-stu-id="5d7a9-161">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="5d7a9-162">Сохранение и получение данных</span><span class="sxs-lookup"><span data-stu-id="5d7a9-162">Persist and Retrieve Data</span></span>

<span data-ttu-id="5d7a9-163">Откройте файл Program.cs, в котором определен метод Main.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-163">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="5d7a9-164">Добавьте следующий код в функцию main.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-164">Add the following code into the Main function.</span></span>

<span data-ttu-id="5d7a9-165">Код добавляет в контекст два новых объекта университета.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-165">The code adds two new University objects to the context.</span></span> <span data-ttu-id="5d7a9-166">Пространственные свойства инициализируются с помощью метода заданное DbGeography. FromText.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-166">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="5d7a9-167">Географическая точка, представленная как Веллкновнтекст, передается в метод.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-167">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="5d7a9-168">Затем код сохраняет данные.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-168">The code then saves the data.</span></span> <span data-ttu-id="5d7a9-169">Затем выполняется построение и выполнение запроса LINQ, который возвращает объект университета, где его расположение ближе всего к указанному расположению.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-169">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

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

<span data-ttu-id="5d7a9-170">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-170">Compile and run the application.</span></span> <span data-ttu-id="5d7a9-171">Программа выдает следующие результаты.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-171">The program produces the following output:</span></span>

```console
The closest University to you is: School of Fine Art.
```

<span data-ttu-id="5d7a9-172">Чтобы просмотреть данные в базе данных, щелкните правой кнопкой мыши имя базы данных в обозреватель объектов SQL Server и выберите **Обновить**.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-172">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="5d7a9-173">Затем щелкните правой кнопкой мыши таблицу и выберите **Просмотр данных**.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-173">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="5d7a9-174">Итоги</span><span class="sxs-lookup"><span data-stu-id="5d7a9-174">Summary</span></span>

<span data-ttu-id="5d7a9-175">В этом пошаговом руководстве мы рассмотрели, как сопоставлять пространственные типы с помощью Entity Framework Designer и как использовать пространственные типы в коде.</span><span class="sxs-lookup"><span data-stu-id="5d7a9-175">In this walkthrough we looked at how to map spatial types using the Entity Framework Designer and how to use spatial types in code.</span></span> 
