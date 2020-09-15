---
title: Наследование TPT в конструкторе — EF6
description: Наследование TPT в конструкторе в Entity Framework 6
author: divega
ms.date: 10/23/2016
uid: ef6/modeling/designer/inheritance/tpt
ms.openlocfilehash: e60965550db3f140dfebf4c1952b3901b9b00aab
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90073374"
---
# <a name="designer-tpt-inheritance"></a><span data-ttu-id="560ec-103">Наследование TPT конструктора</span><span class="sxs-lookup"><span data-stu-id="560ec-103">Designer TPT Inheritance</span></span>
<span data-ttu-id="560ec-104">В этом пошаговом руководстве показано, как реализовать наследование типа "таблица на тип" (TPT) в модели с помощью Entity Framework Designer (конструктор EF).</span><span class="sxs-lookup"><span data-stu-id="560ec-104">This step-by-step walkthrough shows how to implement table-per-type (TPT) inheritance in your model using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="560ec-105">В наследовании типа «одна таблица на тип» используется отдельная таблица в базе данных для сопровождения данных, относящихся к ненаследуемым свойствам и ключевым свойствам для каждого типа в иерархии наследования.</span><span class="sxs-lookup"><span data-stu-id="560ec-105">Table-per-type inheritance uses a separate table in the database to maintain data for non-inherited properties and key properties for each type in the inheritance hierarchy.</span></span>

<span data-ttu-id="560ec-106">В этом пошаговом руководстве мы **Сообразим курс** (базовый тип), **онлинекаурсе** (наследуется от курса) и сущности **OnsiteCourse**   (наследуется от **курса**) к таблицам с теми же именами.</span><span class="sxs-lookup"><span data-stu-id="560ec-106">In this walkthrough we will map the **Course** (base type), **OnlineCourse** (derives from Course), and **OnsiteCourse** (derives from **Course**) entities to tables with the same names.</span></span> <span data-ttu-id="560ec-107">Мы создадим модель из базы данных, а затем изменим модель, чтобы реализовать наследование TPT.</span><span class="sxs-lookup"><span data-stu-id="560ec-107">We'll create a model from the database and then alter the model to implement the TPT inheritance.</span></span>

<span data-ttu-id="560ec-108">Можно также начать с Model First, а затем создать базу данных на основе модели.</span><span class="sxs-lookup"><span data-stu-id="560ec-108">You can also start with the Model First and then generate the database from the model.</span></span> <span data-ttu-id="560ec-109">По умолчанию конструктор EF использует стратегию TPT, поэтому все наследования в модели будут сопоставляться с отдельными таблицами.</span><span class="sxs-lookup"><span data-stu-id="560ec-109">The EF Designer uses the TPT strategy by default and so any inheritance in the model will be mapped to separate tables.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="560ec-110">Другие параметры наследования</span><span class="sxs-lookup"><span data-stu-id="560ec-110">Other Inheritance Options</span></span>

<span data-ttu-id="560ec-111">«Одна таблица на иерархию» — это еще один тип наследования, в котором одна таблица базы данных используется для хранения данных всех типов сущностей в иерархии наследования.</span><span class="sxs-lookup"><span data-stu-id="560ec-111">Table-per-Hierarchy (TPH) is another type of inheritance in which one database table is used to maintain data for all of the entity types in an inheritance hierarchy.</span></span><span data-ttu-id="560ec-112">Сведения о сопоставлении наследования "одна таблица на иерархию" с Entity Designer см. в разделе [наследование в конструкторе EF](xref:ef6/modeling/designer/inheritance/tph).</span><span class="sxs-lookup"><span data-stu-id="560ec-112">  For information about how to map Table-per-Hierarchy inheritance with the Entity Designer, see [EF Designer TPH Inheritance](xref:ef6/modeling/designer/inheritance/tph).</span></span> 

<span data-ttu-id="560ec-113">Обратите внимание, что модель наследования типа "Таблица — конкретная" и смешанное наследование поддерживаются средой выполнения Entity Framework, но не поддерживаются конструктором EF.</span><span class="sxs-lookup"><span data-stu-id="560ec-113">Note that, the Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="560ec-114">Если вы хотите использовать функции TPC или смешанное наследование, у вас есть два варианта: использовать Code First или вручную изменить файл EDMX.</span><span class="sxs-lookup"><span data-stu-id="560ec-114">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="560ec-115">Если вы решили работать с файлом EDMX, окно сведения о сопоставлении будет переведено в "защищенный режим", и вы не сможете использовать конструктор для изменения сопоставлений.</span><span class="sxs-lookup"><span data-stu-id="560ec-115">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="560ec-116">Предварительные условия</span><span class="sxs-lookup"><span data-stu-id="560ec-116">Prerequisites</span></span>

