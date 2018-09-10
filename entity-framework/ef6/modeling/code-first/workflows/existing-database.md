---
title: Code First для существующей базы данных - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: fedfb921919582e2cdb5f3bc497f11889b972ad6
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251080"
---
# <a name="code-first-to-an-existing-database"></a>Code First для существующей базы данных
В этом пошаговом руководстве видео и пошаговые познакомят вас с разработки Code First, предназначенных для существующей базы данных. Во-первых, код позволяет определить модель с помощью c#\# или классам VB.Net. При необходимости дополнительной настройки могут выполняться с помощью атрибутов для классов и свойств или с помощью текучего API.

## <a name="watch-the-video"></a>Просмотреть видео
Этот видеоролик — [теперь доступна на канале Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).

## <a name="pre-requisites"></a>Предварительные требования

Необходимо иметь **Visual Studio 2012** или **Visual Studio 2013** для выполнения этого пошагового руководства.

Вам также потребуется версии **6.1** (или более поздней версии) из **средства платформы Entity Framework для Visual Studio** установлен. См. в разделе [получение Entity Framework](~/ef6/fundamentals/install.md) сведения об установке последней версии средства платформы Entity Framework.

## <a name="1-create-an-existing-database"></a>1. Создание базы данных

Обычно при ориентировании существующей базы данных, он будет уже создан, но для этого пошагового руководства необходимо создать базу данных для доступа к.

Перейдем дальше и создать базу данных.

-   Открытие Visual Studio
-   **Представление —&gt; обозревателя серверов**
-   Щелкните правой кнопкой мыши **подключения к данным -&gt; добавить соединение...**
-   Если вы не подключились к базе данных из **обозревателя серверов** прежде, чем вам нужно будет выбрать **Microsoft SQL Server** как источник данных

    ![Выберите источник данных](~/ef6/media/selectdatasource.png)

-   Подключитесь к экземпляру LocalDB и введите **ведения блогов** имя базы данных

    ![Подключение LocalDB](~/ef6/media/localdbconnection.png)

-   Выберите **ОК** и вам нужно будет Если вы хотите создать новую базу данных, выберите **Да**

    ![Создание диалогового окна базы данных](~/ef6/media/createdatabasedialog.png)

-   Новой базы данных будут отображаться в обозревателе сервера щелкните его правой кнопкой мыши и выберите **новый запрос**
-   Скопируйте следующий запрос SQL в новый запрос, а затем щелкните правой кнопкой мыши запрос и выберите **Execute**

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

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('The Visual Studio Blog', 'http://blogs.msdn.com/visualstudio/')

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('.NET Framework Blog', 'http://blogs.msdn.com/dotnet/')
```

## <a name="2-create-the-application"></a>2. Создание приложения

Для простоты мы собираемся создать простое консольное приложение, которое использует Code First для осуществления доступа к данным:

-   Открытие Visual Studio
-   **Файл —&gt; Новинка —&gt; проекта...**
-   Выберите **Windows** в меню слева и **консольного приложения**
-   Введите **CodeFirstExistingDatabaseSample** как имя
-   Нажмите кнопку **ОК**

 

## <a name="3-reverse-engineer-model"></a>3. Реконструирование модели

Мы будем использовать инструменты Entity Framework для Visual Studio позволяет нам создать некоторый исходный код для сопоставления с базой данных. Эти средства просто генерирует код, который также можно ввести вручную при необходимости.

-   **Проект -&gt; добавить новый элемент...**
-   Выберите **данных** в меню слева и затем **модель EDM ADO.NET**
-   Введите **BloggingContext** имя и нажмите кнопку **ОК**
-   Это откроет **мастер моделей EDM**
-   Выберите **Code First, из базы данных** и нажмите кнопку **Далее**

    ![Мастер один CFE](~/ef6/media/wizardonecfe.png)

-   Выберите соединение с базой данных, созданной в первом разделе и нажмите кнопку **Далее**

    ![Мастер два CFE](~/ef6/media/wizardtwocfe.png)

-   Установите флажок рядом с полем **таблиц** для импорта всех таблиц и нажмите кнопку **Готово**

    ![Мастер три CFE](~/ef6/media/wizardthreecfe.png)

После завершения процесса реконструирования с числом элементов будут добавлены в проект, давайте взглянем на то, что добавляется.

### <a name="configuration-file"></a>файл конфигурации

Файл App.config был добавлен в проект, в этот файл содержит строку подключения к существующей базе данных.

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

*Вы заметите некоторые параметры в файле конфигурации слишком, это параметры по умолчанию EF, которые сообщают, Code First для создания баз данных. Так как мы сопоставляется существующей базы данных, эти параметры будут игнорироваться в нашем приложении.*

### <a name="derived-context"></a>Производного контекста

Объект **BloggingContext** класс будет добавлен в проект. Контекст представляет собой сеанс с базой данных, позволяет запрашивать и сохранять данные.
Предоставляет контекст **DbSet&lt;TEntity&gt;**  для каждого типа в нашей модели. Вы также заметите, что конструктор по умолчанию вызывает конструктор базового класса с помощью **имя =** синтаксис. Это сообщает Code First, что строка подключения, необходимо использовать в этом контексте должна быть загружена из файла конфигурации.

``` csharp
public partial class BloggingContext : DbContext
    {
        public BloggingContext()
            : base("name=BloggingContext")
        {
        }

        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
        }
    }
