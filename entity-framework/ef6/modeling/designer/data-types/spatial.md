---
title: Пространственные - конструктор EF - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: 04ba6e8d4a59816ca31e831b8e466cb1152a3d1b
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490219"
---
# <a name="spatial---ef-designer"></a><span data-ttu-id="bfcc5-102">Пространственные - конструктор EF</span><span class="sxs-lookup"><span data-stu-id="bfcc5-102">Spatial - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="bfcc5-103">**EF5 и более поздних версий только** -функции, интерфейсы API, и т.д., описанных на этой странице появились в Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="bfcc5-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="bfcc5-105">Видео и пошаговые руководства показано, как сопоставить пространственных типов с помощью Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-105">The video and step-by-step walkthrough shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="bfcc5-106">Также показано, как использовать LINQ-запрос для поиска расстояние между двумя расположениями.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="bfcc5-107">В этом пошаговом руководстве будет использоваться для создания новой базы данных Model First, но конструктор EF может также использоваться с [Database First](~/ef6/modeling/designer/workflows/database-first.md) рабочий процесс для сопоставления существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="bfcc5-108">В Entity Framework 5 появилась поддержка пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="bfcc5-109">Обратите внимание на то, что чтобы использовать новые функции, например пространственного типа, перечислимые типы и функции, возвращающие табличные значения, необходимо ориентироваться .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="bfcc5-110">Visual Studio 2012 предназначенного для .NET 4.5 по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="bfcc5-111">Для использования пространственных типов данных необходимо также использовать с поставщиком Entity Framework с поддержкой пространственных.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="bfcc5-112">См. в разделе [поддержка пространственных типов](~/ef6/fundamentals/providers/spatial-support.md) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="bfcc5-113">Существует два типа основных пространственных данных: geography и geometry.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="bfcc5-114">Тип данных сохраняет эллиптические данные (например, GPS координаты широты и долготы).</span><span class="sxs-lookup"><span data-stu-id="bfcc5-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="bfcc5-115">Тип данных geometry представляет Евклидовой (плоской) системе координат.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="bfcc5-116">Просмотреть видео</span><span class="sxs-lookup"><span data-stu-id="bfcc5-116">Watch the video</span></span>
<span data-ttu-id="bfcc5-117">В этом видео показано, как сопоставить пространственных типов с помощью Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-117">This video shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="bfcc5-118">Также показано, как использовать LINQ-запрос для поиска расстояние между двумя расположениями.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="bfcc5-119">**Представленный**: Юлия Корнич</span><span class="sxs-lookup"><span data-stu-id="bfcc5-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="bfcc5-120">**Видео**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span><span class="sxs-lookup"><span data-stu-id="bfcc5-120">**Video**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="bfcc5-121">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="bfcc5-121">Pre-Requisites</span></span>

<span data-ttu-id="bfcc5-122">Необходимо будет установлен выпуск Visual Studio 2012 Ultimate, Premium, Professional и Web Express для выполнения этого пошагового руководства.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="bfcc5-123">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="bfcc5-123">Set up the Project</span></span>

1.  <span data-ttu-id="bfcc5-124">Откройте Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="bfcc5-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="bfcc5-125">На **файл** последовательно выберите пункты **New**, а затем нажмите кнопку **проекта**</span><span class="sxs-lookup"><span data-stu-id="bfcc5-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="bfcc5-126">В левой области щелкните **Visual C\#**, а затем выберите **консоли** шаблона</span><span class="sxs-lookup"><span data-stu-id="bfcc5-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="bfcc5-127">Введите **SpatialEFDesigner** как имя проекта и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="bfcc5-127">Enter **SpatialEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="bfcc5-128">Создание новой модели в конструкторе EF</span><span class="sxs-lookup"><span data-stu-id="bfcc5-128">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="bfcc5-129">Щелкните правой кнопкой мыши имя проекта в обозревателе решений, выберите пункт **добавить**, а затем нажмите кнопку **новый элемент**</span><span class="sxs-lookup"><span data-stu-id="bfcc5-129">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="bfcc5-130">Выберите **данных** меню слева, а затем выберите **ADO.NET Entity Data Model** в области шаблонов</span><span class="sxs-lookup"><span data-stu-id="bfcc5-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="bfcc5-131">Введите **UniversityModel.edmx** имя файла, а затем нажмите кнопку **добавить**</span><span class="sxs-lookup"><span data-stu-id="bfcc5-131">Enter **UniversityModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="bfcc5-132">На странице мастера моделей EDM, выберите **пустую модель** в диалоговом окне Выбор содержимого модели</span><span class="sxs-lookup"><span data-stu-id="bfcc5-132">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="bfcc5-133">Нажмите кнопку **Готово**</span><span class="sxs-lookup"><span data-stu-id="bfcc5-133">Click **Finish**</span></span>

