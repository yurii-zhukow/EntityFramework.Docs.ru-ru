---
title: Конструктор-хранимые процедуры CUD — EF6
description: Конструктор CUD хранимые процедуры в Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
uid: ef6/modeling/designer/stored-procedures/cud
ms.openlocfilehash: f722cb3ac0b6ce21e685dbb7bffe571fa7b783d5
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89620375"
---
# <a name="designer-cud-stored-procedures"></a><span data-ttu-id="8dde6-103">Конструктор хранимых процедур CUD</span><span class="sxs-lookup"><span data-stu-id="8dde6-103">Designer CUD Stored Procedures</span></span>

<span data-ttu-id="8dde6-104">В этом пошаговом руководстве показано, как сопоставлять \\ операции создания операций вставки, обновления и удаления (CUD) типа сущности с хранимыми процедурами с помощью Entity Framework Designer (EF Designer).</span><span class="sxs-lookup"><span data-stu-id="8dde6-104">This step-by-step walkthrough show how to map the create\\insert, update, and delete (CUD) operations of an entity type to stored procedures using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="8dde6-105">По умолчанию Entity Framework автоматически создает инструкции SQL для операций CUD, но можно также сопоставлять хранимые процедуры с этими операциями.</span><span class="sxs-lookup"><span data-stu-id="8dde6-105"> By default, the Entity Framework automatically generates the SQL statements for the CUD operations, but you can also map stored procedures to these operations.</span></span>  

<span data-ttu-id="8dde6-106">Обратите внимание, что Code First не поддерживает сопоставление с хранимыми процедурами или функциями.</span><span class="sxs-lookup"><span data-stu-id="8dde6-106">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="8dde6-107">Однако можно вызывать хранимые процедуры или функции с помощью метода System. Data. Entity. DbSet. SqlQuery.</span><span class="sxs-lookup"><span data-stu-id="8dde6-107">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="8dde6-108">Пример:</span><span class="sxs-lookup"><span data-stu-id="8dde6-108">For example:</span></span>

``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a><span data-ttu-id="8dde6-109">Рекомендации по сопоставлению операций CUD с хранимыми процедурами</span><span class="sxs-lookup"><span data-stu-id="8dde6-109">Considerations when Mapping the CUD Operations to Stored Procedures</span></span>

<span data-ttu-id="8dde6-110">При сопоставлении операций CUD с хранимыми процедурами применяются следующие соображения.</span><span class="sxs-lookup"><span data-stu-id="8dde6-110">When mapping the CUD operations to stored procedures, the following considerations apply:</span></span>

