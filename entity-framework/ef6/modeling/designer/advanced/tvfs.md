---
title: Функции Table-Valued (возвращающие табличное) — EF6
description: Функции Table-Valued (возвращающие табличное) в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/designer/advanced/tvfs
ms.openlocfilehash: 2c699cad3ace253842d17aca4eca4df2318c4795
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92066204"
---
# <a name="table-valued-functions-tvfs"></a><span data-ttu-id="405ad-103">Функции Table-Valued (возвращающие табличное)</span><span class="sxs-lookup"><span data-stu-id="405ad-103">Table-Valued Functions (TVFs)</span></span>
> [!NOTE]
> <span data-ttu-id="405ad-104">**Только EF5** — функции, API и т. д., обсуждаемые на этой странице, появились в Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="405ad-104">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="405ad-105">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="405ad-105">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="405ad-106">В видео и пошаговом руководстве показано, как сопоставлять функции с табличным значением (возвращающие табличное) с помощью Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="405ad-106">The video and step-by-step walkthrough shows how to map table-valued functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="405ad-107">Здесь также показано, как вызвать ТАБЛИЧную функцию из запроса LINQ.</span><span class="sxs-lookup"><span data-stu-id="405ad-107">It also demonstrates how to call a TVF from a LINQ query.</span></span>

<span data-ttu-id="405ad-108">Возвращающие табличное в настоящее время поддерживаются только в рабочем процессе Database First.</span><span class="sxs-lookup"><span data-stu-id="405ad-108">TVFs are currently only supported in the Database First workflow.</span></span>

<span data-ttu-id="405ad-109">В Entity Framework версии 5 появилась поддержка возвращающей табличное значение.</span><span class="sxs-lookup"><span data-stu-id="405ad-109">TVF support was introduced in Entity Framework version 5.</span></span> <span data-ttu-id="405ad-110">Обратите внимание, что для использования новых функций, таких как функции, возвращающие табличное значение, перечисления и пространственные типы, необходимо выбрать .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="405ad-110">Note that to use the new features like table-valued functions, enums, and spatial types you must target .NET Framework 4.5.</span></span> <span data-ttu-id="405ad-111">Visual Studio 2012 нацелена на .NET 4,5 по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="405ad-111">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="405ad-112">Возвращающие табличное очень похожи на хранимые процедуры с одним ключевым отличием: результат функции имеет композицию.</span><span class="sxs-lookup"><span data-stu-id="405ad-112">TVFs are very similar to stored procedures with one key difference: the result of a TVF is composable.</span></span> <span data-ttu-id="405ad-113">Это означает, что результаты из возвращающей табличное значение функции можно использовать в запросе LINQ, в то время как результаты хранимой процедуры не могут.</span><span class="sxs-lookup"><span data-stu-id="405ad-113">That means the results from a TVF can be used in a LINQ query while the results of a stored procedure cannot.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="405ad-114">Просмотреть видео</span><span class="sxs-lookup"><span data-stu-id="405ad-114">Watch the video</span></span>

<span data-ttu-id="405ad-115">**Представлено**: Julia Корнич</span><span class="sxs-lookup"><span data-stu-id="405ad-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="405ad-116">[WMV](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv)  |  [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v)  |  [WMV (ZIP)](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span><span class="sxs-lookup"><span data-stu-id="405ad-116">[WMV](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="405ad-117">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="405ad-117">Pre-Requisites</span></span>

<span data-ttu-id="405ad-118">Для выполнения этого пошагового руководства необходимо следующее:</span><span class="sxs-lookup"><span data-stu-id="405ad-118">To complete this walkthrough, you need to:</span></span>

- <span data-ttu-id="405ad-119">Установите [базу данных School](xref:ef6/resources/school-database).</span><span class="sxs-lookup"><span data-stu-id="405ad-119">Install the [School database](xref:ef6/resources/school-database).</span></span>

- <span data-ttu-id="405ad-120">Наличие последней версии Visual Studio</span><span class="sxs-lookup"><span data-stu-id="405ad-120">Have a recent version of Visual Studio</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="405ad-121">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="405ad-121">Set up the Project</span></span>

1.  <span data-ttu-id="405ad-122">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="405ad-122">Open Visual Studio</span></span>
2.  <span data-ttu-id="405ad-123">В меню **файл** наведите указатель мыши на пункт **создать**и выберите **проект** .</span><span class="sxs-lookup"><span data-stu-id="405ad-123">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="405ad-124">В левой области щелкните \*\*Visual C \# \*\*, а затем выберите шаблон **консоли** .</span><span class="sxs-lookup"><span data-stu-id="405ad-124">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="405ad-125">Введите **табличное** имя в качестве имени проекта и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="405ad-125">Enter **TVF** as the name of the project and click **OK**</span></span>

