---
title: Самоотслеживающиеся сущности Пошаговое руководство. EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
caps.latest.revision: 3
ms.openlocfilehash: d07ae727ffba60a9296b34b9261acd99f7e8e3b6
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122350"
---
# <a name="self-tracking-entities-walkthrough"></a>Самоотслеживающиеся сущности Пошаговое руководство
> [!IMPORTANT]
> Больше не рекомендуется использовать шаблон сущностей с самостоятельным отслеживанием. Он по-прежнему будет доступен только для поддержки существующих приложений. Если приложению необходимо работать с отключенными графами сущностей, рассмотрите другие варианты, такие как [отслеживаемые сущности](http://trackableentities.github.io/), которые технологически эквивалентны сущностям с самостоятельным отслеживанием, активно разрабатываемым сообществом. Или остановитесь на написании пользовательского кода с помощью API-интерфейсов отслеживания изменений низкого уровня.

В этом пошаговом руководстве описывается сценарий, в котором службы Windows Communication Foundation (WCF) предоставляет операцию, которая возвращает граф объекта. Затем клиентское приложение обрабатывает этот граф и передает изменения операции службы, которая проверяет и сохраняет их в базу данных с помощью Entity Framework.

Перед завершением этого пошагового руководства обязательно прочтите статью [сущностей с самостоятельным отслеживанием](index.md) страницы.

В этом пошаговом руководстве выполняются следующие действия.

-   Создает базу данных для доступа к.
-   Создает библиотеку классов, которая содержит модель.
-   Переключений в шаблон генератора сущностей с самостоятельным отслеживанием.
-   Перемещает классы сущностей в отдельном проекте.
-   Создает службу WCF, которая предоставляет операции для запроса и сохранение объектов.
-   Создает клиентские приложения (консольное и WPF), которые используют службу.

Мы будем использовать первой базы данных в этом пошаговом руководстве, но те же методы также применяются Model First.

## <a name="pre-requisites"></a>Предварительные требования

Для выполнения этого пошагового руководства требуется последнюю версию Visual Studio.

## <a name="create-a-database"></a>Создание базы данных

Сервер базы данных, который устанавливается вместе с Visual Studio отличается в зависимости от версии Visual Studio, вы установили:

-   Если вы используете Visual Studio 2012, а затем вы создадите базу данных LocalDB.
-   Если вы используете Visual Studio 2010 вы создадите базу данных SQL Express.

Перейдем дальше и создать базу данных.

-   Открытие Visual Studio
-   **Представление —&gt; обозревателя серверов**
-   Щелкните правой кнопкой мыши **подключения к данным -&gt; добавить соединение...**
-   Если вы не подключились к базе данных с помощью обозревателя сервера прежде, чем вам нужно будет выбрать **Microsoft SQL Server** как источник данных
-   Подключение к LocalDB или SQL Express, в зависимости от того, какой из них установки
-   Введите **STESample** имя базы данных
-   Выберите **ОК** и вам нужно будет Если вы хотите создать новую базу данных, выберите **Да**
-   Новая база данных появится в обозревателе серверов
-   Если вы используете Visual Studio 2012
    -   Щелкните правой кнопкой мыши, в базе данных в обозревателе серверов и выберите **новый запрос**
    -   Скопируйте следующий запрос SQL в новый запрос, а затем щелкните правой кнопкой мыши запрос и выберите **Execute**
-   Если вы используете Visual Studio 2010
    -   Выберите **тарифный план —&gt; Transact редактор SQL -&gt; новое соединение запроса...**
    -   Введите **.\\ SQLEXPRESS** имя сервера и нажмите кнопку **ОК**
    -   Выберите **STESample** базы данных в раскрывающемся вниз в верхней части редактора запросов
    -   Скопируйте следующий запрос SQL в новый запрос, а затем щелкните правой кнопкой мыши запрос и выберите **Выполнение SQL**

``` SQL
    CREATE TABLE [dbo].[Blogs] (
        [BlogId] INT IDENTITY (1, 1) NOT NULL,
        [Name] NVARCHAR (200) NULL,
        [Url]  NVARCHAR (200) NULL,
        CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
    );

    CREATE TABLE [dbo].[Posts] (
        [PostId] INT IDENTITY (1, 1) NOT NULL,
        [Title] NVARCHAR (200) NULL,
        [Content] NTEXT NULL,
        [BlogId] INT NOT NULL,
        CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
        CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
    );

    SET IDENTITY_INSERT [dbo].[Blogs] ON
    INSERT INTO [dbo].[Blogs] ([BlogId], [Name], [Url]) VALUES (1, N'ADO.NET Blog', N'blogs.msdn.com/adonet')
    SET IDENTITY_INSERT [dbo].[Blogs] OFF
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'Intro to EF', N'Interesting stuff...', 1)
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'What is New', N'More interesting stuff...', 1)
```

## <a name="create-the-model"></a>Создание модели

Мы сначала поместить модели в проект.

-   **Файл —&gt; Новинка —&gt; проекта...**
-   Выберите **Visual C\#**  в области слева и затем **библиотеки классов**
-   Введите **STESample** имя и нажмите кнопку **ОК**

Теперь мы создадим простую модель в конструкторе EF для доступа к базе данных:

-   **Проект -&gt; добавить новый элемент...**
-   Выберите **данных** в области слева и затем **модель EDM ADO.NET**
-   Введите **BloggingModel** имя и нажмите кнопку **ОК**
-   Выберите **создать из базы данных** и нажмите кнопку **Далее**
-   Введите сведения о подключении для базы данных, созданный в предыдущем разделе
-   Введите **BloggingContext** как имя строки подключения и нажмите кнопку **Далее**
-   Установите флажок рядом с полем **таблиц** и нажмите кнопку **Готово**

## <a name="swap-to-ste-code-generation"></a>Вы сможете перейти в ШАГЕ создания кода

Теперь нам нужно отключить создание кода по умолчанию и замены для сущностей с самостоятельным отслеживанием.

### <a name="if-you-are-using-visual-studio-2012"></a>Если вы используете Visual Studio 2012

-   Разверните **BloggingModel.edmx** в **обозревателе решений** и удалить **BloggingModel.tt** и **BloggingModel.Context.tt** 
     *Это отключит формирование кода по умолчанию*
-   Щелкните правой кнопкой мыши пустую область в конструкторе EF и выберите пункт **добавить элемент формирования кода...**
-   Выберите **Online** из левой области и выполните поиск **ТАВИТЬ генератор**
-   Выберите **ТАВИТЬ генератор для C\#**  шаблона, введите **STETemplate** имя и нажмите кнопку **добавить**
-   **STETemplate.tt** и **STETemplate.Context.tt** файлы добавляются как вложенные в файле BloggingModel.edmx

### <a name="if-you-are-using-visual-studio-2010"></a>Если вы используете Visual Studio 2010

-   Щелкните правой кнопкой мыши пустую область в конструкторе EF и выберите пункт **добавить элемент формирования кода...**
-   Выберите **кода** в области слева и затем **генератора сущностей с самостоятельным отслеживанием ADO.NET**
-   Введите **STETemplate** имя и нажмите кнопку **добавить**
-   **STETemplate.tt** и **STETemplate.Context.tt** файлы добавляются непосредственно в проект

## <a name="move-entity-types-into-separate-project"></a>Переместите типы сущностей в отдельном проекте

Для использования сущностей с самостоятельным отслеживанием в клиентском приложении требуется доступ к классам сущностей, созданные из нашей модели. Так как мы не хотим предоставлять всю модель в клиентское приложение мы собираемся переместить классы сущностей в отдельном проекте.

Первым шагом является прекращается создание классов сущностей в существующий проект:

-   Щелкните правой кнопкой мыши **STETemplate.tt** в **обозревателе решений** и выберите **свойства**
-   В **свойства** очистить окно **TextTemplatingFileGenerator** из **CustomTool** свойство
-   Разверните **STETemplate.tt** в **обозревателе решений** и удалить все файлы, вложенным в него

Затем мы собираемся добавить новый проект и создания классов сущностей в нем

-   **Файл —&gt; Add -&gt; проекта...**
-   Выберите **Visual C\#**  в области слева и затем **библиотеки классов**
-   Введите **STESample.Entities** имя и нажмите кнопку **ОК**
-   **Проект -&gt; добавить существующий элемент...**
-   Перейдите к **STESample** папки проекта
-   Выберите для просмотра **все файлы (\*.\*)**
-   Выберите **STETemplate.tt** файла
-   Щелкните стрелку раскрывающегося списка рядом **добавить** и выберите **добавить как ссылку**

    ![AddLinkedTemplate](~/ef6/media/addlinkedtemplate.png)

Кроме того, мы собираемся убедитесь, что классы сущностей создаются в том же пространстве имен, как контекст. Это просто уменьшает число с помощью инструкций, которые необходимо добавить на протяжении нашего приложения.

-   Щелкните правой кнопкой мыши на связанном **STETemplate.tt** в **обозревателе решений** и выберите **свойства**
-   В **свойства** задайте **пространство имен CustomTool** для **STESample**

Код, созданный с помощью шаблона ТАВИТЬ будет нужна ссылка на **System.Runtime.Serialization** для компиляции. Эта библиотека нужна для WCF **DataContract** и **DataMember** атрибутов, используемых для сериализуемых типов сущности.

-   Щелкните правой кнопкой мыши **STESample.Entities** в проекте **обозревателе решений** и выберите **добавить ссылку...**
    -   В Visual Studio 2012, установите флажок рядом с полем **System.Runtime.Serialization** и нажмите кнопку **ОК**
    -   В Visual Studio 2010 — выберите **System.Runtime.Serialization** и нажмите кнопку **ОК**

Наконец проект с наш контекст, в нем будет нужна ссылка на типы сущностей.

-   Щелкните правой кнопкой мыши **STESample** в проекте **обозревателе решений** и выберите **добавить ссылку...**
    -   В Visual Studio 2012 – выберите **решение** в левой области, установите флажок рядом с полем **STESample.Entities** и нажмите кнопку **ОК**
    -   В Visual Studio 2010 — выберите **проекты** выберите **STESample.Entities** и нажмите кнопку **ОК**

>[!NOTE]
> Другой вариант перемещения типы сущностей в отдельном проекте — перемещение файла шаблона, а не связывая его из расположения по умолчанию. После этого необходимо обновить **inputFile** переменных в шаблоне, чтобы указать относительный путь в EDMX-файл (в этом примере, которая была бы **.. \\BloggingModel.edmx**).

## <a name="create-a-wcf-service"></a>Создание службы WCF

Пришло время добавить службу WCF для предоставления наши данные, мы начнем с создания проекта.

-   **Файл —&gt; Add -&gt; проекта...**
-   Выберите **Visual C\#**  в области слева и затем **приложение службы WCF**
-   Введите **STESample.Service** имя и нажмите кнопку **ОК**
-   Добавьте ссылку на **System.Data.Entity** сборки
-   Добавьте ссылку на **STESample** и **STESample.Entities** проектов

Нам нужно скопировать строку подключения EF для этого проекта, таким образом, найденных во время выполнения.

-   Откройте **App.Config** файл ** STESample ** проект и скопировать **connectionStrings** элемент
-   Вставить **connectionStrings** элемент как дочерний элемент элемента **конфигурации** элемент **Web.Config** файл **STESample.Service** проекта

Пришло время для реализации фактической службы.

-   Откройте **IService1.cs** и замените его содержимое следующим кодом

``` csharp
    using System.Collections.Generic;
    using System.ServiceModel;

    namespace STESample.Service
    {
        [ServiceContract]
        public interface IService1
        {
            [OperationContract]
            List<Blog> GetBlogs();

            [OperationContract]
            void UpdateBlog(Blog blog);
        }
    }
```

-   Откройте **Service1.svc** и замените его содержимое следующим кодом

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.Linq;

    namespace STESample.Service
    {
        public class Service1 : IService1
        {
            /// <summary>
            /// Gets all the Blogs and related Posts.
            /// </summary>
            public List<Blog> GetBlogs()
            {
                using (BloggingContext context = new BloggingContext())
                {
                    return context.Blogs.Include("Posts").ToList();
                }
            }

            /// <summary>
            /// Updates Blog and its related Posts.
            /// </summary>
            public void UpdateBlog(Blog blog)
            {
                using (BloggingContext context = new BloggingContext())
                {
                    try
                    {
                        // TODO: Perform validation on the updated order before applying the changes.

                        // The ApplyChanges method examines the change tracking information
                        // contained in the graph of self-tracking entities to infer the set of operations
                        // that need to be performed to reflect the changes in the database.
                        context.Blogs.ApplyChanges(blog);
                        context.SaveChanges();

                    }
                    catch (UpdateException)
                    {
                        // To avoid propagating exception messages that contain sensitive data to the client tier
                        // calls to ApplyChanges and SaveChanges should be wrapped in exception handling code.
                        throw new InvalidOperationException("Failed to update. Try your request again.");
                    }
                }
            }        
        }
    }
```

## <a name="consume-the-service-from-a-console-application"></a>Использовать службу в консольном приложении

Давайте создадим консольное приложение, которое использует нашей службы.

-   **Файл —&gt; Новинка —&gt; проекта...**
-   Выберите **Visual C\#**  в области слева и затем **консольного приложения**
-   Введите **STESample.ConsoleTest** имя и нажмите кнопку **ОК**
-   Добавьте ссылку на **STESample.Entities** проекта

Нам нужно ссылку на службу в нашу службу WCF

-   Щелкните правой кнопкой мыши **STESample.ConsoleTest** в проекте **обозревателе решений** и выберите **добавить ссылку на службу...**
-   Нажмите кнопку **обнаружение**
-   Введите **BloggingService** пространства имен и нажмите кнопку **ОК**

Теперь можно написать код для использования службы.

-   Откройте **Program.cs** и замените его содержимое следующим кодом.

``` csharp
    using STESample.ConsoleTest.BloggingService;
    using System;
    using System.Linq;

    namespace STESample.ConsoleTest
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Print out the data before we change anything
                Console.WriteLine("Initial Data:");
                DisplayBlogsAndPosts();

                // Add a new Blog and some Posts
                AddBlogAndPost();
                Console.WriteLine("After Adding:");
                DisplayBlogsAndPosts();

                // Modify the Blog and one of its Posts
                UpdateBlogAndPost();
                Console.WriteLine("After Update:");
                DisplayBlogsAndPosts();

                // Delete the Blog and its Posts
                DeleteBlogAndPost();
                Console.WriteLine("After Delete:");
                DisplayBlogsAndPosts();

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            static void DisplayBlogsAndPosts()
            {
                using (var service = new Service1Client())
                {
                    // Get all Blogs (and Posts) from the service
                    // and print them to the console
                    var blogs = service.GetBlogs();
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(blog.Name);
                        foreach (var post in blog.Posts)
                        {
                            Console.WriteLine(" - {0}", post.Title);
                        }
                    }
                }

                Console.WriteLine();
                Console.WriteLine();
            }

            static void AddBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Create a new Blog with a couple of Posts
                    var newBlog = new Blog
                    {
                        Name = "The New Blog",
                        Posts =
                        {
                            new Post { Title = "Welcome to the new blog"},
                            new Post { Title = "What's new on the new blog"}
                        }
                    };

                    // Save the changes using the service
                    service.UpdateBlog(newBlog);
                }
            }

            static void UpdateBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The New Blog
                    var blog = blogs.First(b => b.Name == "The New Blog");

                    // Update the Blogs name
                    blog.Name = "The Not-So-New Blog";

                    // Update one of the related posts
                    blog.Posts.First().Content = "Some interesting content...";

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }

            static void DeleteBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The Not-So-New Blog
                    var blog = blogs.First(b => b.Name == "The Not-So-New Blog");

                    // Mark all related Posts for deletion
                    // We need to call ToList because each Post will be removed from the
                    // Posts collection when we call MarkAsDeleted
                    foreach (var post in blog.Posts.ToList())
                    {
                        post.MarkAsDeleted();
                    }

                    // Mark the Blog for deletion
                    blog.MarkAsDeleted();

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }
        }
    }
