---
title: Определение конструктора запросов EF — EF6
description: Определение конструктора запросов EF в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/designer/advanced/defining-query
ms.openlocfilehash: 3640156a2ce97dccab563a18d3236aad3dc72ee0
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92066386"
---
# <a name="defining-query---ef-designer"></a><span data-ttu-id="1cee2-103">Определение конструктора запросов EF</span><span class="sxs-lookup"><span data-stu-id="1cee2-103">Defining Query - EF Designer</span></span>
<span data-ttu-id="1cee2-104">В этом пошаговом руководстве показано, как добавить определяющий запрос и соответствующий тип сущности в модель с помощью конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="1cee2-104">This walkthrough demonstrates how to add a defining query and a corresponding entity type to a model using the EF Designer.</span></span> <span data-ttu-id="1cee2-105">Определяющий запрос обычно используется для предоставления функциональных возможностей, аналогичных представленному в представлении базы данных, но представление определяется в модели, а не в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1cee2-105">A defining query is commonly used to provide functionality similar to that provided by a database view, but the view is defined in the model, not the database.</span></span> <span data-ttu-id="1cee2-106">Определяющий запрос позволяет выполнить инструкцию SQL, указанную в элементе **DefiningQuery**   EDMX-файла.</span><span class="sxs-lookup"><span data-stu-id="1cee2-106">A defining query allows you to execute a SQL statement that is specified in the **DefiningQuery** element of an .edmx file.</span></span> <span data-ttu-id="1cee2-107">Дополнительные сведения см. в разделе **DefiningQuery** в [спецификации SSDL](xref:ef6/modeling/designer/advanced/edmx/ssdl-spec).</span><span class="sxs-lookup"><span data-stu-id="1cee2-107">For more information, see **DefiningQuery** in the [SSDL Specification](xref:ef6/modeling/designer/advanced/edmx/ssdl-spec).</span></span>

<span data-ttu-id="1cee2-108">При использовании определения запросов необходимо также определить тип сущности в модели.</span><span class="sxs-lookup"><span data-stu-id="1cee2-108">When using defining queries, you also have to define an entity type in your model.</span></span> <span data-ttu-id="1cee2-109">Тип сущности используется для отображения данных, предоставляемых определяющим запросом.</span><span class="sxs-lookup"><span data-stu-id="1cee2-109">The entity type is used to surface data exposed by the defining query.</span></span> <span data-ttu-id="1cee2-110">Обратите внимание, что данные, которые отображаются с помощью этого типа сущности, доступны только для чтения.</span><span class="sxs-lookup"><span data-stu-id="1cee2-110">Note that data surfaced through this entity type is read-only.</span></span>

<span data-ttu-id="1cee2-111">Параметризованные запросы не могут выполняться как определяющие запросы.</span><span class="sxs-lookup"><span data-stu-id="1cee2-111">Parameterized queries cannot be executed as defining queries.</span></span> <span data-ttu-id="1cee2-112">Однако их обновление возможно через сопоставление функций вставки, обновления и удаления типа сущности, который отображает данные в хранимых процедурах.</span><span class="sxs-lookup"><span data-stu-id="1cee2-112">However, the data can be updated by mapping the insert, update, and delete functions of the entity type that surfaces the data to stored procedures.</span></span> <span data-ttu-id="1cee2-113">Дополнительные сведения см. [в разделе Вставка, обновление и удаление хранимых процедур](xref:ef6/modeling/designer/stored-procedures/cud).</span><span class="sxs-lookup"><span data-stu-id="1cee2-113">For more information, see [Insert, Update, and Delete with Stored Procedures](xref:ef6/modeling/designer/stored-procedures/cud).</span></span>

<span data-ttu-id="1cee2-114">В этом разделе показано, как выполнять следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="1cee2-114">This topic shows how to perform the following tasks.</span></span>

-   <span data-ttu-id="1cee2-115">Добавление определяющего запроса</span><span class="sxs-lookup"><span data-stu-id="1cee2-115">Add a Defining Query</span></span>
-   <span data-ttu-id="1cee2-116">Добавление типа сущности в модель</span><span class="sxs-lookup"><span data-stu-id="1cee2-116">Add an Entity Type to the Model</span></span>
-   <span data-ttu-id="1cee2-117">Сопоставьте определяющий запрос с типом сущности</span><span class="sxs-lookup"><span data-stu-id="1cee2-117">Map the Defining Query to the Entity Type</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1cee2-118">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="1cee2-118">Prerequisites</span></span>