```

*Следует всегда использовать **имя =** синтаксиса при использовании строки подключения в файле конфигурации. Это гарантирует, что если строке подключения отсутствует затем Entity Framework будет генерировать вместо создания новой базы данных по соглашению.*

### <a name="model-classes"></a>Классы модели

Наконец **блог** и **Post** класс также были добавлены в проект. Это доменных классов, входящих в состав нашей модели. Вы увидите заметки данных применяются к классам для указания конфигурации где соглашения Code First бы не согласуются со структурой существующей базы данных. Например, вы увидите **StringLength** заметки на **Blog.Name** и **Blog.Url** так, как они имеют длину не более **200** в базы данных (Code First по умолчанию разрешено использовать максимальное длину, поддерживаемую поставщиком баз данных - **nvarchar(max)** в SQL Server).

``` csharp
public partial class Blog
{
    public Blog()
    {
        Posts = new HashSet<Post>();
    }

    public int BlogId { get; set; }

    [StringLength(200)]
    public string Name { get; set; }

    [StringLength(200)]
    public string Url { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

## <a name="4-reading--writing-data"></a>4. Чтение и запись данных

Теперь, когда у нас есть модель, настала пора использовать ее для доступа к некоторые данные. Реализуйте **Main** метод в **Program.cs** как показано ниже. Этот код создает новый экземпляр класса наш контекст и затем используется, чтобы вставить новый **блог**. Затем она использует запрос LINQ для извлечения всех **блоги** из базы данных, в алфавитном порядке по **Title**.

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

Теперь можно запустить приложение и протестировать его.

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a>Что делать, если Моя база данных изменения?

Code First для базы данных мастер предназначен для создания начальной точки набор классов, которые можно настроить и изменить. При изменении схемы базы данных можно вручную изменить классы или выполнить другую реконструирования перезаписать классы.

## <a name="using-code-first-migrations-to-an-existing-database"></a>С помощью Code First Migrations для существующей базы данных

Если вы хотите использовать Code First Migrations с существующей базы данных, см. в разделе [Code First Migrations для существующей базы данных](~/ef6/modeling/code-first/migrations/existing-database.md).

## <a name="summary"></a>Сводка

В этом пошаговом руководстве мы рассмотрели шаблона разработки Code First с помощью существующей базы данных. Мы использовали средства платформы Entity Framework для Visual Studio, чтобы реконструировать набор классов, которые сопоставляются с базой данных и может использоваться для хранения и извлечения данных.
