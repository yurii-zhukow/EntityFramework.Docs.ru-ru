---
title: Наследование TPT в конструкторе — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 84330fba4807620aa242a70cd8ac76a60284416d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415049"
---
# <a name="designer-tpt-inheritance"></a><span data-ttu-id="8ba98-102">Наследование TPT конструктора</span><span class="sxs-lookup"><span data-stu-id="8ba98-102">Designer TPT Inheritance</span></span>
<span data-ttu-id="8ba98-103">В этом пошаговом руководстве показано, как реализовать наследование типа "таблица на тип" (TPT) в модели с помощью Entity Framework Designer (конструктор EF).</span><span class="sxs-lookup"><span data-stu-id="8ba98-103">This step-by-step walkthrough shows how to implement table-per-type (TPT) inheritance in your model using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="8ba98-104">В наследовании типа «одна таблица на тип» используется отдельная таблица в базе данных для сопровождения данных, относящихся к ненаследуемым свойствам и ключевым свойствам для каждого типа в иерархии наследования.</span><span class="sxs-lookup"><span data-stu-id="8ba98-104">Table-per-type inheritance uses a separate table in the database to maintain data for non-inherited properties and key properties for each type in the inheritance hierarchy.</span></span>

<span data-ttu-id="8ba98-105">В этом пошаговом руководстве мы будем сопоставлять **курс** (базовый тип), **онлинекаурсе** (наследуется от курса) и **OnsiteCourse** (наследуется от **курса**) сущностям с теми же именами.</span><span class="sxs-lookup"><span data-stu-id="8ba98-105">In this walkthrough we will map the **Course** (base type), **OnlineCourse** (derives from Course), and **OnsiteCourse** (derives from **Course**) entities to tables with the same names.</span></span> <span data-ttu-id="8ba98-106">Мы создадим модель из базы данных, а затем изменим модель, чтобы реализовать наследование TPT.</span><span class="sxs-lookup"><span data-stu-id="8ba98-106">We'll create a model from the database and then alter the model to implement the TPT inheritance.</span></span>

<span data-ttu-id="8ba98-107">Можно также начать с Model First, а затем создать базу данных на основе модели.</span><span class="sxs-lookup"><span data-stu-id="8ba98-107">You can also start with the Model First and then generate the database from the model.</span></span> <span data-ttu-id="8ba98-108">По умолчанию конструктор EF использует стратегию TPT, поэтому все наследования в модели будут сопоставляться с отдельными таблицами.</span><span class="sxs-lookup"><span data-stu-id="8ba98-108">The EF Designer uses the TPT strategy by default and so any inheritance in the model will be mapped to separate tables.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="8ba98-109">Другие параметры наследования</span><span class="sxs-lookup"><span data-stu-id="8ba98-109">Other Inheritance Options</span></span>

<span data-ttu-id="8ba98-110">«Одна таблица на иерархию» — это еще один тип наследования, в котором одна таблица базы данных используется для хранения данных всех типов сущностей в иерархии наследования.</span><span class="sxs-lookup"><span data-stu-id="8ba98-110">Table-per-Hierarchy (TPH) is another type of inheritance in which one database table is used to maintain data for all of the entity types in an inheritance hierarchy.</span></span><span data-ttu-id="8ba98-111">  Сведения о сопоставлении наследования "одна таблица на иерархию" с Entity Designer см. в разделе [наследование в конструкторе EF](~/ef6/modeling/designer/inheritance/tph.md).</span><span class="sxs-lookup"><span data-stu-id="8ba98-111">  For information about how to map Table-per-Hierarchy inheritance with the Entity Designer, see [EF Designer TPH Inheritance](~/ef6/modeling/designer/inheritance/tph.md).</span></span> 

<span data-ttu-id="8ba98-112">Обратите внимание, что модель наследования типа "Таблица — конкретная" и смешанное наследование поддерживаются средой выполнения Entity Framework, но не поддерживаются конструктором EF.</span><span class="sxs-lookup"><span data-stu-id="8ba98-112">Note that, the Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="8ba98-113">Если вы хотите использовать функции TPC или смешанное наследование, у вас есть два варианта: использовать Code First или вручную изменить файл EDMX.</span><span class="sxs-lookup"><span data-stu-id="8ba98-113">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="8ba98-114">Если вы решили работать с файлом EDMX, окно сведения о сопоставлении будет переведено в "защищенный режим", и вы не сможете использовать конструктор для изменения сопоставлений.</span><span class="sxs-lookup"><span data-stu-id="8ba98-114">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8ba98-115">предварительные требования</span><span class="sxs-lookup"><span data-stu-id="8ba98-115">Prerequisites</span></span>

