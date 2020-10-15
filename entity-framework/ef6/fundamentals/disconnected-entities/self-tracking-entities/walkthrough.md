---
title: Пошаговое руководство по Self-Tracking сущностям — EF6
description: Пошаговое руководство по самостоятельному отслеживанию сущностей для Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough
ms.openlocfilehash: 7c50bbc9fad10a474728b03e79b685c549cf675d
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92065606"
---
# <a name="self-tracking-entities-walkthrough"></a><span data-ttu-id="bc914-103">Пошаговое руководство по Self-Tracking сущностям</span><span class="sxs-lookup"><span data-stu-id="bc914-103">Self-Tracking Entities Walkthrough</span></span>
> [!IMPORTANT]
> <span data-ttu-id="bc914-104">Больше не рекомендуется использовать шаблон сущностей с самостоятельным отслеживанием.</span><span class="sxs-lookup"><span data-stu-id="bc914-104">We no longer recommend using the self-tracking-entities template.</span></span> <span data-ttu-id="bc914-105">Он по-прежнему будет доступен только для поддержки существующих приложений.</span><span class="sxs-lookup"><span data-stu-id="bc914-105">It will only continue to be available to support existing applications.</span></span> <span data-ttu-id="bc914-106">Если приложению необходимо работать с отключенными графами сущностей, рассмотрите другие варианты, такие как [отслеживаемые сущности](https://trackableentities.github.io/), которые технологически эквивалентны сущностям с самостоятельным отслеживанием, активно разрабатываемым сообществом. Или остановитесь на написании пользовательского кода с помощью API-интерфейсов отслеживания изменений низкого уровня.</span><span class="sxs-lookup"><span data-stu-id="bc914-106">If your application requires working with disconnected graphs of entities, consider other alternatives such as [Trackable Entities](https://trackableentities.github.io/), which is a technology similar to Self-Tracking-Entities that is more actively developed by the community, or writing custom code using the low-level change tracking APIs.</span></span>

<span data-ttu-id="bc914-107">В этом пошаговом руководстве демонстрируется сценарий, в котором служба Windows Communication Foundation (WCF) предоставляет операцию, которая возвращает граф сущностей.</span><span class="sxs-lookup"><span data-stu-id="bc914-107">This walkthrough demonstrates the scenario in which a Windows Communication Foundation (WCF) service exposes an operation that returns an entity graph.</span></span> <span data-ttu-id="bc914-108">Затем клиентское приложение манипулирует этим графом и отправляет изменения в операцию службы, которая проверяет и сохраняет обновления в базе данных с помощью Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="bc914-108">Next, a client application manipulates that graph and submits the modifications to a service operation that validates and saves the updates to a database using Entity Framework.</span></span>

<span data-ttu-id="bc914-109">Перед выполнением этого пошагового руководства убедитесь, что вы читаете страницу [сущностей с самостоятельным отслеживанием](xref:ef6/fundamentals/disconnected-entities/self-tracking-entities/index) .</span><span class="sxs-lookup"><span data-stu-id="bc914-109">Before completing this walkthrough make sure you read the [Self-Tracking Entities](xref:ef6/fundamentals/disconnected-entities/self-tracking-entities/index) page.</span></span>

<span data-ttu-id="bc914-110">В этом пошаговом руководстве выполняются следующие действия.</span><span class="sxs-lookup"><span data-stu-id="bc914-110">This walkthrough completes the following actions:</span></span>

-   <span data-ttu-id="bc914-111">Создает базу данных для доступа.</span><span class="sxs-lookup"><span data-stu-id="bc914-111">Creates a database to access.</span></span>
-   <span data-ttu-id="bc914-112">Создает библиотеку классов, содержащую модель.</span><span class="sxs-lookup"><span data-stu-id="bc914-112">Creates a class library that contains the model.</span></span>
-   <span data-ttu-id="bc914-113">Меняет местами на шаблон генератора сущностей Self-Tracking.</span><span class="sxs-lookup"><span data-stu-id="bc914-113">Swaps to the Self-Tracking Entity Generator template.</span></span>
-   <span data-ttu-id="bc914-114">Перемещает классы сущностей в отдельный проект.</span><span class="sxs-lookup"><span data-stu-id="bc914-114">Moves the entity classes to a separate project.</span></span>
-   <span data-ttu-id="bc914-115">Создает службу WCF, которая предоставляет операции для запросов и сохранения сущностей.</span><span class="sxs-lookup"><span data-stu-id="bc914-115">Creates a WCF service that exposes operations to query and save entities.</span></span>
-   <span data-ttu-id="bc914-116">Создает клиентские приложения (консоль и WPF), которые используют службу.</span><span class="sxs-lookup"><span data-stu-id="bc914-116">Creates client applications (Console and WPF) that consume the service.</span></span>

<span data-ttu-id="bc914-117">В этом пошаговом руководстве мы будем использовать Database First, но одни и те же методы будут применяться одинаково для Model First.</span><span class="sxs-lookup"><span data-stu-id="bc914-117">We'll use Database First in this walkthrough but the same techniques apply equally to Model First.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="bc914-118">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="bc914-118">Pre-Requisites</span></span>

<span data-ttu-id="bc914-119">Для выполнения этого пошагового руководства потребуется последняя версия Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bc914-119">To complete this walkthrough you will need a recent version of Visual Studio.</span></span>

## <a name="create-a-database"></a><span data-ttu-id="bc914-120">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="bc914-120">Create a Database</span></span>

<span data-ttu-id="bc914-121">Сервер базы данных, устанавливаемый с Visual Studio, отличается в зависимости от установленной версии Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bc914-121">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="bc914-122">Если вы используете Visual Studio 2012, то будете создавать базу данных LocalDB.</span><span class="sxs-lookup"><span data-stu-id="bc914-122">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="bc914-123">При использовании Visual Studio 2010 будет создаваться база данных SQL Express.</span><span class="sxs-lookup"><span data-stu-id="bc914-123">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="bc914-124">Давайте создадим базу данных.</span><span class="sxs-lookup"><span data-stu-id="bc914-124">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="bc914-125">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bc914-125">Open Visual Studio</span></span>
-   <span data-ttu-id="bc914-126">**Просмотр — &gt; Обозреватель сервера**</span><span class="sxs-lookup"><span data-stu-id="bc914-126">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="bc914-127">Щелкните правой кнопкой мыши **подключения к данным — &gt; Добавить подключение...**</span><span class="sxs-lookup"><span data-stu-id="bc914-127">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="bc914-128">Если вы не подключились к базе данных из обозреватель сервера, прежде чем нужно будет выбрать **Microsoft SQL Server** в качестве источника данных</span><span class="sxs-lookup"><span data-stu-id="bc914-128">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="bc914-129">Подключение к LocalDB или SQL Express в зависимости от того, какой из установленных служб</span><span class="sxs-lookup"><span data-stu-id="bc914-129">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="bc914-130">Введите **стесампле** в качестве имени базы данных</span><span class="sxs-lookup"><span data-stu-id="bc914-130">Enter **STESample** as the database name</span></span>
-   <span data-ttu-id="bc914-131">Нажмите кнопку **ОК** , и появится запрос на создание новой базы данных, выберите **Да** .</span><span class="sxs-lookup"><span data-stu-id="bc914-131">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="bc914-132">Теперь новая база данных появится в обозреватель сервера</span><span class="sxs-lookup"><span data-stu-id="bc914-132">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="bc914-133">Если вы используете Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="bc914-133">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="bc914-134">Щелкните правой кнопкой мыши базу данных в обозревателе сервера и выберите действие **Создать запрос**.</span><span class="sxs-lookup"><span data-stu-id="bc914-134">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="bc914-135">Скопируйте следующий SQL в новый запрос, щелкните запрос правой кнопкой мыши и выберите команду **выполнить** .</span><span class="sxs-lookup"><span data-stu-id="bc914-135">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="bc914-136">Если вы используете Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="bc914-136">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="bc914-137">Выбор **данных — &gt; Редактор SQL — &gt; создать соединение с запросом...**</span><span class="sxs-lookup"><span data-stu-id="bc914-137">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="bc914-138">Введите \*\*. \\ \*\*В качестве имени сервера в SQLExpress нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="bc914-138">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="bc914-139">Выберите базу данных **стесампле** в раскрывающемся списке в верхней части редактора запросов.</span><span class="sxs-lookup"><span data-stu-id="bc914-139">Select the **STESample** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="bc914-140">Скопируйте следующий SQL в новый запрос, щелкните запрос правой кнопкой мыши и выберите команду **выполнить SQL** .</span><span class="sxs-lookup"><span data-stu-id="bc914-140">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

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

## <a name="create-the-model"></a><span data-ttu-id="bc914-141">Создание модели</span><span class="sxs-lookup"><span data-stu-id="bc914-141">Create the Model</span></span>

<span data-ttu-id="bc914-142">Сначала нам нужен проект, в котором будет размещена модель.</span><span class="sxs-lookup"><span data-stu-id="bc914-142">First up, we need a project to put the model in.</span></span>

-   <span data-ttu-id="bc914-143">**Файл- &gt; создать &gt; проект...**</span><span class="sxs-lookup"><span data-stu-id="bc914-143">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="bc914-144">Выберите \*\*Visual C \# \*\* в левой области, а затем **Библиотека классов**</span><span class="sxs-lookup"><span data-stu-id="bc914-144">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="bc914-145">Введите **стесампле** в качестве имени и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="bc914-145">Enter **STESample** as the name and click **OK**</span></span>

<span data-ttu-id="bc914-146">Теперь создадим простую модель в конструкторе EF для доступа к нашей базе данных:</span><span class="sxs-lookup"><span data-stu-id="bc914-146">Now we'll create a simple model in the EF Designer to access our database:</span></span>

-   <span data-ttu-id="bc914-147">**Проект- &gt; Добавить новый элемент...**</span><span class="sxs-lookup"><span data-stu-id="bc914-147">**Project -&gt; Add New Item...**</span></span>
-   <span data-ttu-id="bc914-148">Выберите **данные** на левой панели, а затем **ADO.NET EDM**</span><span class="sxs-lookup"><span data-stu-id="bc914-148">Select **Data** from the left pane and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="bc914-149">Введите **блоггингмодел** в качестве имени и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="bc914-149">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="bc914-150">Выберите **создать из базы данных** и нажмите кнопку **Далее** .</span><span class="sxs-lookup"><span data-stu-id="bc914-150">Select **Generate from database** and click **Next**</span></span>
-   <span data-ttu-id="bc914-151">Введите сведения о подключении для базы данных, созданной в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="bc914-151">Enter the connection information for the database that you created in the previous section</span></span>
-   <span data-ttu-id="bc914-152">Введите **BloggingContext** в качестве имени для строки подключения и нажмите кнопку " **Далее** ".</span><span class="sxs-lookup"><span data-stu-id="bc914-152">Enter **BloggingContext** as the name for the connection string and click **Next**</span></span>
-   <span data-ttu-id="bc914-153">Установите флажки рядом с **таблицами** и нажмите кнопку **Готово** .</span><span class="sxs-lookup"><span data-stu-id="bc914-153">Check the box next to **Tables** and click **Finish**</span></span>

## <a name="swap-to-ste-code-generation"></a><span data-ttu-id="bc914-154">Переключение на ТАВИТЬ создание кода</span><span class="sxs-lookup"><span data-stu-id="bc914-154">Swap to STE Code Generation</span></span>

<span data-ttu-id="bc914-155">Теперь необходимо отключить создание кода по умолчанию и переключить на Self-Tracking сущности.</span><span class="sxs-lookup"><span data-stu-id="bc914-155">Now we need to disable the default code generation and swap to Self-Tracking Entities.</span></span>

### <a name="if-you-are-using-visual-studio-2012"></a><span data-ttu-id="bc914-156">Если вы используете Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="bc914-156">If you are using Visual Studio 2012</span></span>

-   <span data-ttu-id="bc914-157">Разверните **блоггингмодел. EDMX** в **Обозреватель решений** и удалите **BloggingModel.TT** и **BloggingModel.Context.TT**. 
     *это приведет к отключению создания кода по умолчанию* .</span><span class="sxs-lookup"><span data-stu-id="bc914-157">Expand **BloggingModel.edmx** in **Solution Explorer** and delete the **BloggingModel.tt** and **BloggingModel.Context.tt**
*This will disable the default code generation*</span></span>
-   <span data-ttu-id="bc914-158">Щелкните правой кнопкой мыши пустую область в области конструктора EF и выберите пункт **Добавить элемент создания кода...**</span><span class="sxs-lookup"><span data-stu-id="bc914-158">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="bc914-159">Выберите "в **сети** " в левой области и найдите **генератор тавить** .</span><span class="sxs-lookup"><span data-stu-id="bc914-159">Select **Online** from the left pane and search for **STE Generator**</span></span>
-   <span data-ttu-id="bc914-160">Выберите шаблон \*\*генератор тавить для C \# \*\* , введите **стетемплате** в качестве имени и нажмите кнопку **Добавить** .</span><span class="sxs-lookup"><span data-stu-id="bc914-160">Select the **STE Generator for C\#** template, enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="bc914-161">Файлы **STETemplate.TT** и **STETemplate.Context.TT** добавляются в файл блоггингмодел. edmx.</span><span class="sxs-lookup"><span data-stu-id="bc914-161">The **STETemplate.tt** and **STETemplate.Context.tt** files are added nested under the BloggingModel.edmx file</span></span>

### <a name="if-you-are-using-visual-studio-2010"></a><span data-ttu-id="bc914-162">Если вы используете Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="bc914-162">If you are using Visual Studio 2010</span></span>

-   <span data-ttu-id="bc914-163">Щелкните правой кнопкой мыши пустую область в области конструктора EF и выберите пункт **Добавить элемент создания кода...**</span><span class="sxs-lookup"><span data-stu-id="bc914-163">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="bc914-164">Выберите **код** на левой панели, а затем **ADO.NET Self-Trackingный Генератор сущностей** .</span><span class="sxs-lookup"><span data-stu-id="bc914-164">Select **Code** from the left pane and then **ADO.NET Self-Tracking Entity Generator**</span></span>
-   <span data-ttu-id="bc914-165">Введите **стетемплате** в качестве имени и нажмите кнопку **Добавить** .</span><span class="sxs-lookup"><span data-stu-id="bc914-165">Enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="bc914-166">Файлы **STETemplate.TT** и **STETemplate.Context.TT** добавляются непосредственно в проект.</span><span class="sxs-lookup"><span data-stu-id="bc914-166">The **STETemplate.tt** and **STETemplate.Context.tt** files are added directly to your project</span></span>

## <a name="move-entity-types-into-separate-project"></a><span data-ttu-id="bc914-167">Переместить типы сущностей в отдельный проект</span><span class="sxs-lookup"><span data-stu-id="bc914-167">Move Entity Types into Separate Project</span></span>

<span data-ttu-id="bc914-168">Чтобы использовать Self-Tracking сущности, клиентскому приложению требуется доступ к классам сущностей, созданным из нашей модели.</span><span class="sxs-lookup"><span data-stu-id="bc914-168">To use Self-Tracking Entities our client application needs access to the entity classes generated from our model.</span></span> <span data-ttu-id="bc914-169">Так как мы не хотим предоставлять клиентским приложениям всю модель, мы будем перемещать классы сущностей в отдельный проект.</span><span class="sxs-lookup"><span data-stu-id="bc914-169">Because we don't want to expose the whole model to the client application we're going to move the entity classes into a separate project.</span></span>

<span data-ttu-id="bc914-170">Первым шагом является завершение создания классов сущностей в существующем проекте:</span><span class="sxs-lookup"><span data-stu-id="bc914-170">The first step is to stop generating entity classes in the existing project:</span></span>

-   <span data-ttu-id="bc914-171">Щелкните правой кнопкой мыши **STETemplate.TT** в **Обозреватель решений** и выберите пункт **Свойства** .</span><span class="sxs-lookup"><span data-stu-id="bc914-171">Right-click on **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="bc914-172">В окне **свойств** очистите **Тексттемплатингфилеженератор** из свойства **CustomTool** .</span><span class="sxs-lookup"><span data-stu-id="bc914-172">In the **Properties** window clear **TextTemplatingFileGenerator** from the **CustomTool** property</span></span>
-   <span data-ttu-id="bc914-173">Разверните **STETemplate.TT** в **Обозреватель решений** и удалите все вложенные файлы.</span><span class="sxs-lookup"><span data-stu-id="bc914-173">Expand **STETemplate.tt** in **Solution Explorer** and delete all files nested under it</span></span>

<span data-ttu-id="bc914-174">Далее предстоит добавить новый проект и создать в нем классы сущностей.</span><span class="sxs-lookup"><span data-stu-id="bc914-174">Next, we are going to add a new project and generate the entity classes in it</span></span>

-   <span data-ttu-id="bc914-175">**Файл — &gt; Добавить &gt; проект...**</span><span class="sxs-lookup"><span data-stu-id="bc914-175">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="bc914-176">Выберите \*\*Visual C \# \*\* в левой области, а затем **Библиотека классов**</span><span class="sxs-lookup"><span data-stu-id="bc914-176">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="bc914-177">Введите **стесампле. Entities** в качестве имени и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="bc914-177">Enter **STESample.Entities** as the name and click **OK**</span></span>
-   <span data-ttu-id="bc914-178">**Проект- &gt; Добавить существующий элемент...**</span><span class="sxs-lookup"><span data-stu-id="bc914-178">**Project -&gt; Add Existing Item...**</span></span>
-   <span data-ttu-id="bc914-179">Перейдите в папку проекта **стесампле**</span><span class="sxs-lookup"><span data-stu-id="bc914-179">Navigate to the **STESample** project folder</span></span>
-   <span data-ttu-id="bc914-180">Выберите для просмотра **всех файлов ( \* . \* )**</span><span class="sxs-lookup"><span data-stu-id="bc914-180">Select to view **All Files (\*.\*)**</span></span>
-   <span data-ttu-id="bc914-181">Выберите файл **STETemplate.TT**</span><span class="sxs-lookup"><span data-stu-id="bc914-181">Select the **STETemplate.tt** file</span></span>
-   <span data-ttu-id="bc914-182">Щелкните стрелку раскрывающегося списка рядом с кнопкой **Добавить** и выберите **Добавить как ссылку** .</span><span class="sxs-lookup"><span data-stu-id="bc914-182">Click on the drop down arrow next to the **Add** button and select **Add As Link**</span></span>

    ![Добавить связанный шаблон](~/ef6/media/addlinkedtemplate.png)

<span data-ttu-id="bc914-184">Кроме того, мы собираемся убедиться, что классы сущностей создаются в том же пространстве имен, что и контекст.</span><span class="sxs-lookup"><span data-stu-id="bc914-184">We're also going to make sure the entity classes get generated in the same namespace as the context.</span></span> <span data-ttu-id="bc914-185">Это просто сокращает количество операторов using, которые необходимо добавить в наше приложение.</span><span class="sxs-lookup"><span data-stu-id="bc914-185">This just reduces the number of using statements we need to add throughout our application.</span></span>

-   <span data-ttu-id="bc914-186">Щелкните правой кнопкой мыши связанный **STETemplate.TT** в **Обозреватель решений** и выберите **Свойства** .</span><span class="sxs-lookup"><span data-stu-id="bc914-186">Right-click on the linked **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="bc914-187">В окне **Свойства** задайте для **пространства имен пользовательского инструмента** значение **стесампле** .</span><span class="sxs-lookup"><span data-stu-id="bc914-187">In the **Properties** window set **Custom Tool Namespace** to **STESample**</span></span>

<span data-ttu-id="bc914-188">Для компиляции кода, созданного с помощью шаблона ТАВИТЬ, требуется ссылка на **System. Runtime. Serialization** .</span><span class="sxs-lookup"><span data-stu-id="bc914-188">The code generated by the STE template will need a reference to **System.Runtime.Serialization** in order to compile.</span></span> <span data-ttu-id="bc914-189">Эта библиотека необходима для атрибутов **DATACONTRACT** WCF и **DataMember** , которые используются в сериализуемых типах сущностей.</span><span class="sxs-lookup"><span data-stu-id="bc914-189">This library is needed for the WCF **DataContract** and **DataMember** attributes that are used on the serializable entity types.</span></span>

-   <span data-ttu-id="bc914-190">Щелкните правой кнопкой мыши проект **стесампле. Entities** в **Обозреватель решений** и выберите команду **Добавить ссылку...**</span><span class="sxs-lookup"><span data-stu-id="bc914-190">Right click on the **STESample.Entities** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="bc914-191">В Visual Studio 2012 установите флажок рядом с **System. Runtime. Serialization** и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="bc914-191">In Visual Studio 2012 - check the box next to **System.Runtime.Serialization** and click **OK**</span></span>
    -   <span data-ttu-id="bc914-192">В Visual Studio 2010 выберите **System. Runtime. Serialization** и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="bc914-192">In Visual Studio 2010 - select **System.Runtime.Serialization** and click **OK**</span></span>

<span data-ttu-id="bc914-193">Наконец, в проекте с нашим контекстом потребуется ссылка на типы сущностей.</span><span class="sxs-lookup"><span data-stu-id="bc914-193">Finally, the project with our context in it will need a reference to the entity types.</span></span>

-   <span data-ttu-id="bc914-194">Щелкните правой кнопкой мыши проект **стесампле** в **Обозреватель решений** и выберите **Добавить ссылку...**</span><span class="sxs-lookup"><span data-stu-id="bc914-194">Right click on the **STESample** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="bc914-195">В Visual Studio 2012 — выберите **решение** в левой области, установите флажок рядом с **стесампле. Entities** и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="bc914-195">In Visual Studio 2012 - select **Solution** from the left pane, check the box next to **STESample.Entities** and click **OK**</span></span>
    -   <span data-ttu-id="bc914-196">В Visual Studio 2010 на вкладке **проекты** выберите **стесампле. Entities** и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="bc914-196">In Visual Studio 2010 - select the **Projects** tab, select **STESample.Entities** and click **OK**</span></span>

>[!NOTE]
> <span data-ttu-id="bc914-197">Другим вариантом перемещения типов сущностей в отдельный проект является перемещение файла шаблона вместо его связи с расположением по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="bc914-197">Another option for moving the entity types to a separate project is to move the template file, rather than linking it from its default location.</span></span> <span data-ttu-id="bc914-198">В этом случае необходимо будет обновить переменную **inputFile** в шаблоне, чтобы указать относительный путь к EDMX-файлу (в этом примере это будет **. \\ Блоггингмодел. EDMX**).</span><span class="sxs-lookup"><span data-stu-id="bc914-198">If you do this, you will need to update the **inputFile** variable in the template to provide the relative path to the edmx file (in this example that would be **..\\BloggingModel.edmx**).</span></span>

## <a name="create-a-wcf-service"></a><span data-ttu-id="bc914-199">Создание службы WCF</span><span class="sxs-lookup"><span data-stu-id="bc914-199">Create a WCF Service</span></span>

<span data-ttu-id="bc914-200">Пришло время добавить службу WCF для предоставления наших данных, мы начнем с создания проекта.</span><span class="sxs-lookup"><span data-stu-id="bc914-200">Now it's time to add a WCF Service to expose our data, we'll start by creating the project.</span></span>

-   <span data-ttu-id="bc914-201">**Файл — &gt; Добавить &gt; проект...**</span><span class="sxs-lookup"><span data-stu-id="bc914-201">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="bc914-202">Выберите \*\*Visual C \# \*\* в левой области, а затем — **приложение службы WCF** .</span><span class="sxs-lookup"><span data-stu-id="bc914-202">Select **Visual C\#** from the left pane and then **WCF Service Application**</span></span>
-   <span data-ttu-id="bc914-203">Введите **стесампле. Service** в качестве имени и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="bc914-203">Enter **STESample.Service** as the name and click **OK**</span></span>
-   <span data-ttu-id="bc914-204">Добавление ссылки на сборку **System. Data. Entity**</span><span class="sxs-lookup"><span data-stu-id="bc914-204">Add a reference to the **System.Data.Entity** assembly</span></span>
-   <span data-ttu-id="bc914-205">Добавление ссылки на проекты **стесампле** и **стесампле. Entities**</span><span class="sxs-lookup"><span data-stu-id="bc914-205">Add a reference to the **STESample** and **STESample.Entities** projects</span></span>

<span data-ttu-id="bc914-206">Нам нужно скопировать строку подключения EF в этот проект, чтобы она была найдена во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="bc914-206">We need to copy the EF connection string to this project so that it is found at runtime.</span></span>

-   <span data-ttu-id="bc914-207">Откройте файл **App.Config** для проекта **стесампле **и скопируйте элемент **ConnectionString** .</span><span class="sxs-lookup"><span data-stu-id="bc914-207">Open the **App.Config** file for the **STESample **project and copy the **connectionStrings** element</span></span>
-   <span data-ttu-id="bc914-208">Вставьте элемент **ConnectionString** в качестве дочернего элемента элемента **конфигурации** файла **Web.Config** в проекте **стесампле. Service.**</span><span class="sxs-lookup"><span data-stu-id="bc914-208">Paste the **connectionStrings** element as a child element of the **configuration** element of the **Web.Config** file in the **STESample.Service** project</span></span>

<span data-ttu-id="bc914-209">Теперь пора реализовать фактическую службу.</span><span class="sxs-lookup"><span data-stu-id="bc914-209">Now it's time to implement the actual service.</span></span>

-   <span data-ttu-id="bc914-210">Откройте **IService1.CS** и замените содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="bc914-210">Open **IService1.cs** and replace the contents with the following code</span></span>

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

-   <span data-ttu-id="bc914-211">Откройте **Service1. svc** и замените содержимое следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="bc914-211">Open **Service1.svc** and replace the contents with the following code</span></span>

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

## <a name="consume-the-service-from-a-console-application"></a><span data-ttu-id="bc914-212">Использование службы из консольного приложения</span><span class="sxs-lookup"><span data-stu-id="bc914-212">Consume the Service from a Console Application</span></span>

<span data-ttu-id="bc914-213">Давайте создадим консольное приложение, использующее нашу службу.</span><span class="sxs-lookup"><span data-stu-id="bc914-213">Let's create a console application that uses our service.</span></span>

-   <span data-ttu-id="bc914-214">**Файл- &gt; создать &gt; проект...**</span><span class="sxs-lookup"><span data-stu-id="bc914-214">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="bc914-215">Выберите \*\*Visual C \# \*\* в левой области, а затем **консольное приложение**</span><span class="sxs-lookup"><span data-stu-id="bc914-215">Select **Visual C\#** from the left pane and then **Console Application**</span></span>
-   <span data-ttu-id="bc914-216">Введите **стесампле. консолетест** в качестве имени и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="bc914-216">Enter **STESample.ConsoleTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="bc914-217">Добавление ссылки на проект **стесампле. Entities**</span><span class="sxs-lookup"><span data-stu-id="bc914-217">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="bc914-218">Нам нужна ссылка на службу для нашей службы WCF.</span><span class="sxs-lookup"><span data-stu-id="bc914-218">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="bc914-219">Щелкните правой кнопкой мыши проект **стесампле. консолетест** в **обозреватель решений** и выберите **Добавление ссылки на службу...**</span><span class="sxs-lookup"><span data-stu-id="bc914-219">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="bc914-220">Нажмите кнопку **обнаружить** .</span><span class="sxs-lookup"><span data-stu-id="bc914-220">Click **Discover**</span></span>
-   <span data-ttu-id="bc914-221">Введите **блоггингсервице** в качестве пространства имен и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="bc914-221">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="bc914-222">Теперь мы можем написать код для использования службы.</span><span class="sxs-lookup"><span data-stu-id="bc914-222">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="bc914-223">Откройте **Program.CS** и замените содержимое следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="bc914-223">Open **Program.cs** and replace the contents with the following code.</span></span>

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

<span data-ttu-id="bc914-224">Теперь вы можете запустить приложение и увидеть, как оно работает:</span><span class="sxs-lookup"><span data-stu-id="bc914-224">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="bc914-225">Щелкните правой кнопкой мыши проект **стесампле. консолетест** в **Обозреватель решений** и выберите **Отладка- &gt; запустить новый экземпляр** .</span><span class="sxs-lookup"><span data-stu-id="bc914-225">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>

<span data-ttu-id="bc914-226">При выполнении приложения вы увидите следующие выходные данные.</span><span class="sxs-lookup"><span data-stu-id="bc914-226">You'll see the following output when the application executes.</span></span>

```console
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

## <a name="consume-the-service-from-a-wpf-application"></a><span data-ttu-id="bc914-227">Использование службы из приложения WPF</span><span class="sxs-lookup"><span data-stu-id="bc914-227">Consume the Service from a WPF Application</span></span>

<span data-ttu-id="bc914-228">Давайте создадим приложение WPF, которое использует нашу службу.</span><span class="sxs-lookup"><span data-stu-id="bc914-228">Let's create a WPF application that uses our service.</span></span>

-   <span data-ttu-id="bc914-229">**Файл- &gt; создать &gt; проект...**</span><span class="sxs-lookup"><span data-stu-id="bc914-229">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="bc914-230">Выберите \*\*Visual C \# \*\* в левой области, а затем — **приложение WPF**</span><span class="sxs-lookup"><span data-stu-id="bc914-230">Select **Visual C\#** from the left pane and then **WPF Application**</span></span>
-   <span data-ttu-id="bc914-231">Введите **стесампле. впфтест** в качестве имени и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="bc914-231">Enter **STESample.WPFTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="bc914-232">Добавление ссылки на проект **стесампле. Entities**</span><span class="sxs-lookup"><span data-stu-id="bc914-232">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="bc914-233">Нам нужна ссылка на службу для нашей службы WCF.</span><span class="sxs-lookup"><span data-stu-id="bc914-233">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="bc914-234">Щелкните правой кнопкой мыши проект **стесампле. впфтест** в **обозреватель решений** и выберите **Добавление ссылки на службу...**</span><span class="sxs-lookup"><span data-stu-id="bc914-234">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="bc914-235">Нажмите кнопку **обнаружить** .</span><span class="sxs-lookup"><span data-stu-id="bc914-235">Click **Discover**</span></span>
-   <span data-ttu-id="bc914-236">Введите **блоггингсервице** в качестве пространства имен и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="bc914-236">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="bc914-237">Теперь мы можем написать код для использования службы.</span><span class="sxs-lookup"><span data-stu-id="bc914-237">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="bc914-238">Откройте файл **MainWindow. XAML** и замените его содержимое следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="bc914-238">Open **MainWindow.xaml** and replace the contents with the following code.</span></span>

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

-   <span data-ttu-id="bc914-239">Откройте код программной части для MainWindow (**MainWindow.XAML.CS**) и замените содержимое следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="bc914-239">Open the code behind for MainWindow (**MainWindow.xaml.cs**) and replace the contents with the following code</span></span>

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

<span data-ttu-id="bc914-240">Теперь вы можете запустить приложение и увидеть, как оно работает:</span><span class="sxs-lookup"><span data-stu-id="bc914-240">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="bc914-241">Щелкните правой кнопкой мыши проект **стесампле. впфтест** в **Обозреватель решений** и выберите **Отладка- &gt; запустить новый экземпляр** .</span><span class="sxs-lookup"><span data-stu-id="bc914-241">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>
-   <span data-ttu-id="bc914-242">Вы можете манипулировать данными с помощью экрана и сохранить их через службу с помощью кнопки **сохранить** .</span><span class="sxs-lookup"><span data-stu-id="bc914-242">You can manipulate the data using the screen and save it via the service using the **Save** button</span></span>

![Главное окно WPF](~/ef6/media/wpf.png)
