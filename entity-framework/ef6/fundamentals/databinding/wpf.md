---
title: Привязка данных с помощью WPF-EF6
author: divega
ms.date: 05/19/2020
ms.assetid: e90d48e6-bea7785-47ef-b756-7b89cce4daf0
ms.openlocfilehash: c8e9e9a0810d8192e5184abbc7df2e74e4ee7edb
ms.sourcegitcommit: 59e3d5ce7dfb284457cf1c991091683b2d1afe9d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2020
ms.locfileid: "83672905"
---
# <a name="databinding-with-wpf"></a><span data-ttu-id="767e8-102">Привязка данных с помощью WPF</span><span class="sxs-lookup"><span data-stu-id="767e8-102">Databinding with WPF</span></span>

> [!IMPORTANT]
> <span data-ttu-id="767e8-103">**Этот документ является допустимым для WPF только на .NET Framework**</span><span class="sxs-lookup"><span data-stu-id="767e8-103">**This document is valid for WPF on the .NET Framework only**</span></span>
>
> <span data-ttu-id="767e8-104">В этом документе описывается привязка данных для WPF на .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="767e8-104">This document describes databinding for WPF on the .NET Framework.</span></span> <span data-ttu-id="767e8-105">Для новых проектов .NET Core рекомендуется использовать [EF Core](/ef/core) вместо Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="767e8-105">For new .NET Core projects, we recommend you use [EF Core](/ef/core) instead of Entity Framework 6.</span></span> <span data-ttu-id="767e8-106">Документация по DataBinding в EF Core выполняется в [#778е проблем](https://github.com/dotnet/EntityFramework.Docs/issues/778).</span><span class="sxs-lookup"><span data-stu-id="767e8-106">The documentation for databinding in EF Core is tracked in [Issue #778](https://github.com/dotnet/EntityFramework.Docs/issues/778).</span></span>

<span data-ttu-id="767e8-107">В этом пошаговом руководстве показано, как привязать типы POCO к элементам управления WPF в форме "основной/подробности".</span><span class="sxs-lookup"><span data-stu-id="767e8-107">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="767e8-108">Приложение использует Entity Framework API-интерфейсы для заполнения объектов данными из базы данных, трассировки изменений и сохранения данных в базе данных.</span><span class="sxs-lookup"><span data-stu-id="767e8-108">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="767e8-109">Модель определяет два типа, участвующих в связи "один ко многим": **Категория** (основная \\ Главная) и **продукт** (зависимые \\ Подробности).</span><span class="sxs-lookup"><span data-stu-id="767e8-109">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="767e8-110">Затем средства Visual Studio используются для привязки типов, определенных в модели, к элементам управления WPF.</span><span class="sxs-lookup"><span data-stu-id="767e8-110">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="767e8-111">Платформа привязки данных WPF позволяет переходить между связанными объектами: выбор строк в главном представлении приводит к обновлению подробного представления соответствующими дочерними данными.</span><span class="sxs-lookup"><span data-stu-id="767e8-111">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="767e8-112">Снимки экрана и списки кода в этом пошаговом руководстве взяты из Visual Studio 2013 но вы можете выполнить это пошаговое руководство с помощью Visual Studio 2012 или Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="767e8-112">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="767e8-113">Использование параметра "Object" для создания источников данных WPF</span><span class="sxs-lookup"><span data-stu-id="767e8-113">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="767e8-114">В предыдущей версии Entity Framework мы использовали, чтобы рекомендовать использовать параметр **базы данных** при создании нового источника данных на основе модели, созданной с помощью конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="767e8-114">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="767e8-115">Это было вызвано тем, что конструктор создаст контекст, производный от ObjectContext и классов сущностей, производных от EntityObject.</span><span class="sxs-lookup"><span data-stu-id="767e8-115">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="767e8-116">Использование параметра базы данных поможет написать лучший код для взаимодействия с этой поверхностью API.</span><span class="sxs-lookup"><span data-stu-id="767e8-116">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="767e8-117">Конструкторы EF для Visual Studio 2012 и Visual Studio 2013 создают контекст, производный от DbContext вместе с простыми классами сущностей POCO.</span><span class="sxs-lookup"><span data-stu-id="767e8-117">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="767e8-118">В Visual Studio 2010 мы рекомендуем переключаться на шаблон создания кода, использующий DbContext, как описано далее в этом пошаговом руководстве.</span><span class="sxs-lookup"><span data-stu-id="767e8-118">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="767e8-119">При использовании поверхности API DbContext следует использовать параметр **Object** при создании нового источника данных, как показано в этом пошаговом руководстве.</span><span class="sxs-lookup"><span data-stu-id="767e8-119">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="767e8-120">При необходимости можно [вернуться к созданию кода на основе ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) для моделей, созданных с помощью конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="767e8-120">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="767e8-121">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="767e8-121">Pre-Requisites</span></span>

<span data-ttu-id="767e8-122">Для работы с этим пошаговым руководством необходимо установить Visual Studio 2013, Visual Studio 2012 или Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="767e8-122">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="767e8-123">Если вы используете Visual Studio 2010, необходимо также установить NuGet.</span><span class="sxs-lookup"><span data-stu-id="767e8-123">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="767e8-124">Дополнительные сведения см. в разделе [Установка NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span><span class="sxs-lookup"><span data-stu-id="767e8-124">For more information, see [Installing NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="767e8-125">Создание приложения</span><span class="sxs-lookup"><span data-stu-id="767e8-125">Create the Application</span></span>

-   <span data-ttu-id="767e8-126">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="767e8-126">Open Visual Studio</span></span>
-   <span data-ttu-id="767e8-127">**Файл- &gt; создать &gt; проект...**</span><span class="sxs-lookup"><span data-stu-id="767e8-127">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="767e8-128">Выберите **окна**   в левой области и **впфаппликатион** на правой панели.</span><span class="sxs-lookup"><span data-stu-id="767e8-128">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="767e8-129">Введите **впфвисефсампле**в   качестве имени</span><span class="sxs-lookup"><span data-stu-id="767e8-129">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="767e8-130">Нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="767e8-130">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="767e8-131">Установка пакета NuGet Entity Framework</span><span class="sxs-lookup"><span data-stu-id="767e8-131">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="767e8-132">В обозреватель решений щелкните правой кнопкой мыши проект **винформсвисефсампле**</span><span class="sxs-lookup"><span data-stu-id="767e8-132">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="767e8-133">Выберите **Управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="767e8-133">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="767e8-134">В диалоговом окне Управление пакетами NuGet выберите вкладку в **сети** и выберите пакет **EntityFramework** .</span><span class="sxs-lookup"><span data-stu-id="767e8-134">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="767e8-135">Щелкните **Установить**.</span><span class="sxs-lookup"><span data-stu-id="767e8-135">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="767e8-136">В дополнение к сборке EntityFramework также добавляется ссылка на System. ComponentModel. Аннотация.</span><span class="sxs-lookup"><span data-stu-id="767e8-136">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="767e8-137">Если проект содержит ссылку на System. Data. Entity, то он будет удален при установке пакета EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="767e8-137">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="767e8-138">Сборка System. Data. Entity больше не используется для приложений Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="767e8-138">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="767e8-139">Определение модели</span><span class="sxs-lookup"><span data-stu-id="767e8-139">Define a Model</span></span>

<span data-ttu-id="767e8-140">В этом пошаговом руководстве можно выбрать реализацию модели с помощью Code First или конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="767e8-140">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="767e8-141">Выполните один из двух следующих разделов.</span><span class="sxs-lookup"><span data-stu-id="767e8-141">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="767e8-142">Вариант 1. Определение модели с помощью Code First</span><span class="sxs-lookup"><span data-stu-id="767e8-142">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="767e8-143">В этом разделе показано, как создать модель и связанную с ней базу данных с помощью Code First.</span><span class="sxs-lookup"><span data-stu-id="767e8-143">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="767e8-144">Перейдите к следующему разделу (**вариант 2. Определение модели с помощью Database First)** , если вы предпочитаете использовать Database First для реконструирования модели из базы данных с помощью конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="767e8-144">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="767e8-145">При использовании Code First разработки обычно начинается написание .NET Framework классов, определяющих концептуальную (доменную) модель.</span><span class="sxs-lookup"><span data-stu-id="767e8-145">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="767e8-146">Добавьте новый класс в **впфвисефсампле:**</span><span class="sxs-lookup"><span data-stu-id="767e8-146">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="767e8-147">Щелкните правой кнопкой мыши имя проекта</span><span class="sxs-lookup"><span data-stu-id="767e8-147">Right-click on the project name</span></span>
    -   <span data-ttu-id="767e8-148">Выберите **Добавить**, а затем **новый элемент** .</span><span class="sxs-lookup"><span data-stu-id="767e8-148">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="767e8-149">Выберите **класс** и введите **Product**   в качестве имени класса.</span><span class="sxs-lookup"><span data-stu-id="767e8-149">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="767e8-150">Замените **Product**   Определение класса Product следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="767e8-150">Replace the **Product** class definition with the following code:</span></span>

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

<span data-ttu-id="767e8-151">Свойство **Products** класса **Category** и свойство **Category** в классе **Product** являются свойствами навигации.</span><span class="sxs-lookup"><span data-stu-id="767e8-151">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="767e8-152">В Entity Framework свойства навигации предоставляют способ навигации по связям между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="767e8-152">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="767e8-153">Помимо определения сущностей, необходимо определить класс, производный от DbContext и предоставляющий свойства DbSetости &lt; &gt; .</span><span class="sxs-lookup"><span data-stu-id="767e8-153">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="767e8-154">&lt;Свойства домика DbSet &gt; позволяют контексту понять, какие типы необходимо включить в модель.</span><span class="sxs-lookup"><span data-stu-id="767e8-154">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="767e8-155">Экземпляр производного типа DbContext управляет объектами сущностей во время выполнения, включая заполнение объектов данными из базы данных, отслеживание изменений и сохранение данных в базе данных.</span><span class="sxs-lookup"><span data-stu-id="767e8-155">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="767e8-156">Добавьте в проект новый класс **продуктконтекст** со следующим определением:</span><span class="sxs-lookup"><span data-stu-id="767e8-156">Add a new **ProductContext** class to the project with the following definition:</span></span>

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

<span data-ttu-id="767e8-157">Скомпилируйте проект.</span><span class="sxs-lookup"><span data-stu-id="767e8-157">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="767e8-158">Вариант 2. Определение модели с помощью Database First</span><span class="sxs-lookup"><span data-stu-id="767e8-158">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="767e8-159">В этом разделе показано, как использовать Database First для реконструирования модели из базы данных с помощью конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="767e8-159">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="767e8-160">Если вы завершили предыдущий раздел (**вариант 1. Определение модели с помощью Code First)**, пропустите этот раздел и перейдите непосредственно к разделу " **ленивая загрузка** ".</span><span class="sxs-lookup"><span data-stu-id="767e8-160">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="767e8-161">Создание существующей базы данных</span><span class="sxs-lookup"><span data-stu-id="767e8-161">Create an Existing Database</span></span>

<span data-ttu-id="767e8-162">Обычно при использовании существующей базы данных она будет создана, но в этом пошаговом руководстве необходимо создать базу данных для доступа к ней.</span><span class="sxs-lookup"><span data-stu-id="767e8-162">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="767e8-163">Сервер базы данных, устанавливаемый с Visual Studio, отличается в зависимости от установленной версии Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="767e8-163">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="767e8-164">При использовании Visual Studio 2010 будет создаваться база данных SQL Express.</span><span class="sxs-lookup"><span data-stu-id="767e8-164">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="767e8-165">Если вы используете Visual Studio 2012, то будете создавать базу данных [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) .</span><span class="sxs-lookup"><span data-stu-id="767e8-165">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="767e8-166">Давайте создадим базу данных.</span><span class="sxs-lookup"><span data-stu-id="767e8-166">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="767e8-167">**Просмотр — &gt; Обозреватель сервера**</span><span class="sxs-lookup"><span data-stu-id="767e8-167">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="767e8-168">Щелкните правой кнопкой мыши **подключения к данным — &gt; Добавить подключение...**</span><span class="sxs-lookup"><span data-stu-id="767e8-168">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="767e8-169">Если вы не подключились к базе данных из обозреватель сервера, прежде чем нужно будет выбрать Microsoft SQL Server в качестве источника данных</span><span class="sxs-lookup"><span data-stu-id="767e8-169">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Изменение источника данных](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="767e8-171">Подключитесь к LocalDB или SQL Express, в зависимости от того, какой из них установлен, и введите **Products** в качестве имени базы данных.</span><span class="sxs-lookup"><span data-stu-id="767e8-171">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Добавить соединение LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Добавить подключение Express](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="767e8-174">Нажмите кнопку **ОК** , и появится запрос на создание новой базы данных, выберите **Да** .</span><span class="sxs-lookup"><span data-stu-id="767e8-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Создание базы данных](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="767e8-176">Новая база данных появится в обозреватель сервера, щелкните ее правой кнопкой мыши и выберите пункт **создать запрос** .</span><span class="sxs-lookup"><span data-stu-id="767e8-176">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="767e8-177">Скопируйте следующий SQL в новый запрос, щелкните запрос правой кнопкой мыши и выберите команду **выполнить** .</span><span class="sxs-lookup"><span data-stu-id="767e8-177">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="767e8-178">Модель реконструирования</span><span class="sxs-lookup"><span data-stu-id="767e8-178">Reverse Engineer Model</span></span>

<span data-ttu-id="767e8-179">Мы будем использовать Entity Framework Designer, который входит в состав Visual Studio, для создания нашей модели.</span><span class="sxs-lookup"><span data-stu-id="767e8-179">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="767e8-180">**Проект- &gt; Добавить новый элемент...**</span><span class="sxs-lookup"><span data-stu-id="767e8-180">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="767e8-181">Выберите **данные** в меню слева, а затем **ADO.NET EDM**</span><span class="sxs-lookup"><span data-stu-id="767e8-181">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="767e8-182">Введите **ProductModel** в качестве имени и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="767e8-182">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="767e8-183">Запустится **мастер EDM**</span><span class="sxs-lookup"><span data-stu-id="767e8-183">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="767e8-184">Выберите **создать из базы данных** и нажмите кнопку **Далее** .</span><span class="sxs-lookup"><span data-stu-id="767e8-184">Select **Generate from Database** and click **Next**</span></span>

    ![Выбор содержимого модели](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="767e8-186">Выберите подключение к базе данных, созданной в первом разделе, введите **продуктконтекст** в качестве имени строки подключения и нажмите кнопку **Далее** .</span><span class="sxs-lookup"><span data-stu-id="767e8-186">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Выбор подключения](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="767e8-188">Установите флажок рядом с пунктом "таблицы", чтобы импортировать все таблицы, и нажмите кнопку "Готово".</span><span class="sxs-lookup"><span data-stu-id="767e8-188">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Выбор объектов](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="767e8-190">После завершения процесса реконструирования новая модель добавляется в проект и открывается для просмотра в Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="767e8-190">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="767e8-191">Файл App. config также был добавлен в проект со сведениями о соединении для базы данных.</span><span class="sxs-lookup"><span data-stu-id="767e8-191">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="767e8-192">Дополнительные действия в Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="767e8-192">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="767e8-193">При работе в Visual Studio 2010 необходимо обновить конструктор EF для использования создания кода EF6.</span><span class="sxs-lookup"><span data-stu-id="767e8-193">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="767e8-194">Щелкните правой кнопкой мыши пустое место в модели в конструкторе EF и выберите пункт **Добавить элемент создания кода...**</span><span class="sxs-lookup"><span data-stu-id="767e8-194">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="767e8-195">В меню слева выберите **шаблоны в Интернете** и выполните поиск по запросу **DbContext**</span><span class="sxs-lookup"><span data-stu-id="767e8-195">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="767e8-196">Выберите **генератор EF 6. x DbContext для C \# ,** введите **продуктсмодел** в качестве имени и нажмите кнопку Добавить.</span><span class="sxs-lookup"><span data-stu-id="767e8-196">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="767e8-197">Обновление создания кода для привязки данных</span><span class="sxs-lookup"><span data-stu-id="767e8-197">Updating code generation for data binding</span></span>

<span data-ttu-id="767e8-198">EF создает код из модели с помощью шаблонов T4.</span><span class="sxs-lookup"><span data-stu-id="767e8-198">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="767e8-199">Шаблоны, поставляемые с Visual Studio или скачанные из коллекции Visual Studio, предназначены для использования в общих целях.</span><span class="sxs-lookup"><span data-stu-id="767e8-199">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="767e8-200">Это означает, что сущности, созданные из этих шаблонов, имеют &lt; простые &gt; свойства ICollection T.</span><span class="sxs-lookup"><span data-stu-id="767e8-200">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="767e8-201">Однако при выполнении привязки данных с помощью WPF желательно использовать **ObservableCollection** для свойств коллекции, чтобы WPF мог вести отслеживание изменений, внесенных в коллекции.</span><span class="sxs-lookup"><span data-stu-id="767e8-201">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="767e8-202">К этому концу мы будем изменять шаблоны для использования ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="767e8-202">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="767e8-203">Откройте **Обозреватель решений** и найдите файл **ProductModel. EDMX**</span><span class="sxs-lookup"><span data-stu-id="767e8-203">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="767e8-204">Найдите файл **ProductModel.TT** , который будет вложен в файл ProductModel. EDMX</span><span class="sxs-lookup"><span data-stu-id="767e8-204">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Шаблон модели продукта WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="767e8-206">Дважды щелкните файл ProductModel.tt, чтобы открыть его в редакторе Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="767e8-206">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="767e8-207">Найдите и замените два вхождения "**ICollection**" на "**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="767e8-207">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="767e8-208">Они расположены примерно в строках 296 и 484.</span><span class="sxs-lookup"><span data-stu-id="767e8-208">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="767e8-209">Найдите и замените первое вхождение "**hashing**" на "**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="767e8-209">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="767e8-210">Это событие находится примерно в строке 50.</span><span class="sxs-lookup"><span data-stu-id="767e8-210">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="767e8-211">**Не** заменяйте второе вхождение набора хэширования, найденного далее в коде.</span><span class="sxs-lookup"><span data-stu-id="767e8-211">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="767e8-212">Поиск и замена единственного вхождения "**System. Collections. Generic**" на "**System. Collections. ObjectModel**".</span><span class="sxs-lookup"><span data-stu-id="767e8-212">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="767e8-213">Он расположен примерно в строке 424.</span><span class="sxs-lookup"><span data-stu-id="767e8-213">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="767e8-214">Сохраните файл ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="767e8-214">Save the ProductModel.tt file.</span></span> <span data-ttu-id="767e8-215">Это должно привести к повторному формированию кода для сущностей.</span><span class="sxs-lookup"><span data-stu-id="767e8-215">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="767e8-216">Если код не воссоздается автоматически, щелкните правой кнопкой мыши ProductModel.tt и выберите команду "запустить пользовательский инструмент".</span><span class="sxs-lookup"><span data-stu-id="767e8-216">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="767e8-217">Если теперь открыть файл Category.cs (который вложен в ProductModel.tt), вы должны увидеть, что коллекция Products имеет тип " \*\* &lt; &gt; продукт ObservableCollection\*\*".</span><span class="sxs-lookup"><span data-stu-id="767e8-217">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="767e8-218">Скомпилируйте проект.</span><span class="sxs-lookup"><span data-stu-id="767e8-218">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="767e8-219">Отложенная загрузка</span><span class="sxs-lookup"><span data-stu-id="767e8-219">Lazy Loading</span></span>

<span data-ttu-id="767e8-220">Свойство **Products** класса **Category** и свойство **Category** в классе **Product** являются свойствами навигации.</span><span class="sxs-lookup"><span data-stu-id="767e8-220">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="767e8-221">В Entity Framework свойства навигации предоставляют способ навигации по связям между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="767e8-221">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="767e8-222">EF дает возможность автоматически загружать связанные сущности из базы данных при первом доступе к свойству навигации.</span><span class="sxs-lookup"><span data-stu-id="767e8-222">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="767e8-223">При таком типе загрузки (называемой отложенной загрузкой) имейте в виду, что при первом обращении к каждому свойству навигации будет выполнен отдельный запрос к базе данных, если содержимое еще не находится в контексте.</span><span class="sxs-lookup"><span data-stu-id="767e8-223">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="767e8-224">При использовании типов сущностей POCO EF достигает отложенной загрузки, создавая экземпляры производных прокси-типов во время выполнения, а затем переопределяя виртуальные свойства в классах для добавления обработчика загрузки.</span><span class="sxs-lookup"><span data-stu-id="767e8-224">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="767e8-225">Чтобы получить отложенную загрузку связанных объектов, необходимо объявить методы считывания свойств навигации как **открытые** и **Виртуальные** (**переопределяемые** в Visual Basic), и класс не должен быть **запечатанным** (**NotOverridable** в Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="767e8-225">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="767e8-226">При использовании Database First свойства навигации автоматически становятся виртуальными для включения отложенной загрузки.</span><span class="sxs-lookup"><span data-stu-id="767e8-226">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="767e8-227">В разделе Code First мы решили сделать свойства навигации виртуальными по той же причине.</span><span class="sxs-lookup"><span data-stu-id="767e8-227">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="767e8-228">Привязка объекта к элементам управления</span><span class="sxs-lookup"><span data-stu-id="767e8-228">Bind Object to Controls</span></span>

<span data-ttu-id="767e8-229">Добавьте классы, определенные в модели, в качестве источников данных для этого приложения WPF.</span><span class="sxs-lookup"><span data-stu-id="767e8-229">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="767e8-230">Дважды щелкните файл **MainWindow. XAML** в Обозреватель решений, чтобы открыть главную форму.</span><span class="sxs-lookup"><span data-stu-id="767e8-230">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="767e8-231">В главном меню выберите **проект — &gt; Добавить новый источник данных...**</span><span class="sxs-lookup"><span data-stu-id="767e8-231">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="767e8-232">(в Visual Studio 2010 необходимо выбрать **данные — &gt; Добавить новый источник данных...**)</span><span class="sxs-lookup"><span data-stu-id="767e8-232">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="767e8-233">В поле выберите источник данных Типевиндов выберите **объект** и нажмите кнопку **Далее** .</span><span class="sxs-lookup"><span data-stu-id="767e8-233">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="767e8-234">В диалоговом окне Выбор объектов данных unfold **впфвисефсампле**   два раза и выберите Category ( **Категория** ).</span><span class="sxs-lookup"><span data-stu-id="767e8-234">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="767e8-235">*Нет необходимости выбирать источник данных **продукта** , так как мы будем приступить к нему через свойство **продукта**в источнике данных **категории** .*</span><span class="sxs-lookup"><span data-stu-id="767e8-235">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![Выбор объектов данных](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="767e8-237">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="767e8-237">Click **Finish.**</span></span>
-   <span data-ttu-id="767e8-238">Окно Источники данных открывается рядом с окном MainWindow. XAML \*, если окно Источники данных не отображается, выберите **вид — &gt; другие &gt; Источники данных Windows** \* .</span><span class="sxs-lookup"><span data-stu-id="767e8-238">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="767e8-239">Нажмите значок закрепить, чтобы окно Источники данных не скрывалось автоматически.</span><span class="sxs-lookup"><span data-stu-id="767e8-239">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="767e8-240">Если окно уже видимо, может потребоваться нажать кнопку Обновить.</span><span class="sxs-lookup"><span data-stu-id="767e8-240">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Data Sources](~/ef6/media/datasources.png)

-   <span data-ttu-id="767e8-242">Выберите источник данных **Категория** и перетащите его в форму.</span><span class="sxs-lookup"><span data-stu-id="767e8-242">Select the **Category** data source and drag it on the form.</span></span>

<span data-ttu-id="767e8-243">При перетаскивании этого источника произошло следующее:</span><span class="sxs-lookup"><span data-stu-id="767e8-243">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="767e8-244">Ресурс **категоривиевсаурце** и элемент управления **категоридатагрид** были добавлены в XAML.</span><span class="sxs-lookup"><span data-stu-id="767e8-244">The **categoryViewSource** resource and the **categoryDataGrid** control were added to XAML</span></span> 
-   <span data-ttu-id="767e8-245">Свойству DataContext родительского элемента Grid присвоено значение "{StaticResource **категоривиевсаурце** }".</span><span class="sxs-lookup"><span data-stu-id="767e8-245">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span><span data-ttu-id="767e8-246">Ресурс **категоривиевсаурце** выступает в качестве источника привязки для внешнего \\ родительского элемента Grid.</span><span class="sxs-lookup"><span data-stu-id="767e8-246"> The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="767e8-247">Внутренние элементы сетки затем наследуют значение DataContext от родительской сетки (свойство ItemsSource Категоридатагрид имеет значение {Binding}).</span><span class="sxs-lookup"><span data-stu-id="767e8-247">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}")</span></span>

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

## <a name="adding-a-details-grid"></a><span data-ttu-id="767e8-248">Добавление сетки сведений</span><span class="sxs-lookup"><span data-stu-id="767e8-248">Adding a Details Grid</span></span>

<span data-ttu-id="767e8-249">Теперь, когда у нас есть сетка для вывода категорий, давайте добавим сетку сведений для вывода связанных продуктов.</span><span class="sxs-lookup"><span data-stu-id="767e8-249">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="767e8-250">Выберите свойство **Products** из источника данных **Категория** и перетащите его в форму.</span><span class="sxs-lookup"><span data-stu-id="767e8-250">Select the **Products** property from under the **Category** data source and drag it on the form.</span></span>
    -   <span data-ttu-id="767e8-251">Ресурс **категорипродуктсвиевсаурце** и сетка **продуктдатагрид** добавляются в XAML.</span><span class="sxs-lookup"><span data-stu-id="767e8-251">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="767e8-252">Для пути привязки для этого ресурса задано значение Products</span><span class="sxs-lookup"><span data-stu-id="767e8-252">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="767e8-253">Платформа привязки данных WPF гарантирует, что в **продуктдатагрид** отображаются только продукты, связанные с выбранной категорией.</span><span class="sxs-lookup"><span data-stu-id="767e8-253">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="767e8-254">Из панели элементов перетащите **кнопку** в форму.</span><span class="sxs-lookup"><span data-stu-id="767e8-254">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="767e8-255">Задайте для свойства **Name** значение **Буттонсаве** и свойство **содержимого** для **сохранения**.</span><span class="sxs-lookup"><span data-stu-id="767e8-255">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="767e8-256">Форма должна выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="767e8-256">The form should look similar to this:</span></span>

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="767e8-258">Добавление кода, обрабатывающего взаимодействие с данными</span><span class="sxs-lookup"><span data-stu-id="767e8-258">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="767e8-259">Пора добавить в главное окно несколько обработчиков событий.</span><span class="sxs-lookup"><span data-stu-id="767e8-259">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="767e8-260">В окне XAML щелкните элемент \*\* &lt; Window\*\* , чтобы выбрать главное окно.</span><span class="sxs-lookup"><span data-stu-id="767e8-260">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="767e8-261">В окне **Свойства** выберите **события** в правом верхнем углу, а затем дважды щелкните текстовое поле справа от **загруженной** метки.</span><span class="sxs-lookup"><span data-stu-id="767e8-261">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![Свойства главного окна](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="767e8-263">Также добавьте событие **Click** для кнопки **сохранить** , дважды щелкнув кнопку Сохранить в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="767e8-263">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="767e8-264">В результате вы перейдете к коду программной части формы. Теперь мы выполним редактирование кода, чтобы использовать Продуктконтекст для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="767e8-264">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="767e8-265">Обновите код для MainWindow, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="767e8-265">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="767e8-266">Код объявляет долго выполняющийся экземпляр **продуктконтекст**.</span><span class="sxs-lookup"><span data-stu-id="767e8-266">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="767e8-267">Объект **продуктконтекст** используется для запроса и сохранения данных в базе данных.</span><span class="sxs-lookup"><span data-stu-id="767e8-267">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="767e8-268">Метод **Dispose ()** в экземпляре **продуктконтекст** вызывается из переопределенного метода **onclosinging** .</span><span class="sxs-lookup"><span data-stu-id="767e8-268">The **Dispose()** on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span><span data-ttu-id="767e8-269">Комментарии к коду содержат сведения о том, что делает код.</span><span class="sxs-lookup"><span data-stu-id="767e8-269"> The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-wpf-application"></a><span data-ttu-id="767e8-270">Тестирование приложения WPF</span><span class="sxs-lookup"><span data-stu-id="767e8-270">Test the WPF Application</span></span>

-   <span data-ttu-id="767e8-271">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="767e8-271">Compile and run the application.</span></span> <span data-ttu-id="767e8-272">Если вы использовали Code First, вы увидите, что создана база данных **впфвисефсампле. продуктконтекст** .</span><span class="sxs-lookup"><span data-stu-id="767e8-272">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="767e8-273">Введите имя категории в верхней сетке, а названия продуктов в нижней сетке *не вводят ничего в столбцы идентификаторов, так как первичный ключ создается базой данных* .</span><span class="sxs-lookup"><span data-stu-id="767e8-273">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Главное окно с новыми категориями и продуктами](~/ef6/media/screen1.png)

-   <span data-ttu-id="767e8-275">Нажмите кнопку " **сохранить** ", чтобы сохранить данные в базе данных</span><span class="sxs-lookup"><span data-stu-id="767e8-275">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="767e8-276">После вызова метода **SaveChanges ()** DbContext идентификаторы заполняются сформированными значениями базы данных.</span><span class="sxs-lookup"><span data-stu-id="767e8-276">After the call to DbContext’s **SaveChanges()**, the IDs are populated with the database generated values.</span></span> <span data-ttu-id="767e8-277">Так как мы назвали метод **Refresh ()** после вызова метода **SaveChanges ()** , элементы управления **DataGrid** также обновляются вместе с новыми значениями.</span><span class="sxs-lookup"><span data-stu-id="767e8-277">Because we called **Refresh()** after **SaveChanges()** the **DataGrid** controls are updated with the new values as well.</span></span>

![Главное окно с заполненными идентификаторами](~/ef6/media/screen2.png)

## <a name="additional-resources"></a><span data-ttu-id="767e8-279">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="767e8-279">Additional Resources</span></span>

<span data-ttu-id="767e8-280">Дополнительные сведения о привязке данных к коллекциям с помощью WPF см. в [этом разделе](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) документации по WPF.</span><span class="sxs-lookup"><span data-stu-id="767e8-280">To learn more about data binding to collections using WPF, see [this topic](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) in the WPF documentation.</span></span>  
