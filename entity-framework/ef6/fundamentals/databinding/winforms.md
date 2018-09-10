---
title: Привязка данных с помощью WinForms - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 071172810f7dac45f42aca0efa7f329bac31e9cd
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251197"
---
# <a name="databinding-with-winforms"></a><span data-ttu-id="aa617-102">Привязка данных с помощью WinForms</span><span class="sxs-lookup"><span data-stu-id="aa617-102">Databinding with WinForms</span></span>
<span data-ttu-id="aa617-103">Это пошаговое руководство показывает, как выполнить привязку к элементам управления Windows Forms (WinForms) в форме «основной / подробности» типов POCO.</span><span class="sxs-lookup"><span data-stu-id="aa617-103">This step-by-step walkthrough shows how to bind POCO types to Window Forms (WinForms) controls in a “master-detail" form.</span></span> <span data-ttu-id="aa617-104">Приложение использует Entity Framework для заполнения объектов с данными из базы данных, отслеживать изменения и сохранения данных в базе данных.</span><span class="sxs-lookup"><span data-stu-id="aa617-104">The application uses Entity Framework to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="aa617-105">Модель определяет два типа, участвующие в связи один ко многим: категория (основной\\master) и продукта (зависимые\\подробно).</span><span class="sxs-lookup"><span data-stu-id="aa617-105">The model defines two types that participate in one-to-many relationship: Category (principal\\master) and Product (dependent\\detail).</span></span> <span data-ttu-id="aa617-106">Средства Visual Studio, затем используются для привязки типов, определенных в модели для элементов управления WinForms.</span><span class="sxs-lookup"><span data-stu-id="aa617-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WinForms controls.</span></span> <span data-ttu-id="aa617-107">Платформа привязки данных WinForms позволяет осуществлять переходы между связанными объектами: Выбор строк в главное представление приводит представлении «Подробности» для обновления с соответствующих дочерних данных.</span><span class="sxs-lookup"><span data-stu-id="aa617-107">The WinForms data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="aa617-108">Снимки экрана и примеры кода в этом пошаговом руководстве, взяты из Visual Studio 2013, но можно выполнить в этом пошаговом руководстве с помощью Visual Studio 2012 или Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="aa617-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="aa617-109">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="aa617-109">Pre-Requisites</span></span>

