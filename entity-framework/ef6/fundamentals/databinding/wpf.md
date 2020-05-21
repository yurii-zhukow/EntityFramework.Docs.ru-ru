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
# <a name="databinding-with-wpf"></a>Привязка данных с помощью WPF

> [!IMPORTANT]
> **Этот документ является допустимым для WPF только на .NET Framework**
>
> В этом документе описывается привязка данных для WPF на .NET Framework. Для новых проектов .NET Core рекомендуется использовать [EF Core](/ef/core) вместо Entity Framework 6. Документация по DataBinding в EF Core выполняется в [#778е проблем](https://github.com/dotnet/EntityFramework.Docs/issues/778).

В этом пошаговом руководстве показано, как привязать типы POCO к элементам управления WPF в форме "основной/подробности". Приложение использует Entity Framework API-интерфейсы для заполнения объектов данными из базы данных, трассировки изменений и сохранения данных в базе данных.

Модель определяет два типа, участвующих в связи "один ко многим": **Категория** (основная \\ Главная) и **продукт** (зависимые \\ Подробности). Затем средства Visual Studio используются для привязки типов, определенных в модели, к элементам управления WPF. Платформа привязки данных WPF позволяет переходить между связанными объектами: выбор строк в главном представлении приводит к обновлению подробного представления соответствующими дочерними данными.

Снимки экрана и списки кода в этом пошаговом руководстве взяты из Visual Studio 2013 но вы можете выполнить это пошаговое руководство с помощью Visual Studio 2012 или Visual Studio 2010.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>Использование параметра "Object" для создания источников данных WPF

В предыдущей версии Entity Framework мы использовали, чтобы рекомендовать использовать параметр **базы данных** при создании нового источника данных на основе модели, созданной с помощью конструктора EF. Это было вызвано тем, что конструктор создаст контекст, производный от ObjectContext и классов сущностей, производных от EntityObject. Использование параметра базы данных поможет написать лучший код для взаимодействия с этой поверхностью API.

Конструкторы EF для Visual Studio 2012 и Visual Studio 2013 создают контекст, производный от DbContext вместе с простыми классами сущностей POCO. В Visual Studio 2010 мы рекомендуем переключаться на шаблон создания кода, использующий DbContext, как описано далее в этом пошаговом руководстве.

При использовании поверхности API DbContext следует использовать параметр **Object** при создании нового источника данных, как показано в этом пошаговом руководстве.

При необходимости можно [вернуться к созданию кода на основе ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) для моделей, созданных с помощью конструктора EF.

## <a name="pre-requisites"></a>Предварительные требования

Для работы с этим пошаговым руководством необходимо установить Visual Studio 2013, Visual Studio 2012 или Visual Studio 2010.

Если вы используете Visual Studio 2010, необходимо также установить NuGet. Дополнительные сведения см. в разделе [Установка NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).  

## <a name="create-the-application"></a>Создание приложения

-   Открытие Visual Studio
-   **Файл- &gt; создать &gt; проект...**
-   Выберите **окна**   в левой области и **впфаппликатион** на правой панели.
-   Введите **впфвисефсампле**в   качестве имени
-   Нажмите кнопку **ОК** .

## <a name="install-the-entity-framework-nuget-package"></a>Установка пакета NuGet Entity Framework

-   В обозреватель решений щелкните правой кнопкой мыши проект **винформсвисефсампле**
-   Выберите **Управление пакетами NuGet...**
-   В диалоговом окне Управление пакетами NuGet выберите вкладку в **сети** и выберите пакет **EntityFramework** .
-   Щелкните **Установить**.  
    >[!NOTE]
    > В дополнение к сборке EntityFramework также добавляется ссылка на System. ComponentModel. Аннотация. Если проект содержит ссылку на System. Data. Entity, то он будет удален при установке пакета EntityFramework. Сборка System. Data. Entity больше не используется для приложений Entity Framework 6.

## <a name="define-a-model"></a>Определение модели

В этом пошаговом руководстве можно выбрать реализацию модели с помощью Code First или конструктора EF. Выполните один из двух следующих разделов.

### <a name="option-1-define-a-model-using-code-first"></a>Вариант 1. Определение модели с помощью Code First

В этом разделе показано, как создать модель и связанную с ней базу данных с помощью Code First. Перейдите к следующему разделу (**вариант 2. Определение модели с помощью Database First)** , если вы предпочитаете использовать Database First для реконструирования модели из базы данных с помощью конструктора EF.

При использовании Code First разработки обычно начинается написание .NET Framework классов, определяющих концептуальную (доменную) модель.

-   Добавьте новый класс в **впфвисефсампле:**
    -   Щелкните правой кнопкой мыши имя проекта
    -   Выберите **Добавить**, а затем **новый элемент** .
    -   Выберите **класс** и введите **Product**   в качестве имени класса.
-   Замените **Product**   Определение класса Product следующим кодом:

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

Свойство **Products** класса **Category** и свойство **Category** в классе **Product** являются свойствами навигации. В Entity Framework свойства навигации предоставляют способ навигации по связям между двумя типами сущностей.

Помимо определения сущностей, необходимо определить класс, производный от DbContext и предоставляющий свойства DbSetости &lt; &gt; . &lt;Свойства домика DbSet &gt; позволяют контексту понять, какие типы необходимо включить в модель.

Экземпляр производного типа DbContext управляет объектами сущностей во время выполнения, включая заполнение объектов данными из базы данных, отслеживание изменений и сохранение данных в базе данных.

-   Добавьте в проект новый класс **продуктконтекст** со следующим определением:

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

### <a name="option-2-define-a-model-using-database-first"></a>Вариант 2. Определение модели с помощью Database First

В этом разделе показано, как использовать Database First для реконструирования модели из базы данных с помощью конструктора EF. Если вы завершили предыдущий раздел (**вариант 1. Определение модели с помощью Code First)**, пропустите этот раздел и перейдите непосредственно к разделу " **ленивая загрузка** ".

#### <a name="create-an-existing-database"></a>Создание существующей базы данных

Обычно при использовании существующей базы данных она будет создана, но в этом пошаговом руководстве необходимо создать базу данных для доступа к ней.

Сервер базы данных, устанавливаемый с Visual Studio, отличается в зависимости от установленной версии Visual Studio.

-   При использовании Visual Studio 2010 будет создаваться база данных SQL Express.
-   Если вы используете Visual Studio 2012, то будете создавать базу данных [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) .

Давайте создадим базу данных.

-   **Просмотр — &gt; Обозреватель сервера**
-   Щелкните правой кнопкой мыши **подключения к данным — &gt; Добавить подключение...**
-   Если вы не подключились к базе данных из обозреватель сервера, прежде чем нужно будет выбрать Microsoft SQL Server в качестве источника данных

    ![Изменение источника данных](~/ef6/media/changedatasource.png)

-   Подключитесь к LocalDB или SQL Express, в зависимости от того, какой из них установлен, и введите **Products** в качестве имени базы данных.

    ![Добавить соединение LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Добавить подключение Express](~/ef6/media/addconnectionexpress.png)

-   Нажмите кнопку **ОК** , и появится запрос на создание новой базы данных, выберите **Да** .

    ![Создание базы данных](~/ef6/media/createdatabase.png)

-   Новая база данных появится в обозреватель сервера, щелкните ее правой кнопкой мыши и выберите пункт **создать запрос** .
-   Скопируйте следующий SQL в новый запрос, щелкните запрос правой кнопкой мыши и выберите команду **выполнить** .

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

#### <a name="reverse-engineer-model"></a>Модель реконструирования

Мы будем использовать Entity Framework Designer, который входит в состав Visual Studio, для создания нашей модели.

-   **Проект- &gt; Добавить новый элемент...**
-   Выберите **данные** в меню слева, а затем **ADO.NET EDM**
-   Введите **ProductModel** в качестве имени и нажмите кнопку **ОК** .
-   Запустится **мастер EDM**
-   Выберите **создать из базы данных** и нажмите кнопку **Далее** .

    ![Выбор содержимого модели](~/ef6/media/choosemodelcontents.png)

-   Выберите подключение к базе данных, созданной в первом разделе, введите **продуктконтекст** в качестве имени строки подключения и нажмите кнопку **Далее** .

    ![Выбор подключения](~/ef6/media/chooseyourconnection.png)

-   Установите флажок рядом с пунктом "таблицы", чтобы импортировать все таблицы, и нажмите кнопку "Готово".

    ![Выбор объектов](~/ef6/media/chooseyourobjects.png)

После завершения процесса реконструирования новая модель добавляется в проект и открывается для просмотра в Entity Framework Designer. Файл App. config также был добавлен в проект со сведениями о соединении для базы данных.

#### <a name="additional-steps-in-visual-studio-2010"></a>Дополнительные действия в Visual Studio 2010

При работе в Visual Studio 2010 необходимо обновить конструктор EF для использования создания кода EF6.

-   Щелкните правой кнопкой мыши пустое место в модели в конструкторе EF и выберите пункт **Добавить элемент создания кода...**
-   В меню слева выберите **шаблоны в Интернете** и выполните поиск по запросу **DbContext**
-   Выберите **генератор EF 6. x DbContext для C \# ,** введите **продуктсмодел** в качестве имени и нажмите кнопку Добавить.

#### <a name="updating-code-generation-for-data-binding"></a>Обновление создания кода для привязки данных

EF создает код из модели с помощью шаблонов T4. Шаблоны, поставляемые с Visual Studio или скачанные из коллекции Visual Studio, предназначены для использования в общих целях. Это означает, что сущности, созданные из этих шаблонов, имеют &lt; простые &gt; свойства ICollection T. Однако при выполнении привязки данных с помощью WPF желательно использовать **ObservableCollection** для свойств коллекции, чтобы WPF мог вести отслеживание изменений, внесенных в коллекции. К этому концу мы будем изменять шаблоны для использования ObservableCollection.

-   Откройте **Обозреватель решений** и найдите файл **ProductModel. EDMX**
-   Найдите файл **ProductModel.TT** , который будет вложен в файл ProductModel. EDMX

    ![Шаблон модели продукта WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   Дважды щелкните файл ProductModel.tt, чтобы открыть его в редакторе Visual Studio.
-   Найдите и замените два вхождения "**ICollection**" на "**ObservableCollection**". Они расположены примерно в строках 296 и 484.
-   Найдите и замените первое вхождение "**hashing**" на "**ObservableCollection**". Это событие находится примерно в строке 50. **Не** заменяйте второе вхождение набора хэширования, найденного далее в коде.
-   Поиск и замена единственного вхождения "**System. Collections. Generic**" на "**System. Collections. ObjectModel**". Он расположен примерно в строке 424.
-   Сохраните файл ProductModel.tt. Это должно привести к повторному формированию кода для сущностей. Если код не воссоздается автоматически, щелкните правой кнопкой мыши ProductModel.tt и выберите команду "запустить пользовательский инструмент".

Если теперь открыть файл Category.cs (который вложен в ProductModel.tt), вы должны увидеть, что коллекция Products имеет тип " ** &lt; &gt; продукт ObservableCollection**".

Скомпилируйте проект.

## <a name="lazy-loading"></a>Отложенная загрузка

Свойство **Products** класса **Category** и свойство **Category** в классе **Product** являются свойствами навигации. В Entity Framework свойства навигации предоставляют способ навигации по связям между двумя типами сущностей.

EF дает возможность автоматически загружать связанные сущности из базы данных при первом доступе к свойству навигации. При таком типе загрузки (называемой отложенной загрузкой) имейте в виду, что при первом обращении к каждому свойству навигации будет выполнен отдельный запрос к базе данных, если содержимое еще не находится в контексте.

При использовании типов сущностей POCO EF достигает отложенной загрузки, создавая экземпляры производных прокси-типов во время выполнения, а затем переопределяя виртуальные свойства в классах для добавления обработчика загрузки. Чтобы получить отложенную загрузку связанных объектов, необходимо объявить методы считывания свойств навигации как **открытые** и **Виртуальные** (**переопределяемые** в Visual Basic), и класс не должен быть **запечатанным** (**NotOverridable** в Visual Basic). При использовании Database First свойства навигации автоматически становятся виртуальными для включения отложенной загрузки. В разделе Code First мы решили сделать свойства навигации виртуальными по той же причине.

## <a name="bind-object-to-controls"></a>Привязка объекта к элементам управления

Добавьте классы, определенные в модели, в качестве источников данных для этого приложения WPF.

-   Дважды щелкните файл **MainWindow. XAML** в Обозреватель решений, чтобы открыть главную форму.
-   В главном меню выберите **проект — &gt; Добавить новый источник данных...**
    (в Visual Studio 2010 необходимо выбрать **данные — &gt; Добавить новый источник данных...**)
-   В поле выберите источник данных Типевиндов выберите **объект** и нажмите кнопку **Далее** .
-   В диалоговом окне Выбор объектов данных unfold **впфвисефсампле**   два раза и выберите Category ( **Категория** ).  
    *Нет необходимости выбирать источник данных **продукта** , так как мы будем приступить к нему через свойство **продукта**в источнике данных **категории** .*  

    ![Выбор объектов данных](~/ef6/media/selectdataobjects.png)

-   Нажмите кнопку **Готово**.
-   Окно Источники данных открывается рядом с окном MainWindow. XAML *, если окно Источники данных не отображается, выберите **вид — &gt; другие &gt; Источники данных Windows** * .
-   Нажмите значок закрепить, чтобы окно Источники данных не скрывалось автоматически. Если окно уже видимо, может потребоваться нажать кнопку Обновить.

    ![Data Sources](~/ef6/media/datasources.png)

-   Выберите источник данных **Категория** и перетащите его в форму.

При перетаскивании этого источника произошло следующее:

-   Ресурс **категоривиевсаурце** и элемент управления **категоридатагрид** были добавлены в XAML. 
-   Свойству DataContext родительского элемента Grid присвоено значение "{StaticResource **категоривиевсаурце** }".Ресурс **категоривиевсаурце** выступает в качестве источника привязки для внешнего \\ родительского элемента Grid. Внутренние элементы сетки затем наследуют значение DataContext от родительской сетки (свойство ItemsSource Категоридатагрид имеет значение {Binding}).

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

Теперь, когда у нас есть сетка для вывода категорий, давайте добавим сетку сведений для вывода связанных продуктов.

-   Выберите свойство **Products** из источника данных **Категория** и перетащите его в форму.
    -   Ресурс **категорипродуктсвиевсаурце** и сетка **продуктдатагрид** добавляются в XAML.
    -   Для пути привязки для этого ресурса задано значение Products
    -   Платформа привязки данных WPF гарантирует, что в **продуктдатагрид** отображаются только продукты, связанные с выбранной категорией.
-   Из панели элементов перетащите **кнопку** в форму. Задайте для свойства **Name** значение **Буттонсаве** и свойство **содержимого** для **сохранения**.

Форма должна выглядеть следующим образом:

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>Добавление кода, обрабатывающего взаимодействие с данными

Пора добавить в главное окно несколько обработчиков событий.

-   В окне XAML щелкните элемент ** &lt; Window** , чтобы выбрать главное окно.
-   В окне **Свойства** выберите **события** в правом верхнем углу, а затем дважды щелкните текстовое поле справа от **загруженной** метки.

    ![Свойства главного окна](~/ef6/media/mainwindowproperties.png)

-   Также добавьте событие **Click** для кнопки **сохранить** , дважды щелкнув кнопку Сохранить в конструкторе. 

В результате вы перейдете к коду программной части формы. Теперь мы выполним редактирование кода, чтобы использовать Продуктконтекст для доступа к данным. Обновите код для MainWindow, как показано ниже.

Код объявляет долго выполняющийся экземпляр **продуктконтекст**. Объект **продуктконтекст** используется для запроса и сохранения данных в базе данных. Метод **Dispose ()** в экземпляре **продуктконтекст** вызывается из переопределенного метода **onclosinging** .Комментарии к коду содержат сведения о том, что делает код.

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

-   Скомпилируйте и запустите приложение. Если вы использовали Code First, вы увидите, что создана база данных **впфвисефсампле. продуктконтекст** .
-   Введите имя категории в верхней сетке, а названия продуктов в нижней сетке *не вводят ничего в столбцы идентификаторов, так как первичный ключ создается базой данных* .

    ![Главное окно с новыми категориями и продуктами](~/ef6/media/screen1.png)

-   Нажмите кнопку " **сохранить** ", чтобы сохранить данные в базе данных

После вызова метода **SaveChanges ()** DbContext идентификаторы заполняются сформированными значениями базы данных. Так как мы назвали метод **Refresh ()** после вызова метода **SaveChanges ()** , элементы управления **DataGrid** также обновляются вместе с новыми значениями.

![Главное окно с заполненными идентификаторами](~/ef6/media/screen2.png)

## <a name="additional-resources"></a>Дополнительные ресурсы

Дополнительные сведения о привязке данных к коллекциям с помощью WPF см. в [этом разделе](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) документации по WPF.  
