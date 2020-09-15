---
title: Разделение таблиц конструктора — EF6
description: Разделение таблиц конструктора в Entity Framework 6
author: divega
ms.date: 10/23/2016
uid: ef6/modeling/designer/table-splitting
ms.openlocfilehash: 6fdb5050ab4d3860184f19ea056a0f2257e20a3c
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90073114"
---
# <a name="designer-table-splitting"></a><span data-ttu-id="701fa-103">Разделение таблиц конструктора</span><span class="sxs-lookup"><span data-stu-id="701fa-103">Designer Table Splitting</span></span>
<span data-ttu-id="701fa-104">В этом пошаговом руководстве показано, как сопоставлять несколько типов сущностей с одной таблицей путем изменения модели с помощью Entity Framework Designer (конструктор EF).</span><span class="sxs-lookup"><span data-stu-id="701fa-104">This walkthrough shows how to map multiple entity types to a single table by modifying a model with the Entity Framework Designer (EF Designer).</span></span>

<span data-ttu-id="701fa-105">Одной из причин, по которой можно использовать разделение таблиц, является задержка загрузки некоторых свойств при использовании отложенной загрузки для загрузки объектов.</span><span class="sxs-lookup"><span data-stu-id="701fa-105">One reason you may want to use table splitting is delaying the loading of some properties when using lazy loading to load your objects.</span></span><span data-ttu-id="701fa-106">Можно разделять свойства, которые могут содержать очень большой объем данных, в отдельную сущность и загружать их только при необходимости.</span><span class="sxs-lookup"><span data-stu-id="701fa-106"> You can separate the properties that might contain very large amount of data into a separate entity and only load it when required.</span></span>

