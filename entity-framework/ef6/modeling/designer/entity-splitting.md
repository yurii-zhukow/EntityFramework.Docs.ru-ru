---
title: Разделение сущностей конструктора — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: ba1895ae491cec909ff88a8784eea82f1876f595
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415265"
---
# <a name="designer-entity-splitting"></a><span data-ttu-id="6436c-102">Разделение сущностей конструктора</span><span class="sxs-lookup"><span data-stu-id="6436c-102">Designer Entity Splitting</span></span>
<span data-ttu-id="6436c-103">В этом пошаговом руководстве показано, как сопоставлять тип сущности с двумя таблицами путем изменения модели с помощью Entity Framework Designer (конструктор EF).</span><span class="sxs-lookup"><span data-stu-id="6436c-103">This walkthrough shows how to map an entity type to two tables by modifying a model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="6436c-104">Сущность можно сопоставить с несколькими таблицами, если в таблицах имеется общий ключ.</span><span class="sxs-lookup"><span data-stu-id="6436c-104">You can map an entity to multiple tables when the tables share a common key.</span></span> <span data-ttu-id="6436c-105">Понятия, применяемые для сопоставления типа сущности с двумя таблицами, легко расширяются для сопоставления типа сущности с более чем двумя таблицами.</span><span class="sxs-lookup"><span data-stu-id="6436c-105">The concepts that apply to mapping an entity type to two tables are easily extended to mapping an entity type to more than two tables.</span></span>

<span data-ttu-id="6436c-106">На следующем рисунке показаны основные окна, используемые при работе с конструктором EF.</span><span class="sxs-lookup"><span data-stu-id="6436c-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![Конструктор EF](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="6436c-108">предварительные требования</span><span class="sxs-lookup"><span data-stu-id="6436c-108">Prerequisites</span></span>

<span data-ttu-id="6436c-109">Visual Studio 2012 или Visual Studio 2010, Ultimate, Premium, Professional или Web Express Edition.</span><span class="sxs-lookup"><span data-stu-id="6436c-109">Visual Studio 2012 or Visual Studio 2010, Ultimate, Premium, Professional, or Web Express edition.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="6436c-110">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="6436c-110">Create the Database</span></span>

<span data-ttu-id="6436c-111">Сервер базы данных, который устанавливается вместе с Visual Studio, отличается в зависимости от версии Visual Studio, которую вы установили:</span><span class="sxs-lookup"><span data-stu-id="6436c-111">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="6436c-112">Если вы используете Visual Studio 2012, то будете создавать базу данных LocalDB.</span><span class="sxs-lookup"><span data-stu-id="6436c-112">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="6436c-113">Если вы используете Visual Studio 2010, вы создадите базу данных SQL Express.</span><span class="sxs-lookup"><span data-stu-id="6436c-113">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="6436c-114">Сначала мы создадим базу данных с двумя таблицами, которые мы будем объединять в одну сущность.</span><span class="sxs-lookup"><span data-stu-id="6436c-114">First we'll create a database with two tables that we are going to combine into a single entity.</span></span>

