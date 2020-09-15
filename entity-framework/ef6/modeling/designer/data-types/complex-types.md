---
title: Сложные типы — конструктор EF — EF6
description: Сложные типы — конструктор EF в Entity Framework 6
author: divega
ms.date: 10/23/2016
uid: ef6/modeling/designer/data-types/complex-types
ms.openlocfilehash: 6ebba9ea36ddad1a25b21bf98a6ac3090c327144
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90073517"
---
# <a name="complex-types---ef-designer"></a><span data-ttu-id="0e9cb-103">Сложные типы — конструктор EF</span><span class="sxs-lookup"><span data-stu-id="0e9cb-103">Complex Types - EF Designer</span></span>
<span data-ttu-id="0e9cb-104">В этом разделе показано, как сопоставлять сложные типы с Entity Framework Designer (конструктор EF) и как выполнять запросы сущностей, содержащих свойства сложного типа.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-104">This topic shows how to map complex types with the Entity Framework Designer (EF Designer) and how to query for entities that contain properties of complex type.</span></span>

<span data-ttu-id="0e9cb-105">На следующем рисунке показаны основные окна, используемые при работе с конструктором EF.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-105">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![Конструктор EF](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="0e9cb-107">При построении концептуальной модели в окне Список ошибок могут появиться предупреждения о несопоставленных сущностях и ассоциациях.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-107">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="0e9cb-108">Эти предупреждения можно игнорировать, так как после выбора создания базы данных из модели ошибки будут исчезнуть.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-108">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="what-is-a-complex-type"></a><span data-ttu-id="0e9cb-109">Что такое сложный тип</span><span class="sxs-lookup"><span data-stu-id="0e9cb-109">What is a Complex Type</span></span>

<span data-ttu-id="0e9cb-110">Сложные типы — это нескалярные свойства типов сущности, которые позволяют организовать в сущностях скалярные свойства.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-110">Complex types are non-scalar properties of entity types that enable scalar properties to be organized within entities.</span></span> <span data-ttu-id="0e9cb-111">Подобно сущностям, сложные типы состоят из скалярных свойств или свойств других сложных типов.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-111">Like entities, complex types consist of scalar properties or other complex type properties.</span></span>

<span data-ttu-id="0e9cb-112">При работе с объектами, представляющими сложные типы, учитывайте следующее:</span><span class="sxs-lookup"><span data-stu-id="0e9cb-112">When you work with objects that represent complex types, be aware of the following:</span></span>

-   <span data-ttu-id="0e9cb-113">Сложные типы не имеют ключей и поэтому не могут существовать независимо друг от друга.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-113">Complex types do not have keys and therefore cannot exist independently.</span></span> <span data-ttu-id="0e9cb-114">Сложные типы могут существовать только как свойства типов сущностей или других сложных типов.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-114">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="0e9cb-115">Сложные типы не могут участвовать в связях и не могут содержать свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-115">Complex types cannot participate in associations and cannot contain navigation properties.</span></span>
-   <span data-ttu-id="0e9cb-116">Свойства сложного типа не могут иметь **значение NULL**.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-116">Complex type properties cannot be **null**.</span></span> <span data-ttu-id="0e9cb-117">Исключение **InvalidOperationException **возникает при вызове **DbContext. SaveChanges**   и обнаружении сложного объекта со значением NULL.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-117">An **InvalidOperationException **occurs when **DbContext.SaveChanges** is called and a null complex object is encountered.</span></span> <span data-ttu-id="0e9cb-118">Скалярные свойства сложных объектов могут иметь **значение NULL**.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-118">Scalar properties of complex objects can be **null**.</span></span>
-   <span data-ttu-id="0e9cb-119">Сложные типы не могут наследоваться от других сложных типов.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-119">Complex types cannot inherit from other complex types.</span></span>
-   <span data-ttu-id="0e9cb-120">Сложный тип необходимо определить как **класс**.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-120">You must define the complex type as a **class**.</span></span> 
-   <span data-ttu-id="0e9cb-121">EF обнаруживает изменения элементов в объекте сложного типа при вызове **DbContext. DetectChanges** .</span><span class="sxs-lookup"><span data-stu-id="0e9cb-121">EF detects changes to members on a complex type object when **DbContext.DetectChanges** is called.</span></span> <span data-ttu-id="0e9cb-122">Entity Framework автоматически вызывает **DetectChanges** при вызове следующих членов: **DbSet. Find**, **DbSet. local**, **DbSet. Remove**, **DbSet. Add**, **DbSet. Attach**, **DbContext. SaveChanges**, **DbContext. жетвалидатионеррорс**, **DbContext. Entry**, **дбчанжетраккер.** ALL.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-122">Entity Framework calls **DetectChanges** automatically when the following members are called: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.</span></span>

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a><span data-ttu-id="0e9cb-123">Рефакторинг свойств сущности в новый сложный тип</span><span class="sxs-lookup"><span data-stu-id="0e9cb-123">Refactor an Entity’s Properties into New Complex Type</span></span>

