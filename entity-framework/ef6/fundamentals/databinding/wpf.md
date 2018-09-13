---
title: Привязка данных с помощью WPF - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 5bd4a9b98a12de41e4ec37c2cc7dbdc537210893
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490237"
---
# <a name="databinding-with-wpf"></a><span data-ttu-id="1f318-102">Привязка данных с помощью WPF</span><span class="sxs-lookup"><span data-stu-id="1f318-102">Databinding with WPF</span></span>
<span data-ttu-id="1f318-103">Это пошаговое руководство показывает, как выполнить привязку к элементам управления WPF в форме «основной / подробности» типов POCO.</span><span class="sxs-lookup"><span data-stu-id="1f318-103">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="1f318-104">Приложение использует API-интерфейсов Entity Framework для заполнения объектов с данными из базы данных, отслеживать изменения и сохранения данных в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1f318-104">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="1f318-105">Модель определяет два типа, участвующие в связи один ко многим: **категории** (основной\\master) и **продукта** (зависимые\\подробно).</span><span class="sxs-lookup"><span data-stu-id="1f318-105">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="1f318-106">Затем в Visual Studio tools используются для привязки типов, определенных в модели, чтобы элементы управления WPF.</span><span class="sxs-lookup"><span data-stu-id="1f318-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="1f318-107">Инфраструктура привязки данных WPF позволяет осуществлять навигацию между связанными объектами: Выбор строк в главное представление приводит представлении «Подробности» для обновления с соответствующих дочерних данных.</span><span class="sxs-lookup"><span data-stu-id="1f318-107">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="1f318-108">Снимки экрана и примеры кода в этом пошаговом руководстве, взяты из Visual Studio 2013, но можно выполнить в этом пошаговом руководстве с помощью Visual Studio 2012 или Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="1f318-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="1f318-109">Используйте параметр «Object» для создания источников данных в WPF</span><span class="sxs-lookup"><span data-stu-id="1f318-109">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="1f318-110">С помощью предыдущей версии Entity Framework мы использовали рекомендуем использовать **базы данных** параметр при создании нового источника данных на основе модели создан с помощью конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="1f318-110">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="1f318-111">Это было, так как конструктор создаст контекст, производный от ObjectContext и классов сущностей, производные от класса EntityObject.</span><span class="sxs-lookup"><span data-stu-id="1f318-111">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="1f318-112">С помощью параметра базы данных поможет вам создавать лучший код для взаимодействия с этой области API.</span><span class="sxs-lookup"><span data-stu-id="1f318-112">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="1f318-113">Конструкторы для Visual Studio 2012 и Visual Studio 2013 EF создать контекст, который является производным от DbContext вместе с простых классов сущностей POCO.</span><span class="sxs-lookup"><span data-stu-id="1f318-113">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="1f318-114">С помощью Visual Studio 2010, мы рекомендуем обращений шаблон генерации кода, использующего DbContext, как описано далее в этом пошаговом руководстве.</span><span class="sxs-lookup"><span data-stu-id="1f318-114">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="1f318-115">При использовании рабочей области DbContext API следует использовать **объект** параметр при создании нового источника данных, как показано в этом пошаговом руководстве.</span><span class="sxs-lookup"><span data-stu-id="1f318-115">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="1f318-116">При необходимости вы можете [вернуться к создание кода на основе ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) для моделей, созданных с помощью конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="1f318-116">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="1f318-117">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="1f318-117">Pre-Requisites</span></span>