## <a name="add-a-tvf-to-the-database"></a><span data-ttu-id="405ad-126">Добавление ТАБЛИЧной функции в базу данных</span><span class="sxs-lookup"><span data-stu-id="405ad-126">Add a TVF to the Database</span></span>

-   <span data-ttu-id="405ad-127">Выберите **вид — &gt; Обозреватель объектов SQL Server**</span><span class="sxs-lookup"><span data-stu-id="405ad-127">Select **View -&gt; SQL Server Object Explorer**</span></span>
-   <span data-ttu-id="405ad-128">Если LocalDB отсутствует в списке серверов: щелкните правой кнопкой мыши **SQL Server** и выберите **Добавить SQL Server** использовать **проверку подлинности Windows** по умолчанию для подключения к серверу LocalDB.</span><span class="sxs-lookup"><span data-stu-id="405ad-128">If LocalDB is not in the list of servers: Right-click on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB server</span></span>
-   <span data-ttu-id="405ad-129">Разверните узел LocalDB.</span><span class="sxs-lookup"><span data-stu-id="405ad-129">Expand the LocalDB node</span></span>
-   <span data-ttu-id="405ad-130">В узле базы данных щелкните правой кнопкой мыши узел База данных School и выберите **создать запрос...**</span><span class="sxs-lookup"><span data-stu-id="405ad-130">Under the Databases node, right-click the School database node and select **New Query…**</span></span>
-   <span data-ttu-id="405ad-131">В редакторе T-SQL вставьте следующее определение возвращающего табличное значение</span><span class="sxs-lookup"><span data-stu-id="405ad-131">In T-SQL Editor, paste the following TVF definition</span></span>

``` SQL
CREATE FUNCTION [dbo].[GetStudentGradesForCourse]

(@CourseID INT)

RETURNS TABLE

RETURN
    SELECT [EnrollmentID],
           [CourseID],
           [StudentID],
           [Grade]
    FROM   [dbo].[StudentGrade]
    WHERE  CourseID = @CourseID
```

-   <span data-ttu-id="405ad-132">Щелкните правой кнопкой мыши в редакторе T-SQL и выберите команду **выполнить** .</span><span class="sxs-lookup"><span data-stu-id="405ad-132">Click the right mouse button on the T-SQL editor and select **Execute**</span></span>
-   <span data-ttu-id="405ad-133">Функция Жетстудентградесфоркаурсе добавляется в базу данных School.</span><span class="sxs-lookup"><span data-stu-id="405ad-133">The GetStudentGradesForCourse function is added to the School database</span></span>

 

## <a name="create-a-model"></a><span data-ttu-id="405ad-134">Создание модели</span><span class="sxs-lookup"><span data-stu-id="405ad-134">Create a Model</span></span>

