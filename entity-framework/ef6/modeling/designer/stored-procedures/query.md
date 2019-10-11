---
title: Хранимые процедуры запросов конструктора — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 2e0092b526278597e8477d47eeb642598647bb91
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182480"
---
# <a name="designer-query-stored-procedures"></a><span data-ttu-id="77339-102">Хранимые процедуры запроса конструктора</span><span class="sxs-lookup"><span data-stu-id="77339-102">Designer Query Stored Procedures</span></span>
<span data-ttu-id="77339-103">В этом пошаговом руководстве показано, как использовать Entity Framework Designer (конструктор EF) для импорта хранимых процедур в модель, а затем вызывать импортированные хранимые процедуры для получения результатов.</span><span class="sxs-lookup"><span data-stu-id="77339-103">This step-by-step walkthrough show how to use the Entity Framework Designer (EF Designer) to import stored procedures into a model and then call the imported stored procedures to retrieve results.</span></span> 

<span data-ttu-id="77339-104">Обратите внимание, что Code First не поддерживает сопоставление с хранимыми процедурами или функциями.</span><span class="sxs-lookup"><span data-stu-id="77339-104">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="77339-105">Однако можно вызывать хранимые процедуры или функции с помощью метода System. Data. Entity. DbSet. SqlQuery.</span><span class="sxs-lookup"><span data-stu-id="77339-105">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="77339-106">Пример:</span><span class="sxs-lookup"><span data-stu-id="77339-106">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a><span data-ttu-id="77339-107">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="77339-107">Prerequisites</span></span>

<span data-ttu-id="77339-108">Для выполнения данного пошагового руководства требуется:</span><span class="sxs-lookup"><span data-stu-id="77339-108">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="77339-109">Последняя версия Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="77339-109">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="77339-110">[Образец базы данных School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="77339-110">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="77339-111">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="77339-111">Set up the Project</span></span>

-   <span data-ttu-id="77339-112">Откройте Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="77339-112">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="77339-113">Выберите **файл-&gt; новый проект-&gt;** .</span><span class="sxs-lookup"><span data-stu-id="77339-113">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="77339-114">В левой области щелкните **Visual C @ no__t-1**, а затем выберите шаблон **консоли** .</span><span class="sxs-lookup"><span data-stu-id="77339-114">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="77339-115">Введите **ефвисспрокссампле**@no__t — 1As имя.</span><span class="sxs-lookup"><span data-stu-id="77339-115">Enter **EFwithSProcsSample** as the name.</span></span>
-   <span data-ttu-id="77339-116">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="77339-116">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="77339-117">Создание модели</span><span class="sxs-lookup"><span data-stu-id="77339-117">Create a Model</span></span>

-   <span data-ttu-id="77339-118">Щелкните правой кнопкой мыши проект в обозреватель решений и выберите **Добавить-&gt; новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="77339-118">Right-click the project in Solution Explorer and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="77339-119">Выберите **данные** в меню слева, а затем выберите **ADO.NET EDM** в области Шаблоны.</span><span class="sxs-lookup"><span data-stu-id="77339-119">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="77339-120">Введите **ефвисспроксмодел. EDMX** в поле имя файла и нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="77339-120">Enter **EFwithSProcsModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="77339-121">В диалоговом окне Выбор содержимого модели выберите пункт **создать из базы данных**, а затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="77339-121">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="77339-122">Нажмите кнопку **создать соединение**.</span><span class="sxs-lookup"><span data-stu-id="77339-122">Click **New Connection**.</span></span>  
    <span data-ttu-id="77339-123">В диалоговом окне Свойства соединения введите имя сервера (например, **LocalDB) \\mssqllocaldb**), выберите метод проверки подлинности, введите **School** for имя базы данных и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="77339-123">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>  
    <span data-ttu-id="77339-124">В диалоговом окне Выбор подключения к данным будет обновлен параметр подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="77339-124">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="77339-125">В диалоговом окне Выбор объектов базы данных проверьте **таблицы** checkbox, чтобы выбрать все таблицы.</span><span class="sxs-lookup"><span data-stu-id="77339-125">In the Choose Your Database Objects dialog box, check the **Tables** checkbox to select all the tables.</span></span>  
    <span data-ttu-id="77339-126">Кроме того, выберите следующие хранимые процедуры в узле **хранимые процедуры и функции** : **Жетстудентградес** и **жетдепартментнаме**.</span><span class="sxs-lookup"><span data-stu-id="77339-126">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **GetStudentGrades** and **GetDepartmentName**.</span></span> 

    ![Импорт](~/ef6/media/import.jpg)

    <span data-ttu-id="77339-128">*Starting с Visual Studio 2012. конструктор EF поддерживает импорт хранимых процедур. По умолчанию флажок **Импорт выбранных хранимых процедур и функций в модель синтити** установлен.*</span><span class="sxs-lookup"><span data-stu-id="77339-128">*Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures. The **Import selected stored procedures and functions into theentity model** is checked by default.*</span></span>
-   <span data-ttu-id="77339-129">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="77339-129">Click **Finish**.</span></span>