```

Теперь вы можете запустить приложение и увидеть, как оно работает:

-   Щелкните правой кнопкой мыши **STESample.ConsoleTest** в проекте **обозревателе решений** и выберите **«Отладка» —&gt; запустить новый экземпляр**

При запуске приложения вы увидите следующие выходные данные.

```
Initial Data:
ADO.NET Blog
- Intro to EF
- What is New

After Adding:
ADO.NET Blog
- Intro to EF
- What is New
The New Blog
- Welcome to the new blog
- What's new on the new blog

After Update:
ADO.NET Blog
- Intro to EF
- What is New
The Not-So-New Blog
- Welcome to the new blog
- What's new on the new blog

After Delete:
ADO.NET Blog
- Intro to EF
- What is New

Press any key to exit...
```

## <a name="consume-the-service-from-a-wpf-application"></a>Использование службы в приложении WPF

Давайте создадим приложение WPF, использующего нашей службы.

-   **Файл —&gt; Новинка —&gt; проекта...**
-   Выберите **Visual C\#**  в области слева и затем **приложения WPF**
-   Введите **STESample.WPFTest** имя и нажмите кнопку **ОК**
-   Добавьте ссылку на **STESample.Entities** проекта

Нам нужно ссылку на службу в нашу службу WCF

-   Щелкните правой кнопкой мыши **STESample.WPFTest** в проекте **обозревателе решений** и выберите **добавить ссылку на службу...**
-   Нажмите кнопку **обнаружение**
-   Введите **BloggingService** пространства имен и нажмите кнопку **ОК**

Теперь можно написать код для использования службы.

-   Откройте **MainWindow.xaml** и замените его содержимое следующим кодом.

``` xaml
    <Window
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:STESample="clr-namespace:STESample;assembly=STESample.Entities"
        mc:Ignorable="d" x:Class="STESample.WPFTest.MainWindow"
        Title="MainWindow" Height="350" Width="525" Loaded="Window_Loaded">

        <Window.Resources>
            <CollectionViewSource
                x:Key="blogViewSource"
                d:DesignSource="{d:DesignInstance {x:Type STESample:Blog}, CreateList=True}"/>
            <CollectionViewSource
                x:Key="blogPostsViewSource"
                Source="{Binding Posts, Source={StaticResource blogViewSource}}"/>
        </Window.Resources>

        <Grid DataContext="{StaticResource blogViewSource}">
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding}" Margin="10,10,10,179">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding BlogId}" Header="Id" Width="Auto" IsReadOnly="True" />
                    <DataGridTextColumn Binding="{Binding Name}" Header="Name" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Url}" Header="Url" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding Source={StaticResource blogPostsViewSource}}" Margin="10,145,10,38">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding PostId}" Header="Id" Width="Auto"  IsReadOnly="True"/>
                    <DataGridTextColumn Binding="{Binding Title}" Header="Title" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Content}" Header="Content" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <Button Width="68" Height="23" HorizontalAlignment="Right" VerticalAlignment="Bottom"
                    Margin="0,0,10,10" Click="buttonSave_Click">Save</Button>
        </Grid>
    </Window>
