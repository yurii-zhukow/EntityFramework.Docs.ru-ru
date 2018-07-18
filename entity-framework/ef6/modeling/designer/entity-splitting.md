---
title: Разделение конструктора сущностей - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
caps.latest.revision: 3
ms.openlocfilehash: 386b739363fb78641d5ebd8130fd19008bc8c9f6
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122265"
---
# <a name="designer-entity-splitting"></a><span data-ttu-id="8f20a-102">Разбиение конструктора сущностей</span><span class="sxs-lookup"><span data-stu-id="8f20a-102">Designer Entity Splitting</span></span>
<span data-ttu-id="8f20a-103">В этом пошаговом руководстве показано, как сопоставить тип сущности с двумя таблицами путем изменения модели с Entity Framework Designer (конструктор EF).</span><span class="sxs-lookup"><span data-stu-id="8f20a-103">This walkthrough shows how to map an entity type to two tables by modifying a model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="8f20a-104">Сущность можно сопоставить с несколькими таблицами, если в таблицах имеется общий ключ.</span><span class="sxs-lookup"><span data-stu-id="8f20a-104">You can map an entity to multiple tables when the tables share a common key.</span></span> <span data-ttu-id="8f20a-105">Основные понятия, которые применяются для сопоставления типа сущности с двумя таблицами, легко распространить на сопоставление типа сущности более чем с двумя таблицами.</span><span class="sxs-lookup"><span data-stu-id="8f20a-105">The concepts that apply to mapping an entity type to two tables are easily extended to mapping an entity type to more than two tables.</span></span>

<span data-ttu-id="8f20a-106">На следующем рисунке показана основные окна, которые используются при работе с конструктором EF.</span><span class="sxs-lookup"><span data-stu-id="8f20a-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EFDesigner](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="8f20a-108">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="8f20a-108">Prerequisites</span></span>

<span data-ttu-id="8f20a-109">Visual Studio 2012 или Visual Studio 2010 Ultimate, Premium, Professional и Web Express edition.</span><span class="sxs-lookup"><span data-stu-id="8f20a-109">Visual Studio 2012 or Visual Studio 2010, Ultimate, Premium, Professional, or Web Express edition.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="8f20a-110">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="8f20a-110">Create the Database</span></span>

<span data-ttu-id="8f20a-111">Сервер базы данных, который устанавливается вместе с Visual Studio отличается в зависимости от версии Visual Studio, вы установили:</span><span class="sxs-lookup"><span data-stu-id="8f20a-111">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="8f20a-112">Если вы используете Visual Studio 2012, а затем вы создадите базу данных LocalDB.</span><span class="sxs-lookup"><span data-stu-id="8f20a-112">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="8f20a-113">Если вы используете Visual Studio 2010 вы создадите базу данных SQL Express.</span><span class="sxs-lookup"><span data-stu-id="8f20a-113">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="8f20a-114">Сначала мы создадим базу данных с двумя таблицами, которые мы хотим объединить в одну сущность.</span><span class="sxs-lookup"><span data-stu-id="8f20a-114">First we'll create a database with two tables that we are going to combine into a single entity.</span></span>

-   <span data-ttu-id="8f20a-115">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8f20a-115">Open Visual Studio</span></span>
-   <span data-ttu-id="8f20a-116">**Представление —&gt; обозревателя серверов**</span><span class="sxs-lookup"><span data-stu-id="8f20a-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="8f20a-117">Щелкните правой кнопкой мыши **подключения к данным -&gt; добавить соединение...**</span><span class="sxs-lookup"><span data-stu-id="8f20a-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="8f20a-118">Если вы не подключились к базе данных с помощью обозревателя сервера прежде, чем вам нужно будет выбрать **Microsoft SQL Server** как источник данных</span><span class="sxs-lookup"><span data-stu-id="8f20a-118">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="8f20a-119">Подключение к LocalDB или SQL Express, в зависимости от того, какой из них установки</span><span class="sxs-lookup"><span data-stu-id="8f20a-119">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="8f20a-120">Введите **EntitySplitting** имя базы данных</span><span class="sxs-lookup"><span data-stu-id="8f20a-120">Enter **EntitySplitting** as the database name</span></span>
-   <span data-ttu-id="8f20a-121">Выберите **ОК** и вам нужно будет Если вы хотите создать новую базу данных, выберите **Да**</span><span class="sxs-lookup"><span data-stu-id="8f20a-121">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="8f20a-122">Новая база данных появится в обозревателе серверов</span><span class="sxs-lookup"><span data-stu-id="8f20a-122">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="8f20a-123">Если вы используете Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="8f20a-123">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="8f20a-124">Щелкните правой кнопкой мыши, в базе данных в обозревателе серверов и выберите **новый запрос**</span><span class="sxs-lookup"><span data-stu-id="8f20a-124">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="8f20a-125">Скопируйте следующий запрос SQL в новый запрос, а затем щелкните правой кнопкой мыши запрос и выберите **Execute**</span><span class="sxs-lookup"><span data-stu-id="8f20a-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="8f20a-126">Если вы используете Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="8f20a-126">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="8f20a-127">Выберите **тарифный план —&gt; Transact редактор SQL -&gt; новое соединение запроса...**</span><span class="sxs-lookup"><span data-stu-id="8f20a-127">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="8f20a-128">Введите **.\\ SQLEXPRESS** имя сервера и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="8f20a-128">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="8f20a-129">Выберите **EntitySplitting** базы данных в раскрывающемся вниз в верхней части редактора запросов</span><span class="sxs-lookup"><span data-stu-id="8f20a-129">Select the **EntitySplitting** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="8f20a-130">Скопируйте следующий запрос SQL в новый запрос, а затем щелкните правой кнопкой мыши запрос и выберите **Выполнение SQL**</span><span class="sxs-lookup"><span data-stu-id="8f20a-130">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

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

