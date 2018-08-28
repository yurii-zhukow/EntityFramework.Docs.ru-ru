---
title: Самоотслеживающиеся сущности Пошаговое руководство. EF6
author: divega
ms.date: 2016-10-23
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
ms.openlocfilehash: 64ca9ae42df1a1c740131e254b8f80f67b2f9f97
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995425"
---
# <a name="self-tracking-entities-walkthrough"></a><span data-ttu-id="fe18a-102">Самоотслеживающиеся сущности Пошаговое руководство</span><span class="sxs-lookup"><span data-stu-id="fe18a-102">Self-Tracking Entities Walkthrough</span></span>
> [!IMPORTANT]
> <span data-ttu-id="fe18a-103">Больше не рекомендуется использовать шаблон сущностей с самостоятельным отслеживанием.</span><span class="sxs-lookup"><span data-stu-id="fe18a-103">We no longer recommend using the self-tracking-entities template.</span></span> <span data-ttu-id="fe18a-104">Он по-прежнему будет доступен только для поддержки существующих приложений.</span><span class="sxs-lookup"><span data-stu-id="fe18a-104">It will only continue to be available to support existing applications.</span></span> <span data-ttu-id="fe18a-105">Если приложению необходимо работать с отключенными графами сущностей, рассмотрите другие варианты, такие как [отслеживаемые сущности](http://trackableentities.github.io/), которые технологически эквивалентны сущностям с самостоятельным отслеживанием, активно разрабатываемым сообществом. Или остановитесь на написании пользовательского кода с помощью API-интерфейсов отслеживания изменений низкого уровня.</span><span class="sxs-lookup"><span data-stu-id="fe18a-105">If your application requires working with disconnected graphs of entities, consider other alternatives such as [Trackable Entities](http://trackableentities.github.io/), which is a technology similar to Self-Tracking-Entities that is more actively developed by the community, or writing custom code using the low-level change tracking APIs.</span></span>

<span data-ttu-id="fe18a-106">В этом пошаговом руководстве описывается сценарий, в котором службы Windows Communication Foundation (WCF) предоставляет операцию, которая возвращает граф объекта.</span><span class="sxs-lookup"><span data-stu-id="fe18a-106">This walkthrough demonstrates the scenario in which a Windows Communication Foundation (WCF) service exposes an operation that returns an entity graph.</span></span> <span data-ttu-id="fe18a-107">Затем клиентское приложение обрабатывает этот граф и передает изменения операции службы, которая проверяет и сохраняет их в базу данных с помощью Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fe18a-107">Next, a client application manipulates that graph and submits the modifications to a service operation that validates and saves the updates to a database using Entity Framework.</span></span>

<span data-ttu-id="fe18a-108">Перед завершением этого пошагового руководства обязательно прочтите статью [сущностей с самостоятельным отслеживанием](index.md) страницы.</span><span class="sxs-lookup"><span data-stu-id="fe18a-108">Before completing this walkthrough make sure you read the [Self-Tracking Entities](index.md) page.</span></span>

<span data-ttu-id="fe18a-109">В этом пошаговом руководстве выполняются следующие действия.</span><span class="sxs-lookup"><span data-stu-id="fe18a-109">This walkthrough completes the following actions:</span></span>

-   <span data-ttu-id="fe18a-110">Создает базу данных для доступа к.</span><span class="sxs-lookup"><span data-stu-id="fe18a-110">Creates a database to access.</span></span>
-   <span data-ttu-id="fe18a-111">Создает библиотеку классов, которая содержит модель.</span><span class="sxs-lookup"><span data-stu-id="fe18a-111">Creates a class library that contains the model.</span></span>
-   <span data-ttu-id="fe18a-112">Переключений в шаблон генератора сущностей с самостоятельным отслеживанием.</span><span class="sxs-lookup"><span data-stu-id="fe18a-112">Swaps to the Self-Tracking Entity Generator template.</span></span>
-   <span data-ttu-id="fe18a-113">Перемещает классы сущностей в отдельном проекте.</span><span class="sxs-lookup"><span data-stu-id="fe18a-113">Moves the entity classes to a separate project.</span></span>
-   <span data-ttu-id="fe18a-114">Создает службу WCF, которая предоставляет операции для запроса и сохранение объектов.</span><span class="sxs-lookup"><span data-stu-id="fe18a-114">Creates a WCF service that exposes operations to query and save entities.</span></span>
-   <span data-ttu-id="fe18a-115">Создает клиентские приложения (консольное и WPF), которые используют службу.</span><span class="sxs-lookup"><span data-stu-id="fe18a-115">Creates client applications (Console and WPF) that consume the service.</span></span>

<span data-ttu-id="fe18a-116">Мы будем использовать первой базы данных в этом пошаговом руководстве, но те же методы также применяются Model First.</span><span class="sxs-lookup"><span data-stu-id="fe18a-116">We'll use Database First in this walkthrough but the same techniques apply equally to Model First.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="fe18a-117">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="fe18a-117">Pre-Requisites</span></span>

<span data-ttu-id="fe18a-118">Для выполнения этого пошагового руководства требуется последнюю версию Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fe18a-118">To complete this walkthrough you will need a recent version of Visual Studio.</span></span>

## <a name="create-a-database"></a><span data-ttu-id="fe18a-119">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="fe18a-119">Create a Database</span></span>

<span data-ttu-id="fe18a-120">Сервер базы данных, который устанавливается вместе с Visual Studio отличается в зависимости от версии Visual Studio, вы установили:</span><span class="sxs-lookup"><span data-stu-id="fe18a-120">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="fe18a-121">Если вы используете Visual Studio 2012, а затем вы создадите базу данных LocalDB.</span><span class="sxs-lookup"><span data-stu-id="fe18a-121">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="fe18a-122">Если вы используете Visual Studio 2010 вы создадите базу данных SQL Express.</span><span class="sxs-lookup"><span data-stu-id="fe18a-122">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="fe18a-123">Перейдем дальше и создать базу данных.</span><span class="sxs-lookup"><span data-stu-id="fe18a-123">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="fe18a-124">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fe18a-124">Open Visual Studio</span></span>
-   <span data-ttu-id="fe18a-125">**Представление —&gt; обозревателя серверов**</span><span class="sxs-lookup"><span data-stu-id="fe18a-125">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="fe18a-126">Щелкните правой кнопкой мыши **подключения к данным -&gt; добавить соединение...**</span><span class="sxs-lookup"><span data-stu-id="fe18a-126">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="fe18a-127">Если вы не подключились к базе данных с помощью обозревателя сервера прежде, чем вам нужно будет выбрать **Microsoft SQL Server** как источник данных</span><span class="sxs-lookup"><span data-stu-id="fe18a-127">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="fe18a-128">Подключение к LocalDB или SQL Express, в зависимости от того, какой из них установки</span><span class="sxs-lookup"><span data-stu-id="fe18a-128">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="fe18a-129">Введите **STESample** имя базы данных</span><span class="sxs-lookup"><span data-stu-id="fe18a-129">Enter **STESample** as the database name</span></span>
-   <span data-ttu-id="fe18a-130">Выберите **ОК** и вам нужно будет Если вы хотите создать новую базу данных, выберите **Да**</span><span class="sxs-lookup"><span data-stu-id="fe18a-130">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="fe18a-131">Новая база данных появится в обозревателе серверов</span><span class="sxs-lookup"><span data-stu-id="fe18a-131">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="fe18a-132">Если вы используете Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="fe18a-132">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="fe18a-133">Щелкните правой кнопкой мыши, в базе данных в обозревателе серверов и выберите **новый запрос**</span><span class="sxs-lookup"><span data-stu-id="fe18a-133">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="fe18a-134">Скопируйте следующий запрос SQL в новый запрос, а затем щелкните правой кнопкой мыши запрос и выберите **Execute**</span><span class="sxs-lookup"><span data-stu-id="fe18a-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="fe18a-135">Если вы используете Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="fe18a-135">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="fe18a-136">Выберите **тарифный план —&gt; Transact редактор SQL -&gt; новое соединение запроса...**</span><span class="sxs-lookup"><span data-stu-id="fe18a-136">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="fe18a-137">Введите **.\\ SQLEXPRESS** имя сервера и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="fe18a-137">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="fe18a-138">Выберите **STESample** базы данных в раскрывающемся вниз в верхней части редактора запросов</span><span class="sxs-lookup"><span data-stu-id="fe18a-138">Select the **STESample** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="fe18a-139">Скопируйте следующий запрос SQL в новый запрос, а затем щелкните правой кнопкой мыши запрос и выберите **Выполнение SQL**</span><span class="sxs-lookup"><span data-stu-id="fe18a-139">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

``` SQL
    CREATE TABLE [dbo].[Blogs] (
        [BlogId] INT IDENTITY (1, 1) NOT NULL,
        [Name] NVARCHAR (200) NULL,
        [Url]  NVARCHAR (200) NULL,
        CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
    );

    CREATE TABLE [dbo].[Posts] (
        [PostId] INT IDENTITY (1, 1) NOT NULL,
        [Title] NVARCHAR (200) NULL,
        [Content] NTEXT NULL,
        [BlogId] INT NOT NULL,
        CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
        CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
    );

    SET IDENTITY_INSERT [dbo].[Blogs] ON
    INSERT INTO [dbo].[Blogs] ([BlogId], [Name], [Url]) VALUES (1, N'ADO.NET Blog', N'blogs.msdn.com/adonet')
    SET IDENTITY_INSERT [dbo].[Blogs] OFF
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'Intro to EF', N'Interesting stuff...', 1)
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'What is New', N'More interesting stuff...', 1)
```

## <a name="create-the-model"></a><span data-ttu-id="fe18a-140">Создание модели</span><span class="sxs-lookup"><span data-stu-id="fe18a-140">Create the Model</span></span>

<span data-ttu-id="fe18a-141">Мы сначала поместить модели в проект.</span><span class="sxs-lookup"><span data-stu-id="fe18a-141">First up, we need a project to put the model in.</span></span>

-   <span data-ttu-id="fe18a-142">**Файл —&gt; Новинка —&gt; проекта...**</span><span class="sxs-lookup"><span data-stu-id="fe18a-142">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="fe18a-143">Выберите **Visual C\#**  в области слева и затем **библиотеки классов**</span><span class="sxs-lookup"><span data-stu-id="fe18a-143">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="fe18a-144">Введите **STESample** имя и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="fe18a-144">Enter **STESample** as the name and click **OK**</span></span>

<span data-ttu-id="fe18a-145">Теперь мы создадим простую модель в конструкторе EF для доступа к базе данных:</span><span class="sxs-lookup"><span data-stu-id="fe18a-145">Now we'll create a simple model in the EF Designer to access our database:</span></span>

-   <span data-ttu-id="fe18a-146">**Проект -&gt; добавить новый элемент...**</span><span class="sxs-lookup"><span data-stu-id="fe18a-146">**Project -&gt; Add New Item...**</span></span>
-   <span data-ttu-id="fe18a-147">Выберите **данных** в области слева и затем **модель EDM ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="fe18a-147">Select **Data** from the left pane and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="fe18a-148">Введите **BloggingModel** имя и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="fe18a-148">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="fe18a-149">Выберите **создать из базы данных** и нажмите кнопку **Далее**</span><span class="sxs-lookup"><span data-stu-id="fe18a-149">Select **Generate from database** and click **Next**</span></span>
-   <span data-ttu-id="fe18a-150">Введите сведения о подключении для базы данных, созданный в предыдущем разделе</span><span class="sxs-lookup"><span data-stu-id="fe18a-150">Enter the connection information for the database that you created in the previous section</span></span>
-   <span data-ttu-id="fe18a-151">Введите **BloggingContext** как имя строки подключения и нажмите кнопку **Далее**</span><span class="sxs-lookup"><span data-stu-id="fe18a-151">Enter **BloggingContext** as the name for the connection string and click **Next**</span></span>
-   <span data-ttu-id="fe18a-152">Установите флажок рядом с полем **таблиц** и нажмите кнопку **Готово**</span><span class="sxs-lookup"><span data-stu-id="fe18a-152">Check the box next to **Tables** and click **Finish**</span></span>

## <a name="swap-to-ste-code-generation"></a><span data-ttu-id="fe18a-153">Вы сможете перейти в ШАГЕ создания кода</span><span class="sxs-lookup"><span data-stu-id="fe18a-153">Swap to STE Code Generation</span></span>

<span data-ttu-id="fe18a-154">Теперь нам нужно отключить создание кода по умолчанию и замены для сущностей с самостоятельным отслеживанием.</span><span class="sxs-lookup"><span data-stu-id="fe18a-154">Now we need to disable the default code generation and swap to Self-Tracking Entities.</span></span>

### <a name="if-you-are-using-visual-studio-2012"></a><span data-ttu-id="fe18a-155">Если вы используете Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="fe18a-155">If you are using Visual Studio 2012</span></span>

-   <span data-ttu-id="fe18a-156">Разверните **BloggingModel.edmx** в **обозревателе решений** и удалить **BloggingModel.tt** и **BloggingModel.Context.tt** 
     *Это отключит формирование кода по умолчанию*</span><span class="sxs-lookup"><span data-stu-id="fe18a-156">Expand **BloggingModel.edmx** in **Solution Explorer** and delete the **BloggingModel.tt** and **BloggingModel.Context.tt**
*This will disable the default code generation*</span></span>
-   <span data-ttu-id="fe18a-157">Щелкните правой кнопкой мыши пустую область в конструкторе EF и выберите пункт **добавить элемент формирования кода...**</span><span class="sxs-lookup"><span data-stu-id="fe18a-157">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="fe18a-158">Выберите **Online** из левой области и выполните поиск **ТАВИТЬ генератор**</span><span class="sxs-lookup"><span data-stu-id="fe18a-158">Select **Online** from the left pane and search for **STE Generator**</span></span>
-   <span data-ttu-id="fe18a-159">Выберите **ТАВИТЬ генератор для C\#**  шаблона, введите **STETemplate** имя и нажмите кнопку **добавить**</span><span class="sxs-lookup"><span data-stu-id="fe18a-159">Select the **STE Generator for C\#** template, enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="fe18a-160">**STETemplate.tt** и **STETemplate.Context.tt** файлы добавляются как вложенные в файле BloggingModel.edmx</span><span class="sxs-lookup"><span data-stu-id="fe18a-160">The **STETemplate.tt** and **STETemplate.Context.tt** files are added nested under the BloggingModel.edmx file</span></span>

### <a name="if-you-are-using-visual-studio-2010"></a><span data-ttu-id="fe18a-161">Если вы используете Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="fe18a-161">If you are using Visual Studio 2010</span></span>

-   <span data-ttu-id="fe18a-162">Щелкните правой кнопкой мыши пустую область в конструкторе EF и выберите пункт **добавить элемент формирования кода...**</span><span class="sxs-lookup"><span data-stu-id="fe18a-162">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="fe18a-163">Выберите **кода** в области слева и затем **генератора сущностей с самостоятельным отслеживанием ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="fe18a-163">Select **Code** from the left pane and then **ADO.NET Self-Tracking Entity Generator**</span></span>
-   <span data-ttu-id="fe18a-164">Введите **STETemplate** имя и нажмите кнопку **добавить**</span><span class="sxs-lookup"><span data-stu-id="fe18a-164">Enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="fe18a-165">**STETemplate.tt** и **STETemplate.Context.tt** файлы добавляются непосредственно в проект</span><span class="sxs-lookup"><span data-stu-id="fe18a-165">The **STETemplate.tt** and **STETemplate.Context.tt** files are added directly to your project</span></span>

## <a name="move-entity-types-into-separate-project"></a><span data-ttu-id="fe18a-166">Переместите типы сущностей в отдельном проекте</span><span class="sxs-lookup"><span data-stu-id="fe18a-166">Move Entity Types into Separate Project</span></span>

<span data-ttu-id="fe18a-167">Для использования сущностей с самостоятельным отслеживанием в клиентском приложении требуется доступ к классам сущностей, созданные из нашей модели.</span><span class="sxs-lookup"><span data-stu-id="fe18a-167">To use Self-Tracking Entities our client application needs access to the entity classes generated from our model.</span></span> <span data-ttu-id="fe18a-168">Так как мы не хотим предоставлять всю модель в клиентское приложение мы собираемся переместить классы сущностей в отдельном проекте.</span><span class="sxs-lookup"><span data-stu-id="fe18a-168">Because we don't want to expose the whole model to the client application we're going to move the entity classes into a separate project.</span></span>

<span data-ttu-id="fe18a-169">Первым шагом является прекращается создание классов сущностей в существующий проект:</span><span class="sxs-lookup"><span data-stu-id="fe18a-169">The first step is to stop generating entity classes in the existing project:</span></span>

-   <span data-ttu-id="fe18a-170">Щелкните правой кнопкой мыши **STETemplate.tt** в **обозревателе решений** и выберите **свойства**</span><span class="sxs-lookup"><span data-stu-id="fe18a-170">Right-click on **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="fe18a-171">В **свойства** очистить окно **TextTemplatingFileGenerator** из **CustomTool** свойство</span><span class="sxs-lookup"><span data-stu-id="fe18a-171">In the **Properties** window clear **TextTemplatingFileGenerator** from the **CustomTool** property</span></span>
-   <span data-ttu-id="fe18a-172">Разверните **STETemplate.tt** в **обозревателе решений** и удалить все файлы, вложенным в него</span><span class="sxs-lookup"><span data-stu-id="fe18a-172">Expand **STETemplate.tt** in **Solution Explorer** and delete all files nested under it</span></span>

<span data-ttu-id="fe18a-173">Затем мы собираемся добавить новый проект и создания классов сущностей в нем</span><span class="sxs-lookup"><span data-stu-id="fe18a-173">Next, we are going to add a new project and generate the entity classes in it</span></span>

-   <span data-ttu-id="fe18a-174">**Файл —&gt; Add -&gt; проекта...**</span><span class="sxs-lookup"><span data-stu-id="fe18a-174">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="fe18a-175">Выберите **Visual C\#**  в области слева и затем **библиотеки классов**</span><span class="sxs-lookup"><span data-stu-id="fe18a-175">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="fe18a-176">Введите **STESample.Entities** имя и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="fe18a-176">Enter **STESample.Entities** as the name and click **OK**</span></span>
-   <span data-ttu-id="fe18a-177">**Проект -&gt; добавить существующий элемент...**</span><span class="sxs-lookup"><span data-stu-id="fe18a-177">**Project -&gt; Add Existing Item...**</span></span>
-   <span data-ttu-id="fe18a-178">Перейдите к **STESample** папки проекта</span><span class="sxs-lookup"><span data-stu-id="fe18a-178">Navigate to the **STESample** project folder</span></span>
-   <span data-ttu-id="fe18a-179">Выберите для просмотра **все файлы (\*.\*)**</span><span class="sxs-lookup"><span data-stu-id="fe18a-179">Select to view **All Files (\*.\*)**</span></span>
-   <span data-ttu-id="fe18a-180">Выберите **STETemplate.tt** файла</span><span class="sxs-lookup"><span data-stu-id="fe18a-180">Select the **STETemplate.tt** file</span></span>
-   <span data-ttu-id="fe18a-181">Щелкните стрелку раскрывающегося списка рядом **добавить** и выберите **добавить как ссылку**</span><span class="sxs-lookup"><span data-stu-id="fe18a-181">Click on the drop down arrow next to the **Add** button and select **Add As Link**</span></span>

    ![AddLinkedTemplate](~/ef6/media/addlinkedtemplate.png)

<span data-ttu-id="fe18a-183">Кроме того, мы собираемся убедитесь, что классы сущностей создаются в том же пространстве имен, как контекст.</span><span class="sxs-lookup"><span data-stu-id="fe18a-183">We're also going to make sure the entity classes get generated in the same namespace as the context.</span></span> <span data-ttu-id="fe18a-184">Это просто уменьшает число с помощью инструкций, которые необходимо добавить на протяжении нашего приложения.</span><span class="sxs-lookup"><span data-stu-id="fe18a-184">This just reduces the number of using statements we need to add throughout our application.</span></span>

-   <span data-ttu-id="fe18a-185">Щелкните правой кнопкой мыши на связанном **STETemplate.tt** в **обозревателе решений** и выберите **свойства**</span><span class="sxs-lookup"><span data-stu-id="fe18a-185">Right-click on the linked **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="fe18a-186">В **свойства** задайте **пространство имен CustomTool** для **STESample**</span><span class="sxs-lookup"><span data-stu-id="fe18a-186">In the **Properties** window set **Custom Tool Namespace** to **STESample**</span></span>

<span data-ttu-id="fe18a-187">Код, созданный с помощью шаблона ТАВИТЬ будет нужна ссылка на **System.Runtime.Serialization** для компиляции.</span><span class="sxs-lookup"><span data-stu-id="fe18a-187">The code generated by the STE template will need a reference to **System.Runtime.Serialization** in order to compile.</span></span> <span data-ttu-id="fe18a-188">Эта библиотека нужна для WCF **DataContract** и **DataMember** атрибутов, используемых для сериализуемых типов сущности.</span><span class="sxs-lookup"><span data-stu-id="fe18a-188">This library is needed for the WCF **DataContract** and **DataMember** attributes that are used on the serializable entity types.</span></span>

-   <span data-ttu-id="fe18a-189">Щелкните правой кнопкой мыши **STESample.Entities** в проекте **обозревателе решений** и выберите **добавить ссылку...**</span><span class="sxs-lookup"><span data-stu-id="fe18a-189">Right click on the **STESample.Entities** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="fe18a-190">В Visual Studio 2012, установите флажок рядом с полем **System.Runtime.Serialization** и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="fe18a-190">In Visual Studio 2012 - check the box next to **System.Runtime.Serialization** and click **OK**</span></span>
    -   <span data-ttu-id="fe18a-191">В Visual Studio 2010 — выберите **System.Runtime.Serialization** и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="fe18a-191">In Visual Studio 2010 - select **System.Runtime.Serialization** and click **OK**</span></span>

<span data-ttu-id="fe18a-192">Наконец проект с наш контекст, в нем будет нужна ссылка на типы сущностей.</span><span class="sxs-lookup"><span data-stu-id="fe18a-192">Finally, the project with our context in it will need a reference to the entity types.</span></span>

-   <span data-ttu-id="fe18a-193">Щелкните правой кнопкой мыши **STESample** в проекте **обозревателе решений** и выберите **добавить ссылку...**</span><span class="sxs-lookup"><span data-stu-id="fe18a-193">Right click on the **STESample** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="fe18a-194">В Visual Studio 2012 – выберите **решение** в левой области, установите флажок рядом с полем **STESample.Entities** и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="fe18a-194">In Visual Studio 2012 - select **Solution** from the left pane, check the box next to **STESample.Entities** and click **OK**</span></span>
    -   <span data-ttu-id="fe18a-195">В Visual Studio 2010 — выберите **проекты** выберите **STESample.Entities** и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="fe18a-195">In Visual Studio 2010 - select the **Projects** tab, select **STESample.Entities** and click **OK**</span></span>

>[!NOTE]
> <span data-ttu-id="fe18a-196">Другой вариант перемещения типы сущностей в отдельном проекте — перемещение файла шаблона, а не связывая его из расположения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fe18a-196">Another option for moving the entity types to a separate project is to move the template file, rather than linking it from its default location.</span></span> <span data-ttu-id="fe18a-197">После этого необходимо обновить **inputFile** переменных в шаблоне, чтобы указать относительный путь в EDMX-файл (в этом примере, которая была бы **.. \\BloggingModel.edmx**).</span><span class="sxs-lookup"><span data-stu-id="fe18a-197">If you do this, you will need to update the **inputFile** variable in the template to provide the relative path to the edmx file (in this example that would be **..\\BloggingModel.edmx**).</span></span>

## <a name="create-a-wcf-service"></a><span data-ttu-id="fe18a-198">Создание службы WCF</span><span class="sxs-lookup"><span data-stu-id="fe18a-198">Create a WCF Service</span></span>

<span data-ttu-id="fe18a-199">Пришло время добавить службу WCF для предоставления наши данные, мы начнем с создания проекта.</span><span class="sxs-lookup"><span data-stu-id="fe18a-199">Now it's time to add a WCF Service to expose our data, we'll start by creating the project.</span></span>

-   <span data-ttu-id="fe18a-200">**Файл —&gt; Add -&gt; проекта...**</span><span class="sxs-lookup"><span data-stu-id="fe18a-200">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="fe18a-201">Выберите **Visual C\#**  в области слева и затем **приложение службы WCF**</span><span class="sxs-lookup"><span data-stu-id="fe18a-201">Select **Visual C\#** from the left pane and then **WCF Service Application**</span></span>
-   <span data-ttu-id="fe18a-202">Введите **STESample.Service** имя и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="fe18a-202">Enter **STESample.Service** as the name and click **OK**</span></span>
-   <span data-ttu-id="fe18a-203">Добавьте ссылку на **System.Data.Entity** сборки</span><span class="sxs-lookup"><span data-stu-id="fe18a-203">Add a reference to the **System.Data.Entity** assembly</span></span>
-   <span data-ttu-id="fe18a-204">Добавьте ссылку на **STESample** и **STESample.Entities** проектов</span><span class="sxs-lookup"><span data-stu-id="fe18a-204">Add a reference to the **STESample** and **STESample.Entities** projects</span></span>

<span data-ttu-id="fe18a-205">Нам нужно скопировать строку подключения EF для этого проекта, таким образом, найденных во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="fe18a-205">We need to copy the EF connection string to this project so that it is found at runtime.</span></span>

-   <span data-ttu-id="fe18a-206">Откройте **App.Config** файл ** STESample ** проект и скопировать **connectionStrings** элемент</span><span class="sxs-lookup"><span data-stu-id="fe18a-206">Open the **App.Config** file for the **STESample **project and copy the **connectionStrings** element</span></span>
-   <span data-ttu-id="fe18a-207">Вставить **connectionStrings** элемент как дочерний элемент элемента **конфигурации** элемент **Web.Config** файл **STESample.Service** проекта</span><span class="sxs-lookup"><span data-stu-id="fe18a-207">Paste the **connectionStrings** element as a child element of the **configuration** element of the **Web.Config** file in the **STESample.Service** project</span></span>

<span data-ttu-id="fe18a-208">Пришло время для реализации фактической службы.</span><span class="sxs-lookup"><span data-stu-id="fe18a-208">Now it's time to implement the actual service.</span></span>

-   <span data-ttu-id="fe18a-209">Откройте **IService1.cs** и замените его содержимое следующим кодом</span><span class="sxs-lookup"><span data-stu-id="fe18a-209">Open **IService1.cs** and replace the contents with the following code</span></span>

``` csharp
    using System.Collections.Generic;
    using System.ServiceModel;

    namespace STESample.Service
    {
        [ServiceContract]
        public interface IService1
        {
            [OperationContract]
            List<Blog> GetBlogs();

            [OperationContract]
            void UpdateBlog(Blog blog);
        }
    }
```

-   <span data-ttu-id="fe18a-210">Откройте **Service1.svc** и замените его содержимое следующим кодом</span><span class="sxs-lookup"><span data-stu-id="fe18a-210">Open **Service1.svc** and replace the contents with the following code</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.Linq;

    namespace STESample.Service
    {
        public class Service1 : IService1
        {
            /// <summary>
            /// Gets all the Blogs and related Posts.
            /// </summary>
            public List<Blog> GetBlogs()
            {
                using (BloggingContext context = new BloggingContext())
                {
                    return context.Blogs.Include("Posts").ToList();
                }
            }

            /// <summary>
            /// Updates Blog and its related Posts.
            /// </summary>
            public void UpdateBlog(Blog blog)
            {
                using (BloggingContext context = new BloggingContext())
                {
                    try
                    {
                        // TODO: Perform validation on the updated order before applying the changes.

                        // The ApplyChanges method examines the change tracking information
                        // contained in the graph of self-tracking entities to infer the set of operations
                        // that need to be performed to reflect the changes in the database.
                        context.Blogs.ApplyChanges(blog);
                        context.SaveChanges();

                    }
                    catch (UpdateException)
                    {
                        // To avoid propagating exception messages that contain sensitive data to the client tier
                        // calls to ApplyChanges and SaveChanges should be wrapped in exception handling code.
                        throw new InvalidOperationException("Failed to update. Try your request again.");
                    }
                }
            }        
        }
    }
```

## <a name="consume-the-service-from-a-console-application"></a><span data-ttu-id="fe18a-211">Использовать службу в консольном приложении</span><span class="sxs-lookup"><span data-stu-id="fe18a-211">Consume the Service from a Console Application</span></span>

<span data-ttu-id="fe18a-212">Давайте создадим консольное приложение, которое использует нашей службы.</span><span class="sxs-lookup"><span data-stu-id="fe18a-212">Let's create a console application that uses our service.</span></span>

-   <span data-ttu-id="fe18a-213">**Файл —&gt; Новинка —&gt; проекта...**</span><span class="sxs-lookup"><span data-stu-id="fe18a-213">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="fe18a-214">Выберите **Visual C\#**  в области слева и затем **консольного приложения**</span><span class="sxs-lookup"><span data-stu-id="fe18a-214">Select **Visual C\#** from the left pane and then **Console Application**</span></span>
-   <span data-ttu-id="fe18a-215">Введите **STESample.ConsoleTest** имя и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="fe18a-215">Enter **STESample.ConsoleTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="fe18a-216">Добавьте ссылку на **STESample.Entities** проекта</span><span class="sxs-lookup"><span data-stu-id="fe18a-216">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="fe18a-217">Нам нужно ссылку на службу в нашу службу WCF</span><span class="sxs-lookup"><span data-stu-id="fe18a-217">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="fe18a-218">Щелкните правой кнопкой мыши **STESample.ConsoleTest** в проекте **обозревателе решений** и выберите **добавить ссылку на службу...**</span><span class="sxs-lookup"><span data-stu-id="fe18a-218">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="fe18a-219">Нажмите кнопку **обнаружение**</span><span class="sxs-lookup"><span data-stu-id="fe18a-219">Click **Discover**</span></span>
-   <span data-ttu-id="fe18a-220">Введите **BloggingService** пространства имен и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="fe18a-220">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="fe18a-221">Теперь можно написать код для использования службы.</span><span class="sxs-lookup"><span data-stu-id="fe18a-221">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="fe18a-222">Откройте **Program.cs** и замените его содержимое следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="fe18a-222">Open **Program.cs** and replace the contents with the following code.</span></span>

``` csharp
    using STESample.ConsoleTest.BloggingService;
    using System;
    using System.Linq;

    namespace STESample.ConsoleTest
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Print out the data before we change anything
                Console.WriteLine("Initial Data:");
                DisplayBlogsAndPosts();

                // Add a new Blog and some Posts
                AddBlogAndPost();
                Console.WriteLine("After Adding:");
                DisplayBlogsAndPosts();

                // Modify the Blog and one of its Posts
                UpdateBlogAndPost();
                Console.WriteLine("After Update:");
                DisplayBlogsAndPosts();

                // Delete the Blog and its Posts
                DeleteBlogAndPost();
                Console.WriteLine("After Delete:");
                DisplayBlogsAndPosts();

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            static void DisplayBlogsAndPosts()
            {
                using (var service = new Service1Client())
                {
                    // Get all Blogs (and Posts) from the service
                    // and print them to the console
                    var blogs = service.GetBlogs();
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(blog.Name);
                        foreach (var post in blog.Posts)
                        {
                            Console.WriteLine(" - {0}", post.Title);
                        }
                    }
                }

                Console.WriteLine();
                Console.WriteLine();
            }

            static void AddBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Create a new Blog with a couple of Posts
                    var newBlog = new Blog
                    {
                        Name = "The New Blog",
                        Posts =
                        {
                            new Post { Title = "Welcome to the new blog"},
                            new Post { Title = "What's new on the new blog"}
                        }
                    };

                    // Save the changes using the service
                    service.UpdateBlog(newBlog);
                }
            }

            static void UpdateBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The New Blog
                    var blog = blogs.First(b => b.Name == "The New Blog");

                    // Update the Blogs name
                    blog.Name = "The Not-So-New Blog";

                    // Update one of the related posts
                    blog.Posts.First().Content = "Some interesting content...";

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }

            static void DeleteBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The Not-So-New Blog
                    var blog = blogs.First(b => b.Name == "The Not-So-New Blog");

                    // Mark all related Posts for deletion
                    // We need to call ToList because each Post will be removed from the
                    // Posts collection when we call MarkAsDeleted
                    foreach (var post in blog.Posts.ToList())
                    {
                        post.MarkAsDeleted();
                    }

                    // Mark the Blog for deletion
                    blog.MarkAsDeleted();

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }
        }
    }
