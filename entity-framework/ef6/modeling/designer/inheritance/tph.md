---
title: Наследование конструктора с подиерархией — EF6
description: Наследование конструктора ИЕРАРХИй в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/designer/inheritance/tph
ms.openlocfilehash: bf689e02a76e46e5eee5a64cb9825e667c8cd65c
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92064852"
---
# <a name="designer-tph-inheritance"></a><span data-ttu-id="b1fdf-103">Наследование конструктора с подиерархией</span><span class="sxs-lookup"><span data-stu-id="b1fdf-103">Designer TPH Inheritance</span></span>
<span data-ttu-id="b1fdf-104">В этом пошаговом руководстве показано, как реализовать наследование "одна таблица на иерархию" в концептуальной модели с помощью Entity Framework Designer (конструктор EF).</span><span class="sxs-lookup"><span data-stu-id="b1fdf-104">This step-by-step walkthrough shows how to implement table-per-hierarchy (TPH) inheritance in your conceptual model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="b1fdf-105">Наследование «подстановка» использует одну таблицу базы данных для сохранения данных всех типов сущностей в иерархии наследования.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-105">TPH inheritance uses one database table to maintain data for all of the entity types in an inheritance hierarchy.</span></span>

<span data-ttu-id="b1fdf-106">В этом пошаговом руководстве мы сопоставлению таблицы Person с тремя типами сущностей: Person (базовый тип), Student (является производным от Person) и инструктором (от человека).</span><span class="sxs-lookup"><span data-stu-id="b1fdf-106">In this walkthrough we will map the Person table to three entity types: Person (the base type), Student (derives from Person), and Instructor (derives from Person).</span></span> <span data-ttu-id="b1fdf-107">Мы создадим концептуальную модель из базы данных (Database First), а затем изменим модель, чтобы реализовать наследование с помощью конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-107">We'll create a conceptual model from the database (Database First) and then alter the model to implement the TPH inheritance using the EF Designer.</span></span>

<span data-ttu-id="b1fdf-108">Можно сопоставляться с наследованием с помощью Model First, но вам пришлось бы написать собственный рабочий процесс создания базы данных, который является сложным.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-108">It is possible to map to a TPH inheritance using Model First but you would have to write your own database generation workflow which is complex.</span></span> <span data-ttu-id="b1fdf-109">Затем этот рабочий процесс будет назначен свойству **рабочего процесса создания базы данных** в конструкторе EF.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-109">You would then assign this workflow to the **Database Generation Workflow** property in the EF Designer.</span></span> <span data-ttu-id="b1fdf-110">Более простой альтернативой является использование Code First.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-110">An easier alternative is to use Code First.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="b1fdf-111">Другие параметры наследования</span><span class="sxs-lookup"><span data-stu-id="b1fdf-111">Other Inheritance Options</span></span>

<span data-ttu-id="b1fdf-112">"Одна таблица на тип" (TPT) — это еще один тип наследования, в котором отдельные таблицы в базе данных сопоставляются с сущностями, участвующими в наследовании.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-112">Table-per-Type (TPT) is another type of inheritance in which separate tables in the database are mapped to entities that participate in the inheritance.</span></span> <span data-ttu-id="b1fdf-113">Дополнительные сведения о сопоставлении наследования типа «таблица на тип» с помощью конструктора EF см. в разделе [TPT Designer EF наследование](xref:ef6/modeling/designer/inheritance/tpt).</span><span class="sxs-lookup"><span data-stu-id="b1fdf-113"> For information about how to map Table-per-Type inheritance with the EF Designer, see [EF Designer TPT Inheritance](xref:ef6/modeling/designer/inheritance/tpt).</span></span>

