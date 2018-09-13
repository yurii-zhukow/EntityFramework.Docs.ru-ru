---
title: Конструктора CUD хранимые процедуры — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: 35a00aa817c8643352c517c233977efd49e3baac
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489561"
---
# <a name="designer-cud-stored-procedures"></a><span data-ttu-id="08bac-102">Конструктора CUD хранимых процедур</span><span class="sxs-lookup"><span data-stu-id="08bac-102">Designer CUD Stored Procedures</span></span>
<span data-ttu-id="08bac-103">Это пошаговое руководство показано, как сопоставить создания\\вставки, обновления и удаления (CUD) типа сущности с хранимыми процедурами, с помощью Entity Framework Designer (конструктор EF).</span><span class="sxs-lookup"><span data-stu-id="08bac-103">This step-by-step walkthrough show how to map the create\\insert, update, and delete (CUD) operations of an entity type to stored procedures using the Entity Framework Designer (EF Designer).</span></span>  <span data-ttu-id="08bac-104">По умолчанию Entity Framework автоматически создает инструкции SQL для операций CUD, но также можно сопоставить хранимые процедуры для этих операций.</span><span class="sxs-lookup"><span data-stu-id="08bac-104">By default, the Entity Framework automatically generates the SQL statements for the CUD operations, but you can also map stored procedures to these operations.</span></span>  

<span data-ttu-id="08bac-105">Обратите внимание, что Code First не поддерживает сопоставление хранимых процедур или функций.</span><span class="sxs-lookup"><span data-stu-id="08bac-105">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="08bac-106">Тем не менее можно вызвать хранимые процедуры или функции с помощью метода System.Data.Entity.DbSet.SqlQuery.</span><span class="sxs-lookup"><span data-stu-id="08bac-106">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="08bac-107">Пример:</span><span class="sxs-lookup"><span data-stu-id="08bac-107">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a><span data-ttu-id="08bac-108">Рекомендации при сопоставлении операций CUD для хранимых процедур</span><span class="sxs-lookup"><span data-stu-id="08bac-108">Considerations when Mapping the CUD Operations to Stored Procedures</span></span>

<span data-ttu-id="08bac-109">При сопоставлении операций CUD с хранимыми процедурами, следующие соображения.</span><span class="sxs-lookup"><span data-stu-id="08bac-109">When mapping the CUD operations to stored procedures, the following considerations apply:</span></span> 

-   <span data-ttu-id="08bac-110">Если вы осуществляете сопоставление одной из операций CUD в хранимую процедуру, сопоставьте их все.</span><span class="sxs-lookup"><span data-stu-id="08bac-110">If you are mapping one of the CUD operations to a stored procedure, map all of them.</span></span> <span data-ttu-id="08bac-111">Если не выполнено сопоставление всех трех, то несопоставленные операции завершится ошибкой при выполнении и **UpdateException** будет создано.</span><span class="sxs-lookup"><span data-stu-id="08bac-111">If you do not map all three, the unmapped operations will fail if executed and an **UpdateException** will be thrown.</span></span>
-   <span data-ttu-id="08bac-112">Свойства сущности необходимо сопоставить каждый параметр хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="08bac-112">You must map every parameter of the stored procedure to entity properties.</span></span>
-   <span data-ttu-id="08bac-113">Если сервер создает значение первичного ключа для вставленной строки, необходимо сопоставить это значение обратно к свойству ключа сущности.</span><span class="sxs-lookup"><span data-stu-id="08bac-113">If the server generates the primary key value for the inserted row, you must map this value back to the entity's key property.</span></span> <span data-ttu-id="08bac-114">В следующем примере **InsertPerson** хранимая процедура возвращает вновь созданный первичный ключ как часть результирующего набора хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="08bac-114">In the example that follows, the **InsertPerson** stored procedure returns the newly created primary key as part of the stored procedure's result set.</span></span> <span data-ttu-id="08bac-115">Первичный ключ сопоставляется с ключом сущности (**PersonID**) с помощью **&lt;Добавление привязок результатов&gt;** функции конструктора Entity FRAMEWORK.</span><span class="sxs-lookup"><span data-stu-id="08bac-115">The primary key is mapped to the entity key (**PersonID**) using the **&lt;Add Result Bindings&gt;** feature of the EF Designer.</span></span>
-   <span data-ttu-id="08bac-116">Вызовы хранимых процедур, сопоставленных 1:1 с сущностями в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="08bac-116">The stored procedure calls are mapped 1:1 with the entities in the conceptual model.</span></span> <span data-ttu-id="08bac-117">Например, если вы реализуете иерархии наследования в концептуальной модели и затем карты CUD хранимых процедур для **родительского** (базовый) и **дочерних** (производные) сущности, сохранение **Дочерних** изменений будет вызывать только **дочерних**хранимые процедуры, она не сможет запустить **родительского**хранимые процедуры вызовов.</span><span class="sxs-lookup"><span data-stu-id="08bac-117">For example, if you implement an inheritance hierarchy in your conceptual model and then map the CUD stored procedures for the **Parent** (base) and the **Child** (derived) entities, saving the **Child** changes will only call the **Child**’s stored procedures, it will not trigger the **Parent**’s stored procedures calls.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="08bac-118">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="08bac-118">Prerequisites</span></span>