1.  <span data-ttu-id="405ad-135">Щелкните правой кнопкой мыши имя проекта в обозреватель решений, наведите указатель на пункт **Добавить**и выберите пункт **новый элемент** .</span><span class="sxs-lookup"><span data-stu-id="405ad-135">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="405ad-136">Выберите **данные** в меню слева, а затем выберите **ADO.NET EDM** в области **шаблоны** .</span><span class="sxs-lookup"><span data-stu-id="405ad-136">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the **Templates** pane</span></span>
3.  <span data-ttu-id="405ad-137">Введите **твфмодел. EDMX** в поле имя файла и нажмите кнопку **Добавить** .</span><span class="sxs-lookup"><span data-stu-id="405ad-137">Enter **TVFModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="405ad-138">В диалоговом окне Выбор содержимого модели выберите пункт **создать из базы данных**, а затем нажмите кнопку **Далее** .</span><span class="sxs-lookup"><span data-stu-id="405ad-138">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**</span></span>
5.  <span data-ttu-id="405ad-139">Щелкните **новое соединение** ввод **(LocalDB) \\ mssqllocaldb** в текстовом поле имя сервера введите **School**   для имени базы данных, нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="405ad-139">Click **New Connection** Enter **(localdb)\\mssqllocaldb** in the Server name text box Enter **School** for the database name Click **OK**</span></span>
6.  <span data-ttu-id="405ad-140">В диалоговом окне Выбор объектов базы данных в узле **таблицы**   выберите таблицы **Person**, **студентграде**и **Course**   .</span><span class="sxs-lookup"><span data-stu-id="405ad-140">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person**, **StudentGrade**, and **Course** tables</span></span>
7.  <span data-ttu-id="405ad-141">Выберите функцию **жетстудентградесфоркаурсе** , расположенную в узле **хранимые процедуры и функции**   , которая начинается с Visual Studio 2012, Entity Designer позволяет выполнять пакетный импорт хранимых процедур и функций.</span><span class="sxs-lookup"><span data-stu-id="405ad-141">Select the **GetStudentGradesForCourse** function located under the **Stored Procedures and Functions** node Note, that starting with Visual Studio 2012, the Entity Designer allows you to batch import your Stored Procedures and Functions</span></span>
8.  <span data-ttu-id="405ad-142">Нажмите кнопку **Готово** .</span><span class="sxs-lookup"><span data-stu-id="405ad-142">Click **Finish**</span></span>
9.  <span data-ttu-id="405ad-143">Отобразится Entity Designer, предоставляющий область конструктора для редактирования модели.</span><span class="sxs-lookup"><span data-stu-id="405ad-143">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="405ad-144">Все объекты, выбранные в диалоговом окне **Выбор объектов базы данных**   , добавляются в модель.</span><span class="sxs-lookup"><span data-stu-id="405ad-144">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>
10. <span data-ttu-id="405ad-145">По умолчанию Результирующая форма каждой импортируемой хранимой процедуры или функции будет автоматически становиться новым сложным типом в модели сущностей.</span><span class="sxs-lookup"><span data-stu-id="405ad-145">By default, the result shape of each imported stored procedure or function will automatically become a new complex type in your entity model.</span></span> <span data-ttu-id="405ad-146">Но нам нужно соотнести результаты функции Жетстудентградесфоркаурсе с сущностью Студентграде: щелкните правой кнопкой мыши область конструктора и выберите в обозревателе моделей пункт **Обозреватель моделей** , выберите **Импорт функций**, а затем дважды щелкните функцию **Жетстудентградесфоркаурсе** в диалоговом окне Изменение импорта функции, выберите **сущности**   и выберите **студентграде** .</span><span class="sxs-lookup"><span data-stu-id="405ad-146">But we want to map the results of the GetStudentGradesForCourse function to the StudentGrade entity: Right-click the design surface and select **Model Browser** In Model Browser, select **Function Imports**, and then double-click the **GetStudentGradesForCourse** function In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="405ad-147">Сохранение и получение данных</span><span class="sxs-lookup"><span data-stu-id="405ad-147">Persist and Retrieve Data</span></span>

<span data-ttu-id="405ad-148">Откройте файл, в котором определен метод Main.</span><span class="sxs-lookup"><span data-stu-id="405ad-148">Open the file where the Main method is defined.</span></span> <span data-ttu-id="405ad-149">Добавьте следующий код в функцию main.</span><span class="sxs-lookup"><span data-stu-id="405ad-149">Add the following code into the Main function.</span></span>

<span data-ttu-id="405ad-150">В следующем коде показано, как создать запрос, использующий функцию с табличным значением.</span><span class="sxs-lookup"><span data-stu-id="405ad-150">The following code demonstrates how to build a query that uses a Table-valued Function.</span></span> <span data-ttu-id="405ad-151">Запрос проецирует результаты в анонимный тип, который содержит название соответствующего курса и связанные студенты с уровнем большей или равной 3,5.</span><span class="sxs-lookup"><span data-stu-id="405ad-151">The query projects the results into an anonymous type that contains the related Course title and related students with a grade greater or equal to 3.5.</span></span>

``` csharp
using (var context = new SchoolEntities())
{
    var CourseID = 4022;
    var Grade = 3.5M;

    // Return all the best students in the Microeconomics class.
    var students = from s in context.GetStudentGradesForCourse(CourseID)
                            where s.Grade >= Grade
                            select new
                            {
                                s.Person,
                                s.Course.Title
                            };

    foreach (var result in students)
    {
        Console.WriteLine(
            "Couse: {0}, Student: {1} {2}",
            result.Title,  
            result.Person.FirstName,  
            result.Person.LastName);
    }
}
```

<span data-ttu-id="405ad-152">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="405ad-152">Compile and run the application.</span></span> <span data-ttu-id="405ad-153">Программа выдает следующие результаты.</span><span class="sxs-lookup"><span data-stu-id="405ad-153">The program produces the following output:</span></span>

```console
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a><span data-ttu-id="405ad-154">Итоги</span><span class="sxs-lookup"><span data-stu-id="405ad-154">Summary</span></span>

<span data-ttu-id="405ad-155">В этом пошаговом руководстве мы рассмотрели, как сопоставлять функции с табличным значением (возвращающие табличное) с помощью Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="405ad-155">In this walkthrough we looked at how to map Table-valued Functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="405ad-156">Также показано, как вызвать ТАБЛИЧную функцию из запроса LINQ.</span><span class="sxs-lookup"><span data-stu-id="405ad-156">It also demonstrated how to call a TVF from a LINQ query.</span></span>