```

<span data-ttu-id="fe18a-223">Теперь вы можете запустить приложение и увидеть, как оно работает:</span><span class="sxs-lookup"><span data-stu-id="fe18a-223">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="fe18a-224">Щелкните правой кнопкой мыши **STESample.ConsoleTest** в проекте **обозревателе решений** и выберите **«Отладка» —&gt; запустить новый экземпляр**</span><span class="sxs-lookup"><span data-stu-id="fe18a-224">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>

<span data-ttu-id="fe18a-225">При запуске приложения вы увидите следующие выходные данные.</span><span class="sxs-lookup"><span data-stu-id="fe18a-225">You'll see the following output when the application executes.</span></span>

```
Initial Data:
ADO.NET Blog
- Intro to EF
- What is New

After Adding:
ADO.NET Blog
- Intro to EF
- What is New
The New Blog
- Welcome to the new blog
- What's new on the new blog

After Update:
ADO.NET Blog
- Intro to EF
- What is New
The Not-So-New Blog
- Welcome to the new blog
- What's new on the new blog

After Delete:
ADO.NET Blog
- Intro to EF
- What is New

Press any key to exit...
```

## <a name="consume-the-service-from-a-wpf-application"></a><span data-ttu-id="fe18a-226">Использование службы в приложении WPF</span><span class="sxs-lookup"><span data-stu-id="fe18a-226">Consume the Service from a WPF Application</span></span>

<span data-ttu-id="fe18a-227">Давайте создадим приложение WPF, использующего нашей службы.</span><span class="sxs-lookup"><span data-stu-id="fe18a-227">Let's create a WPF application that uses our service.</span></span>

-   <span data-ttu-id="fe18a-228">**Файл —&gt; Новинка —&gt; проекта...**</span><span class="sxs-lookup"><span data-stu-id="fe18a-228">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="fe18a-229">Выберите **Visual C\#**  в области слева и затем **приложения WPF**</span><span class="sxs-lookup"><span data-stu-id="fe18a-229">Select **Visual C\#** from the left pane and then **WPF Application**</span></span>
-   <span data-ttu-id="fe18a-230">Введите **STESample.WPFTest** имя и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="fe18a-230">Enter **STESample.WPFTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="fe18a-231">Добавьте ссылку на **STESample.Entities** проекта</span><span class="sxs-lookup"><span data-stu-id="fe18a-231">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="fe18a-232">Нам нужно ссылку на службу в нашу службу WCF</span><span class="sxs-lookup"><span data-stu-id="fe18a-232">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="fe18a-233">Щелкните правой кнопкой мыши **STESample.WPFTest** в проекте **обозревателе решений** и выберите **добавить ссылку на службу...**</span><span class="sxs-lookup"><span data-stu-id="fe18a-233">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="fe18a-234">Нажмите кнопку **обнаружение**</span><span class="sxs-lookup"><span data-stu-id="fe18a-234">Click **Discover**</span></span>
-   <span data-ttu-id="fe18a-235">Введите **BloggingService** пространства имен и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="fe18a-235">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="fe18a-236">Теперь можно написать код для использования службы.</span><span class="sxs-lookup"><span data-stu-id="fe18a-236">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="fe18a-237">Откройте **MainWindow.xaml** и замените его содержимое следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="fe18a-237">Open **MainWindow.xaml** and replace the contents with the following code.</span></span>

``` xaml
    <Window
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:STESample="clr-namespace:STESample;assembly=STESample.Entities"
        mc:Ignorable="d" x:Class="STESample.WPFTest.MainWindow"
        Title="MainWindow" Height="350" Width="525" Loaded="Window_Loaded">

        <Window.Resources>
            <CollectionViewSource
                x:Key="blogViewSource"
                d:DesignSource="{d:DesignInstance {x:Type STESample:Blog}, CreateList=True}"/>
            <CollectionViewSource
                x:Key="blogPostsViewSource"
                Source="{Binding Posts, Source={StaticResource blogViewSource}}"/>
        </Window.Resources>

        <Grid DataContext="{StaticResource blogViewSource}">
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding}" Margin="10,10,10,179">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding BlogId}" Header="Id" Width="Auto" IsReadOnly="True" />
                    <DataGridTextColumn Binding="{Binding Name}" Header="Name" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Url}" Header="Url" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding Source={StaticResource blogPostsViewSource}}" Margin="10,145,10,38">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding PostId}" Header="Id" Width="Auto"  IsReadOnly="True"/>
                    <DataGridTextColumn Binding="{Binding Title}" Header="Title" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Content}" Header="Content" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <Button Width="68" Height="23" HorizontalAlignment="Right" VerticalAlignment="Bottom"
                    Margin="0,0,10,10" Click="buttonSave_Click">Save</Button>
        </Grid>
    </Window>
