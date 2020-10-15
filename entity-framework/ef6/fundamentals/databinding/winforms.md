---
title: Привязка данных с помощью WinForms-EF6
description: Привязка данных с помощью WinForms в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/fundamentals/databinding/winforms
ms.openlocfilehash: ef2f19831cd5ce8b1faefeb51f23faef4e167529
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92065684"
---
# <a name="databinding-with-winforms"></a><span data-ttu-id="aaf89-103">Привязка данных с помощью WinForms</span><span class="sxs-lookup"><span data-stu-id="aaf89-103">Databinding with WinForms</span></span>
<span data-ttu-id="aaf89-104">В этом пошаговом руководстве показано, как привязать типы POCO к элементам управления Window Forms (WinForms) в форме "основной/подробности".</span><span class="sxs-lookup"><span data-stu-id="aaf89-104">This step-by-step walkthrough shows how to bind POCO types to Window Forms (WinForms) controls in a “master-detail" form.</span></span> <span data-ttu-id="aaf89-105">Приложение использует Entity Framework для заполнения объектов данными из базы данных, трассировки изменений и сохранения данных в базе данных.</span><span class="sxs-lookup"><span data-stu-id="aaf89-105">The application uses Entity Framework to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="aaf89-106">Модель определяет два типа, участвующих в связи "один ко многим": Категория (основная \\ Главная) и продукт (зависимые \\ Подробности).</span><span class="sxs-lookup"><span data-stu-id="aaf89-106">The model defines two types that participate in one-to-many relationship: Category (principal\\master) and Product (dependent\\detail).</span></span> <span data-ttu-id="aaf89-107">Затем средства Visual Studio используются для привязки типов, определенных в модели, к элементам управления WinForms.</span><span class="sxs-lookup"><span data-stu-id="aaf89-107">Then, the Visual Studio tools are used to bind the types defined in the model to the WinForms controls.</span></span> <span data-ttu-id="aaf89-108">Платформа привязки данных WinForms позволяет переходить между связанными объектами: выбор строк в главном представлении приводит к обновлению подробного представления соответствующими дочерними данными.</span><span class="sxs-lookup"><span data-stu-id="aaf89-108">The WinForms data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="aaf89-109">Снимки экрана и списки кода в этом пошаговом руководстве взяты из Visual Studio 2013 но вы можете выполнить это пошаговое руководство с помощью Visual Studio 2012 или Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="aaf89-109">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="aaf89-110">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="aaf89-110">Pre-Requisites</span></span>

