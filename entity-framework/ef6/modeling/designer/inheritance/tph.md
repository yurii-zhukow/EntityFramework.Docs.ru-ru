---
title: Наследование TPH конструктора — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 43ba34a98c3960a7a3052a00e2ed2751c2f2b121
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490132"
---
# <a name="designer-tph-inheritance"></a><span data-ttu-id="ba0b8-102">Конструктора МОДЕЛИ наследования</span><span class="sxs-lookup"><span data-stu-id="ba0b8-102">Designer TPH Inheritance</span></span>
<span data-ttu-id="ba0b8-103">Это пошаговое руководство показывает, как реализовать наследование таблица на иерархию (TPH) в концептуальной модели с помощью Entity Framework Designer (конструктор EF).</span><span class="sxs-lookup"><span data-stu-id="ba0b8-103">This step-by-step walkthrough shows how to implement table-per-hierarchy (TPH) inheritance in your conceptual model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="ba0b8-104">Наследование TPH использует одну таблицу базы данных для сопровождения данных всех типов сущностей в иерархии наследования.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-104">TPH inheritance uses one database table to maintain data for all of the entity types in an inheritance hierarchy.</span></span>

<span data-ttu-id="ba0b8-105">В этом пошаговом руководстве мы сопоставит таблицы Person с тремя типами сущностей: человек (базовый тип), Student (является производным от пользователя) и Instructor (наследуется от Person).</span><span class="sxs-lookup"><span data-stu-id="ba0b8-105">In this walkthrough we will map the Person table to three entity types: Person (the base type), Student (derives from Person), and Instructor (derives from Person).</span></span> <span data-ttu-id="ba0b8-106">Мы создадим концептуальной модели из базы данных (Database First) и затем изменить модель для реализации МОДЕЛИ наследования, в конструкторе EF.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-106">We'll create a conceptual model from the database (Database First) and then alter the model to implement the TPH inheritance using the EF Designer.</span></span>

<span data-ttu-id="ba0b8-107">Можно сопоставить МОДЕЛИ наследования, с помощью Model First, но вам пришлось бы писать собственные рабочего процесса создания базы данных, который является сложным.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-107">It is possible to map to a TPH inheritance using Model First but you would have to write your own database generation workflow which is complex.</span></span> <span data-ttu-id="ba0b8-108">Можно затем назначить этот рабочий процесс для **рабочего процесса создания базы данных** свойства в конструкторе EF.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-108">You would then assign this workflow to the **Database Generation Workflow** property in the EF Designer.</span></span> <span data-ttu-id="ba0b8-109">Простой альтернативой является использование Code First.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-109">An easier alternative is to use Code First.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="ba0b8-110">Другие параметры наследования</span><span class="sxs-lookup"><span data-stu-id="ba0b8-110">Other Inheritance Options</span></span>

<span data-ttu-id="ba0b8-111">Одна таблица на тип (TPT) – еще один тип наследования, в котором отдельные таблицы в базе данных, сопоставляются сущности, участвующие в наследовании.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-111">Table-per-Type (TPT) is another type of inheritance in which separate tables in the database are mapped to entities that participate in the inheritance.</span></span>  <span data-ttu-id="ba0b8-112">Сведения о сопоставлении таблица на тип наследования в конструкторе EF, см. в разделе [наследование TPT конструктор EF](~/ef6/modeling/designer/inheritance/tpt.md).</span><span class="sxs-lookup"><span data-stu-id="ba0b8-112">For information about how to map Table-per-Type inheritance with the EF Designer, see [EF Designer TPT Inheritance](~/ef6/modeling/designer/inheritance/tpt.md).</span></span>

<span data-ttu-id="ba0b8-113">Таблица на конкретный тип наследования (TPC) и модели смешанного наследования поддерживаются средой выполнения Entity Framework, но не поддерживаются в конструкторе EF.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-113">Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="ba0b8-114">Если вы хотите использовать TPC или смешанных наследования, можно двумя способами: использовать Code First, или вручную изменить EDMX-файла.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-114">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="ba0b8-115">Если вы предпочтете работать с файлом EDMX, окно сведений о сопоставлении будет помещен в «безопасный режим», и вы не сможете использовать конструктор для изменения сопоставлений.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-115">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ba0b8-116">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="ba0b8-116">Prerequisites</span></span>

<span data-ttu-id="ba0b8-117">Для выполнения данного пошагового руководства требуется:</span><span class="sxs-lookup"><span data-stu-id="ba0b8-117">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="ba0b8-118">Последнюю версию Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-118">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="ba0b8-119">[Образца базы данных School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="ba0b8-119">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="ba0b8-120">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="ba0b8-120">Set up the Project</span></span>

