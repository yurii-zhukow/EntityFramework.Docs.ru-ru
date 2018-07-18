---
title: Наследование TPT конструктора — EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
caps.latest.revision: 3
ms.openlocfilehash: c3ccb44f931b830a96a553d5af1e722a9ca4bbf0
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122377"
---
# <a name="designer-tpt-inheritance"></a><span data-ttu-id="585ec-102">Наследование TPT конструктора</span><span class="sxs-lookup"><span data-stu-id="585ec-102">Designer TPT Inheritance</span></span>
<span data-ttu-id="585ec-103">Это пошаговое руководство показывает, как реализовать наследование таблица на тип (TPT) в вашей модели, с помощью Entity Framework Designer (конструктор EF).</span><span class="sxs-lookup"><span data-stu-id="585ec-103">This step-by-step walkthrough shows how to implement table-per-type (TPT) inheritance in your model using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="585ec-104">В наследовании типа «одна таблица на тип» используется отдельная таблица в базе данных для сопровождения данных, относящихся к ненаследуемым свойствам и ключевым свойствам для каждого типа в иерархии наследования.</span><span class="sxs-lookup"><span data-stu-id="585ec-104">Table-per-type inheritance uses a separate table in the database to maintain data for non-inherited properties and key properties for each type in the inheritance hierarchy.</span></span>

<span data-ttu-id="585ec-105">В этом пошаговом руководстве мы сопоставим **курс** (базовый тип), **OnlineCourse** (наследует от курса), и **OnsiteCourse** (является производным от **курс**) сущности для таблиц с одинаковыми именами.</span><span class="sxs-lookup"><span data-stu-id="585ec-105">In this walkthrough we will map the **Course** (base type), **OnlineCourse** (derives from Course), and **OnsiteCourse** (derives from **Course**) entities to tables with the same names.</span></span> <span data-ttu-id="585ec-106">Мы создадим модель из базы данных и затем изменить модель для реализации наследования TPT.</span><span class="sxs-lookup"><span data-stu-id="585ec-106">We'll create a model from the database and then alter the model to implement the TPT inheritance.</span></span>

<span data-ttu-id="585ec-107">Можно также начать работу с первой модели и затем создать базу данных из модели.</span><span class="sxs-lookup"><span data-stu-id="585ec-107">You can also start with the Model First and then generate the database from the model.</span></span> <span data-ttu-id="585ec-108">Конструктор EF по умолчанию использует стратегию TPT и поэтому все наследования в модели будут сопоставлены с несколькими таблицами.</span><span class="sxs-lookup"><span data-stu-id="585ec-108">The EF Designer uses the TPT strategy by default and so any inheritance in the model will be mapped to separate tables.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="585ec-109">Другие параметры наследования</span><span class="sxs-lookup"><span data-stu-id="585ec-109">Other Inheritance Options</span></span>

<span data-ttu-id="585ec-110">Одна таблица на иерархию (TPH) – еще один тип наследования, в которых одна база данных таблица используется для сопровождения данных всех типов сущностей в иерархии наследования.</span><span class="sxs-lookup"><span data-stu-id="585ec-110">Table-per-Hierarchy (TPH) is another type of inheritance in which one database table is used to maintain data for all of the entity types in an inheritance hierarchy.</span></span>  <span data-ttu-id="585ec-111">Сведения о сопоставлении таблица на иерархию наследования с использованием конструктора сущностей см. в разделе [EF конструктор МОДЕЛИ наследования](~/ef6/modeling/designer/inheritance/tph.md).</span><span class="sxs-lookup"><span data-stu-id="585ec-111">For information about how to map Table-per-Hierarchy inheritance with the Entity Designer, see [EF Designer TPH Inheritance](~/ef6/modeling/designer/inheritance/tph.md).</span></span> 

