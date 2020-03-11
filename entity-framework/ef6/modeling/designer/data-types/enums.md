---
title: Поддержка перечисления — конструктор EF-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: 92a763b84a04d3ce7ec0853ef2a4852356cf7997
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415355"
---
# <a name="enum-support---ef-designer"></a><span data-ttu-id="f852b-102">Поддержка перечисления-конструктор EF</span><span class="sxs-lookup"><span data-stu-id="f852b-102">Enum Support - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="f852b-103">**Только EF5** — функции, API и т. д., обсуждаемые на этой странице, появились в Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="f852b-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="f852b-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="f852b-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="f852b-105">В этом видео и пошаговом руководстве показано, как использовать типы Enum с Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="f852b-105">This video and step-by-step walkthrough shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="f852b-106">В нем также показано, как использовать перечисления в запросе LINQ.</span><span class="sxs-lookup"><span data-stu-id="f852b-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="f852b-107">В этом пошаговом руководстве будет использоваться Model First для создания новой базы данных, но конструктор EF также можно использовать с рабочим процессом [Database First](~/ef6/modeling/designer/workflows/database-first.md) для соотнесения с существующей базой данных.</span><span class="sxs-lookup"><span data-stu-id="f852b-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="f852b-108">Поддержка перечисления появилась в Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="f852b-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="f852b-109">Чтобы использовать новые функции, такие как перечисления, пространственные типы данных и функции, возвращающие табличное значение, необходимо выбрать .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="f852b-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="f852b-110">Visual Studio 2012 нацелена на .NET 4,5 по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f852b-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="f852b-111">В Entity Framework перечисление может иметь следующие базовые типы: **Byte**, **Int16**, **Int32**, **Int64** или **SByte**.</span><span class="sxs-lookup"><span data-stu-id="f852b-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="f852b-112">Просмотр видео</span><span class="sxs-lookup"><span data-stu-id="f852b-112">Watch the Video</span></span>
<span data-ttu-id="f852b-113">В этом видео показано, как использовать типы Enum с Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="f852b-113">This video shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="f852b-114">В нем также показано, как использовать перечисления в запросе LINQ.</span><span class="sxs-lookup"><span data-stu-id="f852b-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="f852b-115">**Представлено**: Julia Корнич</span><span class="sxs-lookup"><span data-stu-id="f852b-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="f852b-116">**Видео**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span><span class="sxs-lookup"><span data-stu-id="f852b-116">**Video**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="f852b-117">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="f852b-117">Pre-Requisites</span></span>

<span data-ttu-id="f852b-118">Для выполнения инструкций этого пошагового руководства необходимо установить Visual Studio 2012, Ultimate, Premium, Professional или Web Express Edition.</span><span class="sxs-lookup"><span data-stu-id="f852b-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="f852b-119">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="f852b-119">Set up the Project</span></span>

1.  <span data-ttu-id="f852b-120">Откройте Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="f852b-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="f852b-121">В меню **файл** наведите указатель мыши на пункт **создать**и выберите **проект** .</span><span class="sxs-lookup"><span data-stu-id="f852b-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="f852b-122">В левой области щелкните **Visual C\#** , а затем выберите шаблон **консоли** .</span><span class="sxs-lookup"><span data-stu-id="f852b-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="f852b-123">Введите **енумефдесигнер** в качестве имени проекта и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="f852b-123">Enter **EnumEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="f852b-124">Создание новой модели с помощью конструктора EF</span><span class="sxs-lookup"><span data-stu-id="f852b-124">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="f852b-125">Щелкните правой кнопкой мыши имя проекта в обозреватель решений, наведите указатель на пункт **Добавить**и выберите пункт **новый элемент** .</span><span class="sxs-lookup"><span data-stu-id="f852b-125">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="f852b-126">Выберите **данные** в меню слева, а затем выберите **ADO.NET EDM** в области Шаблоны.</span><span class="sxs-lookup"><span data-stu-id="f852b-126">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="f852b-127">Введите **енумтестмодел. EDMX** в поле имя файла и нажмите кнопку **Добавить** .</span><span class="sxs-lookup"><span data-stu-id="f852b-127">Enter **EnumTestModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="f852b-128">На странице "Мастер EDM" в диалоговом окне Выбор содержимого модели выберите **Пустая модель** .</span><span class="sxs-lookup"><span data-stu-id="f852b-128">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="f852b-129">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="f852b-129">Click **Finish**</span></span>

