---
title: Возвращающие табличные значения функции (TVF) - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: f4b8c1bd442bac67a06584bd23c040381374f303
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993251"
---
# <a name="table-valued-functions-tvfs"></a><span data-ttu-id="0bd2a-102">Возвращающие табличные значения функции (TVF)</span><span class="sxs-lookup"><span data-stu-id="0bd2a-102">Table-Valued Functions (TVFs)</span></span>
> [!NOTE]
> <span data-ttu-id="0bd2a-103">**EF5 и более поздних версий только** -функции, интерфейсы API, и т.д., описанных на этой странице появились в Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="0bd2a-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="0bd2a-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="0bd2a-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="0bd2a-105">Видео и пошаговые руководства показано, как сопоставить возвращающих табличные значения функции (TVF), с помощью Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="0bd2a-105">The video and step-by-step walkthrough shows how to map table-valued functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="0bd2a-106">Он также демонстрирует вызов функции с табличным значением из запроса LINQ.</span><span class="sxs-lookup"><span data-stu-id="0bd2a-106">It also demonstrates how to call a TVF from a LINQ query.</span></span>

<span data-ttu-id="0bd2a-107">Возвращающие табличное значение функции в настоящий момент поддерживается только в базе данных первого рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="0bd2a-107">TVFs are currently only supported in the Database First workflow.</span></span>

<span data-ttu-id="0bd2a-108">Возвращающая табличное значение Функция поддержки появилась в версии 5 платформы Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0bd2a-108">TVF support was introduced in Entity Framework version 5.</span></span> <span data-ttu-id="0bd2a-109">Обратите внимание, что для использования новых возможностей, таких как функции, возвращающие табличные значения, перечислимые типы и Пространственные типы .NET Framework 4.5 необходимо ориентироваться.</span><span class="sxs-lookup"><span data-stu-id="0bd2a-109">Note that to use the new features like table-valued functions, enums, and spatial types you must target .NET Framework 4.5.</span></span> <span data-ttu-id="0bd2a-110">Visual Studio 2012 предназначенного для .NET 4.5 по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0bd2a-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="0bd2a-111">Возвращающие табличное значение функции очень похожи на хранимые процедуры с одним основным отличием: составляема результатом функции с табличным значением.</span><span class="sxs-lookup"><span data-stu-id="0bd2a-111">TVFs are very similar to stored procedures with one key difference: the result of a TVF is composable.</span></span> <span data-ttu-id="0bd2a-112">Это означает, что результаты из функции с табличным значением может использоваться в запросе LINQ, хотя результаты хранимой процедуры нельзя.</span><span class="sxs-lookup"><span data-stu-id="0bd2a-112">That means the results from a TVF can be used in a LINQ query while the results of a stored procedure cannot.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="0bd2a-113">Просмотреть видео</span><span class="sxs-lookup"><span data-stu-id="0bd2a-113">Watch the video</span></span>

