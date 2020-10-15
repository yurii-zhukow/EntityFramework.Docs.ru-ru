---
title: Code First в новую базу данных — EF6
description: Code First в новую базу данных в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/code-first/workflows/new-database
ms.openlocfilehash: 9d0082ac5226fff066d3e18c9164e2230c84b285
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92065023"
---
# <a name="code-first-to-a-new-database"></a>Создание базы данных с помощью Code First
В этом видео и пошаговом руководстве представлены общие сведения о Code First разработке, предназначенной для новой базы данных. Этот сценарий включает в себя целевую базу данных, которая не существует и Code First создаст, или пустую базу данных, в которую Code First добавит новые таблицы. Code First позволяет определить модель с помощью \# классов C или VB.NET. Дополнительную конфигурацию можно также выполнить с помощью атрибутов в классах и свойствах или с помощью API Fluent.

## <a name="watch-the-video"></a>Просмотреть видео
В этом видео представлены общие сведения о Code First разработке, предназначенной для новой базы данных. Этот сценарий включает в себя целевую базу данных, которая не существует и Code First создаст, или пустую базу данных, в которую Code First добавит новые таблицы. Code First позволяет определить модель с помощью классов C# или VB.Net. Дополнительную конфигурацию можно также выполнить с помощью атрибутов в классах и свойствах или с помощью API Fluent.