<span data-ttu-id="8ba98-116">Для выполнения данного пошагового руководства требуется:</span><span class="sxs-lookup"><span data-stu-id="8ba98-116">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="8ba98-117">Последняя версия Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8ba98-117">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="8ba98-118">[Образец базы данных School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="8ba98-118">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="8ba98-119">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="8ba98-119">Set up the Project</span></span>

-   <span data-ttu-id="8ba98-120">Откройте Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="8ba98-120">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="8ba98-121">Выберите **файл —&gt; создать&gt; проект**</span><span class="sxs-lookup"><span data-stu-id="8ba98-121">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="8ba98-122">В левой области щелкните **Visual C\#** , а затем выберите шаблон **консоли** .</span><span class="sxs-lookup"><span data-stu-id="8ba98-122">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="8ba98-123">В качестве имени введите **тптдбфирстсампле** .</span><span class="sxs-lookup"><span data-stu-id="8ba98-123">Enter **TPTDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="8ba98-124">Выберите **OK**.</span><span class="sxs-lookup"><span data-stu-id="8ba98-124">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="8ba98-125">Создание модели</span><span class="sxs-lookup"><span data-stu-id="8ba98-125">Create a Model</span></span>

-   <span data-ttu-id="8ba98-126">Щелкните правой кнопкой мыши проект в обозреватель решений и выберите **Добавить-&gt; новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="8ba98-126">Right-click the project in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="8ba98-127">Выберите **данные** в меню слева, а затем выберите **ADO.NET EDM** в области Шаблоны.</span><span class="sxs-lookup"><span data-stu-id="8ba98-127">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="8ba98-128">Введите **тптмодел. EDMX** в поле имя файла и нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="8ba98-128">Enter **TPTModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="8ba98-129">В диалоговом окне Выбор содержимого модели выберите ** создать из базы данных**, а затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="8ba98-129">In the Choose Model Contents dialog box, select** Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="8ba98-130">Нажмите кнопку **создать соединение**.</span><span class="sxs-lookup"><span data-stu-id="8ba98-130">Click **New Connection**.</span></span>
    <span data-ttu-id="8ba98-131">В диалоговом окне Свойства соединения введите имя сервера (например, **(LocalDB)\\mssqllocaldb**), выберите метод проверки подлинности, введите **School** в качестве имени базы данных и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="8ba98-131">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="8ba98-132">В диалоговом окне Выбор подключения к данным будет обновлен параметр подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="8ba98-132">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="8ba98-133">В диалоговом окне Выбор объектов базы данных в узле таблицы выберите таблицы **Отдел**, **курс, онлинекаурсе и OnsiteCourse** .</span><span class="sxs-lookup"><span data-stu-id="8ba98-133">In the Choose Your Database Objects dialog box, under the Tables node, select the **Department**, **Course, OnlineCourse, and OnsiteCourse** tables.</span></span>
-   <span data-ttu-id="8ba98-134">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="8ba98-134">Click **Finish**.</span></span>

<span data-ttu-id="8ba98-135">Отобразится Entity Designer, предоставляющий область конструктора для редактирования модели.</span><span class="sxs-lookup"><span data-stu-id="8ba98-135">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="8ba98-136">Все объекты, выбранные в диалоговом окне Выбор объектов базы данных, добавляются в модель.</span><span class="sxs-lookup"><span data-stu-id="8ba98-136">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

## <a name="implement-table-per-type-inheritance"></a><span data-ttu-id="8ba98-137">Реализация наследования типа «таблица на тип»</span><span class="sxs-lookup"><span data-stu-id="8ba98-137">Implement Table-per-Type Inheritance</span></span>

