---
title: Привязка данных с помощью WinForms - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 7ceb8e85fe3d8f5ab9a5e58ef9c84599585d8f77
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994533"
---
# <a name="databinding-with-winforms"></a>Привязка данных с помощью WinForms
Это пошаговое руководство показывает, как выполнить привязку к элементам управления Windows Forms (WinForms) в форме «основной / подробности» типов POCO. Приложение использует Entity Framework для заполнения объектов с данными из базы данных, отслеживать изменения и сохранения данных в базе данных.

Модель определяет два типа, участвующие в связи один ко многим: категория (основной\\master) и продукта (зависимые\\подробно). Средства Visual Studio, затем используются для привязки типов, определенных в модели для элементов управления WinForms. Платформа привязки данных WinForms позволяет осуществлять переходы между связанными объектами: Выбор строк в главное представление приводит представлении «Подробности» для обновления с соответствующих дочерних данных.

Снимки экрана и примеры кода в этом пошаговом руководстве, взяты из Visual Studio 2013, но можно выполнить в этом пошаговом руководстве с помощью Visual Studio 2012 или Visual Studio 2010.

## <a name="pre-requisites"></a>Предварительные требования

Вам потребуется Visual Studio 2013, Visual Studio 2012 или Visual Studio 2010 установлен для выполнения этого пошагового руководства.

Если вы используете Visual Studio 2010, необходимо также установить NuGet. Дополнительные сведения см. в разделе [Установка NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).

## <a name="create-the-application"></a>Создание приложения

-   Открытие Visual Studio
-   **Файл —&gt; Новинка —&gt; проекта...**
-   Выберите **Windows** в левой области и **Windows FormsApplication** в области справа
-   Введите **WinFormswithEFSample** как имя
-   Нажмите кнопку **ОК**

## <a name="install-the-entity-framework-nuget-package"></a>Установите пакет Entity Framework NuGet

-   В обозревателе решений щелкните правой кнопкой мыши **WinFormswithEFSample** проекта
-   Выберите **управление пакетами NuGet...**
-   В диалоговом окне «Управление пакетами NuGet» выберите **Online** вкладку и выберите **EntityFramework** пакета
-   Нажмите кнопку **установки**  
    > [!NOTE]
    > Помимо сборки EntityFramework также добавляется ссылка System.ComponentModel.DataAnnotations. Если проект содержит ссылку на System.Data.Entity, затем он будет удален при установке EntityFramework. Сборка System.Data.Entity больше не используется для приложений Entity Framework 6.

## <a name="implementing-ilistsource-for-collections"></a>Реализация IListSource для коллекций

Свойства коллекции необходимо реализовать интерфейс IListSource, чтобы обеспечить двухстороннюю привязку данных с сортировкой, при использовании Windows Forms. Для этого мы хотим расширить ObservableCollection Добавление IListSource функциональных возможностей.

-   Добавить **ObservableListSource** класс в проект:
    -   Щелкните правой кнопкой мыши имя проекта
    -   Выберите **Add -&gt; новый элемент**
    -   Выберите **класс** и введите **ObservableListSource** имени класса
-   Замените код, созданный по умолчанию следующим кодом:

*Этот класс позволяет Двухсторонняя привязка, а также для сортировки. Класс является производным от ObservableCollection&lt;T&gt; и добавляет явной реализации IListSource. Метод встречает IListSource реализуется для возврата IBindingList реализация, которая остается синхронизированной с ObservableCollection. Реализация IBindingList, создаваемые ToBindingList поддерживает сортировку. Метод расширения ToBindingList определен в сборке EntityFramework.*

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

## <a name="define-a-model"></a>Определение модели

В этом пошаговом руководстве, вы можете решил реализовать модель с помощью Code First или конструкторе EF. Выполните одно из двух следующих разделах.

### <a name="option-1-define-a-model-using-code-first"></a>Вариант 1: Определение модели с помощью Code First

В этом разделе показано, как создать модель и его связанные базы данных, с помощью Code First. Перейдите к следующему разделу (**вариант 2: определение модели с помощью Database First)** Если вы предпочитаете использовать Database First обратить реконструирование модели из базы данных в конструкторе EF