<span data-ttu-id="aa617-110">Вам потребуется Visual Studio 2013, Visual Studio 2012 или Visual Studio 2010 установлен для выполнения этого пошагового руководства.</span><span class="sxs-lookup"><span data-stu-id="aa617-110">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="aa617-111">Если вы используете Visual Studio 2010, необходимо также установить NuGet.</span><span class="sxs-lookup"><span data-stu-id="aa617-111">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="aa617-112">Дополнительные сведения см. в разделе [Установка NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="aa617-112">For more information, see [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="aa617-113">Создание приложения</span><span class="sxs-lookup"><span data-stu-id="aa617-113">Create the Application</span></span>

-   <span data-ttu-id="aa617-114">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aa617-114">Open Visual Studio</span></span>
-   <span data-ttu-id="aa617-115">**Файл —&gt; Новинка —&gt; проекта...**</span><span class="sxs-lookup"><span data-stu-id="aa617-115">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="aa617-116">Выберите **Windows** в левой области и **Windows FormsApplication** в области справа</span><span class="sxs-lookup"><span data-stu-id="aa617-116">Select **Windows** in the left pane and **Windows FormsApplication** in the right pane</span></span>
-   <span data-ttu-id="aa617-117">Введите **WinFormswithEFSample** как имя</span><span class="sxs-lookup"><span data-stu-id="aa617-117">Enter **WinFormswithEFSample** as the name</span></span>
-   <span data-ttu-id="aa617-118">Нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="aa617-118">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="aa617-119">Установите пакет Entity Framework NuGet</span><span class="sxs-lookup"><span data-stu-id="aa617-119">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="aa617-120">В обозревателе решений щелкните правой кнопкой мыши **WinFormswithEFSample** проекта</span><span class="sxs-lookup"><span data-stu-id="aa617-120">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="aa617-121">Выберите **управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="aa617-121">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="aa617-122">В диалоговом окне «Управление пакетами NuGet» выберите **Online** вкладку и выберите **EntityFramework** пакета</span><span class="sxs-lookup"><span data-stu-id="aa617-122">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="aa617-123">Нажмите кнопку **установки**</span><span class="sxs-lookup"><span data-stu-id="aa617-123">Click **Install**</span></span>  
    > [!NOTE]
    > <span data-ttu-id="aa617-124">Помимо сборки EntityFramework также добавляется ссылка System.ComponentModel.DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="aa617-124">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="aa617-125">Если проект содержит ссылку на System.Data.Entity, затем он будет удален при установке EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="aa617-125">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="aa617-126">Сборка System.Data.Entity больше не используется для приложений Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="aa617-126">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="implementing-ilistsource-for-collections"></a><span data-ttu-id="aa617-127">Реализация IListSource для коллекций</span><span class="sxs-lookup"><span data-stu-id="aa617-127">Implementing IListSource for Collections</span></span>

<span data-ttu-id="aa617-128">Свойства коллекции необходимо реализовать интерфейс IListSource, чтобы обеспечить двухстороннюю привязку данных с сортировкой, при использовании Windows Forms.</span><span class="sxs-lookup"><span data-stu-id="aa617-128">Collection properties must implement the IListSource interface to enable two-way data binding with sorting when using Windows Forms.</span></span> <span data-ttu-id="aa617-129">Для этого мы хотим расширить ObservableCollection Добавление IListSource функциональных возможностей.</span><span class="sxs-lookup"><span data-stu-id="aa617-129">To do this we are going to extend ObservableCollection to add IListSource functionality.</span></span>

-   <span data-ttu-id="aa617-130">Добавить **ObservableListSource** класс в проект:</span><span class="sxs-lookup"><span data-stu-id="aa617-130">Add an **ObservableListSource** class to the project:</span></span>
    -   <span data-ttu-id="aa617-131">Щелкните правой кнопкой мыши имя проекта</span><span class="sxs-lookup"><span data-stu-id="aa617-131">Right-click on the project name</span></span>
    -   <span data-ttu-id="aa617-132">Выберите **Add -&gt; новый элемент**</span><span class="sxs-lookup"><span data-stu-id="aa617-132">Select **Add -&gt; New Item**</span></span>
    -   <span data-ttu-id="aa617-133">Выберите **класс** и введите **ObservableListSource** имени класса</span><span class="sxs-lookup"><span data-stu-id="aa617-133">Select **Class** and enter **ObservableListSource** for the class name</span></span>
-   <span data-ttu-id="aa617-134">Замените код, созданный по умолчанию следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="aa617-134">Replace the code generated by default with the following code:</span></span>

<span data-ttu-id="aa617-135">*Этот класс позволяет Двухсторонняя привязка, а также для сортировки. Класс является производным от ObservableCollection&lt;T&gt; и добавляет явной реализации IListSource. Метод встречает IListSource реализуется для возврата IBindingList реализация, которая остается синхронизированной с ObservableCollection. Реализация IBindingList, создаваемые ToBindingList поддерживает сортировку. Метод расширения ToBindingList определен в сборке EntityFramework.*</span><span class="sxs-lookup"><span data-stu-id="aa617-135">*This class enables two-way data binding as well as sorting. The class derives from ObservableCollection&lt;T&gt; and adds an explicit implementation of IListSource. The GetList() method of IListSource is implemented to return an IBindingList implementation that stays in sync with the ObservableCollection. The IBindingList implementation generated by ToBindingList supports sorting. The ToBindingList extention method is defined in the EntityFramework assembly.*</span></span>

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
                return _bindingList  (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a><span data-ttu-id="aa617-136">Определение модели</span><span class="sxs-lookup"><span data-stu-id="aa617-136">Define a Model</span></span>

<span data-ttu-id="aa617-137">В этом пошаговом руководстве, вы можете решил реализовать модель с помощью Code First или конструкторе EF.</span><span class="sxs-lookup"><span data-stu-id="aa617-137">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="aa617-138">Выполните одно из двух следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="aa617-138">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="aa617-139">Вариант 1: Определение модели с помощью Code First</span><span class="sxs-lookup"><span data-stu-id="aa617-139">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="aa617-140">В этом разделе показано, как создать модель и его связанные базы данных, с помощью Code First.</span><span class="sxs-lookup"><span data-stu-id="aa617-140">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="aa617-141">Перейдите к следующему разделу (**вариант 2: определение модели с помощью Database First)** Если вы предпочитаете использовать Database First обратить реконструирование модели из базы данных в конструкторе EF</span><span class="sxs-lookup"><span data-stu-id="aa617-141">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="aa617-142">При использовании шаблона разработки Code First обычно начинается с написания классов .NET Framework, которые определяют модель концептуальный (домен).</span><span class="sxs-lookup"><span data-stu-id="aa617-142">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="aa617-143">Добавьте новый **продукта** класс в проект</span><span class="sxs-lookup"><span data-stu-id="aa617-143">Add a new **Product** class to project</span></span>
-   <span data-ttu-id="aa617-144">Замените код, созданный по умолчанию следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="aa617-144">Replace the code generated by default with the following code:</span></span>

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

-   <span data-ttu-id="aa617-145">Добавить **категории** класс в проект.</span><span class="sxs-lookup"><span data-stu-id="aa617-145">Add a **Category** class to the project.</span></span>
-   <span data-ttu-id="aa617-146">Замените код, созданный по умолчанию следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="aa617-146">Replace the code generated by default with the following code:</span></span>

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

<span data-ttu-id="aa617-147">Кроме определения сущностей, необходимо определить класс, производный от **DbContext** и предоставляет **DbSet&lt;TEntity&gt;**  свойства.</span><span class="sxs-lookup"><span data-stu-id="aa617-147">In addition to defining entities, you need to define a class that derives from **DbContext** and exposes **DbSet&lt;TEntity&gt;** properties.</span></span> <span data-ttu-id="aa617-148">**DbSet** свойства let контекст знать, какие типы, которые вы хотите включить в модель.</span><span class="sxs-lookup"><span data-stu-id="aa617-148">The **DbSet** properties let the context know which types you want to include in the model.</span></span> <span data-ttu-id="aa617-149">**DbContext** и **DbSet** определенные типы в сборке EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="aa617-149">The **DbContext** and **DbSet** types are defined in the EntityFramework assembly.</span></span>

<span data-ttu-id="aa617-150">Экземпляр типа DbContext производным управляет объектов сущности во время выполнения, который включает заполнение объектов с данными из базы данных, изменение отслеживания и сохранения данных в базу данных.</span><span class="sxs-lookup"><span data-stu-id="aa617-150">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="aa617-151">Добавьте новый **ProductContext** класс в проект.</span><span class="sxs-lookup"><span data-stu-id="aa617-151">Add a new **ProductContext** class to the project.</span></span>
-   <span data-ttu-id="aa617-152">Замените код, созданный по умолчанию следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="aa617-152">Replace the code generated by default with the following code:</span></span>

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

<span data-ttu-id="aa617-153">Скомпилируйте проект.</span><span class="sxs-lookup"><span data-stu-id="aa617-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="aa617-154">Вариант 2: Определение модели с помощью Database First</span><span class="sxs-lookup"><span data-stu-id="aa617-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="aa617-155">В этом разделе демонстрируется использование Database First чтобы реконструировать модель из базы данных в конструкторе EF.</span><span class="sxs-lookup"><span data-stu-id="aa617-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="aa617-156">Если действия в предыдущем разделе (**вариант 1: определение модели с помощью Code First)**, пропустите этот раздел и перейти непосредственно к **отложенной загрузки** раздел.</span><span class="sxs-lookup"><span data-stu-id="aa617-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="aa617-157">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="aa617-157">Create an Existing Database</span></span>

<span data-ttu-id="aa617-158">Обычно при ориентировании существующей базы данных, он будет уже создан, но для этого пошагового руководства необходимо создать базу данных для доступа к.</span><span class="sxs-lookup"><span data-stu-id="aa617-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="aa617-159">Сервер базы данных, который устанавливается вместе с Visual Studio отличается в зависимости от версии Visual Studio, вы установили:</span><span class="sxs-lookup"><span data-stu-id="aa617-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="aa617-160">Если вы используете Visual Studio 2010 вы создадите базу данных SQL Express.</span><span class="sxs-lookup"><span data-stu-id="aa617-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="aa617-161">Если вы используете Visual Studio 2012, а затем вы создадите [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) базы данных.</span><span class="sxs-lookup"><span data-stu-id="aa617-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="aa617-162">Перейдем дальше и создать базу данных.</span><span class="sxs-lookup"><span data-stu-id="aa617-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="aa617-163">**Представление —&gt; обозревателя серверов**</span><span class="sxs-lookup"><span data-stu-id="aa617-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="aa617-164">Щелкните правой кнопкой мыши **подключения к данным -&gt; добавить соединение...**</span><span class="sxs-lookup"><span data-stu-id="aa617-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="aa617-165">Если вы не подключились к базе данных с помощью обозревателя сервера прежде, чем вам потребуется выбрать в качестве источника данных Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="aa617-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Изменение источника данных](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="aa617-167">Подключение к LocalDB или SQL Express, в зависимости от того, какой из них вы установили и введите **продуктов** имя базы данных</span><span class="sxs-lookup"><span data-stu-id="aa617-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Добавить подключение LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Добавление подключения Express](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="aa617-170">Выберите **ОК** и вам нужно будет Если вы хотите создать новую базу данных, выберите **Да**</span><span class="sxs-lookup"><span data-stu-id="aa617-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Создание базы данных](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="aa617-172">Новой базы данных будут отображаться в обозревателе сервера щелкните его правой кнопкой мыши и выберите **новый запрос**</span><span class="sxs-lookup"><span data-stu-id="aa617-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="aa617-173">Скопируйте следующий запрос SQL в новый запрос, а затем щелкните правой кнопкой мыши запрос и выберите **Execute**</span><span class="sxs-lookup"><span data-stu-id="aa617-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="aa617-174">Реконструирование модели</span><span class="sxs-lookup"><span data-stu-id="aa617-174">Reverse Engineer Model</span></span>

<span data-ttu-id="aa617-175">Мы собираемся использовать Entity Framework Designer, который входит в состав Visual Studio, для создания нашей модели.</span><span class="sxs-lookup"><span data-stu-id="aa617-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="aa617-176">**Проект -&gt; добавить новый элемент...**</span><span class="sxs-lookup"><span data-stu-id="aa617-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="aa617-177">Выберите **данных** в меню слева и затем **модель EDM ADO.NET**</span><span class="sxs-lookup"><span data-stu-id="aa617-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="aa617-178">Введите **ProductModel** имя и нажмите кнопку **ОК**</span><span class="sxs-lookup"><span data-stu-id="aa617-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="aa617-179">Это откроет **мастер моделей EDM**</span><span class="sxs-lookup"><span data-stu-id="aa617-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="aa617-180">Выберите **создать из базы данных** и нажмите кнопку **Далее**</span><span class="sxs-lookup"><span data-stu-id="aa617-180">Select **Generate from Database** and click **Next**</span></span>

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="aa617-182">Выберите соединение с базой данных, созданной в первом разделе, введите **ProductContext** как имя строки подключения и нажмите кнопку **Далее**</span><span class="sxs-lookup"><span data-stu-id="aa617-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Выберите соединение](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="aa617-184">Установите флажок рядом с «Таблицы», чтобы импортировать все таблицы и нажмите кнопку «Готово»</span><span class="sxs-lookup"><span data-stu-id="aa617-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Выбор объектов](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="aa617-186">После завершения процесса реконструирования новой модели добавлен в проект и открывается для просмотра в конструкторе Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="aa617-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="aa617-187">Файл App.config также был добавлен в проект со сведениями о подключении для базы данных.</span><span class="sxs-lookup"><span data-stu-id="aa617-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="aa617-188">Дополнительные действия в Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="aa617-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="aa617-189">Если вы работаете в Visual Studio 2010 будет необходимо обновить конструктор EF будет использовать создание кода EF6.</span><span class="sxs-lookup"><span data-stu-id="aa617-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="aa617-190">Щелкните правой кнопкой мыши пустое место модели в конструкторе EF и выберите **добавить элемент формирования кода...**</span><span class="sxs-lookup"><span data-stu-id="aa617-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="aa617-191">Выберите **шаблоны в Интернете** из меню слева и выполните поиск **DbContext**</span><span class="sxs-lookup"><span data-stu-id="aa617-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="aa617-192">Выберите **EF 6.x генератор DbContext для C\#,** введите **ProductsModel** как имя и щелкните "Добавить"</span><span class="sxs-lookup"><span data-stu-id="aa617-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="aa617-193">Обновление создание кода для привязки данных</span><span class="sxs-lookup"><span data-stu-id="aa617-193">Updating code generation for data binding</span></span>

<span data-ttu-id="aa617-194">EF создает код из модели с помощью шаблонов T4.</span><span class="sxs-lookup"><span data-stu-id="aa617-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="aa617-195">Шаблоны, поставляемых с Visual Studio или скачать из коллекции Visual Studio предназначены для использования общего назначения.</span><span class="sxs-lookup"><span data-stu-id="aa617-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="aa617-196">Это означает, что сущности, созданные на основе этих шаблонов имеют простой ICollection&lt;T&gt; свойства.</span><span class="sxs-lookup"><span data-stu-id="aa617-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="aa617-197">Тем не менее при выполнении привязки данных желательно иметь свойства коллекции, которые реализуют IListSource.</span><span class="sxs-lookup"><span data-stu-id="aa617-197">However, when doing data binding it is desirable to have collection properties that implement IListSource.</span></span> <span data-ttu-id="aa617-198">Именно поэтому мы создали класс ObservableListSource выше, и теперь мы собираемся изменить шаблоны, чтобы сделать использование этого класса.</span><span class="sxs-lookup"><span data-stu-id="aa617-198">This is why we created the ObservableListSource class above and we are now going to modify the templates to make use of this class.</span></span>

-   <span data-ttu-id="aa617-199">Откройте **обозревателе решений** и найти **ProductModel.edmx** файла</span><span class="sxs-lookup"><span data-stu-id="aa617-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="aa617-200">Найти **ProductModel.tt** файл, который будет вложен в файле ProductModel.edmx</span><span class="sxs-lookup"><span data-stu-id="aa617-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Шаблон модели продукта](~/ef6/media/productmodeltemplate.png)

-   <span data-ttu-id="aa617-202">Дважды щелкните файл ProductModel.tt, чтобы открыть его в редакторе Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aa617-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="aa617-203">Найти и заменить два вхождения "**ICollection**«with»**ObservableListSource**«.</span><span class="sxs-lookup"><span data-stu-id="aa617-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="aa617-204">Они находятся в около строки 296 и 484.</span><span class="sxs-lookup"><span data-stu-id="aa617-204">These are located at approximately lines 296 and 484.</span></span>
-   <span data-ttu-id="aa617-205">Найти и заменить первое вхождение "**HashSet**«with»**ObservableListSource**«.</span><span class="sxs-lookup"><span data-stu-id="aa617-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="aa617-206">Это событие находится примерно в строке 50.</span><span class="sxs-lookup"><span data-stu-id="aa617-206">This occurrence is located at approximately line 50.</span></span> <span data-ttu-id="aa617-207">**Не** замените второе вхождение HashSet Найти далее в коде.</span><span class="sxs-lookup"><span data-stu-id="aa617-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="aa617-208">Сохраните файл ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="aa617-208">Save the ProductModel.tt file.</span></span> <span data-ttu-id="aa617-209">Это может вызвать код для сущностей, быть создан повторно.</span><span class="sxs-lookup"><span data-stu-id="aa617-209">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="aa617-210">Если код не создается автоматически, затем щелкнуть правой кнопкой мыши ProductModel.tt и выберите «Запустить пользовательский инструмент».</span><span class="sxs-lookup"><span data-stu-id="aa617-210">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="aa617-211">Если вы теперь откройте файл Category.cs (который вложен в ProductModel.tt), то вы увидите, что продукты коллекция имеет тип **ObservableListSource&lt;продукта&gt;**.</span><span class="sxs-lookup"><span data-stu-id="aa617-211">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableListSource&lt;Product&gt;**.</span></span>

<span data-ttu-id="aa617-212">Скомпилируйте проект.</span><span class="sxs-lookup"><span data-stu-id="aa617-212">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="aa617-213">Отложенная загрузка</span><span class="sxs-lookup"><span data-stu-id="aa617-213">Lazy Loading</span></span>

<span data-ttu-id="aa617-214">**Продуктов** свойство **категории** класс и **категории** свойство **продукта** класс — это свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="aa617-214">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="aa617-215">В Entity Framework свойства навигации позволяют для перехода по связи между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="aa617-215">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="aa617-216">EF дает возможность загрузки связанных сущностей из базы данных автоматически при первом обращении к свойству навигации.</span><span class="sxs-lookup"><span data-stu-id="aa617-216">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="aa617-217">С помощью этого типа загрузки (называется отложенной загрузки) Имейте в виду, что при первом доступе к каждому свойству навигации отдельный запрос будет выполнен в базе данных, если содержимое еще не в контексте.</span><span class="sxs-lookup"><span data-stu-id="aa617-217">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="aa617-218">При использовании типов сущностей POCO, EF обеспечивает отложенную загрузку, создании экземпляров производного прокси-типов во время выполнения и затем переопределение виртуальных свойств в классах для добавления обработчика загрузки.</span><span class="sxs-lookup"><span data-stu-id="aa617-218">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="aa617-219">Чтобы получить отложенная загрузка связанных объектов, необходимо объявить методы получения свойств как навигации **открытый** и **виртуального** (**Overridable** в Visual Basic), и вы класс не должен быть **запечатанный** (**NotOverridable** в Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="aa617-219">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="aa617-220">Когда базы данных с помощью первого свойства навигации, автоматически становятся виртуальные, чтобы включить отложенную загрузку.</span><span class="sxs-lookup"><span data-stu-id="aa617-220">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="aa617-221">В разделе Code First, мы решили сделать виртуальные свойства навигации по той же причине</span><span class="sxs-lookup"><span data-stu-id="aa617-221">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="aa617-222">Привязка объектов к элементам управления</span><span class="sxs-lookup"><span data-stu-id="aa617-222">Bind Object to Controls</span></span>

<span data-ttu-id="aa617-223">Добавьте классы, которые определены в модели в качестве источников данных для этого приложения WinForms.</span><span class="sxs-lookup"><span data-stu-id="aa617-223">Add the classes that are defined in the model as data sources for this WinForms application.</span></span>

-   <span data-ttu-id="aa617-224">В главном меню выберите **проект -&gt; добавить новый источник данных...**</span><span class="sxs-lookup"><span data-stu-id="aa617-224">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="aa617-225">(в Visual Studio 2010, необходимо выбрать **тарифный план —&gt; добавить новый источник данных...** )</span><span class="sxs-lookup"><span data-stu-id="aa617-225">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="aa617-226">В списке выберите тип источника данных окна выберите **объект** и нажмите кнопку **Далее**</span><span class="sxs-lookup"><span data-stu-id="aa617-226">In the Choose a Data Source Type window, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="aa617-227">В диалоговом окне объекты данных выберите unfold **WinFormswithEFSample** двух раз и выберите **категории** нет необходимости для выбора источника данных Product, так как мы вернемся к нему через продукта свойство в источнике данных категории.</span><span class="sxs-lookup"><span data-stu-id="aa617-227">In the Select the Data Objects dialog, unfold the **WinFormswithEFSample** two times and select **Category** There is no need to select the Product data source, because we will get to it through the Product’s property on the Category data source.</span></span>

    ![источника данных](~/ef6/media/datasource.png)

-   <span data-ttu-id="aa617-229">Нажмите кнопку **Готово.** 
     *Если окно "Источники данных", не отображается, выберите \*\*\* представление —&gt; Other Windows -&gt; источников данных*\*</span><span class="sxs-lookup"><span data-stu-id="aa617-229">Click **Finish.**
*If the Data Sources window is not showing up, select\*\*\*View -&gt; Other Windows-&gt; Data Sources*\*</span></span>
-   <span data-ttu-id="aa617-230">Скрыть значок ПИН-код, поэтому окна источников данных автоматически.</span><span class="sxs-lookup"><span data-stu-id="aa617-230">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="aa617-231">Может потребоваться нажмите кнопку "Обновить", если окно уже было открыто.</span><span class="sxs-lookup"><span data-stu-id="aa617-231">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Источник данных 2](~/ef6/media/datasource2.png)

-   <span data-ttu-id="aa617-233">В обозревателе решений дважды щелкните **Form1.cs** файл, чтобы открыть главную форму в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="aa617-233">In Solution Explorer, double-click the **Form1.cs** file to open the main form in designer.</span></span>
-   <span data-ttu-id="aa617-234">Выберите **категории** источника данных и перетащите его на форме.</span><span class="sxs-lookup"><span data-stu-id="aa617-234">Select the **Category** data source and drag it on the form.</span></span> <span data-ttu-id="aa617-235">По умолчанию новый DataGridView (**categoryDataGridView**) и элементы управления панели инструментов навигации добавляются в конструктор.</span><span class="sxs-lookup"><span data-stu-id="aa617-235">By default, a new DataGridView (**categoryDataGridView**) and Navigation toolbar controls are added to the designer.</span></span> <span data-ttu-id="aa617-236">Эти элементы управления привязаны к BindingSource (**categoryBindingSource**) и привязка Навигатор (**categoryBindingNavigator**) компонентов, которые также будут созданы.</span><span class="sxs-lookup"><span data-stu-id="aa617-236">These controls are bound to the BindingSource (**categoryBindingSource**) and Binding Navigator (**categoryBindingNavigator**) components that are created as well.</span></span>
-   <span data-ttu-id="aa617-237">Изменить столбцы, на **categoryDataGridView**.</span><span class="sxs-lookup"><span data-stu-id="aa617-237">Edit the columns on the **categoryDataGridView**.</span></span> <span data-ttu-id="aa617-238">Нам нужно выбрать **CategoryId** столбец только для чтения.</span><span class="sxs-lookup"><span data-stu-id="aa617-238">We want to set the **CategoryId** column to read-only.</span></span> <span data-ttu-id="aa617-239">Значение для **CategoryId** после сохранения данных в базе данных создается свойство.</span><span class="sxs-lookup"><span data-stu-id="aa617-239">The value for the **CategoryId** property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="aa617-240">Щелкните правой кнопкой мыши элемента управления DataGridView и выберите столбцы, изменить...</span><span class="sxs-lookup"><span data-stu-id="aa617-240">Right-click the DataGridView control and select Edit Columns…</span></span>
    -   <span data-ttu-id="aa617-241">Выберите столбец CategoryId и присвоено значение True только для чтения</span><span class="sxs-lookup"><span data-stu-id="aa617-241">Select the CategoryId column and set ReadOnly to True</span></span>
    -   <span data-ttu-id="aa617-242">Нажмите кнопку ОК</span><span class="sxs-lookup"><span data-stu-id="aa617-242">Press OK</span></span>
-   <span data-ttu-id="aa617-243">Выберите продукты из категории источника данных и перетащите его на форме.</span><span class="sxs-lookup"><span data-stu-id="aa617-243">Select Products from under the Category data source and drag it on the form.</span></span> <span data-ttu-id="aa617-244">ProductDataGridView и productBindingSource добавляются в форму.</span><span class="sxs-lookup"><span data-stu-id="aa617-244">The productDataGridView and productBindingSource are added to the form.</span></span>
-   <span data-ttu-id="aa617-245">Измените столбцы, на productDataGridView.</span><span class="sxs-lookup"><span data-stu-id="aa617-245">Edit the columns on the productDataGridView.</span></span> <span data-ttu-id="aa617-246">Нам нужно скрыть столбцы CategoryId и категории и значение ProductId только для чтения.</span><span class="sxs-lookup"><span data-stu-id="aa617-246">We want to hide the CategoryId and Category columns and set ProductId to read-only.</span></span> <span data-ttu-id="aa617-247">Значение свойства ProductId создается в базе данных после сохранения данных.</span><span class="sxs-lookup"><span data-stu-id="aa617-247">The value for the ProductId property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="aa617-248">Щелкните правой кнопкой мыши элемента управления DataGridView и выберите **Правка столбцов...** .</span><span class="sxs-lookup"><span data-stu-id="aa617-248">Right-click the DataGridView control and select **Edit Columns...**.</span></span>
    -   <span data-ttu-id="aa617-249">Выберите **ProductId** столбцы и наборы **ReadOnly** для **True**.</span><span class="sxs-lookup"><span data-stu-id="aa617-249">Select the **ProductId** column and set **ReadOnly** to **True**.</span></span>
    -   <span data-ttu-id="aa617-250">Выберите **CategoryId** столбец и нажмите клавишу **удалить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="aa617-250">Select the **CategoryId** column and press the **Remove** button.</span></span> <span data-ttu-id="aa617-251">Сделайте то же самое с **категории** столбца.</span><span class="sxs-lookup"><span data-stu-id="aa617-251">Do the same with the **Category** column.</span></span>
    -   <span data-ttu-id="aa617-252">Нажмите клавишу **ОК**.</span><span class="sxs-lookup"><span data-stu-id="aa617-252">Press **OK**.</span></span>

    <span data-ttu-id="aa617-253">На данный момент мы связанный элемент управления DataGridView с компонентами BindingSource в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="aa617-253">So far, we associated our DataGridView controls with BindingSource components in the designer.</span></span> <span data-ttu-id="aa617-254">В следующем разделе будет добавлен код для кода программной части присвоено categoryBindingSource.DataSource коллекцию сущностей, которые в настоящее время отслеживаются с DbContext.</span><span class="sxs-lookup"><span data-stu-id="aa617-254">In the next section we will add code to the code behind to set categoryBindingSource.DataSource to the collection of entities that are currently tracked by DbContext.</span></span> <span data-ttu-id="aa617-255">При мы перетащить и удалении продукты из категории, WinForms занималась Настройка свойство productsBindingSource.DataSource свойству categoryBindingSource и productsBindingSource.DataMember к продуктам.</span><span class="sxs-lookup"><span data-stu-id="aa617-255">When we dragged-and-dropped Products from under the Category, the WinForms took care of setting up the productsBindingSource.DataSource property to categoryBindingSource and productsBindingSource.DataMember property to Products.</span></span> <span data-ttu-id="aa617-256">Из-за этой привязкой, только продукты, принадлежащие к текущей выбранной категории будет отображаться в productDataGridView.</span><span class="sxs-lookup"><span data-stu-id="aa617-256">Because of this binding, only the products that belong to the currently selected Category will be displayed in the productDataGridView.</span></span>
-   <span data-ttu-id="aa617-257">Включить **Сохранить** кнопку на панели навигации, щелкнув правой кнопкой мыши и выбрав **включено**.</span><span class="sxs-lookup"><span data-stu-id="aa617-257">Enable the **Save** button on the Navigation toolbar by clicking the right mouse button and selecting **Enabled**.</span></span>

    ![Конструктор форм 1](~/ef6/media/form1-designer.png)

-   <span data-ttu-id="aa617-259">Добавьте обработчик событий для сохранения кнопки, дважды щелкнув кнопку.</span><span class="sxs-lookup"><span data-stu-id="aa617-259">Add the event handler for the save button by double-clicking on the button.</span></span> <span data-ttu-id="aa617-260">Это будет добавьте обработчик событий и вы хотели кода программной части формы.</span><span class="sxs-lookup"><span data-stu-id="aa617-260">This will add the event handler and bring you to the code behind for the form.</span></span> <span data-ttu-id="aa617-261">Код для **categoryBindingNavigatorSaveItem\_щелкните** обработчик событий будет добавлена в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="aa617-261">The code for the **categoryBindingNavigatorSaveItem\_Click** event handler will be added in the next section.</span></span>

## <a name="add-the-code-that-handles-data-interaction"></a><span data-ttu-id="aa617-262">Добавьте код, который обрабатывает взаимодействие с данными</span><span class="sxs-lookup"><span data-stu-id="aa617-262">Add the Code that Handles Data Interaction</span></span>

<span data-ttu-id="aa617-263">Теперь мы добавим код, чтобы использовать ProductContext производить доступ к данным.</span><span class="sxs-lookup"><span data-stu-id="aa617-263">We'll now add the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="aa617-264">Обновите код для окна главной формы, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="aa617-264">Update the code for the main form window as shown below.</span></span>

<span data-ttu-id="aa617-265">Код объявляет экземпляр ProductContext выполняющейся длительное время.</span><span class="sxs-lookup"><span data-stu-id="aa617-265">The code declares a long-running instance of ProductContext.</span></span> <span data-ttu-id="aa617-266">Объект ProductContext позволяет запрашивать и сохранять данные в базу данных.</span><span class="sxs-lookup"><span data-stu-id="aa617-266">The ProductContext object is used to query and save data to the database.</span></span> <span data-ttu-id="aa617-267">Метод Dispose() в экземпляре ProductContext затем вызывается из переопределенного метода OnClosing.</span><span class="sxs-lookup"><span data-stu-id="aa617-267">The Dispose() method on the ProductContext instance is then called from the overridden OnClosing method.</span></span> <span data-ttu-id="aa617-268">В комментариях к коду содержат сведения о что делает код.</span><span class="sxs-lookup"><span data-stu-id="aa617-268">The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-windows-forms-application"></a><span data-ttu-id="aa617-269">Тестирование приложения Windows Forms</span><span class="sxs-lookup"><span data-stu-id="aa617-269">Test the Windows Forms Application</span></span>

-   <span data-ttu-id="aa617-270">Компиляции и выполнения приложения и вы можете проверить работу функции.</span><span class="sxs-lookup"><span data-stu-id="aa617-270">Compile and run the application and you can test out the functionality.</span></span>

    ![Форма 1 перед сохранением](~/ef6/media/form1beforesave.png)

-   <span data-ttu-id="aa617-272">После сохранения хранения созданных ключей отображаются на экране.</span><span class="sxs-lookup"><span data-stu-id="aa617-272">After saving the store generated keys are shown on the screen.</span></span>

    ![Форме 1 после сохранения](~/ef6/media/form1aftersave.png)

-   <span data-ttu-id="aa617-274">Если вы использовали Code First, то вы также увидите, что **WinFormswithEFSample.ProductContext** база данных создается автоматически.</span><span class="sxs-lookup"><span data-stu-id="aa617-274">If you used Code First, then you will also see that a **WinFormswithEFSample.ProductContext** database is created for you.</span></span>

    ![Обозреватель объектов Server](~/ef6/media/serverobjexplorer.png)