<span data-ttu-id="aaf89-111">Для работы с этим пошаговым руководством необходимо установить Visual Studio 2013, Visual Studio 2012 или Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="aaf89-111">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="aaf89-112">Если вы используете Visual Studio 2010, необходимо также установить NuGet.</span><span class="sxs-lookup"><span data-stu-id="aaf89-112">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="aaf89-113">Дополнительные сведения см. в разделе [Установка NuGet](https://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="aaf89-113">For more information, see [Installing NuGet](https://docs.nuget.org/docs/start-here/installing-nuget).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="aaf89-114">Создание приложения</span><span class="sxs-lookup"><span data-stu-id="aaf89-114">Create the Application</span></span>

-   <span data-ttu-id="aaf89-115">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aaf89-115">Open Visual Studio</span></span>
-   <span data-ttu-id="aaf89-116">**Файл- &gt; создать &gt; проект...**</span><span class="sxs-lookup"><span data-stu-id="aaf89-116">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="aaf89-117">Выберите **Windows** в левой области и **Windows формсаппликатион** в правой области.</span><span class="sxs-lookup"><span data-stu-id="aaf89-117">Select **Windows** in the left pane and **Windows FormsApplication** in the right pane</span></span>
-   <span data-ttu-id="aaf89-118">Введите **винформсвисефсампле** в качестве имени</span><span class="sxs-lookup"><span data-stu-id="aaf89-118">Enter **WinFormswithEFSample** as the name</span></span>
-   <span data-ttu-id="aaf89-119">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="aaf89-119">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="aaf89-120">Установка пакета NuGet Entity Framework</span><span class="sxs-lookup"><span data-stu-id="aaf89-120">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="aaf89-121">В обозреватель решений щелкните правой кнопкой мыши проект **винформсвисефсампле**</span><span class="sxs-lookup"><span data-stu-id="aaf89-121">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="aaf89-122">Выберите **Управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="aaf89-122">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="aaf89-123">В диалоговом окне Управление пакетами NuGet выберите вкладку в **сети** и выберите пакет **EntityFramework** .</span><span class="sxs-lookup"><span data-stu-id="aaf89-123">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="aaf89-124">Щелкните **Установить**.</span><span class="sxs-lookup"><span data-stu-id="aaf89-124">Click **Install**</span></span>  
    > [!NOTE]
    > <span data-ttu-id="aaf89-125">В дополнение к сборке EntityFramework также добавляется ссылка на System. ComponentModel. Аннотация.</span><span class="sxs-lookup"><span data-stu-id="aaf89-125">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="aaf89-126">Если проект содержит ссылку на System. Data. Entity, то он будет удален при установке пакета EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="aaf89-126">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="aaf89-127">Сборка System. Data. Entity больше не используется для приложений Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="aaf89-127">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="implementing-ilistsource-for-collections"></a><span data-ttu-id="aaf89-128">Реализация IListSource для коллекций</span><span class="sxs-lookup"><span data-stu-id="aaf89-128">Implementing IListSource for Collections</span></span>

<span data-ttu-id="aaf89-129">Свойства коллекции должны реализовывать интерфейс IListSource, чтобы обеспечить двустороннюю привязку данных с сортировкой при использовании Windows Forms.</span><span class="sxs-lookup"><span data-stu-id="aaf89-129">Collection properties must implement the IListSource interface to enable two-way data binding with sorting when using Windows Forms.</span></span> <span data-ttu-id="aaf89-130">Для этого мы собираемся расширить коллекцию ObservableCollection, чтобы добавить IListSource функциональность.</span><span class="sxs-lookup"><span data-stu-id="aaf89-130">To do this we are going to extend ObservableCollection to add IListSource functionality.</span></span>

-   <span data-ttu-id="aaf89-131">Добавьте класс **обсерваблелистсаурце** в проект:</span><span class="sxs-lookup"><span data-stu-id="aaf89-131">Add an **ObservableListSource** class to the project:</span></span>
    -   <span data-ttu-id="aaf89-132">Щелкните правой кнопкой мыши имя проекта</span><span class="sxs-lookup"><span data-stu-id="aaf89-132">Right-click on the project name</span></span>
    -   <span data-ttu-id="aaf89-133">Выберите **Добавить- &gt; новый элемент** .</span><span class="sxs-lookup"><span data-stu-id="aaf89-133">Select **Add -&gt; New Item**</span></span>
    -   <span data-ttu-id="aaf89-134">Выберите **класс** и введите **обсерваблелистсаурце** в качестве имени класса.</span><span class="sxs-lookup"><span data-stu-id="aaf89-134">Select **Class** and enter **ObservableListSource** for the class name</span></span>
-   <span data-ttu-id="aaf89-135">Замените код, созданный по умолчанию, следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="aaf89-135">Replace the code generated by default with the following code:</span></span>

<span data-ttu-id="aaf89-136">*Этот класс обеспечивает двустороннюю привязку данных, а также сортировку. Класс является производным от ObservableCollection &lt; T &gt; и добавляет явную реализацию IListSource. Метод метода WebMethod () класса IListSource реализуется для возврата реализации IBindingList, которая остается синхронизированной с ObservableCollection. Реализация IBindingList, созданная Тобиндинглист, поддерживает сортировку. Метод расширения Тобиндинглист определен в сборке EntityFramework.*</span><span class="sxs-lookup"><span data-stu-id="aaf89-136">*This class enables two-way data binding as well as sorting. The class derives from ObservableCollection&lt;T&gt; and adds an explicit implementation of IListSource. The GetList() method of IListSource is implemented to return an IBindingList implementation that stays in sync with the ObservableCollection. The IBindingList implementation generated by ToBindingList supports sorting. The ToBindingList extension method is defined in the EntityFramework assembly.*</span></span>

``` csharp
    using System.Collections;
    using System.Collections.Generic;
    using System.Collections.ObjectModel;
    using System.ComponentModel;
    using System.Diagnostics.CodeAnalysis;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public class ObservableListSource<T> : ObservableCollection<T>, IListSource
            where T : class
        {
            private IBindingList _bindingList;

            bool IListSource.ContainsListCollection { get { return false; } }

            IList IListSource.GetList()
            {
                return _bindingList ?? (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a><span data-ttu-id="aaf89-137">Определение модели</span><span class="sxs-lookup"><span data-stu-id="aaf89-137">Define a Model</span></span>

<span data-ttu-id="aaf89-138">В этом пошаговом руководстве можно выбрать реализацию модели с помощью Code First или конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="aaf89-138">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="aaf89-139">Выполните один из двух следующих разделов.</span><span class="sxs-lookup"><span data-stu-id="aaf89-139">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="aaf89-140">Вариант 1. Определение модели с помощью Code First</span><span class="sxs-lookup"><span data-stu-id="aaf89-140">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="aaf89-141">В этом разделе показано, как создать модель и связанную с ней базу данных с помощью Code First.</span><span class="sxs-lookup"><span data-stu-id="aaf89-141">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="aaf89-142">Перейдите к следующему разделу (**вариант 2. Определение модели с помощью Database First)** , если вы предпочитаете использовать Database First для реконструирования модели из базы данных с помощью конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="aaf89-142">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="aaf89-143">При использовании Code First разработки обычно начинается написание .NET Framework классов, определяющих концептуальную (доменную) модель.</span><span class="sxs-lookup"><span data-stu-id="aaf89-143">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="aaf89-144">Добавление нового класса **Product** в проект</span><span class="sxs-lookup"><span data-stu-id="aaf89-144">Add a new **Product** class to project</span></span>
-   <span data-ttu-id="aaf89-145">Замените код, созданный по умолчанию, следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="aaf89-145">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }
```

-   <span data-ttu-id="aaf89-146">Добавьте в проект класс **Category** .</span><span class="sxs-lookup"><span data-stu-id="aaf89-146">Add a **Category** class to the project.</span></span>
-   <span data-ttu-id="aaf89-147">Замените код, созданный по умолчанию, следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="aaf89-147">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Category
        {
            private readonly ObservableListSource<Product> _products =
                    new ObservableListSource<Product>();

            public int CategoryId { get; set; }
            public string Name { get; set; }
            public virtual ObservableListSource<Product> Products { get { return _products; } }
        }
    }