-   <span data-ttu-id="ba0b8-121">Откройте Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-121">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="ba0b8-122">Выберите **файл -&gt; Новинка —&gt; проекта**</span><span class="sxs-lookup"><span data-stu-id="ba0b8-122">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="ba0b8-123">В левой области щелкните **Visual C\#**, а затем выберите **консоли** шаблона.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-123">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="ba0b8-124">Введите **TPHDBFirstSample** как имя.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-124">Enter **TPHDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="ba0b8-125">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-125">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="ba0b8-126">Создание модели</span><span class="sxs-lookup"><span data-stu-id="ba0b8-126">Create a Model</span></span>

-   <span data-ttu-id="ba0b8-127">Щелкните правой кнопкой мыши имя проекта в обозревателе решений и выберите **Add -&gt; новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-127">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="ba0b8-128">Выберите **данных** меню слева, а затем выберите **ADO.NET Entity Data Model** в области шаблонов.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-128">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="ba0b8-129">Введите **TPHModel.edmx** имя файла, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-129">Enter **TPHModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="ba0b8-130">В диалоговом окне Выбор содержимого модели выберите **создать из базы данных**, а затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-130">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="ba0b8-131">Нажмите кнопку **новое подключение**.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-131">Click **New Connection**.</span></span>
    <span data-ttu-id="ba0b8-132">В диалоговом окне Свойства соединения, введите имя сервера (например, **(localdb)\\mssqllocaldb**) выберите метод проверки подлинности, тип **School** для имени базы данных, а затем Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-132">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="ba0b8-133">В диалоговом окне Выбор подключения к базе данных обновляется параметр подключения базы данных.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-133">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="ba0b8-134">В диалоговом окне Выбор объектов базы данных в узле таблицы выберите **Person** таблицы.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-134">In the Choose Your Database Objects dialog box, under the Tables node, select the **Person** table.</span></span>
-   <span data-ttu-id="ba0b8-135">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-135">Click **Finish**.</span></span>

<span data-ttu-id="ba0b8-136">Конструктор сущностей, который предоставляет область конструктора для изменения модели, отображается.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-136">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="ba0b8-137">Все объекты, которые выбраны в диалоговом окне Выбор объектов базы данных добавляются в модель.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-137">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

<span data-ttu-id="ba0b8-138">То есть как **Person** таблица выглядит в базе данных.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-138">That is how the **Person** table looks in the database.</span></span>