<span data-ttu-id="f852b-130">Отобразится Entity Designer, предоставляющий область конструктора для редактирования модели.</span><span class="sxs-lookup"><span data-stu-id="f852b-130">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="f852b-131">Мастер выполняет следующие действия.</span><span class="sxs-lookup"><span data-stu-id="f852b-131">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="f852b-132">Создает файл Енумтестмодел. EDMX, который определяет концептуальную модель, модель хранения и сопоставление между ними.</span><span class="sxs-lookup"><span data-stu-id="f852b-132">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="f852b-133">Задает свойство обработки артефакта метаданных файла EDMX, которое будет внедрено в выходную сборку, чтобы созданные файлы метаданных были внедрены в сборку.</span><span class="sxs-lookup"><span data-stu-id="f852b-133">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="f852b-134">Добавляет ссылку на следующие сборки: EntityFramework, System. ComponentModel. Аннотация и System. Data. Entity.</span><span class="sxs-lookup"><span data-stu-id="f852b-134">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="f852b-135">Создает файлы EnumTestModel.tt и EnumTestModel.Context.tt и добавляет их в EDMX файл.</span><span class="sxs-lookup"><span data-stu-id="f852b-135">Creates EnumTestModel.tt and EnumTestModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="f852b-136">Эти файлы шаблонов T4 создают код, определяющий производный тип DbContext и типы POCO, которые сопоставляются с сущностями в модели EDMX.</span><span class="sxs-lookup"><span data-stu-id="f852b-136">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model.</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="f852b-137">Добавить новый тип сущности</span><span class="sxs-lookup"><span data-stu-id="f852b-137">Add a New Entity Type</span></span>

1.  <span data-ttu-id="f852b-138">Щелкните правой кнопкой мыши пустую область в области конструктора и выберите **Добавить-&gt; сущность**, откроется диалоговое окно Новая сущность.</span><span class="sxs-lookup"><span data-stu-id="f852b-138">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="f852b-139">Укажите **Отдел** для имени типа и укажите **DepartmentID** в качестве имени ключевого свойства, оставьте тип **Int32** .</span><span class="sxs-lookup"><span data-stu-id="f852b-139">Specify **Department** for the type name and specify **DepartmentID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="f852b-140">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="f852b-140">Click **OK**</span></span>
4.  <span data-ttu-id="f852b-141">Щелкните сущность правой кнопкой мыши и выберите пункт **Добавить новое-&gt; скалярное свойство** .</span><span class="sxs-lookup"><span data-stu-id="f852b-141">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="f852b-142">Переименовать новое свойство в **имя**</span><span class="sxs-lookup"><span data-stu-id="f852b-142">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="f852b-143">Измените тип нового свойства на **Int32** (по умолчанию новое свойство имеет значение типа String), чтобы изменить тип, откройте окно свойств и измените свойство Type на **Int32** .</span><span class="sxs-lookup"><span data-stu-id="f852b-143">Change the type of the new property to **Int32** (by default, the new property is of String type) To change the type, open the Properties window and change the Type property to **Int32**</span></span>
7.  <span data-ttu-id="f852b-144">Добавьте еще одно скалярное свойство и переименуйте его в **бюджет**, измените тип на **Decimal** .</span><span class="sxs-lookup"><span data-stu-id="f852b-144">Add another scalar property and rename it to **Budget**, change the type to **Decimal**</span></span>

## <a name="add-an-enum-type"></a><span data-ttu-id="f852b-145">Добавление типа перечисления</span><span class="sxs-lookup"><span data-stu-id="f852b-145">Add an Enum Type</span></span>