<span data-ttu-id="1cee2-119">Для выполнения данного пошагового руководства требуется:</span><span class="sxs-lookup"><span data-stu-id="1cee2-119">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="1cee2-120">Последняя версия Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1cee2-120">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="1cee2-121">[Образец базы данных School](xref:ef6/resources/school-database).</span><span class="sxs-lookup"><span data-stu-id="1cee2-121">The [School sample database](xref:ef6/resources/school-database).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="1cee2-122">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="1cee2-122">Set up the Project</span></span>

<span data-ttu-id="1cee2-123">В этом пошаговом руководстве используется Visual Studio 2012 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="1cee2-123">This walkthrough is using Visual Studio 2012 or newer.</span></span>

-   <span data-ttu-id="1cee2-124">Запустите Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1cee2-124">Open Visual Studio.</span></span>
-   <span data-ttu-id="1cee2-125">В меню **Файл** укажите **Создать**, затем нажмите **Проект**.</span><span class="sxs-lookup"><span data-stu-id="1cee2-125">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="1cee2-126">В левой области щелкните \*\*Visual C \# \*\*, а затем выберите шаблон **консольное приложение** .</span><span class="sxs-lookup"><span data-stu-id="1cee2-126">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="1cee2-127">Введите **дефинингкуерисампле** в качестве имени проекта и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="1cee2-127">Enter **DefiningQuerySample** as the name of the project and click **OK**.</span></span>

 

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="1cee2-128">Создание модели на основе базы данных School</span><span class="sxs-lookup"><span data-stu-id="1cee2-128">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="1cee2-129">Щелкните правой кнопкой мыши имя проекта в обозреватель решений, наведите указатель на пункт **Добавить**и выберите пункт **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="1cee2-129">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="1cee2-130">Выберите **данные** в меню слева, а затем выберите **ADO.NET EDM** в области Шаблоны.</span><span class="sxs-lookup"><span data-stu-id="1cee2-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="1cee2-131">Введите **дефинингкуеримодел. EDMX** в поле имя файла и нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="1cee2-131">Enter **DefiningQueryModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="1cee2-132">В диалоговом окне Выбор содержимого модели выберите пункт **создать из базы данных**, а затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="1cee2-132">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="1cee2-133">Нажмите кнопку Создать соединение.</span><span class="sxs-lookup"><span data-stu-id="1cee2-133">Click New Connection.</span></span> <span data-ttu-id="1cee2-134">В диалоговом окне Свойства соединения введите имя сервера (например, **(LocalDB) \\ mssqllocaldb**), выберите метод проверки подлинности, введите **School**   в качестве имени базы данных и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="1cee2-134">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="1cee2-135">В диалоговом окне Выбор подключения к данным будет обновлен параметр подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="1cee2-135">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="1cee2-136">В диалоговом окне Выбор объектов базы данных проверьте узел **таблицы**   .</span><span class="sxs-lookup"><span data-stu-id="1cee2-136">In the Choose Your Database Objects dialog box, check the **Tables** node.</span></span> <span data-ttu-id="1cee2-137">Это приведет к добавлению всех таблиц в модель **School** .</span><span class="sxs-lookup"><span data-stu-id="1cee2-137">This will add all the tables to the **School** model.</span></span>
-   <span data-ttu-id="1cee2-138">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="1cee2-138">Click **Finish**.</span></span>
-   <span data-ttu-id="1cee2-139">В обозреватель решений щелкните правой кнопкой мыши файл **дефинингкуеримодел. EDMX** и выберите команду **Открыть с помощью...**.</span><span class="sxs-lookup"><span data-stu-id="1cee2-139">In Solution Explorer, right-click the **DefiningQueryModel.edmx** file and select **Open With…**.</span></span>
-   <span data-ttu-id="1cee2-140">Выберите **Редактор XML (текстовый)**.</span><span class="sxs-lookup"><span data-stu-id="1cee2-140">Select **XML (Text) Editor**.</span></span>

    ![Редактор XML](~/ef6/media/xmleditor.png)

