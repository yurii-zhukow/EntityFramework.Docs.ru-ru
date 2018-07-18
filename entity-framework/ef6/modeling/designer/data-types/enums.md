---
title: Поддержка Enum - конструктор EF - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
caps.latest.revision: 3
ms.openlocfilehash: cbf9b01fcbe21274ff3644c6ae6bc8fdfd338e3b
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122585"
---
# <a name="enum-support---ef-designer"></a><span data-ttu-id="6577f-102">Поддержка перечислений — конструктор EF</span><span class="sxs-lookup"><span data-stu-id="6577f-102">Enum Support - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="6577f-103">**EF5 и более поздних версий только** -функции, интерфейсы API, и т.д., описанных на этой странице появились в Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="6577f-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="6577f-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="6577f-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="6577f-105">Это видео и пошаговые пошаговом руководстве показано, как использовать типы перечисления с Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="6577f-105">This video and step-by-step walkthrough shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="6577f-106">Также демонстрируется использование перечислений в запросе LINQ.</span><span class="sxs-lookup"><span data-stu-id="6577f-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="6577f-107">В этом пошаговом руководстве будет использоваться для создания новой базы данных Model First, но конструктор EF может также использоваться с [Database First](~/ef6/modeling/designer/workflows/database-first.md) рабочий процесс для сопоставления существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="6577f-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="6577f-108">В Entity Framework 5 добавлена поддержка перечисления.</span><span class="sxs-lookup"><span data-stu-id="6577f-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="6577f-109">Чтобы использовать новые функции, например перечисления, Пространственные типы данных и функции, возвращающие табличные значения, необходимо ориентироваться .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="6577f-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="6577f-110">Visual Studio 2012 предназначенного для .NET 4.5 по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6577f-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="6577f-111">В Entity Framework, что перечисление может иметь следующие базовые типы: **байтов**, **Int16**, **Int32**, **Int64** , или **SByte**.</span><span class="sxs-lookup"><span data-stu-id="6577f-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="6577f-112">Просмотреть видео</span><span class="sxs-lookup"><span data-stu-id="6577f-112">Watch the Video</span></span>
<span data-ttu-id="6577f-113">В этом видео показано, как использовать типы перечисления с Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="6577f-113">This video shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="6577f-114">Также демонстрируется использование перечислений в запросе LINQ.</span><span class="sxs-lookup"><span data-stu-id="6577f-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="6577f-115">**Представленный**: Юлия Корнич</span><span class="sxs-lookup"><span data-stu-id="6577f-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="6577f-116">**Видео**: [WMV](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span><span class="sxs-lookup"><span data-stu-id="6577f-116">**Video**: [WMV](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="6577f-117">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="6577f-117">Pre-Requisites</span></span>

<span data-ttu-id="6577f-118">Необходимо будет установлен выпуск Visual Studio 2012 Ultimate, Premium, Professional и Web Express для выполнения этого пошагового руководства.</span><span class="sxs-lookup"><span data-stu-id="6577f-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="6577f-119">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="6577f-119">Set up the Project</span></span>

1.  <span data-ttu-id="6577f-120">Откройте Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="6577f-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="6577f-121">На **файл** последовательно выберите пункты **New**, а затем нажмите кнопку **проекта**</span><span class="sxs-lookup"><span data-stu-id="6577f-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="6577f-122">В левой области щелкните **Visual C\#**, а затем выберите **консоли** шаблона</span><span class="sxs-lookup"><span data-stu-id="6577f-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="6577f-123">Введите **EnumEFDesigner** как имя проекта и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="6577f-123">Enter **EnumEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="6577f-124">Создание новой модели в конструкторе EF</span><span class="sxs-lookup"><span data-stu-id="6577f-124">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="6577f-125">Щелкните правой кнопкой мыши имя проекта в обозревателе решений, выберите пункт **добавить**, а затем нажмите кнопку **новый элемент**</span><span class="sxs-lookup"><span data-stu-id="6577f-125">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="6577f-126">Выберите **данных** меню слева, а затем выберите **ADO.NET Entity Data Model** в области шаблонов</span><span class="sxs-lookup"><span data-stu-id="6577f-126">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="6577f-127">Введите **EnumTestModel.edmx** имя файла, а затем нажмите кнопку **добавить**</span><span class="sxs-lookup"><span data-stu-id="6577f-127">Enter **EnumTestModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="6577f-128">На странице мастера моделей EDM, выберите **пустую модель** в диалоговом окне Выбор содержимого модели</span><span class="sxs-lookup"><span data-stu-id="6577f-128">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="6577f-129">Нажмите кнопку **Готово**</span><span class="sxs-lookup"><span data-stu-id="6577f-129">Click **Finish**</span></span>

<span data-ttu-id="6577f-130">Конструктор сущностей, который предоставляет область конструктора для изменения модели, отображается.</span><span class="sxs-lookup"><span data-stu-id="6577f-130">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="6577f-131">Мастер выполняет следующие действия.</span><span class="sxs-lookup"><span data-stu-id="6577f-131">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="6577f-132">Создает файл EnumTestModel.edmx, который определяет концептуальную модель, модель хранения и сопоставления между ними.</span><span class="sxs-lookup"><span data-stu-id="6577f-132">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="6577f-133">Задает свойство обработка артефактов метаданных из EDMX-файл для внедрения в выходной сборки, поэтому созданные метаданные файлы внедряются в сборку.</span><span class="sxs-lookup"><span data-stu-id="6577f-133">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="6577f-134">Добавляет ссылку на следующие сборки: EntityFramework, System.ComponentModel.DataAnnotations и System.Data.Entity.</span><span class="sxs-lookup"><span data-stu-id="6577f-134">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="6577f-135">Создает файлы EnumTestModel.tt и EnumTestModel.Context.tt и добавляет их в EDMX-файл.</span><span class="sxs-lookup"><span data-stu-id="6577f-135">Creates EnumTestModel.tt and EnumTestModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="6577f-136">Эти файлы шаблонов T4 создавать код, который определяет тип производным DbContext и типов POCO, которые сопоставляются с сущностями в модели .edmx.</span><span class="sxs-lookup"><span data-stu-id="6577f-136">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model.</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="6577f-137">Добавить новый тип сущности</span><span class="sxs-lookup"><span data-stu-id="6577f-137">Add a New Entity Type</span></span>

1.  <span data-ttu-id="6577f-138">Щелкните правой кнопкой мыши пустой участок области конструктора, выберите **Add -&gt; сущности**, появится диалоговое окно новой сущности</span><span class="sxs-lookup"><span data-stu-id="6577f-138">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="6577f-139">Укажите **отдел** для типа имя и указать **DepartmentID** оставьте тот тип, что имя ключевого свойства **Int32**</span><span class="sxs-lookup"><span data-stu-id="6577f-139">Specify **Department** for the type name and specify **DepartmentID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="6577f-140">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6577f-140">Click **OK**</span></span>
4.  <span data-ttu-id="6577f-141">Щелкните правой кнопкой мыши сущность и выберите **Add New -&gt; скалярное свойство**</span><span class="sxs-lookup"><span data-stu-id="6577f-141">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="6577f-142">Переименовать новое свойство **имя**</span><span class="sxs-lookup"><span data-stu-id="6577f-142">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="6577f-143">Измените тип нового свойства для **Int32** (по умолчанию свойство имеет строковый тип) чтобы изменить тип, откройте окно свойств и измените свойство Type для **Int32**</span><span class="sxs-lookup"><span data-stu-id="6577f-143">Change the type of the new property to **Int32** (by default, the new property is of String type) To change the type, open the Properties window and change the Type property to **Int32**</span></span>
7.  <span data-ttu-id="6577f-144">Добавить еще одно скалярное свойство и переименуйте его в **бюджета**, измените тип на **Decimal**</span><span class="sxs-lookup"><span data-stu-id="6577f-144">Add another scalar property and rename it to **Budget**, change the type to **Decimal**</span></span>

## <a name="add-an-enum-type"></a><span data-ttu-id="6577f-145">Добавьте тип перечисления</span><span class="sxs-lookup"><span data-stu-id="6577f-145">Add an Enum Type</span></span>

1.  <span data-ttu-id="6577f-146">В конструкторе Entity Framework, щелкните правой кнопкой мыши свойство Name, выберите **преобразовать перечисление**</span><span class="sxs-lookup"><span data-stu-id="6577f-146">In the Entity Framework Designer, right-click the Name property, select **Convert to enum**</span></span>

    ![ConvertToEnum](~/ef6/media/converttoenum.png)

2.  <span data-ttu-id="6577f-148">В **перечислить** тип диалогового окна **DepartmentNames** для именем типа перечисления, изменить базовый тип для **Int32**, а затем добавьте следующие элементы в тип: английского языка, Математические и экономики</span><span class="sxs-lookup"><span data-stu-id="6577f-148">In the **Add Enum** dialog box type **DepartmentNames** for the Enum Type Name, change the Underlying Type to **Int32**, and then add the following members to the type: English, Math, and Economics</span></span>

    ![AddEnumType](~/ef6/media/addenumtype.png)

3.  <span data-ttu-id="6577f-150">Нажмите клавишу **ОК**</span><span class="sxs-lookup"><span data-stu-id="6577f-150">Press **OK**</span></span>
4.  <span data-ttu-id="6577f-151">Сохранение модели и построение проекта</span><span class="sxs-lookup"><span data-stu-id="6577f-151">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="6577f-152">При создании, предупреждения о несопоставленных сущностях и ассоциациях, появляются в списке ошибок.</span><span class="sxs-lookup"><span data-stu-id="6577f-152">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="6577f-153">Эти предупреждения можно игнорировать, поскольку после мы решили создать базу данных из модели, пропадут.</span><span class="sxs-lookup"><span data-stu-id="6577f-153">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

<span data-ttu-id="6577f-154">Если взглянуть на окне «Свойства», можно заметить, что тип свойства имя было изменено на **DepartmentNames** и новые перечисляемого типа была добавлена в список типов.</span><span class="sxs-lookup"><span data-stu-id="6577f-154">If you look at the Properties window, you will notice that the type of the Name property was changed to **DepartmentNames** and the newly added enum type was added to the list of types.</span></span>

<span data-ttu-id="6577f-155">При переключении в окно браузера модели, вы увидите сообщение о том, что тип также был добавлен к узлу для перечислимых типов.</span><span class="sxs-lookup"><span data-stu-id="6577f-155">If you switch to the Model Browser window, you will see that the type was also added to the Enum Types node.</span></span>

![ModelBrowser](~/ef6/media/modelbrowser.png)

>[!NOTE]
> <span data-ttu-id="6577f-157">Можно также добавить новые типы перечисления из этого окна, щелкнув правой кнопкой мыши и выбрав **добавить тип перечисления**.</span><span class="sxs-lookup"><span data-stu-id="6577f-157">You can also add new enum types from this window by clicking the right mouse button and selecting **Add Enum Type**.</span></span> <span data-ttu-id="6577f-158">После создания типа, он будет отображаться в списке типов, и вы смогли бы связать со свойством</span><span class="sxs-lookup"><span data-stu-id="6577f-158">Once the type is created it will appear in the list of types and you would be able to associate with a property</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="6577f-159">Создание базы данных на основе модели</span><span class="sxs-lookup"><span data-stu-id="6577f-159">Generate Database from Model</span></span>

<span data-ttu-id="6577f-160">Теперь мы сможем создать базу данных, основанный на модели.</span><span class="sxs-lookup"><span data-stu-id="6577f-160">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="6577f-161">Щелкните правой кнопкой мыши пустое место в области конструктора сущностей и выберите пункт **создать базу данных из модели**</span><span class="sxs-lookup"><span data-stu-id="6577f-161">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="6577f-162">Диалогового окна соединения данных из мастера создания базы данных является выбор отображаемых щелкните **новое подключение** кнопку укажите **(localdb)\\mssqllocaldb** для имени сервера и  **EnumTest** базы данных и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="6577f-162">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **EnumTest** for the database and click **OK**</span></span>
3.  <span data-ttu-id="6577f-163">Диалоговое окно с запросом, если вы хотите создать новую базу данных будет всплывающее окно, нажмите кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="6577f-163">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="6577f-164">Нажмите кнопку **Далее** и мастер создания базы данных приводит к возникновению ошибки языка описания данных (DDL) для создания базы данных, сформированный код DDL отображается сводка и параметры диалогового окна поле Обратите внимание, что, DDL не содержит определение для таблицу, которая сопоставляется с типом перечисления</span><span class="sxs-lookup"><span data-stu-id="6577f-164">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="6577f-165">Нажмите кнопку **Готово** щелкнув Готово не выполняет скрипт DDL.</span><span class="sxs-lookup"><span data-stu-id="6577f-165">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="6577f-166">Мастер создания базы данных выполняет следующие: открывает **EnumTest.edmx.sql** в редакторе T-SQL формирует разделы схемы и сопоставления хранилища EDMX-файла файл строку подключения Adds в файл App.config</span><span class="sxs-lookup"><span data-stu-id="6577f-166">The Create Database Wizard does the following: Opens the **EnumTest.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="6577f-167">Щелкните правой кнопкой мыши в редакторе T-SQL и выберите **Execute** The Server диалоговое окно подключения к откроется, введите сведения о соединении из шага 2 и нажмите кнопку **Connect**</span><span class="sxs-lookup"><span data-stu-id="6577f-167">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="6577f-168">Чтобы просмотреть созданную схему, щелкните правой кнопкой мыши имя базы данных в обозревателе объектов SQL Server и выберите **обновления**</span><span class="sxs-lookup"><span data-stu-id="6577f-168">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="6577f-169">Сохранения и извлечения данных</span><span class="sxs-lookup"><span data-stu-id="6577f-169">Persist and Retrieve Data</span></span>

<span data-ttu-id="6577f-170">Откройте файл Program.cs, в котором определен метод Main.</span><span class="sxs-lookup"><span data-stu-id="6577f-170">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="6577f-171">Добавьте следующий код в функцию Main.</span><span class="sxs-lookup"><span data-stu-id="6577f-171">Add the following code into the Main function.</span></span> <span data-ttu-id="6577f-172">Код добавляет новый объект отдела в контекст.</span><span class="sxs-lookup"><span data-stu-id="6577f-172">The code adds a new Department object to the context.</span></span> <span data-ttu-id="6577f-173">Затем она сохраняет данные.</span><span class="sxs-lookup"><span data-stu-id="6577f-173">It then saves the data.</span></span> <span data-ttu-id="6577f-174">Код также выполняется запрос LINQ, возвращающий подразделение, где имя задается DepartmentNames.English.</span><span class="sxs-lookup"><span data-stu-id="6577f-174">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

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

<span data-ttu-id="6577f-175">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="6577f-175">Compile and run the application.</span></span> <span data-ttu-id="6577f-176">Программа выдает следующие результаты.</span><span class="sxs-lookup"><span data-stu-id="6577f-176">The program produces the following output:</span></span>

```
DepartmentID: 1 Name: English
```

<span data-ttu-id="6577f-177">Чтобы просмотреть данные в базе данных, щелкните правой кнопкой мыши имя базы данных в обозревателе объектов SQL Server и выберите **обновить**.</span><span class="sxs-lookup"><span data-stu-id="6577f-177">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="6577f-178">Щелкните правой кнопкой мыши таблицу и выберите **данные представления**.</span><span class="sxs-lookup"><span data-stu-id="6577f-178">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="6577f-179">Сводка</span><span class="sxs-lookup"><span data-stu-id="6577f-179">Summary</span></span>

<span data-ttu-id="6577f-180">В этом пошаговом руководстве мы рассмотрели сведения о сопоставлении типов перечисления, с помощью Entity Framework Designer и как использовать перечисления в коде.</span><span class="sxs-lookup"><span data-stu-id="6577f-180">In this walkthrough we looked at how to map enum types using the Entity Framework Designer and how to use enums in code.</span></span> 