<span data-ttu-id="bfcc5-134">Конструктор сущностей, который предоставляет область конструктора для изменения модели, отображается.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-134">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="bfcc5-135">Мастер выполняет следующие действия.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-135">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="bfcc5-136">Создает файл EnumTestModel.edmx, который определяет концептуальную модель, модель хранения и сопоставления между ними.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-136">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="bfcc5-137">Задает свойство обработка артефактов метаданных из EDMX-файл для внедрения в выходной сборки, поэтому созданные метаданные файлы внедряются в сборку.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-137">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="bfcc5-138">Добавляет ссылку на следующие сборки: EntityFramework, System.ComponentModel.DataAnnotations и System.Data.Entity.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-138">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="bfcc5-139">Создает файлы UniversityModel.tt и UniversityModel.Context.tt и добавляет их в EDMX-файл.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-139">Creates UniversityModel.tt and UniversityModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="bfcc5-140">Эти файлы шаблонов T4 создания кода, который определяет тип производным DbContext и типов POCO, которые сопоставляются с сущностями в модели .edmx</span><span class="sxs-lookup"><span data-stu-id="bfcc5-140">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="bfcc5-141">Добавить новый тип сущности</span><span class="sxs-lookup"><span data-stu-id="bfcc5-141">Add a New Entity Type</span></span>

1.  <span data-ttu-id="bfcc5-142">Щелкните правой кнопкой мыши пустой участок области конструктора, выберите **Add -&gt; сущности**, появится диалоговое окно новой сущности</span><span class="sxs-lookup"><span data-stu-id="bfcc5-142">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="bfcc5-143">Укажите **университета** для типа имя и указать **UniversityID** оставьте тот тип, что имя ключевого свойства **Int32**</span><span class="sxs-lookup"><span data-stu-id="bfcc5-143">Specify **University** for the type name and specify **UniversityID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="bfcc5-144">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-144">Click **OK**</span></span>
4.  <span data-ttu-id="bfcc5-145">Щелкните правой кнопкой мыши сущность и выберите **Add New -&gt; скалярное свойство**</span><span class="sxs-lookup"><span data-stu-id="bfcc5-145">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="bfcc5-146">Переименовать новое свойство **имя**</span><span class="sxs-lookup"><span data-stu-id="bfcc5-146">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="bfcc5-147">Добавить еще одно скалярное свойство и переименуйте его в **расположение** откройте окно свойств и измените тип нового свойства для **Geography**</span><span class="sxs-lookup"><span data-stu-id="bfcc5-147">Add another scalar property and rename it to **Location** Open the Properties window and change the type of the new property to **Geography**</span></span>
7.  <span data-ttu-id="bfcc5-148">Сохранение модели и построение проекта</span><span class="sxs-lookup"><span data-stu-id="bfcc5-148">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="bfcc5-149">При создании, предупреждения о несопоставленных сущностях и ассоциациях, появляются в списке ошибок.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-149">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="bfcc5-150">Эти предупреждения можно игнорировать, поскольку после мы решили создать базу данных из модели, пропадут.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-150">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="bfcc5-151">Создание базы данных на основе модели</span><span class="sxs-lookup"><span data-stu-id="bfcc5-151">Generate Database from Model</span></span>

