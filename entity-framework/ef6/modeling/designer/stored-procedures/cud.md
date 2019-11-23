---
title: Конструктор-хранимые процедуры CUD — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: bdb0df969c33d5ad3f103bfa9af6002c9c2bb9b3
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813559"
---
# <a name="designer-cud-stored-procedures"></a><span data-ttu-id="8549b-102">Конструктор хранимых процедур CUD</span><span class="sxs-lookup"><span data-stu-id="8549b-102">Designer CUD Stored Procedures</span></span>

<span data-ttu-id="8549b-103">В этом пошаговом руководстве показано, как сопоставлять операции создания\\вставки, обновления и удаления (CUD) типа сущности с хранимыми процедурами с помощью Entity Framework Designer (EF Designer).</span><span class="sxs-lookup"><span data-stu-id="8549b-103">This step-by-step walkthrough show how to map the create\\insert, update, and delete (CUD) operations of an entity type to stored procedures using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="8549b-104"> По умолчанию Entity Framework автоматически создает инструкции SQL для операций CUD, но можно также сопоставлять хранимые процедуры с этими операциями.</span><span class="sxs-lookup"><span data-stu-id="8549b-104"> By default, the Entity Framework automatically generates the SQL statements for the CUD operations, but you can also map stored procedures to these operations.</span></span>  

<span data-ttu-id="8549b-105">Обратите внимание, что Code First не поддерживает сопоставление с хранимыми процедурами или функциями.</span><span class="sxs-lookup"><span data-stu-id="8549b-105">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="8549b-106">Однако можно вызывать хранимые процедуры или функции с помощью метода System. Data. Entity. DbSet. SqlQuery.</span><span class="sxs-lookup"><span data-stu-id="8549b-106">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="8549b-107">Например:</span><span class="sxs-lookup"><span data-stu-id="8549b-107">For example:</span></span>

``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a><span data-ttu-id="8549b-108">Рекомендации по сопоставлению операций CUD с хранимыми процедурами</span><span class="sxs-lookup"><span data-stu-id="8549b-108">Considerations when Mapping the CUD Operations to Stored Procedures</span></span>

<span data-ttu-id="8549b-109">При сопоставлении операций CUD с хранимыми процедурами применяются следующие соображения.</span><span class="sxs-lookup"><span data-stu-id="8549b-109">When mapping the CUD operations to stored procedures, the following considerations apply:</span></span>

- <span data-ttu-id="8549b-110">При сопоставлении одной из операций CUD с хранимой процедурой сопоставьте все эти операции.</span><span class="sxs-lookup"><span data-stu-id="8549b-110">If you are mapping one of the CUD operations to a stored procedure, map all of them.</span></span> <span data-ttu-id="8549b-111">Если не выполнить сопоставление всех трех, то несопоставленные операции завершатся неудачно, если будет вызвано  **упдатиксцептион** .</span><span class="sxs-lookup"><span data-stu-id="8549b-111">If you do not map all three, the unmapped operations will fail if executed and an **UpdateException** will be thrown.</span></span>
- <span data-ttu-id="8549b-112">Необходимо сопоставлять каждый параметр хранимой процедуры со свойствами сущности.</span><span class="sxs-lookup"><span data-stu-id="8549b-112">You must map every parameter of the stored procedure to entity properties.</span></span>
- <span data-ttu-id="8549b-113">Если сервер создает значение первичного ключа для вставленной строки, необходимо преобразовать это значение обратно в свойство ключа сущности.</span><span class="sxs-lookup"><span data-stu-id="8549b-113">If the server generates the primary key value for the inserted row, you must map this value back to the entity's key property.</span></span> <span data-ttu-id="8549b-114">В следующем примере хранимая процедура **инсертперсон** Возвращает созданный первичный ключ как часть результирующего набора хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="8549b-114">In the example that follows, the **InsertPerson** stored procedure returns the newly created primary key as part of the stored procedure's result set.</span></span> <span data-ttu-id="8549b-115">Первичный ключ сопоставляется с ключом сущности (**PersonID**) с помощью **&lt;Добавление привязок результатов&gt;**  функции конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="8549b-115">The primary key is mapped to the entity key (**PersonID**) using the **&lt;Add Result Bindings&gt;** feature of the EF Designer.</span></span>
- <span data-ttu-id="8549b-116">Вызовы хранимых процедур сопоставляются с сущностями в концептуальной модели 1:1.</span><span class="sxs-lookup"><span data-stu-id="8549b-116">The stored procedure calls are mapped 1:1 with the entities in the conceptual model.</span></span> <span data-ttu-id="8549b-117">Например, если в концептуальной модели реализуется иерархия наследования, а затем сопоставляются хранимые процедуры CUD **для родительских** (базовых) **и дочерних** (производных) сущностей, сохранение **дочерних** изменений приведет к вызову только хранимых процедур **дочернего**класса, а вызовы хранимых процедур **родительского**объекта не будут вызываться.</span><span class="sxs-lookup"><span data-stu-id="8549b-117">For example, if you implement an inheritance hierarchy in your conceptual model and then map the CUD stored procedures for the **Parent** (base) and the **Child** (derived) entities, saving the **Child** changes will only call the **Child**’s stored procedures, it will not trigger the **Parent**’s stored procedures calls.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8549b-118">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="8549b-118">Prerequisites</span></span>

