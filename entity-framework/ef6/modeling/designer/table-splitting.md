---
title: Разделение конструктора таблицы - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
caps.latest.revision: 3
ms.openlocfilehash: 6ecf9a77f7c6a743e3902de65bb75349c6ef799f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121886"
---
# <a name="designer-table-splitting"></a><span data-ttu-id="cc36a-102">Разбиение конструктора таблиц</span><span class="sxs-lookup"><span data-stu-id="cc36a-102">Designer Table Splitting</span></span>
<span data-ttu-id="cc36a-103">В этом пошаговом руководстве показано, как сопоставить несколько типов сущностей с одной таблицей путем изменения модели с Entity Framework Designer (конструктор EF).</span><span class="sxs-lookup"><span data-stu-id="cc36a-103">This walkthrough shows how to map multiple entity types to a single table by modifying a model with the Entity Framework Designer (EF Designer).</span></span>

<span data-ttu-id="cc36a-104">Одна из причин, которые можно использовать разбиение таблиц задерживает загрузки некоторых свойств при использовании отложенной загрузки для загрузки объектов.</span><span class="sxs-lookup"><span data-stu-id="cc36a-104">One reason you may want to use table splitting is delaying the loading of some properties when using lazy loading to load your objects.</span></span> <span data-ttu-id="cc36a-105">Свойства, которые могут содержать очень большой объем данных в отдельную сущность и загружать только его при необходимости можно разделить.</span><span class="sxs-lookup"><span data-stu-id="cc36a-105">You can separate the properties that might contain very large amount of data into a seperate entity and only load it when required.</span></span>

<span data-ttu-id="cc36a-106">На следующем рисунке показана основные окна, которые используются при работе с конструктором EF.</span><span class="sxs-lookup"><span data-stu-id="cc36a-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EFDesigner](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="cc36a-108">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="cc36a-108">Prerequisites</span></span>

<span data-ttu-id="cc36a-109">Для выполнения данного пошагового руководства требуется:</span><span class="sxs-lookup"><span data-stu-id="cc36a-109">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="cc36a-110">Последнюю версию Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cc36a-110">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="cc36a-111">[Образца базы данных School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="cc36a-111">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="cc36a-112">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="cc36a-112">Set up the Project</span></span>

<span data-ttu-id="cc36a-113">В этом пошаговом руководстве используется Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="cc36a-113">This walkthrough is using Visual Studio 2012.</span></span>

-   <span data-ttu-id="cc36a-114">Откройте Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="cc36a-114">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="cc36a-115">В меню **Файл** выберите пункт **Создать**, а затем команду **Проект**.</span><span class="sxs-lookup"><span data-stu-id="cc36a-115">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="cc36a-116">В левой области выберите Visual C\#, а затем выберите шаблон консольного приложения.</span><span class="sxs-lookup"><span data-stu-id="cc36a-116">In the left pane, click Visual C\#, and then select the Console Application template.</span></span>
-   <span data-ttu-id="cc36a-117">Введите **TableSplittingSample** как имя проекта и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="cc36a-117">Enter **TableSplittingSample** as the name of the project and click **OK**.</span></span>

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="cc36a-118">Создать модель, основанную на базе данных School</span><span class="sxs-lookup"><span data-stu-id="cc36a-118">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="cc36a-119">Щелкните правой кнопкой мыши имя проекта в обозревателе решений, выберите пункт **добавить**, а затем нажмите кнопку **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="cc36a-119">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="cc36a-120">Выберите **данных** меню слева, а затем выберите **ADO.NET Entity Data Model** в области шаблонов.</span><span class="sxs-lookup"><span data-stu-id="cc36a-120">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="cc36a-121">Введите **TableSplittingModel.edmx** имя файла, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="cc36a-121">Enter **TableSplittingModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="cc36a-122">В диалоговом окне Выбор содержимого модели выберите **создать из базы данных**, а затем нажмите кнопку **Далее.**</span><span class="sxs-lookup"><span data-stu-id="cc36a-122">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="cc36a-123">Щелкните новое подключение.</span><span class="sxs-lookup"><span data-stu-id="cc36a-123">Click New Connection.</span></span> <span data-ttu-id="cc36a-124">В диалоговом окне Свойства соединения, введите имя сервера (например, **(localdb)\\mssqllocaldb**) выберите метод проверки подлинности, тип **School** для имени базы данных, а затем Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="cc36a-124">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="cc36a-125">В диалоговом окне Выбор подключения к базе данных обновляется параметр подключения базы данных.</span><span class="sxs-lookup"><span data-stu-id="cc36a-125">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="cc36a-126">В диалоговом окне Выбор объектов базы данных можно раскрыть **таблиц** узел и проверьте **Person** таблицы.</span><span class="sxs-lookup"><span data-stu-id="cc36a-126">In the Choose Your Database Objects dialog box, unfold the **Tables** node and check the **Person** table.</span></span> <span data-ttu-id="cc36a-127">Это добавит к указанной таблице **School** модели.</span><span class="sxs-lookup"><span data-stu-id="cc36a-127">This will add the specified table to the **School** model.</span></span>
-   <span data-ttu-id="cc36a-128">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="cc36a-128">Click **Finish**.</span></span>

