---
title: Привязка данных с помощью WinForms-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 4b3eee20ff238864b94ef4edfb97c1bae0713300
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181790"
---
# <a name="databinding-with-winforms"></a>Привязка данных с помощью WinForms
В этом пошаговом руководстве показано, как привязать типы POCO к элементам управления Window Forms (WinForms) в форме "основной/подробности". Приложение использует Entity Framework для заполнения объектов данными из базы данных, трассировки изменений и сохранения данных в базе данных.

Модель определяет два типа, участвующие в связи "один ко многим": Категория (основная\\Главная) и продукт (зависимые\\сведения). Затем средства Visual Studio используются для привязки типов, определенных в модели, к элементам управления WinForms. Платформа привязки данных WinForms позволяет переходить между связанными объектами: выбор строк в главном представлении приводит к обновлению подробного представления соответствующими дочерними данными.

Снимки экрана и списки кода в этом пошаговом руководстве взяты из Visual Studio 2013 но вы можете выполнить это пошаговое руководство с помощью Visual Studio 2012 или Visual Studio 2010.

## <a name="pre-requisites"></a>Предварительные требования

Для работы с этим пошаговым руководством необходимо установить Visual Studio 2013, Visual Studio 2012 или Visual Studio 2010.

Если вы используете Visual Studio 2010, необходимо также установить NuGet. Дополнительные сведения см. в разделе [Установка NuGet](https://docs.nuget.org/docs/start-here/installing-nuget).

## <a name="create-the-application"></a>Создание приложения

-   Запустите Visual Studio.
-   **Файл-&gt; проект New-&gt;...**
-   Выберите **Windows** в левой области и **Windows формсаппликатион** в правой области.
-   Введите **винформсвисефсампле** в качестве имени
-   Нажмите кнопку **ОК**

## <a name="install-the-entity-framework-nuget-package"></a>Установка пакета NuGet Entity Framework

-   В обозреватель решений щелкните правой кнопкой мыши проект **винформсвисефсампле**
-   Выберите **Управление пакетами NuGet...**
-   В диалоговом окне Управление пакетами NuGet выберите вкладку в **сети** и выберите пакет **EntityFramework** .
-   Нажмите кнопку **Установить**  
    > [!NOTE]
    > В дополнение к сборке EntityFramework также добавляется ссылка на System. ComponentModel. Аннотация. Если проект содержит ссылку на System. Data. Entity, то он будет удален при установке пакета EntityFramework. Сборка System. Data. Entity больше не используется для приложений Entity Framework 6.

## <a name="implementing-ilistsource-for-collections"></a>Реализация IListSource для коллекций

Свойства коллекции должны реализовывать интерфейс IListSource, чтобы обеспечить двустороннюю привязку данных с сортировкой при использовании Windows Forms. Для этого мы собираемся расширить коллекцию ObservableCollection, чтобы добавить IListSource функциональность.

-   Добавьте класс **обсерваблелистсаурце** в проект:
    -   Щелкните правой кнопкой мыши имя проекта
    -   Выберите **Добавить-&gt; новый элемент**
    -   Выберите **класс** и введите **обсерваблелистсаурце** в качестве имени класса.
-   Замените код, созданный по умолчанию, следующим кодом:

*Этот класс обеспечивает двустороннюю привязку данных, а также сортировку. Класс является производным от ObservableCollection&lt;T&gt; и добавляет явную реализацию IListSource. Метод метода WebMethod () класса IListSource реализуется для возврата реализации IBindingList, которая остается синхронизированной с ObservableCollection. Реализация IBindingList, созданная Тобиндинглист, поддерживает сортировку. Метод расширения Тобиндинглист определен в сборке EntityFramework.*

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

## <a name="define-a-model"></a>Определение модели

В этом пошаговом руководстве можно выбрать реализацию модели с помощью Code First или конструктора EF. Выполните один из двух следующих разделов.

### <a name="option-1-define-a-model-using-code-first"></a>Вариант 1. Определение модели с помощью Code First

В этом разделе показано, как создать модель и связанную с ней базу данных с помощью Code First. Перейдите к следующему разделу (**вариант 2. Определение модели с помощью Database First)** , если вы предпочитаете использовать Database First для реконструирования модели из базы данных с помощью конструктора EF.

При использовании Code First разработки обычно начинается написание .NET Framework классов, определяющих концептуальную (доменную) модель.

-   Добавление нового класса **Product** в проект
-   Замените код, созданный по умолчанию, следующим кодом:

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

-   Добавьте в проект класс **Category** .
-   Замените код, созданный по умолчанию, следующим кодом:

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

В дополнение к определению сущностей необходимо определить класс, производный от **DbContext** и предоставляющий **DbSet&lt;&gt;** свойств. Свойства **DbSet** позволяют контексту понять, какие типы необходимо включить в модель. Типы **DbContext** и **DbSet** определены в сборке EntityFramework.

Экземпляр производного типа DbContext управляет объектами сущностей во время выполнения, включая заполнение объектов данными из базы данных, отслеживание изменений и сохранение данных в базе данных.

-   Добавьте в проект новый класс **продуктконтекст** .
-   Замените код, созданный по умолчанию, следующим кодом:

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

Скомпилируйте проект.

### <a name="option-2-define-a-model-using-database-first"></a>Вариант 2. Определение модели с помощью Database First

В этом разделе показано, как использовать Database First для реконструирования модели из базы данных с помощью конструктора EF. Если вы завершили предыдущий раздел (**вариант 1. Определение модели с помощью Code First)** , пропустите этот раздел и перейдите непосредственно к разделу " **ленивая загрузка** ".

#### <a name="create-an-existing-database"></a>Создание существующей базы данных

Обычно при ориентировании на существующую базу данных она уже будет создана, но в этом пошаговом руководстве нам нужно создать базу данных для доступа.

Сервер базы данных, который устанавливается вместе с Visual Studio, отличается в зависимости от версии Visual Studio, которую вы установили:

-   Если вы используете Visual Studio 2010, вы создадите базу данных SQL Express.
-   Если вы используете Visual Studio 2012, вы создадите базу данных [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).

Перейдем дальше и создадим базу данных.

-   **Вид" —&gt; "Обозреватель сервера**
-   Щелкните правой кнопкой мыши на **Подключения к данным -&gt; Добавить подключение…**
-   Если вы не подключались к базе данных с помощью "Обозревателя сервера" ранее, потребуется выбрать Microsoft SQL Server в качестве источника данных

    ![Изменение источника данных](~/ef6/media/changedatasource.png)

-   Подключитесь к LocalDB или SQL Express, в зависимости от того, какой из них установлен, и введите **Products** в качестве имени базы данных.

    ![Добавить соединение LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Добавить подключение Express](~/ef6/media/addconnectionexpress.png)

-   Выберите **ОК**, и вам будет задан вопрос, хотите ли вы создать новую базу данных. Выберите **Да**

    ![Создание базы данных](~/ef6/media/createdatabase.png)

-   Новая база данных появится в Обозревателе сервера. Щелкните ее правой кнопкой мыши и выберите **Новый запрос**
-   Скопируйте следующий код SQL в новый запрос, а затем щелкните запрос правой кнопкой мыши и выберите **Выполнить**

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

#### <a name="reverse-engineer-model"></a>Реконструируйте модель

Для создания нашей модели мы собираемся использовать конструктор Entity Framework Designer, который входит в состав Visual Studio.

-   **Проект -&gt; Добавить новый элемент…**
-   Выберите **Данные** в меню слева и затем **Модель ADO.NET EDM**
-   Введите **ProductModel** в качестве имени и нажмите кнопку **ОК** .
-   Это откроет **Мастер моделей EDM**
-   Выберите **Создать из базы данных** и нажмите кнопку **Далее**

    ![чусемоделконтентс](~/ef6/media/choosemodelcontents.png)

-   Выберите подключение к базе данных, созданной в первом разделе, введите **продуктконтекст** в качестве имени строки подключения и нажмите кнопку **Далее** .

    ![Выбор подключения](~/ef6/media/chooseyourconnection.png)

-   Установите флажок рядом с пунктом "таблицы", чтобы импортировать все таблицы, и нажмите кнопку "Готово".

    ![Выбор объектов](~/ef6/media/chooseyourobjects.png)

После завершения процесса реконструирования новая модель будет добавлена в проект и откроется для просмотра в Entity Framework Designer. В проект также будет добавлен файл App.config со сведениями о подключении к базе данных.

#### <a name="additional-steps-in-visual-studio-2010"></a>Дополнительные действия в Visual Studio 2010

При работе в Visual Studio 2010 необходимо обновить конструктор EF для использования создания кода EF6.

-   Щелкните правой кнопкой мыши на пустом месте модели в EF Designer и выберите **Добавить элемент создания кода…**
-   Выберите **Шаблоны в Интернете** из меню слева и выполните поиск **DbContext**
-   Выберите **генератор EF 6. x DbContext для C\#,** введите **продуктсмодел** в качестве имени и нажмите кнопку Добавить.

#### <a name="updating-code-generation-for-data-binding"></a>Обновление создания кода для привязки данных

EF создает код из модели с помощью шаблонов T4. Шаблоны, поставляемые с Visual Studio или скачанные из коллекции Visual Studio, предназначены для использования в общих целях. Это означает, что сущности, созданные на основе этих шаблонов, имеют простые&lt;свойства ICollection&gt;. Однако при выполнении привязки данных желательно иметь свойства коллекции, реализующие IListSource. Именно поэтому мы создали класс Обсерваблелистсаурце выше, и теперь будем изменять шаблоны, чтобы использовать этот класс.

-   Откройте **Обозреватель решений** и найдите файл **ProductModel. EDMX**
-   Найдите файл **ProductModel.TT** , который будет вложен в файл ProductModel. EDMX

    ![Шаблон модели продукта](~/ef6/media/productmodeltemplate.png)

-   Дважды щелкните файл ProductModel.tt, чтобы открыть его в редакторе Visual Studio.
-   Найдите и замените два вхождения "**ICollection**" на "**обсерваблелистсаурце**". Они расположены примерно в строках 296 и 484.
-   Найдите и замените первое вхождение "**hashing**" на "**обсерваблелистсаурце**". Это вхождение расположено примерно в строке 50. **Не** заменяйте второе вхождение набора хэширования, найденного далее в коде.
-   Сохраните файл ProductModel.tt. Это должно привести к повторному формированию кода для сущностей. Если код не воссоздается автоматически, щелкните правой кнопкой мыши ProductModel.tt и выберите команду "запустить пользовательский инструмент".

Если теперь открыть файл Category.cs (который вложен в ProductModel.tt), вы увидите, что коллекция Products имеет тип **обсерваблелистсаурце&lt;продукт&gt;** .

Скомпилируйте проект.

## <a name="lazy-loading"></a>Отложенная загрузка

Свойство **Products** класса **Category** и свойство **Category** в классе **Product** являются свойствами навигации. В Entity Framework свойства навигации предоставляют способ навигации по связям между двумя типами сущностей.

EF дает возможность автоматически загружать связанные сущности из базы данных при первом доступе к свойству навигации. При таком типе загрузки (называемой отложенной загрузкой) имейте в виду, что при первом обращении к каждому свойству навигации будет выполнен отдельный запрос к базе данных, если содержимое еще не находится в контексте.

При использовании типов сущностей POCO EF достигает отложенной загрузки, создавая экземпляры производных прокси-типов во время выполнения, а затем переопределяя виртуальные свойства в классах для добавления обработчика загрузки. Чтобы получить отложенную загрузку связанных объектов, необходимо объявить методы считывания свойств навигации как **открытые** и **Виртуальные** (**переопределяемые** в Visual Basic), и класс не должен быть **запечатанным** (**NotOverridable** в Visual Basic). При использовании Database First свойства навигации автоматически становятся виртуальными для включения отложенной загрузки. В разделе Code First мы решили сделать свойства навигации виртуальными по той же причине.

## <a name="bind-object-to-controls"></a>Привязка объекта к элементам управления

Добавьте классы, определенные в модели, в качестве источников данных для этого приложения WinForms.

-   В главном меню выберите **проект —&gt; добавить новый источник данных...**
    (в Visual Studio 2010 необходимо выбрать **данные —&gt; добавить новый источник данных...** )
-   В окне Выбор типа источника данных выберите **объект** и нажмите кнопку **Далее** .
-   В диалоговом окне Выбор объектов данных unfold **винформсвисефсампле** два раза и выберите пункт **Категория** нет необходимости выбирать источник данных продукта, так как мы будем получать его через свойство продукта в источнике данных категории.

    ![Источник данных](~/ef6/media/datasource.png)

-   Нажмите кнопку **Готово**.
    Если окно Источники данных не отображается, выберите **вид-&gt; другие источники данных&gt; Windows**
-   Нажмите значок закрепить, чтобы окно Источники данных не скрывалось автоматически. Если окно уже видимо, может потребоваться нажать кнопку Обновить.

    ![Источник данных 2](~/ef6/media/datasource2.png)

-   В обозреватель решений дважды щелкните файл **Form1.CS** , чтобы открыть главную форму в конструкторе.
-   Выберите источник данных **Категория** и перетащите его в форму. По умолчанию в конструктор добавляются новые элементы управления DataGridView (**категоридатагридвиев**) и панели инструментов навигации. Эти элементы управления привязаны к компонентам BindingSource (**категорибиндингсаурце**) и Binding Navigator (**категорибиндингнавигатор**), которые также создаются.
-   Измените столбцы в **категоридатагридвиев**. Мы хотим задать столбцу **CategoryID** значение только для чтения. Значение свойства **CategoryID** создается базой данных после сохранения данных.
    -   Щелкните правой кнопкой мыши элемент управления DataGridView и выберите пункт Изменить столбцы...
    -   Выберите столбец CategoryId и установите для параметра ReadOnly значение true.
    -   Нажмите кнопку ОК.
-   Выберите продукты из источника данных Категория и перетащите его в форму. Продуктдатагридвиев и Продуктбиндингсаурце добавляются в форму.
-   Измените столбцы в Продуктдатагридвиев. Мы хотим скрыть столбцы CategoryId и Category и задать для свойства ProductId значение только для чтения. Значение свойства ProductId создается базой данных после сохранения данных.
    -   Щелкните правой кнопкой мыши элемент управления DataGridView и выберите пункт **изменить столбцы...** .
    -   Выберите столбец **ProductID** и установите для параметра **ReadOnly** значение **true**.
    -   Выберите столбец **CategoryID** и нажмите кнопку **Удалить** . Сделайте то же самое с столбцом **Category** .
    -   Нажмите кнопку **ОК**.

    До сих пор мы собрали элементы управления DataGridView с компонентами BindingSource в конструкторе. В следующем разделе мы добавим код в код программной части, чтобы задать Категорибиндингсаурце. DataSource для коллекции сущностей, которые в настоящее время отправляются DbContext. При перемещении и удалении продуктов из категории в WinForms требовалось настроить свойство Продуктсбиндингсаурце. DataSource для Категорибиндингсаурце и свойства Продуктсбиндингсаурце. DataMember на Products. Из-за этой привязки в Продуктдатагридвиев будут отображаться только продукты, принадлежащие к выбранной категории.
-   Включите кнопку **сохранить** на панели инструментов навигации, щелкнув правой кнопкой мыши и выбрав пункт **включено**.

    ![Конструктор форм 1](~/ef6/media/form1-designer.png)

-   Добавьте обработчик событий для кнопки Сохранить, дважды щелкнув кнопку. Это приведет к добавлению обработчика событий и переводу его в код программной части для формы. В следующем разделе будет добавлен код обработчика событий **категорибиндингнавигаторсавеитем\_Click** .

## <a name="add-the-code-that-handles-data-interaction"></a>Добавление кода, обрабатывающего взаимодействие с данными

Теперь мы добавим код для использования Продуктконтекст для доступа к данным. Обновите код для главного окна формы, как показано ниже.

Код объявляет долго выполняющийся экземпляр Продуктконтекст. Объект Продуктконтекст используется для запроса и сохранения данных в базе данных. Затем метод Dispose () в экземпляре Продуктконтекст вызывается из переопределенного метода onclosinging. Комментарии к коду содержат сведения о том, что делает код.

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

## <a name="test-the-windows-forms-application"></a>Тестирование приложения Windows Forms

-   Скомпилируйте и запустите приложение, и вы можете протестировать функциональные возможности.

    ![Форма 1 перед сохранением](~/ef6/media/form1beforesave.png)

-   После сохранения созданные хранилища ключи отобразятся на экране.

    ![Форма 1 после сохранения](~/ef6/media/form1aftersave.png)

-   Если вы использовали Code First, вы также увидите, что создана база данных **винформсвисефсампле. продуктконтекст** .

    ![Обозреватель объектов сервера](~/ef6/media/serverobjexplorer.png)
