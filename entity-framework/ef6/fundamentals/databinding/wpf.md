---
title: Привязка данных с помощью WPF - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 1933988277d3be8fecc02fced3293f2b7f80c901
ms.sourcegitcommit: ae399f9f3d1bae2c446b552247bd3af3ca5a2cf9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48575669"
---
# <a name="databinding-with-wpf"></a>Привязка данных с помощью WPF
Это пошаговое руководство показывает, как выполнить привязку к элементам управления WPF в форме «основной / подробности» типов POCO. Приложение использует API-интерфейсов Entity Framework для заполнения объектов с данными из базы данных, отслеживать изменения и сохранения данных в базе данных.

Модель определяет два типа, участвующие в связи один ко многим: **категории** (основной\\master) и **продукта** (зависимые\\подробно). Затем в Visual Studio tools используются для привязки типов, определенных в модели, чтобы элементы управления WPF. Инфраструктура привязки данных WPF позволяет осуществлять навигацию между связанными объектами: Выбор строк в главное представление приводит представлении «Подробности» для обновления с соответствующих дочерних данных.

Снимки экрана и примеры кода в этом пошаговом руководстве, взяты из Visual Studio 2013, но можно выполнить в этом пошаговом руководстве с помощью Visual Studio 2012 или Visual Studio 2010.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>Используйте параметр «Object» для создания источников данных в WPF

С помощью предыдущей версии Entity Framework мы использовали рекомендуем использовать **базы данных** параметр при создании нового источника данных на основе модели создан с помощью конструктора EF. Это было, так как конструктор создаст контекст, производный от ObjectContext и классов сущностей, производные от класса EntityObject. С помощью параметра базы данных поможет вам создавать лучший код для взаимодействия с этой области API.

Конструкторы для Visual Studio 2012 и Visual Studio 2013 EF создать контекст, который является производным от DbContext вместе с простых классов сущностей POCO. С помощью Visual Studio 2010, мы рекомендуем обращений шаблон генерации кода, использующего DbContext, как описано далее в этом пошаговом руководстве.

При использовании рабочей области DbContext API следует использовать **объект** параметр при создании нового источника данных, как показано в этом пошаговом руководстве.

При необходимости вы можете [вернуться к создание кода на основе ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) для моделей, созданных с помощью конструктора EF.

## <a name="pre-requisites"></a>Предварительные требования

Вам потребуется Visual Studio 2013, Visual Studio 2012 или Visual Studio 2010 установлен для выполнения этого пошагового руководства.

Если вы используете Visual Studio 2010, необходимо также установить NuGet. Дополнительные сведения см. в разделе [Установка NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).  

## <a name="create-the-application"></a>Создание приложения

-   Открытие Visual Studio
-   **Файл —&gt; Новинка —&gt; проекта...**
-   Выберите **Windows** в левой области и **WPFApplication** в области справа
-   Введите **WPFwithEFSample** как имя
-   Нажмите кнопку **ОК**

## <a name="install-the-entity-framework-nuget-package"></a>Установите пакет Entity Framework NuGet

-   В обозревателе решений щелкните правой кнопкой мыши **WinFormswithEFSample** проекта
-   Выберите **управление пакетами NuGet...**
-   В диалоговом окне «Управление пакетами NuGet» выберите **Online** вкладку и выберите **EntityFramework** пакета
-   Нажмите кнопку **установки**  
    >[!NOTE]
    > Помимо сборки EntityFramework также добавляется ссылка System.ComponentModel.DataAnnotations. Если проект содержит ссылку на System.Data.Entity, затем он будет удален при установке EntityFramework. Сборка System.Data.Entity больше не используется для приложений Entity Framework 6.

## <a name="define-a-model"></a>Определение модели

В этом пошаговом руководстве, вы можете решил реализовать модель с помощью Code First или конструкторе EF. Выполните одно из двух следующих разделах.

### <a name="option-1-define-a-model-using-code-first"></a>Вариант 1: Определение модели с помощью Code First

В этом разделе показано, как создать модель и его связанные базы данных, с помощью Code First. Перейдите к следующему разделу (**вариант 2: определение модели с помощью Database First)** Если вы предпочитаете использовать Database First обратить реконструирование модели из базы данных в конструкторе EF

При использовании шаблона разработки Code First обычно начинается с написания классов .NET Framework, которые определяют модель концептуальный (домен).

-   Добавьте новый класс к **WPFwithEFSample:**
    -   Щелкните правой кнопкой мыши имя проекта
    -   Выберите **добавить**, затем **новый элемент**
    -   Выберите **класс** и введите **продукта** имени класса
-   Замените **продукта** класса определения следующим кодом:

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

**Продуктов** свойство **категории** класс и **категории** свойство **продукта** класс — это свойства навигации. В Entity Framework свойства навигации позволяют для перехода по связи между двумя типами сущностей.

Кроме определения сущностей, необходимо определить класс, который является производным от DbContext и DbSet предоставляет&lt;TEntity&gt; свойства. DbSet&lt;TEntity&gt; свойства let контекст знать, какие типы, которые вы хотите включить в модель.

