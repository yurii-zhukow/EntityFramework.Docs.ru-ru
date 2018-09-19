---
title: Конструктора запросов хранимые процедуры — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 04478ea1c8cd43a7ba4ee788e464992af3de7f64
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283905"
---
# <a name="designer-query-stored-procedures"></a><span data-ttu-id="5c5ed-102">Конструктора запроса хранимых процедур</span><span class="sxs-lookup"><span data-stu-id="5c5ed-102">Designer Query Stored Procedures</span></span>
<span data-ttu-id="5c5ed-103">Это пошаговое руководство показано, как используется Entity Framework Designer (конструктор EF) для импорта хранимой процедуры в модели, а затем вызвать импортированных хранимые процедуры для получения результатов.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-103">This step-by-step walkthrough show how to use the Entity Framework Designer (EF Designer) to import stored procedures into a model and then call the imported stored procedures to retrieve results.</span></span> 

<span data-ttu-id="5c5ed-104">Обратите внимание, что Code First не поддерживает сопоставление хранимых процедур или функций.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-104">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="5c5ed-105">Тем не менее можно вызвать хранимые процедуры или функции с помощью метода System.Data.Entity.DbSet.SqlQuery.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-105">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="5c5ed-106">Пример:</span><span class="sxs-lookup"><span data-stu-id="5c5ed-106">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a><span data-ttu-id="5c5ed-107">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="5c5ed-107">Prerequisites</span></span>

<span data-ttu-id="5c5ed-108">Для выполнения данного пошагового руководства требуется:</span><span class="sxs-lookup"><span data-stu-id="5c5ed-108">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="5c5ed-109">Последнюю версию Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-109">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="5c5ed-110">[Образца базы данных School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="5c5ed-110">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="5c5ed-111">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="5c5ed-111">Set up the Project</span></span>

-   <span data-ttu-id="5c5ed-112">Откройте Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-112">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="5c5ed-113">Выберите **файл -&gt; Новинка —&gt; проекта**</span><span class="sxs-lookup"><span data-stu-id="5c5ed-113">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="5c5ed-114">В левой области щелкните **Visual C\#**, а затем выберите **консоли** шаблона.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-114">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="5c5ed-115">Введите **EFwithSProcsSample** как имя.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-115">Enter **EFwithSProcsSample** as the name.</span></span>
-   <span data-ttu-id="5c5ed-116">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-116">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="5c5ed-117">Создание модели</span><span class="sxs-lookup"><span data-stu-id="5c5ed-117">Create a Model</span></span>

-   <span data-ttu-id="5c5ed-118">Щелкните правой кнопкой мыши проект в обозревателе решений и выберите **Add -&gt; новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-118">Right-click the project in Solution Explorer and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="5c5ed-119">Выберите **данных** меню слева, а затем выберите **ADO.NET Entity Data Model** в области шаблонов.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-119">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="5c5ed-120">Введите **EFwithSProcsModel.edmx** имя файла, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-120">Enter **EFwithSProcsModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="5c5ed-121">В диалоговом окне Выбор содержимого модели выберите **создать из базы данных**, а затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-121">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="5c5ed-122">Нажмите кнопку **новое подключение**.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-122">Click **New Connection**.</span></span>  
    <span data-ttu-id="5c5ed-123">В диалоговом окне Свойства соединения, введите имя сервера (например, **(localdb)\\mssqllocaldb**) выберите метод проверки подлинности, тип **School** для имени базы данных, а затем Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-123">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>  
    <span data-ttu-id="5c5ed-124">В диалоговом окне Выбор подключения к базе данных обновляется параметр подключения базы данных.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-124">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="5c5ed-125">В диалоговом окне Выбор объектов базы данных проверьте **таблиц** флажок, чтобы выбрать все таблицы.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-125">In the Choose Your Database Objects dialog box, check the **Tables** checkbox to select all the tables.</span></span>  
    <span data-ttu-id="5c5ed-126">Кроме того, выберите следующие хранимые процедуры в разделе **хранимые процедуры и функции** узла: **GetStudentGrades** и **GetDepartmentName**.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-126">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **GetStudentGrades** and **GetDepartmentName**.</span></span> 

    ![Импорт](~/ef6/media/import.jpg)

    <span data-ttu-id="5c5ed-128">*Начиная с Visual Studio 2012 конструкторе EF поддерживает массовый импорт хранимых процедур. **Импортировать выбранные хранимые процедуры и функции в модели theentity** установлен по умолчанию.*</span><span class="sxs-lookup"><span data-stu-id="5c5ed-128">*Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures. The **Import selected stored procedures and functions into theentity model** is checked by default.*</span></span>
-   <span data-ttu-id="5c5ed-129">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-129">Click **Finish**.</span></span>