<span data-ttu-id="b1fdf-114">Модель с принаследованием типа "Таблица — конкретная" (TPC) и смешанные модели наследования поддерживаются средой выполнения Entity Framework, но не поддерживаются конструктором EF.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-114">Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="b1fdf-115">Если вы хотите использовать функции TPC или смешанное наследование, у вас есть два варианта: использовать Code First или вручную изменить файл EDMX.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-115">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="b1fdf-116">Если вы решили работать с файлом EDMX, окно сведения о сопоставлении будет переведено в "защищенный режим", и вы не сможете использовать конструктор для изменения сопоставлений.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-116">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b1fdf-117">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="b1fdf-117">Prerequisites</span></span>

<span data-ttu-id="b1fdf-118">Для выполнения данного пошагового руководства требуется:</span><span class="sxs-lookup"><span data-stu-id="b1fdf-118">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="b1fdf-119">Последняя версия Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-119">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="b1fdf-120">[Образец базы данных School](xref:ef6/resources/school-database).</span><span class="sxs-lookup"><span data-stu-id="b1fdf-120">The [School sample database](xref:ef6/resources/school-database).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="b1fdf-121">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="b1fdf-121">Set up the Project</span></span>

-   <span data-ttu-id="b1fdf-122">Откройте Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-122">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="b1fdf-123">Выбор **файла- &gt; создать- &gt; проект**</span><span class="sxs-lookup"><span data-stu-id="b1fdf-123">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="b1fdf-124">В левой области щелкните \*\*Visual C \# \*\*, а затем выберите шаблон **консоли** .</span><span class="sxs-lookup"><span data-stu-id="b1fdf-124">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="b1fdf-125">Введите **тфдбфирстсампле**в   качестве имени.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-125">Enter **TPHDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="b1fdf-126">Выберите **OK**.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-126">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="b1fdf-127">Создание модели</span><span class="sxs-lookup"><span data-stu-id="b1fdf-127">Create a Model</span></span>

-   <span data-ttu-id="b1fdf-128">Щелкните правой кнопкой мыши имя проекта в обозреватель решений и выберите **Добавить- &gt; новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-128">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="b1fdf-129">Выберите **данные** в меню слева, а затем выберите **ADO.NET EDM** в области Шаблоны.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-129">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="b1fdf-130">Введите **тфмодел. EDMX** в поле имя файла и нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-130">Enter **TPHModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="b1fdf-131">В диалоговом окне Выбор содержимого модели выберите пункт **создать из базы данных**, а затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-131">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="b1fdf-132">Нажмите кнопку **создать соединение**.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-132">Click **New Connection**.</span></span>
    <span data-ttu-id="b1fdf-133">В диалоговом окне Свойства соединения введите имя сервера (например, **(LocalDB) \\ mssqllocaldb**), выберите метод проверки подлинности, введите **School**   в качестве имени базы данных и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-133">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="b1fdf-134">В диалоговом окне Выбор подключения к данным будет обновлен параметр подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-134">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="b1fdf-135">В диалоговом окне Выбор объектов базы данных в узле таблицы выберите таблицу **Person** .</span><span class="sxs-lookup"><span data-stu-id="b1fdf-135">In the Choose Your Database Objects dialog box, under the Tables node, select the **Person** table.</span></span>
-   <span data-ttu-id="b1fdf-136">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-136">Click **Finish**.</span></span>

<span data-ttu-id="b1fdf-137">Отобразится Entity Designer, предоставляющий область конструктора для редактирования модели.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-137">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="b1fdf-138">Все объекты, выбранные в диалоговом окне Выбор объектов базы данных, добавляются в модель.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-138">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

<span data-ttu-id="b1fdf-139">Таким образом таблица **Person** будет выглядеть в базе данных.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-139">That is how the **Person** table looks in the database.</span></span>