<span data-ttu-id="701fa-107">На следующем рисунке показаны основные окна, используемые при работе с конструктором EF.</span><span class="sxs-lookup"><span data-stu-id="701fa-107">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![Конструктор EF](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="701fa-109">Предварительные условия</span><span class="sxs-lookup"><span data-stu-id="701fa-109">Prerequisites</span></span>

<span data-ttu-id="701fa-110">Для выполнения данного пошагового руководства требуется:</span><span class="sxs-lookup"><span data-stu-id="701fa-110">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="701fa-111">Последняя версия Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="701fa-111">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="701fa-112">[Образец базы данных School](xref:ef6/resources/school-database).</span><span class="sxs-lookup"><span data-stu-id="701fa-112">The [School sample database](xref:ef6/resources/school-database).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="701fa-113">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="701fa-113">Set up the Project</span></span>

<span data-ttu-id="701fa-114">В этом пошаговом руководстве используется Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="701fa-114">This walkthrough is using Visual Studio 2012.</span></span>

-   <span data-ttu-id="701fa-115">Откройте Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="701fa-115">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="701fa-116">В меню **Файл** укажите **Создать**, затем нажмите **Проект**.</span><span class="sxs-lookup"><span data-stu-id="701fa-116">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="701fa-117">В левой области щелкните Visual C \# , а затем выберите шаблон Консольное приложение.</span><span class="sxs-lookup"><span data-stu-id="701fa-117">In the left pane, click Visual C\#, and then select the Console Application template.</span></span>
-   <span data-ttu-id="701fa-118">Введите **таблесплиттингсампле** в качестве имени проекта и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="701fa-118">Enter **TableSplittingSample** as the name of the project and click **OK**.</span></span>

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="701fa-119">Создание модели на основе базы данных School</span><span class="sxs-lookup"><span data-stu-id="701fa-119">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="701fa-120">Щелкните правой кнопкой мыши имя проекта в обозреватель решений, наведите указатель на пункт **Добавить**и выберите пункт **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="701fa-120">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="701fa-121">Выберите **данные** в меню слева, а затем выберите **ADO.NET EDM** в области Шаблоны.</span><span class="sxs-lookup"><span data-stu-id="701fa-121">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="701fa-122">Введите **таблесплиттингмодел. EDMX** в поле имя файла и нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="701fa-122">Enter **TableSplittingModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="701fa-123">В диалоговом окне Выбор содержимого модели выберите пункт **создать из базы данных**, а затем нажмите кнопку **Далее.**</span><span class="sxs-lookup"><span data-stu-id="701fa-123">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="701fa-124">Нажмите кнопку Создать соединение.</span><span class="sxs-lookup"><span data-stu-id="701fa-124">Click New Connection.</span></span> <span data-ttu-id="701fa-125">В диалоговом окне Свойства соединения введите имя сервера (например, **(LocalDB) \\ mssqllocaldb**), выберите метод проверки подлинности, введите **School**   в качестве имени базы данных и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="701fa-125">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="701fa-126">В диалоговом окне Выбор подключения к данным будет обновлен параметр подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="701fa-126">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="701fa-127">В диалоговом окне Выбор объектов базы данных unfold узел **таблицы**   и проверьте таблицу **Person** .</span><span class="sxs-lookup"><span data-stu-id="701fa-127">In the Choose Your Database Objects dialog box, unfold the **Tables** node and check the **Person** table.</span></span> <span data-ttu-id="701fa-128">Это приведет к добавлению указанной таблицы в модель **School** .</span><span class="sxs-lookup"><span data-stu-id="701fa-128">This will add the specified table to the **School** model.</span></span>
-   <span data-ttu-id="701fa-129">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="701fa-129">Click **Finish**.</span></span>

<span data-ttu-id="701fa-130">Отобразится Entity Designer, предоставляющий область конструктора для редактирования модели.</span><span class="sxs-lookup"><span data-stu-id="701fa-130">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="701fa-131">Все объекты, выбранные в диалоговом окне **Выбор объектов базы данных**   , добавляются в модель.</span><span class="sxs-lookup"><span data-stu-id="701fa-131">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>

## <a name="map-two-entities-to-a-single-table"></a><span data-ttu-id="701fa-132">Сопоставьте две сущности с одной таблицей</span><span class="sxs-lookup"><span data-stu-id="701fa-132">Map Two Entities to a Single Table</span></span>

<span data-ttu-id="701fa-133">В этом разделе сущность **Person** будет разделяться на две сущности, а затем сопоставляться с одной таблицей.</span><span class="sxs-lookup"><span data-stu-id="701fa-133">In this section you will split the **Person** entity into two entities and then map them to a single table.</span></span>

> [!NOTE]
> <span data-ttu-id="701fa-134">Сущность **Person** не содержит свойств, которые могут содержать большой объем данных. Он просто используется в качестве примера.</span><span class="sxs-lookup"><span data-stu-id="701fa-134">The **Person** entity does not contain any properties that may contain large amount of data; it is just used as an example.</span></span>

-   <span data-ttu-id="701fa-135">Щелкните правой кнопкой мыши пустую область области конструктора, наведите указатель на пункт **Добавить новый**и выберите пункт **сущность**.</span><span class="sxs-lookup"><span data-stu-id="701fa-135">Right-click an empty area of the design surface, point to **Add New**, and click **Entity**.</span></span>
    <span data-ttu-id="701fa-136">Откроется диалоговое окно **создать сущность**   .</span><span class="sxs-lookup"><span data-stu-id="701fa-136">The **New Entity** dialog box appears.</span></span>
-   <span data-ttu-id="701fa-137">Введите **хиреинфо**   в качестве **имени сущности** и **PersonID** в качестве имени **ключевого свойства** .</span><span class="sxs-lookup"><span data-stu-id="701fa-137">Type **HireInfo** for the **Entity name** and **PersonID** for the **Key Property** name.</span></span>
-   <span data-ttu-id="701fa-138">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="701fa-138">Click **OK**.</span></span>
-   <span data-ttu-id="701fa-139">Новый тип сущности будет создан и отобразится в области конструктора.</span><span class="sxs-lookup"><span data-stu-id="701fa-139">A new entity type is created and displayed on the design surface.</span></span>
-   <span data-ttu-id="701fa-140">Выберите свойство **HireDate**    **Person**   типа сущности Person и нажмите клавиши **CTRL + X** .</span><span class="sxs-lookup"><span data-stu-id="701fa-140">Select the **HireDate** property of the **Person** entity type and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="701fa-141">Выберите **HireInfo**   сущность хиреинфо и нажмите клавиши **CTRL + V** .</span><span class="sxs-lookup"><span data-stu-id="701fa-141">Select the **HireInfo** entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="701fa-142">Создание связи между **лицом** и **хиреинфо**.</span><span class="sxs-lookup"><span data-stu-id="701fa-142">Create an association between **Person** and **HireInfo**.</span></span> <span data-ttu-id="701fa-143">Для этого щелкните правой кнопкой мыши пустую область в области конструктора, наведите указатель на пункт **Добавить новый**и выберите пункт **Ассоциация**.</span><span class="sxs-lookup"><span data-stu-id="701fa-143">To do this, right-click an empty area of the design surface, point to **Add New**, and click **Association**.</span></span>
-   <span data-ttu-id="701fa-144"> **Add Association**   Откроется диалоговое окно Добавление связи.</span><span class="sxs-lookup"><span data-stu-id="701fa-144">The **Add Association** dialog box appears.</span></span> <span data-ttu-id="701fa-145">Имя **персонхиреинфо** предоставляется по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="701fa-145">The **PersonHireInfo** name is given by default.</span></span>
-   <span data-ttu-id="701fa-146">Укажите кратность **1 (один)** на обоих концах связи.</span><span class="sxs-lookup"><span data-stu-id="701fa-146">Specify multiplicity **1(One)** on both ends of the relationship.</span></span>
-   <span data-ttu-id="701fa-147">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="701fa-147">Press **OK**.</span></span>

<span data-ttu-id="701fa-148">Для следующего шага требуется окно **сведения о сопоставлении**   .</span><span class="sxs-lookup"><span data-stu-id="701fa-148">The next step requires the **Mapping Details** window.</span></span> <span data-ttu-id="701fa-149">Если это окно не отображается, щелкните правой кнопкой мыши область конструктора и выберите пункт **сведения о сопоставлении**.</span><span class="sxs-lookup"><span data-stu-id="701fa-149">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="701fa-150">Выберите **HireInfo**   тип сущности хиреинфо и нажмите кнопку \*\* &lt; Добавить таблицу или представление &gt; \*\*   в окне **сведения о сопоставлении**   .</span><span class="sxs-lookup"><span data-stu-id="701fa-150">Select the **HireInfo** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="701fa-151">Выберите **пользователь** из раскрывающегося списка \*\* &lt; Добавить таблицу &gt; или представление\*\*   .</span><span class="sxs-lookup"><span data-stu-id="701fa-151">Select **Person** from the **&lt;Add a Table or View&gt;** field drop-down list.</span></span> <span data-ttu-id="701fa-152">Список содержит таблицы или представления, к которым можно сопоставить выбранную сущность.</span><span class="sxs-lookup"><span data-stu-id="701fa-152">The list contains tables or views to which the selected entity can be mapped.</span></span>
    <span data-ttu-id="701fa-153">Соответствующие свойства должны быть сопоставлены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="701fa-153">The appropriate properties should be mapped by default.</span></span>

    ![Свойства сопоставления](~/ef6/media/mapping.png)

-   <span data-ttu-id="701fa-155">Выберите ассоциацию **персонхиреинфо** в области конструктора.</span><span class="sxs-lookup"><span data-stu-id="701fa-155">Select the **PersonHireInfo** association on the design surface.</span></span>
-   <span data-ttu-id="701fa-156">Щелкните правой кнопкой мыши ассоциацию в области конструктора и выберите пункт **Свойства**.</span><span class="sxs-lookup"><span data-stu-id="701fa-156">Right-click the association on the design surface and select **Properties**.</span></span>
-   <span data-ttu-id="701fa-157">В окне **Свойства** выберите свойство **ссылочные ограничения** и нажмите кнопку с многоточием.</span><span class="sxs-lookup"><span data-stu-id="701fa-157">In the **Properties** window, select the **Referential Constraints** property and click the ellipses button.</span></span>
-   <span data-ttu-id="701fa-158">Выберите **Person** из раскрывающегося списка **основное** .</span><span class="sxs-lookup"><span data-stu-id="701fa-158">Select **Person** from the **Principal** drop-down list.</span></span>
-   <span data-ttu-id="701fa-159">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="701fa-159">Press **OK**.</span></span>

 

## <a name="use-the-model"></a><span data-ttu-id="701fa-160">Использование модели</span><span class="sxs-lookup"><span data-stu-id="701fa-160">Use the Model</span></span>

-   <span data-ttu-id="701fa-161">Вставьте следующий код в метод Main.</span><span class="sxs-lookup"><span data-stu-id="701fa-161">Paste the following code in the Main method.</span></span>

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
-   <span data-ttu-id="701fa-162">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="701fa-162">Compile and run the application.</span></span>

<span data-ttu-id="701fa-163">Следующие инструкции T-SQL были выполнены в базе данных **School** в результате выполнения этого приложения.</span><span class="sxs-lookup"><span data-stu-id="701fa-163">The following T-SQL statements were executed against the **School** database as a result of running this application.</span></span> 

-   <span data-ttu-id="701fa-164">Следующая **инструкция INSERT** была выполнена в результате выполнения context. SaveChanges () и объединяет данные из сущностей **Person** и **хиреинфо**</span><span class="sxs-lookup"><span data-stu-id="701fa-164">The following **INSERT** was executed as a result of executing context.SaveChanges() and combines data from the **Person** and **HireInfo** entities</span></span>

    ![Вставка объединения данных Person и Хиреинфо](~/ef6/media/insert.png)

-   <span data-ttu-id="701fa-166">Следующий **SELECT** был выполнен в результате выполнения context. People. FirstOrDefault () и выбирает только столбцы, сопоставленные с **лицом**</span><span class="sxs-lookup"><span data-stu-id="701fa-166">The following **SELECT** was executed as a result of executing context.People.FirstOrDefault() and selects just the columns mapped to **Person**</span></span>

    ![Выберите 1.](~/ef6/media/select1.png)

-   <span data-ttu-id="701fa-168">Следующий **SELECT** был выполнен в результате обращения к свойству навигации Ексистингперсон. Instructor и выбирает только столбцы, сопоставленные с **хиреинфо**</span><span class="sxs-lookup"><span data-stu-id="701fa-168">The following **SELECT** was executed as a result of accessing the navigation property existingPerson.Instructor and selects just the columns mapped to **HireInfo**</span></span>

    ![Выберите 2](~/ef6/media/select2.png)