```

-   <span data-ttu-id="fe18a-238">Откройте код программной части для MainWindow (**MainWindow.xaml.cs**) и замените его содержимое следующим кодом</span><span class="sxs-lookup"><span data-stu-id="fe18a-238">Open the code behind for MainWindow (**MainWindow.xaml.cs**) and replace the contents with the following code</span></span>

``` csharp
    using STESample.WPFTest.BloggingService;
    using System.Collections.Generic;
    using System.Linq;
    using System.Windows;
    using System.Windows.Data;

    namespace STESample.WPFTest
    {
        public partial class MainWindow : Window
        {
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Find the view source for Blogs and populate it with all Blogs (and related Posts)
                    // from the Service. The default editing functionality of WPF will allow the objects
                    // to be manipulated on the screen.
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Get the blogs that are bound to the screen
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    var blogs = (List<Blog>)blogsViewSource.Source;

                    // Save all Blogs and related Posts
                    foreach (var blog in blogs)
                    {
                        service.UpdateBlog(blog);
                    }

                    // Re-query for data to get database-generated keys etc.
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }
        }
    }
```

<span data-ttu-id="fe18a-239">Теперь вы можете запустить приложение и увидеть, как оно работает:</span><span class="sxs-lookup"><span data-stu-id="fe18a-239">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="fe18a-240">Щелкните правой кнопкой мыши **STESample.WPFTest** в проекте **обозревателе решений** и выберите **«Отладка» —&gt; запустить новый экземпляр**</span><span class="sxs-lookup"><span data-stu-id="fe18a-240">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>
-   <span data-ttu-id="fe18a-241">Можно обрабатывать данные, используя экрана и сохраните его с помощью службы, используя **Сохранить** кнопки</span><span class="sxs-lookup"><span data-stu-id="fe18a-241">You can manipulate the data using the screen and save it via the service using the **Save** button</span></span>

![WPF](~/ef6/media/wpf.png)
