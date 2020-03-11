---
title: Определение конструктора запросов EF — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: b1589dc12ccb50754c2e950932a2d82bc4869f6b
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415535"
---
# <a name="defining-query---ef-designer"></a><span data-ttu-id="f8d09-102">Определение конструктора запросов EF</span><span class="sxs-lookup"><span data-stu-id="f8d09-102">Defining Query - EF Designer</span></span>
<span data-ttu-id="f8d09-103">В этом пошаговом руководстве показано, как добавить определяющий запрос и соответствующий тип сущности в модель с помощью конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="f8d09-103">This walkthrough demonstrates how to add a defining query and a corresponding entity type to a model using the EF Designer.</span></span> <span data-ttu-id="f8d09-104">Определяющий запрос обычно используется для предоставления функциональных возможностей, аналогичных представленному в представлении базы данных, но представление определяется в модели, а не в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f8d09-104">A defining query is commonly used to provide functionality similar to that provided by a database view, but the view is defined in the model, not the database.</span></span> <span data-ttu-id="f8d09-105">Определяющий запрос позволяет выполнить инструкцию SQL, указанную в элементе  **DefiningQuery** файла EDMX.</span><span class="sxs-lookup"><span data-stu-id="f8d09-105">A defining query allows you to execute a SQL statement that is specified in the **DefiningQuery** element of an .edmx file.</span></span> <span data-ttu-id="f8d09-106">Дополнительные сведения см. в разделе **DefiningQuery** в [спецификации SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="f8d09-106">For more information, see **DefiningQuery** in the [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="f8d09-107">При использовании определения запросов необходимо также определить тип сущности в модели.</span><span class="sxs-lookup"><span data-stu-id="f8d09-107">When using defining queries, you also have to define an entity type in your model.</span></span> <span data-ttu-id="f8d09-108">Тип сущности используется для отображения данных, предоставляемых определяющим запросом.</span><span class="sxs-lookup"><span data-stu-id="f8d09-108">The entity type is used to surface data exposed by the defining query.</span></span> <span data-ttu-id="f8d09-109">Обратите внимание, что данные, которые отображаются с помощью этого типа сущности, доступны только для чтения.</span><span class="sxs-lookup"><span data-stu-id="f8d09-109">Note that data surfaced through this entity type is read-only.</span></span>

<span data-ttu-id="f8d09-110">Параметризованные запросы не могут выполняться как определяющие запросы.</span><span class="sxs-lookup"><span data-stu-id="f8d09-110">Parameterized queries cannot be executed as defining queries.</span></span> <span data-ttu-id="f8d09-111">Однако их обновление возможно через сопоставление функций вставки, обновления и удаления типа сущности, который отображает данные в хранимых процедурах.</span><span class="sxs-lookup"><span data-stu-id="f8d09-111">However, the data can be updated by mapping the insert, update, and delete functions of the entity type that surfaces the data to stored procedures.</span></span> <span data-ttu-id="f8d09-112">Дополнительные сведения см. [в разделе Вставка, обновление и удаление хранимых процедур](~/ef6/modeling/designer/stored-procedures/cud.md).</span><span class="sxs-lookup"><span data-stu-id="f8d09-112">For more information, see [Insert, Update, and Delete with Stored Procedures](~/ef6/modeling/designer/stored-procedures/cud.md).</span></span>

<span data-ttu-id="f8d09-113">В этом разделе показано, как выполнять следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="f8d09-113">This topic shows how to perform the following tasks.</span></span>

-   <span data-ttu-id="f8d09-114">Добавление определяющего запроса</span><span class="sxs-lookup"><span data-stu-id="f8d09-114">Add a Defining Query</span></span>
-   <span data-ttu-id="f8d09-115">Добавление типа сущности в модель</span><span class="sxs-lookup"><span data-stu-id="f8d09-115">Add an Entity Type to the Model</span></span>
-   <span data-ttu-id="f8d09-116">Сопоставьте определяющий запрос с типом сущности</span><span class="sxs-lookup"><span data-stu-id="f8d09-116">Map the Defining Query to the Entity Type</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f8d09-117">предварительные требования</span><span class="sxs-lookup"><span data-stu-id="f8d09-117">Prerequisites</span></span>