<span data-ttu-id="5c5ed-130">По умолчанию результирующая форма каждого импортированного хранимую процедуру или функцию, которая возвращает более одного столбца автоматически становится новый сложный тип.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-130">By default, the result shape of each imported stored procedure or function that returns more than one column will automatically become a new complex type.</span></span> <span data-ttu-id="5c5ed-131">В этом примере требуется сопоставить результаты **GetStudentGrades** функцию **StudentGrade** сущности и результаты **GetDepartmentName** для **none** (**none** значение по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="5c5ed-131">In this example we want to map the results of the **GetStudentGrades** function to the **StudentGrade** entity and the results of the **GetDepartmentName** to **none** (**none** is the default value).</span></span>

<span data-ttu-id="5c5ed-132">Импорт функции для возврата типа сущности столбцы, возвращенные соответствующей хранимой процедуры должно совпадать скалярных свойств типа возвращаемой сущности.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-132">For a function import to return an entity type, the columns returned by the corresponding stored procedure must exactly match the scalar properties of the returned entity type.</span></span> <span data-ttu-id="5c5ed-133">Импорт функции также может возвращать коллекции простых типов, сложных типов или нет значения.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-133">A function import can also return collections of simple types, complex types, or no value.</span></span>

-   <span data-ttu-id="5c5ed-134">Щелкните правой кнопкой мыши область конструктора и выберите **браузер моделей**.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-134">Right-click the design surface and select **Model Browser**.</span></span>
-   <span data-ttu-id="5c5ed-135">В **браузер моделей**выберите **импортируемые функции**, а затем дважды щелкните **GetStudentGrades** функции.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-135">In **Model Browser**, select **Function Imports**, and then double-click the **GetStudentGrades** function.</span></span>
-   <span data-ttu-id="5c5ed-136">В диалоговом окне Изменение импорта функции выберите **сущностей** и выберите **StudentGrade**.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-136">In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**.</span></span>  
    <span data-ttu-id="5c5ed-137">***Импорт функции составляема** флажок в верхней части **импортируемые функции** диалогового окна можно сопоставить с поддержкой композиции функции. Если этот флажок установлен, только составную функциях (функции, возвращающие табличные значения) будет отображаться в **хранимой процедуры или имя функции** стрелку раскрывающегося списка. Если вы не установите этот флажок, в списке отобразятся только функции, не допускающие композицию.*</span><span class="sxs-lookup"><span data-stu-id="5c5ed-137">*The **Function Import is composable** checkbox at the top of the **Function Imports** dialog will let you map to composable functions. If you do check this box, only composable functions (Table-valued Functions) will appear in the **Stored Procedure / Function Name** drop-down list. If you do not check this box, only non-composable functions will be shown in the list.*</span></span>

## <a name="use-the-model"></a><span data-ttu-id="5c5ed-138">Использование модели</span><span class="sxs-lookup"><span data-stu-id="5c5ed-138">Use the Model</span></span>

<span data-ttu-id="5c5ed-139">Откройте **Program.cs** файл, в котором **Main** определен метод.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-139">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="5c5ed-140">Добавьте следующий код в функцию Main.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-140">Add the following code into the Main function.</span></span>

<span data-ttu-id="5c5ed-141">Код вызывает две хранимые процедуры: **GetStudentGrades** (возвращает **StudentGrades** для указанного *StudentId*) и **GetDepartmentName** (Возвращает имя подразделения в выходном параметре).</span><span class="sxs-lookup"><span data-stu-id="5c5ed-141">The code calls two stored procedures: **GetStudentGrades** (returns **StudentGrades** for the specified *StudentId*) and **GetDepartmentName** (returns the name of the department in the output parameter).</span></span>  

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        // Specify the Student ID.
        int studentId = 2;

        // Call GetStudentGrades and iterate through the returned collection.
        foreach (StudentGrade grade in context.GetStudentGrades(studentId))
        {
            Console.WriteLine("StudentID: {0}\tSubject={1}", studentId, grade.Subject);
            Console.WriteLine("Student grade: " + grade.Grade);
        }

        // Call GetDepartmentName.
        // Declare the name variable that will contain the value returned by the output parameter.
        ObjectParameter name = new ObjectParameter("Name", typeof(String));
        context.GetDepartmentName(1, name);
        Console.WriteLine("The department name is {0}", name.Value);

    }
```

<span data-ttu-id="5c5ed-142">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-142">Compile and run the application.</span></span> <span data-ttu-id="5c5ed-143">Программа выдает следующие результаты.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-143">The program produces the following output:</span></span>

```
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a><span data-ttu-id="5c5ed-144">Выходные параметры</span><span class="sxs-lookup"><span data-stu-id="5c5ed-144">Output Parameters</span></span>
-----------------

<span data-ttu-id="5c5ed-145">Если используются выходные параметры, их значения не будут доступны, пока не полностью считать результаты.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-145">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="5c5ed-146">Это происходит из-за особенностей DbDataReader, см. в разделе [получение данных с помощью объекта DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="5c5ed-146">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>