Экземпляр типа DbContext производным управляет объектов сущности во время выполнения, который включает заполнение объектов с данными из базы данных, изменение отслеживания и сохранения данных в базу данных.

-   Добавьте новый **ProductContext** класс в проект со следующим определением:

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

    ![Изменение источника данных](~/ef6/media/changedatasource.png)

-   Подключение к LocalDB или SQL Express, в зависимости от того, какой из них вы установили и введите **продуктов** имя базы данных

    ![Добавить подключение LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Добавление подключения Express](~/ef6/media/addconnectionexpress.png)

-   Выберите **ОК** и вам нужно будет Если вы хотите создать новую базу данных, выберите **Да**

    ![Создание базы данных](~/ef6/media/createdatabase.png)

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

    ![Выбор содержимого модели](~/ef6/media/choosemodelcontents.png)

-   Выберите соединение с базой данных, созданной в первом разделе, введите **ProductContext** как имя строки подключения и нажмите кнопку **Далее**

    ![Выберите соединение](~/ef6/media/chooseyourconnection.png)

-   Установите флажок рядом с «Таблицы», чтобы импортировать все таблицы и нажмите кнопку «Готово»

    ![Выбор объектов](~/ef6/media/chooseyourobjects.png)

После завершения процесса реконструирования новой модели добавлен в проект и открывается для просмотра в конструкторе Entity Framework. Файл App.config также был добавлен в проект со сведениями о подключении для базы данных.

#### <a name="additional-steps-in-visual-studio-2010"></a>Дополнительные действия в Visual Studio 2010

Если вы работаете в Visual Studio 2010 будет необходимо обновить конструктор EF будет использовать создание кода EF6.

-   Щелкните правой кнопкой мыши пустое место модели в конструкторе EF и выберите **добавить элемент формирования кода...**
-   Выберите **шаблоны в Интернете** из меню слева и выполните поиск **DbContext**
-   Выберите **EF 6.x генератор DbContext для C\#,** введите **ProductsModel** как имя и щелкните "Добавить"

#### <a name="updating-code-generation-for-data-binding"></a>Обновление создание кода для привязки данных

EF создает код из модели с помощью шаблонов T4. Шаблоны, поставляемых с Visual Studio или скачать из коллекции Visual Studio предназначены для использования общего назначения. Это означает, что сущности, созданные на основе этих шаблонов имеют простой ICollection&lt;T&gt; свойства. Тем не менее, при этом данные привязки с использованием WPF желательно использовать **ObservableCollection** для свойств коллекции, так что WPF можно отслеживать, изменения, внесенные в коллекции. Для этого нужно будет изменить шаблоны для использования ObservableCollection.