<span data-ttu-id="08bac-119">Для выполнения данного пошагового руководства требуется:</span><span class="sxs-lookup"><span data-stu-id="08bac-119">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="08bac-120">Последнюю версию Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="08bac-120">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="08bac-121">[Образца базы данных School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="08bac-121">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="08bac-122">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="08bac-122">Set up the Project</span></span>

-   <span data-ttu-id="08bac-123">Откройте Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="08bac-123">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="08bac-124">Выберите **файл -&gt; Новинка —&gt; проекта**</span><span class="sxs-lookup"><span data-stu-id="08bac-124">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="08bac-125">В левой области щелкните **Visual C\#**, а затем выберите **консоли** шаблона.</span><span class="sxs-lookup"><span data-stu-id="08bac-125">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="08bac-126">Введите **CUDSProcsSample** как имя.</span><span class="sxs-lookup"><span data-stu-id="08bac-126">Enter **CUDSProcsSample** as the name.</span></span>
-   <span data-ttu-id="08bac-127">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="08bac-127">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="08bac-128">Создание модели</span><span class="sxs-lookup"><span data-stu-id="08bac-128">Create a Model</span></span>

-   <span data-ttu-id="08bac-129">Щелкните правой кнопкой мыши имя проекта в обозревателе решений и выберите **Add -&gt; новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="08bac-129">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="08bac-130">Выберите **данных** меню слева, а затем выберите **ADO.NET Entity Data Model** в области шаблонов.</span><span class="sxs-lookup"><span data-stu-id="08bac-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="08bac-131">Введите **CUDSProcs.edmx** имя файла, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="08bac-131">Enter **CUDSProcs.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="08bac-132">В диалоговом окне Выбор содержимого модели выберите **создать из базы данных**, а затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="08bac-132">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="08bac-133">Нажмите кнопку **новое подключение**.</span><span class="sxs-lookup"><span data-stu-id="08bac-133">Click **New Connection**.</span></span> <span data-ttu-id="08bac-134">В диалоговом окне Свойства соединения, введите имя сервера (например, **(localdb)\\mssqllocaldb**) выберите метод проверки подлинности, тип **School** для имени базы данных, а затем Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="08bac-134">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="08bac-135">В диалоговом окне Выбор подключения к базе данных обновляется параметр подключения базы данных.</span><span class="sxs-lookup"><span data-stu-id="08bac-135">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="08bac-136">В поле выберите ваши объекты базы данных диалогового **таблиц** выберите **Person** таблицы.</span><span class="sxs-lookup"><span data-stu-id="08bac-136">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person** table.</span></span>
-   <span data-ttu-id="08bac-137">Кроме того, выберите следующие хранимые процедуры в разделе **хранимые процедуры и функции** узла: **DeletePerson**, **InsertPerson**, и **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="08bac-137">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **DeletePerson**, **InsertPerson**, and **UpdatePerson**.</span></span> 
-   <span data-ttu-id="08bac-138">Начиная с Visual Studio 2012 конструкторе EF поддерживает массовый импорт хранимых процедур.</span><span class="sxs-lookup"><span data-stu-id="08bac-138">Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures.</span></span> <span data-ttu-id="08bac-139">**Импортировать выбранные хранимые процедуры и функции в модели сущности** установлен по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="08bac-139">The **Import selected stored procedures and functions into the entity model** is checked by default.</span></span> <span data-ttu-id="08bac-140">Так как в этом примере мы сохранили процедуры, вставки, обновления и удаления типов сущностей, мы не требуется импортировать их и будет снимите этот флажок.</span><span class="sxs-lookup"><span data-stu-id="08bac-140">Since in this example we have stored procedures that insert, update, and delete entity types, we do not want to import them and will uncheck this checkbox.</span></span> 

    ![Импорт S Procs](~/ef6/media/importsprocs.jpg)

-   <span data-ttu-id="08bac-142">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="08bac-142">Click **Finish**.</span></span>
    <span data-ttu-id="08bac-143">Конструктор EF, который предоставляет область конструктора для изменения модели, отображается.</span><span class="sxs-lookup"><span data-stu-id="08bac-143">The EF Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-the-person-entity-to-stored-procedures"></a><span data-ttu-id="08bac-144">Сопоставление сущности Person с хранимыми процедурами</span><span class="sxs-lookup"><span data-stu-id="08bac-144">Map the Person Entity to Stored Procedures</span></span>

-   <span data-ttu-id="08bac-145">Щелкните правой кнопкой мыши **Person** типа сущности и выберите **сопоставление хранимых процедур**.</span><span class="sxs-lookup"><span data-stu-id="08bac-145">Right-click the **Person** entity type and select **Stored Procedure Mapping**.</span></span>
-   <span data-ttu-id="08bac-146">Сопоставления хранимых процедур, появятся в **сведения о сопоставлении** окна.</span><span class="sxs-lookup"><span data-stu-id="08bac-146">The stored procedure mappings appear in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="08bac-147">Нажмите кнопку  **&lt;функция Insert Select&gt;**.</span><span class="sxs-lookup"><span data-stu-id="08bac-147">Click **&lt;Select Insert Function&gt;**.</span></span>
    <span data-ttu-id="08bac-148">Поле становится раскрывающимся списком хранимых процедур в модели хранения, которые могут быть сопоставлены с типами сущностей в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="08bac-148">The field becomes a drop-down list of the stored procedures in the storage model that can be mapped to entity types in the conceptual model.</span></span>
    <span data-ttu-id="08bac-149">Выберите **InsertPerson** из раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="08bac-149">Select **InsertPerson** from the drop-down list.</span></span>
-   <span data-ttu-id="08bac-150">Появятся применяемые по умолчанию сопоставления параметров хранимой процедуры со свойствами сущности.</span><span class="sxs-lookup"><span data-stu-id="08bac-150">Default mappings between stored procedure parameters and entity properties appear.</span></span> <span data-ttu-id="08bac-151">Обратите внимание, что стрелки указывают направление сопоставления: «Значения свойств передаются параметрам хранимой процедуры».</span><span class="sxs-lookup"><span data-stu-id="08bac-151">Note that arrows indicate the mapping direction: Property values are supplied to stored procedure parameters.</span></span>
-   <span data-ttu-id="08bac-152">Нажмите кнопку  **&lt;Добавление привязки к результату&gt;**.</span><span class="sxs-lookup"><span data-stu-id="08bac-152">Click **&lt;Add Result Binding&gt;**.</span></span>
-   <span data-ttu-id="08bac-153">Тип **NewPersonID**, имя параметра, возвращенного **InsertPerson** хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="08bac-153">Type **NewPersonID**, the name of the parameter returned by the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="08bac-154">Убедитесь, что не введите начальные или конечные пробелы.</span><span class="sxs-lookup"><span data-stu-id="08bac-154">Make sure not to type leading or trailing spaces.</span></span>
-   <span data-ttu-id="08bac-155">Нажмите клавишу **ВВОД**.</span><span class="sxs-lookup"><span data-stu-id="08bac-155">Press **Enter**.</span></span>
-   <span data-ttu-id="08bac-156">По умолчанию **NewPersonID** сопоставляется с ключом сущности **PersonID**.</span><span class="sxs-lookup"><span data-stu-id="08bac-156">By default, **NewPersonID** is mapped to the entity key **PersonID**.</span></span> <span data-ttu-id="08bac-157">Обратите внимание, что стрелка указывает направление сопоставления — значение столбца результата передается свойству.</span><span class="sxs-lookup"><span data-stu-id="08bac-157">Note that an arrow indicates the direction of the mapping: The value of the result column is supplied to the property.</span></span>

    ![Сведения о сопоставлении](~/ef6/media/mappingdetails.png)

-   <span data-ttu-id="08bac-159">Нажмите кнопку **&lt;Выбор функции обновления&gt;** и выберите **UpdatePerson** из результирующего раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="08bac-159">Click **&lt;Select Update Function&gt;** and select **UpdatePerson** from the resulting drop-down list.</span></span>
-   <span data-ttu-id="08bac-160">Появятся применяемые по умолчанию сопоставления параметров хранимой процедуры со свойствами сущности.</span><span class="sxs-lookup"><span data-stu-id="08bac-160">Default mappings between stored procedure parameters and entity properties appear.</span></span>
-   <span data-ttu-id="08bac-161">Нажмите кнопку **&lt;Выбор функции удаления&gt;** и выберите **DeletePerson** из результирующего раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="08bac-161">Click **&lt;Select Delete Function&gt;** and select **DeletePerson** from the resulting drop-down list.</span></span>
-   <span data-ttu-id="08bac-162">Появятся применяемые по умолчанию сопоставления параметров хранимой процедуры со свойствами сущности.</span><span class="sxs-lookup"><span data-stu-id="08bac-162">Default mappings between stored procedure parameters and entity properties appear.</span></span>

<span data-ttu-id="08bac-163">Вставки, обновления и удаления операций **Person** типа сущности теперь сопоставлены с хранимыми процедурами.</span><span class="sxs-lookup"><span data-stu-id="08bac-163">The insert, update, and delete operations of the **Person** entity type are now mapped to stored procedures.</span></span>

<span data-ttu-id="08bac-164">Если вы хотите включить проверку при обновлении или удалении сущности с хранимыми процедурами параллелизма, используйте один из следующих вариантов:</span><span class="sxs-lookup"><span data-stu-id="08bac-164">If you want to enable concurrency checking when updating or deleting an entity with stored procedures, use one of the following options:</span></span>

-   <span data-ttu-id="08bac-165">Используйте **ВЫВОДА** параметр для возврата числа обработанных строк из хранимой процедуры и проверка **параметр числа затронутых строк** флажок рядом с именем параметра.</span><span class="sxs-lookup"><span data-stu-id="08bac-165">Use an **OUTPUT** parameter to return the number of affected rows from the stored procedure and check the **Rows Affected Parameter** checkbox next to the parameter name.</span></span> <span data-ttu-id="08bac-166">Если возвращенное значение равно нулю, при вызове операции [ **OptimisticConcurrencyException** ](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) будет создано.</span><span class="sxs-lookup"><span data-stu-id="08bac-166">If the value returned is zero when the operation is called, an  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will be thrown.</span></span>
-   <span data-ttu-id="08bac-167">Проверьте **использовать исходное значение** флажок рядом со свойством, которое вы хотите использовать для проверки параллелизма.</span><span class="sxs-lookup"><span data-stu-id="08bac-167">Check the **Use Original Value** checkbox next to a property that you want to use for concurrency checking.</span></span> <span data-ttu-id="08bac-168">При попытке обновления значение свойства, которое было первоначально считано из базы данных будет использоваться при записи данных обратно в базу данных.</span><span class="sxs-lookup"><span data-stu-id="08bac-168">When an update is attempted, the value of the property that was originally read from the database will be used when writing data back to the database.</span></span> <span data-ttu-id="08bac-169">Если значение не соответствует значению в базе данных, **OptimisticConcurrencyException** будет создано.</span><span class="sxs-lookup"><span data-stu-id="08bac-169">If the value does not match the value in the database, an **OptimisticConcurrencyException** will be thrown.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="08bac-170">Использование модели</span><span class="sxs-lookup"><span data-stu-id="08bac-170">Use the Model</span></span>

<span data-ttu-id="08bac-171">Откройте **Program.cs** файл, в котором **Main** определен метод.</span><span class="sxs-lookup"><span data-stu-id="08bac-171">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="08bac-172">Добавьте следующий код в функцию Main.</span><span class="sxs-lookup"><span data-stu-id="08bac-172">Add the following code into the Main function.</span></span>

<span data-ttu-id="08bac-173">Код создает новый **Person** объекта, затем обновляет объект и наконец удаляет объект.</span><span class="sxs-lookup"><span data-stu-id="08bac-173">The code creates a new **Person** object, then updates the object, and finally deletes the object.</span></span>         

``` csharp
    using (var context = new SchoolEntities())
    {
        var newInstructor = new Person
        {
            FirstName = "Robyn",
            LastName = "Martin",
            HireDate = DateTime.Now,
            Discriminator = "Instructor"
        }

        // Add the new object to the context.
        context.People.Add(newInstructor);

        Console.WriteLine("Added {0} {1} to the context.",
            newInstructor.FirstName, newInstructor.LastName);

        Console.WriteLine("Before SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // SaveChanges will call the InsertPerson sproc.  
        // The PersonID property will be assigned the value
        // returned by the sproc.
        context.SaveChanges();

        Console.WriteLine("After SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // Modify the object and call SaveChanges.
        // This time, the UpdatePerson will be called.
        newInstructor.FirstName = "Rachel";
        context.SaveChanges();

        // Remove the object from the context and call SaveChanges.
        // The DeletePerson sproc will be called.
        context.People.Remove(newInstructor);
        context.SaveChanges();

        Person deletedInstructor = context.People.
            Where(p => p.PersonID == newInstructor.PersonID).
            FirstOrDefault();

        if (deletedInstructor == null)
            Console.WriteLine("A person with PersonID {0} was deleted.",
                newInstructor.PersonID);
    }
```

-   <span data-ttu-id="08bac-174">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="08bac-174">Compile and run the application.</span></span> <span data-ttu-id="08bac-175">Программа создает следующие выходные данные \*</span><span class="sxs-lookup"><span data-stu-id="08bac-175">The program produces the following output \*</span></span>
    >[!NOTE]
> <span data-ttu-id="08bac-176">PersonID формируется автоматически сервером, поэтому вы скорее всего увидите другой номер \*</span><span class="sxs-lookup"><span data-stu-id="08bac-176">PersonID is auto-generated by the server, so you will most likely see a different number\*</span></span>

```
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

<span data-ttu-id="08bac-177">Если вы работаете с версией Visual Studio Ultimate, Intellitrace можно использовать с помощью отладчика для просмотра инструкций SQL, которые выполняются.</span><span class="sxs-lookup"><span data-stu-id="08bac-177">If you are working with the Ultimate version of Visual Studio, you can use Intellitrace with the debugger to see the SQL statements that get executed.</span></span>

![IntelliTrace](~/ef6/media/intellitrace.png)