<span data-ttu-id="f8d09-118">Для выполнения данного пошагового руководства требуется:</span><span class="sxs-lookup"><span data-stu-id="f8d09-118">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="f8d09-119">Последняя версия Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8d09-119">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="f8d09-120">[Образец базы данных School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="f8d09-120">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="f8d09-121">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="f8d09-121">Set up the Project</span></span>

<span data-ttu-id="f8d09-122">В этом пошаговом руководстве используется Visual Studio 2012 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="f8d09-122">This walkthrough is using Visual Studio 2012 or newer.</span></span>

-   <span data-ttu-id="f8d09-123">Запустите Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8d09-123">Open Visual Studio.</span></span>
-   <span data-ttu-id="f8d09-124">В меню **Файл** укажите **Создать**, затем нажмите **Проект**.</span><span class="sxs-lookup"><span data-stu-id="f8d09-124">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="f8d09-125">В левой области щелкните **Visual C\#** , а затем выберите шаблон **консольное приложение** .</span><span class="sxs-lookup"><span data-stu-id="f8d09-125">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="f8d09-126">Введите **дефинингкуерисампле** в качестве имени проекта и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="f8d09-126">Enter **DefiningQuerySample** as the name of the project and click **OK**.</span></span>

 

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="f8d09-127">Создание модели на основе базы данных School</span><span class="sxs-lookup"><span data-stu-id="f8d09-127">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="f8d09-128">Щелкните правой кнопкой мыши имя проекта в обозреватель решений, наведите указатель на пункт **Добавить**и выберите пункт **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="f8d09-128">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="f8d09-129">Выберите **данные** в меню слева, а затем выберите **ADO.NET EDM** в области Шаблоны.</span><span class="sxs-lookup"><span data-stu-id="f8d09-129">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="f8d09-130">Введите **дефинингкуеримодел. EDMX** в поле имя файла и нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="f8d09-130">Enter **DefiningQueryModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="f8d09-131">В диалоговом окне Выбор содержимого модели выберите пункт **создать из базы данных**, а затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="f8d09-131">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="f8d09-132">Нажмите кнопку Создать соединение.</span><span class="sxs-lookup"><span data-stu-id="f8d09-132">Click New Connection.</span></span> <span data-ttu-id="f8d09-133">В диалоговом окне Свойства соединения введите имя сервера (например, **(LocalDB)\\mssqllocaldb**), выберите метод проверки подлинности, введите **School** в качестве имени базы данных и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="f8d09-133">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="f8d09-134">В диалоговом окне Выбор подключения к данным будет обновлен параметр подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="f8d09-134">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="f8d09-135">В диалоговом окне Выбор объектов базы данных проверьте узел **таблицы** .</span><span class="sxs-lookup"><span data-stu-id="f8d09-135">In the Choose Your Database Objects dialog box, check the **Tables** node.</span></span> <span data-ttu-id="f8d09-136">Это приведет к добавлению всех таблиц в модель **School** .</span><span class="sxs-lookup"><span data-stu-id="f8d09-136">This will add all the tables to the **School** model.</span></span>
-   <span data-ttu-id="f8d09-137">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="f8d09-137">Click **Finish**.</span></span>
-   <span data-ttu-id="f8d09-138">В обозреватель решений щелкните правой кнопкой мыши файл **дефинингкуеримодел. EDMX** и выберите команду **Открыть с помощью...** .</span><span class="sxs-lookup"><span data-stu-id="f8d09-138">In Solution Explorer, right-click the **DefiningQueryModel.edmx** file and select **Open With…**.</span></span>
-   <span data-ttu-id="f8d09-139">Выберите **Редактор XML (текстовый)** .</span><span class="sxs-lookup"><span data-stu-id="f8d09-139">Select **XML (Text) Editor**.</span></span>

    ![Редактор XML](~/ef6/media/xmleditor.png)