При использовании шаблона разработки Code First обычно начинается с написания классов .NET Framework, которые определяют модель концептуальный (домен).

-   Добавьте новый **продукта** класс в проект
-   Замените код, созданный по умолчанию следующим кодом:

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

-   Добавить **категории** класс в проект.
-   Замените код, созданный по умолчанию следующим кодом:

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

Кроме определения сущностей, необходимо определить класс, производный от **DbContext** и предоставляет **DbSet&lt;TEntity&gt;**  свойства. **DbSet** свойства let контекст знать, какие типы, которые вы хотите включить в модель. **DbContext** и **DbSet** определенные типы в сборке EntityFramework.

Экземпляр типа DbContext производным управляет объектов сущности во время выполнения, который включает заполнение объектов с данными из базы данных, изменение отслеживания и сохранения данных в базу данных.

-   Добавьте новый **ProductContext** класс в проект.
-   Замените код, созданный по умолчанию следующим кодом:

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

### <a name="option-2-define-a-model-using-database-first"></a>Вариант 2: Определение модели с помощью Database First

В этом разделе демонстрируется использование Database First чтобы реконструировать модель из базы данных в конструкторе EF. Если действия в предыдущем разделе (**вариант 1: определение модели с помощью Code First)**, пропустите этот раздел и перейти непосредственно к **отложенной загрузки** раздел.

#### <a name="create-an-existing-database"></a>Создание базы данных

Обычно при ориентировании существующей базы данных, он будет уже создан, но для этого пошагового руководства необходимо создать базу данных для доступа к.

Сервер базы данных, который устанавливается вместе с Visual Studio отличается в зависимости от версии Visual Studio, вы установили:

-   Если вы используете Visual Studio 2010 вы создадите базу данных SQL Express.
-   Если вы используете Visual Studio 2012, а затем вы создадите [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) базы данных.

Перейдем дальше и создать базу данных.

-   **Представление —&gt; обозревателя серверов**
-   Щелкните правой кнопкой мыши **подключения к данным -&gt; добавить соединение...**
-   Если вы не подключились к базе данных с помощью обозревателя сервера прежде, чем вам потребуется выбрать в качестве источника данных Microsoft SQL Server

    ![ChangeDataSource](~/ef6/media/changedatasource.png)

