---
title: Сложные типы - конструктор EF - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
ms.openlocfilehash: a3c5578acee23688c04772d2dd0a2221779af562
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489912"
---
# <a name="complex-types---ef-designer"></a><span data-ttu-id="01a48-102">Сложные типы - конструктор EF</span><span class="sxs-lookup"><span data-stu-id="01a48-102">Complex Types - EF Designer</span></span>
<span data-ttu-id="01a48-103">В этом разделе показано, как сопоставить сложных типов с помощью Entity Framework Designer (конструктор EF) и как выполнять запросы для сущностей, которые содержат свойства сложного типа.</span><span class="sxs-lookup"><span data-stu-id="01a48-103">This topic shows how to map complex types with the Entity Framework Designer (EF Designer) and how to query for entities that contain properties of complex type.</span></span>

<span data-ttu-id="01a48-104">На следующем рисунке показана основные окна, которые используются при работе с конструктором EF.</span><span class="sxs-lookup"><span data-stu-id="01a48-104">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![Конструктор EF](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="01a48-106">При создании концептуальной модели, предупреждения о несопоставленных сущностях и ассоциациях, появляются в списке ошибок.</span><span class="sxs-lookup"><span data-stu-id="01a48-106">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="01a48-107">Эти предупреждения можно игнорировать, поскольку после нажатия кнопки для создания базы данных из модели, пропадут.</span><span class="sxs-lookup"><span data-stu-id="01a48-107">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="what-is-a-complex-type"></a><span data-ttu-id="01a48-108">Что такое сложный тип</span><span class="sxs-lookup"><span data-stu-id="01a48-108">What is a Complex Type</span></span>

<span data-ttu-id="01a48-109">Сложные типы — это нескалярные свойства типов сущности, которые позволяют организовать в сущностях скалярные свойства.</span><span class="sxs-lookup"><span data-stu-id="01a48-109">Complex types are non-scalar properties of entity types that enable scalar properties to be organized within entities.</span></span> <span data-ttu-id="01a48-110">Подобно сущностям, сложные типы состоят из скалярных свойств или свойств других сложных типов.</span><span class="sxs-lookup"><span data-stu-id="01a48-110">Like entities, complex types consist of scalar properties or other complex type properties.</span></span>

<span data-ttu-id="01a48-111">При работе с объектами, представляющими сложные типы, следует учитывать следующее:</span><span class="sxs-lookup"><span data-stu-id="01a48-111">When you work with objects that represent complex types, be aware of the following:</span></span>

-   <span data-ttu-id="01a48-112">Сложные типы не имеют ключей и поэтому не могут существовать независимо друг от друга.</span><span class="sxs-lookup"><span data-stu-id="01a48-112">Complex types do not have keys and therefore cannot exist independently.</span></span> <span data-ttu-id="01a48-113">Сложные типы могут существовать только как свойства типов сущностей или других сложных типов.</span><span class="sxs-lookup"><span data-stu-id="01a48-113">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="01a48-114">Сложные типы не могут участвовать в ассоциации и не может содержать свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="01a48-114">Complex types cannot participate in associations and cannot contain navigation properties.</span></span>
-   <span data-ttu-id="01a48-115">Свойства сложного типа не может быть **null**.</span><span class="sxs-lookup"><span data-stu-id="01a48-115">Complex type properties cannot be **null**.</span></span> <span data-ttu-id="01a48-116">\*\* InvalidOperationException \*\* возникает, когда **DbContext.SaveChanges** вызывается и обнаруживается неопределенный сложный объект.</span><span class="sxs-lookup"><span data-stu-id="01a48-116">An \*\*InvalidOperationException \*\*occurs when **DbContext.SaveChanges** is called and a null complex object is encountered.</span></span> <span data-ttu-id="01a48-117">Скалярные свойства сложных объектов могут принимать **null**.</span><span class="sxs-lookup"><span data-stu-id="01a48-117">Scalar properties of complex objects can be **null**.</span></span>
-   <span data-ttu-id="01a48-118">Сложные типы не могут наследоваться от других сложных типов.</span><span class="sxs-lookup"><span data-stu-id="01a48-118">Complex types cannot inherit from other complex types.</span></span>
-   <span data-ttu-id="01a48-119">Необходимо определить сложный тип как **класс**.</span><span class="sxs-lookup"><span data-stu-id="01a48-119">You must define the complex type as a **class**.</span></span> 
-   <span data-ttu-id="01a48-120">EF определяет изменения в члены на объект сложного типа при **DbContext.DetectChanges** вызывается.</span><span class="sxs-lookup"><span data-stu-id="01a48-120">EF detects changes to members on a complex type object when **DbContext.DetectChanges** is called.</span></span> <span data-ttu-id="01a48-121">Платформа Entity Framework вызывает **DetectChanges** автоматически, когда вызываются следующие члены: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.</span><span class="sxs-lookup"><span data-stu-id="01a48-121">Entity Framework calls **DetectChanges** automatically when the following members are called: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.</span></span>

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a><span data-ttu-id="01a48-122">Оптимизация свойств сущности в новый сложный тип</span><span class="sxs-lookup"><span data-stu-id="01a48-122">Refactor an Entity’s Properties into New Complex Type</span></span>

<span data-ttu-id="01a48-123">При наличии сущности в концептуальной модели может потребоваться реструктурировать некоторые свойства в свойство сложного типа.</span><span class="sxs-lookup"><span data-stu-id="01a48-123">If you already have an entity in your conceptual model you may want to refactor some of the properties into a complex type property.</span></span>

<span data-ttu-id="01a48-124">В рабочей области конструктора, выберите один или несколько свойств (кроме свойств навигации) сущность, затем щелкните правой кнопкой мыши и выберите **рефакторинг -&gt; переместить в новый сложный тип**.</span><span class="sxs-lookup"><span data-stu-id="01a48-124">On the designer surface, select one or more properties (excluding navigation properties) of an entity, then right-click and select **Refactor -&gt; Move to New Complex Type**.</span></span>

![Рефакторинг](~/ef6/media/refactor.png)

<span data-ttu-id="01a48-126">Добавляется новый сложный тип с выбранными свойствами **браузер моделей**.</span><span class="sxs-lookup"><span data-stu-id="01a48-126">A new complex type with the selected properties is added to the **Model Browser**.</span></span> <span data-ttu-id="01a48-127">Сложному типу присваивается имя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="01a48-127">The complex type is given a default name.</span></span>

<span data-ttu-id="01a48-128">Сложное свойство только что созданного типа заменит выбранные свойства.</span><span class="sxs-lookup"><span data-stu-id="01a48-128">A complex property of the newly created type replaces the selected properties.</span></span> <span data-ttu-id="01a48-129">Все сопоставления свойств будут сохранены.</span><span class="sxs-lookup"><span data-stu-id="01a48-129">All property mappings are preserved.</span></span>

![Рефакторинг 2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a><span data-ttu-id="01a48-131">Создать новый сложный тип</span><span class="sxs-lookup"><span data-stu-id="01a48-131">Create a New Complex Type</span></span>

<span data-ttu-id="01a48-132">Можно также создать новый сложный тип, который не содержит свойства существующей сущности.</span><span class="sxs-lookup"><span data-stu-id="01a48-132">You can also create a new complex type that does not contain properties of an existing entity.</span></span>

<span data-ttu-id="01a48-133">Щелкните правой кнопкой мыши **сложные типы** папки в браузере моделей, пункты **AddNew сложный тип...** .</span><span class="sxs-lookup"><span data-stu-id="01a48-133">Right-click the **Complex Types** folder in the Model Browser, point to **AddNew Complex Type…**.</span></span> <span data-ttu-id="01a48-134">Кроме того, можно выбрать **сложные типы** папку и нажмите клавишу **вставить** на клавиатуре.</span><span class="sxs-lookup"><span data-stu-id="01a48-134">Alternatively, you can select the **Complex Types** folder and press the **Insert** key on your keyboard.</span></span>

![Добавление нового сложного типа](~/ef6/media/addnewcomplextype.png)

<span data-ttu-id="01a48-136">Новый сложный тип будет добавлен в папку с именем по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="01a48-136">A new complex type is added to the folder with a default name.</span></span> <span data-ttu-id="01a48-137">Теперь можно добавить свойства к типу.</span><span class="sxs-lookup"><span data-stu-id="01a48-137">You can now add properties to the type.</span></span>

## <a name="add-properties-to-a-complex-type"></a><span data-ttu-id="01a48-138">Добавлять свойства к сложному типу</span><span class="sxs-lookup"><span data-stu-id="01a48-138">Add Properties to a Complex Type</span></span>

<span data-ttu-id="01a48-139">Свойства сложного типа могут иметь как скалярные, так и существующие сложные типы.</span><span class="sxs-lookup"><span data-stu-id="01a48-139">Properties of a complex type can be scalar types or existing complex types.</span></span> <span data-ttu-id="01a48-140">Однако свойства сложного типа не могут иметь циклических ссылок.</span><span class="sxs-lookup"><span data-stu-id="01a48-140">However, complex type properties cannot have circular references.</span></span> <span data-ttu-id="01a48-141">Например, сложный тип **OnsiteCourseDetails** свойство сложного типа не может быть **OnsiteCourseDetails**.</span><span class="sxs-lookup"><span data-stu-id="01a48-141">For example, a complex type **OnsiteCourseDetails** cannot have a property of complex type **OnsiteCourseDetails**.</span></span>

<span data-ttu-id="01a48-142">Добавить свойство к сложному типу можно любым из следующих способов.</span><span class="sxs-lookup"><span data-stu-id="01a48-142">You can add a property to a complex type in any of the ways listed below.</span></span>

-   <span data-ttu-id="01a48-143">Щелкните правой кнопкой мыши сложный тип в браузере моделей, выберите пункт **добавить**, наведите указатель на **скалярное свойство** или **сложное свойство**, затем выберите необходимый тип свойства.</span><span class="sxs-lookup"><span data-stu-id="01a48-143">Right-click a complex type in the Model Browser, point to **Add**, then point to **Scalar Property** or **Complex Property**, then select the desired property type.</span></span> <span data-ttu-id="01a48-144">Кроме того, можно выбрать сложный тип и нажмите клавишу **вставить** на клавиатуре.</span><span class="sxs-lookup"><span data-stu-id="01a48-144">Alternatively, you can select a complex type and then press the **Insert** key on your keyboard.</span></span>  

    ![Добавить свойства сложного типа](~/ef6/media/addpropertiestocomplextype.png)

    <span data-ttu-id="01a48-146">Новое свойство будет добавлено к сложному типу с именем по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="01a48-146">A new property is added to the complex type with a default name.</span></span>

- <span data-ttu-id="01a48-147">OR-</span><span class="sxs-lookup"><span data-stu-id="01a48-147">OR -</span></span>

-   <span data-ttu-id="01a48-148">Щелкните правой кнопкой мыши свойство сущности **конструктор EF** и выберите пункт **копирования**, щелкните правой кнопкой мыши сложный тип в **браузер моделей** и выберите  **Вставить**.</span><span class="sxs-lookup"><span data-stu-id="01a48-148">Right-click an entity property on the **EF  Designer** surface and select **Copy**, then right-click the complex type in the **Model Browser** and select **Paste**.</span></span>

## <a name="rename-a-complex-type"></a><span data-ttu-id="01a48-149">Переименование сложного типа</span><span class="sxs-lookup"><span data-stu-id="01a48-149">Rename a Complex Type</span></span>

<span data-ttu-id="01a48-150">При переименовании сложного типа все ссылки на тип обновляются по всему проекту.</span><span class="sxs-lookup"><span data-stu-id="01a48-150">When you rename a complex type, all references to the type are updated throughout the project.</span></span>

-   <span data-ttu-id="01a48-151">Дважды щелкните сложный тип в **браузер моделей**.</span><span class="sxs-lookup"><span data-stu-id="01a48-151">Slowly double-click a complex type in the **Model Browser**.</span></span>
    <span data-ttu-id="01a48-152">Имя будет выбрано в режиме редактирования.</span><span class="sxs-lookup"><span data-stu-id="01a48-152">The name will be selected and in edit mode.</span></span>

- <span data-ttu-id="01a48-153">OR-</span><span class="sxs-lookup"><span data-stu-id="01a48-153">OR -</span></span>

-   <span data-ttu-id="01a48-154">Щелкните правой кнопкой мыши сложный тип в **браузер моделей** и выберите **Переименовать**.</span><span class="sxs-lookup"><span data-stu-id="01a48-154">Right-click a complex type in the **Model Browser** and select **Rename**.</span></span>

- <span data-ttu-id="01a48-155">OR-</span><span class="sxs-lookup"><span data-stu-id="01a48-155">OR -</span></span>

-   <span data-ttu-id="01a48-156">Выберите сложный тип в браузере моделей и нажмите клавишу F2.</span><span class="sxs-lookup"><span data-stu-id="01a48-156">Select a complex type in the Model Browser and press the F2 key.</span></span>

- <span data-ttu-id="01a48-157">OR-</span><span class="sxs-lookup"><span data-stu-id="01a48-157">OR -</span></span>

-   <span data-ttu-id="01a48-158">Щелкните правой кнопкой мыши сложный тип в **браузер моделей** и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="01a48-158">Right-click a complex type in the **Model Browser** and select **Properties**.</span></span> <span data-ttu-id="01a48-159">Измените имя в **свойства** окна.</span><span class="sxs-lookup"><span data-stu-id="01a48-159">Edit the name in the **Properties** window.</span></span>

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a><span data-ttu-id="01a48-160">Добавление существующего сложного типа к сущности и сопоставить ее свойства со столбцами таблицы</span><span class="sxs-lookup"><span data-stu-id="01a48-160">Add an Existing Complex Type to an Entity and Map its Properties to Table Columns</span></span>

1.  <span data-ttu-id="01a48-161">Щелкните правой кнопкой мыши сущность, выберите пункт **Add New**и выберите **сложное свойство**.</span><span class="sxs-lookup"><span data-stu-id="01a48-161">Right-click an entity, point to **Add New**, and select **Complex Property**.</span></span>
    <span data-ttu-id="01a48-162">Свойство сложного типа с именем по умолчанию будет добавлено к сущности.</span><span class="sxs-lookup"><span data-stu-id="01a48-162">A complex type property with a default name is added to the entity.</span></span> <span data-ttu-id="01a48-163">Свойству назначается тип по умолчанию (выбранный из имеющихся сложных типов).</span><span class="sxs-lookup"><span data-stu-id="01a48-163">A default type (chosen from the existing complex types) is assigned to the property.</span></span>
2.  <span data-ttu-id="01a48-164">Назначьте свойству в требуемый тип **свойства** окна.</span><span class="sxs-lookup"><span data-stu-id="01a48-164">Assign the desired type to the property in the **Properties** window.</span></span>
    <span data-ttu-id="01a48-165">После добавления свойства сложного типа к сущности необходимо сопоставить ее свойства со столбцами таблицы.</span><span class="sxs-lookup"><span data-stu-id="01a48-165">After adding a complex type property to an entity, you must map its properties to table columns.</span></span>
3.  <span data-ttu-id="01a48-166">Щелкните правой кнопкой мыши тип сущности в области конструктора или в **браузер моделей** и выберите **сопоставления таблиц**.</span><span class="sxs-lookup"><span data-stu-id="01a48-166">Right-click an entity type on the design surface or in the **Model Browser** and select **Table Mappings**.</span></span>
    <span data-ttu-id="01a48-167">Сопоставления таблицы отображаются в **сведения о сопоставлении** окна.</span><span class="sxs-lookup"><span data-stu-id="01a48-167">The table mappings are displayed in the **Mapping Details** window.</span></span>
4.  <span data-ttu-id="01a48-168">Разверните **сопоставляется &lt;имя таблицы&gt;**  узла.</span><span class="sxs-lookup"><span data-stu-id="01a48-168">Expand the **Maps to &lt;Table Name&gt;** node.</span></span>
    <span data-ttu-id="01a48-169">Объект **сопоставления столбцов** появится узел.</span><span class="sxs-lookup"><span data-stu-id="01a48-169">A **Column Mappings** node appears.</span></span>
5.  <span data-ttu-id="01a48-170">Разверните **сопоставления столбцов** узла.</span><span class="sxs-lookup"><span data-stu-id="01a48-170">Expand the **Column Mappings** node.</span></span>
    <span data-ttu-id="01a48-171">Появится список всех столбцов таблицы.</span><span class="sxs-lookup"><span data-stu-id="01a48-171">A list of all the columns in the table appears.</span></span> <span data-ttu-id="01a48-172">Свойства по умолчанию (если таковые имеются) с которыми сопоставляются столбцы отображаются в категории **значение/свойство** заголовок.</span><span class="sxs-lookup"><span data-stu-id="01a48-172">The default properties (if any) to which the columns map are listed under the **Value/Property** heading.</span></span>
6.  <span data-ttu-id="01a48-173">Выберите столбец, необходимо сопоставить и щелкните правой кнопкой мыши соответствующий **значение/свойство** поля.</span><span class="sxs-lookup"><span data-stu-id="01a48-173">Select the column you want to map, and then right-click the corresponding **Value/Property** field.</span></span>
    <span data-ttu-id="01a48-174">Отобразится раскрывающийся список всех скалярных свойств.</span><span class="sxs-lookup"><span data-stu-id="01a48-174">A drop-down list of all the scalar properties is displayed.</span></span>
7.  <span data-ttu-id="01a48-175">Выберите соответствующее свойство.</span><span class="sxs-lookup"><span data-stu-id="01a48-175">Select the appropriate property.</span></span>

    ![Сложный тип карты](~/ef6/media/mapcomplextype.png)

8.  <span data-ttu-id="01a48-177">Повторите шаги 6 и 7 для каждого столбца таблицы.</span><span class="sxs-lookup"><span data-stu-id="01a48-177">Repeat steps 6 and 7 for each table column.</span></span>

>[!NOTE]
> <span data-ttu-id="01a48-178">Чтобы удалить сопоставление столбцов, выберите столбец, который необходимо сопоставить, а затем нажмите кнопку **значение/свойство** поля.</span><span class="sxs-lookup"><span data-stu-id="01a48-178">To delete a column mapping, select the column that you want to map, and then click the **Value/Property** field.</span></span> <span data-ttu-id="01a48-179">Выберите **удалить** из раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="01a48-179">Then, select **Delete** from the drop-down list.</span></span>

## <a name="map-a-function-import-to-a-complex-type"></a><span data-ttu-id="01a48-180">Сопоставить импорт функции со сложным типом</span><span class="sxs-lookup"><span data-stu-id="01a48-180">Map a Function Import to a Complex Type</span></span>

<span data-ttu-id="01a48-181">Импорт функций основан на хранимых процедурах.</span><span class="sxs-lookup"><span data-stu-id="01a48-181">Function imports are based on stored procedures.</span></span> <span data-ttu-id="01a48-182">Чтобы сопоставить импорт со сложным типом, столбцы, возвращаемые соответствующей хранимой процедурой, должны соответствовать свойствам сложного типа по числу и должны иметь типы хранения, совместимые с типами свойств.</span><span class="sxs-lookup"><span data-stu-id="01a48-182">To map a function import to a complex type, the columns returned by the corresponding stored procedure must match the properties of the complex type in number and must have storage types that are compatible with the property types.</span></span>

-   <span data-ttu-id="01a48-183">Дважды щелкните импортированной функции, которую необходимо сопоставить со сложным типом.</span><span class="sxs-lookup"><span data-stu-id="01a48-183">Double-click on an imported function that you want to map to a complex type.</span></span>

    ![Импорт функций](~/ef6/media/functionimports.png)

-   <span data-ttu-id="01a48-185">Задайте параметры для нового импорта функции следующим образом.</span><span class="sxs-lookup"><span data-stu-id="01a48-185">Fill in the settings for the new function import, as follows:</span></span>
    -   <span data-ttu-id="01a48-186">Укажите хранимую процедуру, для которой создается импорт функции в **имя хранимой процедуры** поля.</span><span class="sxs-lookup"><span data-stu-id="01a48-186">Specify the stored procedure for which you are creating a function import in the **Stored Procedure Name** field.</span></span> <span data-ttu-id="01a48-187">Это поле представляет собой раскрывающийся список, содержащий все хранимые процедуры, которые имеются в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="01a48-187">This field is a drop-down list that displays all the stored procedures in the storage model.</span></span>
    -   <span data-ttu-id="01a48-188">Укажите имя импорта функции в **имя импорта функции** поля.</span><span class="sxs-lookup"><span data-stu-id="01a48-188">Specify the name of the function import in the **Function Import Name** field.</span></span>
    -   <span data-ttu-id="01a48-189">Выберите **сложных** как возвращаемый тип, а затем укажите конкретный сложный возвращаемый тип, выбрав соответствующий тип из раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="01a48-189">Select **Complex** as the return type and then specify the specific complex return type by choosing the appropriate type from the drop-down list.</span></span>

        ![Изменение импорта функции](~/ef6/media/editfunctionimport.png)

-   <span data-ttu-id="01a48-191">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="01a48-191">Click **OK**.</span></span>
    <span data-ttu-id="01a48-192">В концептуальной модели создается запись импорта функции.</span><span class="sxs-lookup"><span data-stu-id="01a48-192">The function import entry is created in the conceptual model.</span></span>

### <a name="customize-column-mapping-for-function-import"></a><span data-ttu-id="01a48-193">Настроить сопоставление для импорта функции столбцов</span><span class="sxs-lookup"><span data-stu-id="01a48-193">Customize Column Mapping for Function Import</span></span>

-   <span data-ttu-id="01a48-194">Щелкните правой кнопкой мыши импорт функции в браузере моделей и выберите **сопоставление импорта функций**.</span><span class="sxs-lookup"><span data-stu-id="01a48-194">Right-click the function import in the Model Browser and select **Function Import Mapping**.</span></span>
    <span data-ttu-id="01a48-195">**Сведения о сопоставлении** окно отобразится сопоставление по умолчанию для импорта функции.</span><span class="sxs-lookup"><span data-stu-id="01a48-195">The **Mapping Details** window appears and shows the default mapping for the function import.</span></span> <span data-ttu-id="01a48-196">Стрелки указывают сопоставления между значениями столбцов и значениями свойств.</span><span class="sxs-lookup"><span data-stu-id="01a48-196">Arrows indicate the mappings between column values and property values.</span></span> <span data-ttu-id="01a48-197">По умолчанию предполагается, что имена столбцов совпадают с именами свойств сложного типа.</span><span class="sxs-lookup"><span data-stu-id="01a48-197">By default, the column names are assumed to be the same as the complex type's property names.</span></span> <span data-ttu-id="01a48-198">Имена столбцов по умолчанию отображаются серым текстом.</span><span class="sxs-lookup"><span data-stu-id="01a48-198">The default column names appear in gray text.</span></span>
-   <span data-ttu-id="01a48-199">Если необходимо, измените имена столбцов таким образом, чтобы они совпадали с именами столбцов, возвращаемых хранимой процедурой, соответствующей импорту функции.</span><span class="sxs-lookup"><span data-stu-id="01a48-199">If necessary, change the column names to match the column names that are returned by the stored procedure that corresponds to the function import.</span></span>

## <a name="delete-a-complex-type"></a><span data-ttu-id="01a48-200">Удаление сложного типа</span><span class="sxs-lookup"><span data-stu-id="01a48-200">Delete a Complex Type</span></span>

<span data-ttu-id="01a48-201">При удалении сложного типа он удаляется из концептуальной модели. Кроме того, удаляются сопоставления для всех экземпляров типа.</span><span class="sxs-lookup"><span data-stu-id="01a48-201">When you delete a complex type, the type is deleted from the conceptual model and mappings for all instances of the type are deleted.</span></span> <span data-ttu-id="01a48-202">Однако ссылки на тип не обновляются.</span><span class="sxs-lookup"><span data-stu-id="01a48-202">However, references to the type are not updated.</span></span> <span data-ttu-id="01a48-203">Например, если сущность имеет свойство сложного типа типа ComplexType1, и ComplexType1 был удален в **браузер моделей**, то соответствующее свойство сущности не обновляется.</span><span class="sxs-lookup"><span data-stu-id="01a48-203">For example, if an entity has a complex type property of type ComplexType1 and ComplexType1 is deleted in the **Model Browser**, the corresponding entity property is not updated.</span></span> <span data-ttu-id="01a48-204">Модель не пройдет проверку, так как он содержит сущность, которая ссылается на удаленный сложный тип.</span><span class="sxs-lookup"><span data-stu-id="01a48-204">The model will not validate  because it contains an entity that references a deleted complex type.</span></span> <span data-ttu-id="01a48-205">Обновить или удалить ссылки на удаленные сложные типы можно с помощью конструктора сущностей.</span><span class="sxs-lookup"><span data-stu-id="01a48-205">You can update or delete references to deleted complex types by using the Entity Designer.</span></span>

-   <span data-ttu-id="01a48-206">Щелкните правой кнопкой мыши сложный тип в браузере моделей и выберите **удалить**.</span><span class="sxs-lookup"><span data-stu-id="01a48-206">Right-click a complex type in the Model Browser and select **Delete**.</span></span>

- <span data-ttu-id="01a48-207">OR-</span><span class="sxs-lookup"><span data-stu-id="01a48-207">OR -</span></span>

-   <span data-ttu-id="01a48-208">В браузере моделей выберите сложный тип и нажмите на клавиатуре клавишу DELETE.</span><span class="sxs-lookup"><span data-stu-id="01a48-208">Select a complex type in the Model Browser and press the Delete key on your keyboard.</span></span>

## <a name="query-for-entities-containing-properties-of-complex-type"></a><span data-ttu-id="01a48-209">Запрос для сущностей, содержащий свойства сложного типа</span><span class="sxs-lookup"><span data-stu-id="01a48-209">Query for Entities Containing Properties of Complex Type</span></span>

<span data-ttu-id="01a48-210">Ниже показано, как выполнить запрос, возвращающий коллекцию сущностей типа объектов, содержащих свойства сложного типа.</span><span class="sxs-lookup"><span data-stu-id="01a48-210">The following code shows how to execute a query that returns a collection of entity type objects that contain a complex type property.</span></span>

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        var courses =
            from c in context.OnsiteCourses
            order by c.Details.Time
            select c;

        foreach (var c in courses)
        {
            Console.WriteLine("Time: " + c.Details.Time);
            Console.WriteLine("Days: " + c.Details.Days);
            Console.WriteLine("Location: " + c.Details.Location);
        }
    }
```