## <a name="create-the-project"></a><span data-ttu-id="8f20a-131">Создание проекта</span><span class="sxs-lookup"><span data-stu-id="8f20a-131">Create the Project</span></span>

-   <span data-ttu-id="8f20a-132">В меню **Файл** выберите пункт **Создать**, а затем команду **Проект**.</span><span class="sxs-lookup"><span data-stu-id="8f20a-132">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="8f20a-133">В левой области щелкните **Visual C\#**, а затем выберите **консольное приложение** шаблона.</span><span class="sxs-lookup"><span data-stu-id="8f20a-133">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="8f20a-134">Введите **MapEntityToTablesSample** как имя проекта и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="8f20a-134">Enter **MapEntityToTablesSample** as the name of the project and click **OK**.</span></span>
-   <span data-ttu-id="8f20a-135">Нажмите кнопку **нет** Если будет предложено сохранить запрос SQL, созданный в первом разделе.</span><span class="sxs-lookup"><span data-stu-id="8f20a-135">Click **No** if prompted to save the SQL query created in the first section.</span></span>

## <a name="create-a-model-based-on-the-database"></a><span data-ttu-id="8f20a-136">Создать модель на основе базы данных</span><span class="sxs-lookup"><span data-stu-id="8f20a-136">Create a Model based on the Database</span></span>

-   <span data-ttu-id="8f20a-137">Щелкните правой кнопкой мыши имя проекта в обозревателе решений, выберите пункт **добавить**, а затем нажмите кнопку **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="8f20a-137">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="8f20a-138">Выберите **данных** меню слева, а затем выберите **ADO.NET Entity Data Model** в области шаблонов.</span><span class="sxs-lookup"><span data-stu-id="8f20a-138">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="8f20a-139">Введите **MapEntityToTablesModel.edmx** имя файла, а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="8f20a-139">Enter **MapEntityToTablesModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="8f20a-140">В диалоговом окне Выбор содержимого модели выберите **создать из базы данных**, а затем нажмите кнопку **Далее.**</span><span class="sxs-lookup"><span data-stu-id="8f20a-140">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="8f20a-141">Выберите **EntitySplitting** подключения из раскрывающегося списка и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="8f20a-141">Select the **EntitySplitting** connection from the drop down and click **Next**.</span></span>
-   <span data-ttu-id="8f20a-142">В диалоговом окне Выбор объектов базы данных, установите флажок рядом с полем **таблиц** узла.</span><span class="sxs-lookup"><span data-stu-id="8f20a-142">In the Choose Your Database Objects dialog box, check the box next to the **Tables** node.</span></span>
    <span data-ttu-id="8f20a-143">Это добавит все таблицы из **EntitySplitting** базы данных в модель.</span><span class="sxs-lookup"><span data-stu-id="8f20a-143">This will add all the tables from the **EntitySplitting** database to the model.</span></span>
-   <span data-ttu-id="8f20a-144">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="8f20a-144">Click **Finish**.</span></span>

<span data-ttu-id="8f20a-145">Конструктор сущностей, который предоставляет область конструктора для изменения модели, отображается.</span><span class="sxs-lookup"><span data-stu-id="8f20a-145">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-an-entity-to-two-tables"></a><span data-ttu-id="8f20a-146">Сопоставить сущности с двумя таблицами</span><span class="sxs-lookup"><span data-stu-id="8f20a-146">Map an Entity to Two Tables</span></span>

<span data-ttu-id="8f20a-147">На этом шаге мы обновим **Person** тип сущности для объединения данных из **Person** и **PersonInfo** таблиц.</span><span class="sxs-lookup"><span data-stu-id="8f20a-147">In this step we will update the **Person** entity type to combine data from the **Person** and **PersonInfo** tables.</span></span>