-   Подключение к LocalDB или SQL Express, в зависимости от того, какой из них вы установили и введите **продуктов** имя базы данных

    ![AddConnectionLocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![AddConnectionExpress](~/ef6/media/addconnectionexpress.png)

-   Выберите **ОК** и вам нужно будет Если вы хотите создать новую базу данных, выберите **Да**

    ![CreateDatabase](~/ef6/media/createdatabase.png)

-   Новой базы данных будут отображаться в обозревателе сервера щелкните его правой кнопкой мыши и выберите **новый запрос**
-   Скопируйте следующий запрос SQL в новый запрос, а затем щелкните правой кнопкой мыши запрос и выберите **Execute**

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

#### <a name="reverse-engineer-model"></a>Реконструирование модели

Мы собираемся использовать Entity Framework Designer, который входит в состав Visual Studio, для создания нашей модели.

-   **Проект -&gt; добавить новый элемент...**
-   Выберите **данных** в меню слева и затем **модель EDM ADO.NET**
-   Введите **ProductModel** имя и нажмите кнопку **ОК**
-   Это откроет **мастер моделей EDM**
-   Выберите **создать из базы данных** и нажмите кнопку **Далее**

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   Выберите соединение с базой данных, созданной в первом разделе, введите **ProductContext** как имя строки подключения и нажмите кнопку **Далее**

    ![ChooseYourConnection](~/ef6/media/chooseyourconnection.png)

-   Установите флажок рядом с «Таблицы», чтобы импортировать все таблицы и нажмите кнопку «Готово»

    ![ChooseYourObjects](~/ef6/media/chooseyourobjects.png)

После завершения процесса реконструирования новой модели добавлен в проект и открывается для просмотра в конструкторе Entity Framework. Файл App.config также был добавлен в проект со сведениями о подключении для базы данных.

#### <a name="additional-steps-in-visual-studio-2010"></a>Дополнительные действия в Visual Studio 2010

Если вы работаете в Visual Studio 2010 будет необходимо обновить конструктор EF будет использовать создание кода EF6.

-   Щелкните правой кнопкой мыши пустое место модели в конструкторе EF и выберите **добавить элемент формирования кода...**
-   Выберите **шаблоны в Интернете** из меню слева и выполните поиск **DbContext**
-   Выберите **EF 6.x генератор DbContext для C\#,** введите **ProductsModel** как имя и щелкните "Добавить"

#### <a name="updating-code-generation-for-data-binding"></a>Обновление создание кода для привязки данных

EF создает код из модели с помощью шаблонов T4. Шаблоны, поставляемых с Visual Studio или скачать из коллекции Visual Studio предназначены для использования общего назначения. Это означает, что сущности, созданные на основе этих шаблонов имеют простой ICollection&lt;T&gt; свойства. Тем не менее при выполнении привязки данных желательно иметь свойства коллекции, которые реализуют IListSource. Именно поэтому мы создали класс ObservableListSource выше, и теперь мы собираемся изменить шаблоны, чтобы сделать использование этого класса.

-   Откройте **обозревателе решений** и найти **ProductModel.edmx** файла
-   Найти **ProductModel.tt** файл, который будет вложен в файле ProductModel.edmx

    ![ProductModelTemplate](~/ef6/media/productmodeltemplate.png)

-   Дважды щелкните файл ProductModel.tt, чтобы открыть его в редакторе Visual Studio
-   Найти и заменить два вхождения "**ICollection**«with»**ObservableListSource**«. Они находятся в около строки 296 и 484.
-   Найти и заменить первое вхождение "**HashSet**«with»**ObservableListSource**«. Это событие находится примерно в строке 50. **Не** замените второе вхождение HashSet Найти далее в коде.
-   Сохраните файл ProductModel.tt. Это может вызвать код для сущностей, быть создан повторно. Если код не создается автоматически, затем щелкнуть правой кнопкой мыши ProductModel.tt и выберите «Запустить пользовательский инструмент».

Если вы теперь откройте файл Category.cs (который вложен в ProductModel.tt), то вы увидите, что продукты коллекция имеет тип **ObservableListSource&lt;продукта&gt;**.

Скомпилируйте проект.

## <a name="lazy-loading"></a>Отложенная загрузка

**Продуктов** свойство **категории** класс и **категории** свойство **продукта** класс — это свойства навигации. В Entity Framework свойства навигации позволяют для перехода по связи между двумя типами сущностей.

EF дает возможность загрузки связанных сущностей из базы данных автоматически при первом обращении к свойству навигации. С помощью этого типа загрузки (называется отложенной загрузки) Имейте в виду, что при первом доступе к каждому свойству навигации отдельный запрос будет выполнен в базе данных, если содержимое еще не в контексте.

При использовании типов сущностей POCO, EF обеспечивает отложенную загрузку, создании экземпляров производного прокси-типов во время выполнения и затем переопределение виртуальных свойств в классах для добавления обработчика загрузки. Чтобы получить отложенная загрузка связанных объектов, необходимо объявить методы получения свойств как навигации **открытый** и **виртуального** (**Overridable** в Visual Basic), и вы класс не должен быть **запечатанный** (**NotOverridable** в Visual Basic). Когда базы данных с помощью первого свойства навигации, автоматически становятся виртуальные, чтобы включить отложенную загрузку. В разделе Code First, мы решили сделать виртуальные свойства навигации по той же причине

## <a name="bind-object-to-controls"></a>Привязка объектов к элементам управления

Добавьте классы, которые определены в модели в качестве источников данных для этого приложения WinForms.

-   В главном меню выберите **проект -&gt; добавить новый источник данных...**
    (в Visual Studio 2010, необходимо выбрать **тарифный план —&gt; добавить новый источник данных...** )
-   В списке выберите тип источника данных окна выберите **объект** и нажмите кнопку **Далее**
-   В диалоговом окне объекты данных выберите unfold **WinFormswithEFSample** двух раз и выберите **категории** нет необходимости для выбора источника данных Product, так как мы вернемся к нему через продукта свойство в источнике данных категории.

    ![DataSource](~/ef6/media/datasource.png)

-   Нажмите кнопку **Готово.** 
     *Если окно "Источники данных", не отображается, выберите *** представление —&gt; Other Windows -&gt; источников данных**
-   Скрыть значок ПИН-код, поэтому окна источников данных автоматически. Может потребоваться нажмите кнопку "Обновить", если окно уже было открыто.

    ![DataSource2](~/ef6/media/datasource2.png)

-   В обозревателе решений дважды щелкните **Form1.cs** файл, чтобы открыть главную форму в конструкторе.
-   Выберите **категории** источника данных и перетащите его на форме. По умолчанию новый DataGridView (**categoryDataGridView**) и элементы управления панели инструментов навигации добавляются в конструктор. Эти элементы управления привязаны к BindingSource (**categoryBindingSource**) и привязка Навигатор (**categoryBindingNavigator**) компонентов, которые также будут созданы.
-   Изменить столбцы, на **categoryDataGridView**. Нам нужно выбрать **CategoryId** столбец только для чтения. Значение для **CategoryId** после сохранения данных в базе данных создается свойство.
    -   Щелкните правой кнопкой мыши элемента управления DataGridView и выберите столбцы, изменить...
    -   Выберите столбец CategoryId и присвоено значение True только для чтения
    -   Нажмите кнопку ОК
-   Выберите продукты из категории источника данных и перетащите его на форме. ProductDataGridView и productBindingSource добавляются в форму.
-   Измените столбцы, на productDataGridView. Нам нужно скрыть столбцы CategoryId и категории и значение ProductId только для чтения. Значение свойства ProductId создается в базе данных после сохранения данных.
    -   Щелкните правой кнопкой мыши элемента управления DataGridView и выберите **Правка столбцов...** .
    -   Выберите **ProductId** столбцы и наборы **ReadOnly** для **True**.
    -   Выберите **CategoryId** столбец и нажмите клавишу **удалить** кнопки. Сделайте то же самое с **категории** столбца.
    -   Нажмите клавишу **ОК**.

    На данный момент мы связанный элемент управления DataGridView с компонентами BindingSource в конструкторе. В следующем разделе будет добавлен код для кода программной части присвоено categoryBindingSource.DataSource коллекцию сущностей, которые в настоящее время отслеживаются с DbContext. При мы перетащить и удалении продукты из категории, WinForms занималась Настройка свойство productsBindingSource.DataSource свойству categoryBindingSource и productsBindingSource.DataMember к продуктам. Из-за этой привязкой, только продукты, принадлежащие к текущей выбранной категории будет отображаться в productDataGridView.
-   Включить **Сохранить** кнопку на панели навигации, щелкнув правой кнопкой мыши и выбрав **включено**.

    ![Конструктор Form1](~/ef6/media/form1-designer.png)

-   Добавьте обработчик событий для сохранения кнопки, дважды щелкнув кнопку. Это будет добавьте обработчик событий и вы хотели кода программной части формы. Код для **categoryBindingNavigatorSaveItem\_щелкните** обработчик событий будет добавлена в следующем разделе.

## <a name="add-the-code-that-handles-data-interaction"></a>Добавьте код, который обрабатывает взаимодействие с данными

Теперь мы добавим код, чтобы использовать ProductContext производить доступ к данным. Обновите код для окна главной формы, как показано ниже.

Код объявляет экземпляр ProductContext выполняющейся длительное время. Объект ProductContext позволяет запрашивать и сохранять данные в базу данных. Метод Dispose() в экземпляре ProductContext затем вызывается из переопределенного метода OnClosing. В комментариях к коду содержат сведения о что делает код.

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

-   Компиляции и выполнения приложения и вы можете проверить работу функции.

    ![Form1BeforeSave](~/ef6/media/form1beforesave.png)

-   После сохранения хранения созданных ключей отображаются на экране.

    ![Form1AfterSave](~/ef6/media/form1aftersave.png)

-   Если вы использовали Code First, то вы также увидите, что **WinFormswithEFSample.ProductContext** база данных создается автоматически.

    ![ServerObjExplorer](~/ef6/media/serverobjexplorer.png)
