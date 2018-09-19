---
title: Code First в новой базе данных - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: a19db575b685cde98509fff4a0efaf26106b26bc
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284126"
---
# <a name="code-first-to-a-new-database"></a>Code First в новой базе данных
В этом пошаговом руководстве видео и пошаговые познакомят вас с разработки Code First, предназначенные для новой базы данных. Этот сценарий включает предназначенные для базы данных, которая не существует и Code First создаст пустую базу данных, Code First добавит новые таблицы для. Во-первых, код позволяет определить модель с помощью c#\# или классам VB.Net. Дополнительная настройка при необходимости выполняются с помощью атрибутов для классов и свойств или с помощью текучего API.

## <a name="watch-the-video"></a>Просмотреть видео
Этот видеоролик представляет собой введение разработки Code First, предназначенные для новой базы данных. Этот сценарий включает предназначенные для базы данных, которая не существует и Code First создаст пустую базу данных, Code First добавит новые таблицы для. Во-первых, код позволяет определить модель с помощью классов C# или VB.Net. Дополнительная настройка при необходимости выполняются с помощью атрибутов для классов и свойств или с помощью текучего API.

**Представляет**: [Роуэн Миллер (Rowan Miller)](http://romiller.com/)

**Видео**: [WMV](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)

## <a name="pre-requisites"></a>Предварительные требования

Для выполнения этого пошагового руководства необходимо иметь по крайней мере Visual Studio 2010 или Visual Studio 2012.

Если вы используете Visual Studio 2010, также необходимо будет иметь [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) установлен.

## <a name="1-create-the-application"></a>1. Создание приложения

Для простоты мы собираемся создать простое консольное приложение, которое использует Code First для осуществления доступа к данным.

-   Открытие Visual Studio
-   **Файл —&gt; Новинка —&gt; проекта...**
-   Выберите **Windows** в меню слева и **консольного приложения**
-   Введите **CodeFirstNewDatabaseSample** как имя
-   Нажмите кнопку **ОК**

## <a name="2-create-the-model"></a>2. Создание модели

Давайте определим очень простую модель с помощью классов. Мы просто определяем их в файл Program.cs, но в случае реального приложения должно разбивать классы out в отдельных файлах и, возможно, отдельный проект.

Ниже определении класса программы в файле Program.cs добавьте следующие два класса.

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

Вы заметите, что мы создаем два свойства навигации (Blog.Posts и Post.Blog) виртуальный. Это позволяет функции отложенной загрузки в Entity Framework. Отложенная загрузка означает, что содержимое этих свойств будет автоматически загружаться из базы данных при попытке доступа к ним.

## <a name="3-create-a-context"></a>3. Создайте контекст

Пришло время для определения производного контекста, который представляет сеанс подключения с базой данных, позволяет запрашивать и сохранять данные. Мы определяем контекст, который является производным от System.Data.Entity.DbContext и предоставляет типизированный DbSet&lt;TEntity&gt; для каждого класса в нашей модели.

Теперь мы запускаем следует использовать типы из Entity Framework, поэтому нам нужно добавить пакет EntityFramework NuGet.

-   **Проект —&gt; управление пакетами NuGet...**
    Примечание: Если у вас нет **управление пакетами NuGet...** параметр, необходимо установить [последнюю версию NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)
-   Выберите **Online** вкладку
-   Выберите **EntityFramework** пакета
-   Нажмите кнопку **установки**

Добавить с помощью инструкции для System.Data.Entity в верхней части Program.cs.

``` csharp
using System.Data.Entity;
```

Под Post класса в файле Program.cs добавьте следующие производного контекста.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

Ниже приведен полный листинг что еще теперь должно содержать файл Program.cs.

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

Это весь код, который необходимо запустить, хранения и извлечения данных. Очевидно, что есть довольно много происходит за кулисами, и мы рассмотрим краткий обзор, что в момент, однако сначала посмотрим на это в действии.

## <a name="4-reading--writing-data"></a>4. Чтение и запись данных

Реализуйте метод Main в файле Program.cs, как показано ниже. Этот код создает новый экземпляр класса наш контекст, а затем использует его для вставки нового блога. Затем он использует запрос LINQ для извлечения из базы данных, представлены в алфавитном порядке по названию все блоги.

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
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a>Где мои данные?

По соглашению DbContext автоматически создала базу данных.

-   Если локальный экземпляр SQL Express (устанавливается по умолчанию вместе с Visual Studio 2010) затем Code First базы данных на этом экземпляре создается
-   Если SQL Express недоступен, то Code First будет попробуйте и использовать [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (устанавливается по умолчанию с помощью Visual Studio 2012)
-   База данных называется после полное имя производного контекста, в нашем случае это **CodeFirstNewDatabaseSample.BloggingContext**

Это просто соглашения по умолчанию существуют различные способы изменения базы данных, который использует Code First, Дополнительные сведения можно найти в **как DbContext обнаруживает модель и подключение к базе данных** раздела.
Можно подключиться к этой базе данных, с помощью обозревателя серверов в Visual Studio

-   **Представление —&gt; обозревателя серверов**
-   Щелкните правой кнопкой мыши **подключения к данным** и выберите **добавить соединение...**
-   Если вы не подключились к базе данных с помощью обозревателя сервера прежде, чем вам потребуется выбрать в качестве источника данных Microsoft SQL Server

    ![Выберите источник данных](~/ef6/media/selectdatasource.png)

-   Подключение к LocalDB или SQL Express, в зависимости от того, какой из них установки

Теперь можно проверить, Code First создает схему.

![Начальный схемы](~/ef6/media/schemainitial.png)

Классы для включения в модель, просмотрев свойства DbSet, которые мы определили работы DbContext. Затем набор соглашения Code First, по умолчанию используется для определения имен таблиц и столбцов, определения типов данных, найти первичные ключи и т. д. Далее в этом пошаговом руководстве мы рассмотрим, как можно переопределить эти соглашения.

## <a name="5-dealing-with-model-changes"></a>5. Изменения модели

Теперь пора внести некоторые изменения в нашей модели, когда мы внести эти изменения, необходимо также обновить схему базы данных. Для этого мы собираемся использовать средство Code First Migrations и миграции для краткости.

Миграция позволяет нам упорядоченный набор шагов, которые описывают обновление (или понижение) нашей схемы базы данных. Каждая из этих действий, известных как миграцию, содержит код, который описывает изменения, применяемые. 

Первым шагом является включение Code First Migrations для наших BloggingContext.

-   **Средства —&gt; диспетчер пакетов библиотеки -&gt; консоль диспетчера пакетов**
-   Запустите команду **Enable-Migrations** в консоли диспетчера пакетов.
-   Новая папка миграции был добавлен в наш проект, содержащий два элемента:
    -   **Configuration.cs** — этот файл содержит параметры, которые будут использовать миграции для переноса BloggingContext. Нет необходимости изменять что-либо в этом пошаговом руководстве, но Вот где можно указать начальное значение данных, поставщики register для других баз данных, изменяет пространство имен что миграции создаются в и т.д.
    -   **&lt;Метка времени&gt;\_InitialCreate.cs** – это первую миграцию, он представляет изменения, которые уже были применены к базы данных, пользоваться не пустую базу данных, который включает в себя блогов и записей таблицы . Несмотря на то, что мы позволим Code First автоматически создавать эти таблицы для нас, раз уж мы выбрали для миграций, они были преобразованы в миграции. Код сначала также записывается в нашей локальной базы данных, что уже был применен такой миграции. Метка времени в имени файла используется для сортировки целей.

    Теперь давайте внести изменения в нашей модели, добавьте в класс блог свойства URL-адреса:

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   Запустите **AddUrl Add-Migration** команду в консоли диспетчера пакетов.
    Команда Add-Migration проверяет наличие изменений с момента последнего перехода и формирует шаблоны новой миграции с любыми изменениями, которые находятся. Мы можем дать миграций имени; в этом случае мы называем миграции «AddUrl».
    Шаблонный код говорит, что нам нужно добавить столбец URL-адрес, который может содержать строковые данные, dbo. Таблица блоги. При необходимости, мы может изменить сформированный код, но в данном случае это не обязательно.

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

-   Запустите **Update-Database** команду в консоли диспетчера пакетов. Эта команда применит все незавершенные миграции к базе данных. Миграция это InitialCreate уже был применен, поэтому миграция будет просто применить нашей новой миграции AddUrl.
    Совет: Можно использовать **– Verbose** при вызове Update-Database, чтобы увидеть SQL, который выполняется в базе данных.

Теперь новый столбец URL-адрес добавляется в блоги таблицу в базе данных:

![Схема с URL-адрес](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a>6. Заметки к данным

Пока мы просто позволили EF обнаруживать модели с помощью его соглашения по умолчанию, но будут ли раз, когда наших классов не соответствуют соглашениям об, и нам нужно иметь возможность дальнейшей настройки. Существует два варианта. Мы рассмотрим заметок к данным в этом разделе, а затем текучего API в следующем разделе.

-   Добавим к нашей модели пользовательского класса

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Необходимо также добавить набор в нашей производного контекста

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   Если мы попытались добавить миграцию, то получили бы ошибка "*EntityType «User» имеет ключ не определен. Определите ключ для этого типа EntityType.»* так как EF не имеет возможности узнать, что имя пользователя должно быть первичный ключ для пользователя.
-   Мы собираемся теперь использовать заметки к данным, поэтому нам нужно добавить с помощью оператора в верхней части Program.cs

```
using System.ComponentModel.DataAnnotations;
```

-   Теперь Добавление заметок свойство Username, чтобы определить, что он является первичным ключом

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Используйте **AddUser Add-Migration** команду, чтобы сформировать шаблон миграции для применения этих изменений в базу данных
-   Запустите **Update-Database** команду, чтобы применить созданную миграцию к базе данных

Теперь новая таблица добавляется в базу данных:

![Схема с пользователями](~/ef6/media/schemawithusers.png)

Приведен полный список заметок, поддерживаемых EF.

-   [KeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [StringLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [MaxLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [ConcurrencyCheckAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [RequiredAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [TimestampAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [ComplexTypeAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [ColumnAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [TableAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [InversePropertyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [ForeignKeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [DatabaseGeneratedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [NotMappedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a>7. Текучий API

В предыдущем разделе мы рассмотрели использование заметок к данным для дополнения или переопределить, что была обнаружена по соглашению. Другой способ настройки модели — с помощью Code First fluent API.

Большинство конфигураций модели можно сделать с помощью простых данных заметок. Текучий API — это более сложных способ указания модели конфигурации, который охватывает все, что заметки к данным можно делать в дополнение к некоторые более сложные конфигурации, недоступные в заметки к данным. Заметки к данным и текучий API могут использоваться совместно.

Для доступа к fluent API переопределить метод OnModelCreating в DbContext. Предположим, мы хотели бы переименовать столбец, который User.DisplayName хранится в для отображения\_имя.

-   Переопределите метод OnModelCreating на BloggingContext следующим кодом

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

-   Используйте **ChangeDisplayName Add-Migration** команду, чтобы сформировать шаблон миграции для применения этих изменений в базу данных.
-   Запустите **Update-Database** команду, чтобы применить созданную миграцию к базе данных.

Теперь в столбце DisplayName переименовывается для отображения\_имя:

![Схема с отображаемым именем переименован](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a>Сводка

В этом пошаговом руководстве мы рассмотрели шаблона разработки Code First с помощью новой базы данных. Мы определена модель, с помощью классов, затем использовать эту модель для создания базы данных и хранения и извлечения данных. После создания базы данных мы использовали Code First Migrations для изменения схемы, как наша модель развитие. Мы также увидели, как настроить модель с помощью заметок к данным и Fluent API.