<span data-ttu-id="0bd2a-114">**Представленный**: Юлия Корнич</span><span class="sxs-lookup"><span data-stu-id="0bd2a-114">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="0bd2a-115">[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span><span class="sxs-lookup"><span data-stu-id="0bd2a-115">[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="0bd2a-116">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="0bd2a-116">Pre-Requisites</span></span>

<span data-ttu-id="0bd2a-117">Для выполнения этого пошагового руководства, необходимо:</span><span class="sxs-lookup"><span data-stu-id="0bd2a-117">To complete this walkthrough, you need to:</span></span>

- <span data-ttu-id="0bd2a-118">Установка [базы данных School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="0bd2a-118">Install the [School database](~/ef6/resources/school-database.md).</span></span>

- <span data-ttu-id="0bd2a-119">У последняя версия Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0bd2a-119">Have a recent version of Visual Studio</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="0bd2a-120">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="0bd2a-120">Set up the Project</span></span>

1.  <span data-ttu-id="0bd2a-121">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0bd2a-121">Open Visual Studio</span></span>
2.  <span data-ttu-id="0bd2a-122">На **файл** последовательно выберите пункты **New**, а затем нажмите кнопку **проекта**</span><span class="sxs-lookup"><span data-stu-id="0bd2a-122">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="0bd2a-123">В левой области щелкните **Visual C\#**, а затем выберите **консоли** шаблона</span><span class="sxs-lookup"><span data-stu-id="0bd2a-123">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="0bd2a-124">Введите **возвращающей табличное значение функции** как имя проекта и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="0bd2a-124">Enter **TVF** as the name of the project and click **OK**</span></span>

## <a name="add-a-tvf-to-the-database"></a><span data-ttu-id="0bd2a-125">Добавление функции с табличным значением в базу данных</span><span class="sxs-lookup"><span data-stu-id="0bd2a-125">Add a TVF to the Database</span></span>

-   <span data-ttu-id="0bd2a-126">Выберите **представление —&gt; обозреватель объектов SQL Server**</span><span class="sxs-lookup"><span data-stu-id="0bd2a-126">Select **View -&gt; SQL Server Object Explorer**</span></span>
-   <span data-ttu-id="0bd2a-127">Если LocalDB отсутствует в списке серверов: щелкните правой кнопкой мыши **SQL Server** и выберите **добавить SQL Server** используйте значение по умолчанию **проверки подлинности Windows** для подключения к серверу LocalDB</span><span class="sxs-lookup"><span data-stu-id="0bd2a-127">If LocalDB is not in the list of servers: Right-click on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB server</span></span>
-   <span data-ttu-id="0bd2a-128">Разверните узел LocalDB</span><span class="sxs-lookup"><span data-stu-id="0bd2a-128">Expand the LocalDB node</span></span>
-   <span data-ttu-id="0bd2a-129">В узле базы данных, щелкните правой кнопкой мыши узел базы данных School и выберите **новый запрос...**</span><span class="sxs-lookup"><span data-stu-id="0bd2a-129">Under the Databases node, right-click the School database node and select **New Query…**</span></span>
-   <span data-ttu-id="0bd2a-130">В редакторе T-SQL, вставьте следующее определение функции с табличным значением</span><span class="sxs-lookup"><span data-stu-id="0bd2a-130">In T-SQL Editor, paste the following TVF definition</span></span>

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

-   <span data-ttu-id="0bd2a-131">Щелкните правой кнопкой мыши в редакторе T-SQL и выберите **Execute**</span><span class="sxs-lookup"><span data-stu-id="0bd2a-131">Click the right mouse button on the T-SQL editor and select **Execute**</span></span>
-   <span data-ttu-id="0bd2a-132">Функция GetStudentGradesForCourse будет добавлена в базу данных School</span><span class="sxs-lookup"><span data-stu-id="0bd2a-132">The GetStudentGradesForCourse function is added to the School database</span></span>

 

## <a name="create-a-model"></a><span data-ttu-id="0bd2a-133">Создание модели</span><span class="sxs-lookup"><span data-stu-id="0bd2a-133">Create a Model</span></span>

1.  <span data-ttu-id="0bd2a-134">Щелкните правой кнопкой мыши имя проекта в обозревателе решений, выберите пункт **добавить**, а затем нажмите кнопку **новый элемент**</span><span class="sxs-lookup"><span data-stu-id="0bd2a-134">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="0bd2a-135">Выберите **данных** меню слева, а затем выберите **ADO.NET Entity Data Model** в **шаблоны** области</span><span class="sxs-lookup"><span data-stu-id="0bd2a-135">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the **Templates** pane</span></span>
3.  <span data-ttu-id="0bd2a-136">Введите **TVFModel.edmx** имя файла, а затем нажмите кнопку **добавить**</span><span class="sxs-lookup"><span data-stu-id="0bd2a-136">Enter **TVFModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="0bd2a-137">В диалоговом окне Выбор содержимого модели выберите **создать из базы данных**, а затем нажмите кнопку **Далее**</span><span class="sxs-lookup"><span data-stu-id="0bd2a-137">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**</span></span>
5.  <span data-ttu-id="0bd2a-138">Нажмите кнопку **новое подключение** ввод **(localdb)\\mssqllocaldb** в текст имени сервера поле Ввод **School** для базы данных имя щелкните **ОК**</span><span class="sxs-lookup"><span data-stu-id="0bd2a-138">Click **New Connection** Enter **(localdb)\\mssqllocaldb** in the Server name text box Enter **School** for the database name Click **OK**</span></span>
6.  <span data-ttu-id="0bd2a-139">В поле выберите ваши объекты базы данных диалогового **таблиц** выберите **Person**, **StudentGrade**, и **курс** таблиц</span><span class="sxs-lookup"><span data-stu-id="0bd2a-139">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person**, **StudentGrade**, and **Course** tables</span></span>
7.  <span data-ttu-id="0bd2a-140">Выберите **GetStudentGradesForCourse** функции, расположенный в **хранимые процедуры и функции** узла Обратите внимание, что, начиная с Visual Studio 2012 г. конструктор сущностей позволяет пакетный Импорт Хранимые процедуры и функции</span><span class="sxs-lookup"><span data-stu-id="0bd2a-140">Select the **GetStudentGradesForCourse** function located under the **Stored Procedures and Functions** node Note, that starting with Visual Studio 2012, the Entity Designer allows you to batch import your Stored Procedures and Functions</span></span>
8.  <span data-ttu-id="0bd2a-141">Нажмите кнопку **Готово**</span><span class="sxs-lookup"><span data-stu-id="0bd2a-141">Click **Finish**</span></span>
9.  <span data-ttu-id="0bd2a-142">Конструктор сущностей, который предоставляет область конструктора для изменения модели, отображается.</span><span class="sxs-lookup"><span data-stu-id="0bd2a-142">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="0bd2a-143">Все объекты, которые выбраны в **Choose Your Database Objects** диалоговое окно добавляются в модель.</span><span class="sxs-lookup"><span data-stu-id="0bd2a-143">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>
10. <span data-ttu-id="0bd2a-144">По умолчанию результирующая форма каждый импортированный хранимой процедуры или функции автоматически становится новый сложный тип в модели сущности.</span><span class="sxs-lookup"><span data-stu-id="0bd2a-144">By default, the result shape of each imported stored procedure or function will automatically become a new complex type in your entity model.</span></span> <span data-ttu-id="0bd2a-145">Но нам нужно сопоставить с сущностью StudentGrade результаты функции GetStudentGradesForCourse: щелкните правой кнопкой мыши область конструктора и выберите **браузер моделей** в браузере моделей выберите **импортируемые функции**, а затем дважды щелкните **GetStudentGradesForCourse** функции в изменение импорта функции установите флажок **сущностей** и выберите **StudentGrade**</span><span class="sxs-lookup"><span data-stu-id="0bd2a-145">But we want to map the results of the GetStudentGradesForCourse function to the StudentGrade entity: Right-click the design surface and select **Model Browser** In Model Browser, select **Function Imports**, and then double-click the **GetStudentGradesForCourse** function In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="0bd2a-146">Сохранения и извлечения данных</span><span class="sxs-lookup"><span data-stu-id="0bd2a-146">Persist and Retrieve Data</span></span>

<span data-ttu-id="0bd2a-147">Откройте файл, в котором определен метод Main.</span><span class="sxs-lookup"><span data-stu-id="0bd2a-147">Open the file where the Main method is defined.</span></span> <span data-ttu-id="0bd2a-148">Добавьте следующий код в функцию Main.</span><span class="sxs-lookup"><span data-stu-id="0bd2a-148">Add the following code into the Main function.</span></span>

<span data-ttu-id="0bd2a-149">Следующий код демонстрирует построение запроса, использующего Table-valued Function.</span><span class="sxs-lookup"><span data-stu-id="0bd2a-149">The following code demonstrates how to build a query that uses a Table-valued Function.</span></span> <span data-ttu-id="0bd2a-150">Результаты запроса проецируются в анонимный тип, содержащий связанные название курса и связанных слушателям оценку больше или равно 3.5.</span><span class="sxs-lookup"><span data-stu-id="0bd2a-150">The query projects the results into an anonymous type that contains the related Course title and related students with a grade greater or equal to 3.5.</span></span>

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

<span data-ttu-id="0bd2a-151">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="0bd2a-151">Compile and run the application.</span></span> <span data-ttu-id="0bd2a-152">Программа выдает следующие результаты.</span><span class="sxs-lookup"><span data-stu-id="0bd2a-152">The program produces the following output:</span></span>

```
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a><span data-ttu-id="0bd2a-153">Сводка</span><span class="sxs-lookup"><span data-stu-id="0bd2a-153">Summary</span></span>

<span data-ttu-id="0bd2a-154">В этом пошаговом руководстве мы рассмотрели способ сопоставления возвращающих табличные значения функции (TVF), с помощью Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="0bd2a-154">In this walkthrough we looked at how to map Table-valued Functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="0bd2a-155">Кроме того, вы узнали, как для вызова функции с табличным значением из запроса LINQ.</span><span class="sxs-lookup"><span data-stu-id="0bd2a-155">It also demonstrated how to call a TVF from a LINQ query.</span></span>