<span data-ttu-id="0e9cb-124">Если у вас уже есть сущность в концептуальной модели, может потребоваться рефакторинг некоторых свойств в свойство сложного типа.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-124">If you already have an entity in your conceptual model you may want to refactor some of the properties into a complex type property.</span></span>

<span data-ttu-id="0e9cb-125">В области конструктора выберите одно или несколько свойств (за исключением свойств навигации) сущности, щелкните правой кнопкой мыши и выберите **Рефакторинг — &gt; переместить в новый сложный тип**.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-125">On the designer surface, select one or more properties (excluding navigation properties) of an entity, then right-click and select **Refactor -&gt; Move to New Complex Type**.</span></span>

![Рефакторинг для создания сложного типа](~/ef6/media/refactor.png)

<span data-ttu-id="0e9cb-127">Новый сложный тип с выбранными свойствами добавляется в **Обозреватель моделей**.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-127">A new complex type with the selected properties is added to the **Model Browser**.</span></span> <span data-ttu-id="0e9cb-128">Сложному типу присваивается имя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-128">The complex type is given a default name.</span></span>

<span data-ttu-id="0e9cb-129">Сложное свойство только что созданного типа заменит выбранные свойства.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-129">A complex property of the newly created type replaces the selected properties.</span></span> <span data-ttu-id="0e9cb-130">Все сопоставления свойств будут сохранены.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-130">All property mappings are preserved.</span></span>

