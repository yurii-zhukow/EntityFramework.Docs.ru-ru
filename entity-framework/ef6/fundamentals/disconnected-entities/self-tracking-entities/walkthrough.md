---
title: Пошаговое руководство для самостоятельного отслеживания сущностей — EF6
description: Пошаговое руководство по самостоятельному отслеживанию сущностей для Entity Framework 6
author: divega
ms.date: 10/23/2016
uid: ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough
ms.openlocfilehash: 398be11d330f5a7413f5a84424217ea3eda446ef
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90072802"
---
# <a name="self-tracking-entities-walkthrough"></a>Пошаговое руководство по самостоятельному отслеживанию сущностей
> [!IMPORTANT]
> Больше не рекомендуется использовать шаблон сущностей с самостоятельным отслеживанием. Он по-прежнему будет доступен только для поддержки существующих приложений. Если приложению необходимо работать с отключенными графами сущностей, рассмотрите другие варианты, такие как [отслеживаемые сущности](https://trackableentities.github.io/), которые технологически эквивалентны сущностям с самостоятельным отслеживанием, активно разрабатываемым сообществом. Или остановитесь на написании пользовательского кода с помощью API-интерфейсов отслеживания изменений низкого уровня.

В этом пошаговом руководстве демонстрируется сценарий, в котором служба Windows Communication Foundation (WCF) предоставляет операцию, которая возвращает граф сущностей. Затем клиентское приложение манипулирует этим графом и отправляет изменения в операцию службы, которая проверяет и сохраняет обновления в базе данных с помощью Entity Framework.

Перед выполнением этого пошагового руководства убедитесь, что вы читаете страницу [сущностей с самостоятельным отслеживанием](xref:ef6/fundamentals/disconnected-entities/self-tracking-entities/index) .

В этом пошаговом руководстве выполняются следующие действия.

-   Создает базу данных для доступа.
-   Создает библиотеку классов, содержащую модель.
-   Меняет местами на шаблон генератора сущностей с самостоятельным отслеживанием.
-   Перемещает классы сущностей в отдельный проект.
-   Создает службу WCF, которая предоставляет операции для запросов и сохранения сущностей.
-   Создает клиентские приложения (консоль и WPF), которые используют службу.

В этом пошаговом руководстве мы будем использовать Database First, но одни и те же методы будут применяться одинаково для Model First.

## <a name="pre-requisites"></a>Предварительные требования

Для выполнения этого пошагового руководства потребуется последняя версия Visual Studio.

## <a name="create-a-database"></a>Создание базы данных

Сервер базы данных, устанавливаемый с Visual Studio, отличается в зависимости от установленной версии Visual Studio.

-   Если вы используете Visual Studio 2012, то будете создавать базу данных LocalDB.
-   При использовании Visual Studio 2010 будет создаваться база данных SQL Express.

Давайте создадим базу данных.

-   Открытие Visual Studio
-   **Просмотр — &gt; Обозреватель сервера**
-   Щелкните правой кнопкой мыши **подключения к данным — &gt; Добавить подключение...**
-   Если вы не подключились к базе данных из обозреватель сервера, прежде чем нужно будет выбрать **Microsoft SQL Server** в качестве источника данных
-   Подключение к LocalDB или SQL Express в зависимости от того, какой из установленных служб
-   Введите **стесампле** в качестве имени базы данных
-   Нажмите кнопку **ОК** , и появится запрос на создание новой базы данных, выберите **Да** .
-   Теперь новая база данных появится в обозреватель сервера
-   Если вы используете Visual Studio 2012
    -   Щелкните правой кнопкой мыши базу данных в обозревателе сервера и выберите действие **Создать запрос**.
    -   Скопируйте следующий SQL в новый запрос, щелкните запрос правой кнопкой мыши и выберите команду **выполнить** .
-   Если вы используете Visual Studio 2010
    -   Выбор **данных — &gt; Редактор SQL — &gt; создать соединение с запросом...**
    -   Введите **. \\ **В качестве имени сервера в SQLExpress нажмите кнопку **ОК** .
    -   Выберите базу данных **стесампле** в раскрывающемся списке в верхней части редактора запросов.
    -   Скопируйте следующий SQL в новый запрос, щелкните запрос правой кнопкой мыши и выберите команду **выполнить SQL** .

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

Сначала нам нужен проект, в котором будет размещена модель.

-   **Файл- &gt; создать &gt; проект...**
-   Выберите **Visual C \# ** в левой области, а затем **Библиотека классов**
-   Введите **стесампле** в качестве имени и нажмите кнопку **ОК** .

Теперь создадим простую модель в конструкторе EF для доступа к нашей базе данных:

-   **Проект- &gt; Добавить новый элемент...**
-   Выберите **данные** на левой панели, а затем **ADO.NET EDM**
-   Введите **блоггингмодел** в качестве имени и нажмите кнопку **ОК** .
-   Выберите **создать из базы данных** и нажмите кнопку **Далее** .
-   Введите сведения о подключении для базы данных, созданной в предыдущем разделе.
-   Введите **BloggingContext** в качестве имени для строки подключения и нажмите кнопку " **Далее** ".
-   Установите флажки рядом с **таблицами** и нажмите кнопку **Готово** .

## <a name="swap-to-ste-code-generation"></a>Переключение на ТАВИТЬ создание кода

Теперь необходимо отключить создание кода по умолчанию и переключение на сущности с самостоятельным отслеживанием.

### <a name="if-you-are-using-visual-studio-2012"></a>Если вы используете Visual Studio 2012

-   Разверните **блоггингмодел. EDMX** в **Обозреватель решений** и удалите **BloggingModel.TT** и **BloggingModel.Context.TT**. 
     *это приведет к отключению создания кода по умолчанию* .
-   Щелкните правой кнопкой мыши пустую область в области конструктора EF и выберите пункт **Добавить элемент создания кода...**
-   Выберите "в **сети** " в левой области и найдите **генератор тавить** .
-   Выберите шаблон **генератор тавить для C \# ** , введите **стетемплате** в качестве имени и нажмите кнопку **Добавить** .
-   Файлы **STETemplate.TT** и **STETemplate.Context.TT** добавляются в файл блоггингмодел. edmx.

### <a name="if-you-are-using-visual-studio-2010"></a>Если вы используете Visual Studio 2010

-   Щелкните правой кнопкой мыши пустую область в области конструктора EF и выберите пункт **Добавить элемент создания кода...**
-   Выберите **код** на левой панели, а затем **ADO.NET самостоятельный Генератор сущностей**
-   Введите **стетемплате** в качестве имени и нажмите кнопку **Добавить** .
-   Файлы **STETemplate.TT** и **STETemplate.Context.TT** добавляются непосредственно в проект.

## <a name="move-entity-types-into-separate-project"></a>Переместить типы сущностей в отдельный проект

Для использования сущностей с самостоятельным отслеживанием клиентскому приложению требуется доступ к классам сущностей, созданным из нашей модели. Так как мы не хотим предоставлять клиентским приложениям всю модель, мы будем перемещать классы сущностей в отдельный проект.

Первым шагом является завершение создания классов сущностей в существующем проекте:

-   Щелкните правой кнопкой мыши **STETemplate.TT** в **Обозреватель решений** и выберите пункт **Свойства** .
-   В окне **свойств** очистите **Тексттемплатингфилеженератор** из свойства **CustomTool** .
-   Разверните **STETemplate.TT** в **Обозреватель решений** и удалите все вложенные файлы.

Далее предстоит добавить новый проект и создать в нем классы сущностей.

-   **Файл — &gt; Добавить &gt; проект...**
-   Выберите **Visual C \# ** в левой области, а затем **Библиотека классов**
-   Введите **стесампле. Entities** в качестве имени и нажмите кнопку **ОК** .
-   **Проект- &gt; Добавить существующий элемент...**
-   Перейдите в папку проекта **стесампле**
-   Выберите для просмотра **всех файлов ( \* . \* )**
-   Выберите файл **STETemplate.TT**
-   Щелкните стрелку раскрывающегося списка рядом с кнопкой **Добавить** и выберите **Добавить как ссылку** .

    ![Добавить связанный шаблон](~/ef6/media/addlinkedtemplate.png)

Кроме того, мы собираемся убедиться, что классы сущностей создаются в том же пространстве имен, что и контекст. Это просто сокращает количество операторов using, которые необходимо добавить в наше приложение.

-   Щелкните правой кнопкой мыши связанный **STETemplate.TT** в **Обозреватель решений** и выберите **Свойства** .
-   В окне **Свойства** задайте для **пространства имен пользовательского инструмента** значение **стесампле** .

Для компиляции кода, созданного с помощью шаблона ТАВИТЬ, требуется ссылка на **System. Runtime. Serialization** . Эта библиотека необходима для атрибутов **DATACONTRACT** WCF и **DataMember** , которые используются в сериализуемых типах сущностей.

-   Щелкните правой кнопкой мыши проект **стесампле. Entities** в **Обозреватель решений** и выберите команду **Добавить ссылку...**
    -   В Visual Studio 2012 установите флажок рядом с **System. Runtime. Serialization** и нажмите кнопку **ОК** .
    -   В Visual Studio 2010 выберите **System. Runtime. Serialization** и нажмите кнопку **ОК** .

Наконец, в проекте с нашим контекстом потребуется ссылка на типы сущностей.

-   Щелкните правой кнопкой мыши проект **стесампле** в **Обозреватель решений** и выберите **Добавить ссылку...**
    -   В Visual Studio 2012 — выберите **решение** в левой области, установите флажок рядом с **стесампле. Entities** и нажмите кнопку **ОК** .
    -   В Visual Studio 2010 на вкладке **проекты** выберите **стесампле. Entities** и нажмите кнопку **ОК** .

>[!NOTE]
> Другим вариантом перемещения типов сущностей в отдельный проект является перемещение файла шаблона вместо его связи с расположением по умолчанию. В этом случае необходимо будет обновить переменную **inputFile** в шаблоне, чтобы указать относительный путь к EDMX-файлу (в этом примере это будет **. \\ Блоггингмодел. EDMX**).

## <a name="create-a-wcf-service"></a>Создание службы WCF

Пришло время добавить службу WCF для предоставления наших данных, мы начнем с создания проекта.

-   **Файл — &gt; Добавить &gt; проект...**
-   Выберите **Visual C \# ** в левой области, а затем — **приложение службы WCF** .
-   Введите **стесампле. Service** в качестве имени и нажмите кнопку **ОК** .
-   Добавление ссылки на сборку **System. Data. Entity**
-   Добавление ссылки на проекты **стесампле** и **стесампле. Entities**

Нам нужно скопировать строку подключения EF в этот проект, чтобы она была найдена во время выполнения.

-   Откройте файл **App.Config** для проекта **стесампле **и скопируйте элемент **ConnectionString** .
-   Вставьте элемент **ConnectionString** в качестве дочернего элемента элемента **конфигурации** файла **Web.Config** в проекте **стесампле. Service.**

Теперь пора реализовать фактическую службу.

-   Откройте **IService1.CS** и замените содержимое следующим кодом:

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

-   Откройте **Service1. svc** и замените содержимое следующим кодом:

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

## <a name="consume-the-service-from-a-console-application"></a>Использование службы из консольного приложения

Давайте создадим консольное приложение, использующее нашу службу.

-   **Файл- &gt; создать &gt; проект...**
-   Выберите **Visual C \# ** в левой области, а затем **консольное приложение**
-   Введите **стесампле. консолетест** в качестве имени и нажмите кнопку **ОК** .
-   Добавление ссылки на проект **стесампле. Entities**

Нам нужна ссылка на службу для нашей службы WCF.

-   Щелкните правой кнопкой мыши проект **стесампле. консолетест** в **обозреватель решений** и выберите **Добавление ссылки на службу...**
-   Нажмите кнопку **обнаружить** .
-   Введите **блоггингсервице** в качестве пространства имен и нажмите кнопку **ОК** .

Теперь мы можем написать код для использования службы.

-   Откройте **Program.CS** и замените содержимое следующим кодом.

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

-   Щелкните правой кнопкой мыши проект **стесампле. консолетест** в **Обозреватель решений** и выберите **Отладка- &gt; запустить новый экземпляр** .

При выполнении приложения вы увидите следующие выходные данные.

```console
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

## <a name="consume-the-service-from-a-wpf-application"></a>Использование службы из приложения WPF

Давайте создадим приложение WPF, которое использует нашу службу.

-   **Файл- &gt; создать &gt; проект...**
-   Выберите **Visual C \# ** в левой области, а затем — **приложение WPF**
-   Введите **стесампле. впфтест** в качестве имени и нажмите кнопку **ОК** .
-   Добавление ссылки на проект **стесампле. Entities**

Нам нужна ссылка на службу для нашей службы WCF.

-   Щелкните правой кнопкой мыши проект **стесампле. впфтест** в **обозреватель решений** и выберите **Добавление ссылки на службу...**
-   Нажмите кнопку **обнаружить** .
-   Введите **блоггингсервице** в качестве пространства имен и нажмите кнопку **ОК** .

Теперь мы можем написать код для использования службы.

-   Откройте файл **MainWindow. XAML** и замените его содержимое следующим кодом.

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

-   Откройте код программной части для MainWindow (**MainWindow.XAML.CS**) и замените содержимое следующим кодом.

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

-   Щелкните правой кнопкой мыши проект **стесампле. впфтест** в **Обозреватель решений** и выберите **Отладка- &gt; запустить новый экземпляр** .
-   Вы можете манипулировать данными с помощью экрана и сохранить их через службу с помощью кнопки **сохранить** .

![Главное окно WPF](~/ef6/media/wpf.png)
