---
title: Code First существующей базы данных — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: 0a51f826422d7e2bff33b968605eace1e754c425
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415601"
---
# <a name="code-first-to-an-existing-database"></a>Code First существующей базы данных
В этом видео и пошаговом руководстве представлены общие сведения о Code First разработке, предназначенной для существующей базы данных. Code First позволяет определить модель с помощью классов C\# или VB.Net. Дополнительно можно выполнить дополнительную настройку с помощью атрибутов в классах и свойствах или с помощью API-интерфейса Fluent.

## <a name="watch-the-video"></a>Просмотреть видео
Это видео [теперь доступно на канале Channel 9](https://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).

## <a name="pre-requisites"></a>Предварительные требования

Для выполнения инструкций этого пошагового руководства необходимо установить **Visual Studio 2012** или **Visual Studio 2013** .

Также потребуется версия **6,1** (или более поздняя) **Entity Framework Tools для Visual Studio** . Сведения об установке последней версии Entity Framework Tools см. в статье [получение Entity Framework](~/ef6/fundamentals/install.md) .

## <a name="1-create-an-existing-database"></a>1. Создание существующей базы данных

Обычно при ориентировании на существующую базу данных она уже будет создана, но в этом пошаговом руководстве нам нужно создать базу данных для доступа.

Перейдем дальше и создадим базу данных.

-   Запустите Visual Studio
-   **Просмотр —&gt; обозреватель сервера**
-   Щелкните правой кнопкой мыши **подключения к данным —&gt; добавить подключение...**
-   Если вы не подключились к базе данных из **Обозреватель сервера** , прежде чем нужно будет выбрать **Microsoft SQL Server** в качестве источника данных

    ![Выберите источник данных](~/ef6/media/selectdatasource.png)

-   Подключитесь к экземпляру LocalDB и введите в качестве имени базы данных **блог**

    ![Соединение LocalDB](~/ef6/media/localdbconnection.png)

-   Нажмите кнопку **ОК** , и появится запрос на создание новой базы данных, выберите **Да** .

    ![Диалоговое окно создания базы данных](~/ef6/media/createdatabasedialog.png)

-   Новая база данных появится в обозреватель сервера, щелкните ее правой кнопкой мыши и выберите пункт **создать запрос** .
-   Скопируйте следующий SQL в новый запрос, щелкните запрос правой кнопкой мыши и выберите команду **выполнить** .

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

Для простоты мы создадим простое консольное приложение, которое использует Code First для доступа к данным:

-   Запустите Visual Studio
-   **Файл-&gt; проект New-&gt;...**
-   Выбор **окон** в меню слева и **консольное приложение**
-   Введите **кодефирстексистингдатабасесампле** в качестве имени
-   Нажмите кнопку **ОК**.

 

## <a name="3-reverse-engineer-model"></a>3. реконструирование модели

Мы будем использовать Entity Framework Tools для Visual Studio, чтобы помочь нам создать начальный код для сопоставлений с базой данных. Эти средства просто создают код, который можно также ввести вручную при желании.

-   **Проект-&gt; добавить новый элемент...**
-   Выберите **данные** в меню слева, а затем **ADO.NET EDM**
-   Введите **BloggingContext** в качестве имени и нажмите кнопку **ОК** .
-   Запустится **мастер EDM**
-   Выберите **Code First из базы данных** и нажмите кнопку **Далее** .

    ![Мастер 1 CFE](~/ef6/media/wizardonecfe.png)

-   Выберите подключение к базе данных, созданной в первом разделе, и нажмите кнопку **Далее** .

    ![Мастер 2 CFE](~/ef6/media/wizardtwocfe.png)

-   Установите флажок рядом с **таблицами** , чтобы импортировать все таблицы, и нажмите кнопку **Готово** .

    ![Мастер 3 CFE](~/ef6/media/wizardthreecfe.png)

После завершения процесса реконструирования в проект будет добавлено несколько элементов, давайте посмотрим, что было добавлено.

### <a name="configuration-file"></a>Файл конфигурации

Файл App. config добавлен в проект, этот файл содержит строку подключения к существующей базе данных.

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

*В файле конфигурации также можно заметить некоторые другие параметры. это параметры по умолчанию EF, которые указывают Code First, где создавать базы данных. Так как мы сопоставлены с существующей базой данных, этот параметр будет пропущен в нашем приложении.*

### <a name="derived-context"></a>Производный контекст

В проект был добавлен класс **BloggingContext** . Контекст представляет сеанс с базой данных, что позволяет нам запрашивать и сохранять данные.
Контекст предоставляет **&gt;DbSet&lt;** для каждого типа в нашей модели. Также обратите внимание, что конструктор по умолчанию вызывает базовый конструктор, используя синтаксис **Name =** . Это говорит Code First, что строка подключения, используемая для этого контекста, должна быть загружена из файла конфигурации.

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

*При использовании строки подключения в файле конфигурации всегда следует использовать синтаксис **Name =** . Это гарантирует, что если строка подключения отсутствует, Entity Framework выдаст исключение, а не создает новую базу данных по соглашению.*

### <a name="model-classes"></a>Классы моделей

Наконец, в проект также добавлены **блог** и класс **POST** . Это классы домена, составляющие модель. Вы увидите заметки к данным, примененные к классам, чтобы указать конфигурацию, в которой Code First соглашения не будут согласованы с структурой существующей базы данных. Например, вы увидите заметку **StringLength** для **Blog.Name** и **Blog. URL** , так как в базе данных максимальная длина **200** (Code First по умолчанию — использование длины максимун, поддерживаемой поставщиком базы данных- **nvarchar (max)** в SQL Server).

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

## <a name="4-reading--writing-data"></a>4. Чтение & запись данных

Теперь, когда у нас есть модель, настала пора использовать ее для доступа к каким-нибудь данным. Реализуйте метод **Main** в **Program.CS** , как показано ниже. Этот код создает новый экземпляр нашего контекста, а затем использует его для вставки нового **блога**. Затем он использует запрос LINQ для получения всех **блогов** из базы данных, упорядоченного в алфавитном порядке по **названию**.

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

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a>Что делать, если изменилась моя база данных?

Мастер Code First к базе данных предназначен для создания начального набора классов, которые затем можно настраивать и изменять. Если схема базы данных изменяется, можно либо вручную изменить классы, либо выполнить другой реконструирование, чтобы перезаписать классы.

## <a name="using-code-first-migrations-to-an-existing-database"></a>Использование Code First Migrations к существующей базе данных

Если вы хотите использовать Code First Migrations с существующей базой данных, см. статью [Code First migrations к существующей базе данных](~/ef6/modeling/code-first/migrations/existing-database.md).

## <a name="summary"></a>Сводка

В этом пошаговом руководстве мы рассматривали Code Firstную разработку, используя существующую базу данных. В Visual Studio используется Entity Framework Tools для реконструирования набора классов, сопоставленных с базой данных, которые можно использовать для хранения и извлечения данных.