**Представляет**: [Роуэн Миллер (Rowan Miller)](https://romiller.com/)

**Видео**: [WMV](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv)  |  [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v)  |  [WMV (ZIP)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)

## <a name="pre-requisites"></a>Предварительные требования

Для выполнения инструкций этого пошагового руководства необходимо установить Visual Studio 2010 или Visual Studio 2012.

Если вы используете Visual Studio 2010, вам также потребуется установить [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .

## <a name="1-create-the-application"></a>1. Создание приложения

Чтобы не усложнять, мы создадим простое консольное приложение, которое использует Code First для доступа к данным.

-   Открытие Visual Studio
-   **Файл- &gt; создать &gt; проект...**
-   Выбор **окон** в меню слева и **консольное приложение**
-   Введите **кодефирстневдатабасесампле** в качестве имени
-   Нажмите кнопку **ОК**.

## <a name="2-create-the-model"></a>2. Создание модели

Давайте определим очень простую модель с помощью классов. Мы просто определяем их в файле Program.cs, но в реальном мире вы разбиваете классы на отдельные файлы и, возможно, на отдельный проект.

Под определением класса программы в Program.cs добавьте следующие два класса.

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public virtual Blog Blog { get; set; }
}
```

Вы заметите, что два свойства навигации (blog. posts и POST. blog) являются виртуальными. Это включает функцию отложенной загрузки Entity Framework. Отложенная загрузка означает, что содержимое этих свойств будет автоматически загружаться из базы данных при попытке доступа к ним.

## <a name="3-create-a-context"></a>3. Создание контекста

Теперь пора определить производный контекст, который представляет сеанс с базой данных, что позволит нам запрашивать и сохранять данные. Мы определяем контекст, производный от System. Data. Entity. DbContext, и предоставляет типизированный DbSetный &lt; домик &gt; для каждого класса в нашей модели.

Теперь мы начали использовать типы из Entity Framework, поэтому нам нужно добавить пакет NuGet EntityFramework.

-   **Проект — &gt; Управление пакетами NuGet...**
    Примечание. Если у вас нет **пакетов NuGet для управления...** параметр следует установить [последнюю версию NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)
-   Выбор вкладки "в **сети** "
-   Выберите пакет **EntityFramework**
-   Щелкните **Установить**.

Добавьте инструкцию using для System. Data. Entity в верхнюю часть Program.cs.

``` csharp
using System.Data.Entity;
```

Под классом POST в Program.cs добавьте следующий производный контекст.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

Ниже приведен полный список элементов, которые теперь должны содержаться в Program.cs.

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Entity;

namespace CodeFirstNewDatabaseSample
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }

    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }
}
```

Это весь код, необходимый для начала хранения и извлечения данных. Очевидно, что в фоновом режиме происходит довольно много времени, и мы рассмотрим его в действии.

## <a name="4-reading--writing-data"></a>4. чтение & запись данных

Реализуйте метод Main в Program.cs, как показано ниже. Этот код создает новый экземпляр нашего контекста, а затем использует его для вставки нового блога. Затем он использует запрос LINQ для получения всех блогов из базы данных, упорядоченного в алфавитном порядке по названию.

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
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a>Где мои данные?

По соглашению DbContext создала базу данных.

-   Если локальный экземпляр SQL Express доступен (устанавливается по умолчанию в Visual Studio 2010), Code First создал базу данных на этом экземпляре
-   Если экспресс-выпуск SQL Server недоступен, Code First попытается использовать [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (устанавливается по умолчанию в Visual Studio 2012).
-   База данных именуется после полного имени производного контекста, в нашем случае это **кодефирстневдатабасесампле. BloggingContext**

Это только соглашения по умолчанию, и существуют различные способы изменения базы данных, которую Code First использует. Дополнительные сведения см. в разделе **DbContext обнаружение модели и подключения к базе данных** .
Вы можете подключиться к этой базе данных с помощью обозреватель сервера в Visual Studio.

-   **Просмотр — &gt; Обозреватель сервера**
-   Щелкните правой кнопкой мыши **подключения к данным** и выберите **Добавить подключение...**
-   Если вы не подключились к базе данных из обозреватель сервера, прежде чем нужно будет выбрать Microsoft SQL Server в качестве источника данных

    ![Выберите источник данных](~/ef6/media/selectdatasource.png)

-   Подключение к LocalDB или SQL Express в зависимости от того, какой из установленных служб

Теперь можно проверить схему, созданную Code First.

![Начальная схема](~/ef6/media/schemainitial.png)

DbContext выработала, какие классы следует включить в модель, взглянув на определяемые нами свойства DbSet. Затем он использует набор соглашений по умолчанию Code First для определения имен таблиц и столбцов, определения типов данных, поиска первичных ключей и т. д. Далее в этом пошаговом руководстве мы рассмотрим, как можно переопределить эти соглашения.

## <a name="5-dealing-with-model-changes"></a>5. Работа с изменениями модели

Пришло время внести некоторые изменения в нашу модель, при внесении этих изменений нам также потребуется обновить схему базы данных. Для этого мы будем использовать функцию, именуемую Code First Migrations, или миграцию для коротких.

Миграция позволяет нам выполнить упорядоченный набор шагов, описывающих порядок обновления схемы базы данных. Каждый из этих шагов, называемый миграцией, содержит код, описывающий применяемые изменения. 

Первым шагом является включение Code First Migrations для нашего BloggingContext.

-   **Средства — &gt; Диспетчер пакетов библиотек — &gt; консоль диспетчера пакетов**
-   Запустите команду **Enable-Migrations** в консоли диспетчера пакетов.
-   В наш проект добавлена новая папка миграции, содержащая два элемента:
    -   **Configuration.CS** — этот файл содержит параметры, которые будут использоваться миграцией для миграции BloggingContext. Нам не нужно ничего менять в этом пошаговом руководстве, но здесь можно указать начальные данные, зарегистрировать поставщики для других баз данных, изменить пространство имен, в котором создаются миграции, и т. д.
    -   ** &lt; timestamp &gt; \_ InitialCreate.CS** — это первая миграция, которая представляет изменения, уже примененные к базе данных для того, чтобы сделать ее пустой базой данных, которая включает таблицы блогов и записей. Хотя мы позволяем Code First автоматически создавать эти таблицы для нас, теперь, когда мы участвуем в миграции, они были преобразованы в миграцию. Code First также записана в локальную базу данных, к которой уже применена эта миграция. Метка времени для имени файла используется для упорядочения.

    Теперь изменим нашу модель, добавив свойство URL в класс Blog:

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   Выполните команду **Add-Migration аддурл** в консоли диспетчера пакетов.
    Команда Add-Migration проверяет наличие изменений с момента последней миграции и формирует новую миграцию с любыми найденными изменениями. Мы можем присвоить имя миграции. в этом случае вызывается миграция "Аддурл".
    Шаблонный код говорит, что нам нужно добавить столбец URL-адреса, который может содержать строковые данные, в dbo. Таблица блогов. При необходимости можно изменить шаблонный код, но это не является обязательным в этом случае.

``` csharp
namespace CodeFirstNewDatabaseSample.Migrations
{
    using System;
    using System.Data.Entity.Migrations;

    public partial class AddUrl : DbMigration
    {
        public override void Up()
        {
            AddColumn("dbo.Blogs", "Url", c => c.String());
        }

        public override void Down()
        {
            DropColumn("dbo.Blogs", "Url");
        }
    }
}
```

-   Выполните команду **обновления базы данных** в консоли диспетчера пакетов. Эта команда применит все незавершенные миграции к базе данных. Наша InitialCreate миграция уже применена, поэтому миграция будет просто применена к нашей новой Аддурл миграции.
    Совет. при вызове Update-Database можно использовать параметр **– verbose** , чтобы увидеть SQL, выполняемый в базе данных.

Новый столбец URL-адреса теперь добавляется в таблицу блоги базы данных:

![Схема с URL-адресом](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a>6. заметки к данным

До сих пор мы просто добавим EF к модели, используя соглашения по умолчанию, но бывают случаи, когда наши классы не соответствуют соглашениям, и нам нужно иметь возможность выполнить дальнейшую настройку. Для этого есть два варианта. Мы рассмотрим примечания к данным в этом разделе, а затем API Fluent в следующем разделе.

-   Давайте добавим в нашу модель класс User.

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Также необходимо добавить набор в производный контекст

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   Если мы попытались добавить миграцию, возникнет ошибка, говорящая, что*ключ EntityType "User" не определен. Определите ключ для этого типа EntityType ".* Поскольку EF не имеет возможности знать, что имя пользователя должно быть первичным ключом для пользователя.
-   Теперь мы будем использовать заметки к данным, поэтому нам нужно добавить инструкцию using в начало Program.cs

```csharp
using System.ComponentModel.DataAnnotations;
```

-   Теперь добавьте аннотацию для свойства UserName, чтобы обозначить, что это первичный ключ

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Используйте команду **adduser для добавления и миграции** , чтобы сформировать шаблон миграции, чтобы применить эти изменения к базе данных.
-   Выполните команду **обновления базы данных** , чтобы применить новую миграцию к базе данных.

Новая таблица будет добавлена в базу данных:

![Схема с пользователями](~/ef6/media/schemawithusers.png)

Полный список заметок, поддерживаемых EF,:

-   [кэйаттрибуте](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [стрингленгсаттрибуте](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [MaxLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [конкурренцичеккаттрибуте](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [RequiredAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [тиместампаттрибуте](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [комплекстипеаттрибуте](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [колумнаттрибуте](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [TableAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [инверсепропертяттрибуте](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [фореигнкэйаттрибуте](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [датабасеженератедаттрибуте](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [нотмаппедаттрибуте](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a>7. fluent API

В предыдущем разделе мы рассматривали использование заметок к данным для дополнения или переопределения того, что было обнаружено соглашением. Другой способ настройки модели — через API-интерфейс Code First Fluent.

Большую часть конфигурации модели можно выполнить с помощью простых заметок к данным. API-интерфейс Fluent — это более сложный способ определения конфигурации модели, охватывающий все возможности, которые могут делать заметки к данным в дополнение к более сложной конфигурации с заметками данных. Заметки к данным и API Fluent можно использовать вместе.

Чтобы получить доступ к API Fluent, переопределите метод OnModelCreating в DbContext. Предположим, требуется переименовать столбец, в котором хранится User. DisplayName, в для отображаемого \_ имени.

-   Переопределите метод OnModelCreating в BloggingContext с помощью следующего кода

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .Property(u => u.DisplayName)
            .HasColumnName("display_name");
    }
}
```

-   Используйте команду **Add-Migration чанжедисплайнаме** , чтобы сформировать шаблон миграции, чтобы применить эти изменения к базе данных.
-   Выполните команду **обновления базы данных** , чтобы применить новую миграцию к базе данных.

Теперь столбец DisplayName переименован в отображаемое \_ Имя:

![Схема с переименованным отображаемым именем](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a>Итоги

В этом пошаговом руководстве мы рассматривали Code Firstную разработку с помощью новой базы данных. Мы определили модель, используя классы, которые затем использовали эту модель для создания базы данных, хранения и извлечения данных. После создания базы данных мы использовали Code First Migrations, чтобы изменить схему по мере развития нашей модели. Мы также увидели, как настроить модель с помощью заметок к данным и API Fluent.
