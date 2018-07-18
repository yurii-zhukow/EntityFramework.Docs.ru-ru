---
title: Связей - конструктор EF - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
caps.latest.revision: 3
ms.openlocfilehash: f924945b19dd6d73847ff3ec52c0b5a286c591bb
ms.sourcegitcommit: 9ae4473425c5e76337c9d032b0e5dbfedf1fcf57
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122718"
---
# <a name="relationships---ef-designer"></a><span data-ttu-id="d307f-102">Связей - конструктор EF</span><span class="sxs-lookup"><span data-stu-id="d307f-102">Relationships - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="d307f-103">Эта страница содержит сведения о настройке связи в модели в конструкторе EF.</span><span class="sxs-lookup"><span data-stu-id="d307f-103">This page provides information about setting up relationships in your model using the EF Designer.</span></span> <span data-ttu-id="d307f-104">Общие сведения о связях в EF и способах доступа к данных и управления ими с помощью связей см. в разделе [связей и свойств навигации](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="d307f-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>

<span data-ttu-id="d307f-105">Ассоциации определяют связи между типами сущностей в модели.</span><span class="sxs-lookup"><span data-stu-id="d307f-105">Associations define relationships between entity types in a model.</span></span> <span data-ttu-id="d307f-106">В этом разделе показано, как сопоставить ассоциации с Entity Framework Designer (конструктор EF).</span><span class="sxs-lookup"><span data-stu-id="d307f-106">This topic shows how to map associations with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="d307f-107">На следующем рисунке показана основные окна, которые используются при работе с конструктором EF.</span><span class="sxs-lookup"><span data-stu-id="d307f-107">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EFDesigner](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="d307f-109">При создании концептуальной модели, предупреждения о несопоставленных сущностях и ассоциациях, появляются в списке ошибок.</span><span class="sxs-lookup"><span data-stu-id="d307f-109">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="d307f-110">Эти предупреждения можно игнорировать, поскольку после нажатия кнопки для создания базы данных из модели, пропадут.</span><span class="sxs-lookup"><span data-stu-id="d307f-110">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="associations-overview"></a><span data-ttu-id="d307f-111">Общие сведения о связи</span><span class="sxs-lookup"><span data-stu-id="d307f-111">Associations Overview</span></span>

<span data-ttu-id="d307f-112">При разработке модели в конструкторе EF, EDMX-файл представляет модель.</span><span class="sxs-lookup"><span data-stu-id="d307f-112">When you design your model using the EF Designer, an .edmx file represents your model.</span></span> <span data-ttu-id="d307f-113">В файле EDMX **ассоциации** элемент определяет связь между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="d307f-113">In the .edmx file, an **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="d307f-114">Ассоциация должна указывать типы сущностей, которые участвуют в связи, и возможное количество типов сущностей на каждом конце связи, которое называется кратностью.</span><span class="sxs-lookup"><span data-stu-id="d307f-114">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="d307f-115">Кратность элемента ассоциации может иметь значение (1), ноль или один (0.. 1) или многие (\*).</span><span class="sxs-lookup"><span data-stu-id="d307f-115">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="d307f-116">Эта информация указывается в двух дочерних **окончания** элементов.</span><span class="sxs-lookup"><span data-stu-id="d307f-116">This information is specified in two child **End** elements.</span></span>

<span data-ttu-id="d307f-117">Во время выполнения экземпляров типа сущности на одном конце ассоциации может осуществляться через свойства навигации или внешних ключей (Если вы решили предоставляют внешние ключи в сущностях).</span><span class="sxs-lookup"><span data-stu-id="d307f-117">At run time, entity type instances at one end of an association can be accessed through navigation properties or foreign keys (if you choose to expose foreign keys in your entities).</span></span> <span data-ttu-id="d307f-118">Внешние ключи представлены, связи между сущностями, управляются с помощью **ReferentialConstraint** элемент (дочерний элемент элемента **ассоциации** элемент).</span><span class="sxs-lookup"><span data-stu-id="d307f-118">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element (a child element of the **Association** element).</span></span> <span data-ttu-id="d307f-119">Рекомендуется всегда предоставлять внешние ключи для связи в сущностях.</span><span class="sxs-lookup"><span data-stu-id="d307f-119">It is recommended that you always expose foreign keys for relationships in your entities.</span></span>

> [!NOTE]
> <span data-ttu-id="d307f-120">В многие ко многим (\*:\*) не удается добавить внешних ключей в сущности.</span><span class="sxs-lookup"><span data-stu-id="d307f-120">In many-to-many (\*:\*) you cannot add foreign keys to the entities.</span></span> <span data-ttu-id="d307f-121">В \*:\* связи, данные сопоставления управляется с помощью независимый объект.</span><span class="sxs-lookup"><span data-stu-id="d307f-121">In a \*:\* relationship, the association information is managed with an independent object.</span></span>

<span data-ttu-id="d307f-122">Дополнительные сведения об элементах языка CSDL (**ReferentialConstraint**, **ассоциации**т. д.) см. в разделе [спецификация языка CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="d307f-122">For information about CSDL elements (**ReferentialConstraint**, **Association**, etc.) see the [CSDL specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span>

## <a name="create-and-delete-associations"></a><span data-ttu-id="d307f-123">Создание и удаление ассоциации</span><span class="sxs-lookup"><span data-stu-id="d307f-123">Create and Delete Associations</span></span>

<span data-ttu-id="d307f-124">Создание ассоциации с обновлениями конструктор EF содержимое модели в EDMX-файле.</span><span class="sxs-lookup"><span data-stu-id="d307f-124">Creating an association with the EF Designer updates the model content of the .edmx file.</span></span> <span data-ttu-id="d307f-125">После создания ассоциации необходимо создать сопоставления для ассоциации (обсуждается далее в этом разделе).</span><span class="sxs-lookup"><span data-stu-id="d307f-125">After creating an association, you must create the mappings for the association (discussed later in this topic).</span></span>

> [!NOTE]
> <span data-ttu-id="d307f-126">В этом разделе предполагается, что вы уже добавили сущностей, которые вы хотите создать ассоциацию между для вашей модели.</span><span class="sxs-lookup"><span data-stu-id="d307f-126">This section assumes that you already added the entities you wish to create an association between to your model.</span></span>

### <a name="to-create-an-association"></a><span data-ttu-id="d307f-127">Создание ассоциации</span><span class="sxs-lookup"><span data-stu-id="d307f-127">To create an association</span></span>

1.  <span data-ttu-id="d307f-128">Щелкните правой кнопкой мыши пустой участок области конструктора, выберите пункт **Add New**и выберите **связи...** .</span><span class="sxs-lookup"><span data-stu-id="d307f-128">Right-click an empty area of the design surface, point to **Add New**, and select **Association…**.</span></span>
2.  <span data-ttu-id="d307f-129">Задайте параметры для ассоциации в **добавить сопоставление** диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="d307f-129">Fill in the settings for the association in the **Add Association** dialog.</span></span>

    ![AddAssociation](~/ef6/media/addassociation.png)

    > [!NOTE]
    > <span data-ttu-id="d307f-131">Вы можете не добавлять свойства навигации или свойства внешнего ключа для сущностей в элементах ассоциации, сбросив ** свойство навигации ** и ** добавить свойства внешнего ключа для &lt;имя типа сущности&gt; сущности ** флажки.</span><span class="sxs-lookup"><span data-stu-id="d307f-131">You can choose to not add navigation properties or foreign key properties to the entities at the ends of the association by clearing the **Navigation Property **and **Add foreign key properties to the &lt;entity type name&gt; Entity **checkboxes.</span></span> <span data-ttu-id="d307f-132">При добавлении одного свойства навигации ассоциацию можно будет перемещать только в одном направлении.</span><span class="sxs-lookup"><span data-stu-id="d307f-132">If you add only one navigation property, the association will be traversable in only one direction.</span></span> <span data-ttu-id="d307f-133">При добавлении свойств навигации необходимо добавить свойства внешнего ключа, чтобы обеспечить доступ к сущностям в элементах ассоциации.</span><span class="sxs-lookup"><span data-stu-id="d307f-133">If you add no navigation properties, you must choose to add foreign key properties in order to access entities at the ends of the association.</span></span>
    
3.  <span data-ttu-id="d307f-134">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="d307f-134">Click **OK**.</span></span>

### <a name="to-delete-an-association"></a><span data-ttu-id="d307f-135">Удаление ассоциации</span><span class="sxs-lookup"><span data-stu-id="d307f-135">To delete an association</span></span>

<span data-ttu-id="d307f-136">Чтобы удалить связь выполните одно из следующих:</span><span class="sxs-lookup"><span data-stu-id="d307f-136">To delete an association do one of the following:</span></span>

-   <span data-ttu-id="d307f-137">Щелкните правой кнопкой мыши связь в конструкторе EF и выберите пункт **удалить**.</span><span class="sxs-lookup"><span data-stu-id="d307f-137">Right-click the association on the EF Designer surface and select **Delete**.</span></span>

- <span data-ttu-id="d307f-138">OR-</span><span class="sxs-lookup"><span data-stu-id="d307f-138">OR -</span></span>

-   <span data-ttu-id="d307f-139">Выберите одну или несколько ассоциаций и нажмите клавишу DELETE.</span><span class="sxs-lookup"><span data-stu-id="d307f-139">Select one or more associations and press the DELETE key.</span></span>

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a><span data-ttu-id="d307f-140">Включить свойства внешнего ключа в сущности (ссылочные ограничения)</span><span class="sxs-lookup"><span data-stu-id="d307f-140">Include Foreign Key Properties in Your Entities (Referential Constraints)</span></span>

<span data-ttu-id="d307f-141">Рекомендуется всегда предоставлять внешние ключи для связи в сущностях.</span><span class="sxs-lookup"><span data-stu-id="d307f-141">It is recommended that you always expose foreign keys for relationships in your entities.</span></span> <span data-ttu-id="d307f-142">Entity Framework использует ссылочное ограничение, чтобы определить, что свойство действует как внешний ключ для связи.</span><span class="sxs-lookup"><span data-stu-id="d307f-142">Entity Framework uses a referential constraint to identify that a property acts as the foreign key for a relationship.</span></span>

<span data-ttu-id="d307f-143">Если вы установили флажок ***добавить свойства внешнего ключа для &lt;имя типа сущности&gt; сущности*** флажок был добавлен при создании связи этого справочного ограничения для вас.</span><span class="sxs-lookup"><span data-stu-id="d307f-143">If you checked the ***Add foreign key properties to the &lt;entity type name&gt; Entity*** checkbox when creating a relationship, this referential constraint was added for you.</span></span>

<span data-ttu-id="d307f-144">При использовании для добавления или изменения справочных ограничений конструкторе EF, EF добавляет или изменяет **ReferentialConstraint** элемент в содержимом CSDL EDMX-файла.</span><span class="sxs-lookup"><span data-stu-id="d307f-144">When you use the EF Designer to add or edit a referential constraint, the EF Designer adds or modifies a **ReferentialConstraint** element in the CSDL content of the .edmx file.</span></span>

-   <span data-ttu-id="d307f-145">Дважды щелкните ассоциацию, которую необходимо изменить.</span><span class="sxs-lookup"><span data-stu-id="d307f-145">Double-click the association that you want to edit.</span></span>
    <span data-ttu-id="d307f-146">**Справочного ограничения** откроется диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="d307f-146">The **Referential Constraint** dialog box appears.</span></span>
-   <span data-ttu-id="d307f-147">Из **участника** выберите в раскрывающемся списке, выберите основную сущность в ссылочном ограничении.</span><span class="sxs-lookup"><span data-stu-id="d307f-147">From the **Principal** drop-down list, select the principal entity in the referential constraint.</span></span>
    <span data-ttu-id="d307f-148">Свойства ключа сущности добавляются к **ключ субъекта-службы** списка в диалоговом окне.</span><span class="sxs-lookup"><span data-stu-id="d307f-148">The entity's key properties are added to the **Principal Key** list in the dialog box.</span></span>
-   <span data-ttu-id="d307f-149">Из **зависимых** выберите в раскрывающемся списке, выберите зависимую сущность в ссылочном ограничении.</span><span class="sxs-lookup"><span data-stu-id="d307f-149">From the **Dependent** drop-down list, select the dependent entity in the referential constraint.</span></span>
-   <span data-ttu-id="d307f-150">Для каждого основного ключа, который имеет зависимый ключ, выберите соответствующий зависимый ключ из раскрывающихся списков в **зависящий от него ключ** столбца.</span><span class="sxs-lookup"><span data-stu-id="d307f-150">For each principal key that has a dependent key, select a corresponding dependent key from the drop-down lists in the **Dependent Key** column.</span></span>

    ![RefConstraint](~/ef6/media/refconstraint.png)

-   <span data-ttu-id="d307f-152">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="d307f-152">Click **OK**.</span></span>

## <a name="create-and-edit-association-mappings"></a><span data-ttu-id="d307f-153">создавать и изменять сопоставления ассоциаций</span><span class="sxs-lookup"><span data-stu-id="d307f-153">Create and Edit Association Mappings</span></span>

<span data-ttu-id="d307f-154">Можно указать сопоставление ассоциации в базу данных в **сведения о сопоставлении** окно конструктора Entity FRAMEWORK.</span><span class="sxs-lookup"><span data-stu-id="d307f-154">You can specify how an association maps to the database in the **Mapping Details** window of the EF Designer.</span></span>

> [!NOTE]
> <span data-ttu-id="d307f-155">Можно сопоставить только сведения для связей, которые не имеют ссылочные ограничения, указанные.</span><span class="sxs-lookup"><span data-stu-id="d307f-155">You can only map details for the associations that do not have a referential constraint specified.</span></span> <span data-ttu-id="d307f-156">Если указан справочного ограничения свойство внешнего ключа входит в сущности, и можно использовать сведения о сопоставлении сущности, к элементу управления, какой столбец сопоставляется внешний ключ.</span><span class="sxs-lookup"><span data-stu-id="d307f-156">If a referential constraint is specified then a foreign key property is included in the entity and you can use the Mapping Details for the entity to control which column the foreign key maps to.</span></span>

### <a name="create-an-association-mapping"></a><span data-ttu-id="d307f-157">Создание сопоставления ассоциаций</span><span class="sxs-lookup"><span data-stu-id="d307f-157">Create an association mapping</span></span>

-   <span data-ttu-id="d307f-158">Щелкните правой кнопкой мыши ассоциацию в области конструктора и выберите **сопоставление таблицы**.</span><span class="sxs-lookup"><span data-stu-id="d307f-158">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="d307f-159">Это появится сопоставление ассоциации в **сведения о сопоставлении** окна.</span><span class="sxs-lookup"><span data-stu-id="d307f-159">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="d307f-160">Нажмите кнопку **добавить таблицу или представление**.</span><span class="sxs-lookup"><span data-stu-id="d307f-160">Click **Add a Table or View**.</span></span>
    <span data-ttu-id="d307f-161">Появится раскрывающийся список, содержащий все таблицы в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="d307f-161">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="d307f-162">Выберите таблицу, с которой будет сопоставлена ассоциация.</span><span class="sxs-lookup"><span data-stu-id="d307f-162">Select the table to which the association will map.</span></span>
    <span data-ttu-id="d307f-163">**Сведения о сопоставлении** окне отображаются оба элемента ассоциации и свойства ключа для типа сущности на каждом **окончания**.</span><span class="sxs-lookup"><span data-stu-id="d307f-163">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each **End**.</span></span>
-   <span data-ttu-id="d307f-164">Для каждого свойства ключа щелкните **столбец** поле и выберите столбец, к которому будет сопоставлено это свойство.</span><span class="sxs-lookup"><span data-stu-id="d307f-164">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

    ![MappingDetails4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a><span data-ttu-id="d307f-166">Изменение сопоставления ассоциаций</span><span class="sxs-lookup"><span data-stu-id="d307f-166">Edit an association mapping</span></span>

-   <span data-ttu-id="d307f-167">Щелкните правой кнопкой мыши ассоциацию в области конструктора и выберите **сопоставление таблицы**.</span><span class="sxs-lookup"><span data-stu-id="d307f-167">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="d307f-168">Это появится сопоставление ассоциации в **сведения о сопоставлении** окна.</span><span class="sxs-lookup"><span data-stu-id="d307f-168">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="d307f-169">Нажмите кнопку **сопоставляется &lt;имя таблицы&gt;**.</span><span class="sxs-lookup"><span data-stu-id="d307f-169">Click **Maps to &lt;Table Name&gt;**.</span></span>
    <span data-ttu-id="d307f-170">Появится раскрывающийся список, содержащий все таблицы в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="d307f-170">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="d307f-171">Выберите таблицу, с которой будет сопоставлена ассоциация.</span><span class="sxs-lookup"><span data-stu-id="d307f-171">Select the table to which the association will map.</span></span>
    <span data-ttu-id="d307f-172">**Сведения о сопоставлении** окне отображаются оба элемента ассоциации и свойства ключа для типа сущности на концах.</span><span class="sxs-lookup"><span data-stu-id="d307f-172">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each End.</span></span>
-   <span data-ttu-id="d307f-173">Для каждого свойства ключа щелкните **столбец** поле и выберите столбец, к которому будет сопоставлено это свойство.</span><span class="sxs-lookup"><span data-stu-id="d307f-173">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

## <a name="edit-and-delete-navigation-properties"></a><span data-ttu-id="d307f-174">Изменение и удаление свойства навигации</span><span class="sxs-lookup"><span data-stu-id="d307f-174">Edit and Delete Navigation Properties</span></span>

<span data-ttu-id="d307f-175">Свойства навигации — это свойства быстрого доступа, используемые для нахождения сущностей в элементах ассоциации в модели.</span><span class="sxs-lookup"><span data-stu-id="d307f-175">Navigation properties are shortcut properties that are used to locate the entities at the ends of an association in a model.</span></span> <span data-ttu-id="d307f-176">Свойства навигации могут быть созданы при создании ассоциации между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="d307f-176">Navigation properties can be created when you create an association between two entity types.</span></span>

#### <a name="to-edit-navigation-properties"></a><span data-ttu-id="d307f-177">Чтобы изменить свойства навигации</span><span class="sxs-lookup"><span data-stu-id="d307f-177">To edit navigation properties</span></span>

-   <span data-ttu-id="d307f-178">Выберите свойство навигации на поверхности конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="d307f-178">Select a navigation property on the EF Designer surface.</span></span>
    <span data-ttu-id="d307f-179">Сведения о свойстве навигации отображаются в Visual Studio **свойства** окна.</span><span class="sxs-lookup"><span data-stu-id="d307f-179">Information about the navigation property is displayed in the Visual Studio **Properties** window.</span></span>
-   <span data-ttu-id="d307f-180">Измените параметры свойств в **свойства** окна.</span><span class="sxs-lookup"><span data-stu-id="d307f-180">Change the property settings in the **Properties** window.</span></span>

#### <a name="to-delete-navigation-properties"></a><span data-ttu-id="d307f-181">Удалить свойства навигации</span><span class="sxs-lookup"><span data-stu-id="d307f-181">To delete navigation properties</span></span>

-   <span data-ttu-id="d307f-182">Если внешние ключи не представлены в типах сущностей в концептуальной модели, то удаление свойства навигации может обеспечить перемещение соответствующей ассоциации только в одном направлении или сделать ее неперемещаемой.</span><span class="sxs-lookup"><span data-stu-id="d307f-182">If foreign keys are not exposed on entity types in the conceptual model, deleting a navigation property may make the corresponding association traversable in only one direction or not traversable at all.</span></span>
-   <span data-ttu-id="d307f-183">Щелкните правой кнопкой мыши свойство навигации в конструкторе EF и выберите пункт **удалить**.</span><span class="sxs-lookup"><span data-stu-id="d307f-183">Right-click a navigation property on the EF Designer surface and select **Delete**.</span></span>