-   <span data-ttu-id="f8d09-141">Нажмите кнопку **Да** , если появится следующее сообщение:</span><span class="sxs-lookup"><span data-stu-id="f8d09-141">Click **Yes** if prompted with the following message:</span></span>

    ![Предупреждение 2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a><span data-ttu-id="f8d09-143">Добавление определяющего запроса</span><span class="sxs-lookup"><span data-stu-id="f8d09-143">Add a Defining Query</span></span>

<span data-ttu-id="f8d09-144">На этом шаге мы будем использовать редактор XML для добавления определяющего запроса и типа сущности в раздел SSDL EDMX-файла.</span><span class="sxs-lookup"><span data-stu-id="f8d09-144">In this step we will use the XML Editor to add a defining query and an entity type to the SSDL section of the .edmx file.</span></span> 

-   <span data-ttu-id="f8d09-145">Добавьте элемент **entityset** в раздел SSDL EDMX-файла (строка 5 – 13).</span><span class="sxs-lookup"><span data-stu-id="f8d09-145">Add an **EntitySet** element to the SSDL section of the .edmx file (line 5 thru 13).</span></span> <span data-ttu-id="f8d09-146">Укажите следующее.</span><span class="sxs-lookup"><span data-stu-id="f8d09-146">Specify the following:</span></span>
    -   <span data-ttu-id="f8d09-147">Указаны только атрибуты **Name** и **EntityType** элемента **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="f8d09-147">Only the **Name** and **EntityType** attributes of the **EntitySet** element are specified.</span></span>
    -   <span data-ttu-id="f8d09-148">Полное имя типа сущности используется в атрибуте **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="f8d09-148">The fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="f8d09-149">Выполняемая инструкция SQL указывается в элементе  **DefiningQuery** .</span><span class="sxs-lookup"><span data-stu-id="f8d09-149">The SQL statement to be executed is specified in the **DefiningQuery** element.</span></span>

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

-   <span data-ttu-id="f8d09-150">Добавьте элемент **EntityType** в раздел SSDL в EDMX.</span><span class="sxs-lookup"><span data-stu-id="f8d09-150">Add the **EntityType** element to the SSDL section of the .edmx.</span></span> <span data-ttu-id="f8d09-151">файл, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="f8d09-151">file as shown below.</span></span> <span data-ttu-id="f8d09-152">Следует отметить следующее.</span><span class="sxs-lookup"><span data-stu-id="f8d09-152">Note the following:</span></span>
    -   <span data-ttu-id="f8d09-153">Значение атрибута **Name** соответствует значению атрибута **EntityType** в элементе **EntitySet** , приведенном выше, хотя полное имя типа сущности используется в атрибуте **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="f8d09-153">The value of the **Name** attribute corresponds to the value of the **EntityType** attribute in the **EntitySet** element above, although the fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="f8d09-154">Имена свойств соответствуют именам столбцов, возвращаемых инструкцией SQL в элементе **DefiningQuery** (выше).</span><span class="sxs-lookup"><span data-stu-id="f8d09-154">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element (above).</span></span>
    -   <span data-ttu-id="f8d09-155">В данном примере ключ сущности состоит из трех свойств, что обеспечивает уникальность значения ключа.</span><span class="sxs-lookup"><span data-stu-id="f8d09-155">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

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
> <span data-ttu-id="f8d09-156">Если позже вы запустите диалоговое окно **мастера обновления моделей** , любые изменения, внесенные в модель хранения, включая определение запросов, будут перезаписаны.</span><span class="sxs-lookup"><span data-stu-id="f8d09-156">If later you run the **Update Model Wizard** dialog, any changes made to the storage model, including defining queries, will be overwritten.</span></span>

 

## <a name="add-an-entity-type-to-the-model"></a><span data-ttu-id="f8d09-157">Добавление типа сущности в модель</span><span class="sxs-lookup"><span data-stu-id="f8d09-157">Add an Entity Type to the Model</span></span>

<span data-ttu-id="f8d09-158">На этом шаге мы добавим тип сущности в концептуальную модель с помощью конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="f8d09-158">In this step we will add the entity type to the conceptual model using the EF Designer.</span></span> <span data-ttu-id="f8d09-159"> Обратите внимание на следующее:</span><span class="sxs-lookup"><span data-stu-id="f8d09-159"> Note the following:</span></span>

-   <span data-ttu-id="f8d09-160">**Имя** сущности соответствует значению атрибута **EntityType** в элементе **EntitySet** , приведенном выше.</span><span class="sxs-lookup"><span data-stu-id="f8d09-160">The **Name** of the entity corresponds to the value of the **EntityType** attribute in the **EntitySet** element above.</span></span>
-   <span data-ttu-id="f8d09-161">Имена свойств соответствуют именам столбцов, возвращаемых инструкцией SQL в элементе **DefiningQuery** выше.</span><span class="sxs-lookup"><span data-stu-id="f8d09-161">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element above.</span></span>
-   <span data-ttu-id="f8d09-162">В данном примере ключ сущности состоит из трех свойств, что обеспечивает уникальность значения ключа.</span><span class="sxs-lookup"><span data-stu-id="f8d09-162">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

<span data-ttu-id="f8d09-163">Откройте модель в конструкторе EF.</span><span class="sxs-lookup"><span data-stu-id="f8d09-163">Open the model in the EF Designer.</span></span>

-   <span data-ttu-id="f8d09-164">Дважды щелкните Дефинингкуеримодел. edmx.</span><span class="sxs-lookup"><span data-stu-id="f8d09-164">Double-click the DefiningQueryModel.edmx.</span></span>
-   <span data-ttu-id="f8d09-165">Скажите « **Да** », чтобы получить следующее сообщение:</span><span class="sxs-lookup"><span data-stu-id="f8d09-165">Say **Yes** to the following message:</span></span>

    ![Предупреждение 2](~/ef6/media/warning2.png)

 

<span data-ttu-id="f8d09-167">Отобразится Entity Designer, предоставляющий область конструктора для редактирования модели.</span><span class="sxs-lookup"><span data-stu-id="f8d09-167">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

-   <span data-ttu-id="f8d09-168">Щелкните правой кнопкой мыши область конструктора и выберите **Добавить новый**-&gt;**сущность...** .</span><span class="sxs-lookup"><span data-stu-id="f8d09-168">Right-click the designer surface and select **Add New**-&gt;**Entity…**.</span></span>
-   <span data-ttu-id="f8d09-169">Укажите **градерепорт** для имени сущности и **CourseID** для **ключевого свойства**.</span><span class="sxs-lookup"><span data-stu-id="f8d09-169">Specify **GradeReport** for the entity name and **CourseID** for the **Key Property**.</span></span>
-   <span data-ttu-id="f8d09-170">Щелкните правой кнопкой мыши сущность **градерепорт** и выберите **добавить новое**-&gt; **скалярное свойство**.</span><span class="sxs-lookup"><span data-stu-id="f8d09-170">Right-click the **GradeReport** entity and select **Add New**-&gt; **Scalar Property**.</span></span>
-   <span data-ttu-id="f8d09-171">Измените имя свойства по умолчанию на **FirstName**.</span><span class="sxs-lookup"><span data-stu-id="f8d09-171">Change the default name of the property to **FirstName**.</span></span>
-   <span data-ttu-id="f8d09-172">Добавьте еще одно скалярное свойство и укажите **LastName** в качестве имени.</span><span class="sxs-lookup"><span data-stu-id="f8d09-172">Add another scalar property and specify **LastName** for the name.</span></span>
-   <span data-ttu-id="f8d09-173">Добавьте еще одно скалярное свойство и укажите **Grade** для имени.</span><span class="sxs-lookup"><span data-stu-id="f8d09-173">Add another scalar property and specify **Grade** for the name.</span></span>
-   <span data-ttu-id="f8d09-174">В окне **Свойства** измените свойство **тип** на **десятичное**.</span><span class="sxs-lookup"><span data-stu-id="f8d09-174">In the **Properties** window, change the **Grade**’s **Type** property to **Decimal**.</span></span>
-   <span data-ttu-id="f8d09-175">Выберите свойства **FirstName** и **LastName** .</span><span class="sxs-lookup"><span data-stu-id="f8d09-175">Select the **FirstName** and **LastName** properties.</span></span>
-   <span data-ttu-id="f8d09-176">В окне **Свойства** измените значение свойства **EntityKey** на **true**.</span><span class="sxs-lookup"><span data-stu-id="f8d09-176">In the **Properties** window, change the **EntityKey** property value to **True**.</span></span>

<span data-ttu-id="f8d09-177">В результате в раздел **CSDL** EDMX-файла были добавлены следующие элементы.</span><span class="sxs-lookup"><span data-stu-id="f8d09-177">As a result, the following elements were added to the **CSDL** section of the .edmx file.</span></span>

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a><span data-ttu-id="f8d09-178">Сопоставьте определяющий запрос с типом сущности</span><span class="sxs-lookup"><span data-stu-id="f8d09-178">Map the Defining Query to the Entity Type</span></span>

<span data-ttu-id="f8d09-179">На этом шаге мы будем использовать окно сведения о сопоставлении, чтобы сопоставить концептуальные и типы сущностей хранилища.</span><span class="sxs-lookup"><span data-stu-id="f8d09-179">In this step, we will use the Mapping Details window to map the conceptual and storage entity types.</span></span>

-   <span data-ttu-id="f8d09-180">Щелкните правой кнопкой мыши сущность **градерепорт** в области конструктора и выберите пункт **Сопоставление таблиц**.</span><span class="sxs-lookup"><span data-stu-id="f8d09-180">Right-click the **GradeReport** entity on the design surface and select **Table Mapping**.</span></span>  
    <span data-ttu-id="f8d09-181">Откроется окно **сведения о сопоставлении** .</span><span class="sxs-lookup"><span data-stu-id="f8d09-181">The **Mapping Details** window is displayed.</span></span>
-   <span data-ttu-id="f8d09-182">Выберите **градерепорт** в **&lt;добавить таблицу или представление&gt;** раскрывающийся список (находится в разделе **таблицы**s).</span><span class="sxs-lookup"><span data-stu-id="f8d09-182">Select **GradeReport** from the **&lt;Add a Table or View&gt;** dropdown list (located under **Table**s).</span></span>  
    <span data-ttu-id="f8d09-183">Отобразятся сопоставления по умолчанию между концептуальным и типом сущности **градерепорт** хранилища.</span><span class="sxs-lookup"><span data-stu-id="f8d09-183">Default mappings between the conceptual and storage **GradeReport** entity type appear.</span></span>  
    <span data-ttu-id="f8d09-184">![сопоставление Details3](~/ef6/media/mappingdetails.png)</span><span class="sxs-lookup"><span data-stu-id="f8d09-184">![Mapping Details3](~/ef6/media/mappingdetails.png)</span></span>

<span data-ttu-id="f8d09-185">В результате элемент **EntitySetMapping** добавляется в раздел сопоставления EDMX-файла.</span><span class="sxs-lookup"><span data-stu-id="f8d09-185">As a result, the **EntitySetMapping** element is added to the mapping section of the .edmx file.</span></span> 

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

-   <span data-ttu-id="f8d09-186">Скомпилируйте приложение.</span><span class="sxs-lookup"><span data-stu-id="f8d09-186">Compile the application.</span></span>

 

## <a name="call-the-defining-query-in-your-code"></a><span data-ttu-id="f8d09-187">Вызов определяющего запроса в коде</span><span class="sxs-lookup"><span data-stu-id="f8d09-187">Call the Defining Query in your Code</span></span>

<span data-ttu-id="f8d09-188">Теперь можно выполнить определяющий запрос с помощью типа сущности **градерепорт** .</span><span class="sxs-lookup"><span data-stu-id="f8d09-188">You can now execute the defining query by using the **GradeReport** entity type.</span></span> 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