-   <span data-ttu-id="8ba98-138">В области конструктора щелкните правой кнопкой мыши тип сущности **онлинекаурсе** и выберите пункт **свойства**.</span><span class="sxs-lookup"><span data-stu-id="8ba98-138">On the design surface, right-click the **OnlineCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="8ba98-139">В окне " **Свойства** " задайте для свойства "базовый тип" значение " **курс**".</span><span class="sxs-lookup"><span data-stu-id="8ba98-139">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="8ba98-140">Щелкните правой кнопкой мыши тип сущности **OnsiteCourse** и выберите пункт **свойства**.</span><span class="sxs-lookup"><span data-stu-id="8ba98-140">Right-click the **OnsiteCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="8ba98-141">В окне " **Свойства** " задайте для свойства "базовый тип" значение " **курс**".</span><span class="sxs-lookup"><span data-stu-id="8ba98-141">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="8ba98-142">Щелкните правой кнопкой мыши ассоциацию (линию) между типами сущностей **онлинекаурсе** и **Course** .</span><span class="sxs-lookup"><span data-stu-id="8ba98-142">Right-click the association (the line) between the **OnlineCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="8ba98-143">Выберите **удалить из модели**.</span><span class="sxs-lookup"><span data-stu-id="8ba98-143">Select **Delete from Model**.</span></span>
-   <span data-ttu-id="8ba98-144">Щелкните правой кнопкой мыши ассоциацию между типами сущностей **OnsiteCourse** и **Course** .</span><span class="sxs-lookup"><span data-stu-id="8ba98-144">Right-click the association between the **OnsiteCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="8ba98-145">Выберите **удалить из модели**.</span><span class="sxs-lookup"><span data-stu-id="8ba98-145">Select **Delete from Model**.</span></span>

<span data-ttu-id="8ba98-146">Теперь мы удалим свойство **CourseID** из **онлинекаурсе** и **OnsiteCourse** , так как эти классы наследуют **CourseID** от базового типа **курса** .</span><span class="sxs-lookup"><span data-stu-id="8ba98-146">We will now delete the **CourseID** property from **OnlineCourse** and **OnsiteCourse** because these classes inherit **CourseID** from the **Course** base type.</span></span>

-   <span data-ttu-id="8ba98-147">Щелкните правой кнопкой мыши свойство **CourseID** типа сущности **онлинекаурсе** и выберите пункт **удалить из модели**.</span><span class="sxs-lookup"><span data-stu-id="8ba98-147">Right-click the **CourseID** property of the **OnlineCourse** entity type, and then select **Delete from Model**.</span></span>
-   <span data-ttu-id="8ba98-148">Щелкните правой кнопкой мыши свойство **CourseID** типа сущности **OnsiteCourse** и выберите пункт **удалить из модели** .</span><span class="sxs-lookup"><span data-stu-id="8ba98-148">Right-click the **CourseID** property of the **OnsiteCourse** entity type, and then select **Delete from Model**</span></span>
-   <span data-ttu-id="8ba98-149">Теперь наследование типа «одна таблица на тип» реализовано.</span><span class="sxs-lookup"><span data-stu-id="8ba98-149">Table-per-type inheritance is now implemented.</span></span>

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a><span data-ttu-id="8ba98-151">Использование модели</span><span class="sxs-lookup"><span data-stu-id="8ba98-151">Use the Model</span></span>

<span data-ttu-id="8ba98-152">Откройте файл **Program.CS** , в котором определен метод **Main** .</span><span class="sxs-lookup"><span data-stu-id="8ba98-152">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="8ba98-153">Вставьте следующий код в функцию **Main** .</span><span class="sxs-lookup"><span data-stu-id="8ba98-153">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="8ba98-154">Код выполняет три запроса.</span><span class="sxs-lookup"><span data-stu-id="8ba98-154">The code executes three queries.</span></span> <span data-ttu-id="8ba98-155">Первый запрос возвращает все **курсы** , связанные с указанным подразделением.</span><span class="sxs-lookup"><span data-stu-id="8ba98-155">The first query brings back all **Courses** related to the specified department.</span></span> <span data-ttu-id="8ba98-156">Второй запрос использует метод **OFTYPE** для возврата **онлинекаурсес** , связанного с указанным подразделением.</span><span class="sxs-lookup"><span data-stu-id="8ba98-156">The second query uses the **OfType** method to return **OnlineCourses** related to the specified department.</span></span> <span data-ttu-id="8ba98-157">Третий запрос возвращает **онситекаурсес**.</span><span class="sxs-lookup"><span data-stu-id="8ba98-157">The third query returns **OnsiteCourses**.</span></span>

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