-   <span data-ttu-id="8f20a-148">Выберите **электронной почты** и **Phone** свойства ** PersonInfo ** сущности и нажмите клавишу **Ctrl + X** ключи.</span><span class="sxs-lookup"><span data-stu-id="8f20a-148">Select the **Email** and **Phone** properties of the **PersonInfo **entity and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="8f20a-149">Выберите ** Person ** сущности и нажмите клавишу **Ctrl + V** ключи.</span><span class="sxs-lookup"><span data-stu-id="8f20a-149">Select the **Person **entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="8f20a-150">В рабочей области конструирования выберите **PersonInfo** сущности и нажмите клавишу **удалить** кнопку на клавиатуре.</span><span class="sxs-lookup"><span data-stu-id="8f20a-150">On the design surface, select the **PersonInfo** entity and press **Delete** button on the keyboard.</span></span>
-   <span data-ttu-id="8f20a-151">Нажмите кнопку **нет** при запросе, если вы хотите удалить **PersonInfo** таблицы из модели, мы сейчас будет сопоставить с **Person** сущности.</span><span class="sxs-lookup"><span data-stu-id="8f20a-151">Click **No** when asked if you want to remove the **PersonInfo** table from the model, we are about to map it to the **Person** entity.</span></span>

    ![DeleteTables](~/ef6/media/deletetables.png)

<span data-ttu-id="8f20a-153">Для следующего шага требуется **сведения о сопоставлении** окна.</span><span class="sxs-lookup"><span data-stu-id="8f20a-153">The next steps require the **Mapping Details** window.</span></span> <span data-ttu-id="8f20a-154">Если это окно не отображается, щелкните правой кнопкой мыши область конструктора и выберите **сведения о сопоставлении**.</span><span class="sxs-lookup"><span data-stu-id="8f20a-154">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="8f20a-155">Выберите **Person** типа сущности и нажмите кнопку **&lt;добавить таблицу или представление&gt;** в **сведения о сопоставлении** окна.</span><span class="sxs-lookup"><span data-stu-id="8f20a-155">Select the **Person** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="8f20a-156">Выберите ** PersonInfo ** из раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="8f20a-156">Select **PersonInfo ** from the drop-down list.</span></span>
    <span data-ttu-id="8f20a-157">**Сведения о сопоставлении** окно обновляется с помощью сопоставления столбцов по умолчанию, но это нормально для нашего сценария.</span><span class="sxs-lookup"><span data-stu-id="8f20a-157">The **Mapping Details** window is updated with default column mappings, these are fine for our scenario.</span></span>

<span data-ttu-id="8f20a-158">**Person** типа сущности теперь сопоставлена с **Person** и **PersonInfo** таблиц.</span><span class="sxs-lookup"><span data-stu-id="8f20a-158">The **Person** entity type is now mapped to the **Person** and **PersonInfo** tables.</span></span>

![Mapping2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a><span data-ttu-id="8f20a-160">Использование модели</span><span class="sxs-lookup"><span data-stu-id="8f20a-160">Use the Model</span></span>

-   <span data-ttu-id="8f20a-161">Вставьте следующий код в методе Main.</span><span class="sxs-lookup"><span data-stu-id="8f20a-161">Paste the following code in the Main method.</span></span>

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

-   <span data-ttu-id="8f20a-162">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="8f20a-162">Compile and run the application.</span></span>

<span data-ttu-id="8f20a-163">Следующие инструкции T-SQL были выполнены в базе данных в результате выполнения этого приложения.</span><span class="sxs-lookup"><span data-stu-id="8f20a-163">The following T-SQL statements were executed against the database as a result of running this application.</span></span> 

-   <span data-ttu-id="8f20a-164">Следующие два **вставить** инструкции были выполнены в результате выполнения контекста. SaveChanges().</span><span class="sxs-lookup"><span data-stu-id="8f20a-164">The following two **INSERT** statements were executed as a result of executing context.SaveChanges().</span></span> <span data-ttu-id="8f20a-165">Они принимают данные из **Person** сущности и разделения между **Person** и **PersonInfo** таблиц.</span><span class="sxs-lookup"><span data-stu-id="8f20a-165">They take the data from the **Person** entity and split it between the **Person** and **PersonInfo** tables.</span></span>

    ![Insert1](~/ef6/media/insert1.png)

    ![Insert2](~/ef6/media/insert2.png)
-   <span data-ttu-id="8f20a-168">Следующие **ВЫБЕРИТЕ** был выполнен в результате перечисление людей в базе данных.</span><span class="sxs-lookup"><span data-stu-id="8f20a-168">The following **SELECT** was executed as a result of enumerating the people in the database.</span></span> <span data-ttu-id="8f20a-169">Он объединяет данные из **Person** и **PersonInfo** таблицы.</span><span class="sxs-lookup"><span data-stu-id="8f20a-169">It combines the data from the **Person** and **PersonInfo** table.</span></span>

    ![Выбрать](~/ef6/media/select.png)