-   <span data-ttu-id="6436c-115">Запустите Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6436c-115">Open Visual Studio</span></span>
-   <span data-ttu-id="6436c-116">**Просмотр —&gt; обозреватель сервера**</span><span class="sxs-lookup"><span data-stu-id="6436c-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="6436c-117">Щелкните правой кнопкой мыши **подключения к данным —&gt; добавить подключение...**</span><span class="sxs-lookup"><span data-stu-id="6436c-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="6436c-118">Если вы не подключились к базе данных из обозреватель сервера, прежде чем нужно будет выбрать **Microsoft SQL Server** в качестве источника данных</span><span class="sxs-lookup"><span data-stu-id="6436c-118">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="6436c-119">Подключение к LocalDB или SQL Express в зависимости от того, какой из установленных служб</span><span class="sxs-lookup"><span data-stu-id="6436c-119">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="6436c-120">Введите **ентитисплиттинг** в качестве имени базы данных</span><span class="sxs-lookup"><span data-stu-id="6436c-120">Enter **EntitySplitting** as the database name</span></span>
-   <span data-ttu-id="6436c-121">Нажмите кнопку **ОК** , и появится запрос на создание новой базы данных, выберите **Да** .</span><span class="sxs-lookup"><span data-stu-id="6436c-121">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="6436c-122">Теперь новая база данных появится в обозреватель сервера</span><span class="sxs-lookup"><span data-stu-id="6436c-122">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="6436c-123">Если вы используете Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="6436c-123">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="6436c-124">Щелкните правой кнопкой мыши базу данных в обозреватель сервера и выберите **создать запрос** .</span><span class="sxs-lookup"><span data-stu-id="6436c-124">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="6436c-125">Скопируйте следующий SQL в новый запрос, щелкните запрос правой кнопкой мыши и выберите команду **выполнить** .</span><span class="sxs-lookup"><span data-stu-id="6436c-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="6436c-126">Если вы используете Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="6436c-126">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="6436c-127">Выберите **данные —&gt; редактор SQL,&gt; создать соединение с запросом...**</span><span class="sxs-lookup"><span data-stu-id="6436c-127">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="6436c-128">Введите **.\\SQLExpress** в качестве имени сервера и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="6436c-128">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="6436c-129">Выберите базу данных **ентитисплиттинг** в раскрывающемся списке в верхней части редактора запросов.</span><span class="sxs-lookup"><span data-stu-id="6436c-129">Select the **EntitySplitting** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="6436c-130">Скопируйте следующий SQL в новый запрос, щелкните запрос правой кнопкой мыши и выберите команду **выполнить SQL** .</span><span class="sxs-lookup"><span data-stu-id="6436c-130">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

``` SQL
CREATE TABLE [dbo].[Person] (
[PersonId] INT IDENTITY (1, 1) NOT NULL,
[FirstName] NVARCHAR (200) NULL,
[LastName] NVARCHAR (200) NULL,
CONSTRAINT [PK_Person] PRIMARY KEY CLUSTERED ([PersonId] ASC)
);

CREATE TABLE [dbo].[PersonInfo] (
[PersonId] INT NOT NULL,
[Email] NVARCHAR (200) NULL,
[Phone] NVARCHAR (50) NULL,
CONSTRAINT [PK_PersonInfo] PRIMARY KEY CLUSTERED ([PersonId] ASC),
CONSTRAINT [FK_Person_PersonInfo] FOREIGN KEY ([PersonId]) REFERENCES [dbo].[Person] ([PersonId]) ON DELETE CASCADE
);
```

## <a name="create-the-project"></a><span data-ttu-id="6436c-131">Создание проекта</span><span class="sxs-lookup"><span data-stu-id="6436c-131">Create the Project</span></span>

-   <span data-ttu-id="6436c-132">В меню **Файл** укажите **Создать**, затем нажмите **Проект**.</span><span class="sxs-lookup"><span data-stu-id="6436c-132">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="6436c-133">В левой области щелкните **Visual C\#** , а затем выберите шаблон **консольное приложение** .</span><span class="sxs-lookup"><span data-stu-id="6436c-133">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="6436c-134">Введите **мапентититотаблессампле** в качестве имени проекта и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6436c-134">Enter **MapEntityToTablesSample** as the name of the project and click **OK**.</span></span>
-   <span data-ttu-id="6436c-135">Если будет предложено сохранить SQL запрос, созданный в первом разделе, нажмите кнопку **нет** .</span><span class="sxs-lookup"><span data-stu-id="6436c-135">Click **No** if prompted to save the SQL query created in the first section.</span></span>