<span data-ttu-id="bfcc5-152">Теперь мы сможем создать базу данных, основанный на модели.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-152">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="bfcc5-153">Щелкните правой кнопкой мыши пустое место в области конструктора сущностей и выберите пункт **создать базу данных из модели**</span><span class="sxs-lookup"><span data-stu-id="bfcc5-153">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="bfcc5-154">Диалогового окна соединения данных из мастера создания базы данных является выбор отображаемых щелкните **новое подключение** кнопку укажите **(localdb)\\mssqllocaldb** для имени сервера и  **Университет** базы данных и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="bfcc5-154">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **University** for the database and click **OK**</span></span>
3.  <span data-ttu-id="bfcc5-155">Диалоговое окно с запросом, если вы хотите создать новую базу данных будет всплывающее окно, нажмите кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-155">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="bfcc5-156">Нажмите кнопку **Далее** и мастер создания базы данных приводит к возникновению ошибки языка описания данных (DDL) для создания базы данных, сформированный код DDL отображается сводка и параметры диалогового окна поле Обратите внимание, что, DDL не содержит определение для таблицу, которая сопоставляется с типом перечисления</span><span class="sxs-lookup"><span data-stu-id="bfcc5-156">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="bfcc5-157">Нажмите кнопку **Готово** щелкнув Готово не выполняет скрипт DDL.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-157">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="bfcc5-158">Мастер создания базы данных выполняет следующие: открывает **UniversityModel.edmx.sql** в редакторе T-SQL формирует разделы схемы и сопоставления хранилища EDMX-файла файл строку подключения Adds в файл App.config</span><span class="sxs-lookup"><span data-stu-id="bfcc5-158">The Create Database Wizard does the following: Opens the **UniversityModel.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="bfcc5-159">Щелкните правой кнопкой мыши в редакторе T-SQL и выберите **Execute** The Server диалоговое окно подключения к откроется, введите сведения о соединении из шага 2 и нажмите кнопку **Connect**</span><span class="sxs-lookup"><span data-stu-id="bfcc5-159">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="bfcc5-160">Чтобы просмотреть созданную схему, щелкните правой кнопкой мыши имя базы данных в обозревателе объектов SQL Server и выберите **обновления**</span><span class="sxs-lookup"><span data-stu-id="bfcc5-160">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="bfcc5-161">Сохранения и извлечения данных</span><span class="sxs-lookup"><span data-stu-id="bfcc5-161">Persist and Retrieve Data</span></span>

<span data-ttu-id="bfcc5-162">Откройте файл Program.cs, в котором определен метод Main.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-162">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="bfcc5-163">Добавьте следующий код в функцию Main.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-163">Add the following code into the Main function.</span></span>

<span data-ttu-id="bfcc5-164">Код добавляет два новых университета объектов к контексту.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-164">The code adds two new University objects to the context.</span></span> <span data-ttu-id="bfcc5-165">Пространственные свойства инициализируются с помощью метода DbGeography.FromText.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-165">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="bfcc5-166">Географическая точка представить в виде WellKnownText передается в метод.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-166">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="bfcc5-167">Затем код сохраняет данные.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-167">The code then saves the data.</span></span> <span data-ttu-id="bfcc5-168">Затем запрос LINQ, который возвращает объект университета, где она будет находиться в указанное расположение, ближайшее составлением и выполнением.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-168">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

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

<span data-ttu-id="bfcc5-169">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-169">Compile and run the application.</span></span> <span data-ttu-id="bfcc5-170">Программа выдает следующие результаты.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-170">The program produces the following output:</span></span>

```
The closest University to you is: School of Fine Art.
```

<span data-ttu-id="bfcc5-171">Чтобы просмотреть данные в базе данных, щелкните правой кнопкой мыши имя базы данных в обозревателе объектов SQL Server и выберите **обновить**.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-171">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="bfcc5-172">Щелкните правой кнопкой мыши таблицу и выберите **данные представления**.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-172">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="bfcc5-173">Сводка</span><span class="sxs-lookup"><span data-stu-id="bfcc5-173">Summary</span></span>

<span data-ttu-id="bfcc5-174">В этом пошаговом руководстве мы рассмотрели способ сопоставления пространственных типов, с помощью Entity Framework Designer и использовании пространственных типов в коде.</span><span class="sxs-lookup"><span data-stu-id="bfcc5-174">In this walkthrough we looked at how to map spatial types using the Entity Framework Designer and how to use spatial types in code.</span></span> 