```

<span data-ttu-id="aaf89-148">Помимо определения сущностей, необходимо определить класс, производный от **DbContext** и предоставляющий свойства \*\*DbSetости &lt; &gt; \*\* .</span><span class="sxs-lookup"><span data-stu-id="aaf89-148">In addition to defining entities, you need to define a class that derives from **DbContext** and exposes **DbSet&lt;TEntity&gt;** properties.</span></span> <span data-ttu-id="aaf89-149">Свойства **DbSet** позволяют контексту понять, какие типы необходимо включить в модель.</span><span class="sxs-lookup"><span data-stu-id="aaf89-149">The **DbSet** properties let the context know which types you want to include in the model.</span></span> <span data-ttu-id="aaf89-150">Типы **DbContext** и **DbSet** определены в сборке EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="aaf89-150">The **DbContext** and **DbSet** types are defined in the EntityFramework assembly.</span></span>

<span data-ttu-id="aaf89-151">Экземпляр производного типа DbContext управляет объектами сущностей во время выполнения, в частности заполняет объекты данными из базы данных, отслеживает изменения и сохраняет данные в базе данных.</span><span class="sxs-lookup"><span data-stu-id="aaf89-151">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="aaf89-152">Добавьте в проект новый класс **продуктконтекст** .</span><span class="sxs-lookup"><span data-stu-id="aaf89-152">Add a new **ProductContext** class to the project.</span></span>
-   <span data-ttu-id="aaf89-153">Замените код, созданный по умолчанию, следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="aaf89-153">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Text;

    namespace WinFormswithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

<span data-ttu-id="aaf89-154">Скомпилируйте проект.</span><span class="sxs-lookup"><span data-stu-id="aaf89-154">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="aaf89-155">Вариант 2. Определение модели с помощью Database First</span><span class="sxs-lookup"><span data-stu-id="aaf89-155">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="aaf89-156">В этом разделе показано, как использовать Database First для реконструирования модели из базы данных с помощью конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="aaf89-156">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="aaf89-157">Если вы завершили предыдущий раздел (**вариант 1. Определение модели с помощью Code First)**, пропустите этот раздел и перейдите непосредственно к разделу " **ленивая загрузка** ".</span><span class="sxs-lookup"><span data-stu-id="aaf89-157">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="aaf89-158">Создание существующей базы данных</span><span class="sxs-lookup"><span data-stu-id="aaf89-158">Create an Existing Database</span></span>

<span data-ttu-id="aaf89-159">Обычно при использовании существующей базы данных она будет создана, но в этом пошаговом руководстве необходимо создать базу данных для доступа к ней.</span><span class="sxs-lookup"><span data-stu-id="aaf89-159">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="aaf89-160">Сервер базы данных, устанавливаемый с Visual Studio, отличается в зависимости от установленной версии Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aaf89-160">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="aaf89-161">При использовании Visual Studio 2010 будет создаваться база данных SQL Express.</span><span class="sxs-lookup"><span data-stu-id="aaf89-161">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="aaf89-162">Если вы используете Visual Studio 2012, то будете создавать базу данных [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) .</span><span class="sxs-lookup"><span data-stu-id="aaf89-162">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="aaf89-163">Давайте создадим базу данных.</span><span class="sxs-lookup"><span data-stu-id="aaf89-163">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="aaf89-164">**Просмотр — &gt; Обозреватель сервера**</span><span class="sxs-lookup"><span data-stu-id="aaf89-164">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="aaf89-165">Щелкните правой кнопкой мыши **подключения к данным — &gt; Добавить подключение...**</span><span class="sxs-lookup"><span data-stu-id="aaf89-165">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="aaf89-166">Если вы не подключились к базе данных из обозреватель сервера, прежде чем нужно будет выбрать Microsoft SQL Server в качестве источника данных</span><span class="sxs-lookup"><span data-stu-id="aaf89-166">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Изменение источника данных](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="aaf89-168">Подключитесь к LocalDB или SQL Express, в зависимости от того, какой из них установлен, и введите **Products** в качестве имени базы данных.</span><span class="sxs-lookup"><span data-stu-id="aaf89-168">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Добавить соединение LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Добавить подключение Express](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="aaf89-171">Нажмите кнопку **ОК** , и появится запрос на создание новой базы данных, выберите **Да** .</span><span class="sxs-lookup"><span data-stu-id="aaf89-171">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Создание базы данных](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="aaf89-173">Новая база данных появится в обозреватель сервера, щелкните ее правой кнопкой мыши и выберите пункт **создать запрос** .</span><span class="sxs-lookup"><span data-stu-id="aaf89-173">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="aaf89-174">Скопируйте следующий SQL в новый запрос, щелкните запрос правой кнопкой мыши и выберите команду **выполнить** .</span><span class="sxs-lookup"><span data-stu-id="aaf89-174">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="aaf89-175">Модель реконструирования</span><span class="sxs-lookup"><span data-stu-id="aaf89-175">Reverse Engineer Model</span></span>

<span data-ttu-id="aaf89-176">Мы будем использовать Entity Framework Designer, который входит в состав Visual Studio, для создания нашей модели.</span><span class="sxs-lookup"><span data-stu-id="aaf89-176">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="aaf89-177">**Проект- &gt; Добавить новый элемент...**</span><span class="sxs-lookup"><span data-stu-id="aaf89-177">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="aaf89-178">Выберите **данные** в меню слева, а затем **ADO.NET EDM**</span><span class="sxs-lookup"><span data-stu-id="aaf89-178">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="aaf89-179">Введите **ProductModel** в качестве имени и нажмите кнопку **ОК** .</span><span class="sxs-lookup"><span data-stu-id="aaf89-179">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="aaf89-180">Запустится **мастер EDM**</span><span class="sxs-lookup"><span data-stu-id="aaf89-180">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="aaf89-181">Выберите **создать из базы данных** и нажмите кнопку **Далее** .</span><span class="sxs-lookup"><span data-stu-id="aaf89-181">Select **Generate from Database** and click **Next**</span></span>

    ![Выбор содержимого модели](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="aaf89-183">Выберите подключение к базе данных, созданной в первом разделе, введите **продуктконтекст** в качестве имени строки подключения и нажмите кнопку **Далее** .</span><span class="sxs-lookup"><span data-stu-id="aaf89-183">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Выбор подключения](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="aaf89-185">Установите флажок рядом с пунктом "таблицы", чтобы импортировать все таблицы, и нажмите кнопку "Готово".</span><span class="sxs-lookup"><span data-stu-id="aaf89-185">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Выбор объектов](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="aaf89-187">После завершения процесса реконструирования новая модель добавляется в проект и открывается для просмотра в Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="aaf89-187">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="aaf89-188">В проект также добавлен файл App.config с подробными сведениями о соединении для базы данных.</span><span class="sxs-lookup"><span data-stu-id="aaf89-188">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="aaf89-189">Дополнительные действия в Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="aaf89-189">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="aaf89-190">При работе в Visual Studio 2010 необходимо обновить конструктор EF для использования создания кода EF6.</span><span class="sxs-lookup"><span data-stu-id="aaf89-190">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="aaf89-191">Щелкните правой кнопкой мыши пустое место в модели в конструкторе EF и выберите пункт **Добавить элемент создания кода...**</span><span class="sxs-lookup"><span data-stu-id="aaf89-191">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="aaf89-192">В меню слева выберите **шаблоны в Интернете** и выполните поиск по запросу **DbContext**</span><span class="sxs-lookup"><span data-stu-id="aaf89-192">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="aaf89-193">Выберите **генератор EF 6. x DbContext для C \# ,** введите **продуктсмодел** в качестве имени и нажмите кнопку Добавить.</span><span class="sxs-lookup"><span data-stu-id="aaf89-193">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="aaf89-194">Обновление создания кода для привязки данных</span><span class="sxs-lookup"><span data-stu-id="aaf89-194">Updating code generation for data binding</span></span>

<span data-ttu-id="aaf89-195">EF создает код из модели с помощью шаблонов T4.</span><span class="sxs-lookup"><span data-stu-id="aaf89-195">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="aaf89-196">Шаблоны, поставляемые с Visual Studio или скачанные из коллекции Visual Studio, предназначены для использования в общих целях.</span><span class="sxs-lookup"><span data-stu-id="aaf89-196">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="aaf89-197">Это означает, что сущности, созданные из этих шаблонов, имеют &lt; простые &gt; свойства ICollection T.</span><span class="sxs-lookup"><span data-stu-id="aaf89-197">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="aaf89-198">Однако при выполнении привязки данных желательно иметь свойства коллекции, реализующие IListSource.</span><span class="sxs-lookup"><span data-stu-id="aaf89-198">However, when doing data binding it is desirable to have collection properties that implement IListSource.</span></span> <span data-ttu-id="aaf89-199">Именно поэтому мы создали класс Обсерваблелистсаурце выше, и теперь будем изменять шаблоны, чтобы использовать этот класс.</span><span class="sxs-lookup"><span data-stu-id="aaf89-199">This is why we created the ObservableListSource class above and we are now going to modify the templates to make use of this class.</span></span>

-   <span data-ttu-id="aaf89-200">Откройте **Обозреватель решений** и найдите файл **ProductModel. EDMX**</span><span class="sxs-lookup"><span data-stu-id="aaf89-200">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="aaf89-201">Найдите файл **ProductModel.TT** , который будет вложен в файл ProductModel. EDMX</span><span class="sxs-lookup"><span data-stu-id="aaf89-201">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Шаблон модели продукта](~/ef6/media/productmodeltemplate.png)

-   <span data-ttu-id="aaf89-203">Дважды щелкните файл ProductModel.tt, чтобы открыть его в редакторе Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aaf89-203">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="aaf89-204">Найдите и замените два вхождения "**ICollection**" на "**обсерваблелистсаурце**".</span><span class="sxs-lookup"><span data-stu-id="aaf89-204">Find and replace the two occurrences of “**ICollection**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="aaf89-205">Они расположены примерно в строках 296 и 484.</span><span class="sxs-lookup"><span data-stu-id="aaf89-205">These are located at approximately lines 296 and 484.</span></span>
-   <span data-ttu-id="aaf89-206">Найдите и замените первое вхождение "**hashing**" на "**обсерваблелистсаурце**".</span><span class="sxs-lookup"><span data-stu-id="aaf89-206">Find and replace the first occurrence of “**HashSet**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="aaf89-207">Это вхождение расположено примерно в строке 50.</span><span class="sxs-lookup"><span data-stu-id="aaf89-207">This occurrence is located at approximately line 50.</span></span> <span data-ttu-id="aaf89-208">**Не** заменяйте второе вхождение набора хэширования, найденного далее в коде.</span><span class="sxs-lookup"><span data-stu-id="aaf89-208">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="aaf89-209">Сохраните файл ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="aaf89-209">Save the ProductModel.tt file.</span></span> <span data-ttu-id="aaf89-210">Это должно привести к повторному формированию кода для сущностей.</span><span class="sxs-lookup"><span data-stu-id="aaf89-210">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="aaf89-211">Если код не воссоздается автоматически, щелкните правой кнопкой мыши ProductModel.tt и выберите команду "запустить пользовательский инструмент".</span><span class="sxs-lookup"><span data-stu-id="aaf89-211">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="aaf89-212">Если теперь открыть файл Category.cs (который вложен в ProductModel.tt), то вы должны увидеть, что коллекция Products имеет тип \*\*обсерваблелистсаурце &lt; Product &gt; \*\*.</span><span class="sxs-lookup"><span data-stu-id="aaf89-212">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableListSource&lt;Product&gt;**.</span></span>

<span data-ttu-id="aaf89-213">Скомпилируйте проект.</span><span class="sxs-lookup"><span data-stu-id="aaf89-213">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="aaf89-214">Отложенная загрузка</span><span class="sxs-lookup"><span data-stu-id="aaf89-214">Lazy Loading</span></span>

<span data-ttu-id="aaf89-215">Свойство **Products** класса **Category** и свойство **Category** класса **Product** — это свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="aaf89-215">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="aaf89-216">В Entity Framework свойства навигации обеспечивают возможность навигации по связям между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="aaf89-216">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="aaf89-217">EF дает возможность автоматически загружать связанные сущности из базы данных при первом доступе к свойству навигации.</span><span class="sxs-lookup"><span data-stu-id="aaf89-217">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="aaf89-218">В случае использования такого типа загрузки (называемой отложенной) следует помнить о следующем: если содержимое еще отсутствует в контексте, при первом обращении к каждому свойству навигации будет выполняться отдельный запрос базы данных.</span><span class="sxs-lookup"><span data-stu-id="aaf89-218">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="aaf89-219">При использовании типов сущностей POCO EF достигает отложенной загрузки, создавая экземпляры производных прокси-типов во время выполнения, а затем переопределяя виртуальные свойства в классах для добавления обработчика загрузки.</span><span class="sxs-lookup"><span data-stu-id="aaf89-219">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="aaf89-220">Чтобы получить отложенную загрузку связанных объектов, необходимо объявить методы считывания свойств навигации как **открытые** и **Виртуальные** (**переопределяемые** в Visual Basic), и класс не должен быть **запечатанным** (**NotOverridable** в Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="aaf89-220">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="aaf89-221">При использовании Database First свойства навигации автоматически становятся виртуальными для включения отложенной загрузки.</span><span class="sxs-lookup"><span data-stu-id="aaf89-221">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="aaf89-222">В разделе Code First мы решили сделать свойства навигации виртуальными по той же причине.</span><span class="sxs-lookup"><span data-stu-id="aaf89-222">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="aaf89-223">Привязка объекта к элементам управления</span><span class="sxs-lookup"><span data-stu-id="aaf89-223">Bind Object to Controls</span></span>

<span data-ttu-id="aaf89-224">Добавьте классы, определенные в модели, в качестве источников данных для этого приложения WinForms.</span><span class="sxs-lookup"><span data-stu-id="aaf89-224">Add the classes that are defined in the model as data sources for this WinForms application.</span></span>

-   <span data-ttu-id="aaf89-225">В главном меню выберите **проект — &gt; Добавить новый источник данных...**</span><span class="sxs-lookup"><span data-stu-id="aaf89-225">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="aaf89-226">(в Visual Studio 2010 необходимо выбрать **данные — &gt; Добавить новый источник данных...**)</span><span class="sxs-lookup"><span data-stu-id="aaf89-226">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="aaf89-227">В окне Выбор типа источника данных выберите **объект** и нажмите кнопку **Далее** .</span><span class="sxs-lookup"><span data-stu-id="aaf89-227">In the Choose a Data Source Type window, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="aaf89-228">В диалоговом окне Выбор объектов данных unfold **винформсвисефсампле** два раза и выберите пункт **Категория** нет необходимости выбирать источник данных продукта, так как мы будем получать его через свойство продукта в источнике данных категории.</span><span class="sxs-lookup"><span data-stu-id="aaf89-228">In the Select the Data Objects dialog, unfold the **WinFormswithEFSample** two times and select **Category** There is no need to select the Product data source, because we will get to it through the Product’s property on the Category data source.</span></span>

    ![источника данных](~/ef6/media/datasource.png)

-   <span data-ttu-id="aaf89-230">Нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="aaf89-230">Click **Finish.**</span></span>
    <span data-ttu-id="aaf89-231">Если окно Источники данных не отображается, выберите **вид — &gt; другие &gt; Источники данных Windows** .</span><span class="sxs-lookup"><span data-stu-id="aaf89-231">If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources**</span></span>
-   <span data-ttu-id="aaf89-232">Нажмите значок закрепить, чтобы окно Источники данных не скрывалось автоматически.</span><span class="sxs-lookup"><span data-stu-id="aaf89-232">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="aaf89-233">Если окно уже видимо, может потребоваться нажать кнопку Обновить.</span><span class="sxs-lookup"><span data-stu-id="aaf89-233">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Источник данных 2](~/ef6/media/datasource2.png)

-   <span data-ttu-id="aaf89-235">В обозреватель решений дважды щелкните файл **Form1.CS** , чтобы открыть главную форму в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="aaf89-235">In Solution Explorer, double-click the **Form1.cs** file to open the main form in designer.</span></span>
-   <span data-ttu-id="aaf89-236">Выберите источник данных **Категория** и перетащите его в форму.</span><span class="sxs-lookup"><span data-stu-id="aaf89-236">Select the **Category** data source and drag it on the form.</span></span> <span data-ttu-id="aaf89-237">По умолчанию в конструктор добавляются новые элементы управления DataGridView (**категоридатагридвиев**) и панели инструментов навигации.</span><span class="sxs-lookup"><span data-stu-id="aaf89-237">By default, a new DataGridView (**categoryDataGridView**) and Navigation toolbar controls are added to the designer.</span></span> <span data-ttu-id="aaf89-238">Эти элементы управления привязаны к компонентам BindingSource (**категорибиндингсаурце**) и Binding Navigator (**категорибиндингнавигатор**), которые также создаются.</span><span class="sxs-lookup"><span data-stu-id="aaf89-238">These controls are bound to the BindingSource (**categoryBindingSource**) and Binding Navigator (**categoryBindingNavigator**) components that are created as well.</span></span>
-   <span data-ttu-id="aaf89-239">Измените столбцы в **категоридатагридвиев**.</span><span class="sxs-lookup"><span data-stu-id="aaf89-239">Edit the columns on the **categoryDataGridView**.</span></span> <span data-ttu-id="aaf89-240">Мы хотим задать столбцу **CategoryID** значение только для чтения.</span><span class="sxs-lookup"><span data-stu-id="aaf89-240">We want to set the **CategoryId** column to read-only.</span></span> <span data-ttu-id="aaf89-241">Значение свойства **CategoryID** создается базой данных после сохранения данных.</span><span class="sxs-lookup"><span data-stu-id="aaf89-241">The value for the **CategoryId** property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="aaf89-242">Щелкните правой кнопкой мыши элемент управления DataGridView и выберите пункт Изменить столбцы...</span><span class="sxs-lookup"><span data-stu-id="aaf89-242">Right-click the DataGridView control and select Edit Columns…</span></span>
    -   <span data-ttu-id="aaf89-243">Выберите столбец CategoryId и установите для параметра ReadOnly значение true.</span><span class="sxs-lookup"><span data-stu-id="aaf89-243">Select the CategoryId column and set ReadOnly to True</span></span>
    -   <span data-ttu-id="aaf89-244">Нажмите кнопку ОК.</span><span class="sxs-lookup"><span data-stu-id="aaf89-244">Press OK</span></span>
-   <span data-ttu-id="aaf89-245">Выберите продукты из источника данных Категория и перетащите его в форму.</span><span class="sxs-lookup"><span data-stu-id="aaf89-245">Select Products from under the Category data source and drag it on the form.</span></span> <span data-ttu-id="aaf89-246">Продуктдатагридвиев и Продуктбиндингсаурце добавляются в форму.</span><span class="sxs-lookup"><span data-stu-id="aaf89-246">The productDataGridView and productBindingSource are added to the form.</span></span>
-   <span data-ttu-id="aaf89-247">Измените столбцы в Продуктдатагридвиев.</span><span class="sxs-lookup"><span data-stu-id="aaf89-247">Edit the columns on the productDataGridView.</span></span> <span data-ttu-id="aaf89-248">Мы хотим скрыть столбцы CategoryId и Category и задать для свойства ProductId значение только для чтения.</span><span class="sxs-lookup"><span data-stu-id="aaf89-248">We want to hide the CategoryId and Category columns and set ProductId to read-only.</span></span> <span data-ttu-id="aaf89-249">Значение свойства ProductId создается базой данных после сохранения данных.</span><span class="sxs-lookup"><span data-stu-id="aaf89-249">The value for the ProductId property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="aaf89-250">Щелкните правой кнопкой мыши элемент управления DataGridView и выберите пункт **изменить столбцы...**.</span><span class="sxs-lookup"><span data-stu-id="aaf89-250">Right-click the DataGridView control and select **Edit Columns...**.</span></span>
    -   <span data-ttu-id="aaf89-251">Выберите столбец **ProductID** и установите для параметра **ReadOnly** значение **true**.</span><span class="sxs-lookup"><span data-stu-id="aaf89-251">Select the **ProductId** column and set **ReadOnly** to **True**.</span></span>
    -   <span data-ttu-id="aaf89-252">Выберите столбец **CategoryID** и нажмите кнопку **Удалить** .</span><span class="sxs-lookup"><span data-stu-id="aaf89-252">Select the **CategoryId** column and press the **Remove** button.</span></span> <span data-ttu-id="aaf89-253">Сделайте то же самое с столбцом **Category** .</span><span class="sxs-lookup"><span data-stu-id="aaf89-253">Do the same with the **Category** column.</span></span>
    -   <span data-ttu-id="aaf89-254">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="aaf89-254">Press **OK**.</span></span>

    <span data-ttu-id="aaf89-255">До сих пор мы собрали элементы управления DataGridView с компонентами BindingSource в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="aaf89-255">So far, we associated our DataGridView controls with BindingSource components in the designer.</span></span> <span data-ttu-id="aaf89-256">В следующем разделе мы добавим код в код программной части, чтобы задать Категорибиндингсаурце. DataSource для коллекции сущностей, которые в настоящее время отправляются DbContext.</span><span class="sxs-lookup"><span data-stu-id="aaf89-256">In the next section we will add code to the code behind to set categoryBindingSource.DataSource to the collection of entities that are currently tracked by DbContext.</span></span> <span data-ttu-id="aaf89-257">При перемещении и удалении продуктов из категории в WinForms требовалось настроить свойство Продуктсбиндингсаурце. DataSource для Категорибиндингсаурце и свойства Продуктсбиндингсаурце. DataMember на Products.</span><span class="sxs-lookup"><span data-stu-id="aaf89-257">When we dragged-and-dropped Products from under the Category, the WinForms took care of setting up the productsBindingSource.DataSource property to categoryBindingSource and productsBindingSource.DataMember property to Products.</span></span> <span data-ttu-id="aaf89-258">Из-за этой привязки в Продуктдатагридвиев будут отображаться только продукты, принадлежащие к выбранной категории.</span><span class="sxs-lookup"><span data-stu-id="aaf89-258">Because of this binding, only the products that belong to the currently selected Category will be displayed in the productDataGridView.</span></span>
-   <span data-ttu-id="aaf89-259">Включите кнопку **сохранить** на панели инструментов навигации, щелкнув правой кнопкой мыши и выбрав пункт **включено**.</span><span class="sxs-lookup"><span data-stu-id="aaf89-259">Enable the **Save** button on the Navigation toolbar by clicking the right mouse button and selecting **Enabled**.</span></span>

    ![Конструктор форм 1](~/ef6/media/form1-designer.png)

-   <span data-ttu-id="aaf89-261">Добавьте обработчик событий для кнопки Сохранить, дважды щелкнув кнопку.</span><span class="sxs-lookup"><span data-stu-id="aaf89-261">Add the event handler for the save button by double-clicking on the button.</span></span> <span data-ttu-id="aaf89-262">Это приведет к добавлению обработчика событий и переводу его в код программной части для формы.</span><span class="sxs-lookup"><span data-stu-id="aaf89-262">This will add the event handler and bring you to the code behind for the form.</span></span> <span data-ttu-id="aaf89-263">Код для обработчика **событий \_ Click категорибиндингнавигаторсавеитем** будет добавлен в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="aaf89-263">The code for the **categoryBindingNavigatorSaveItem\_Click** event handler will be added in the next section.</span></span>

## <a name="add-the-code-that-handles-data-interaction"></a><span data-ttu-id="aaf89-264">Добавление кода, обрабатывающего взаимодействие с данными</span><span class="sxs-lookup"><span data-stu-id="aaf89-264">Add the Code that Handles Data Interaction</span></span>

<span data-ttu-id="aaf89-265">Теперь мы добавим код для использования Продуктконтекст для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="aaf89-265">We'll now add the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="aaf89-266">Обновите код для главного окна формы, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="aaf89-266">Update the code for the main form window as shown below.</span></span>

<span data-ttu-id="aaf89-267">Код объявляет долго выполняющийся экземпляр Продуктконтекст.</span><span class="sxs-lookup"><span data-stu-id="aaf89-267">The code declares a long-running instance of ProductContext.</span></span> <span data-ttu-id="aaf89-268">Объект Продуктконтекст используется для запроса и сохранения данных в базе данных.</span><span class="sxs-lookup"><span data-stu-id="aaf89-268">The ProductContext object is used to query and save data to the database.</span></span> <span data-ttu-id="aaf89-269">Затем метод Dispose () в экземпляре Продуктконтекст вызывается из переопределенного метода onclosinging.</span><span class="sxs-lookup"><span data-stu-id="aaf89-269">The Dispose() method on the ProductContext instance is then called from the overridden OnClosing method.</span></span> <span data-ttu-id="aaf89-270">Комментарии к коду содержат сведения о том, что делает код.</span><span class="sxs-lookup"><span data-stu-id="aaf89-270">The code comments provide details about what the code does.</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Data;
    using System.Drawing;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows.Forms;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public partial class Form1 : Form
        {
            ProductContext _context;
            public Form1()
            {
                InitializeComponent();
            }

            protected override void OnLoad(EventArgs e)
            {
                base.OnLoad(e);
                _context = new ProductContext();

                // Call the Load method to get the data for the given DbSet
                // from the database.
                // The data is materialized as entities. The entities are managed by
                // the DbContext instance.
                _context.Categories.Load();

                // Bind the categoryBindingSource.DataSource to
                // all the Unchanged, Modified and Added Category objects that
                // are currently tracked by the DbContext.
                // Note that we need to call ToBindingList() on the
                // ObservableCollection<TEntity> returned by
                // the DbSet.Local property to get the BindingList<T>
                // in order to facilitate two-way binding in WinForms.
                this.categoryBindingSource.DataSource =
                    _context.Categories.Local.ToBindingList();
            }

            private void categoryBindingNavigatorSaveItem_Click(object sender, EventArgs e)
            {
                this.Validate();

                // Currently, the Entity Framework doesn’t mark the entities
                // that are removed from a navigation property (in our example the Products)
                // as deleted in the context.
                // The following code uses LINQ to Objects against the Local collection
                // to find all products and marks any that do not have
                // a Category reference as deleted.
                // The ToList call is required because otherwise
                // the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can do LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                // Save the changes to the database.
                this._context.SaveChanges();

                // Refresh the controls to show the values         
                // that were generated by the database.
                this.categoryDataGridView.Refresh();
                this.productsDataGridView.Refresh();
            }

            protected override void OnClosing(CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }
    }
```

## <a name="test-the-windows-forms-application"></a><span data-ttu-id="aaf89-271">Тестирование приложения Windows Forms</span><span class="sxs-lookup"><span data-stu-id="aaf89-271">Test the Windows Forms Application</span></span>

-   <span data-ttu-id="aaf89-272">Скомпилируйте и запустите приложение, и вы можете протестировать функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="aaf89-272">Compile and run the application and you can test out the functionality.</span></span>

    ![Форма 1 перед сохранением](~/ef6/media/form1beforesave.png)

-   <span data-ttu-id="aaf89-274">После сохранения созданные хранилища ключи отобразятся на экране.</span><span class="sxs-lookup"><span data-stu-id="aaf89-274">After saving the store generated keys are shown on the screen.</span></span>

    ![Форма 1 после сохранения](~/ef6/media/form1aftersave.png)

-   <span data-ttu-id="aaf89-276">Если вы использовали Code First, вы также увидите, что создана база данных **винформсвисефсампле. продуктконтекст** .</span><span class="sxs-lookup"><span data-stu-id="aaf89-276">If you used Code First, then you will also see that a **WinFormswithEFSample.ProductContext** database is created for you.</span></span>

    ![Обозреватель объектов сервера](~/ef6/media/serverobjexplorer.png)