![Таблица Person](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a><span data-ttu-id="b1fdf-141">Реализация наследования одной таблицы на иерархию</span><span class="sxs-lookup"><span data-stu-id="b1fdf-141">Implement Table-per-Hierarchy Inheritance</span></span>

<span data-ttu-id="b1fdf-142">Таблица **Person** имеет столбец **дискриминатора** , который может иметь одно из двух значений: "Student" и "Instructor".</span><span class="sxs-lookup"><span data-stu-id="b1fdf-142">The **Person** table has the **Discriminator** column, which can have one of two values: “Student” and “Instructor”.</span></span> <span data-ttu-id="b1fdf-143">В зависимости от значения Таблица **Person** будет сопоставлена с сущностью **Student** или с сущностью **Instructor** .</span><span class="sxs-lookup"><span data-stu-id="b1fdf-143">Depending on the value the **Person** table will be mapped to the **Student** entity or the **Instructor** entity.</span></span> <span data-ttu-id="b1fdf-144">Таблица **Person** также содержит два столбца, **HireDate**   и **енроллментдате**, которые должны **допускать значения NULL** , поскольку человек не может одновременно быть студентом и преподавателем (по крайней мере, в этом пошаговом руководстве).</span><span class="sxs-lookup"><span data-stu-id="b1fdf-144">The **Person** table also has two columns, **HireDate** and **EnrollmentDate**, which must be **nullable** because a person cannot be a student and an instructor at the same time (at least not in this walkthrough).</span></span>

### <a name="add-new-entities"></a><span data-ttu-id="b1fdf-145">Добавить новые сущности</span><span class="sxs-lookup"><span data-stu-id="b1fdf-145">Add new Entities</span></span>

-   <span data-ttu-id="b1fdf-146">Добавьте новую сущность.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-146">Add a new entity.</span></span>
    <span data-ttu-id="b1fdf-147">Для этого щелкните правой кнопкой мыши пустое пространство в области конструктора Entity Framework Designer и выберите пункт **Добавить- &gt; сущность**.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-147">To do this, right-click on an empty space of the design surface of the Entity Framework Designer, and select **Add-&gt;Entity**.</span></span>
-   <span data-ttu-id="b1fdf-148">Введите **Instructor**   в поле **имя сущности**   и выберите **Person**из раскрывающегося   списка для **базового типа**.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-148">Type **Instructor** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>
-   <span data-ttu-id="b1fdf-149">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-149">Click **OK**.</span></span>
-   <span data-ttu-id="b1fdf-150">Добавьте еще одну новую сущность.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-150">Add another new entity.</span></span> <span data-ttu-id="b1fdf-151">Введите **Student**   в поле **имя сущности**   и выберите **Person**из раскрывающегося   списка для **базового типа**.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-151">Type **Student** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>

<span data-ttu-id="b1fdf-152">В область конструктора были добавлены два новых типа сущностей.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-152">Two new entity types were added to the design surface.</span></span> <span data-ttu-id="b1fdf-153">Стрелка указывает от новых типов сущностей на тип сущности **Person**   ; это означает, что **пользователь**   является базовым типом для новых типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-153">An arrow points from the new entity types to the **Person** entity type; this indicates that **Person** is the base type for the new entity types.</span></span>

-   <span data-ttu-id="b1fdf-154">Щелкните правой кнопкой мыши свойство **HireDate**   сущности **Person**   .</span><span class="sxs-lookup"><span data-stu-id="b1fdf-154">Right-click the **HireDate** property of the **Person** entity.</span></span> <span data-ttu-id="b1fdf-155">Выберите **Вырезать** (или используйте клавишу CTRL + X).</span><span class="sxs-lookup"><span data-stu-id="b1fdf-155">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="b1fdf-156">Щелкните сущность **Instructor**правой кнопкой мыши   и выберите команду **Вставить** (или используйте клавишу CTRL + V).</span><span class="sxs-lookup"><span data-stu-id="b1fdf-156">Right-click the **Instructor** entity and select **Paste** (or use the Ctrl-V key).</span></span>
-   <span data-ttu-id="b1fdf-157">Щелкните правой кнопкой мыши свойство **HireDate**   и выберите пункт **Свойства**.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-157">Right-click the **HireDate** property and select **Properties**.</span></span>
-   <span data-ttu-id="b1fdf-158">В окне **Свойства**   присвойте свойству **Nullable**значение    **false**.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-158">In the **Properties** window, set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="b1fdf-159">Щелкните правой кнопкой мыши свойство **енроллментдате**   сущности **Person**   .</span><span class="sxs-lookup"><span data-stu-id="b1fdf-159">Right-click the **EnrollmentDate** property of the **Person** entity.</span></span> <span data-ttu-id="b1fdf-160">Выберите **Вырезать** (или используйте клавишу CTRL + X).</span><span class="sxs-lookup"><span data-stu-id="b1fdf-160">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="b1fdf-161">Щелкните сущность **Student** правой кнопкой мыши и выберите команду **Вставить (или используйте клавишу CTRL + V).**</span><span class="sxs-lookup"><span data-stu-id="b1fdf-161">Right-click the **Student** entity and select **Paste(or use the Ctrl-V key).**</span></span>
-   <span data-ttu-id="b1fdf-162">Выберите свойство **енроллментдате**   и присвойте **Nullable**   свойству Nullable значение **false**.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-162">Select the **EnrollmentDate** property and set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="b1fdf-163">Выберите **Person**   тип сущности Person.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-163">Select the **Person** entity type.</span></span> <span data-ttu-id="b1fdf-164">В окне **Свойства**   задайте **Abstract**   для свойства abstract значение **true**.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-164">In the **Properties** window, set its **Abstract** property to **true**.</span></span>
-   <span data-ttu-id="b1fdf-165">Удалите свойство **дискриминатора** из **Person**.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-165">Delete the **Discriminator** property from **Person**.</span></span> <span data-ttu-id="b1fdf-166">Причина его удаления объясняется в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-166">The reason it should be deleted is explained in the following section.</span></span>

### <a name="map-the-entities"></a><span data-ttu-id="b1fdf-167">Сопоставьте сущности</span><span class="sxs-lookup"><span data-stu-id="b1fdf-167">Map the entities</span></span>

-   <span data-ttu-id="b1fdf-168">Щелкните **лектора** правой кнопкой мыши и выберите пункт **Сопоставление таблиц.**</span><span class="sxs-lookup"><span data-stu-id="b1fdf-168">Right-click the **Instructor** and select **Table Mapping.**</span></span>
    <span data-ttu-id="b1fdf-169">Сущность Instructor выбирается в окне сведения о сопоставлении.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-169">The Instructor entity is selected in the Mapping Details window.</span></span>
-   <span data-ttu-id="b1fdf-170">Нажмите кнопку \*\* &lt; Добавить таблицу или представление &gt; \*\*   в окне **сведения о сопоставлении**   .</span><span class="sxs-lookup"><span data-stu-id="b1fdf-170">Click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
    <span data-ttu-id="b1fdf-171">Поле \*\* &lt; Добавить таблицу или представление &gt; \*\*преобразуется в раскрывающийся   список таблиц или представлений, к которым можно сопоставить выбранную сущность.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-171">The **&lt;Add a Table or View&gt;** field becomes a drop-down list of tables or views to which the selected entity can be mapped.</span></span>
-   <span data-ttu-id="b1fdf-172">Выберите **Person**из раскрывающегося   списка.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-172">Select **Person** from the drop-down list.</span></span>
-   <span data-ttu-id="b1fdf-173">В окне **сведения о сопоставлении**   обновляется сопоставление столбцов по умолчанию и параметр для добавления условия.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-173">The **Mapping Details** window is updated with default column mappings and an option for adding a condition.</span></span>
-   <span data-ttu-id="b1fdf-174">Щелкните \*\* &lt; Добавить условие &gt; \*\*.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-174">Click on **&lt;Add a Condition&gt;**.</span></span>
    <span data-ttu-id="b1fdf-175">Поле \*\* &lt; Добавить условие &gt; \*\*преобразуется в раскрывающийся   список столбцов, для которых можно задать условия.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-175">The **&lt;Add a Condition&gt;** field becomes a drop-down list of columns for which conditions can be set.</span></span>
-   <span data-ttu-id="b1fdf-176">Выберите **Дискриминатор**   из раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-176">Select **Discriminator** from the drop-down list.</span></span>
-   <span data-ttu-id="b1fdf-177">В столбце **оператор**   окна **сведения о сопоставлении**   выберите значение = из раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-177">In the **Operator** column of the **Mapping Details** window, select = from the drop-down list.</span></span>
-   <span data-ttu-id="b1fdf-178">В столбце **значение/свойство** введите **Instructor**.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-178">In the **Value/Property** column, type **Instructor**.</span></span> <span data-ttu-id="b1fdf-179">Конечный результат должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b1fdf-179">The end result should look like this:</span></span>

    ![Сведения о сопоставлении](~/ef6/media/mappingdetails2.png)

-   <span data-ttu-id="b1fdf-181">Повторите эти шаги для **Student**   типа сущности Student, но сделайте условие равным значению **Student** .</span><span class="sxs-lookup"><span data-stu-id="b1fdf-181">Repeat these steps for the **Student** entity type, but make the condition equal to **Student** value.</span></span>  
    <span data-ttu-id="b1fdf-182">*Причина, по которой мы хотели удалить свойство **дискриминатора** , заключается в том, что нельзя сопоставлять столбец таблицы более одного раза. Этот столбец будет использоваться для условного сопоставления, поэтому его нельзя использовать для сопоставления свойств. Единственный способ, который можно использовать для обоих, если в условии используется сравнение со \*\*значением NULL*\*   или не равно **null**   .\*</span><span class="sxs-lookup"><span data-stu-id="b1fdf-182">*The reason we wanted to remove the **Discriminator** property, is because you cannot map a table column more than once. This column will be used for conditional mapping, so it cannot be used for property mapping as well. The only way it can be used for both, if a condition uses an **Is Null** or **Is Not Null** comparison.*</span></span>

<span data-ttu-id="b1fdf-183">Теперь наследование типа «одна таблица на иерархию» успешно реализовано.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-183">Table-per-hierarchy inheritance is now implemented.</span></span>

![Окончательная подтаблица](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a><span data-ttu-id="b1fdf-185">Использование модели</span><span class="sxs-lookup"><span data-stu-id="b1fdf-185">Use the Model</span></span>

<span data-ttu-id="b1fdf-186">Откройте файл **Program.CS** , в котором определен метод **Main** .</span><span class="sxs-lookup"><span data-stu-id="b1fdf-186">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="b1fdf-187">Вставьте следующий код в функцию **Main** .</span><span class="sxs-lookup"><span data-stu-id="b1fdf-187">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="b1fdf-188">Код выполняет три запроса.</span><span class="sxs-lookup"><span data-stu-id="b1fdf-188">The code executes three queries.</span></span> <span data-ttu-id="b1fdf-189">Первый запрос возвращает все объекты **Person** .</span><span class="sxs-lookup"><span data-stu-id="b1fdf-189">The first query brings back all **Person** objects.</span></span> <span data-ttu-id="b1fdf-190">Второй запрос использует метод **OFTYPE** для возврата объектов **инструктора** .</span><span class="sxs-lookup"><span data-stu-id="b1fdf-190">The second query uses the **OfType** method to return **Instructor** objects.</span></span> <span data-ttu-id="b1fdf-191">Третий запрос использует метод **OFTYPE** для возвращения объектов **Student** .</span><span class="sxs-lookup"><span data-stu-id="b1fdf-191">The third query uses the **OfType** method to return **Student** objects.</span></span>

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