![Таблица Person](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a><span data-ttu-id="ba0b8-140">Реализация наследования таблица на иерархию</span><span class="sxs-lookup"><span data-stu-id="ba0b8-140">Implement Table-per-Hierarchy Inheritance</span></span>

<span data-ttu-id="ba0b8-141">**Person** таблица имеет **дискриминатора** столбец, который может иметь одно из двух значений: «Student» и «Instructor».</span><span class="sxs-lookup"><span data-stu-id="ba0b8-141">The **Person** table has the **Discriminator** column, which can have one of two values: “Student” and “Instructor”.</span></span> <span data-ttu-id="ba0b8-142">В зависимости от значения **Person** таблицы будут сопоставлены с **учащихся** сущности или **преподавателя** сущности.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-142">Depending on the value the **Person** table will be mapped to the **Student** entity or the **Instructor** entity.</span></span> <span data-ttu-id="ba0b8-143">**Person** таблица также содержит два столбца, **HireDate** и **EnrollmentDate**, который должен быть **допускает значения NULL** так, как пользователь не может быть Student и instructor, в то же время (по крайней мере не в этом пошаговом руководстве).</span><span class="sxs-lookup"><span data-stu-id="ba0b8-143">The **Person** table also has two columns, **HireDate** and **EnrollmentDate**, which must be **nullable** because a person cannot be a student and an instructor at the same time (at least not in this walkthrough).</span></span>

### <a name="add-new-entities"></a><span data-ttu-id="ba0b8-144">Добавление новых сущностей</span><span class="sxs-lookup"><span data-stu-id="ba0b8-144">Add new Entities</span></span>

-   <span data-ttu-id="ba0b8-145">Добавьте новую сущность.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-145">Add a new entity.</span></span>
    <span data-ttu-id="ba0b8-146">Чтобы сделать это, щелкните правой кнопкой мыши на пустую часть области конструктора в конструкторе Entity Framework и выберите **Add -&gt;сущности**.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-146">To do this, right-click on an empty space of the design surface of the Entity Framework Designer, and select **Add-&gt;Entity**.</span></span>
-   <span data-ttu-id="ba0b8-147">Тип **преподавателя** для **имя сущности** и выберите **Person** из раскрывающегося списка для **базовый тип**.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-147">Type **Instructor** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>
-   <span data-ttu-id="ba0b8-148">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-148">Click **OK**.</span></span>
-   <span data-ttu-id="ba0b8-149">Добавьте другой новой сущности.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-149">Add another new entity.</span></span> <span data-ttu-id="ba0b8-150">Тип **учащихся** для **имя сущности** и выберите **Person** из раскрывающегося списка для **базовый тип**.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-150">Type **Student** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>

<span data-ttu-id="ba0b8-151">Были добавлены два новых типа сущности в область конструктора.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-151">Two new entity types were added to the design surface.</span></span> <span data-ttu-id="ba0b8-152">Стрелка идет от новых типов сущности к **Person** типа сущности; это означает, что **Person** является базовым типом для новых типов сущности.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-152">An arrow points from the new entity types to the **Person** entity type; this indicates that **Person** is the base type for the new entity types.</span></span>

-   <span data-ttu-id="ba0b8-153">Щелкните правой кнопкой мыши **HireDate** свойство **Person** сущности.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-153">Right-click the **HireDate** property of the **Person** entity.</span></span> <span data-ttu-id="ba0b8-154">Выберите **Вырезать** (или с помощью клавиши Ctrl + X).</span><span class="sxs-lookup"><span data-stu-id="ba0b8-154">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="ba0b8-155">Щелкните правой кнопкой мыши **преподавателя** сущности и выберите **вставить** (или с помощью клавиши Ctrl + V).</span><span class="sxs-lookup"><span data-stu-id="ba0b8-155">Right-click the **Instructor** entity and select **Paste** (or use the Ctrl-V key).</span></span>
-   <span data-ttu-id="ba0b8-156">Щелкните правой кнопкой мыши **HireDate** свойство и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-156">Right-click the **HireDate** property and select **Properties**.</span></span>
-   <span data-ttu-id="ba0b8-157">В **свойства** окне **Nullable** свойства **false**.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-157">In the **Properties** window, set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="ba0b8-158">Щелкните правой кнопкой мыши **EnrollmentDate** свойство **Person** сущности.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-158">Right-click the **EnrollmentDate** property of the **Person** entity.</span></span> <span data-ttu-id="ba0b8-159">Выберите **Вырезать** (или с помощью клавиши Ctrl + X).</span><span class="sxs-lookup"><span data-stu-id="ba0b8-159">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="ba0b8-160">Щелкните правой кнопкой мыши **учащихся** сущности и выберите **вставки (или раздела используйте Ctrl + V).**</span><span class="sxs-lookup"><span data-stu-id="ba0b8-160">Right-click the **Student** entity and select **Paste(or use the Ctrl-V key).**</span></span>
-   <span data-ttu-id="ba0b8-161">Выберите **EnrollmentDate** и установите **Nullable** свойства **false**.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-161">Select the **EnrollmentDate** property and set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="ba0b8-162">Выберите **Person** типа сущности.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-162">Select the **Person** entity type.</span></span> <span data-ttu-id="ba0b8-163">В **свойства** окна, задайте его **абстрактный** свойства **true**.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-163">In the **Properties** window, set its **Abstract** property to **true**.</span></span>
-   <span data-ttu-id="ba0b8-164">Удалить **дискриминатора** свойства из **Person**.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-164">Delete the **Discriminator** property from **Person**.</span></span> <span data-ttu-id="ba0b8-165">В следующем разделе объясняется причина, по которой ее следует удалить.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-165">The reason it should be deleted is explained in the following section.</span></span>

### <a name="map-the-entities"></a><span data-ttu-id="ba0b8-166">Сопоставьте сущности</span><span class="sxs-lookup"><span data-stu-id="ba0b8-166">Map the entities</span></span>

-   <span data-ttu-id="ba0b8-167">Щелкните правой кнопкой мыши **преподавателя** и выберите **сопоставление таблицы.**</span><span class="sxs-lookup"><span data-stu-id="ba0b8-167">Right-click the **Instructor** and select **Table Mapping.**</span></span>
    <span data-ttu-id="ba0b8-168">Сущности Instructor выбран в окне «сведения о сопоставлении».</span><span class="sxs-lookup"><span data-stu-id="ba0b8-168">The Instructor entity is selected in the Mapping Details window.</span></span>
-   <span data-ttu-id="ba0b8-169">Нажмите кнопку **&lt;добавить таблицу или представление&gt;** в **сведения о сопоставлении** окна.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-169">Click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
    <span data-ttu-id="ba0b8-170">**&lt;Добавить таблицу или представление&gt;** поле становится стрелку раскрывающегося списка таблиц и представлений, с которой могут быть сопоставлены выбранной сущности.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-170">The **&lt;Add a Table or View&gt;** field becomes a drop-down list of tables or views to which the selected entity can be mapped.</span></span>
-   <span data-ttu-id="ba0b8-171">Выберите **Person** из раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-171">Select **Person** from the drop-down list.</span></span>
-   <span data-ttu-id="ba0b8-172">**Сведения о сопоставлении** окно обновляется с помощью сопоставления столбцов по умолчанию и параметр для добавления условия.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-172">The **Mapping Details** window is updated with default column mappings and an option for adding a condition.</span></span>
-   <span data-ttu-id="ba0b8-173">Щелкните  **&lt;добавить условие&gt;**.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-173">Click on **&lt;Add a Condition&gt;**.</span></span>
    <span data-ttu-id="ba0b8-174">**&lt;Добавить условие&gt;** поле становится стрелку раскрывающегося списка столбцов, для которых можно задать условия.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-174">The **&lt;Add a Condition&gt;** field becomes a drop-down list of columns for which conditions can be set.</span></span>
-   <span data-ttu-id="ba0b8-175">Выберите **дискриминатора** из раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-175">Select **Discriminator** from the drop-down list.</span></span>
-   <span data-ttu-id="ba0b8-176">В **оператор** столбец **сведения о сопоставлении** выберите = из раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-176">In the **Operator** column of the **Mapping Details** window, select = from the drop-down list.</span></span>
-   <span data-ttu-id="ba0b8-177">В **значение/свойство** столбец, тип **преподавателя**.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-177">In the **Value/Property** column, type **Instructor**.</span></span> <span data-ttu-id="ba0b8-178">Конечный результат должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ba0b8-178">The end result should look like this:</span></span>

    ![Сведения о сопоставлении](~/ef6/media/mappingdetails2.png)

-   <span data-ttu-id="ba0b8-180">Повторите эти шаги для **учащихся** тип сущности, но создать условие равно **учащихся** значение.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-180">Repeat these steps for the **Student** entity type, but make the condition equal to **Student** value.</span></span>  
    <span data-ttu-id="ba0b8-181">*Причина, мы хотели бы удалить **дискриминатора** свойство, является, поскольку невозможно сопоставить столбец таблицы более одного раза. Этот столбец будет использоваться для условного сопоставления, поэтому его нельзя использовать для сопоставления свойства, а также. Единственным способом, он может использоваться в обоих случаях если условие использует **Is Null** или **Is Not Null** сравнения.*</span><span class="sxs-lookup"><span data-stu-id="ba0b8-181">*The reason we wanted to remove the **Discriminator** property, is because you cannot map a table column more than once. This column will be used for conditional mapping, so it cannot be used for property mapping as well. The only way it can be used for both, if a condition uses an **Is Null** or **Is Not Null** comparison.*</span></span>

<span data-ttu-id="ba0b8-182">Теперь наследование типа «одна таблица на иерархию» успешно реализовано.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-182">Table-per-hierarchy inheritance is now implemented.</span></span>

![Окончательный TPH](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a><span data-ttu-id="ba0b8-184">Использование модели</span><span class="sxs-lookup"><span data-stu-id="ba0b8-184">Use the Model</span></span>

<span data-ttu-id="ba0b8-185">Откройте **Program.cs** файл, в котором **Main** определен метод.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-185">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="ba0b8-186">Вставьте следующий код в **Main** функции.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-186">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="ba0b8-187">Код выполняет три запроса.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-187">The code executes three queries.</span></span> <span data-ttu-id="ba0b8-188">Первый запрос возвращает все **Person** объектов.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-188">The first query brings back all **Person** objects.</span></span> <span data-ttu-id="ba0b8-189">Второй запрос использует **OfType** метод для возврата **преподавателя** объектов.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-189">The second query uses the **OfType** method to return **Instructor** objects.</span></span> <span data-ttu-id="ba0b8-190">Третий запрос использует **OfType** метод для возврата **учащихся** объектов.</span><span class="sxs-lookup"><span data-stu-id="ba0b8-190">The third query uses the **OfType** method to return **Student** objects.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Console.WriteLine("All people:");
        foreach (var person in context.People)
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Instructors only: ");
        foreach (var person in context.People.OfType<Instructor>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Students only: ");
        foreach (var person in context.People.OfType<Student>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }
    }
```