-   <span data-ttu-id="1cee2-142">Нажмите кнопку **Да** , если появится следующее сообщение:</span><span class="sxs-lookup"><span data-stu-id="1cee2-142">Click **Yes** if prompted with the following message:</span></span>

    ![Предупреждение 2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a><span data-ttu-id="1cee2-144">Добавление определяющего запроса</span><span class="sxs-lookup"><span data-stu-id="1cee2-144">Add a Defining Query</span></span>

<span data-ttu-id="1cee2-145">На этом шаге мы будем использовать редактор XML для добавления определяющего запроса и типа сущности в раздел SSDL EDMX-файла.</span><span class="sxs-lookup"><span data-stu-id="1cee2-145">In this step we will use the XML Editor to add a defining query and an entity type to the SSDL section of the .edmx file.</span></span> 

-   <span data-ttu-id="1cee2-146">Добавьте элемент **EntitySet**   в раздел SSDL EDMX-файла (строка 5 – 13).</span><span class="sxs-lookup"><span data-stu-id="1cee2-146">Add an **EntitySet** element to the SSDL section of the .edmx file (line 5 thru 13).</span></span> <span data-ttu-id="1cee2-147">Укажите следующее.</span><span class="sxs-lookup"><span data-stu-id="1cee2-147">Specify the following:</span></span>
    -   <span data-ttu-id="1cee2-148">Указаны только атрибуты **Name**   и **EntityType**   элемента **EntitySet**   .</span><span class="sxs-lookup"><span data-stu-id="1cee2-148">Only the **Name** and **EntityType** attributes of the **EntitySet** element are specified.</span></span>
    -   <span data-ttu-id="1cee2-149">Полное имя типа сущности используется в **EntityType**   атрибуте EntityType.</span><span class="sxs-lookup"><span data-stu-id="1cee2-149">The fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="1cee2-150">Инструкция SQL, которая должна быть выполнена, указывается в элементе **DefiningQuery**   .</span><span class="sxs-lookup"><span data-stu-id="1cee2-150">The SQL statement to be executed is specified in the **DefiningQuery** element.</span></span>

``` xml
    <!-- SSDL content -->
    <edmx:StorageModels>
      <Schema Namespace="SchoolModel.Store" Alias="Self" Provider="System.Data.SqlClient" ProviderManifestToken="2008" xmlns:store="http://schemas.microsoft.com/ado/2007/12/edm/EntityStoreSchemaGenerator" xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
        <EntityContainer Name="SchoolModelStoreContainer">
           <EntitySet Name="GradeReport" EntityType="SchoolModel.Store.GradeReport">
              <DefiningQuery>
                SELECT CourseID, Grade, FirstName, LastName
                FROM StudentGrade
                JOIN
                (SELECT * FROM Person WHERE EnrollmentDate IS NOT NULL) AS p
                ON StudentID = p.PersonID
              </DefiningQuery>
          </EntitySet>
          <EntitySet Name="Course" EntityType="SchoolModel.Store.Course" store:Type="Tables" Schema="dbo" />
```

-   <span data-ttu-id="1cee2-151">Добавьте элемент **EntityType** в раздел SSDL в EDMX.</span><span class="sxs-lookup"><span data-stu-id="1cee2-151">Add the **EntityType** element to the SSDL section of the .edmx.</span></span> <span data-ttu-id="1cee2-152">файл, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="1cee2-152">file as shown below.</span></span> <span data-ttu-id="1cee2-153">Следует отметить следующее.</span><span class="sxs-lookup"><span data-stu-id="1cee2-153">Note the following:</span></span>
    -   <span data-ttu-id="1cee2-154">Значение атрибута **Name** соответствует значению атрибута **EntityType** в элементе **EntitySet** , приведенном выше, хотя полное имя типа сущности используется в атрибуте **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="1cee2-154">The value of the **Name** attribute corresponds to the value of the **EntityType** attribute in the **EntitySet** element above, although the fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="1cee2-155">Имена свойств соответствуют именам столбцов, возвращаемых инструкцией SQL в элементе **DefiningQuery** (выше).</span><span class="sxs-lookup"><span data-stu-id="1cee2-155">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element (above).</span></span>
    -   <span data-ttu-id="1cee2-156">В данном примере ключ сущности состоит из трех свойств, что обеспечивает уникальность значения ключа.</span><span class="sxs-lookup"><span data-stu-id="1cee2-156">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

``` xml
    <EntityType Name="GradeReport">
      <Key>
        <PropertyRef Name="CourseID" />
        <PropertyRef Name="FirstName" />
        <PropertyRef Name="LastName" />
      </Key>
      <Property Name="CourseID"
                Type="int"
                Nullable="false" />
      <Property Name="Grade"
                Type="decimal"
                Precision="3"
                Scale="2" />
      <Property Name="FirstName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
      <Property Name="LastName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
    </EntityType>
```

>[!NOTE]
> <span data-ttu-id="1cee2-157">Если позже вы запустите диалоговое окно **мастера обновления моделей** , любые изменения, внесенные в модель хранения, включая определение запросов, будут перезаписаны.</span><span class="sxs-lookup"><span data-stu-id="1cee2-157">If later you run the **Update Model Wizard** dialog, any changes made to the storage model, including defining queries, will be overwritten.</span></span>

 

## <a name="add-an-entity-type-to-the-model"></a><span data-ttu-id="1cee2-158">Добавление типа сущности в модель</span><span class="sxs-lookup"><span data-stu-id="1cee2-158">Add an Entity Type to the Model</span></span>

<span data-ttu-id="1cee2-159">На этом шаге мы добавим тип сущности в концептуальную модель с помощью конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="1cee2-159">In this step we will add the entity type to the conceptual model using the EF Designer.</span></span> <span data-ttu-id="1cee2-160">Следует отметить следующее.</span><span class="sxs-lookup"><span data-stu-id="1cee2-160"> Note the following:</span></span>

-   <span data-ttu-id="1cee2-161">**Имя** сущности соответствует значению атрибута **EntityType** в элементе **EntitySet** , приведенном выше.</span><span class="sxs-lookup"><span data-stu-id="1cee2-161">The **Name** of the entity corresponds to the value of the **EntityType** attribute in the **EntitySet** element above.</span></span>
-   <span data-ttu-id="1cee2-162">Имена свойств соответствуют именам столбцов, возвращаемых инструкцией SQL в элементе **DefiningQuery** выше.</span><span class="sxs-lookup"><span data-stu-id="1cee2-162">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element above.</span></span>
-   <span data-ttu-id="1cee2-163">В данном примере ключ сущности состоит из трех свойств, что обеспечивает уникальность значения ключа.</span><span class="sxs-lookup"><span data-stu-id="1cee2-163">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

<span data-ttu-id="1cee2-164">Откройте модель в конструкторе EF.</span><span class="sxs-lookup"><span data-stu-id="1cee2-164">Open the model in the EF Designer.</span></span>

-   <span data-ttu-id="1cee2-165">Дважды щелкните Дефинингкуеримодел. edmx.</span><span class="sxs-lookup"><span data-stu-id="1cee2-165">Double-click the DefiningQueryModel.edmx.</span></span>
-   <span data-ttu-id="1cee2-166">Скажите « **Да** », чтобы получить следующее сообщение:</span><span class="sxs-lookup"><span data-stu-id="1cee2-166">Say **Yes** to the following message:</span></span>

    ![Предупреждение 2](~/ef6/media/warning2.png)

 

<span data-ttu-id="1cee2-168">Отобразится Entity Designer, предоставляющий область конструктора для редактирования модели.</span><span class="sxs-lookup"><span data-stu-id="1cee2-168">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

-   <span data-ttu-id="1cee2-169">Щелкните правой кнопкой мыши область конструктора и выберите команду **Добавить новую** - &gt; **сущность...**.</span><span class="sxs-lookup"><span data-stu-id="1cee2-169">Right-click the designer surface and select **Add New**-&gt;**Entity…**.</span></span>
-   <span data-ttu-id="1cee2-170">Укажите **градерепорт** для имени сущности и **CourseID** для **ключевого свойства**.</span><span class="sxs-lookup"><span data-stu-id="1cee2-170">Specify **GradeReport** for the entity name and **CourseID** for the **Key Property**.</span></span>
-   <span data-ttu-id="1cee2-171">Щелкните правой кнопкой мыши сущность **градерепорт** и выберите пункт **Добавить новое** - &gt; **скалярное свойство**.</span><span class="sxs-lookup"><span data-stu-id="1cee2-171">Right-click the **GradeReport** entity and select **Add New**-&gt; **Scalar Property**.</span></span>
-   <span data-ttu-id="1cee2-172">Измените имя свойства по умолчанию на **FirstName**.</span><span class="sxs-lookup"><span data-stu-id="1cee2-172">Change the default name of the property to **FirstName**.</span></span>
-   <span data-ttu-id="1cee2-173">Добавьте еще одно скалярное свойство и укажите **LastName** в качестве имени.</span><span class="sxs-lookup"><span data-stu-id="1cee2-173">Add another scalar property and specify **LastName** for the name.</span></span>
-   <span data-ttu-id="1cee2-174">Добавьте еще одно скалярное свойство и укажите **Grade** для имени.</span><span class="sxs-lookup"><span data-stu-id="1cee2-174">Add another scalar property and specify **Grade** for the name.</span></span>
-   <span data-ttu-id="1cee2-175">В окне **Свойства** измените свойство **тип** на **Grade** **десятичное**.</span><span class="sxs-lookup"><span data-stu-id="1cee2-175">In the **Properties** window, change the **Grade**’s **Type** property to **Decimal**.</span></span>
-   <span data-ttu-id="1cee2-176">Выберите свойства **FirstName** и **LastName** .</span><span class="sxs-lookup"><span data-stu-id="1cee2-176">Select the **FirstName** and **LastName** properties.</span></span>
-   <span data-ttu-id="1cee2-177">В окне **Свойства** измените значение свойства **EntityKey** на **true**.</span><span class="sxs-lookup"><span data-stu-id="1cee2-177">In the **Properties** window, change the **EntityKey** property value to **True**.</span></span>

<span data-ttu-id="1cee2-178">В результате в раздел **CSDL** EDMX-файла были добавлены следующие элементы.</span><span class="sxs-lookup"><span data-stu-id="1cee2-178">As a result, the following elements were added to the **CSDL** section of the .edmx file.</span></span>

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a><span data-ttu-id="1cee2-179">Сопоставьте определяющий запрос с типом сущности</span><span class="sxs-lookup"><span data-stu-id="1cee2-179">Map the Defining Query to the Entity Type</span></span>

<span data-ttu-id="1cee2-180">На этом шаге мы будем использовать окно сведения о сопоставлении, чтобы сопоставить концептуальные и типы сущностей хранилища.</span><span class="sxs-lookup"><span data-stu-id="1cee2-180">In this step, we will use the Mapping Details window to map the conceptual and storage entity types.</span></span>

-   <span data-ttu-id="1cee2-181">Щелкните правой кнопкой мыши сущность **градерепорт** в области конструктора и выберите пункт **Сопоставление таблиц**.</span><span class="sxs-lookup"><span data-stu-id="1cee2-181">Right-click the **GradeReport** entity on the design surface and select **Table Mapping**.</span></span>  
    <span data-ttu-id="1cee2-182">Откроется окно **сведения о сопоставлении** .</span><span class="sxs-lookup"><span data-stu-id="1cee2-182">The **Mapping Details** window is displayed.</span></span>
-   <span data-ttu-id="1cee2-183">Выберите **градерепорт** в раскрывающемся списке \*\* &lt; Добавить таблицу или &gt; представление\*\* (находится в разделе **таблицы**s).</span><span class="sxs-lookup"><span data-stu-id="1cee2-183">Select **GradeReport** from the **&lt;Add a Table or View&gt;** dropdown list (located under **Table**s).</span></span>  
    <span data-ttu-id="1cee2-184">Отобразятся сопоставления по умолчанию между концептуальным и типом сущности **градерепорт** хранилища.</span><span class="sxs-lookup"><span data-stu-id="1cee2-184">Default mappings between the conceptual and storage **GradeReport** entity type appear.</span></span>  
    <span data-ttu-id="1cee2-185">![Сопоставление Details3](~/ef6/media/mappingdetails.png)</span><span class="sxs-lookup"><span data-stu-id="1cee2-185">![Mapping Details3](~/ef6/media/mappingdetails.png)</span></span>

<span data-ttu-id="1cee2-186">В результате элемент **EntitySetMapping**   добавляется в раздел сопоставления EDMX-файла.</span><span class="sxs-lookup"><span data-stu-id="1cee2-186">As a result, the **EntitySetMapping** element is added to the mapping section of the .edmx file.</span></span> 

``` xml
    <EntitySetMapping Name="GradeReports">
      <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.GradeReport)">
        <MappingFragment StoreEntitySet="GradeReport">
          <ScalarProperty Name="LastName" ColumnName="LastName" />
          <ScalarProperty Name="FirstName" ColumnName="FirstName" />
          <ScalarProperty Name="Grade" ColumnName="Grade" />
          <ScalarProperty Name="CourseID" ColumnName="CourseID" />
        </MappingFragment>
      </EntityTypeMapping>
    </EntitySetMapping>
```

-   <span data-ttu-id="1cee2-187">Скомпилируйте приложение.</span><span class="sxs-lookup"><span data-stu-id="1cee2-187">Compile the application.</span></span>

 

## <a name="call-the-defining-query-in-your-code"></a><span data-ttu-id="1cee2-188">Вызов определяющего запроса в коде</span><span class="sxs-lookup"><span data-stu-id="1cee2-188">Call the Defining Query in your Code</span></span>

<span data-ttu-id="1cee2-189">Теперь можно выполнить определяющий запрос с помощью типа сущности **градерепорт** .</span><span class="sxs-lookup"><span data-stu-id="1cee2-189">You can now execute the defining query by using the **GradeReport** entity type.</span></span> 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