- <span data-ttu-id="8dde6-111">При сопоставлении одной из операций CUD с хранимой процедурой сопоставьте все эти операции.</span><span class="sxs-lookup"><span data-stu-id="8dde6-111">If you are mapping one of the CUD operations to a stored procedure, map all of them.</span></span> <span data-ttu-id="8dde6-112">Если не выполнить сопоставление всех трех, несопоставленные операции завершатся неудачей, если **UpdateException**   они будут выполнены и будет создано исключение упдатиксцептион.</span><span class="sxs-lookup"><span data-stu-id="8dde6-112">If you do not map all three, the unmapped operations will fail if executed and an **UpdateException** will be thrown.</span></span>
- <span data-ttu-id="8dde6-113">Необходимо сопоставлять каждый параметр хранимой процедуры со свойствами сущности.</span><span class="sxs-lookup"><span data-stu-id="8dde6-113">You must map every parameter of the stored procedure to entity properties.</span></span>
- <span data-ttu-id="8dde6-114">Если сервер создает значение первичного ключа для вставленной строки, необходимо преобразовать это значение обратно в свойство ключа сущности.</span><span class="sxs-lookup"><span data-stu-id="8dde6-114">If the server generates the primary key value for the inserted row, you must map this value back to the entity's key property.</span></span> <span data-ttu-id="8dde6-115">В приведенном ниже примере **InsertPerson**   хранимая процедура инсертперсон Возвращает созданный первичный ключ как часть результирующего набора хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="8dde6-115">In the example that follows, the **InsertPerson** stored procedure returns the newly created primary key as part of the stored procedure's result set.</span></span> <span data-ttu-id="8dde6-116">Первичный ключ сопоставляется с ключом сущности (**PersonID**) с помощью функции \*\* &lt; добавления &gt; результатов привязки\*\*в   конструкторе EF.</span><span class="sxs-lookup"><span data-stu-id="8dde6-116">The primary key is mapped to the entity key (**PersonID**) using the **&lt;Add Result Bindings&gt;** feature of the EF Designer.</span></span>
- <span data-ttu-id="8dde6-117">Вызовы хранимых процедур сопоставляются с сущностями в концептуальной модели 1:1.</span><span class="sxs-lookup"><span data-stu-id="8dde6-117">The stored procedure calls are mapped 1:1 with the entities in the conceptual model.</span></span> <span data-ttu-id="8dde6-118">Например, если в концептуальной модели реализуется иерархия наследования, а затем сопоставляются хранимые процедуры CUD **для родительских** (базовых) **и дочерних** (производных) сущностей, сохранение **дочерних** изменений приведет к вызову только хранимых процедур **дочернего**класса, а вызовы хранимых процедур **родительского**объекта не будут вызываться.</span><span class="sxs-lookup"><span data-stu-id="8dde6-118">For example, if you implement an inheritance hierarchy in your conceptual model and then map the CUD stored procedures for the **Parent** (base) and the **Child** (derived) entities, saving the **Child** changes will only call the **Child**’s stored procedures, it will not trigger the **Parent**’s stored procedures calls.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8dde6-119">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="8dde6-119">Prerequisites</span></span>

<span data-ttu-id="8dde6-120">Для выполнения данного пошагового руководства требуется:</span><span class="sxs-lookup"><span data-stu-id="8dde6-120">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="8dde6-121">Последняя версия Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8dde6-121">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="8dde6-122">[Образец базы данных School](xref:ef6/resources/school-database).</span><span class="sxs-lookup"><span data-stu-id="8dde6-122">The [School sample database](xref:ef6/resources/school-database).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="8dde6-123">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="8dde6-123">Set up the Project</span></span>

- <span data-ttu-id="8dde6-124">Откройте Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="8dde6-124">Open Visual Studio 2012.</span></span>
- <span data-ttu-id="8dde6-125">Выбор **файла- &gt; создать- &gt; проект**</span><span class="sxs-lookup"><span data-stu-id="8dde6-125">Select **File-&gt; New -&gt; Project**</span></span>
- <span data-ttu-id="8dde6-126">В левой области щелкните \*\*Visual C \# \*\*, а затем выберите шаблон **консоли** .</span><span class="sxs-lookup"><span data-stu-id="8dde6-126">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
- <span data-ttu-id="8dde6-127">Введите **кудспрокссампле**в   качестве имени.</span><span class="sxs-lookup"><span data-stu-id="8dde6-127">Enter **CUDSProcsSample** as the name.</span></span>
- <span data-ttu-id="8dde6-128">Выберите **OK**.</span><span class="sxs-lookup"><span data-stu-id="8dde6-128">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="8dde6-129">Создание модели</span><span class="sxs-lookup"><span data-stu-id="8dde6-129">Create a Model</span></span>

