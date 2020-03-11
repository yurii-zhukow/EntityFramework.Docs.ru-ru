---
title: Связи — конструктор EF — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: d429c39dafbf183caabdc85748c188deb8dd6f66
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415079"
---
# <a name="relationships---ef-designer"></a><span data-ttu-id="30317-102">Связи — конструктор EF</span><span class="sxs-lookup"><span data-stu-id="30317-102">Relationships - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="30317-103">На этой странице содержатся сведения о настройке связей в модели с помощью конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="30317-103">This page provides information about setting up relationships in your model using the EF Designer.</span></span> <span data-ttu-id="30317-104">Общие сведения о связях в EF и доступе к данным и управлении ими с помощью связей см. в разделе [связи & свойства навигации](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="30317-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>

<span data-ttu-id="30317-105">Ассоциации определяют связи между типами сущностей в модели.</span><span class="sxs-lookup"><span data-stu-id="30317-105">Associations define relationships between entity types in a model.</span></span> <span data-ttu-id="30317-106">В этом разделе показано, как сопоставлять ассоциации с Entity Framework Designer (конструктор EF).</span><span class="sxs-lookup"><span data-stu-id="30317-106">This topic shows how to map associations with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="30317-107">На следующем рисунке показаны основные окна, используемые при работе с конструктором EF.</span><span class="sxs-lookup"><span data-stu-id="30317-107">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![Конструктор EF](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="30317-109">При построении концептуальной модели в окне Список ошибок могут появиться предупреждения о несопоставленных сущностях и ассоциациях.</span><span class="sxs-lookup"><span data-stu-id="30317-109">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="30317-110">Эти предупреждения можно игнорировать, так как после выбора создания базы данных из модели ошибки будут исчезнуть.</span><span class="sxs-lookup"><span data-stu-id="30317-110">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="associations-overview"></a><span data-ttu-id="30317-111">Общие сведения о ассоциациях</span><span class="sxs-lookup"><span data-stu-id="30317-111">Associations Overview</span></span>

<span data-ttu-id="30317-112">При проектировании модели с помощью конструктора EF файл. EDMX представляет вашу модель.</span><span class="sxs-lookup"><span data-stu-id="30317-112">When you design your model using the EF Designer, an .edmx file represents your model.</span></span> <span data-ttu-id="30317-113">В EDMX-файле элемент **Association** определяет связь между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="30317-113">In the .edmx file, an **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="30317-114">Ассоциация должна указывать типы сущностей, которые участвуют в связи, и возможное количество типов сущностей на каждом конце связи, которое называется кратностью.</span><span class="sxs-lookup"><span data-stu-id="30317-114">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="30317-115">Кратность элемента ассоциации может иметь значение один (1), ноль или один (0.. 1) или многие (\*).</span><span class="sxs-lookup"><span data-stu-id="30317-115">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="30317-116">Эти сведения указываются в **двух дочерних** элементах.</span><span class="sxs-lookup"><span data-stu-id="30317-116">This information is specified in two child **End** elements.</span></span>

<span data-ttu-id="30317-117">Во время выполнения экземпляры типа сущности на одном конце ассоциации могут быть доступны через свойства навигации или внешние ключи (если вы решили предоставить внешние ключи в сущностях).</span><span class="sxs-lookup"><span data-stu-id="30317-117">At run time, entity type instances at one end of an association can be accessed through navigation properties or foreign keys (if you choose to expose foreign keys in your entities).</span></span> <span data-ttu-id="30317-118">При предоставлении внешних ключей связь между сущностями управляется с помощью элемента **ReferentialConstraint** (дочерний элемент элемента **Association** ).</span><span class="sxs-lookup"><span data-stu-id="30317-118">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element (a child element of the **Association** element).</span></span> <span data-ttu-id="30317-119">Рекомендуется всегда предоставлять внешние ключи для связей в сущностях.</span><span class="sxs-lookup"><span data-stu-id="30317-119">It is recommended that you always expose foreign keys for relationships in your entities.</span></span>

> [!NOTE]
> <span data-ttu-id="30317-120">В отношении "многие ко многим" (\*:\*) нельзя добавлять внешние ключи в сущности.</span><span class="sxs-lookup"><span data-stu-id="30317-120">In many-to-many (\*:\*) you cannot add foreign keys to the entities.</span></span> <span data-ttu-id="30317-121">В связи \*:\* сведения о взаимосвязи управляются с помощью независимого объекта.</span><span class="sxs-lookup"><span data-stu-id="30317-121">In a \*:\* relationship, the association information is managed with an independent object.</span></span>

<span data-ttu-id="30317-122">Сведения об элементах CSDL (**ReferentialConstraint**, **Association**и т. д.) см. в [спецификации языка CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="30317-122">For information about CSDL elements (**ReferentialConstraint**, **Association**, etc.) see the [CSDL specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span>

## <a name="create-and-delete-associations"></a><span data-ttu-id="30317-123">Создание и удаление ассоциаций</span><span class="sxs-lookup"><span data-stu-id="30317-123">Create and Delete Associations</span></span>

<span data-ttu-id="30317-124">При создании связи с помощью конструктора EF содержимое модели обновляется в файле EDMX.</span><span class="sxs-lookup"><span data-stu-id="30317-124">Creating an association with the EF Designer updates the model content of the .edmx file.</span></span> <span data-ttu-id="30317-125">После создания ассоциации необходимо создать сопоставления для ассоциации (рассматривается далее в этом разделе).</span><span class="sxs-lookup"><span data-stu-id="30317-125">After creating an association, you must create the mappings for the association (discussed later in this topic).</span></span>

> [!NOTE]
> <span data-ttu-id="30317-126">В этом разделе предполагается, что вы уже добавили сущности, между которыми вы хотите создать связь, с моделью.</span><span class="sxs-lookup"><span data-stu-id="30317-126">This section assumes that you already added the entities you wish to create an association between to your model.</span></span>

### <a name="to-create-an-association"></a><span data-ttu-id="30317-127">Создание ассоциации</span><span class="sxs-lookup"><span data-stu-id="30317-127">To create an association</span></span>

1.  <span data-ttu-id="30317-128">Щелкните правой кнопкой мыши пустую область области конструктора, наведите указатель на пункт **Добавить новый**и выберите **связь...** .</span><span class="sxs-lookup"><span data-stu-id="30317-128">Right-click an empty area of the design surface, point to **Add New**, and select **Association…**.</span></span>
2.  <span data-ttu-id="30317-129">Заполните параметры сопоставления в диалоговом окне **Добавление ассоциации** .</span><span class="sxs-lookup"><span data-stu-id="30317-129">Fill in the settings for the association in the **Add Association** dialog.</span></span>

    ![Добавить ассоциацию](~/ef6/media/addassociation.png)

    > [!NOTE]
    > <span data-ttu-id="30317-131">Вы можете не добавлять свойства навигации или свойства внешнего ключа в сущности на концах ассоциации, сняв **свойство навигации **и **добавив свойства внешнего ключа в &lt;имя типа сущности&gt; **флажки сущности.</span><span class="sxs-lookup"><span data-stu-id="30317-131">You can choose to not add navigation properties or foreign key properties to the entities at the ends of the association by clearing the **Navigation Property **and **Add foreign key properties to the &lt;entity type name&gt; Entity **checkboxes.</span></span> <span data-ttu-id="30317-132">При добавлении одного свойства навигации ассоциацию можно будет перемещать только в одном направлении.</span><span class="sxs-lookup"><span data-stu-id="30317-132">If you add only one navigation property, the association will be traversable in only one direction.</span></span> <span data-ttu-id="30317-133">При добавлении свойств навигации необходимо добавить свойства внешнего ключа, чтобы обеспечить доступ к сущностям в элементах ассоциации.</span><span class="sxs-lookup"><span data-stu-id="30317-133">If you add no navigation properties, you must choose to add foreign key properties in order to access entities at the ends of the association.</span></span>
    
3.  <span data-ttu-id="30317-134">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="30317-134">Click **OK**.</span></span>

### <a name="to-delete-an-association"></a><span data-ttu-id="30317-135">Удаление ассоциации</span><span class="sxs-lookup"><span data-stu-id="30317-135">To delete an association</span></span>

<span data-ttu-id="30317-136">Чтобы удалить связь, выполните одно из следующих действий.</span><span class="sxs-lookup"><span data-stu-id="30317-136">To delete an association do one of the following:</span></span>

-   <span data-ttu-id="30317-137">Щелкните ассоциацию правой кнопкой мыши в области конструктора EF и выберите **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="30317-137">Right-click the association on the EF Designer surface and select **Delete**.</span></span>

- <span data-ttu-id="30317-138">OR -</span><span class="sxs-lookup"><span data-stu-id="30317-138">OR -</span></span>

-   <span data-ttu-id="30317-139">Выберите одну или несколько ассоциаций и нажмите клавишу DELETE.</span><span class="sxs-lookup"><span data-stu-id="30317-139">Select one or more associations and press the DELETE key.</span></span>

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a><span data-ttu-id="30317-140">Включить свойства внешнего ключа в сущности (ссылочные ограничения)</span><span class="sxs-lookup"><span data-stu-id="30317-140">Include Foreign Key Properties in Your Entities (Referential Constraints)</span></span>

<span data-ttu-id="30317-141">Рекомендуется всегда предоставлять внешние ключи для связей в сущностях.</span><span class="sxs-lookup"><span data-stu-id="30317-141">It is recommended that you always expose foreign keys for relationships in your entities.</span></span> <span data-ttu-id="30317-142">Entity Framework использует ссылочное ограничение, чтобы выяснить, что свойство выступает в качестве внешнего ключа для связи.</span><span class="sxs-lookup"><span data-stu-id="30317-142">Entity Framework uses a referential constraint to identify that a property acts as the foreign key for a relationship.</span></span>

<span data-ttu-id="30317-143">Если установить флажок ***Добавить свойства внешнего ключа в &lt;имя типа сущности&gt; сущность*** при создании связи, это ссылочное ограничение было добавлено за вас.</span><span class="sxs-lookup"><span data-stu-id="30317-143">If you checked the ***Add foreign key properties to the &lt;entity type name&gt; Entity*** checkbox when creating a relationship, this referential constraint was added for you.</span></span>

<span data-ttu-id="30317-144">При использовании конструктора EF для добавления или изменения ссылочного ограничения конструктор EF добавляет или изменяет элемент **ReferentialConstraint** в CSDL-файле EDMX.</span><span class="sxs-lookup"><span data-stu-id="30317-144">When you use the EF Designer to add or edit a referential constraint, the EF Designer adds or modifies a **ReferentialConstraint** element in the CSDL content of the .edmx file.</span></span>

-   <span data-ttu-id="30317-145">Дважды щелкните ассоциацию, которую необходимо изменить.</span><span class="sxs-lookup"><span data-stu-id="30317-145">Double-click the association that you want to edit.</span></span>
    <span data-ttu-id="30317-146">Откроется диалоговое окно **ссылочное ограничение** .</span><span class="sxs-lookup"><span data-stu-id="30317-146">The **Referential Constraint** dialog box appears.</span></span>
-   <span data-ttu-id="30317-147">В раскрывающемся списке **основной** выберите сущность субъекта в справочном ограничении.</span><span class="sxs-lookup"><span data-stu-id="30317-147">From the **Principal** drop-down list, select the principal entity in the referential constraint.</span></span>
    <span data-ttu-id="30317-148">Ключевые свойства сущности добавляются в список  **основного ключа** в диалоговом окне.</span><span class="sxs-lookup"><span data-stu-id="30317-148">The entity's key properties are added to the **Principal Key** list in the dialog box.</span></span>
-   <span data-ttu-id="30317-149">В раскрывающемся списке **зависимые** выберите зависимую сущность в справочном ограничении.</span><span class="sxs-lookup"><span data-stu-id="30317-149">From the **Dependent** drop-down list, select the dependent entity in the referential constraint.</span></span>
-   <span data-ttu-id="30317-150">Для каждого ключа субъекта, имеющего зависимый ключ, выберите соответствующий зависимый ключ из раскрывающихся списков в столбце **зависимого ключа** .</span><span class="sxs-lookup"><span data-stu-id="30317-150">For each principal key that has a dependent key, select a corresponding dependent key from the drop-down lists in the **Dependent Key** column.</span></span>

    ![Ограничение ref](~/ef6/media/refconstraint.png)

-   <span data-ttu-id="30317-152">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="30317-152">Click **OK**.</span></span>

## <a name="create-and-edit-association-mappings"></a><span data-ttu-id="30317-153">создавать и изменять сопоставления ассоциаций</span><span class="sxs-lookup"><span data-stu-id="30317-153">Create and Edit Association Mappings</span></span>

<span data-ttu-id="30317-154">Можно указать, как Ассоциация сопоставляется с базой данных в окне **сведения о сопоставлении** в конструкторе EF.</span><span class="sxs-lookup"><span data-stu-id="30317-154">You can specify how an association maps to the database in the **Mapping Details** window of the EF Designer.</span></span>

> [!NOTE]
> <span data-ttu-id="30317-155">Можно сопоставлять только сведения для ассоциаций, для которых не задано ссылочное ограничение.</span><span class="sxs-lookup"><span data-stu-id="30317-155">You can only map details for the associations that do not have a referential constraint specified.</span></span> <span data-ttu-id="30317-156">Если указано ссылочное ограничение, свойство внешнего ключа включается в сущность и можно использовать сведения о сопоставлении для сущности, чтобы контролировать, к какому столбцу сопоставлен внешний ключ.</span><span class="sxs-lookup"><span data-stu-id="30317-156">If a referential constraint is specified then a foreign key property is included in the entity and you can use the Mapping Details for the entity to control which column the foreign key maps to.</span></span>

### <a name="create-an-association-mapping"></a><span data-ttu-id="30317-157">Создание сопоставления ассоциации</span><span class="sxs-lookup"><span data-stu-id="30317-157">Create an association mapping</span></span>

-   <span data-ttu-id="30317-158">Щелкните правой кнопкой мыши ассоциацию в области конструктора и выберите **Сопоставление таблиц**.</span><span class="sxs-lookup"><span data-stu-id="30317-158">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="30317-159">Отобразится сопоставление ассоциации в окне **сведения о Сопоставлении** .</span><span class="sxs-lookup"><span data-stu-id="30317-159">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="30317-160">Нажмите кнопку **Добавить таблицу или представление**.</span><span class="sxs-lookup"><span data-stu-id="30317-160">Click **Add a Table or View**.</span></span>
    <span data-ttu-id="30317-161">Появится раскрывающийся список, содержащий все таблицы в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="30317-161">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="30317-162">Выберите таблицу, с которой будет сопоставлена ассоциация.</span><span class="sxs-lookup"><span data-stu-id="30317-162">Select the table to which the association will map.</span></span>
    <span data-ttu-id="30317-163">В окне **сведения о Сопоставлении** отображаются оба конца ассоциации и ключевые свойства для типа сущности в каждой **стороне**.</span><span class="sxs-lookup"><span data-stu-id="30317-163">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each **End**.</span></span>
-   <span data-ttu-id="30317-164">Для каждого ключевого свойства щелкните **столбец** поле и выберите столбец, с которым будет сопоставляться свойство.</span><span class="sxs-lookup"><span data-stu-id="30317-164">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

    ![Сведения о сопоставлении 4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a><span data-ttu-id="30317-166">Изменение сопоставления ассоциации</span><span class="sxs-lookup"><span data-stu-id="30317-166">Edit an association mapping</span></span>

-   <span data-ttu-id="30317-167">Щелкните правой кнопкой мыши ассоциацию в области конструктора и выберите **Сопоставление таблиц**.</span><span class="sxs-lookup"><span data-stu-id="30317-167">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="30317-168">Отобразится сопоставление ассоциации в окне **сведения о Сопоставлении** .</span><span class="sxs-lookup"><span data-stu-id="30317-168">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="30317-169">Щелкните **Maps, чтобы &lt;имя таблицы&gt;** .</span><span class="sxs-lookup"><span data-stu-id="30317-169">Click **Maps to &lt;Table Name&gt;**.</span></span>
    <span data-ttu-id="30317-170">Появится раскрывающийся список, содержащий все таблицы в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="30317-170">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="30317-171">Выберите таблицу, с которой будет сопоставлена ассоциация.</span><span class="sxs-lookup"><span data-stu-id="30317-171">Select the table to which the association will map.</span></span>
    <span data-ttu-id="30317-172">В окне **сведения о Сопоставлении** отображаются оба конца ассоциации и ключевые свойства для типа сущности в каждой стороне.</span><span class="sxs-lookup"><span data-stu-id="30317-172">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each End.</span></span>
-   <span data-ttu-id="30317-173">Для каждого ключевого свойства щелкните **столбец** поле и выберите столбец, с которым будет сопоставляться свойство.</span><span class="sxs-lookup"><span data-stu-id="30317-173">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

## <a name="edit-and-delete-navigation-properties"></a><span data-ttu-id="30317-174">Изменение и удаление свойств навигации</span><span class="sxs-lookup"><span data-stu-id="30317-174">Edit and Delete Navigation Properties</span></span>

<span data-ttu-id="30317-175">Свойства навигации — это свойства ярлыка, используемые для поиска сущностей на концах ассоциации в модели.</span><span class="sxs-lookup"><span data-stu-id="30317-175">Navigation properties are shortcut properties that are used to locate the entities at the ends of an association in a model.</span></span> <span data-ttu-id="30317-176">Свойства навигации могут быть созданы при создании ассоциации между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="30317-176">Navigation properties can be created when you create an association between two entity types.</span></span>

#### <a name="to-edit-navigation-properties"></a><span data-ttu-id="30317-177">Изменение свойств навигации</span><span class="sxs-lookup"><span data-stu-id="30317-177">To edit navigation properties</span></span>

-   <span data-ttu-id="30317-178">Выберите свойство навигации в области конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="30317-178">Select a navigation property on the EF Designer surface.</span></span>
    <span data-ttu-id="30317-179">Сведения о свойстве навигации отображаются в окне  **Свойства** Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="30317-179">Information about the navigation property is displayed in the Visual Studio **Properties** window.</span></span>
-   <span data-ttu-id="30317-180">Измените значения свойств в окне **свойства** .</span><span class="sxs-lookup"><span data-stu-id="30317-180">Change the property settings in the **Properties** window.</span></span>

#### <a name="to-delete-navigation-properties"></a><span data-ttu-id="30317-181">Удаление свойств навигации</span><span class="sxs-lookup"><span data-stu-id="30317-181">To delete navigation properties</span></span>

-   <span data-ttu-id="30317-182">Если внешние ключи не представлены в типах сущностей в концептуальной модели, то удаление свойства навигации может обеспечить перемещение соответствующей ассоциации только в одном направлении или сделать ее неперемещаемой.</span><span class="sxs-lookup"><span data-stu-id="30317-182">If foreign keys are not exposed on entity types in the conceptual model, deleting a navigation property may make the corresponding association traversable in only one direction or not traversable at all.</span></span>
-   <span data-ttu-id="30317-183">Щелкните правой кнопкой мыши свойство навигации в области конструктора EF и выберите **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="30317-183">Right-click a navigation property on the EF Designer surface and select **Delete**.</span></span>