<span data-ttu-id="cc36a-129">Конструктор сущностей, который предоставляет область конструктора для изменения модели, отображается.</span><span class="sxs-lookup"><span data-stu-id="cc36a-129">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="cc36a-130">Все объекты, которые выбраны в **Choose Your Database Objects** диалоговое окно добавляются в модель.</span><span class="sxs-lookup"><span data-stu-id="cc36a-130">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>

## <a name="map-two-entities-to-a-single-table"></a><span data-ttu-id="cc36a-131">Сопоставить две сущности с одной таблицей</span><span class="sxs-lookup"><span data-stu-id="cc36a-131">Map Two Entities to a Single Table</span></span>

<span data-ttu-id="cc36a-132">В этом разделе вы разделите **Person** сущность на две сущности и сопоставьте их с одной таблицей.</span><span class="sxs-lookup"><span data-stu-id="cc36a-132">In this section you will split the **Person** entity into two entities and then map them to a single table.</span></span>

> [!NOTE]
> <span data-ttu-id="cc36a-133">**Person** сущность не содержит все свойства, которые могут содержать большой объем данных; он используется только в качестве примера.</span><span class="sxs-lookup"><span data-stu-id="cc36a-133">The **Person** entity does not contain any properties that may contain large amount of data; it is just used as an example.</span></span>

-   <span data-ttu-id="cc36a-134">Щелкните правой кнопкой мыши пустой участок области конструктора, выберите пункт **Add New**и нажмите кнопку **сущности**.</span><span class="sxs-lookup"><span data-stu-id="cc36a-134">Right-click an empty area of the design surface, point to **Add New**, and click **Entity**.</span></span>
    <span data-ttu-id="cc36a-135">**Новую сущность** откроется диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="cc36a-135">The **New Entity** dialog box appears.</span></span>
-   <span data-ttu-id="cc36a-136">Тип **HireInfo** для **имя сущности** и **PersonID** для **свойство Key** имя.</span><span class="sxs-lookup"><span data-stu-id="cc36a-136">Type **HireInfo** for the **Entity name** and **PersonID** for the **Key Property** name.</span></span>
-   <span data-ttu-id="cc36a-137">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="cc36a-137">Click **OK**.</span></span>
-   <span data-ttu-id="cc36a-138">Новый тип сущности будет создан и отобразится в области конструктора.</span><span class="sxs-lookup"><span data-stu-id="cc36a-138">A new entity type is created and displayed on the design surface.</span></span>
-   <span data-ttu-id="cc36a-139">Выберите **HireDate** свойство **Person** типа сущности и нажмите клавишу **Ctrl + X** ключи.</span><span class="sxs-lookup"><span data-stu-id="cc36a-139">Select the **HireDate** property of the **Person** entity type and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="cc36a-140">Выберите **HireInfo** сущности и нажмите клавишу **Ctrl + V** ключи.</span><span class="sxs-lookup"><span data-stu-id="cc36a-140">Select the **HireInfo** entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="cc36a-141">Создание ассоциации между **Person** и **HireInfo**.</span><span class="sxs-lookup"><span data-stu-id="cc36a-141">Create an association between **Person** and **HireInfo**.</span></span> <span data-ttu-id="cc36a-142">Чтобы сделать это, щелкните правой кнопкой мыши пустой участок области конструктора, выберите пункт **Add New**и нажмите кнопку **ассоциации**.</span><span class="sxs-lookup"><span data-stu-id="cc36a-142">To do this, right-click an empty area of the design surface, point to **Add New**, and click **Association**.</span></span>
-   <span data-ttu-id="cc36a-143">**Добавить сопоставление** откроется диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="cc36a-143">The **Add Association** dialog box appears.</span></span> <span data-ttu-id="cc36a-144">**PersonHireInfo** присвоено имя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cc36a-144">The **PersonHireInfo** name is given by default.</span></span>
-   <span data-ttu-id="cc36a-145">Укажите кратность **1(One)** на обеих сторонах связи.</span><span class="sxs-lookup"><span data-stu-id="cc36a-145">Specify multiplicity **1(One)** on both ends of the relationship.</span></span>
-   <span data-ttu-id="cc36a-146">Нажмите клавишу **ОК**.</span><span class="sxs-lookup"><span data-stu-id="cc36a-146">Press **OK**.</span></span>