<span data-ttu-id="77339-130">По умолчанию Результирующая форма каждой импортируемой хранимой процедуры или функции, которая возвращает более одного столбца, автоматически становится новым сложным типом.</span><span class="sxs-lookup"><span data-stu-id="77339-130">By default, the result shape of each imported stored procedure or function that returns more than one column will automatically become a new complex type.</span></span> <span data-ttu-id="77339-131">В этом примере мы хотим сопоставлять результаты функции **жетстудентградес** с сущностью **студентграде** , а результаты **жетдепартментнаме** — **нет** (значение по умолчанию —**None** ).</span><span class="sxs-lookup"><span data-stu-id="77339-131">In this example we want to map the results of the **GetStudentGrades** function to the **StudentGrade** entity and the results of the **GetDepartmentName** to **none** (**none** is the default value).</span></span>

<span data-ttu-id="77339-132">Чтобы импорт функции возвращал тип сущности, столбцы, возвращаемые соответствующей хранимой процедурой, должны точно соответствовать скалярным свойствам возвращаемого типа сущности.</span><span class="sxs-lookup"><span data-stu-id="77339-132">For a function import to return an entity type, the columns returned by the corresponding stored procedure must exactly match the scalar properties of the returned entity type.</span></span> <span data-ttu-id="77339-133">Импорт функции также может возвращать коллекции простых типов, сложных типов или без значений.</span><span class="sxs-lookup"><span data-stu-id="77339-133">A function import can also return collections of simple types, complex types, or no value.</span></span>

-   <span data-ttu-id="77339-134">Щелкните правой кнопкой мыши область конструктора и выберите пункт **Обозреватель моделей**.</span><span class="sxs-lookup"><span data-stu-id="77339-134">Right-click the design surface and select **Model Browser**.</span></span>
-   <span data-ttu-id="77339-135">В **обозревателе моделей**выберите **Импорт функций**, а затем дважды щелкните функцию **жетстудентградес** .</span><span class="sxs-lookup"><span data-stu-id="77339-135">In **Model Browser**, select **Function Imports**, and then double-click the **GetStudentGrades** function.</span></span>
-   <span data-ttu-id="77339-136">В диалоговом окне Изменение импорта функции выберите **сущности** And выберите **студентграде**.</span><span class="sxs-lookup"><span data-stu-id="77339-136">In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**.</span></span>  
    <span data-ttu-id="77339-137">*The **функция импорта** данных в верхней части диалогового окна **Импорт функций** позволяет сопоставляться с функциями, поддерживающими композицию. Если этот флажок установлен, в раскрывающемся списке **имя хранимой процедуры или функции** будут отображаться только функции с поддержкой композиций (функции, возвращающие табличное значение). Если этот флажок не установлен, в списке будут отображаться только функции, не допускающие композицию.*</span><span class="sxs-lookup"><span data-stu-id="77339-137">*The **Function Import is composable** checkbox at the top of the **Function Imports** dialog will let you map to composable functions. If you do check this box, only composable functions (Table-valued Functions) will appear in the **Stored Procedure / Function Name** drop-down list. If you do not check this box, only non-composable functions will be shown in the list.*</span></span>

## <a name="use-the-model"></a><span data-ttu-id="77339-138">Использование модели</span><span class="sxs-lookup"><span data-stu-id="77339-138">Use the Model</span></span>

<span data-ttu-id="77339-139">Откройте файл **Program.CS** , в котором определен метод **Main** .</span><span class="sxs-lookup"><span data-stu-id="77339-139">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="77339-140">Добавьте следующий код в функцию main.</span><span class="sxs-lookup"><span data-stu-id="77339-140">Add the following code into the Main function.</span></span>

<span data-ttu-id="77339-141">Код вызывает две хранимые процедуры: **Жетстудентградес** (Возвращает **Студентградес** для указанного *StudentId*) и **жетдепартментнаме** (Возвращает имя отдела в выходном параметре).</span><span class="sxs-lookup"><span data-stu-id="77339-141">The code calls two stored procedures: **GetStudentGrades** (returns **StudentGrades** for the specified *StudentId*) and **GetDepartmentName** (returns the name of the department in the output parameter).</span></span>  

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

<span data-ttu-id="77339-142">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="77339-142">Compile and run the application.</span></span> <span data-ttu-id="77339-143">Программа выдает следующие результаты.</span><span class="sxs-lookup"><span data-stu-id="77339-143">The program produces the following output:</span></span>

```console
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a><span data-ttu-id="77339-144">Выходные параметры</span><span class="sxs-lookup"><span data-stu-id="77339-144">Output Parameters</span></span>
-----------------

<span data-ttu-id="77339-145">Если используются выходные параметры, их значения будут недоступны до тех пор, пока результаты не считаны полностью.</span><span class="sxs-lookup"><span data-stu-id="77339-145">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="77339-146">Это обусловлено поведением DbDataReader. Дополнительные сведения см. в разделе [Получение данных с помощью DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) .</span><span class="sxs-lookup"><span data-stu-id="77339-146">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>