<span data-ttu-id="8549b-119">Для выполнения данного пошагового руководства требуется:</span><span class="sxs-lookup"><span data-stu-id="8549b-119">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="8549b-120">Последняя версия Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8549b-120">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="8549b-121">[Образец базы данных School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="8549b-121">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="8549b-122">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="8549b-122">Set up the Project</span></span>

- <span data-ttu-id="8549b-123">Откройте Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="8549b-123">Open Visual Studio 2012.</span></span>
- <span data-ttu-id="8549b-124">Выберите **файл —&gt; создать&gt; проект**</span><span class="sxs-lookup"><span data-stu-id="8549b-124">Select **File-&gt; New -&gt; Project**</span></span>
- <span data-ttu-id="8549b-125">В левой области щелкните **Visual C\#** , а затем выберите шаблон **консоли** .</span><span class="sxs-lookup"><span data-stu-id="8549b-125">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
- <span data-ttu-id="8549b-126">В качестве имени введите **кудспрокссампле** .</span><span class="sxs-lookup"><span data-stu-id="8549b-126">Enter **CUDSProcsSample** as the name.</span></span>
- <span data-ttu-id="8549b-127">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="8549b-127">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="8549b-128">Создание модели</span><span class="sxs-lookup"><span data-stu-id="8549b-128">Create a Model</span></span>

- <span data-ttu-id="8549b-129">Щелкните правой кнопкой мыши имя проекта в обозреватель решений и выберите **Добавить-&gt; новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="8549b-129">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
- <span data-ttu-id="8549b-130">Выберите **данные** в меню слева, а затем выберите **ADO.NET EDM** в области Шаблоны.</span><span class="sxs-lookup"><span data-stu-id="8549b-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
- <span data-ttu-id="8549b-131">Введите **кудспрокс. EDMX** в поле имя файла и нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="8549b-131">Enter **CUDSProcs.edmx** for the file name, and then click **Add**.</span></span>
- <span data-ttu-id="8549b-132">В диалоговом окне Выбор содержимого модели выберите пункт **создать из базы данных**, а затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="8549b-132">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
- <span data-ttu-id="8549b-133">Нажмите кнопку **создать соединение**.</span><span class="sxs-lookup"><span data-stu-id="8549b-133">Click **New Connection**.</span></span> <span data-ttu-id="8549b-134">В диалоговом окне Свойства соединения введите имя сервера (например, **(LocalDB)\\mssqllocaldb**), выберите метод проверки подлинности, введите **School** в качестве имени базы данных и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="8549b-134">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="8549b-135">В диалоговом окне Выбор подключения к данным будет обновлен параметр подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="8549b-135">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
- <span data-ttu-id="8549b-136">В диалоговом окне Выбор объектов базы данных в разделе **таблицы** узел выберите таблицу **Person** .</span><span class="sxs-lookup"><span data-stu-id="8549b-136">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person** table.</span></span>
- <span data-ttu-id="8549b-137">Кроме того, выберите следующие хранимые процедуры в узле **хранимые процедуры и функции** : **делетеперсон**, **инсертперсон**и **упдатеперсон**.</span><span class="sxs-lookup"><span data-stu-id="8549b-137">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **DeletePerson**, **InsertPerson**, and **UpdatePerson**.</span></span>
- <span data-ttu-id="8549b-138">Начиная с Visual Studio 2012 конструктор EF поддерживает неполное импортирование хранимых процедур.</span><span class="sxs-lookup"><span data-stu-id="8549b-138">Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures.</span></span> <span data-ttu-id="8549b-139">По умолчанию флажок **Импорт выбранных хранимых процедур и функций в сущностную модель** установлен.</span><span class="sxs-lookup"><span data-stu-id="8549b-139">The **Import selected stored procedures and functions into the entity model** is checked by default.</span></span> <span data-ttu-id="8549b-140">Поскольку в этом примере у нас есть хранимые процедуры для вставки, обновления и удаления типов сущностей, мы не хотим импортировать их и снять этот флажок.</span><span class="sxs-lookup"><span data-stu-id="8549b-140">Since in this example we have stored procedures that insert, update, and delete entity types, we do not want to import them and will uncheck this checkbox.</span></span>

    ![Процедуры импорта S](~/ef6/media/importsprocs.jpg)