## <a name="create-a-model-based-on-the-database"></a><span data-ttu-id="6436c-136">Создание модели на основе базы данных</span><span class="sxs-lookup"><span data-stu-id="6436c-136">Create a Model based on the Database</span></span>

-   <span data-ttu-id="6436c-137">Щелкните правой кнопкой мыши имя проекта в обозреватель решений, наведите указатель на пункт **Добавить**и выберите пункт **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="6436c-137">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="6436c-138">Выберите **данные** в меню слева, а затем выберите **ADO.NET EDM** в области Шаблоны.</span><span class="sxs-lookup"><span data-stu-id="6436c-138">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="6436c-139">Введите **мапентититотаблесмодел. EDMX** в поле имя файла и нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="6436c-139">Enter **MapEntityToTablesModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="6436c-140">В диалоговом окне Выбор содержимого модели выберите пункт **создать из базы данных**, а затем нажмите кнопку **Далее.**</span><span class="sxs-lookup"><span data-stu-id="6436c-140">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="6436c-141">Выберите подключение **ентитисплиттинг** в раскрывающемся списке и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="6436c-141">Select the **EntitySplitting** connection from the drop down and click **Next**.</span></span>
-   <span data-ttu-id="6436c-142">В диалоговом окне Выбор объектов базы данных установите флажок рядом с узлом **таблицы** .</span><span class="sxs-lookup"><span data-stu-id="6436c-142">In the Choose Your Database Objects dialog box, check the box next to the **Tables** node.</span></span>
    <span data-ttu-id="6436c-143">Это приведет к добавлению всех таблиц из базы данных **ентитисплиттинг** в модель.</span><span class="sxs-lookup"><span data-stu-id="6436c-143">This will add all the tables from the **EntitySplitting** database to the model.</span></span>
-   <span data-ttu-id="6436c-144">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="6436c-144">Click **Finish**.</span></span>

<span data-ttu-id="6436c-145">Отобразится Entity Designer, предоставляющий область конструктора для редактирования модели.</span><span class="sxs-lookup"><span data-stu-id="6436c-145">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-an-entity-to-two-tables"></a><span data-ttu-id="6436c-146">Сопоставьте сущность с двумя таблицами</span><span class="sxs-lookup"><span data-stu-id="6436c-146">Map an Entity to Two Tables</span></span>

<span data-ttu-id="6436c-147">На этом шаге мы будем обновлять тип сущности **Person** , чтобы объединить данные из таблиц **Person** и **персонинфо** .</span><span class="sxs-lookup"><span data-stu-id="6436c-147">In this step we will update the **Person** entity type to combine data from the **Person** and **PersonInfo** tables.</span></span>

-   <span data-ttu-id="6436c-148">Выберите  **электронной почты** и свойства **телефона** сущности **персонинфо **и нажмите клавиши **CTRL + X** .</span><span class="sxs-lookup"><span data-stu-id="6436c-148">Select the **Email** and **Phone** properties of the **PersonInfo **entity and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="6436c-149">Выберите сущность **Person **и нажмите клавиши **CTRL + V** .</span><span class="sxs-lookup"><span data-stu-id="6436c-149">Select the **Person **entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="6436c-150">В области конструктора выберите сущность **персонинфо** и нажмите кнопку **Delete (удалить** ) на клавиатуре.</span><span class="sxs-lookup"><span data-stu-id="6436c-150">On the design surface, select the **PersonInfo** entity and press **Delete** button on the keyboard.</span></span>
-   <span data-ttu-id="6436c-151">Нажмите кнопку **нет** при появлении запроса на удаление таблицы **персонинфо** из модели, мы будем сопоставлять ее с сущностью **Person** .</span><span class="sxs-lookup"><span data-stu-id="6436c-151">Click **No** when asked if you want to remove the **PersonInfo** table from the model, we are about to map it to the **Person** entity.</span></span>

    ![Удаление таблиц](~/ef6/media/deletetables.png)