![Рефакторинг для нового сложного типа 2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a><span data-ttu-id="0e9cb-132">Создание нового сложного типа</span><span class="sxs-lookup"><span data-stu-id="0e9cb-132">Create a New Complex Type</span></span>

<span data-ttu-id="0e9cb-133">Можно также создать новый сложный тип, который не содержит свойств существующей сущности.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-133">You can also create a new complex type that does not contain properties of an existing entity.</span></span>

<span data-ttu-id="0e9cb-134">Щелкните правой кнопкой мыши папку **Составные типы** в обозревателе моделей и выберите команду **AddNew сложный тип..**..</span><span class="sxs-lookup"><span data-stu-id="0e9cb-134">Right-click the **Complex Types** folder in the Model Browser, point to **AddNew Complex Type…**.</span></span> <span data-ttu-id="0e9cb-135">Кроме того, можно выбрать папку **сложных типов** и нажать клавишу **INSERT**   на клавиатуре.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-135">Alternatively, you can select the **Complex Types** folder and press the **Insert** key on your keyboard.</span></span>

![Добавить новый сложный тип](~/ef6/media/addnewcomplextype.png)

<span data-ttu-id="0e9cb-137">Новый сложный тип будет добавлен в папку с именем по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-137">A new complex type is added to the folder with a default name.</span></span> <span data-ttu-id="0e9cb-138">Теперь в тип можно добавлять свойства.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-138">You can now add properties to the type.</span></span>

## <a name="add-properties-to-a-complex-type"></a><span data-ttu-id="0e9cb-139">Добавление свойств в сложный тип</span><span class="sxs-lookup"><span data-stu-id="0e9cb-139">Add Properties to a Complex Type</span></span>

<span data-ttu-id="0e9cb-140">Свойства сложного типа могут иметь как скалярные, так и существующие сложные типы.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-140">Properties of a complex type can be scalar types or existing complex types.</span></span> <span data-ttu-id="0e9cb-141">Однако свойства сложного типа не могут иметь циклических ссылок.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-141">However, complex type properties cannot have circular references.</span></span> <span data-ttu-id="0e9cb-142">Например, сложный тип **онситекаурседетаилс**   не может иметь свойство сложного типа **онситекаурседетаилс**.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-142">For example, a complex type **OnsiteCourseDetails** cannot have a property of complex type **OnsiteCourseDetails**.</span></span>

<span data-ttu-id="0e9cb-143">Добавить свойство к сложному типу можно любым из следующих способов.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-143">You can add a property to a complex type in any of the ways listed below.</span></span>

-   <span data-ttu-id="0e9cb-144">В обозревателе моделей щелкните правой кнопкой мыши сложный тип, наведите указатель на пункт **Добавить**, укажите **скалярное свойство**   или **сложное свойство**, а затем выберите нужный тип свойства.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-144">Right-click a complex type in the Model Browser, point to **Add**, then point to **Scalar Property** or **Complex Property**, then select the desired property type.</span></span> <span data-ttu-id="0e9cb-145">Кроме того, можно выбрать сложный тип и нажать клавишу **INSERT**   на клавиатуре.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-145">Alternatively, you can select a complex type and then press the **Insert** key on your keyboard.</span></span>  

    ![Добавить свойства в сложный тип](~/ef6/media/addpropertiestocomplextype.png)

    <span data-ttu-id="0e9cb-147">Новое свойство будет добавлено к сложному типу с именем по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-147">A new property is added to the complex type with a default name.</span></span>

- <span data-ttu-id="0e9cb-148">OR -</span><span class="sxs-lookup"><span data-stu-id="0e9cb-148">OR -</span></span>

-   <span data-ttu-id="0e9cb-149">Щелкните правой кнопкой мыши свойство сущности в области **конструктора EF** и выберите **Копировать**, затем щелкните правой кнопкой мыши сложный тип в **обозревателе моделей** и выберите команду **Вставить**.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-149">Right-click an entity property on the **EF  Designer** surface and select **Copy**, then right-click the complex type in the **Model Browser** and select **Paste**.</span></span>

## <a name="rename-a-complex-type"></a><span data-ttu-id="0e9cb-150">Переименование сложного типа</span><span class="sxs-lookup"><span data-stu-id="0e9cb-150">Rename a Complex Type</span></span>

<span data-ttu-id="0e9cb-151">При переименовании сложного типа все ссылки на тип обновляются по всему проекту.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-151">When you rename a complex type, all references to the type are updated throughout the project.</span></span>

-   <span data-ttu-id="0e9cb-152">Медленно дважды щелкните сложный тип в **обозревателе моделей**.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-152">Slowly double-click a complex type in the **Model Browser**.</span></span>
    <span data-ttu-id="0e9cb-153">Имя будет выбрано в режиме редактирования.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-153">The name will be selected and in edit mode.</span></span>

- <span data-ttu-id="0e9cb-154">OR -</span><span class="sxs-lookup"><span data-stu-id="0e9cb-154">OR -</span></span>

-   <span data-ttu-id="0e9cb-155">Щелкните правой кнопкой мыши сложный тип в **обозревателе моделей** и выберите команду **Переименовать**.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-155">Right-click a complex type in the **Model Browser** and select **Rename**.</span></span>

- <span data-ttu-id="0e9cb-156">OR -</span><span class="sxs-lookup"><span data-stu-id="0e9cb-156">OR -</span></span>

-   <span data-ttu-id="0e9cb-157">Выберите сложный тип в браузере моделей и нажмите клавишу F2.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-157">Select a complex type in the Model Browser and press the F2 key.</span></span>

- <span data-ttu-id="0e9cb-158">OR -</span><span class="sxs-lookup"><span data-stu-id="0e9cb-158">OR -</span></span>

-   <span data-ttu-id="0e9cb-159">Щелкните правой кнопкой мыши сложный тип в **обозревателе моделей** и выберите пункт **свойства**.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-159">Right-click a complex type in the **Model Browser** and select **Properties**.</span></span> <span data-ttu-id="0e9cb-160">Измените имя в окне **Свойства**   .</span><span class="sxs-lookup"><span data-stu-id="0e9cb-160">Edit the name in the **Properties** window.</span></span>

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a><span data-ttu-id="0e9cb-161">Добавьте существующий сложный тип в сущность и сопоставьте его свойства со столбцами таблицы.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-161">Add an Existing Complex Type to an Entity and Map its Properties to Table Columns</span></span>

1.  <span data-ttu-id="0e9cb-162">Щелкните сущность правой кнопкой мыши, наведите указатель на пункт **Добавить новый**и выберите пункт **сложное свойство**.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-162">Right-click an entity, point to **Add New**, and select **Complex Property**.</span></span>
    <span data-ttu-id="0e9cb-163">Свойство сложного типа с именем по умолчанию будет добавлено к сущности.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-163">A complex type property with a default name is added to the entity.</span></span> <span data-ttu-id="0e9cb-164">Свойству назначается тип по умолчанию (выбранный из имеющихся сложных типов).</span><span class="sxs-lookup"><span data-stu-id="0e9cb-164">A default type (chosen from the existing complex types) is assigned to the property.</span></span>
2.  <span data-ttu-id="0e9cb-165">Назначьте требуемый тип свойству в окне **Свойства**   .</span><span class="sxs-lookup"><span data-stu-id="0e9cb-165">Assign the desired type to the property in the **Properties** window.</span></span>
    <span data-ttu-id="0e9cb-166">После добавления свойства сложного типа к сущности необходимо сопоставить ее свойства со столбцами таблицы.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-166">After adding a complex type property to an entity, you must map its properties to table columns.</span></span>
3.  <span data-ttu-id="0e9cb-167">Щелкните правой кнопкой мыши тип сущности в области конструктора или в **обозревателе моделей**   и выберите **сопоставления таблиц**.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-167">Right-click an entity type on the design surface or in the **Model Browser** and select **Table Mappings**.</span></span>
    <span data-ttu-id="0e9cb-168">Сопоставления таблиц отображаются в окне **сведения о сопоставлении**   .</span><span class="sxs-lookup"><span data-stu-id="0e9cb-168">The table mappings are displayed in the **Mapping Details** window.</span></span>
4.  <span data-ttu-id="0e9cb-169">Разверните узел **сопоставления с &lt; именем &gt; таблицы**   .</span><span class="sxs-lookup"><span data-stu-id="0e9cb-169">Expand the **Maps to &lt;Table Name&gt;** node.</span></span>
    <span data-ttu-id="0e9cb-170">Появится узел **сопоставления столбцов**   .</span><span class="sxs-lookup"><span data-stu-id="0e9cb-170">A **Column Mappings** node appears.</span></span>
5.  <span data-ttu-id="0e9cb-171">Разверните узел **сопоставления столбцов**   .</span><span class="sxs-lookup"><span data-stu-id="0e9cb-171">Expand the **Column Mappings** node.</span></span>
    <span data-ttu-id="0e9cb-172">Появится список всех столбцов таблицы.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-172">A list of all the columns in the table appears.</span></span> <span data-ttu-id="0e9cb-173">Свойства по умолчанию (если таковые имеются), к которым соответствует таблица Columns, перечислены под заголовком **значение/свойство**   .</span><span class="sxs-lookup"><span data-stu-id="0e9cb-173">The default properties (if any) to which the columns map are listed under the **Value/Property** heading.</span></span>
6.  <span data-ttu-id="0e9cb-174">Выберите столбец, который необходимо соотнести, и щелкните правой кнопкой мыши соответствующее поле **значения или свойства**   .</span><span class="sxs-lookup"><span data-stu-id="0e9cb-174">Select the column you want to map, and then right-click the corresponding **Value/Property** field.</span></span>
    <span data-ttu-id="0e9cb-175">Отобразится раскрывающийся список всех скалярных свойств.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-175">A drop-down list of all the scalar properties is displayed.</span></span>
7.  <span data-ttu-id="0e9cb-176">Выберите соответствующее свойство.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-176">Select the appropriate property.</span></span>

    ![Составной тип Map](~/ef6/media/mapcomplextype.png)

8.  <span data-ttu-id="0e9cb-178">Повторите шаги 6 и 7 для каждого столбца таблицы.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-178">Repeat steps 6 and 7 for each table column.</span></span>

>[!NOTE]
> <span data-ttu-id="0e9cb-179">Чтобы удалить сопоставление столбцов, выберите столбец, который необходимо сопоставить, а затем щелкните поле **значение/свойство**   .</span><span class="sxs-lookup"><span data-stu-id="0e9cb-179">To delete a column mapping, select the column that you want to map, and then click the **Value/Property** field.</span></span> <span data-ttu-id="0e9cb-180">Затем в раскрывающемся списке выберите **Удалить** .</span><span class="sxs-lookup"><span data-stu-id="0e9cb-180">Then, select **Delete** from the drop-down list.</span></span>

## <a name="map-a-function-import-to-a-complex-type"></a><span data-ttu-id="0e9cb-181">Преобразование импорта функции в сложный тип</span><span class="sxs-lookup"><span data-stu-id="0e9cb-181">Map a Function Import to a Complex Type</span></span>

<span data-ttu-id="0e9cb-182">Импорт функций основан на хранимых процедурах.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-182">Function imports are based on stored procedures.</span></span> <span data-ttu-id="0e9cb-183">Чтобы сопоставить импорт со сложным типом, столбцы, возвращаемые соответствующей хранимой процедурой, должны соответствовать свойствам сложного типа по числу и должны иметь типы хранения, совместимые с типами свойств.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-183">To map a function import to a complex type, the columns returned by the corresponding stored procedure must match the properties of the complex type in number and must have storage types that are compatible with the property types.</span></span>

-   <span data-ttu-id="0e9cb-184">Дважды щелкните импортированную функцию, которая должна быть сопоставлена с сложным типом.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-184">Double-click on an imported function that you want to map to a complex type.</span></span>

    ![Импорты функций](~/ef6/media/functionimports.png)

-   <span data-ttu-id="0e9cb-186">Задайте параметры для нового импорта функции следующим образом.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-186">Fill in the settings for the new function import, as follows:</span></span>
    -   <span data-ttu-id="0e9cb-187">Укажите хранимую процедуру, для которой создается импорт функции, в поле **имя хранимой процедуры**   .</span><span class="sxs-lookup"><span data-stu-id="0e9cb-187">Specify the stored procedure for which you are creating a function import in the **Stored Procedure Name** field.</span></span> <span data-ttu-id="0e9cb-188">Это поле представляет собой раскрывающийся список, содержащий все хранимые процедуры, которые имеются в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-188">This field is a drop-down list that displays all the stored procedures in the storage model.</span></span>
    -   <span data-ttu-id="0e9cb-189">Укажите имя импорта функции в поле **имя импорта функции**   .</span><span class="sxs-lookup"><span data-stu-id="0e9cb-189">Specify the name of the function import in the **Function Import Name** field.</span></span>
    -   <span data-ttu-id="0e9cb-190">Выберите **сложный**   тип возвращаемого значения, а затем укажите конкретный сложный тип возвращаемого значения, выбрав соответствующий тип из раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-190">Select **Complex** as the return type and then specify the specific complex return type by choosing the appropriate type from the drop-down list.</span></span>

        ![Изменить импорт функции](~/ef6/media/editfunctionimport.png)

-   <span data-ttu-id="0e9cb-192">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-192">Click **OK**.</span></span>
    <span data-ttu-id="0e9cb-193">В концептуальной модели создается запись импорта функции.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-193">The function import entry is created in the conceptual model.</span></span>

### <a name="customize-column-mapping-for-function-import"></a><span data-ttu-id="0e9cb-194">Настройка сопоставления столбцов для импорта функции</span><span class="sxs-lookup"><span data-stu-id="0e9cb-194">Customize Column Mapping for Function Import</span></span>

-   <span data-ttu-id="0e9cb-195">Щелкните правой кнопкой мыши импорт функции в обозревателе моделей и выберите **Сопоставление импорта функций**.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-195">Right-click the function import in the Model Browser and select **Function Import Mapping**.</span></span>
    <span data-ttu-id="0e9cb-196">Появится окно **сведения о сопоставлении**, в котором будет   показано сопоставление по умолчанию для импорта функции.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-196">The **Mapping Details** window appears and shows the default mapping for the function import.</span></span> <span data-ttu-id="0e9cb-197">Стрелки указывают сопоставления между значениями столбцов и значениями свойств.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-197">Arrows indicate the mappings between column values and property values.</span></span> <span data-ttu-id="0e9cb-198">По умолчанию предполагается, что имена столбцов совпадают с именами свойств сложного типа.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-198">By default, the column names are assumed to be the same as the complex type's property names.</span></span> <span data-ttu-id="0e9cb-199">Имена столбцов по умолчанию отображаются серым текстом.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-199">The default column names appear in gray text.</span></span>
-   <span data-ttu-id="0e9cb-200">Если необходимо, измените имена столбцов таким образом, чтобы они совпадали с именами столбцов, возвращаемых хранимой процедурой, соответствующей импорту функции.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-200">If necessary, change the column names to match the column names that are returned by the stored procedure that corresponds to the function import.</span></span>

## <a name="delete-a-complex-type"></a><span data-ttu-id="0e9cb-201">Удаление сложного типа</span><span class="sxs-lookup"><span data-stu-id="0e9cb-201">Delete a Complex Type</span></span>

<span data-ttu-id="0e9cb-202">При удалении сложного типа он удаляется из концептуальной модели. Кроме того, удаляются сопоставления для всех экземпляров типа.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-202">When you delete a complex type, the type is deleted from the conceptual model and mappings for all instances of the type are deleted.</span></span> <span data-ttu-id="0e9cb-203">Однако ссылки на тип не обновляются.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-203">However, references to the type are not updated.</span></span> <span data-ttu-id="0e9cb-204">Например, если сущность имеет свойство сложного типа типа ComplexType1 и ComplexType1 удаляется в **обозревателе моделей**, соответствующее свойство сущности не обновляется.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-204">For example, if an entity has a complex type property of type ComplexType1 and ComplexType1 is deleted in the **Model Browser**, the corresponding entity property is not updated.</span></span> <span data-ttu-id="0e9cb-205">Модель не будет проверена, так как она содержит сущность, ссылающуюся на удаленный сложный тип.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-205">The model will not validate  because it contains an entity that references a deleted complex type.</span></span> <span data-ttu-id="0e9cb-206">Обновить или удалить ссылки на удаленные сложные типы можно с помощью конструктора сущностей.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-206">You can update or delete references to deleted complex types by using the Entity Designer.</span></span>

-   <span data-ttu-id="0e9cb-207">Щелкните в обозревателе моделей правой кнопкой мыши сложный тип и выберите пункт **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-207">Right-click a complex type in the Model Browser and select **Delete**.</span></span>

- <span data-ttu-id="0e9cb-208">OR -</span><span class="sxs-lookup"><span data-stu-id="0e9cb-208">OR -</span></span>

-   <span data-ttu-id="0e9cb-209">В браузере моделей выберите сложный тип и нажмите на клавиатуре клавишу DELETE.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-209">Select a complex type in the Model Browser and press the Delete key on your keyboard.</span></span>

## <a name="query-for-entities-containing-properties-of-complex-type"></a><span data-ttu-id="0e9cb-210">Запрос сущностей, содержащих свойства сложного типа</span><span class="sxs-lookup"><span data-stu-id="0e9cb-210">Query for Entities Containing Properties of Complex Type</span></span>

<span data-ttu-id="0e9cb-211">В следующем коде показано, как выполнить запрос, возвращающий коллекцию объектов типа сущности, содержащих свойство сложного типа.</span><span class="sxs-lookup"><span data-stu-id="0e9cb-211">The following code shows how to execute a query that returns a collection of entity type objects that contain a complex type property.</span></span>

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