- <span data-ttu-id="8549b-142">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="8549b-142">Click **Finish**.</span></span>
    <span data-ttu-id="8549b-143">Отобразится конструктор EF, предоставляющий область конструктора для редактирования модели.</span><span class="sxs-lookup"><span data-stu-id="8549b-143">The EF Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-the-person-entity-to-stored-procedures"></a><span data-ttu-id="8549b-144">Сопоставьте сущность Person с хранимыми процедурами</span><span class="sxs-lookup"><span data-stu-id="8549b-144">Map the Person Entity to Stored Procedures</span></span>

- <span data-ttu-id="8549b-145">Щелкните правой кнопкой мыши объект **Person** тип сущности и выберите **сопоставление хранимых процедур**.</span><span class="sxs-lookup"><span data-stu-id="8549b-145">Right-click the **Person** entity type and select **Stored Procedure Mapping**.</span></span>
- <span data-ttu-id="8549b-146">Сопоставления хранимых процедур отображаются в окне **сведения о Сопоставлении** .</span><span class="sxs-lookup"><span data-stu-id="8549b-146">The stored procedure mappings appear in the **Mapping Details** window.</span></span>
- <span data-ttu-id="8549b-147">Нажмите кнопку **&lt;выберите Вставить функцию&gt;** .</span><span class="sxs-lookup"><span data-stu-id="8549b-147">Click **&lt;Select Insert Function&gt;**.</span></span>
    <span data-ttu-id="8549b-148">Поле становится раскрывающимся списком хранимых процедур в модели хранения, которые могут быть сопоставлены с типами сущностей в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="8549b-148">The field becomes a drop-down list of the stored procedures in the storage model that can be mapped to entity types in the conceptual model.</span></span>
    <span data-ttu-id="8549b-149">Выберите **инсертперсон** из раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="8549b-149">Select **InsertPerson** from the drop-down list.</span></span>
- <span data-ttu-id="8549b-150">Появятся применяемые по умолчанию сопоставления параметров хранимой процедуры со свойствами сущности.</span><span class="sxs-lookup"><span data-stu-id="8549b-150">Default mappings between stored procedure parameters and entity properties appear.</span></span> <span data-ttu-id="8549b-151">Обратите внимание, что стрелки указывают направление сопоставления: «Значения свойств передаются параметрам хранимой процедуры».</span><span class="sxs-lookup"><span data-stu-id="8549b-151">Note that arrows indicate the mapping direction: Property values are supplied to stored procedure parameters.</span></span>
- <span data-ttu-id="8549b-152">Щелкните **&lt;добавить привязку результата&gt;** .</span><span class="sxs-lookup"><span data-stu-id="8549b-152">Click **&lt;Add Result Binding&gt;**.</span></span>
- <span data-ttu-id="8549b-153">Введите **невперсонид**, имя параметра, возвращаемое хранимой процедурой **инсертперсон** .</span><span class="sxs-lookup"><span data-stu-id="8549b-153">Type **NewPersonID**, the name of the parameter returned by the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="8549b-154">Убедитесь, что не вводите начальные или конечные пробелы.</span><span class="sxs-lookup"><span data-stu-id="8549b-154">Make sure not to type leading or trailing spaces.</span></span>
- <span data-ttu-id="8549b-155">Нажмите клавишу **Ввод**.</span><span class="sxs-lookup"><span data-stu-id="8549b-155">Press **Enter**.</span></span>
- <span data-ttu-id="8549b-156">По умолчанию **невперсонид** сопоставляется с ключом сущности **PersonID**.</span><span class="sxs-lookup"><span data-stu-id="8549b-156">By default, **NewPersonID** is mapped to the entity key **PersonID**.</span></span> <span data-ttu-id="8549b-157">Обратите внимание, что стрелка указывает направление сопоставления — значение столбца результата передается свойству.</span><span class="sxs-lookup"><span data-stu-id="8549b-157">Note that an arrow indicates the direction of the mapping: The value of the result column is supplied to the property.</span></span>

    ![Сведения о сопоставлении](~/ef6/media/mappingdetails.png)

- <span data-ttu-id="8549b-159">Щелкните **&lt;выберите обновить функцию&gt;**  а затем выберите **упдатеперсон** в открывшемся раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="8549b-159">Click **&lt;Select Update Function&gt;** and select **UpdatePerson** from the resulting drop-down list.</span></span>
- <span data-ttu-id="8549b-160">Появятся применяемые по умолчанию сопоставления параметров хранимой процедуры со свойствами сущности.</span><span class="sxs-lookup"><span data-stu-id="8549b-160">Default mappings between stored procedure parameters and entity properties appear.</span></span>
- <span data-ttu-id="8549b-161">Щелкните **&lt;выберите Удалить функцию&gt;**  а затем выберите **делетеперсон** в открывшемся раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="8549b-161">Click **&lt;Select Delete Function&gt;** and select **DeletePerson** from the resulting drop-down list.</span></span>
- <span data-ttu-id="8549b-162">Появятся применяемые по умолчанию сопоставления параметров хранимой процедуры со свойствами сущности.</span><span class="sxs-lookup"><span data-stu-id="8549b-162">Default mappings between stored procedure parameters and entity properties appear.</span></span>