1.  <span data-ttu-id="f852b-146">В Entity Framework Designer щелкните правой кнопкой мыши свойство имя и выберите **преобразовать в перечисление** .</span><span class="sxs-lookup"><span data-stu-id="f852b-146">In the Entity Framework Designer, right-click the Name property, select **Convert to enum**</span></span>

    ![Преобразовать в перечисление](~/ef6/media/converttoenum.png)

2.  <span data-ttu-id="f852b-148">В диалоговом окне **Добавление перечисления** введите **Департментнамес** в поле имя типа Enum, измените базовый тип на **Int32**, а затем добавьте следующие члены в тип: Английский, математика и экономические.</span><span class="sxs-lookup"><span data-stu-id="f852b-148">In the **Add Enum** dialog box type **DepartmentNames** for the Enum Type Name, change the Underlying Type to **Int32**, and then add the following members to the type: English, Math, and Economics</span></span>

    ![Добавить тип перечисления](~/ef6/media/addenumtype.png)

3.  <span data-ttu-id="f852b-150">Нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="f852b-150">Press **OK**</span></span>
4.  <span data-ttu-id="f852b-151">Сохранение модели и сборка проекта</span><span class="sxs-lookup"><span data-stu-id="f852b-151">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="f852b-152">При сборке в Список ошибок могут отображаться предупреждения о несопоставленных сущностях и ассоциациях.</span><span class="sxs-lookup"><span data-stu-id="f852b-152">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="f852b-153">Эти предупреждения можно игнорировать, так как после выбора создания базы данных из модели ошибки будут исчезнуть.</span><span class="sxs-lookup"><span data-stu-id="f852b-153">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

<span data-ttu-id="f852b-154">Если взглянуть на окно свойств, вы увидите, что тип свойства Name был изменен на **департментнамес** , а вновь добавленный тип перечисления был добавлен в список типов.</span><span class="sxs-lookup"><span data-stu-id="f852b-154">If you look at the Properties window, you will notice that the type of the Name property was changed to **DepartmentNames** and the newly added enum type was added to the list of types.</span></span>

<span data-ttu-id="f852b-155">При переключении в окно обозревателя моделей вы увидите, что тип также был добавлен в узел Типы перечислений.</span><span class="sxs-lookup"><span data-stu-id="f852b-155">If you switch to the Model Browser window, you will see that the type was also added to the Enum Types node.</span></span>

![Обозреватель моделей](~/ef6/media/modelbrowser.png)

>[!NOTE]
> <span data-ttu-id="f852b-157">Можно также добавить новые типы перечислений из этого окна, щелкнув правой кнопкой мыши и выбрав **Добавить тип перечисления**.</span><span class="sxs-lookup"><span data-stu-id="f852b-157">You can also add new enum types from this window by clicking the right mouse button and selecting **Add Enum Type**.</span></span> <span data-ttu-id="f852b-158">После создания тип появится в списке типов, и вы сможете связать его со свойством.</span><span class="sxs-lookup"><span data-stu-id="f852b-158">Once the type is created it will appear in the list of types and you would be able to associate with a property</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="f852b-159">Создать базу данных из модели</span><span class="sxs-lookup"><span data-stu-id="f852b-159">Generate Database from Model</span></span>