<span data-ttu-id="560ec-117">Для выполнения данного пошагового руководства требуется:</span><span class="sxs-lookup"><span data-stu-id="560ec-117">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="560ec-118">Последняя версия Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="560ec-118">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="560ec-119">[Образец базы данных School](xref:ef6/resources/school-database).</span><span class="sxs-lookup"><span data-stu-id="560ec-119">The [School sample database](xref:ef6/resources/school-database).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="560ec-120">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="560ec-120">Set up the Project</span></span>

-   <span data-ttu-id="560ec-121">Откройте Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="560ec-121">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="560ec-122">Выбор **файла- &gt; создать- &gt; проект**</span><span class="sxs-lookup"><span data-stu-id="560ec-122">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="560ec-123">В левой области щелкните \*\*Visual C \# \*\*, а затем выберите шаблон **консоли** .</span><span class="sxs-lookup"><span data-stu-id="560ec-123">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="560ec-124">Введите **тптдбфирстсампле**в   качестве имени.</span><span class="sxs-lookup"><span data-stu-id="560ec-124">Enter **TPTDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="560ec-125">Выберите **OK**.</span><span class="sxs-lookup"><span data-stu-id="560ec-125">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="560ec-126">Создание модели</span><span class="sxs-lookup"><span data-stu-id="560ec-126">Create a Model</span></span>

-   <span data-ttu-id="560ec-127">Щелкните правой кнопкой мыши проект в обозреватель решений и выберите **Добавить- &gt; новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="560ec-127">Right-click the project in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="560ec-128">Выберите **данные** в меню слева, а затем выберите **ADO.NET EDM** в области Шаблоны.</span><span class="sxs-lookup"><span data-stu-id="560ec-128">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="560ec-129">Введите **тптмодел. EDMX** в поле имя файла и нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="560ec-129">Enter **TPTModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="560ec-130">В диалоговом окне Выбор содержимого модели выберите пункт\*\*   создать из базы данных**, а затем нажмите кнопку **Далее\*\*.</span><span class="sxs-lookup"><span data-stu-id="560ec-130">In the Choose Model Contents dialog box, select** Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="560ec-131">Нажмите кнопку **создать соединение**.</span><span class="sxs-lookup"><span data-stu-id="560ec-131">Click **New Connection**.</span></span>
    <span data-ttu-id="560ec-132">В диалоговом окне Свойства соединения введите имя сервера (например, **(LocalDB) \\ mssqllocaldb**), выберите метод проверки подлинности, введите **School**   в качестве имени базы данных и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="560ec-132">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="560ec-133">В диалоговом окне Выбор подключения к данным будет обновлен параметр подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="560ec-133">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="560ec-134">В диалоговом окне Выбор объектов базы данных в узле таблицы выберите таблицы **Отдел**, **курс, онлинекаурсе и OnsiteCourse** .</span><span class="sxs-lookup"><span data-stu-id="560ec-134">In the Choose Your Database Objects dialog box, under the Tables node, select the **Department**, **Course, OnlineCourse, and OnsiteCourse** tables.</span></span>
-   <span data-ttu-id="560ec-135">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="560ec-135">Click **Finish**.</span></span>

<span data-ttu-id="560ec-136">Отобразится Entity Designer, предоставляющий область конструктора для редактирования модели.</span><span class="sxs-lookup"><span data-stu-id="560ec-136">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="560ec-137">Все объекты, выбранные в диалоговом окне Выбор объектов базы данных, добавляются в модель.</span><span class="sxs-lookup"><span data-stu-id="560ec-137">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

## <a name="implement-table-per-type-inheritance"></a><span data-ttu-id="560ec-138">Реализация наследования типа «таблица на тип»</span><span class="sxs-lookup"><span data-stu-id="560ec-138">Implement Table-per-Type Inheritance</span></span>