<span data-ttu-id="cc36a-147">Для следующего шага требуется **сведения о сопоставлении** окна.</span><span class="sxs-lookup"><span data-stu-id="cc36a-147">The next step requires the **Mapping Details** window.</span></span> <span data-ttu-id="cc36a-148">Если это окно не отображается, щелкните правой кнопкой мыши область конструктора и выберите **сведения о сопоставлении**.</span><span class="sxs-lookup"><span data-stu-id="cc36a-148">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="cc36a-149">Выберите **HireInfo** типа сущности и нажмите кнопку **&lt;добавить таблицу или представление&gt;** в **сведения о сопоставлении** окна.</span><span class="sxs-lookup"><span data-stu-id="cc36a-149">Select the **HireInfo** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="cc36a-150">Выберите **Person** из **&lt;добавить таблицу или представление&gt;** поле с раскрывающимся списком.</span><span class="sxs-lookup"><span data-stu-id="cc36a-150">Select **Person** from the **&lt;Add a Table or View&gt;** field drop-down list.</span></span> <span data-ttu-id="cc36a-151">Список содержит таблицы или представления, который может быть сопоставлен выбранной сущности.</span><span class="sxs-lookup"><span data-stu-id="cc36a-151">The list contains tables or views to which the selected entity can be mapped.</span></span>
    <span data-ttu-id="cc36a-152">Необходимо сопоставить соответствующие свойства по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cc36a-152">The appropriate properties should be mapped by default.</span></span>

    ![Сопоставление](~/ef6/media/mapping.png)

-   <span data-ttu-id="cc36a-154">Выберите **PersonHireInfo** ассоциации в рабочей области конструирования.</span><span class="sxs-lookup"><span data-stu-id="cc36a-154">Select the **PersonHireInfo** association on the design surface.</span></span>
-   <span data-ttu-id="cc36a-155">Щелкните правой кнопкой мыши связь на область конструктора и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="cc36a-155">Right-click the association on the design surface and select **Properties**.</span></span>
-   <span data-ttu-id="cc36a-156">В **свойства** выберите **ссылочные ограничения** свойство и нажмите кнопку с многоточием.</span><span class="sxs-lookup"><span data-stu-id="cc36a-156">In the **Properties** window, select the **Referential Constraints** property and click the ellipses button.</span></span>
-   <span data-ttu-id="cc36a-157">Выберите **Person** из **участника** стрелку раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="cc36a-157">Select **Person** from the **Principal** drop-down list.</span></span>
-   <span data-ttu-id="cc36a-158">Нажмите клавишу **ОК**.</span><span class="sxs-lookup"><span data-stu-id="cc36a-158">Press **OK**.</span></span>

 

## <a name="use-the-model"></a><span data-ttu-id="cc36a-159">Использование модели</span><span class="sxs-lookup"><span data-stu-id="cc36a-159">Use the Model</span></span>

-   <span data-ttu-id="cc36a-160">Вставьте следующий код в методе Main.</span><span class="sxs-lookup"><span data-stu-id="cc36a-160">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Person person = new Person()
        {
            FirstName = "Kimberly",
            LastName = "Morgan",
            Discriminator = "Instructor",
        };

        person.HireInfo = new HireInfo()
        {
            HireDate = DateTime.Now
        };

        // Add the new person to the context.
        context.People.Add(person);

        // Insert a row into the Person table.  
        context.SaveChanges();

        // Execute a query against the Person table.
        // The query returns columns that map to the Person entity.
        var existingPerson = context.People.FirstOrDefault();

        // Execute a query against the Person table.
        // The query returns columns that map to the Instructor entity.
        var hireInfo = existingPerson.HireInfo;

        Console.WriteLine("{0} was hired on {1}",
            existingPerson.LastName, hireInfo.HireDate);
    }
```
-   <span data-ttu-id="cc36a-161">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="cc36a-161">Compile and run the application.</span></span>

<span data-ttu-id="cc36a-162">Следующие инструкции T-SQL были выполнены с **School** базы данных в результате запуска этого приложения.</span><span class="sxs-lookup"><span data-stu-id="cc36a-162">The following T-SQL statements were executed against the **School** database as a result of running this application.</span></span> 

-   <span data-ttu-id="cc36a-163">Следующие **вставить** был выполнен в результате выполнения контекста. SaveChanges() и объединяет данные из **Person** и **HireInfo** сущностей</span><span class="sxs-lookup"><span data-stu-id="cc36a-163">The following **INSERT** was executed as a result of executing context.SaveChanges() and combines data from the **Person** and **HireInfo** entities</span></span>

    ![Insert](~/ef6/media/insert.png)

-   <span data-ttu-id="cc36a-165">Следующие **ВЫБЕРИТЕ** был выполнен в результате выполнения контекста. People.FirstOrDefault() и выбирает только столбцы сопоставляются **Person**</span><span class="sxs-lookup"><span data-stu-id="cc36a-165">The following **SELECT** was executed as a result of executing context.People.FirstOrDefault() and selects just the columns mapped to **Person**</span></span>

    ![Select1](~/ef6/media/select1.png)

-   <span data-ttu-id="cc36a-167">Следующие **ВЫБЕРИТЕ** была выполнена из-за доступа к existingPerson.Instructor свойство навигации и выбирает только нужные столбцы, которые сопоставляются с **HireInfo**</span><span class="sxs-lookup"><span data-stu-id="cc36a-167">The following **SELECT** was executed as a result of accessing the navigation property existingPerson.Instructor and selects just the columns mapped to **HireInfo**</span></span>

    ![Select2](~/ef6/media/select2.png)