- <span data-ttu-id="8dde6-130">Щелкните правой кнопкой мыши имя проекта в обозреватель решений и выберите **Добавить- &gt; новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="8dde6-130">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
- <span data-ttu-id="8dde6-131">Выберите **данные** в меню слева, а затем выберите **ADO.NET EDM** в области Шаблоны.</span><span class="sxs-lookup"><span data-stu-id="8dde6-131">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
- <span data-ttu-id="8dde6-132">Введите **кудспрокс. EDMX** в поле имя файла и нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="8dde6-132">Enter **CUDSProcs.edmx** for the file name, and then click **Add**.</span></span>
- <span data-ttu-id="8dde6-133">В диалоговом окне Выбор содержимого модели выберите пункт **создать из базы данных**, а затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="8dde6-133">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
- <span data-ttu-id="8dde6-134">Нажмите кнопку **создать соединение**.</span><span class="sxs-lookup"><span data-stu-id="8dde6-134">Click **New Connection**.</span></span> <span data-ttu-id="8dde6-135">В диалоговом окне Свойства соединения введите имя сервера (например, **(LocalDB) \\ mssqllocaldb**), выберите метод проверки подлинности, введите **School**   в качестве имени базы данных и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="8dde6-135">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="8dde6-136">В диалоговом окне Выбор подключения к данным будет обновлен параметр подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="8dde6-136">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
- <span data-ttu-id="8dde6-137">В диалоговом окне Выбор объектов базы данных в узле **таблицы**   выберите таблицу **Person** .</span><span class="sxs-lookup"><span data-stu-id="8dde6-137">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person** table.</span></span>
- <span data-ttu-id="8dde6-138">Кроме того, выберите следующие хранимые процедуры в узле **хранимые процедуры и функции** : **делетеперсон**, **инсертперсон**и **упдатеперсон**.</span><span class="sxs-lookup"><span data-stu-id="8dde6-138">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **DeletePerson**, **InsertPerson**, and **UpdatePerson**.</span></span>
- <span data-ttu-id="8dde6-139">Начиная с Visual Studio 2012 конструктор EF поддерживает неполное импортирование хранимых процедур.</span><span class="sxs-lookup"><span data-stu-id="8dde6-139">Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures.</span></span> <span data-ttu-id="8dde6-140">По умолчанию флажок **Импорт выбранных хранимых процедур и функций в сущностную модель** установлен.</span><span class="sxs-lookup"><span data-stu-id="8dde6-140">The **Import selected stored procedures and functions into the entity model** is checked by default.</span></span> <span data-ttu-id="8dde6-141">Поскольку в этом примере у нас есть хранимые процедуры для вставки, обновления и удаления типов сущностей, мы не хотим импортировать их и снять этот флажок.</span><span class="sxs-lookup"><span data-stu-id="8dde6-141">Since in this example we have stored procedures that insert, update, and delete entity types, we do not want to import them and will uncheck this checkbox.</span></span>

    ![Процедуры импорта S](~/ef6/media/importsprocs.jpg)

- <span data-ttu-id="8dde6-143">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="8dde6-143">Click **Finish**.</span></span>
    <span data-ttu-id="8dde6-144">Отобразится конструктор EF, предоставляющий область конструктора для редактирования модели.</span><span class="sxs-lookup"><span data-stu-id="8dde6-144">The EF Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-the-person-entity-to-stored-procedures"></a><span data-ttu-id="8dde6-145">Сопоставьте сущность Person с хранимыми процедурами</span><span class="sxs-lookup"><span data-stu-id="8dde6-145">Map the Person Entity to Stored Procedures</span></span>

- <span data-ttu-id="8dde6-146">Щелкните правой кнопкой мыши **Person**   тип сущности Person и выберите **сопоставление хранимых процедур**.</span><span class="sxs-lookup"><span data-stu-id="8dde6-146">Right-click the **Person** entity type and select **Stored Procedure Mapping**.</span></span>
- <span data-ttu-id="8dde6-147">Сопоставления хранимых процедур отображаются в окне **сведения о сопоставлении**   .</span><span class="sxs-lookup"><span data-stu-id="8dde6-147">The stored procedure mappings appear in the **Mapping Details** window.</span></span>
- <span data-ttu-id="8dde6-148">Нажмите кнопку \*\* &lt; выбрать команду &gt; Вставить функцию\*\*.</span><span class="sxs-lookup"><span data-stu-id="8dde6-148">Click **&lt;Select Insert Function&gt;**.</span></span>
    <span data-ttu-id="8dde6-149">Поле становится раскрывающимся списком хранимых процедур в модели хранения, которые могут быть сопоставлены с типами сущностей в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="8dde6-149">The field becomes a drop-down list of the stored procedures in the storage model that can be mapped to entity types in the conceptual model.</span></span>
    <span data-ttu-id="8dde6-150">Выберите **инсертперсон**   из раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="8dde6-150">Select **InsertPerson** from the drop-down list.</span></span>
