---
title: Определение запроса - конструктор EF - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: 60d5310589bb9bc3fdb971673422e80537357e55
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996311"
---
# <a name="defining-query---ef-designer"></a><span data-ttu-id="0bb52-102">Определяющий запрос - конструктор EF</span><span class="sxs-lookup"><span data-stu-id="0bb52-102">Defining Query - EF Designer</span></span>
<span data-ttu-id="0bb52-103">В этом пошаговом руководстве показан способ добавления определяющего запроса и соответствующей сущности, тип модели, в конструкторе EF.</span><span class="sxs-lookup"><span data-stu-id="0bb52-103">This walkthrough demonstrates how to add a defining query and a corresponding entity type to a model using the EF Designer.</span></span> <span data-ttu-id="0bb52-104">Определяющий запрос обычно используется для предоставления функций, которые аналогичны предоставляемым представления базы данных, но оно определено в модели, а не в базе данных.</span><span class="sxs-lookup"><span data-stu-id="0bb52-104">A defining query is commonly used to provide functionality similar to that provided by a database view, but the view is defined in the model, not the database.</span></span> <span data-ttu-id="0bb52-105">Определяющий запрос позволяет выполнить инструкцию SQL, который указан в **DefiningQuery** элемент EDMX-файла.</span><span class="sxs-lookup"><span data-stu-id="0bb52-105">A defining query allows you to execute a SQL statement that is specified in the **DefiningQuery** element of an .edmx file.</span></span> <span data-ttu-id="0bb52-106">Дополнительные сведения см. в разделе **DefiningQuery** в [спецификация SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="0bb52-106">For more information, see **DefiningQuery** in the [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="0bb52-107">При использовании определение запросов, необходимо также определить тип сущности в модели.</span><span class="sxs-lookup"><span data-stu-id="0bb52-107">When using defining queries, you also have to define an entity type in your model.</span></span> <span data-ttu-id="0bb52-108">Тип сущности используется для отображения данных с помощью определения запроса.</span><span class="sxs-lookup"><span data-stu-id="0bb52-108">The entity type is used to surface data exposed by the defining query.</span></span> <span data-ttu-id="0bb52-109">Обратите внимание, что данные, отображаемые через этот тип сущности только для чтения.</span><span class="sxs-lookup"><span data-stu-id="0bb52-109">Note that data surfaced through this entity type is read-only.</span></span>

<span data-ttu-id="0bb52-110">Параметризованные запросы не могут выполняться как определяющие запросы.</span><span class="sxs-lookup"><span data-stu-id="0bb52-110">Parameterized queries cannot be executed as defining queries.</span></span> <span data-ttu-id="0bb52-111">Однако их обновление возможно через сопоставление функций вставки, обновления и удаления типа сущности, который отображает данные в хранимых процедурах.</span><span class="sxs-lookup"><span data-stu-id="0bb52-111">However, the data can be updated by mapping the insert, update, and delete functions of the entity type that surfaces the data to stored procedures.</span></span> <span data-ttu-id="0bb52-112">Дополнительные сведения см. в разделе [вставки, обновления и удаления с помощью хранимых процедур](~/ef6/modeling/designer/stored-procedures/cud.md).</span><span class="sxs-lookup"><span data-stu-id="0bb52-112">For more information, see [Insert, Update, and Delete with Stored Procedures](~/ef6/modeling/designer/stored-procedures/cud.md).</span></span>

<span data-ttu-id="0bb52-113">В этом разделе показано, как выполнять следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="0bb52-113">This topic shows how to perform the following tasks.</span></span>

-   <span data-ttu-id="0bb52-114">Добавление определяющего запроса</span><span class="sxs-lookup"><span data-stu-id="0bb52-114">Add a Defining Query</span></span>
-   <span data-ttu-id="0bb52-115">Добавление типа сущности в модель</span><span class="sxs-lookup"><span data-stu-id="0bb52-115">Add an Entity Type to the Model</span></span>
-   <span data-ttu-id="0bb52-116">Тип сущности сопоставляются с определяющего запроса</span><span class="sxs-lookup"><span data-stu-id="0bb52-116">Map the Defining Query to the Entity Type</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0bb52-117">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="0bb52-117">Prerequisites</span></span>

<span data-ttu-id="0bb52-118">Для выполнения данного пошагового руководства требуется:</span><span class="sxs-lookup"><span data-stu-id="0bb52-118">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="0bb52-119">Последнюю версию Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0bb52-119">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="0bb52-120">[Образца базы данных School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="0bb52-120">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="0bb52-121">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="0bb52-121">Set up the Project</span></span>

<span data-ttu-id="0bb52-122">В этом пошаговом руководстве используется Visual Studio 2012 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="0bb52-122">This walkthrough is using Visual Studio 2012 or newer.</span></span>

-   <span data-ttu-id="0bb52-123">Запустите Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0bb52-123">Open Visual Studio.</span></span>
-   <span data-ttu-id="0bb52-124">В меню **Файл** выберите пункт **Создать**, а затем команду **Проект**.</span><span class="sxs-lookup"><span data-stu-id="0bb52-124">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="0bb52-125">В левой области щелкните **Visual C\#**, а затем выберите **консольное приложение** шаблона.</span><span class="sxs-lookup"><span data-stu-id="0bb52-125">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="0bb52-126">Введите **DefiningQuerySample** как имя проекта и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="0bb52-126">Enter **DefiningQuerySample** as the name of the project and click **OK**.</span></span>

 

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="0bb52-127">Создать модель, основанную на базе данных School</span><span class="sxs-lookup"><span data-stu-id="0bb52-127">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="0bb52-128">Щелкните правой кнопкой мыши имя проекта в обозревателе решений, выберите пункт **добавить**, а затем нажмите кнопку **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="0bb52-128">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="0bb52-129">Выберите **данных** меню слева, а затем выберите **ADO.NET Entity Data Model** в области шаблонов.</span><span class="sxs-lookup"><span data-stu-id="0bb52-129">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="0bb52-130">Введите **DefiningQueryModel.edmx** имя файла, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="0bb52-130">Enter **DefiningQueryModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="0bb52-131">В диалоговом окне Выбор содержимого модели выберите **создать из базы данных**, а затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="0bb52-131">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="0bb52-132">Щелкните новое подключение.</span><span class="sxs-lookup"><span data-stu-id="0bb52-132">Click New Connection.</span></span> <span data-ttu-id="0bb52-133">В диалоговом окне Свойства соединения, введите имя сервера (например, **(localdb)\\mssqllocaldb**) выберите метод проверки подлинности, тип **School** для имени базы данных, а затем Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="0bb52-133">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="0bb52-134">В диалоговом окне Выбор подключения к базе данных обновляется параметр подключения базы данных.</span><span class="sxs-lookup"><span data-stu-id="0bb52-134">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="0bb52-135">В диалоговом окне Выбор объектов базы данных проверьте **таблиц** узла.</span><span class="sxs-lookup"><span data-stu-id="0bb52-135">In the Choose Your Database Objects dialog box, check the **Tables** node.</span></span> <span data-ttu-id="0bb52-136">Это добавит все таблицы, которые **School** модели.</span><span class="sxs-lookup"><span data-stu-id="0bb52-136">This will add all the tables to the **School** model.</span></span>
-   <span data-ttu-id="0bb52-137">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="0bb52-137">Click **Finish**.</span></span>
-   <span data-ttu-id="0bb52-138">В обозревателе решений щелкните правой кнопкой мыши **DefiningQueryModel.edmx** файл и выберите **открыть с помощью...** .</span><span class="sxs-lookup"><span data-stu-id="0bb52-138">In Solution Explorer, right-click the **DefiningQueryModel.edmx** file and select **Open With…**.</span></span>
-   <span data-ttu-id="0bb52-139">Выберите **редактор (текстовый) XML**.</span><span class="sxs-lookup"><span data-stu-id="0bb52-139">Select **XML (Text) Editor**.</span></span>

    ![XMLEditor](~/ef6/media/xmleditor.png)

-   <span data-ttu-id="0bb52-141">Нажмите кнопку **Да** при появлении соответствующего запроса со следующим сообщением:</span><span class="sxs-lookup"><span data-stu-id="0bb52-141">Click **Yes** if prompted with the following message:</span></span>

    ![Warning2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a><span data-ttu-id="0bb52-143">Добавление определяющего запроса</span><span class="sxs-lookup"><span data-stu-id="0bb52-143">Add a Defining Query</span></span>

<span data-ttu-id="0bb52-144">На этом шаге мы воспользуемся редактора XML для добавления определяющего запроса и тип сущности в разделе SSDL EDMX-файла.</span><span class="sxs-lookup"><span data-stu-id="0bb52-144">In this step we will use the XML Editor to add a defining query and an entity type to the SSDL section of the .edmx file.</span></span> 

-   <span data-ttu-id="0bb52-145">Добавить **EntitySet** элемент в раздел SSDL EDMX-файла (строка 5 по 13).</span><span class="sxs-lookup"><span data-stu-id="0bb52-145">Add an **EntitySet** element to the SSDL section of the .edmx file (line 5 thru 13).</span></span> <span data-ttu-id="0bb52-146">Укажите следующие сведения:</span><span class="sxs-lookup"><span data-stu-id="0bb52-146">Specify the following:</span></span>
    -   <span data-ttu-id="0bb52-147">Только **имя** и **EntityType** атрибуты **EntitySet** указанных элемента.</span><span class="sxs-lookup"><span data-stu-id="0bb52-147">Only the **Name** and **EntityType** attributes of the **EntitySet** element are specified.</span></span>
    -   <span data-ttu-id="0bb52-148">Полное имя типа сущности используется в **EntityType** атрибута.</span><span class="sxs-lookup"><span data-stu-id="0bb52-148">The fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="0bb52-149">Инструкция SQL для выполнения указана в **DefiningQuery** элемент.</span><span class="sxs-lookup"><span data-stu-id="0bb52-149">The SQL statement to be executed is specified in the **DefiningQuery** element.</span></span>

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

-   <span data-ttu-id="0bb52-150">Добавить **EntityType** элемент в раздел SSDL EDMX-файла.</span><span class="sxs-lookup"><span data-stu-id="0bb52-150">Add the **EntityType** element to the SSDL section of the .edmx.</span></span> <span data-ttu-id="0bb52-151">файл, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="0bb52-151">file as shown below.</span></span> <span data-ttu-id="0bb52-152">Обратите внимание на следующее условия:</span><span class="sxs-lookup"><span data-stu-id="0bb52-152">Note the following:</span></span>
    -   <span data-ttu-id="0bb52-153">Значение **имя** атрибут соответствует значению **EntityType** атрибут в **EntitySet** элемент выше, несмотря на то что полное доменное имя Тип сущности используется в **EntityType** атрибута.</span><span class="sxs-lookup"><span data-stu-id="0bb52-153">The value of the **Name** attribute corresponds to the value of the **EntityType** attribute in the **EntitySet** element above, although the fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="0bb52-154">Имена свойств соответствуют именам столбцов, возвращаемых инструкцией SQL в **DefiningQuery** элемент (см. выше).</span><span class="sxs-lookup"><span data-stu-id="0bb52-154">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element (above).</span></span>
    -   <span data-ttu-id="0bb52-155">В данном примере ключ сущности состоит из трех свойств, что обеспечивает уникальность значения ключа.</span><span class="sxs-lookup"><span data-stu-id="0bb52-155">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

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
> <span data-ttu-id="0bb52-156">Если вы запустите **мастер обновления моделей** диалоговое окно, любые изменения, внесенные в модель хранения, включая определение запросов, будут перезаписаны.</span><span class="sxs-lookup"><span data-stu-id="0bb52-156">If later you run the **Update Model Wizard** dialog, any changes made to the storage model, including defining queries, will be overwritten.</span></span>

 

## <a name="add-an-entity-type-to-the-model"></a><span data-ttu-id="0bb52-157">Добавление типа сущности в модель</span><span class="sxs-lookup"><span data-stu-id="0bb52-157">Add an Entity Type to the Model</span></span>

<span data-ttu-id="0bb52-158">На этом этапе мы добавим тип сущности в концептуальную модель, в конструкторе EF.</span><span class="sxs-lookup"><span data-stu-id="0bb52-158">In this step we will add the entity type to the conceptual model using the EF Designer.</span></span>  <span data-ttu-id="0bb52-159">Обратите внимание на следующее условия:</span><span class="sxs-lookup"><span data-stu-id="0bb52-159">Note the following:</span></span>

-   <span data-ttu-id="0bb52-160">**Имя** сущности соответствует значению **EntityType** атрибут в **EntitySet** элемент выше.</span><span class="sxs-lookup"><span data-stu-id="0bb52-160">The **Name** of the entity corresponds to the value of the **EntityType** attribute in the **EntitySet** element above.</span></span>
-   <span data-ttu-id="0bb52-161">Имена свойств соответствуют именам столбцов, возвращаемых инструкцией SQL в **DefiningQuery** элемент выше.</span><span class="sxs-lookup"><span data-stu-id="0bb52-161">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element above.</span></span>
-   <span data-ttu-id="0bb52-162">В данном примере ключ сущности состоит из трех свойств, что обеспечивает уникальность значения ключа.</span><span class="sxs-lookup"><span data-stu-id="0bb52-162">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

<span data-ttu-id="0bb52-163">Откройте модель в конструкторе EF.</span><span class="sxs-lookup"><span data-stu-id="0bb52-163">Open the model in the EF Designer.</span></span>

-   <span data-ttu-id="0bb52-164">Дважды щелкните DefiningQueryModel.edmx.</span><span class="sxs-lookup"><span data-stu-id="0bb52-164">Double-click the DefiningQueryModel.edmx.</span></span>
-   <span data-ttu-id="0bb52-165">Скажем **Да** следующее сообщение:</span><span class="sxs-lookup"><span data-stu-id="0bb52-165">Say **Yes** to the following message:</span></span>

    ![Warning2](~/ef6/media/warning2.png)

 

<span data-ttu-id="0bb52-167">Конструктор сущностей, который предоставляет область конструктора для изменения модели, отображается.</span><span class="sxs-lookup"><span data-stu-id="0bb52-167">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

-   <span data-ttu-id="0bb52-168">Щелкните правой кнопкой мыши в конструкторе и выберите пункт **Add New**-&gt;**сущности...** .</span><span class="sxs-lookup"><span data-stu-id="0bb52-168">Right-click the designer surface and select **Add New**-&gt;**Entity…**.</span></span>
-   <span data-ttu-id="0bb52-169">Укажите **GradeReport** имя сущности и **CourseID** для **свойство Key**.</span><span class="sxs-lookup"><span data-stu-id="0bb52-169">Specify **GradeReport** for the entity name and **CourseID** for the **Key Property**.</span></span>
-   <span data-ttu-id="0bb52-170">Щелкните правой кнопкой мыши **GradeReport** сущности и выберите **Add New** - &gt; **скалярное свойство**.</span><span class="sxs-lookup"><span data-stu-id="0bb52-170">Right-click the **GradeReport** entity and select **Add New**-&gt; **Scalar Property**.</span></span>
-   <span data-ttu-id="0bb52-171">Изменить имя по умолчанию свойства **FirstName**.</span><span class="sxs-lookup"><span data-stu-id="0bb52-171">Change the default name of the property to **FirstName**.</span></span>
-   <span data-ttu-id="0bb52-172">Добавить еще одно скалярное свойство и указать **LastName** для имени.</span><span class="sxs-lookup"><span data-stu-id="0bb52-172">Add another scalar property and specify **LastName** for the name.</span></span>
-   <span data-ttu-id="0bb52-173">Добавить еще одно скалярное свойство и указать **корпоративного класса** для имени.</span><span class="sxs-lookup"><span data-stu-id="0bb52-173">Add another scalar property and specify **Grade** for the name.</span></span>
-   <span data-ttu-id="0bb52-174">В **свойства** измените **корпоративного класса** **тип** свойства **десятичное**.</span><span class="sxs-lookup"><span data-stu-id="0bb52-174">In the **Properties** window, change the **Grade**’s **Type** property to **Decimal**.</span></span>
-   <span data-ttu-id="0bb52-175">Выберите **FirstName** и **LastName** свойства.</span><span class="sxs-lookup"><span data-stu-id="0bb52-175">Select the **FirstName** and **LastName** properties.</span></span>
-   <span data-ttu-id="0bb52-176">В **свойства** измените **EntityKey** значение свойства **True**.</span><span class="sxs-lookup"><span data-stu-id="0bb52-176">In the **Properties** window, change the **EntityKey** property value to **True**.</span></span>

<span data-ttu-id="0bb52-177">Таким образом, были добавлены следующие элементы **CSDL** раздел EDMX-файла.</span><span class="sxs-lookup"><span data-stu-id="0bb52-177">As a result, the following elements were added to the **CSDL** section of the .edmx file.</span></span>

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a><span data-ttu-id="0bb52-178">Тип сущности сопоставляются с определяющего запроса</span><span class="sxs-lookup"><span data-stu-id="0bb52-178">Map the Defining Query to the Entity Type</span></span>

<span data-ttu-id="0bb52-179">На этом шаге мы будем использовать окно Mapping Details для сопоставления концептуальной и типа сущности хранилища.</span><span class="sxs-lookup"><span data-stu-id="0bb52-179">In this step, we will use the Mapping Details window to map the conceptual and storage entity types.</span></span>

-   <span data-ttu-id="0bb52-180">Щелкните правой кнопкой мыши **GradeReport** сущности в область конструктора и выберите **сопоставление таблицы**.</span><span class="sxs-lookup"><span data-stu-id="0bb52-180">Right-click the **GradeReport** entity on the design surface and select **Table Mapping**.</span></span>  
    <span data-ttu-id="0bb52-181">**Сведения о сопоставлении** откроется диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="0bb52-181">The **Mapping Details** window is displayed.</span></span>
-   <span data-ttu-id="0bb52-182">Выберите **GradeReport** из **&lt;добавить таблицу или представление&gt;** раскрывающегося списка (расположенный в **таблицы**s).</span><span class="sxs-lookup"><span data-stu-id="0bb52-182">Select **GradeReport** from the **&lt;Add a Table or View&gt;** dropdown list (located under **Table**s).</span></span>  
    <span data-ttu-id="0bb52-183">По умолчанию сопоставления между концептуальной и хранения **GradeReport** отображаются тип сущности.</span><span class="sxs-lookup"><span data-stu-id="0bb52-183">Default mappings between the conceptual and storage **GradeReport** entity type appear.</span></span>  
    <span data-ttu-id="0bb52-184">![MappingDetails3](~/ef6/media/mappingdetails.png)</span><span class="sxs-lookup"><span data-stu-id="0bb52-184">![MappingDetails3](~/ef6/media/mappingdetails.png)</span></span>

<span data-ttu-id="0bb52-185">В результате **EntitySetMapping** элемент добавляется в раздел сопоставления EDMX-файла.</span><span class="sxs-lookup"><span data-stu-id="0bb52-185">As a result, the **EntitySetMapping** element is added to the mapping section of the .edmx file.</span></span> 

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

-   <span data-ttu-id="0bb52-186">Скомпилируйте приложение.</span><span class="sxs-lookup"><span data-stu-id="0bb52-186">Compile the application.</span></span>

 

## <a name="call-the-defining-query-in-your-code"></a><span data-ttu-id="0bb52-187">Вызывать определяющего запроса в коде</span><span class="sxs-lookup"><span data-stu-id="0bb52-187">Call the Defining Query in your Code</span></span>

<span data-ttu-id="0bb52-188">Теперь, определяющий запрос можно выполнить с помощью **GradeReport** типа сущности.</span><span class="sxs-lookup"><span data-stu-id="0bb52-188">You can now execute the defining query by using the **GradeReport** entity type.</span></span> 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