-   <span data-ttu-id="560ec-139">В области конструктора щелкните правой кнопкой мыши тип сущности **онлинекаурсе** и выберите пункт **свойства**.</span><span class="sxs-lookup"><span data-stu-id="560ec-139">On the design surface, right-click the **OnlineCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="560ec-140">В окне " **Свойства** " задайте для свойства "базовый тип" значение " **курс**".</span><span class="sxs-lookup"><span data-stu-id="560ec-140">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="560ec-141">Щелкните правой кнопкой мыши тип сущности **OnsiteCourse** и выберите пункт **свойства**.</span><span class="sxs-lookup"><span data-stu-id="560ec-141">Right-click the **OnsiteCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="560ec-142">В окне " **Свойства** " задайте для свойства "базовый тип" значение " **курс**".</span><span class="sxs-lookup"><span data-stu-id="560ec-142">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="560ec-143">Щелкните правой кнопкой мыши ассоциацию (линию) между типами сущностей **онлинекаурсе** и **Course** .</span><span class="sxs-lookup"><span data-stu-id="560ec-143">Right-click the association (the line) between the **OnlineCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="560ec-144">Выберите **удалить из модели**.</span><span class="sxs-lookup"><span data-stu-id="560ec-144">Select **Delete from Model**.</span></span>
-   <span data-ttu-id="560ec-145">Щелкните правой кнопкой мыши ассоциацию между типами сущностей **OnsiteCourse** и **Course** .</span><span class="sxs-lookup"><span data-stu-id="560ec-145">Right-click the association between the **OnsiteCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="560ec-146">Выберите **удалить из модели**.</span><span class="sxs-lookup"><span data-stu-id="560ec-146">Select **Delete from Model**.</span></span>

<span data-ttu-id="560ec-147">Теперь мы удалим свойство **CourseID** из **онлинекаурсе** и **OnsiteCourse** , так как эти классы наследуют **CourseID** от базового типа **курса** .</span><span class="sxs-lookup"><span data-stu-id="560ec-147">We will now delete the **CourseID** property from **OnlineCourse** and **OnsiteCourse** because these classes inherit **CourseID** from the **Course** base type.</span></span>

-   <span data-ttu-id="560ec-148">Щелкните правой кнопкой мыши свойство **CourseID** типа сущности **онлинекаурсе** и выберите пункт **удалить из модели**.</span><span class="sxs-lookup"><span data-stu-id="560ec-148">Right-click the **CourseID** property of the **OnlineCourse** entity type, and then select **Delete from Model**.</span></span>
-   <span data-ttu-id="560ec-149">Щелкните правой кнопкой мыши свойство **CourseID** типа сущности **OnsiteCourse** и выберите пункт **удалить из модели** .</span><span class="sxs-lookup"><span data-stu-id="560ec-149">Right-click the **CourseID** property of the **OnsiteCourse** entity type, and then select **Delete from Model**</span></span>
-   <span data-ttu-id="560ec-150">Теперь наследование типа «одна таблица на тип» реализовано.</span><span class="sxs-lookup"><span data-stu-id="560ec-150">Table-per-type inheritance is now implemented.</span></span>

![Таблица на тип](~/ef6/media/tpt.png)

## <a name="use-the-model"></a><span data-ttu-id="560ec-152">Использование модели</span><span class="sxs-lookup"><span data-stu-id="560ec-152">Use the Model</span></span>

<span data-ttu-id="560ec-153">Откройте файл **Program.CS** , в котором определен метод **Main** .</span><span class="sxs-lookup"><span data-stu-id="560ec-153">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="560ec-154">Вставьте следующий код в функцию **Main** .</span><span class="sxs-lookup"><span data-stu-id="560ec-154">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="560ec-155">Код выполняет три запроса.</span><span class="sxs-lookup"><span data-stu-id="560ec-155">The code executes three queries.</span></span> <span data-ttu-id="560ec-156">Первый запрос возвращает все **курсы** , связанные с указанным подразделением.</span><span class="sxs-lookup"><span data-stu-id="560ec-156">The first query brings back all **Courses** related to the specified department.</span></span> <span data-ttu-id="560ec-157">Второй запрос использует метод **OFTYPE** для возврата **онлинекаурсес** , связанного с указанным подразделением.</span><span class="sxs-lookup"><span data-stu-id="560ec-157">The second query uses the **OfType** method to return **OnlineCourses** related to the specified department.</span></span> <span data-ttu-id="560ec-158">Третий запрос возвращает **онситекаурсес**.</span><span class="sxs-lookup"><span data-stu-id="560ec-158">The third query returns **OnsiteCourses**.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        foreach (var department in context.Departments)
        {
            Console.WriteLine("The {0} department has the following courses:",
                               department.Name);

            Console.WriteLine("   All courses");
            foreach (var course in department.Courses )
            {
                Console.WriteLine("     {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnlineCourse>())
            {
                Console.WriteLine("   Online - {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnsiteCourse>())
            {
                Console.WriteLine("   Onsite - {0}", course.Title);
            }
        }
    }
```
