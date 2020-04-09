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
> **Этот документ действителен только для WPF в рамках .NET**
>
> В этом документе описаны данные, связывающие для WPF в рамках .NET. Для новых проектов .NET Core мы рекомендуем использовать [EF Core](/ef/core) вместо Entity Framework 6. Документация для связывания данных в EF Core отслеживается в [выпуске #778.](https://github.com/dotnet/EntityFramework.Docs/issues/778)

# <a name="databinding-with-wpf"></a>Привязка данных с помощью WPF
Это шаг за шагом пошагонасто показывает, как связать типы POCO с элементами управления WPF в форме "мастер-детали". Приложение использует ApIs Entity Framework для заполнения объектов данными из базы данных, отслеживания изменений и сохранения данных в базе данных.

Модель определяет два типа, которые участвуют в отношениях\\от одного к\\многим: **Категория** (основной мастер) и **Продукт** (зависимая деталь). Затем инструменты Visual Studio используются для привязки типов, определенных в модели, к элементам управления WPF. Платформа wPF, связывающая данные, позволяет осуществлять навигацию между связанными объектами: выбор строк в главном представлении приводит к обновлению представления деталей с соответствующими данными о добилище.

Скриншоты и код списки в этом пошаговое покадрово взяты из Visual Studio 2013, но вы можете завершить это пошаговое решение с Visual Studio 2012 или Visual Studio 2010.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>Используйте опцию «Объект» для создания источников данных WPF

В предыдущей версии Entity Framework мы рекомендуем использовать опцию **базы данных** при создании нового источника данных на основе модели, созданной с EF Designer. Это произошло потому, что конструктор генерирует контекст, полученный из классов ObjectContext и сущности, полученных из EntityObject. Использование опции Database поможет вам написать лучший код для взаимодействия с этой поверхностью API.

ДИЗАЙНЕРы EF для Visual Studio 2012 и Visual Studio 2013 создают контекст, который вытекает из DbContext вместе с простыми классами сущности POCO. С Visual Studio 2010 мы рекомендуем поменять на шаблон генерации кода, который использует DbContext, как описано позже в этом пошаговочном.

При использовании поверхности API DbContext при создании нового источника данных следует использовать опцию **«Объект»,** как показано в этом пошаговом слове.

При необходимости можно [вернуться к созданию кода](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) на основе ObjectContext для моделей, созданных с помощью EF Designer.

## <a name="pre-requisites"></a>Предварительные требования

Вы должны иметь Visual Studio 2013, Visual Studio 2012 или Visual Studio 2010 установлен для завершения этого пошагового покистраза.

Если вы используете Visual Studio 2010, вы также должны установить NuGet. Для получения дополнительной информации [см.](https://docs.microsoft.com/nuget/install-nuget-client-tools)  

## <a name="create-the-application"></a>Создание приложения

-   Запустите Visual Studio
-   **Файл&gt; -&gt; Новый - Проект....**
-   Выберите **Windows** в левом стеку и **WPFApplication** в правом стене
-   Введите **WPFwithEFSample** как имя
-   Выберите **OK**

## <a name="install-the-entity-framework-nuget-package"></a>Установка пакета Сущность Framework NuGet

-   В Solution Explorer нажмите правой кнопкой мыши на проекте **WinFormswithEFSample**
-   Выберите **Пакеты Управления NuGet...**
-   В диалоге пакетов Управления NuGet выберите вкладку **Online** и выберите пакет **EntityFramework**
-   Нажмите **Установить**  
    >[!NOTE]
    > В дополнение к сборке EntityFramework также добавлена ссылка на System.ComponentModel.DataAnnotations. Если в проекте есть ссылка на System.Data.Entity, то он будет удален при установке пакета EntityFramework. Сборка System.Data.Entity больше не используется для приложений Entity Framework 6.

## <a name="define-a-model"></a>Определение модели

В этом пошаговом шаге вы можете реализовать модель с помощью Code First или EF Designer. Заполните один из двух следующих разделов.

### <a name="option-1-define-a-model-using-code-first"></a>Вариант 1: Определите модель с помощью Code First

В этом разделе показано, как создать модель и связанную с ней базу данных с помощью Code First. Перейти к следующему разделу **(Вариант 2: Определите модель с помощью базы данных First),** если вы предпочитаете использовать Database First для обратного проектирования модели из базы данных с помощью конструктора EF

При использовании code First разработка обычно начинается с написания классов .NET Framework, которые определяют концептуальную (доменную) модель.

-   Добавьте новый класс в **WPFwithEFSample:**
    -   Нажмите справа на название проекта
    -   Выберите **Добавить,** затем **новый пункт**
    -   Выберите **класс** и введите **продукт** для имени класса
-   Замените определение класса **продукта** следующим кодом:

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

Свойство **Продуктов** **в категории** класса и **категории** в классе **Product** является навигационными свойствами. В рамках сущности свойства навигации обеспечивают способ навигации между двумя типами сущностей.

В дополнение к определению сущностей необходимо определить класс, который вытекает из&lt;DbContext и предоставляет свойства DbSet TEntity.&gt; Свойства DbSet&lt;&gt; TEntity позволяют контексту знать, какие типы вы хотите включить в модель.

Экземпляр типа двузаемых DbContext управляет объектами сущности во время выполнения, что включает в себя заполнение объектов данными из базы данных, отслеживание изменений и сохраняющиеся данные в базу данных.

-   Добавьте в проект новый класс **ProductContext** со следующим определением:

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

### <a name="option-2-define-a-model-using-database-first"></a>Вариант 2: Определите модель с помощью базы данных First

В этом разделе показано, как использовать Database First для обратного проектирования модели из базы данных с помощью конструктора EF. Если вы завершили предыдущий раздел **(Вариант 1: Определите модель с помощью Code First),** то пропустите этот раздел и перейдите прямо в раздел **Lazy Loading.**

#### <a name="create-an-existing-database"></a>Создание существующей базы данных

Обычно при таргетинге на существующую базу данных она уже будет создана, но для этого пошагового мы должны создать базу данных для доступа.

Сервер базы данных, установленный visual Studio, отличается в зависимости от установленной версии Visual Studio:

-   Если вы используете Visual Studio 2010, вы создаете базу данных S'L Express.
-   Если вы используете Visual Studio 2012, то вы будете создавать базу данных [LocalDB.](https://msdn.microsoft.com/library/hh510202.aspx)

Давайте идти вперед и генерировать базу данных.

-   **Просмотр&gt; - Исследователь серверов**
-   Нажмите правой кнопкой мыши на **подключение к данным -&gt; Добавить соединение...**
-   Если вы не подключились к базе данных от Server Explorer, прежде чем вам нужно будет выбрать сервер Microsoft S'L в качестве источника данных

    ![Изменение источника данных](~/ef6/media/changedatasource.png)

-   Подключитесь к LocalDB или S'L Express, в зависимости от того, какой из них вы установили, и введите **Продукты** в качестве имени базы данных

    ![Добавление connection LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Добавить соединение Экспресс](~/ef6/media/addconnectionexpress.png)

-   Выберите **OK,** и вас спросят, хотите ли вы создать новую базу данных, выберите **Yes**

    ![Создание базы данных](~/ef6/media/createdatabase.png)

-   Новая база данных теперь будет отображаться в Server Explorer, нажмите на нее правой кнопкой мыши и выберите **новый запрос**
-   Скопируйте следующий S'L в новый запрос, затем нажмите правой кнопкой мыши на запрос и выберите **Execute**

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

#### <a name="reverse-engineer-model"></a>Обратная модель инженера

Мы собираемся использовать Entity Framework Designer, который включен в состав Visual Studio, для создания нашей модели.

-   **Проект&gt; - Добавить новый пункт...**
-   Выберите **данные** из левого меню, а затем **ADO.NET модель данных entity**
-   Введите **ProductModel** как имя и нажмите **OK**
-   Это запускает **Entity Data Model Мастер**
-   Выберите **генерировать из базы данных** и нажмите **далее**

    ![Выбор содержимого модели](~/ef6/media/choosemodelcontents.png)

-   Выберите подключение к базе данных, созданной в первом разделе, введите **ProductContext** в качестве названия строки соединения и нажмите **Далее**

    ![Выберите соединение](~/ef6/media/chooseyourconnection.png)

-   Нажмите на флажок рядом с "Таблицы", чтобы импортировать все таблицы и нажмите кнопку "Финиш"

    ![Выберите объекты](~/ef6/media/chooseyourobjects.png)

Как только процесс обратного инженера завершает, новая модель добавляется в ваш проект и открывается для просмотра в проекте Entity Framework Designer. Файл App.config также был добавлен в ваш проект с сведениями о подключении к базе данных.

#### <a name="additional-steps-in-visual-studio-2010"></a>Дополнительные шаги в визуальной студии 2010

Если вы работаете в Visual Studio 2010, то вам нужно будет обновить EF-дизайнер, чтобы использовать генерацию кода EF6.

-   Нажмите правой кнопкой мыши на пустое место вашей модели в EF Designer и выберите **Добавить код поколения пункт ...**
-   Выберите **онлайн-шаблоны** из левого меню и поиск **DbContext**
-   Выберите **генератор EF 6.x DbContext для C,\#** введите **ProductsModel** в качестве имени и нажмите Добавить

#### <a name="updating-code-generation-for-data-binding"></a>Обновление генерации кода для привязки данных

EF генерирует код из модели с помощью шаблонов T4. Шаблоны, отправленные visual Studio или загруженные из галереи Visual Studio, предназначены для общего использования. Это означает, что сущности, генерируемые&lt;&gt; из этих шаблонов, имеют простые свойства ICollection T. Однако при использовании связывания данных с помощью WPF желательно использовать **ObservableCollection** для сбора свойств, чтобы WPF мог отслеживать изменения, внесенные в коллекции. С этой целью мы будем изменять шаблоны для использования ObservableCollection.

-   Откройте **Explorer решений** и найдите файл **ProductModel.edmx**
-   Найдите **ProductModel.tt** файл, который будет вложен под файл Омело.edmx

    ![Шаблон модели продукции WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   Дважды нажмите на файл ProductModel.tt, чтобы открыть его в редакторе Visual Studio
-   Найти и заменить два случая "**ICollection**" с "**ObservableCollection**". Они расположены примерно на линиях 296 и 484.
-   Найдите и замените первое появление «**HashSet**» на «**ObservableCollection**». Это явление расположено примерно на линии 50. **Не заменяйте** второе появление HashSet, найденное позже в коде.
-   Найти и заменить только появление "**System.Collections.Generic**" с "**System.Collections.ObjectModel**". Это расположено примерно на линии 424.
-   Сохранить файл ProductModel.tt. Это должно привести к регенерированию кода для сущностей. Если код не регенерируется автоматически, то нажмите правой кнопкой мыши на ProductModel.tt и выберите "Запуск пользовательского инструмента".

Если теперь вы открываете Category.cs файл (который вложен под ProductModel.tt), то вы должны увидеть, что коллекция продуктов имеет тип **&lt;ObservableCollection&gt;Product.**

Скомпилируйте проект.

## <a name="lazy-loading"></a>Отложенная загрузка

Свойство **Продуктов** **в категории** класса и **категории** в классе **Product** является навигационными свойствами. В рамках сущности свойства навигации обеспечивают способ навигации между двумя типами сущностей.

EF предоставляет вам возможность загрузки связанных объектов из базы данных автоматически при первом доступе к свойству навигации. При этом типе загрузки (так называемой ленивой загрузки) следует знать, что при первом доступе к каждому навигационному свойству отдельный запрос будет выполнен в базе данных, если содержимое еще не в контексте.

При использовании типов сущностей POCO EF достигает ленивой загрузки, создавая экземпляры производных типов прокси во время выполнения, а затем переопределяет виртуальные свойства в классах, чтобы добавить погрузочный крюк. Чтобы получить ленивый загрузки связанных объектов, вы должны объявить навигационные свойства getters как **общественные** и **виртуальные** **(Переизвлечимые** в Visual Basic), и вы класс не должны быть **запечатаны** **(Невимало в** Visual Basic). При использовании Database First навигационные свойства автоматически становятся виртуальными, чтобы включить ленивую загрузку. В разделе Code First мы решили сделать навигационные свойства виртуальными по той же причине

## <a name="bind-object-to-controls"></a>Привязать объект к элементам управления

Добавьте классы, определенные в модели, в качестве источников данных для этого приложения WPF.

-   Дважды щелкните **MainWindow.xaml** в Solution Explorer, чтобы открыть основную форму
-   Из основного меню выберите **проект -&gt; Добавьте новый источник данных ...**
    (в Visual Studio 2010, вам нужно выбрать **данные -&gt; Добавить новый источник данных ...**)
-   В типе исходного окна исходного кода выберите **объект** и **нажмите далее**
-   В диалоге Select the Data Objects двараза разворачивайте **WPFwithEFSample** и выберите **категорию**  
    *Нет необходимости выбирать источник данных **Продукта,** потому что мы перейдем к нему через свойство **продукта**на источнике данных **категории***  

    ![Выберите объекты данных](~/ef6/media/selectdataobjects.png)

-   Нажмите кнопку **Готово**.
-   Окно источников данных открывается рядом с окном MainWindow.xaml *Если окно источников данных не отображается, выберите **View -&gt; Другие источники данных Windows-&gt; ** *
-   Нажмите значок штифта, чтобы окно источников данных не скрывалось автоматически. Возможно, вам придется нажать кнопку обновления, если окно уже было видно.

    ![обозревателе решений](~/ef6/media/datasources.png)

-   Выберите источник данных **категории** и перетащите его в форму.

Следующее произошло, когда мы тащили этот источник:

-   Ресурс **ыиВитиИи и** **категорияУправлениеDataGrid** были добавлены в XAML 
-   Свойство DataContext на элементе родительской сетки было настроено на **"Категорию StaticResourceViewSource".**Ресурс **категорииViewSource** служит связующим\\источником для внешнего элемента родительской сетки. Элементы внутренней сетки затем наследуют значение DataContext из родительской сетки (свойство категорииDataGrid's ItemsSource настроено на "Связывание")

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

## <a name="adding-a-details-grid"></a>Добавление сетки деталей

Теперь, когда у нас есть сетка для отображения Категории давайте добавим сетку деталей для отображения связанных продуктов.

-   Выберите свойство **Продуктов** из-под источника данных **категории** и перетащите его в форму.
    -   **КатегорияProductsViewSource** ресурс и **сетка productDataGrid** добавляются в XAML
    -   Путь связывания для этого ресурса установлен для Продуктов
    -   Платформа wPF, имеющая обязательную силу для данных, гарантирует, что только продукты, относящиеся к выбранной категории, отображясь в **productDataGrid**
-   От Toolbox перетащите **кнопку** на форму. Установите свойство **имя** **кнопкиСохранить** и свойство **содержимого,** чтобы **сохранить.**

Форма должна выглядеть так же, как и эта:

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>Добавление кода, который обрабатывает взаимодействие с данными

Пришло время добавить некоторые обработчики событий в основное окно.

-   В окне XAML нажмите на элемент ** &lt;window,** это выбирает основное окно
-   В окне **Свойства** выберите **События** в правом верхнем виде, а затем дважды щелкните текстовый ящик справа от **загруженной** метки

    ![Основные свойства окна](~/ef6/media/mainwindowproperties.png)

-   Также добавьте событие **Нажмите** для кнопки **«Сохранить»,** дважды нажав кнопку «Сохранить» в дизайнере. 

Это подводит вас к коду позади для формы, теперь мы будем отодействовать код для использования ProductContext для выполнения доступа к данным. Обновите код для MainWindow, как показано ниже.

Код объявляет давний экземпляр **ProductContext.** Объект **ProductContext** используется для запроса и сохранения данных в базе данных. **Утилизированный ()** экземпляр **ProductContext** вызывается из перебора **метода OnClosing.**Комментарии к коду предоставляют подробную информацию о том, что делает код.

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

## <a name="test-the-wpf-application"></a>Протестировать приложение WPF

-   Скомпилируйте и запустите приложение. Если вы использовали Code First, то вы увидите, что для вас создана база данных **WPFwithEFSample.ProductContext.**
-   Введите имя категории в верхней сетке и названия продуктов в нижней сетке *Не введите ничего в столбцах ID, потому что основной ключ генерируется базой данных*

    ![Главное окно с новыми категориями и продуктами](~/ef6/media/screen1.png)

-   Нажмите кнопку **«Сохранить»,** чтобы сохранить данные в базе данных

После вызова ВСУ **DbContext ()** идентионные данные заполняются значениями, генерируемыми базой данных. Потому что мы вызвали **Refresh()** после **SaveChanges()** элементы управления **DataGrid** обновляются с новыми значениями, а также.

![Главное окно с заселены идентимативными конторами](~/ef6/media/screen2.png)

## <a name="additional-resources"></a>Дополнительные ресурсы

Чтобы узнать больше о привязке [this topic](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) данных к коллекциям с помощью WPF, см.  