- <span data-ttu-id="8dde6-151">Появятся применяемые по умолчанию сопоставления параметров хранимой процедуры со свойствами сущности.</span><span class="sxs-lookup"><span data-stu-id="8dde6-151">Default mappings between stored procedure parameters and entity properties appear.</span></span> <span data-ttu-id="8dde6-152">Обратите внимание, что стрелки указывают направление сопоставления: «Значения свойств передаются параметрам хранимой процедуры».</span><span class="sxs-lookup"><span data-stu-id="8dde6-152">Note that arrows indicate the mapping direction: Property values are supplied to stored procedure parameters.</span></span>
- <span data-ttu-id="8dde6-153">Нажмите кнопку \*\* &lt; Добавить привязку &gt; к результату\*\*.</span><span class="sxs-lookup"><span data-stu-id="8dde6-153">Click **&lt;Add Result Binding&gt;**.</span></span>
- <span data-ttu-id="8dde6-154">Введите **невперсонид**, имя параметра, возвращаемого **InsertPerson**   хранимой процедурой инсертперсон.</span><span class="sxs-lookup"><span data-stu-id="8dde6-154">Type **NewPersonID**, the name of the parameter returned by the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="8dde6-155">Убедитесь, что не вводите начальные или конечные пробелы.</span><span class="sxs-lookup"><span data-stu-id="8dde6-155">Make sure not to type leading or trailing spaces.</span></span>
- <span data-ttu-id="8dde6-156">Нажмите клавишу **Ввод**.</span><span class="sxs-lookup"><span data-stu-id="8dde6-156">Press **Enter**.</span></span>
- <span data-ttu-id="8dde6-157">По умолчанию **невперсонид**   сопоставляется с ключом сущности **PersonID**.</span><span class="sxs-lookup"><span data-stu-id="8dde6-157">By default, **NewPersonID** is mapped to the entity key **PersonID**.</span></span> <span data-ttu-id="8dde6-158">Обратите внимание, что стрелка указывает направление сопоставления — значение столбца результата передается свойству.</span><span class="sxs-lookup"><span data-stu-id="8dde6-158">Note that an arrow indicates the direction of the mapping: The value of the result column is supplied to the property.</span></span>

    ![Сведения о сопоставлении](~/ef6/media/mappingdetails.png)

- <span data-ttu-id="8dde6-160">Щелкните \*\* &lt; выбрать обновление функции &gt; \*\*   и выберите **упдатеперсон**в   раскрывающемся списке получено.</span><span class="sxs-lookup"><span data-stu-id="8dde6-160">Click **&lt;Select Update Function&gt;** and select **UpdatePerson** from the resulting drop-down list.</span></span>
- <span data-ttu-id="8dde6-161">Появятся применяемые по умолчанию сопоставления параметров хранимой процедуры со свойствами сущности.</span><span class="sxs-lookup"><span data-stu-id="8dde6-161">Default mappings between stored procedure parameters and entity properties appear.</span></span>
- <span data-ttu-id="8dde6-162">Щелкните \*\* &lt; выбрать Удалить функцию &gt; \*\*   и выберите **делетеперсон**в   раскрывающемся списке получено.</span><span class="sxs-lookup"><span data-stu-id="8dde6-162">Click **&lt;Select Delete Function&gt;** and select **DeletePerson** from the resulting drop-down list.</span></span>
- <span data-ttu-id="8dde6-163">Появятся применяемые по умолчанию сопоставления параметров хранимой процедуры со свойствами сущности.</span><span class="sxs-lookup"><span data-stu-id="8dde6-163">Default mappings between stored procedure parameters and entity properties appear.</span></span>

<span data-ttu-id="8dde6-164">Операции вставки, обновления и удаления **Person**   типа сущности Person теперь сопоставлены с хранимыми процедурами.</span><span class="sxs-lookup"><span data-stu-id="8dde6-164">The insert, update, and delete operations of the **Person** entity type are now mapped to stored procedures.</span></span>