<span data-ttu-id="8549b-163">Операции вставки, обновления и удаления типа сущности **Person** теперь сопоставлены с хранимыми процедурами.</span><span class="sxs-lookup"><span data-stu-id="8549b-163">The insert, update, and delete operations of the **Person** entity type are now mapped to stored procedures.</span></span>

<span data-ttu-id="8549b-164">Если необходимо включить проверку параллелизма при обновлении или удалении сущности с хранимыми процедурами, используйте один из следующих вариантов.</span><span class="sxs-lookup"><span data-stu-id="8549b-164">If you want to enable concurrency checking when updating or deleting an entity with stored procedures, use one of the following options:</span></span>

- <span data-ttu-id="8549b-165">Используйте **выходной** параметр, чтобы получить количество затронутых строк из хранимой процедуры и установить флажок **затронутые строки** флажок рядом с именем параметра.</span><span class="sxs-lookup"><span data-stu-id="8549b-165">Use an **OUTPUT** parameter to return the number of affected rows from the stored procedure and check the **Rows Affected Parameter** checkbox next to the parameter name.</span></span> <span data-ttu-id="8549b-166">Если возвращаемое значение равно нулю при вызове операции, будет выдано исключение  [**оптимистикконкурренциексцептион**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) .</span><span class="sxs-lookup"><span data-stu-id="8549b-166">If the value returned is zero when the operation is called, an  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will be thrown.</span></span>
- <span data-ttu-id="8549b-167">Установите флажок **использовать исходное значение** рядом со свойством, которое необходимо использовать для проверки параллелизма.</span><span class="sxs-lookup"><span data-stu-id="8549b-167">Check the **Use Original Value** checkbox next to a property that you want to use for concurrency checking.</span></span> <span data-ttu-id="8549b-168">При попытке обновления значение свойства, которое было первоначально считано из базы данных, будет использоваться при записи данных обратно в базу данных.</span><span class="sxs-lookup"><span data-stu-id="8549b-168">When an update is attempted, the value of the property that was originally read from the database will be used when writing data back to the database.</span></span> <span data-ttu-id="8549b-169">Если значение не совпадает со значением в базе данных, будет выдано **оптимистикконкурренциексцептион** .</span><span class="sxs-lookup"><span data-stu-id="8549b-169">If the value does not match the value in the database, an **OptimisticConcurrencyException** will be thrown.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="8549b-170">Использование модели</span><span class="sxs-lookup"><span data-stu-id="8549b-170">Use the Model</span></span>

<span data-ttu-id="8549b-171">Откройте файл **Program.CS** , в котором определен метод **Main** .</span><span class="sxs-lookup"><span data-stu-id="8549b-171">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="8549b-172">Добавьте следующий код в функцию main.</span><span class="sxs-lookup"><span data-stu-id="8549b-172">Add the following code into the Main function.</span></span>

<span data-ttu-id="8549b-173">Код создает новый объект **Person** , затем обновляет объект и, наконец, удаляет объект.</span><span class="sxs-lookup"><span data-stu-id="8549b-173">The code creates a new **Person** object, then updates the object, and finally deletes the object.</span></span>

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

- <span data-ttu-id="8549b-174">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="8549b-174">Compile and run the application.</span></span> <span data-ttu-id="8549b-175">Программа выдает следующие выходные данные \*</span><span class="sxs-lookup"><span data-stu-id="8549b-175">The program produces the following output \*</span></span>

> [!NOTE]
> <span data-ttu-id="8549b-176">PersonID автоматически создается сервером, поэтому, скорее всего, будет отображаться другое число \*</span><span class="sxs-lookup"><span data-stu-id="8549b-176">PersonID is auto-generated by the server, so you will most likely see a different number\*</span></span>

``` Output
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

<span data-ttu-id="8549b-177">При работе с конечной версией Visual Studio можно использовать IntelliTrace с отладчиком для просмотра инструкций SQL, которые будут выполнены.</span><span class="sxs-lookup"><span data-stu-id="8549b-177">If you are working with the Ultimate version of Visual Studio, you can use Intellitrace with the debugger to see the SQL statements that get executed.</span></span>

![IntelliTrace](~/ef6/media/intellitrace.png)