<span data-ttu-id="6436c-153">Для дальнейших действий требуется окно **сведения о сопоставлении** .</span><span class="sxs-lookup"><span data-stu-id="6436c-153">The next steps require the **Mapping Details** window.</span></span> <span data-ttu-id="6436c-154">Если это окно не отображается, щелкните правой кнопкой мыши область конструктора и выберите пункт **сведения о сопоставлении**.</span><span class="sxs-lookup"><span data-stu-id="6436c-154">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="6436c-155">Выберите тип сущности **Person** и нажмите кнопку **&lt;добавить таблицу или представление&gt;**  в окне **сведения о сопоставлении** .</span><span class="sxs-lookup"><span data-stu-id="6436c-155">Select the **Person** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="6436c-156">Выберите **персонинфо ** из раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="6436c-156">Select **PersonInfo ** from the drop-down list.</span></span>
    <span data-ttu-id="6436c-157">Окно **сведения о Сопоставлении** обновляется с сопоставлением столбцов по умолчанию, но для нашего сценария это нормально.</span><span class="sxs-lookup"><span data-stu-id="6436c-157">The **Mapping Details** window is updated with default column mappings, these are fine for our scenario.</span></span>

<span data-ttu-id="6436c-158">Тип сущности **person** теперь сопоставлен с таблицами **Person** и **персонинфо** .</span><span class="sxs-lookup"><span data-stu-id="6436c-158">The **Person** entity type is now mapped to the **Person** and **PersonInfo** tables.</span></span>

![Сопоставление 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a><span data-ttu-id="6436c-160">Использование модели</span><span class="sxs-lookup"><span data-stu-id="6436c-160">Use the Model</span></span>

-   <span data-ttu-id="6436c-161">Вставьте следующий код в метод Main.</span><span class="sxs-lookup"><span data-stu-id="6436c-161">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new EntitySplittingEntities())
    {
        var person = new Person
        {
            FirstName = "John",
            LastName = "Doe",
            Email = "john@example.com",
            Phone = "555-555-5555"
        };

        context.People.Add(person);
        context.SaveChanges();

        foreach (var item in context.People)
        {
            Console.WriteLine(item.FirstName);
        }
    }
```

-   <span data-ttu-id="6436c-162">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="6436c-162">Compile and run the application.</span></span>

<span data-ttu-id="6436c-163">Следующие инструкции T-SQL были выполнены в базе данных в результате выполнения этого приложения.</span><span class="sxs-lookup"><span data-stu-id="6436c-163">The following T-SQL statements were executed against the database as a result of running this application.</span></span> 

-   <span data-ttu-id="6436c-164">Следующие две инструкции **INSERT** были выполнены в результате выполнения context. SaveChanges ().</span><span class="sxs-lookup"><span data-stu-id="6436c-164">The following two **INSERT** statements were executed as a result of executing context.SaveChanges().</span></span> <span data-ttu-id="6436c-165">Они принимают данные из сущности **Person** и разбивают их между таблицами **Person** и **персонинфо** .</span><span class="sxs-lookup"><span data-stu-id="6436c-165">They take the data from the **Person** entity and split it between the **Person** and **PersonInfo** tables.</span></span>

    ![Вставить 1](~/ef6/media/insert1.png)

    ![Вставить 2](~/ef6/media/insert2.png)
-   <span data-ttu-id="6436c-168">Следующая операция **SELECT** была выполнена в результате перечисления пользователей в базе данных.</span><span class="sxs-lookup"><span data-stu-id="6436c-168">The following **SELECT** was executed as a result of enumerating the people in the database.</span></span> <span data-ttu-id="6436c-169">Он сочетает данные из таблицы **Person** и **персонинфо** .</span><span class="sxs-lookup"><span data-stu-id="6436c-169">It combines the data from the **Person** and **PersonInfo** table.</span></span>

    ![Выберите пункт](~/ef6/media/select.png)