<span data-ttu-id="f852b-160">Теперь можно создать базу данных, основанную на модели.</span><span class="sxs-lookup"><span data-stu-id="f852b-160">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="f852b-161">Щелкните правой кнопкой мыши пустое пространство на поверхности Entity Designer и выберите пункт **создать базу данных из модели** .</span><span class="sxs-lookup"><span data-stu-id="f852b-161">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="f852b-162">Откроется диалоговое окно Выбор подключения к данным мастера создания базы данных. Нажмите кнопку **создать подключение** , укажите **(LocalDB)\\mssqllocaldb** для имени сервера и **енумтест** для базы данных и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="f852b-162">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **EnumTest** for the database and click **OK**</span></span>
3.  <span data-ttu-id="f852b-163">Появится диалоговое окно с вопросом о том, нужно ли создать новую базу данных, и нажать кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="f852b-163">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="f852b-164">Нажмите кнопку **Далее** , и мастер создания базы данных создаст язык описания данных (DDL) для создания базы данных. в диалоговом окне "Сводка и параметры" отображается созданная DDL-инструкция. Обратите внимание, что в DDL не содержится определение таблицы, сопоставленной с типом перечисления.</span><span class="sxs-lookup"><span data-stu-id="f852b-164">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="f852b-165">Нажмите кнопку **Готово** , чтобы не выполнять скрипт DDL.</span><span class="sxs-lookup"><span data-stu-id="f852b-165">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="f852b-166">Мастер создания базы данных выполняет следующие действия: открывает файл **енумтест. edmx. SQL** в редакторе T-SQL, который создает схему хранилища и разделы сопоставления EDMX-файла, добавляя сведения строки подключения к файлу app. config.</span><span class="sxs-lookup"><span data-stu-id="f852b-166">The Create Database Wizard does the following: Opens the **EnumTest.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="f852b-167">Щелкните правой кнопкой мыши в редакторе T-SQL и выберите пункт **выполнить** диалоговое окно соединение с сервером, введите сведения о подключении на шаге 2 и нажмите кнопку **подключить** .</span><span class="sxs-lookup"><span data-stu-id="f852b-167">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="f852b-168">Чтобы просмотреть созданную схему, щелкните правой кнопкой мыши имя базы данных в обозреватель объектов SQL Server и выберите **Обновить** .</span><span class="sxs-lookup"><span data-stu-id="f852b-168">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="f852b-169">Сохранение и получение данных</span><span class="sxs-lookup"><span data-stu-id="f852b-169">Persist and Retrieve Data</span></span>

<span data-ttu-id="f852b-170">Откройте файл Program.cs, в котором определен метод Main.</span><span class="sxs-lookup"><span data-stu-id="f852b-170">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="f852b-171">Добавьте следующий код в функцию main.</span><span class="sxs-lookup"><span data-stu-id="f852b-171">Add the following code into the Main function.</span></span> <span data-ttu-id="f852b-172">Код добавляет новый объект Department в контекст.</span><span class="sxs-lookup"><span data-stu-id="f852b-172">The code adds a new Department object to the context.</span></span> <span data-ttu-id="f852b-173">Затем данные сохраняются.</span><span class="sxs-lookup"><span data-stu-id="f852b-173">It then saves the data.</span></span> <span data-ttu-id="f852b-174">Код также выполняет запрос LINQ, возвращающий отдел, имя которого — Департментнамес. English.</span><span class="sxs-lookup"><span data-stu-id="f852b-174">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

``` csharp
using (var context = new EnumTestModelContainer())
{
    context.Departments.Add(new Department{ Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} and Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

<span data-ttu-id="f852b-175">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="f852b-175">Compile and run the application.</span></span> <span data-ttu-id="f852b-176">Программа выдает следующие результаты.</span><span class="sxs-lookup"><span data-stu-id="f852b-176">The program produces the following output:</span></span>

```console
DepartmentID: 1 Name: English
```

<span data-ttu-id="f852b-177">Чтобы просмотреть данные в базе данных, щелкните правой кнопкой мыши имя базы данных в обозреватель объектов SQL Server и выберите **Обновить**.</span><span class="sxs-lookup"><span data-stu-id="f852b-177">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="f852b-178">Затем щелкните правой кнопкой мыши таблицу и выберите **Просмотр данных**.</span><span class="sxs-lookup"><span data-stu-id="f852b-178">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="f852b-179">Сводка</span><span class="sxs-lookup"><span data-stu-id="f852b-179">Summary</span></span>

<span data-ttu-id="f852b-180">В этом пошаговом руководстве мы рассмотрели, как сопоставлять перечисляемые типы с помощью Entity Framework Designer и как использовать перечисления в коде.</span><span class="sxs-lookup"><span data-stu-id="f852b-180">In this walkthrough we looked at how to map enum types using the Entity Framework Designer and how to use enums in code.</span></span> 