<span data-ttu-id="1f318-118">Вам потребуется Visual Studio 2013, Visual Studio 2012 или Visual Studio 2010 установлен для выполнения этого пошагового руководства.</span><span class="sxs-lookup"><span data-stu-id="1f318-118">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="1f318-119">Если вы используете Visual Studio 2010, необходимо также установить NuGet.</span><span class="sxs-lookup"><span data-stu-id="1f318-119">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="1f318-120">Дополнительные сведения см. в разделе [Установка NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="1f318-120">For more information, see [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="1f318-121">Создание приложения</span><span class="sxs-lookup"><span data-stu-id="1f318-121">Create the Application</span></span>

-   <span data-ttu-id="1f318-122">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1f318-122">Open Visual Studio</span></span>
-   <span data-ttu-id="1f318-123">**Файл —&gt; Новинка —&gt; проекта...**</span><span class="sxs-lookup"><span data-stu-id="1f318-123">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="1f318-124">Выберите **Windows** в левой области и **WPFApplication** в области справа</span><span class="sxs-lookup"><span data-stu-id="1f318-124">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="1f318-125">Введите **WPFwithEFSample** как имя</span><span class="sxs-lookup"><span data-stu-id="1f318-125">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="1f318-126">Нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="1f318-126">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="1f318-127">Установите пакет Entity Framework NuGet</span><span class="sxs-lookup"><span data-stu-id="1f318-127">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="1f318-128">В обозревателе решений щелкните правой кнопкой мыши **WinFormswithEFSample** проекта</span><span class="sxs-lookup"><span data-stu-id="1f318-128">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="1f318-129">Выберите **управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="1f318-129">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="1f318-130">В диалоговом окне «Управление пакетами NuGet» выберите **Online** вкладку и выберите **EntityFramework** пакета</span><span class="sxs-lookup"><span data-stu-id="1f318-130">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="1f318-131">Нажмите кнопку **установки**</span><span class="sxs-lookup"><span data-stu-id="1f318-131">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="1f318-132">Помимо сборки EntityFramework также добавляется ссылка System.ComponentModel.DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="1f318-132">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="1f318-133">Если проект содержит ссылку на System.Data.Entity, затем он будет удален при установке EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="1f318-133">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="1f318-134">Сборка System.Data.Entity больше не используется для приложений Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="1f318-134">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="1f318-135">Определение модели</span><span class="sxs-lookup"><span data-stu-id="1f318-135">Define a Model</span></span>

<span data-ttu-id="1f318-136">В этом пошаговом руководстве, вы можете решил реализовать модель с помощью Code First или конструкторе EF.</span><span class="sxs-lookup"><span data-stu-id="1f318-136">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="1f318-137">Выполните одно из двух следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="1f318-137">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="1f318-138">Вариант 1: Определение модели с помощью Code First</span><span class="sxs-lookup"><span data-stu-id="1f318-138">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="1f318-139">В этом разделе показано, как создать модель и его связанные базы данных, с помощью Code First.</span><span class="sxs-lookup"><span data-stu-id="1f318-139">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="1f318-140">Перейдите к следующему разделу (**вариант 2: определение модели с помощью Database First)** Если вы предпочитаете использовать Database First обратить реконструирование модели из базы данных в конструкторе EF</span><span class="sxs-lookup"><span data-stu-id="1f318-140">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="1f318-141">При использовании шаблона разработки Code First обычно начинается с написания классов .NET Framework, которые определяют модель концептуальный (домен).</span><span class="sxs-lookup"><span data-stu-id="1f318-141">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="1f318-142">Добавьте новый класс к **WPFwithEFSample:**</span><span class="sxs-lookup"><span data-stu-id="1f318-142">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="1f318-143">Щелкните правой кнопкой мыши имя проекта</span><span class="sxs-lookup"><span data-stu-id="1f318-143">Right-click on the project name</span></span>
    -   <span data-ttu-id="1f318-144">Выберите **добавить**, затем **новый элемент**</span><span class="sxs-lookup"><span data-stu-id="1f318-144">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="1f318-145">Выберите **класс** и введите **продукта** имени класса</span><span class="sxs-lookup"><span data-stu-id="1f318-145">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="1f318-146">Замените **продукта** класса определения следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="1f318-146">Replace the **Product** class definition with the following code:</span></span>

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

<span data-ttu-id="1f318-147">**Продуктов** свойство **категории** класс и **категории** свойство **продукта** класс — это свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="1f318-147">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="1f318-148">В Entity Framework свойства навигации позволяют для перехода по связи между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="1f318-148">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="1f318-149">Кроме определения сущностей, необходимо определить класс, который является производным от DbContext и DbSet предоставляет&lt;TEntity&gt; свойства.</span><span class="sxs-lookup"><span data-stu-id="1f318-149">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="1f318-150">DbSet&lt;TEntity&gt; свойства let контекст знать, какие типы, которые вы хотите включить в модель.</span><span class="sxs-lookup"><span data-stu-id="1f318-150">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="1f318-151">Экземпляр типа DbContext производным управляет объектов сущности во время выполнения, который включает заполнение объектов с данными из базы данных, изменение отслеживания и сохранения данных в базу данных.</span><span class="sxs-lookup"><span data-stu-id="1f318-151">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="1f318-152">Добавьте новый **ProductContext** класс в проект со следующим определением:</span><span class="sxs-lookup"><span data-stu-id="1f318-152">Add a new **ProductContext** class to the project with the following definition:</span></span>

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

<span data-ttu-id="1f318-153">Скомпилируйте проект.</span><span class="sxs-lookup"><span data-stu-id="1f318-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="1f318-154">Вариант 2: Определение модели с помощью Database First</span><span class="sxs-lookup"><span data-stu-id="1f318-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="1f318-155">В этом разделе демонстрируется использование Database First чтобы реконструировать модель из базы данных в конструкторе EF.</span><span class="sxs-lookup"><span data-stu-id="1f318-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="1f318-156">Если действия в предыдущем разделе (**вариант 1: определение модели с помощью Code First)**, пропустите этот раздел и перейти непосредственно к **отложенной загрузки** раздел.</span><span class="sxs-lookup"><span data-stu-id="1f318-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="1f318-157">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="1f318-157">Create an Existing Database</span></span>

<span data-ttu-id="1f318-158">Обычно при ориентировании существующей базы данных, он будет уже создан, но для этого пошагового руководства необходимо создать базу данных для доступа к.</span><span class="sxs-lookup"><span data-stu-id="1f318-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="1f318-159">Сервер базы данных, который устанавливается вместе с Visual Studio отличается в зависимости от версии Visual Studio, вы установили:</span><span class="sxs-lookup"><span data-stu-id="1f318-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="1f318-160">Если вы используете Visual Studio 2010 вы создадите базу данных SQL Express.</span><span class="sxs-lookup"><span data-stu-id="1f318-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="1f318-161">Если вы используете Visual Studio 2012, а затем вы создадите [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) базы данных.</span><span class="sxs-lookup"><span data-stu-id="1f318-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="1f318-162">Перейдем дальше и создать базу данных.</span><span class="sxs-lookup"><span data-stu-id="1f318-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="1f318-163">**Представление —&gt; обозревателя серверов**</span><span class="sxs-lookup"><span data-stu-id="1f318-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="1f318-164">Щелкните правой кнопкой мыши **подключения к данным -&gt; добавить соединение...**</span><span class="sxs-lookup"><span data-stu-id="1f318-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="1f318-165">Если вы не подключились к базе данных с помощью обозревателя сервера прежде, чем вам потребуется выбрать в качестве источника данных Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="1f318-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Изменение источника данных](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="1f318-167">Подключение к LocalDB или SQL Express, в зависимости от того, какой из них вы установили и введите **продуктов** имя базы данных</span><span class="sxs-lookup"><span data-stu-id="1f318-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Добавить подключение LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Добавление подключения Express](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="1f318-170">Выберите **ОК** и вам нужно будет Если вы хотите создать новую базу данных, выберите **Да**</span><span class="sxs-lookup"><span data-stu-id="1f318-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Создание базы данных](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="1f318-172">Новой базы данных будут отображаться в обозревателе сервера щелкните его правой кнопкой мыши и выберите **новый запрос**</span><span class="sxs-lookup"><span data-stu-id="1f318-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="1f318-173">Скопируйте следующий запрос SQL в новый запрос, а затем щелкните правой кнопкой мыши запрос и выберите **Execute**</span><span class="sxs-lookup"><span data-stu-id="1f318-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="1f318-174">Реконструирование модели</span><span class="sxs-lookup"><span data-stu-id="1f318-174">Reverse Engineer Model</span></span>

<span data-ttu-id="1f318-175">Мы собираемся использовать Entity Framework Designer, который входит в состав Visual Studio, для создания нашей модели.</span><span class="sxs-lookup"><span data-stu-id="1f318-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="1f318-176">**Проект -&gt; добавить новый элемент...**</span><span class="sxs-lookup"><span data-stu-id="1f318-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="1f318-177">Выберите **данных** в меню слева и затем **модель EDM ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="1f318-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="1f318-178">Введите **ProductModel** имя и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="1f318-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="1f318-179">Это откроет **мастер моделей EDM**</span><span class="sxs-lookup"><span data-stu-id="1f318-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="1f318-180">Выберите **создать из базы данных** и нажмите кнопку **Далее**</span><span class="sxs-lookup"><span data-stu-id="1f318-180">Select **Generate from Database** and click **Next**</span></span>

    ![Выбор содержимого модели](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="1f318-182">Выберите соединение с базой данных, созданной в первом разделе, введите **ProductContext** как имя строки подключения и нажмите кнопку **Далее**</span><span class="sxs-lookup"><span data-stu-id="1f318-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Выберите соединение](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="1f318-184">Установите флажок рядом с «Таблицы», чтобы импортировать все таблицы и нажмите кнопку «Готово»</span><span class="sxs-lookup"><span data-stu-id="1f318-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Выбор объектов](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="1f318-186">После завершения процесса реконструирования новой модели добавлен в проект и открывается для просмотра в конструкторе Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1f318-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="1f318-187">Файл App.config также был добавлен в проект со сведениями о подключении для базы данных.</span><span class="sxs-lookup"><span data-stu-id="1f318-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="1f318-188">Дополнительные действия в Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="1f318-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="1f318-189">Если вы работаете в Visual Studio 2010 будет необходимо обновить конструктор EF будет использовать создание кода EF6.</span><span class="sxs-lookup"><span data-stu-id="1f318-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="1f318-190">Щелкните правой кнопкой мыши пустое место модели в конструкторе EF и выберите **добавить элемент формирования кода...**</span><span class="sxs-lookup"><span data-stu-id="1f318-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="1f318-191">Выберите **шаблоны в Интернете** из меню слева и выполните поиск **DbContext**</span><span class="sxs-lookup"><span data-stu-id="1f318-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="1f318-192">Выберите **EF 6.x генератор DbContext для C\#,** введите **ProductsModel** как имя и щелкните "Добавить"</span><span class="sxs-lookup"><span data-stu-id="1f318-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="1f318-193">Обновление создание кода для привязки данных</span><span class="sxs-lookup"><span data-stu-id="1f318-193">Updating code generation for data binding</span></span>

<span data-ttu-id="1f318-194">EF создает код из модели с помощью шаблонов T4.</span><span class="sxs-lookup"><span data-stu-id="1f318-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="1f318-195">Шаблоны, поставляемых с Visual Studio или скачать из коллекции Visual Studio предназначены для использования общего назначения.</span><span class="sxs-lookup"><span data-stu-id="1f318-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="1f318-196">Это означает, что сущности, созданные на основе этих шаблонов имеют простой ICollection&lt;T&gt; свойства.</span><span class="sxs-lookup"><span data-stu-id="1f318-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="1f318-197">Тем не менее, при этом данные привязки с использованием WPF желательно использовать **ObservableCollection** для свойств коллекции, так что WPF можно отслеживать, изменения, внесенные в коллекции.</span><span class="sxs-lookup"><span data-stu-id="1f318-197">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="1f318-198">Для этого нужно будет изменить шаблоны для использования ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="1f318-198">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="1f318-199">Откройте **обозревателе решений** и найти **ProductModel.edmx** файла</span><span class="sxs-lookup"><span data-stu-id="1f318-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="1f318-200">Найти **ProductModel.tt** файл, который будет вложен в файле ProductModel.edmx</span><span class="sxs-lookup"><span data-stu-id="1f318-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Шаблон модели продукта в WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="1f318-202">Дважды щелкните файл ProductModel.tt, чтобы открыть его в редакторе Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1f318-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="1f318-203">Найти и заменить два вхождения "**ICollection**«with»**ObservableCollection**«.</span><span class="sxs-lookup"><span data-stu-id="1f318-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="1f318-204">Они находятся приблизительно в строках 296 и 484.</span><span class="sxs-lookup"><span data-stu-id="1f318-204">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="1f318-205">Найти и заменить первое вхождение "**HashSet**«with»**ObservableCollection**«.</span><span class="sxs-lookup"><span data-stu-id="1f318-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="1f318-206">Это событие находится примерно в строке 50.</span><span class="sxs-lookup"><span data-stu-id="1f318-206">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="1f318-207">**Не** замените второе вхождение HashSet Найти далее в коде.</span><span class="sxs-lookup"><span data-stu-id="1f318-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="1f318-208">Поиск и замена только вхождения "**System.Collections.Generic**«with»**System.Collections.ObjectModel**«.</span><span class="sxs-lookup"><span data-stu-id="1f318-208">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="1f318-209">Это примерно в строке 424.</span><span class="sxs-lookup"><span data-stu-id="1f318-209">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="1f318-210">Сохраните файл ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="1f318-210">Save the ProductModel.tt file.</span></span> <span data-ttu-id="1f318-211">Это может вызвать код для сущностей, быть создан повторно.</span><span class="sxs-lookup"><span data-stu-id="1f318-211">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="1f318-212">Если код не создается автоматически, затем щелкнуть правой кнопкой мыши ProductModel.tt и выберите «Запустить пользовательский инструмент».</span><span class="sxs-lookup"><span data-stu-id="1f318-212">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="1f318-213">Если вы теперь откройте файл Category.cs (который вложен в ProductModel.tt), то вы увидите, что продукты коллекция имеет тип **ObservableCollection&lt;продукта&gt;**.</span><span class="sxs-lookup"><span data-stu-id="1f318-213">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="1f318-214">Скомпилируйте проект.</span><span class="sxs-lookup"><span data-stu-id="1f318-214">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="1f318-215">Отложенная загрузка</span><span class="sxs-lookup"><span data-stu-id="1f318-215">Lazy Loading</span></span>

<span data-ttu-id="1f318-216">**Продуктов** свойство **категории** класс и **категории** свойство **продукта** класс — это свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="1f318-216">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="1f318-217">В Entity Framework свойства навигации позволяют для перехода по связи между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="1f318-217">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="1f318-218">EF дает возможность загрузки связанных сущностей из базы данных автоматически при первом обращении к свойству навигации.</span><span class="sxs-lookup"><span data-stu-id="1f318-218">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="1f318-219">С помощью этого типа загрузки (называется отложенной загрузки) Имейте в виду, что при первом доступе к каждому свойству навигации отдельный запрос будет выполнен в базе данных, если содержимое еще не в контексте.</span><span class="sxs-lookup"><span data-stu-id="1f318-219">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="1f318-220">При использовании типов сущностей POCO, EF обеспечивает отложенную загрузку, создании экземпляров производного прокси-типов во время выполнения и затем переопределение виртуальных свойств в классах для добавления обработчика загрузки.</span><span class="sxs-lookup"><span data-stu-id="1f318-220">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="1f318-221">Чтобы получить отложенная загрузка связанных объектов, необходимо объявить методы получения свойств как навигации **открытый** и **виртуального** (**Overridable** в Visual Basic), и вы класс не должен быть **запечатанный** (**NotOverridable** в Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="1f318-221">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="1f318-222">Когда базы данных с помощью первого свойства навигации, автоматически становятся виртуальные, чтобы включить отложенную загрузку.</span><span class="sxs-lookup"><span data-stu-id="1f318-222">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="1f318-223">В разделе Code First, мы решили сделать виртуальные свойства навигации по той же причине</span><span class="sxs-lookup"><span data-stu-id="1f318-223">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="1f318-224">Привязка объектов к элементам управления</span><span class="sxs-lookup"><span data-stu-id="1f318-224">Bind Object to Controls</span></span>

<span data-ttu-id="1f318-225">Добавьте классы, которые определены в модели в качестве источников данных для этого приложения WPF.</span><span class="sxs-lookup"><span data-stu-id="1f318-225">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="1f318-226">Дважды щелкните **MainWindow.xaml** в обозревателе решений, чтобы открыть главную форму</span><span class="sxs-lookup"><span data-stu-id="1f318-226">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="1f318-227">В главном меню выберите **проект -&gt; добавить новый источник данных...**</span><span class="sxs-lookup"><span data-stu-id="1f318-227">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="1f318-228">(в Visual Studio 2010, необходимо выбрать **тарифный план —&gt; добавить новый источник данных...** )</span><span class="sxs-lookup"><span data-stu-id="1f318-228">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="1f318-229">В выберите Typewindow источника данных, выберите **объект** и нажмите кнопку **Далее**</span><span class="sxs-lookup"><span data-stu-id="1f318-229">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="1f318-230">В диалоговом окне объекты данных выберите unfold **WPFwithEFSample** двух раз и выберите **категории**</span><span class="sxs-lookup"><span data-stu-id="1f318-230">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="1f318-231">*Нет необходимости, чтобы выбрать **продукта** источника данных, так как мы вернемся к нему через **продукта**свойство **категории** источника данных*</span><span class="sxs-lookup"><span data-stu-id="1f318-231">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![Выбор объектов данных](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="1f318-233">Нажмите кнопку **Готово.**</span><span class="sxs-lookup"><span data-stu-id="1f318-233">Click **Finish.**</span></span>
-   <span data-ttu-id="1f318-234">Открывается окно "Источники данных" рядом с окном MainWindow.xaml *Если окно "Источники данных", не отображается, выберите **представление —&gt; Other Windows -&gt; источников данных***</span><span class="sxs-lookup"><span data-stu-id="1f318-234">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="1f318-235">Скрыть значок ПИН-код, поэтому окна источников данных автоматически.</span><span class="sxs-lookup"><span data-stu-id="1f318-235">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="1f318-236">Может потребоваться нажмите кнопку "Обновить", если окно уже было открыто.</span><span class="sxs-lookup"><span data-stu-id="1f318-236">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Data Sources](~/ef6/media/datasources.png)

-   <span data-ttu-id="1f318-238">Выберите \*\* категории \*\* источника данных и перетащите его на форме.</span><span class="sxs-lookup"><span data-stu-id="1f318-238">Select the \*\*Category \*\*data source and drag it on the form.</span></span>

<span data-ttu-id="1f318-239">Когда мы перетащили этот источник произошло следующее:</span><span class="sxs-lookup"><span data-stu-id="1f318-239">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="1f318-240">**CategoryViewSource** ресурсов и \*\* categoryDataGrid \*\* управления были добавлены в XAML.</span><span class="sxs-lookup"><span data-stu-id="1f318-240">The **categoryViewSource** resource and the\*\* categoryDataGrid\*\* control were added to XAML.</span></span> <span data-ttu-id="1f318-241">Дополнительные сведения о DataViewSources, см. в разделе http://bea.stollnitz.com/blog/?p=387.</span><span class="sxs-lookup"><span data-stu-id="1f318-241">For more information about DataViewSources, see http://bea.stollnitz.com/blog/?p=387.</span></span>
-   <span data-ttu-id="1f318-242">Свойство DataContext для родительского элемента сетки установлено в «{StaticResource **categoryViewSource** }».</span><span class="sxs-lookup"><span data-stu-id="1f318-242">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span>  <span data-ttu-id="1f318-243">**CategoryViewSource** ресурсов выступает в качестве источника привязки для внешнего\\родительского элемента сетки.</span><span class="sxs-lookup"><span data-stu-id="1f318-243">The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="1f318-244">Внутренние элементы сетки, наследуют значение DataContext от родительского элемента сетки (свойство ItemsSource categoryDataGrid имеет значение «{Binding}»).</span><span class="sxs-lookup"><span data-stu-id="1f318-244">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}").</span></span> 

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

## <a name="adding-a-details-grid"></a><span data-ttu-id="1f318-245">Добавление сетки сведений</span><span class="sxs-lookup"><span data-stu-id="1f318-245">Adding a Details Grid</span></span>

<span data-ttu-id="1f318-246">Теперь, когда у нас есть сетки для отображения категорий, давайте добавьте сведения о сетке для отображения соответствующих продуктов.</span><span class="sxs-lookup"><span data-stu-id="1f318-246">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="1f318-247">Выберите \*\* продуктов \*\* свойства в разделе \*\* категории \*\* источника данных и перетащите его на форме.</span><span class="sxs-lookup"><span data-stu-id="1f318-247">Select the \*\*Products \*\*property from under the \*\*Category \*\*data source and drag it on the form.</span></span>
    -   <span data-ttu-id="1f318-248">**CategoryProductsViewSource** ресурсов и **productDataGrid** сетки добавляются в XAML</span><span class="sxs-lookup"><span data-stu-id="1f318-248">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="1f318-249">Путь привязки для этого ресурса будет присвоено продуктов</span><span class="sxs-lookup"><span data-stu-id="1f318-249">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="1f318-250">Инфраструктура привязки данных WPF гарантирует, что только продукты, относящиеся к выбранной категории будут отображаться в **productDataGrid**</span><span class="sxs-lookup"><span data-stu-id="1f318-250">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="1f318-251">Из панели элементов перетащите **кнопку** в форму.</span><span class="sxs-lookup"><span data-stu-id="1f318-251">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="1f318-252">Задайте **имя** свойства **buttonSave** и **содержимого** свойства **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="1f318-252">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="1f318-253">Форма должна выглядеть примерно так:</span><span class="sxs-lookup"><span data-stu-id="1f318-253">The form should look similar to this:</span></span>

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="1f318-255">Добавьте код, который обрабатывает взаимодействие с данными</span><span class="sxs-lookup"><span data-stu-id="1f318-255">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="1f318-256">Пришло время добавить некоторые обработчики событий в главное окно.</span><span class="sxs-lookup"><span data-stu-id="1f318-256">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="1f318-257">В окне XAML, щелкните  **&lt;окно** элемент, Выбор главного окна</span><span class="sxs-lookup"><span data-stu-id="1f318-257">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="1f318-258">В **свойства** выберите окно **события** в правом верхнем углу, затем дважды щелкните текстовое поле справа от **Loaded** метки</span><span class="sxs-lookup"><span data-stu-id="1f318-258">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![Главное окно свойств](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="1f318-260">Также добавьте **щелкните** событие для **Сохранить** кнопки, дважды щелкнув "Сохранить" в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="1f318-260">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="1f318-261">Откроется кода программной части формы, теперь мы отредактируем код, чтобы использовать ProductContext производить доступ к данным.</span><span class="sxs-lookup"><span data-stu-id="1f318-261">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="1f318-262">Обновите код для MainWindow, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="1f318-262">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="1f318-263">Код объявляет экземпляр длительных **ProductContext**.</span><span class="sxs-lookup"><span data-stu-id="1f318-263">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="1f318-264">**ProductContext** объект используется для запроса и сохранения данных в базу данных.</span><span class="sxs-lookup"><span data-stu-id="1f318-264">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="1f318-265">**Dispose**() на **ProductContext** экземпляра вызывается из переопределенного **OnClosing** метод.</span><span class="sxs-lookup"><span data-stu-id="1f318-265">The **Dispose**() on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span> <span data-ttu-id="1f318-266">В комментариях к коду содержат сведения о что делает код.</span><span class="sxs-lookup"><span data-stu-id="1f318-266">The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-wpf-application"></a><span data-ttu-id="1f318-267">Тестирование приложения WPF</span><span class="sxs-lookup"><span data-stu-id="1f318-267">Test the WPF Application</span></span>

-   <span data-ttu-id="1f318-268">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="1f318-268">Compile and run the application.</span></span> <span data-ttu-id="1f318-269">Если вы использовали Code First, то вы увидите, что **WPFwithEFSample.ProductContext** база данных создается автоматически.</span><span class="sxs-lookup"><span data-stu-id="1f318-269">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="1f318-270">Введите имя категории в верхней сетке и продукта имена в нижней таблице *ничего не вводится в столбцов ID, так как первичный ключ создается в базе данных*</span><span class="sxs-lookup"><span data-stu-id="1f318-270">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Главное окно с помощью новых категорий и продуктов](~/ef6/media/screen1.png)

-   <span data-ttu-id="1f318-272">Нажмите клавишу **Сохранить** кнопку, чтобы сохранить данные в базе данных</span><span class="sxs-lookup"><span data-stu-id="1f318-272">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="1f318-273">После вызова в DbContext **SaveChanges**(), идентификаторы заполняются значениями сформированный базой данных.</span><span class="sxs-lookup"><span data-stu-id="1f318-273">After the call to DbContext’s **SaveChanges**(), the IDs are populated with the database generated values.</span></span> <span data-ttu-id="1f318-274">Так как мы вызывали метод **обновить**() после **SaveChanges**() **DataGrid** элементы управления обновляются с использованием новых значений.</span><span class="sxs-lookup"><span data-stu-id="1f318-274">Because we called **Refresh**() after **SaveChanges**() the **DataGrid** controls are updated with the new values as well.</span></span>

![Главное окно с идентификаторами заполнен](~/ef6/media/screen2.png)
