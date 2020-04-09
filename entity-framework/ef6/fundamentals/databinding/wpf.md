---
title: Связывание данных с WPF - EF6
author: divega
ms.date: 04/02/2020
ms.assetid: e90d48e6-bea7785-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 6908e2a7597d0c199620c6015ed3ea06226c5ea9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639143"
---
> [!IMPORTANT]
> <span data-ttu-id="dd6de-102">**Этот документ действителен только для WPF в рамках .NET**</span><span class="sxs-lookup"><span data-stu-id="dd6de-102">**This document is valid for WPF on the .NET Framework only**</span></span>
>
> <span data-ttu-id="dd6de-103">В этом документе описаны данные, связывающие для WPF в рамках .NET.</span><span class="sxs-lookup"><span data-stu-id="dd6de-103">This document describes databinding for WPF on the .NET Framework.</span></span> <span data-ttu-id="dd6de-104">Для новых проектов .NET Core мы рекомендуем использовать [EF Core](/ef/core) вместо Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="dd6de-104">For new .NET Core projects, we recommend you use [EF Core](/ef/core) instead of Entity Framework 6.</span></span> <span data-ttu-id="dd6de-105">Документация для связывания данных в EF Core отслеживается в [выпуске #778.](https://github.com/dotnet/EntityFramework.Docs/issues/778)</span><span class="sxs-lookup"><span data-stu-id="dd6de-105">The documentation for databinding in EF Core is tracked in [Issue #778](https://github.com/dotnet/EntityFramework.Docs/issues/778).</span></span>

# <a name="databinding-with-wpf"></a><span data-ttu-id="dd6de-106">Привязка данных с помощью WPF</span><span class="sxs-lookup"><span data-stu-id="dd6de-106">Databinding with WPF</span></span>
<span data-ttu-id="dd6de-107">Это шаг за шагом пошагонасто показывает, как связать типы POCO с элементами управления WPF в форме "мастер-детали".</span><span class="sxs-lookup"><span data-stu-id="dd6de-107">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="dd6de-108">Приложение использует ApIs Entity Framework для заполнения объектов данными из базы данных, отслеживания изменений и сохранения данных в базе данных.</span><span class="sxs-lookup"><span data-stu-id="dd6de-108">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="dd6de-109">Модель определяет два типа, которые участвуют в отношениях\\от одного к\\многим: **Категория** (основной мастер) и **Продукт** (зависимая деталь).</span><span class="sxs-lookup"><span data-stu-id="dd6de-109">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="dd6de-110">Затем инструменты Visual Studio используются для привязки типов, определенных в модели, к элементам управления WPF.</span><span class="sxs-lookup"><span data-stu-id="dd6de-110">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="dd6de-111">Платформа wPF, связывающая данные, позволяет осуществлять навигацию между связанными объектами: выбор строк в главном представлении приводит к обновлению представления деталей с соответствующими данными о добилище.</span><span class="sxs-lookup"><span data-stu-id="dd6de-111">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="dd6de-112">Скриншоты и код списки в этом пошаговое покадрово взяты из Visual Studio 2013, но вы можете завершить это пошаговое решение с Visual Studio 2012 или Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="dd6de-112">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="dd6de-113">Используйте опцию «Объект» для создания источников данных WPF</span><span class="sxs-lookup"><span data-stu-id="dd6de-113">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="dd6de-114">В предыдущей версии Entity Framework мы рекомендуем использовать опцию **базы данных** при создании нового источника данных на основе модели, созданной с EF Designer.</span><span class="sxs-lookup"><span data-stu-id="dd6de-114">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="dd6de-115">Это произошло потому, что конструктор генерирует контекст, полученный из классов ObjectContext и сущности, полученных из EntityObject.</span><span class="sxs-lookup"><span data-stu-id="dd6de-115">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="dd6de-116">Использование опции Database поможет вам написать лучший код для взаимодействия с этой поверхностью API.</span><span class="sxs-lookup"><span data-stu-id="dd6de-116">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="dd6de-117">ДИЗАЙНЕРы EF для Visual Studio 2012 и Visual Studio 2013 создают контекст, который вытекает из DbContext вместе с простыми классами сущности POCO.</span><span class="sxs-lookup"><span data-stu-id="dd6de-117">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="dd6de-118">С Visual Studio 2010 мы рекомендуем поменять на шаблон генерации кода, который использует DbContext, как описано позже в этом пошаговочном.</span><span class="sxs-lookup"><span data-stu-id="dd6de-118">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="dd6de-119">При использовании поверхности API DbContext при создании нового источника данных следует использовать опцию **«Объект»,** как показано в этом пошаговом слове.</span><span class="sxs-lookup"><span data-stu-id="dd6de-119">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="dd6de-120">При необходимости можно [вернуться к созданию кода](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) на основе ObjectContext для моделей, созданных с помощью EF Designer.</span><span class="sxs-lookup"><span data-stu-id="dd6de-120">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="dd6de-121">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="dd6de-121">Pre-Requisites</span></span>

<span data-ttu-id="dd6de-122">Вы должны иметь Visual Studio 2013, Visual Studio 2012 или Visual Studio 2010 установлен для завершения этого пошагового покистраза.</span><span class="sxs-lookup"><span data-stu-id="dd6de-122">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="dd6de-123">Если вы используете Visual Studio 2010, вы также должны установить NuGet.</span><span class="sxs-lookup"><span data-stu-id="dd6de-123">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="dd6de-124">Для получения дополнительной информации [см.](https://docs.microsoft.com/nuget/install-nuget-client-tools)</span><span class="sxs-lookup"><span data-stu-id="dd6de-124">For more information, see [Installing NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="dd6de-125">Создание приложения</span><span class="sxs-lookup"><span data-stu-id="dd6de-125">Create the Application</span></span>

-   <span data-ttu-id="dd6de-126">Запустите Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dd6de-126">Open Visual Studio</span></span>
-   <span data-ttu-id="dd6de-127">**Файл&gt; -&gt; Новый - Проект....**</span><span class="sxs-lookup"><span data-stu-id="dd6de-127">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="dd6de-128">Выберите **Windows** в левом стеку и **WPFApplication** в правом стене</span><span class="sxs-lookup"><span data-stu-id="dd6de-128">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="dd6de-129">Введите **WPFwithEFSample** как имя</span><span class="sxs-lookup"><span data-stu-id="dd6de-129">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="dd6de-130">Выберите **OK**</span><span class="sxs-lookup"><span data-stu-id="dd6de-130">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="dd6de-131">Установка пакета Сущность Framework NuGet</span><span class="sxs-lookup"><span data-stu-id="dd6de-131">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="dd6de-132">В Solution Explorer нажмите правой кнопкой мыши на проекте **WinFormswithEFSample**</span><span class="sxs-lookup"><span data-stu-id="dd6de-132">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="dd6de-133">Выберите **Пакеты Управления NuGet...**</span><span class="sxs-lookup"><span data-stu-id="dd6de-133">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="dd6de-134">В диалоге пакетов Управления NuGet выберите вкладку **Online** и выберите пакет **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="dd6de-134">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="dd6de-135">Нажмите **Установить**</span><span class="sxs-lookup"><span data-stu-id="dd6de-135">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="dd6de-136">В дополнение к сборке EntityFramework также добавлена ссылка на System.ComponentModel.DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="dd6de-136">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="dd6de-137">Если в проекте есть ссылка на System.Data.Entity, то он будет удален при установке пакета EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="dd6de-137">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="dd6de-138">Сборка System.Data.Entity больше не используется для приложений Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="dd6de-138">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="dd6de-139">Определение модели</span><span class="sxs-lookup"><span data-stu-id="dd6de-139">Define a Model</span></span>

<span data-ttu-id="dd6de-140">В этом пошаговом шаге вы можете реализовать модель с помощью Code First или EF Designer.</span><span class="sxs-lookup"><span data-stu-id="dd6de-140">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="dd6de-141">Заполните один из двух следующих разделов.</span><span class="sxs-lookup"><span data-stu-id="dd6de-141">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="dd6de-142">Вариант 1: Определите модель с помощью Code First</span><span class="sxs-lookup"><span data-stu-id="dd6de-142">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="dd6de-143">В этом разделе показано, как создать модель и связанную с ней базу данных с помощью Code First.</span><span class="sxs-lookup"><span data-stu-id="dd6de-143">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="dd6de-144">Перейти к следующему разделу **(Вариант 2: Определите модель с помощью базы данных First),** если вы предпочитаете использовать Database First для обратного проектирования модели из базы данных с помощью конструктора EF</span><span class="sxs-lookup"><span data-stu-id="dd6de-144">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="dd6de-145">При использовании code First разработка обычно начинается с написания классов .NET Framework, которые определяют концептуальную (доменную) модель.</span><span class="sxs-lookup"><span data-stu-id="dd6de-145">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="dd6de-146">Добавьте новый класс в **WPFwithEFSample:**</span><span class="sxs-lookup"><span data-stu-id="dd6de-146">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="dd6de-147">Нажмите справа на название проекта</span><span class="sxs-lookup"><span data-stu-id="dd6de-147">Right-click on the project name</span></span>
    -   <span data-ttu-id="dd6de-148">Выберите **Добавить,** затем **новый пункт**</span><span class="sxs-lookup"><span data-stu-id="dd6de-148">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="dd6de-149">Выберите **класс** и введите **продукт** для имени класса</span><span class="sxs-lookup"><span data-stu-id="dd6de-149">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="dd6de-150">Замените определение класса **продукта** следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="dd6de-150">Replace the **Product** class definition with the following code:</span></span>

``` csharp
    namespace WPFwithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }

-   Add a **Category** class with the following definition:

    using System.Collections.ObjectModel;

    namespace WPFwithEFSample
    {
        public class Category
        {
            public Category()
            {
                this.Products = new ObservableCollection<Product>();
            }

            public int CategoryId { get; set; }
            public string Name { get; set; }

            public virtual ObservableCollection<Product> Products { get; private set; }
        }
    }
```

<span data-ttu-id="dd6de-151">Свойство **Продуктов** **в категории** класса и **категории** в классе **Product** является навигационными свойствами.</span><span class="sxs-lookup"><span data-stu-id="dd6de-151">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="dd6de-152">В рамках сущности свойства навигации обеспечивают способ навигации между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="dd6de-152">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="dd6de-153">В дополнение к определению сущностей необходимо определить класс, который вытекает из&lt;DbContext и предоставляет свойства DbSet TEntity.&gt;</span><span class="sxs-lookup"><span data-stu-id="dd6de-153">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="dd6de-154">Свойства DbSet&lt;&gt; TEntity позволяют контексту знать, какие типы вы хотите включить в модель.</span><span class="sxs-lookup"><span data-stu-id="dd6de-154">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="dd6de-155">Экземпляр типа двузаемых DbContext управляет объектами сущности во время выполнения, что включает в себя заполнение объектов данными из базы данных, отслеживание изменений и сохраняющиеся данные в базу данных.</span><span class="sxs-lookup"><span data-stu-id="dd6de-155">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="dd6de-156">Добавьте в проект новый класс **ProductContext** со следующим определением:</span><span class="sxs-lookup"><span data-stu-id="dd6de-156">Add a new **ProductContext** class to the project with the following definition:</span></span>

``` csharp
    using System.Data.Entity;

    namespace WPFwithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

<span data-ttu-id="dd6de-157">Скомпилируйте проект.</span><span class="sxs-lookup"><span data-stu-id="dd6de-157">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="dd6de-158">Вариант 2: Определите модель с помощью базы данных First</span><span class="sxs-lookup"><span data-stu-id="dd6de-158">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="dd6de-159">В этом разделе показано, как использовать Database First для обратного проектирования модели из базы данных с помощью конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="dd6de-159">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="dd6de-160">Если вы завершили предыдущий раздел **(Вариант 1: Определите модель с помощью Code First),** то пропустите этот раздел и перейдите прямо в раздел **Lazy Loading.**</span><span class="sxs-lookup"><span data-stu-id="dd6de-160">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="dd6de-161">Создание существующей базы данных</span><span class="sxs-lookup"><span data-stu-id="dd6de-161">Create an Existing Database</span></span>

<span data-ttu-id="dd6de-162">Обычно при таргетинге на существующую базу данных она уже будет создана, но для этого пошагового мы должны создать базу данных для доступа.</span><span class="sxs-lookup"><span data-stu-id="dd6de-162">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="dd6de-163">Сервер базы данных, установленный visual Studio, отличается в зависимости от установленной версии Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="dd6de-163">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="dd6de-164">Если вы используете Visual Studio 2010, вы создаете базу данных S'L Express.</span><span class="sxs-lookup"><span data-stu-id="dd6de-164">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="dd6de-165">Если вы используете Visual Studio 2012, то вы будете создавать базу данных [LocalDB.](https://msdn.microsoft.com/library/hh510202.aspx)</span><span class="sxs-lookup"><span data-stu-id="dd6de-165">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="dd6de-166">Давайте идти вперед и генерировать базу данных.</span><span class="sxs-lookup"><span data-stu-id="dd6de-166">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="dd6de-167">**Просмотр&gt; - Исследователь серверов**</span><span class="sxs-lookup"><span data-stu-id="dd6de-167">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="dd6de-168">Нажмите правой кнопкой мыши на **подключение к данным -&gt; Добавить соединение...**</span><span class="sxs-lookup"><span data-stu-id="dd6de-168">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="dd6de-169">Если вы не подключились к базе данных от Server Explorer, прежде чем вам нужно будет выбрать сервер Microsoft S'L в качестве источника данных</span><span class="sxs-lookup"><span data-stu-id="dd6de-169">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Изменение источника данных](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="dd6de-171">Подключитесь к LocalDB или S'L Express, в зависимости от того, какой из них вы установили, и введите **Продукты** в качестве имени базы данных</span><span class="sxs-lookup"><span data-stu-id="dd6de-171">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Добавление connection LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Добавить соединение Экспресс](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="dd6de-174">Выберите **OK,** и вас спросят, хотите ли вы создать новую базу данных, выберите **Yes**</span><span class="sxs-lookup"><span data-stu-id="dd6de-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Создание базы данных](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="dd6de-176">Новая база данных теперь будет отображаться в Server Explorer, нажмите на нее правой кнопкой мыши и выберите **новый запрос**</span><span class="sxs-lookup"><span data-stu-id="dd6de-176">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="dd6de-177">Скопируйте следующий S'L в новый запрос, затем нажмите правой кнопкой мыши на запрос и выберите **Execute**</span><span class="sxs-lookup"><span data-stu-id="dd6de-177">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
    CREATE TABLE [dbo].[Categories] (
        [CategoryId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        CONSTRAINT [PK_dbo.Categories] PRIMARY KEY ([CategoryId])
    )

    CREATE TABLE [dbo].[Products] (
        [ProductId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        [CategoryId] [int] NOT NULL,
        CONSTRAINT [PK_dbo.Products] PRIMARY KEY ([ProductId])
    )

    CREATE INDEX [IX_CategoryId] ON [dbo].[Products]([CategoryId])

    ALTER TABLE [dbo].[Products] ADD CONSTRAINT [FK_dbo.Products_dbo.Categories_CategoryId] FOREIGN KEY ([CategoryId]) REFERENCES [dbo].[Categories] ([CategoryId]) ON DELETE CASCADE
```

#### <a name="reverse-engineer-model"></a><span data-ttu-id="dd6de-178">Обратная модель инженера</span><span class="sxs-lookup"><span data-stu-id="dd6de-178">Reverse Engineer Model</span></span>

<span data-ttu-id="dd6de-179">Мы собираемся использовать Entity Framework Designer, который включен в состав Visual Studio, для создания нашей модели.</span><span class="sxs-lookup"><span data-stu-id="dd6de-179">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="dd6de-180">**Проект&gt; - Добавить новый пункт...**</span><span class="sxs-lookup"><span data-stu-id="dd6de-180">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="dd6de-181">Выберите **данные** из левого меню, а затем **ADO.NET модель данных entity**</span><span class="sxs-lookup"><span data-stu-id="dd6de-181">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="dd6de-182">Введите **ProductModel** как имя и нажмите **OK**</span><span class="sxs-lookup"><span data-stu-id="dd6de-182">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="dd6de-183">Это запускает **Entity Data Model Мастер**</span><span class="sxs-lookup"><span data-stu-id="dd6de-183">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="dd6de-184">Выберите **генерировать из базы данных** и нажмите **далее**</span><span class="sxs-lookup"><span data-stu-id="dd6de-184">Select **Generate from Database** and click **Next**</span></span>

    ![Выбор содержимого модели](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="dd6de-186">Выберите подключение к базе данных, созданной в первом разделе, введите **ProductContext** в качестве названия строки соединения и нажмите **Далее**</span><span class="sxs-lookup"><span data-stu-id="dd6de-186">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Выберите соединение](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="dd6de-188">Нажмите на флажок рядом с "Таблицы", чтобы импортировать все таблицы и нажмите кнопку "Финиш"</span><span class="sxs-lookup"><span data-stu-id="dd6de-188">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Выберите объекты](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="dd6de-190">Как только процесс обратного инженера завершает, новая модель добавляется в ваш проект и открывается для просмотра в проекте Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="dd6de-190">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="dd6de-191">Файл App.config также был добавлен в ваш проект с сведениями о подключении к базе данных.</span><span class="sxs-lookup"><span data-stu-id="dd6de-191">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="dd6de-192">Дополнительные шаги в визуальной студии 2010</span><span class="sxs-lookup"><span data-stu-id="dd6de-192">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="dd6de-193">Если вы работаете в Visual Studio 2010, то вам нужно будет обновить EF-дизайнер, чтобы использовать генерацию кода EF6.</span><span class="sxs-lookup"><span data-stu-id="dd6de-193">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="dd6de-194">Нажмите правой кнопкой мыши на пустое место вашей модели в EF Designer и выберите **Добавить код поколения пункт ...**</span><span class="sxs-lookup"><span data-stu-id="dd6de-194">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="dd6de-195">Выберите **онлайн-шаблоны** из левого меню и поиск **DbContext**</span><span class="sxs-lookup"><span data-stu-id="dd6de-195">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="dd6de-196">Выберите **генератор EF 6.x DbContext для C,\#** введите **ProductsModel** в качестве имени и нажмите Добавить</span><span class="sxs-lookup"><span data-stu-id="dd6de-196">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="dd6de-197">Обновление генерации кода для привязки данных</span><span class="sxs-lookup"><span data-stu-id="dd6de-197">Updating code generation for data binding</span></span>

<span data-ttu-id="dd6de-198">EF генерирует код из модели с помощью шаблонов T4.</span><span class="sxs-lookup"><span data-stu-id="dd6de-198">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="dd6de-199">Шаблоны, отправленные visual Studio или загруженные из галереи Visual Studio, предназначены для общего использования.</span><span class="sxs-lookup"><span data-stu-id="dd6de-199">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="dd6de-200">Это означает, что сущности, генерируемые&lt;&gt; из этих шаблонов, имеют простые свойства ICollection T.</span><span class="sxs-lookup"><span data-stu-id="dd6de-200">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="dd6de-201">Однако при использовании связывания данных с помощью WPF желательно использовать **ObservableCollection** для сбора свойств, чтобы WPF мог отслеживать изменения, внесенные в коллекции.</span><span class="sxs-lookup"><span data-stu-id="dd6de-201">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="dd6de-202">С этой целью мы будем изменять шаблоны для использования ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="dd6de-202">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="dd6de-203">Откройте **Explorer решений** и найдите файл **ProductModel.edmx**</span><span class="sxs-lookup"><span data-stu-id="dd6de-203">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="dd6de-204">Найдите **ProductModel.tt** файл, который будет вложен под файл Омело.edmx</span><span class="sxs-lookup"><span data-stu-id="dd6de-204">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Шаблон модели продукции WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="dd6de-206">Дважды нажмите на файл ProductModel.tt, чтобы открыть его в редакторе Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dd6de-206">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="dd6de-207">Найти и заменить два случая "**ICollection**" с "**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="dd6de-207">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="dd6de-208">Они расположены примерно на линиях 296 и 484.</span><span class="sxs-lookup"><span data-stu-id="dd6de-208">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="dd6de-209">Найдите и замените первое появление «**HashSet**» на «**ObservableCollection**».</span><span class="sxs-lookup"><span data-stu-id="dd6de-209">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="dd6de-210">Это явление расположено примерно на линии 50.</span><span class="sxs-lookup"><span data-stu-id="dd6de-210">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="dd6de-211">**Не заменяйте** второе появление HashSet, найденное позже в коде.</span><span class="sxs-lookup"><span data-stu-id="dd6de-211">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="dd6de-212">Найти и заменить только появление "**System.Collections.Generic**" с "**System.Collections.ObjectModel**".</span><span class="sxs-lookup"><span data-stu-id="dd6de-212">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="dd6de-213">Это расположено примерно на линии 424.</span><span class="sxs-lookup"><span data-stu-id="dd6de-213">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="dd6de-214">Сохранить файл ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="dd6de-214">Save the ProductModel.tt file.</span></span> <span data-ttu-id="dd6de-215">Это должно привести к регенерированию кода для сущностей.</span><span class="sxs-lookup"><span data-stu-id="dd6de-215">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="dd6de-216">Если код не регенерируется автоматически, то нажмите правой кнопкой мыши на ProductModel.tt и выберите "Запуск пользовательского инструмента".</span><span class="sxs-lookup"><span data-stu-id="dd6de-216">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="dd6de-217">Если теперь вы открываете Category.cs файл (который вложен под ProductModel.tt), то вы должны увидеть, что коллекция продуктов имеет тип **&lt;ObservableCollection&gt;Product.**</span><span class="sxs-lookup"><span data-stu-id="dd6de-217">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="dd6de-218">Скомпилируйте проект.</span><span class="sxs-lookup"><span data-stu-id="dd6de-218">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="dd6de-219">Отложенная загрузка</span><span class="sxs-lookup"><span data-stu-id="dd6de-219">Lazy Loading</span></span>

<span data-ttu-id="dd6de-220">Свойство **Продуктов** **в категории** класса и **категории** в классе **Product** является навигационными свойствами.</span><span class="sxs-lookup"><span data-stu-id="dd6de-220">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="dd6de-221">В рамках сущности свойства навигации обеспечивают способ навигации между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="dd6de-221">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="dd6de-222">EF предоставляет вам возможность загрузки связанных объектов из базы данных автоматически при первом доступе к свойству навигации.</span><span class="sxs-lookup"><span data-stu-id="dd6de-222">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="dd6de-223">При этом типе загрузки (так называемой ленивой загрузки) следует знать, что при первом доступе к каждому навигационному свойству отдельный запрос будет выполнен в базе данных, если содержимое еще не в контексте.</span><span class="sxs-lookup"><span data-stu-id="dd6de-223">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="dd6de-224">При использовании типов сущностей POCO EF достигает ленивой загрузки, создавая экземпляры производных типов прокси во время выполнения, а затем переопределяет виртуальные свойства в классах, чтобы добавить погрузочный крюк.</span><span class="sxs-lookup"><span data-stu-id="dd6de-224">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="dd6de-225">Чтобы получить ленивый загрузки связанных объектов, вы должны объявить навигационные свойства getters как **общественные** и **виртуальные** **(Переизвлечимые** в Visual Basic), и вы класс не должны быть **запечатаны** **(Невимало в** Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="dd6de-225">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="dd6de-226">При использовании Database First навигационные свойства автоматически становятся виртуальными, чтобы включить ленивую загрузку.</span><span class="sxs-lookup"><span data-stu-id="dd6de-226">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="dd6de-227">В разделе Code First мы решили сделать навигационные свойства виртуальными по той же причине</span><span class="sxs-lookup"><span data-stu-id="dd6de-227">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="dd6de-228">Привязать объект к элементам управления</span><span class="sxs-lookup"><span data-stu-id="dd6de-228">Bind Object to Controls</span></span>

<span data-ttu-id="dd6de-229">Добавьте классы, определенные в модели, в качестве источников данных для этого приложения WPF.</span><span class="sxs-lookup"><span data-stu-id="dd6de-229">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="dd6de-230">Дважды щелкните **MainWindow.xaml** в Solution Explorer, чтобы открыть основную форму</span><span class="sxs-lookup"><span data-stu-id="dd6de-230">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="dd6de-231">Из основного меню выберите **проект -&gt; Добавьте новый источник данных ...**</span><span class="sxs-lookup"><span data-stu-id="dd6de-231">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="dd6de-232">(в Visual Studio 2010, вам нужно выбрать **данные -&gt; Добавить новый источник данных ...**)</span><span class="sxs-lookup"><span data-stu-id="dd6de-232">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="dd6de-233">В типе исходного окна исходного кода выберите **объект** и **нажмите далее**</span><span class="sxs-lookup"><span data-stu-id="dd6de-233">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="dd6de-234">В диалоге Select the Data Objects двараза разворачивайте **WPFwithEFSample** и выберите **категорию**</span><span class="sxs-lookup"><span data-stu-id="dd6de-234">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="dd6de-235">*Нет необходимости выбирать источник данных **Продукта,** потому что мы перейдем к нему через свойство **продукта**на источнике данных **категории***</span><span class="sxs-lookup"><span data-stu-id="dd6de-235">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![Выберите объекты данных](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="dd6de-237">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="dd6de-237">Click **Finish.**</span></span>
-   <span data-ttu-id="dd6de-238">Окно источников данных открывается рядом с окном MainWindow.xaml \*Если окно источников данных не отображается, выберите \*\*View -&gt; Другие источники данных Windows-&gt; \*\* \*</span><span class="sxs-lookup"><span data-stu-id="dd6de-238">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="dd6de-239">Нажмите значок штифта, чтобы окно источников данных не скрывалось автоматически.</span><span class="sxs-lookup"><span data-stu-id="dd6de-239">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="dd6de-240">Возможно, вам придется нажать кнопку обновления, если окно уже было видно.</span><span class="sxs-lookup"><span data-stu-id="dd6de-240">You may need to hit the refresh button if the window was already visible.</span></span>

    ![обозревателе решений](~/ef6/media/datasources.png)

-   <span data-ttu-id="dd6de-242">Выберите источник данных **категории** и перетащите его в форму.</span><span class="sxs-lookup"><span data-stu-id="dd6de-242">Select the **Category** data source and drag it on the form.</span></span>

<span data-ttu-id="dd6de-243">Следующее произошло, когда мы тащили этот источник:</span><span class="sxs-lookup"><span data-stu-id="dd6de-243">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="dd6de-244">Ресурс **ыиВитиИи и** **категорияУправлениеDataGrid** были добавлены в XAML</span><span class="sxs-lookup"><span data-stu-id="dd6de-244">The **categoryViewSource** resource and the **categoryDataGrid** control were added to XAML</span></span> 
-   <span data-ttu-id="dd6de-245">Свойство DataContext на элементе родительской сетки было настроено на **"Категорию StaticResourceViewSource".**</span><span class="sxs-lookup"><span data-stu-id="dd6de-245">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span><span data-ttu-id="dd6de-246">Ресурс **категорииViewSource** служит связующим\\источником для внешнего элемента родительской сетки.</span><span class="sxs-lookup"><span data-stu-id="dd6de-246"> The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="dd6de-247">Элементы внутренней сетки затем наследуют значение DataContext из родительской сетки (свойство категорииDataGrid's ItemsSource настроено на "Связывание")</span><span class="sxs-lookup"><span data-stu-id="dd6de-247">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}")</span></span>

``` xml
    <Window.Resources>
        <CollectionViewSource x:Key="categoryViewSource"
                                d:DesignSource="{d:DesignInstance {x:Type local:Category}, CreateList=True}"/>
    </Window.Resources>
    <Grid DataContext="{StaticResource categoryViewSource}">
        <DataGrid x:Name="categoryDataGrid" AutoGenerateColumns="False" EnableRowVirtualization="True"
                    ItemsSource="{Binding}" Margin="13,13,43,191"
                    RowDetailsVisibilityMode="VisibleWhenSelected">
            <DataGrid.Columns>
                <DataGridTextColumn x:Name="categoryIdColumn" Binding="{Binding CategoryId}"
                                    Header="Category Id" Width="SizeToHeader"/>
                <DataGridTextColumn x:Name="nameColumn" Binding="{Binding Name}"
                                    Header="Name" Width="SizeToHeader"/>
            </DataGrid.Columns>
        </DataGrid>
    </Grid>
```

## <a name="adding-a-details-grid"></a><span data-ttu-id="dd6de-248">Добавление сетки деталей</span><span class="sxs-lookup"><span data-stu-id="dd6de-248">Adding a Details Grid</span></span>

<span data-ttu-id="dd6de-249">Теперь, когда у нас есть сетка для отображения Категории давайте добавим сетку деталей для отображения связанных продуктов.</span><span class="sxs-lookup"><span data-stu-id="dd6de-249">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="dd6de-250">Выберите свойство **Продуктов** из-под источника данных **категории** и перетащите его в форму.</span><span class="sxs-lookup"><span data-stu-id="dd6de-250">Select the **Products** property from under the **Category** data source and drag it on the form.</span></span>
    -   <span data-ttu-id="dd6de-251">**КатегорияProductsViewSource** ресурс и **сетка productDataGrid** добавляются в XAML</span><span class="sxs-lookup"><span data-stu-id="dd6de-251">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="dd6de-252">Путь связывания для этого ресурса установлен для Продуктов</span><span class="sxs-lookup"><span data-stu-id="dd6de-252">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="dd6de-253">Платформа wPF, имеющая обязательную силу для данных, гарантирует, что только продукты, относящиеся к выбранной категории, отображясь в **productDataGrid**</span><span class="sxs-lookup"><span data-stu-id="dd6de-253">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="dd6de-254">От Toolbox перетащите **кнопку** на форму.</span><span class="sxs-lookup"><span data-stu-id="dd6de-254">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="dd6de-255">Установите свойство **имя** **кнопкиСохранить** и свойство **содержимого,** чтобы **сохранить.**</span><span class="sxs-lookup"><span data-stu-id="dd6de-255">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="dd6de-256">Форма должна выглядеть так же, как и эта:</span><span class="sxs-lookup"><span data-stu-id="dd6de-256">The form should look similar to this:</span></span>

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="dd6de-258">Добавление кода, который обрабатывает взаимодействие с данными</span><span class="sxs-lookup"><span data-stu-id="dd6de-258">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="dd6de-259">Пришло время добавить некоторые обработчики событий в основное окно.</span><span class="sxs-lookup"><span data-stu-id="dd6de-259">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="dd6de-260">В окне XAML нажмите на элемент \*\* &lt;window,\*\* это выбирает основное окно</span><span class="sxs-lookup"><span data-stu-id="dd6de-260">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="dd6de-261">В окне **Свойства** выберите **События** в правом верхнем виде, а затем дважды щелкните текстовый ящик справа от **загруженной** метки</span><span class="sxs-lookup"><span data-stu-id="dd6de-261">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![Основные свойства окна](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="dd6de-263">Также добавьте событие **Нажмите** для кнопки **«Сохранить»,** дважды нажав кнопку «Сохранить» в дизайнере.</span><span class="sxs-lookup"><span data-stu-id="dd6de-263">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="dd6de-264">Это подводит вас к коду позади для формы, теперь мы будем отодействовать код для использования ProductContext для выполнения доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="dd6de-264">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="dd6de-265">Обновите код для MainWindow, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="dd6de-265">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="dd6de-266">Код объявляет давний экземпляр **ProductContext.**</span><span class="sxs-lookup"><span data-stu-id="dd6de-266">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="dd6de-267">Объект **ProductContext** используется для запроса и сохранения данных в базе данных.</span><span class="sxs-lookup"><span data-stu-id="dd6de-267">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="dd6de-268">**Утилизированный ()** экземпляр **ProductContext** вызывается из перебора **метода OnClosing.**</span><span class="sxs-lookup"><span data-stu-id="dd6de-268">The **Dispose()** on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span><span data-ttu-id="dd6de-269">Комментарии к коду предоставляют подробную информацию о том, что делает код.</span><span class="sxs-lookup"><span data-stu-id="dd6de-269"> The code comments provide details about what the code does.</span></span>

``` csharp
    using System.Data.Entity;
    using System.Linq;
    using System.Windows;

    namespace WPFwithEFSample
    {
        public partial class MainWindow : Window
        {
            private ProductContext _context = new ProductContext();
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                System.Windows.Data.CollectionViewSource categoryViewSource =
                    ((System.Windows.Data.CollectionViewSource)(this.FindResource("categoryViewSource")));

                // Load is an extension method on IQueryable,
                // defined in the System.Data.Entity namespace.
                // This method enumerates the results of the query,
                // similar to ToList but without creating a list.
                // When used with Linq to Entities this method
                // creates entity objects and adds them to the context.
                _context.Categories.Load();

                // After the data is loaded call the DbSet<T>.Local property
                // to use the DbSet<T> as a binding source.
                categoryViewSource.Source = _context.Categories.Local;
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                // When you delete an object from the related entities collection
                // (in this case Products), the Entity Framework doesn’t mark
                // these child entities as deleted.
                // Instead, it removes the relationship between the parent and the child
                // by setting the parent reference to null.
                // So we manually have to delete the products
                // that have a Category reference set to null.

                // The following code uses LINQ to Objects
                // against the Local collection of Products.
                // The ToList call is required because otherwise the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can use LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                _context.SaveChanges();
                // Refresh the grids so the database generated values show up.
                this.categoryDataGrid.Items.Refresh();
                this.productsDataGrid.Items.Refresh();
            }

            protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }

    }
```

## <a name="test-the-wpf-application"></a><span data-ttu-id="dd6de-270">Протестировать приложение WPF</span><span class="sxs-lookup"><span data-stu-id="dd6de-270">Test the WPF Application</span></span>

-   <span data-ttu-id="dd6de-271">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="dd6de-271">Compile and run the application.</span></span> <span data-ttu-id="dd6de-272">Если вы использовали Code First, то вы увидите, что для вас создана база данных **WPFwithEFSample.ProductContext.**</span><span class="sxs-lookup"><span data-stu-id="dd6de-272">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="dd6de-273">Введите имя категории в верхней сетке и названия продуктов в нижней сетке *Не введите ничего в столбцах ID, потому что основной ключ генерируется базой данных*</span><span class="sxs-lookup"><span data-stu-id="dd6de-273">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Главное окно с новыми категориями и продуктами](~/ef6/media/screen1.png)

-   <span data-ttu-id="dd6de-275">Нажмите кнопку **«Сохранить»,** чтобы сохранить данные в базе данных</span><span class="sxs-lookup"><span data-stu-id="dd6de-275">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="dd6de-276">После вызова ВСУ **DbContext ()** идентионные данные заполняются значениями, генерируемыми базой данных.</span><span class="sxs-lookup"><span data-stu-id="dd6de-276">After the call to DbContext’s **SaveChanges()**, the IDs are populated with the database generated values.</span></span> <span data-ttu-id="dd6de-277">Потому что мы вызвали **Refresh()** после **SaveChanges()** элементы управления **DataGrid** обновляются с новыми значениями, а также.</span><span class="sxs-lookup"><span data-stu-id="dd6de-277">Because we called **Refresh()** after **SaveChanges()** the **DataGrid** controls are updated with the new values as well.</span></span>

![Главное окно с заселены идентимативными конторами](~/ef6/media/screen2.png)

## <a name="additional-resources"></a><span data-ttu-id="dd6de-279">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="dd6de-279">Additional Resources</span></span>

<span data-ttu-id="dd6de-280">Чтобы узнать больше о привязке [this topic](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) данных к коллекциям с помощью WPF, см.</span><span class="sxs-lookup"><span data-stu-id="dd6de-280">To learn more about data binding to collections using WPF, see [this topic](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) in the WPF documentation.</span></span>  