<span data-ttu-id="8dde6-165">Если необходимо включить проверку параллелизма при обновлении или удалении сущности с хранимыми процедурами, используйте один из следующих вариантов.</span><span class="sxs-lookup"><span data-stu-id="8dde6-165">If you want to enable concurrency checking when updating or deleting an entity with stored procedures, use one of the following options:</span></span>

- <span data-ttu-id="8dde6-166">Используйте **выходной** параметр, чтобы получить количество затронутых строк из хранимой процедуры и установить флажок **затронутые строки**   рядом с именем параметра.</span><span class="sxs-lookup"><span data-stu-id="8dde6-166">Use an **OUTPUT** parameter to return the number of affected rows from the stored procedure and check the **Rows Affected Parameter** checkbox next to the parameter name.</span></span> <span data-ttu-id="8dde6-167">Если возвращаемое значение равно нулю при вызове операции,  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx)   будет создано исключение оптимистикконкурренциексцептион.</span><span class="sxs-lookup"><span data-stu-id="8dde6-167">If the value returned is zero when the operation is called, an  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will be thrown.</span></span>
- <span data-ttu-id="8dde6-168">Установите флажок **использовать исходное значение** рядом со свойством, которое необходимо использовать для проверки параллелизма.</span><span class="sxs-lookup"><span data-stu-id="8dde6-168">Check the **Use Original Value** checkbox next to a property that you want to use for concurrency checking.</span></span> <span data-ttu-id="8dde6-169">При попытке обновления значение свойства, которое было первоначально считано из базы данных, будет использоваться при записи данных обратно в базу данных.</span><span class="sxs-lookup"><span data-stu-id="8dde6-169">When an update is attempted, the value of the property that was originally read from the database will be used when writing data back to the database.</span></span> <span data-ttu-id="8dde6-170">Если значение не совпадает со значением в базе данных, **OptimisticConcurrencyException**   будет выдано исключение оптимистикконкурренциексцептион.</span><span class="sxs-lookup"><span data-stu-id="8dde6-170">If the value does not match the value in the database, an **OptimisticConcurrencyException** will be thrown.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="8dde6-171">Использование модели</span><span class="sxs-lookup"><span data-stu-id="8dde6-171">Use the Model</span></span>

<span data-ttu-id="8dde6-172">Откройте файл **Program.CS** , в котором определен метод **Main** .</span><span class="sxs-lookup"><span data-stu-id="8dde6-172">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="8dde6-173">Добавьте следующий код в функцию main.</span><span class="sxs-lookup"><span data-stu-id="8dde6-173">Add the following code into the Main function.</span></span>

<span data-ttu-id="8dde6-174">Код создает новый объект **Person** , затем обновляет объект и, наконец, удаляет объект.</span><span class="sxs-lookup"><span data-stu-id="8dde6-174">The code creates a new **Person** object, then updates the object, and finally deletes the object.</span></span>

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

- <span data-ttu-id="8dde6-175">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="8dde6-175">Compile and run the application.</span></span> <span data-ttu-id="8dde6-176">Программа выдает следующие выходные данные \*</span><span class="sxs-lookup"><span data-stu-id="8dde6-176">The program produces the following output \*</span></span>

> [!NOTE]
> <span data-ttu-id="8dde6-177">PersonID автоматически создается сервером, поэтому, скорее всего, будет отображаться другое число \*</span><span class="sxs-lookup"><span data-stu-id="8dde6-177">PersonID is auto-generated by the server, so you will most likely see a different number\*</span></span>

``` Output
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

<span data-ttu-id="8dde6-178">При работе с конечной версией Visual Studio можно использовать IntelliTrace с отладчиком для просмотра инструкций SQL, которые будут выполнены.</span><span class="sxs-lookup"><span data-stu-id="8dde6-178">If you are working with the Ultimate version of Visual Studio, you can use Intellitrace with the debugger to see the SQL statements that get executed.</span></span>

![Отладка с помощью IntelliTrace](~/ef6/media/intellitrace.png)