-   Откройте **обозревателе решений** и найти **ProductModel.edmx** файла
-   Найти **ProductModel.tt** файл, который будет вложен в файле ProductModel.edmx

    ![Шаблон модели продукта в WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   Дважды щелкните файл ProductModel.tt, чтобы открыть его в редакторе Visual Studio
-   Найти и заменить два вхождения "**ICollection**«with»**ObservableCollection**«. Они находятся приблизительно в строках 296 и 484.
-   Найти и заменить первое вхождение "**HashSet**«with»**ObservableCollection**«. Это событие находится примерно в строке 50. **Не** замените второе вхождение HashSet Найти далее в коде.
-   Поиск и замена только вхождения "**System.Collections.Generic**«with»**System.Collections.ObjectModel**«. Это примерно в строке 424.
-   Сохраните файл ProductModel.tt. Это может вызвать код для сущностей, быть создан повторно. Если код не создается автоматически, затем щелкнуть правой кнопкой мыши ProductModel.tt и выберите «Запустить пользовательский инструмент».

Если вы теперь откройте файл Category.cs (который вложен в ProductModel.tt), то вы увидите, что продукты коллекция имеет тип **ObservableCollection&lt;продукта&gt;**.

Скомпилируйте проект.

## <a name="lazy-loading"></a>Отложенная загрузка

**Продуктов** свойство **категории** класс и **категории** свойство **продукта** класс — это свойства навигации. В Entity Framework свойства навигации позволяют для перехода по связи между двумя типами сущностей.

EF дает возможность загрузки связанных сущностей из базы данных автоматически при первом обращении к свойству навигации. С помощью этого типа загрузки (называется отложенной загрузки) Имейте в виду, что при первом доступе к каждому свойству навигации отдельный запрос будет выполнен в базе данных, если содержимое еще не в контексте.

При использовании типов сущностей POCO, EF обеспечивает отложенную загрузку, создании экземпляров производного прокси-типов во время выполнения и затем переопределение виртуальных свойств в классах для добавления обработчика загрузки. Чтобы получить отложенная загрузка связанных объектов, необходимо объявить методы получения свойств как навигации **открытый** и **виртуального** (**Overridable** в Visual Basic), и вы класс не должен быть **запечатанный** (**NotOverridable** в Visual Basic). Когда базы данных с помощью первого свойства навигации, автоматически становятся виртуальные, чтобы включить отложенную загрузку. В разделе Code First, мы решили сделать виртуальные свойства навигации по той же причине

## <a name="bind-object-to-controls"></a>Привязка объектов к элементам управления

Добавьте классы, которые определены в модели в качестве источников данных для этого приложения WPF.

-   Дважды щелкните **MainWindow.xaml** в обозревателе решений, чтобы открыть главную форму
-   В главном меню выберите **проект -&gt; добавить новый источник данных...**
    (в Visual Studio 2010, необходимо выбрать **тарифный план —&gt; добавить новый источник данных...** )
-   В выберите Typewindow источника данных, выберите **объект** и нажмите кнопку **Далее**
-   В диалоговом окне объекты данных выберите unfold **WPFwithEFSample** двух раз и выберите **категории**  
    *Нет необходимости, чтобы выбрать **продукта** источника данных, так как мы вернемся к нему через **продукта**свойство **категории** источника данных*  

    ![Выбор объектов данных](~/ef6/media/selectdataobjects.png)

-   Нажмите кнопку **Готово.**
-   Открывается окно "Источники данных" рядом с окном MainWindow.xaml *Если окно "Источники данных", не отображается, выберите **представление —&gt; Other Windows -&gt; источников данных***
-   Скрыть значок ПИН-код, поэтому окна источников данных автоматически. Может потребоваться нажмите кнопку "Обновить", если окно уже было открыто.

    ![Data Sources](~/ef6/media/datasources.png)

-   Выберите **категории** источника данных и перетащите его на форме.

Когда мы перетащили этот источник произошло следующее:

-   **CategoryViewSource** ресурсов и **categoryDataGrid** управления были добавлены в XAML 
-   Свойство DataContext для родительского элемента сетки установлено в «{StaticResource **categoryViewSource** }». **CategoryViewSource** ресурсов выступает в качестве источника привязки для внешнего\\родительского элемента сетки. Внутренние элементы сетки, наследуют значение DataContext от родительского элемента сетки (свойство ItemsSource categoryDataGrid имеет значение «{Binding}»)

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

## <a name="adding-a-details-grid"></a>Добавление сетки сведений

Теперь, когда у нас есть сетки для отображения категорий, давайте добавьте сведения о сетке для отображения соответствующих продуктов.

-   Выберите **продуктов** свойства в разделе **категории** источника данных и перетащите его на форме.
    -   **CategoryProductsViewSource** ресурсов и **productDataGrid** сетки добавляются в XAML
    -   Путь привязки для этого ресурса будет присвоено продуктов
    -   Инфраструктура привязки данных WPF гарантирует, что только продукты, относящиеся к выбранной категории будут отображаться в **productDataGrid**
-   Из панели элементов перетащите **кнопку** в форму. Задайте **имя** свойства **buttonSave** и **содержимого** свойства **Сохранить**.

Форма должна выглядеть примерно так:

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>Добавьте код, который обрабатывает взаимодействие с данными

Пришло время добавить некоторые обработчики событий в главное окно.

-   В окне XAML, щелкните  **&lt;окно** элемент, Выбор главного окна
-   В **свойства** выберите окно **события** в правом верхнем углу, затем дважды щелкните текстовое поле справа от **Loaded** метки

    ![Главное окно свойств](~/ef6/media/mainwindowproperties.png)

-   Также добавьте **щелкните** событие для **Сохранить** кнопки, дважды щелкнув "Сохранить" в конструкторе. 

Откроется кода программной части формы, теперь мы отредактируем код, чтобы использовать ProductContext производить доступ к данным. Обновите код для MainWindow, как показано ниже.

Код объявляет экземпляр длительных **ProductContext**. **ProductContext** объект используется для запроса и сохранения данных в базу данных. **Dispose()** на **ProductContext** экземпляра вызывается из переопределенного **OnClosing** метод. В комментариях к коду содержат сведения о что делает код.

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

## <a name="test-the-wpf-application"></a>Тестирование приложения WPF

-   Скомпилируйте и запустите приложение. Если вы использовали Code First, то вы увидите, что **WPFwithEFSample.ProductContext** база данных создается автоматически.
-   Введите имя категории в верхней сетке и продукта имена в нижней таблице *ничего не вводится в столбцов ID, так как первичный ключ создается в базе данных*

    ![Главное окно с помощью новых категорий и продуктов](~/ef6/media/screen1.png)

-   Нажмите клавишу **Сохранить** кнопку, чтобы сохранить данные в базе данных

После вызова в DbContext **SaveChanges()**, идентификаторы заполняются значениями сформированный базой данных. Так как мы называли **Refresh()** после **SaveChanges()** **DataGrid** элементы управления обновляются с использованием новых значений.

![Главное окно с идентификаторами заполнен](~/ef6/media/screen2.png)

## <a name="additional-resources"></a>Дополнительные ресурсы

Дополнительные сведения о привязке данных в коллекции с помощью WPF, см. в разделе [в этом разделе](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) в документации по WPF.  