```

-   Откройте код программной части для MainWindow (**MainWindow.xaml.cs**) и замените его содержимое следующим кодом

``` csharp
    using STESample.WPFTest.BloggingService;
    using System.Collections.Generic;
    using System.Linq;
    using System.Windows;
    using System.Windows.Data;

    namespace STESample.WPFTest
    {
        public partial class MainWindow : Window
        {
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Find the view source for Blogs and populate it with all Blogs (and related Posts)
                    // from the Service. The default editing functionality of WPF will allow the objects
                    // to be manipulated on the screen.
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Get the blogs that are bound to the screen
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    var blogs = (List<Blog>)blogsViewSource.Source;

                    // Save all Blogs and related Posts
                    foreach (var blog in blogs)
                    {
                        service.UpdateBlog(blog);
                    }

                    // Re-query for data to get database-generated keys etc.
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }
        }
    }
```

Теперь вы можете запустить приложение и увидеть, как оно работает:

-   Щелкните правой кнопкой мыши **STESample.WPFTest** в проекте **обозревателе решений** и выберите **«Отладка» —&gt; запустить новый экземпляр**
-   Можно обрабатывать данные, используя экрана и сохраните его с помощью службы, используя **Сохранить** кнопки

![WPF](~/ef6/media/wpf.png)