<span data-ttu-id="585ec-112">Обратите внимание, что таблица на конкретный тип наследования (TPC) и смешанных наследование моделей поддерживаются средой выполнения Entity Framework, но не поддерживаются в конструкторе EF.</span><span class="sxs-lookup"><span data-stu-id="585ec-112">Note that, the Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="585ec-113">Если вы хотите использовать TPC или смешанных наследования, можно двумя способами: использовать Code First, или вручную изменить EDMX-файла.</span><span class="sxs-lookup"><span data-stu-id="585ec-113">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="585ec-114">Если вы предпочтете работать с файлом EDMX, окно сведений о сопоставлении будет помещен в «безопасный режим», и вы не сможете использовать конструктор для изменения сопоставлений.</span><span class="sxs-lookup"><span data-stu-id="585ec-114">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="585ec-115">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="585ec-115">Prerequisites</span></span>

<span data-ttu-id="585ec-116">Для выполнения данного пошагового руководства требуется:</span><span class="sxs-lookup"><span data-stu-id="585ec-116">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="585ec-117">Последнюю версию Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="585ec-117">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="585ec-118">[Образца базы данных School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="585ec-118">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="585ec-119">Настройка проекта</span><span class="sxs-lookup"><span data-stu-id="585ec-119">Set up the Project</span></span>

-   <span data-ttu-id="585ec-120">Откройте Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="585ec-120">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="585ec-121">Выберите **файл -&gt; Новинка —&gt; проекта**</span><span class="sxs-lookup"><span data-stu-id="585ec-121">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="585ec-122">В левой области щелкните **Visual C\#**, а затем выберите **консоли** шаблона.</span><span class="sxs-lookup"><span data-stu-id="585ec-122">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="585ec-123">Введите **TPTDBFirstSample** как имя.</span><span class="sxs-lookup"><span data-stu-id="585ec-123">Enter **TPTDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="585ec-124">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="585ec-124">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="585ec-125">Создание модели</span><span class="sxs-lookup"><span data-stu-id="585ec-125">Create a Model</span></span>

-   <span data-ttu-id="585ec-126">Щелкните правой кнопкой мыши проект в обозревателе решений и выберите **Add -&gt; новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="585ec-126">Right-click the project in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="585ec-127">Выберите **данных** меню слева, а затем выберите **ADO.NET Entity Data Model** в области шаблонов.</span><span class="sxs-lookup"><span data-stu-id="585ec-127">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="585ec-128">Введите **TPTModel.edmx** имя файла, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="585ec-128">Enter **TPTModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="585ec-129">В диалоговом окне Выбор содержимого модели, выберите ** Создать из базы данных ** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="585ec-129">In the Choose Model Contents dialog box, select** Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="585ec-130">Нажмите кнопку **новое подключение**.</span><span class="sxs-lookup"><span data-stu-id="585ec-130">Click **New Connection**.</span></span>
    <span data-ttu-id="585ec-131">В диалоговом окне Свойства соединения, введите имя сервера (например, **(localdb)\\mssqllocaldb**) выберите метод проверки подлинности, тип **School** для имени базы данных, а затем Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="585ec-131">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="585ec-132">В диалоговом окне Выбор подключения к базе данных обновляется параметр подключения базы данных.</span><span class="sxs-lookup"><span data-stu-id="585ec-132">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="585ec-133">В диалоговом окне Выбор объектов базы данных в узле таблицы выберите **отдел**, **курс, OnlineCourse и OnsiteCourse** таблиц.</span><span class="sxs-lookup"><span data-stu-id="585ec-133">In the Choose Your Database Objects dialog box, under the Tables node, select the **Department**, **Course, OnlineCourse, and OnsiteCourse** tables.</span></span>
-   <span data-ttu-id="585ec-134">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="585ec-134">Click **Finish**.</span></span>

<span data-ttu-id="585ec-135">Конструктор сущностей, который предоставляет область конструктора для изменения модели, отображается.</span><span class="sxs-lookup"><span data-stu-id="585ec-135">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="585ec-136">Все объекты, которые выбраны в диалоговом окне Выбор объектов базы данных добавляются в модель.</span><span class="sxs-lookup"><span data-stu-id="585ec-136">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

## <a name="implement-table-per-type-inheritance"></a><span data-ttu-id="585ec-137">Реализация наследования таблица на тип</span><span class="sxs-lookup"><span data-stu-id="585ec-137">Implement Table-per-Type Inheritance</span></span>

-   <span data-ttu-id="585ec-138">В области конструктора щелкните правой кнопкой мыши **OnlineCourse** типа сущности и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="585ec-138">On the design surface, right-click the **OnlineCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="585ec-139">В **свойства** окна, задайте для свойства Type базы **курс**.</span><span class="sxs-lookup"><span data-stu-id="585ec-139">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="585ec-140">Щелкните правой кнопкой мыши **OnsiteCourse** типа сущности и выберите **свойства**.</span><span class="sxs-lookup"><span data-stu-id="585ec-140">Right-click the **OnsiteCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="585ec-141">В **свойства** окна, задайте для свойства Type базы **курс**.</span><span class="sxs-lookup"><span data-stu-id="585ec-141">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="585ec-142">Щелкните правой кнопкой мыши ассоциацию (линию) между **OnlineCourse** и **курс** типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="585ec-142">Right-click the association (the line) between the **OnlineCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="585ec-143">Выберите **удалить из модели**.</span><span class="sxs-lookup"><span data-stu-id="585ec-143">Select **Delete from Model**.</span></span>
-   <span data-ttu-id="585ec-144">Щелкните правой кнопкой мыши связь между **OnsiteCourse** и **курс** типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="585ec-144">Right-click the association between the **OnsiteCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="585ec-145">Выберите **удалить из модели**.</span><span class="sxs-lookup"><span data-stu-id="585ec-145">Select **Delete from Model**.</span></span>

<span data-ttu-id="585ec-146">Теперь удалим **CourseID** свойства из **OnlineCourse** и **OnsiteCourse** так, как эти классы наследуют **CourseID** из **курс** базовый тип.</span><span class="sxs-lookup"><span data-stu-id="585ec-146">We will now delete the **CourseID** property from **OnlineCourse** and **OnsiteCourse** because these classes inherit **CourseID** from the **Course** base type.</span></span>

-   <span data-ttu-id="585ec-147">Щелкните правой кнопкой мыши **CourseID** свойство **OnlineCourse** тип сущности, а затем выберите **удалить из модели**.</span><span class="sxs-lookup"><span data-stu-id="585ec-147">Right-click the **CourseID** property of the **OnlineCourse** entity type, and then select **Delete from Model**.</span></span>
-   <span data-ttu-id="585ec-148">Щелкните правой кнопкой мыши **CourseID** свойство **OnsiteCourse** тип сущности, а затем выберите **удалить из модели**</span><span class="sxs-lookup"><span data-stu-id="585ec-148">Right-click the **CourseID** property of the **OnsiteCourse** entity type, and then select **Delete from Model**</span></span>
-   <span data-ttu-id="585ec-149">Теперь наследование типа «одна таблица на тип» реализовано.</span><span class="sxs-lookup"><span data-stu-id="585ec-149">Table-per-type inheritance is now implemented.</span></span>

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a><span data-ttu-id="585ec-151">Использование модели</span><span class="sxs-lookup"><span data-stu-id="585ec-151">Use the Model</span></span>

<span data-ttu-id="585ec-152">Откройте **Program.cs** файл, в котором **Main** определен метод.</span><span class="sxs-lookup"><span data-stu-id="585ec-152">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="585ec-153">Вставьте следующий код в **Main** функции.</span><span class="sxs-lookup"><span data-stu-id="585ec-153">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="585ec-154">Код выполняет три запроса.</span><span class="sxs-lookup"><span data-stu-id="585ec-154">The code executes three queries.</span></span> <span data-ttu-id="585ec-155">Первый запрос возвращает все **курсы** связанные с указанное подразделение.</span><span class="sxs-lookup"><span data-stu-id="585ec-155">The first query brings back all **Courses** related to the specified department.</span></span> <span data-ttu-id="585ec-156">Второй запрос использует **OfType** метод для возврата **OnlineCourses** связанные с указанное подразделение.</span><span class="sxs-lookup"><span data-stu-id="585ec-156">The second query uses the **OfType** method to return **OnlineCourses** related to the specified department.</span></span> <span data-ttu-id="585ec-157">Возвращает третий запрос **OnsiteCourses**.</span><span class="sxs-lookup"><span data-stu-id="585ec-157">The third query returns **OnsiteCourses**.</span></span>

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
