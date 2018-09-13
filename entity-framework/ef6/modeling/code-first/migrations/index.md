---
title: Code First Migrations — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 36591d8f-36e1-4835-8a51-90f34f633d1e
ms.openlocfilehash: 0e2f33d3402929c93a339ea26dcade875da52a61
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490291"
---
# <a name="code-first-migrations"></a>Code First Migrations
Code First Migrations — это рекомендуемый способ разработки схемы базы данных приложения при использовании рабочего процесса Code First. Code First Migrations предоставляет набор средств со следующими функциями:

1. Создание исходной базы данных, которая работает с моделью EF
2. Создание миграций для отслеживания изменений в модели EF
2. Поддержание актуальности базы данных с учетом этих изменений

Следующее пошаговое руководство содержит обзор Code First Migrations в Entity Framework. Вы можете выполнить все пошаговое руководство или сразу перейти в нужный раздел. Здесь представлены следующие разделы.

## <a name="building-an-initial-model--database"></a>Привязка начальной модели и базы данных

Прежде чем мы начнем использовать миграции, нам потребуется проект и модель Code First, с которой мы будем работать. В этом пошаговом руководстве мы собираемся использовать каноническую модель **Blog** и **Post**.

-   Создайте новое консольное приложение **MigrationsDemo**.
-   Добавьте последнюю версию пакета NuGet **EntityFramework** в проект.
    -   **Средства —&gt; Диспетчер пакетов библиотеки —&gt; Консоль диспетчера пакетов**
    -   Запустите команду **EntityFramework Install-Package**
-   Добавьте файл **Model.cs** с кодом, показанным ниже. Этот код определяет один класс **Blog**, составляющий нашу модель предметной области, и класс **BlogContext**, который будет нашим контекстом EF Code First.

  ``` csharp
      using System.Data.Entity;
      using System.Collections.Generic;
      using System.ComponentModel.DataAnnotations;
      using System.Data.Entity.Infrastructure;

      namespace MigrationsDemo
      {
          public class BlogContext : DbContext
          {
              public DbSet<Blog> Blogs { get; set; }
          }

          public class Blog
          {
              public int BlogId { get; set; }
              public string Name { get; set; }
          }
      }
  ```

-   Теперь, когда у нас есть модель, пора использовать ее для доступа к данным. Внесите в файл **Program.cs** приведенный ниже код.

  ``` csharp
      using System;
      using System.Collections.Generic;
      using System.Linq;
      using System.Text;

      namespace MigrationsDemo
      {
          class Program
          {
              static void Main(string[] args)
              {
                  using (var db = new BlogContext())
                  {
                      db.Blogs.Add(new Blog { Name = "Another Blog " });
                      db.SaveChanges();

                      foreach (var blog in db.Blogs)
                      {
                          Console.WriteLine(blog.Name);
                      }
                  }

                  Console.WriteLine("Press any key to exit...");
                  Console.ReadKey();
              }
          }
      }
  ```

-   Запустите приложение, и вы увидите, что база данных **MigrationsCodeDemo.BlogContext** создается автоматически.

    ![База данных LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Включение миграций

Пора внести дополнительные изменения в нашу модель.

-   Давайте внесем в класс Blog свойство Url.

``` csharp
    public string Url { get; set; }
```

Если бы вы запустили приложение снова, оно выдало бы исключение InvalidOperationException с сообщением: *Модель, поддерживающая контекст BlogContext, изменилась с момента создания базы данных. Попробуйте обновить базу данных с помощью Code First Migrations (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](http://go.microsoft.com/fwlink/?LinkId=238269)*).*

Как видно из исключения, пора приступить к использованию Code First Migrations. Сначала нужно включить миграции для нашего контекста.

-   Запустите команду **Enable-Migrations** в консоли диспетчера пакетов.

    В результате этой команды в проект будет добавлена папка **Migrations**. В новой папке два файла:

-   **Класс конфигурации.** Этот класс позволяет настраивать поведение миграций для контекста. В этом пошаговом руководстве мы будем просто использовать конфигурацию по умолчанию.
    *Поскольку имеется только один контекст Code First в проекте, Enable-Migrations автоматически заполняет тип контекста, к которому относится эта конфигурация.*
-   **Миграция InitialCreate**. Эта миграция создана, так как мы уже использовали Code First для создания базы данных, прежде чем включили миграции. Код в этой созданной по шаблону миграции представляет объекты, которые уже были созданы в базе данных. В нашем случае это таблица **Blog** со столбцами **BlogId** и **Name**. Имя файла содержит метку времени для удобства упорядочения.
    *Если бы база данных еще не была создана, миграция InitialCreate не была бы добавлена в проект. Вместо этого, когда мы впервые вызвали бы Add-Migration, код для создания этих таблиц был бы перенесен в новую миграцию.*

### <a name="multiple-models-targeting-the-same-database"></a>Несколько моделей для одной целевой базы данных

При использовании версий до EF6 можно было использовать только одну модель Code First для создания схемы базы данных и управления ей. Это было связано с наличием всего одной таблицы **\_\_MigrationsHistory** на базу данных, когда невозможно было определить, какая запись какой модели принадлежит.

Начиная с EF6, класс **Configuration** включает свойство **ContextKey**. Это уникальный идентификатор для каждой модели Code First. В соответствующий столбец в таблице **\_\_MigrationsHistory** вносятся записи из нескольких моделей, которые используют таблицу совместно. По умолчанию этому свойству присвоено полное имя контекста.

## <a name="generating--running-migrations"></a>Создание и запуск миграций

Вам нужно знать о двух основных командах Code First Migrations.

-   **Add-Migration** будет автоматически формировать следующую миграцию на основе изменений, внесенных в модель с момента создания последней миграции.
-   **Update-Database** будет применять ожидающие обработки миграции к базе данных.

Нам нужно сформировать шаблон миграции для нового свойства Url. Команда **Add-Migration** позволяет нам давать этим миграциям имя. Давайте назовем нашу **AddBlogUrl**.

-   Запустите команду **Add-Migration AddBlogUrl** в консоли диспетчера пакетов.
-   В папке **Migrations** у нас теперь есть новая миграция **AddBlogUrl**. Имя файла миграции имеет метку времени в целях упорядочения.

``` csharp
    namespace MigrationsDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddBlogUrl : DbMigration
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

Теперь мы могли бы изменить или дополнить эту миграцию, но все и так в порядке. Мы используем **Update-Database**, чтобы применить эту миграцию к базе данных.

-   Запустите команду **Update-Database** в консоли диспетчера пакетов.
-   Code First Migrations сравнит миграции из папки **Migrations** с теми, которые были применены к базе данных. Вы увидите, что необходимо применить и запустить миграцию **AddBlogUrl**.

База данных **MigrationsDemo.BlogContext** обновлена и включает столбец **Url** в таблице **Blog**.

## <a name="customizing-migrations"></a>Настройка миграции

Пока мы создавали и запускали миграции без изменений. Теперь давайте рассмотрим редактирование кода, который создается по умолчанию.

-   Пора внести некоторые изменения в нашу модель. Давайте добавим новое свойство **Rating** в класс **Blog**.

``` csharp
    public int Rating { get; set; }
```

-   Также добавим новый класс **Post**.

``` csharp
    public class Post
    {
        public int PostId { get; set; }
        [MaxLength(200)]
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
```

-   Еще мы добавим коллекцию **Posts** в класс **Blog**, чтобы создать второй конец связи между **Blog** и **Post**.

``` csharp
    public virtual List<Post> Posts { get; set; }
```

Мы будем использовать команду **Add-Migration**, чтобы с помощью Code First Migrations автоматически создать миграцию. Мы назовем эту миграцию **AddPostClass**.

-   Запустите команду **Add-Migration AddPostClass** в консоли диспетчера пакетов.

С помощью Code First Migrations прекрасно удалось создать эти изменения по шаблону, но мы хотим изменить кое-что еще.

1.  Сначала давайте добавим уникальный индекс в столбец **Posts.Title** (строки 22 и 29 в приведенном ниже коде).
2.  Мы также добавим столбец **Blogs.Rating**, не допускающий значение null. Если в таблице есть данные, новому столбцу будет назначено значение CLR по умолчанию в зависимости от типа данных (оценка — это целое число, так что это будет **0**). Но нам нужно указать значение по умолчанию **3**, чтобы существующие строки в таблице **Blogs** начинались с неплохой оценки.
    (Значение по умолчанию присваивается в строке 24 приведенного ниже кода.)

``` csharp
    namespace MigrationsDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddPostClass : DbMigration
        {
            public override void Up()
            {
                CreateTable(
                    "dbo.Posts",
                    c => new
                        {
                            PostId = c.Int(nullable: false, identity: true),
                            Title = c.String(maxLength: 200),
                            Content = c.String(),
                            BlogId = c.Int(nullable: false),
                        })
                    .PrimaryKey(t => t.PostId)
                    .ForeignKey("dbo.Blogs", t => t.BlogId, cascadeDelete: true)
                    .Index(t => t.BlogId)
                    .Index(p => p.Title, unique: true);

                AddColumn("dbo.Blogs", "Rating", c => c.Int(nullable: false, defaultValue: 3));
            }

            public override void Down()
            {
                DropIndex("dbo.Posts", new[] { "Title" });
                DropIndex("dbo.Posts", new[] { "BlogId" });
                DropForeignKey("dbo.Posts", "BlogId", "dbo.Blogs");
                DropColumn("dbo.Blogs", "Rating");
                DropTable("dbo.Posts");
            }
        }
    }
```

Измененная миграция готова к запуску, так что давайте обновим ее с помощью команды **Update-Database**. Давайте укажем флаг **–Verbose**, чтобы вы могли видеть SQL, который выполняет Code First Migrations.

-   Запустите команду **Update-Database –Verbose** в консоли диспетчера пакетов.

## <a name="data-motion--custom-sql"></a>Перемещение данных или настраиваемый SQL

До сих пор мы рассматривали операции миграции, которые не меняют и не перемещают данные. Теперь давайте посмотрим на примеры, где мы перемещаем данные. Для перемещения данных пока нет собственной поддержки, но мы можем запустить произвольные команды SQL в любой момент в нашем сценарии.

-   Давайте добавим свойство **Post.Abstract** в нашу модель. Позже мы заполним свойство **Abstract** для существующих записей текстом из начала столбца **Content**.

``` csharp
    public string Abstract { get; set; }
```

Мы будем использовать команду **Add-Migration**, чтобы с помощью Code First Migrations автоматически создать миграцию.

-   Запустите команду **Add-Migration AddPostAbstract** в консоли диспетчера пакетов.
-   Созданная миграция обрабатывает изменения схемы, но мы хотим предварительно заполнить столбец **Abstract** первыми 100 символами содержимого для каждой записи. Для этого мы можем перейти к SQL и выполнить оператор **UPDATE** после добавления столбца.
    (Добавление в строке 12 в приведенном ниже коде)

``` csharp
    namespace MigrationsDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddPostAbstract : DbMigration
        {
            public override void Up()
            {
                AddColumn("dbo.Posts", "Abstract", c => c.String());

                Sql("UPDATE dbo.Posts SET Abstract = LEFT(Content, 100) WHERE Abstract IS NULL");
            }

            public override void Down()
            {
                DropColumn("dbo.Posts", "Abstract");
            }
        }
    }
```

Измененная миграция выглядит прекрасно, так что давайте обновим ее с помощью команды **Update-Database**. Необходимо указать флаг **–Verbose**, чтобы видеть выполнение SQL в базе данных.

-   Запустите команду **Update-Database –Verbose** в консоли диспетчера пакетов.

## <a name="migrate-to-a-specific-version-including-downgrade"></a>Перенос в определенную версию (в том числе более раннюю)

Пока мы всегда выполняли обновление до последней миграции, но иногда могут возникнуть ситуации, когда вы хотите перейти к более поздней или более ранней версии миграции.

Предположим, мы хотим перенести базу данных в состояние до запуска миграции **AddBlogUrl**. Мы можем использовать переключатель **–TargetMigration**, чтобы перейти на эту более раннюю версию миграции.

-   Запустите команду **Update-Database –TargetMigration: AddBlogUrl** в консоли диспетчера пакетов.

Эта команда запустит скрипт понижения версии для миграций **AddBlogAbstract** и **AddPostClass**.

Если вы хотите выполнить откат до пустой базы данных, используйте команду **Update-Database –TargetMigration: $InitialDatabase**.

## <a name="getting-a-sql-script"></a>Получение скрипта SQL

Если другой разработчик хочет использовать эти изменения на своем компьютере, он может просто синхронизироваться, когда мы вернем наши изменения в систему управления версиями. Когда он получит наши новые миграции, он может выполнить команду Update-Database, чтобы применить эти изменения локально. Но если мы хотим передать эти изменения на тестовый сервер, а затем и в рабочую среду, возможно, нам потребуется скрипт SQL, который мы сможем передать нашему администратору базы данных.

-   Запустите команду **Update-Database**, но на этот раз укажите флаг **–Script**, чтобы записать изменения в скрипт, а не применить. Мы также укажем исходную и целевую миграцию, для которой необходимо создать скрипт. Мы хотим, чтобы скрипт переходил от пустой базы данных (**$InitialDatabase**) до последней версии (миграция **AddPostAbstract**).
    *Если вы не укажете целевую миграцию, Code First Migrations будет использовать последнюю версию миграции в качестве целевого объекта. Если вы не укажете исходную миграцию, Code First Migrations будет использовать текущее состояние базы данных.*
-   Запустите команду **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: AddPostAbstract** в консоли диспетчера пакетов.

Code First Migrations будет выполнять конвейер миграций, но будет не применять изменения, а записывать их в SQL-файл. Созданный скрипт автоматически открывается в Visual Studio, готовый для просмотра или сохранения.

### <a name="generating-idempotent-scripts"></a>Создание идемпотентных скриптов

Начиная с EF6, если вы укажете **–SourceMigration $InitialDatabase**, создаваемый скрипт будет идемпотентным. Идемпотентные скрипты могут обновить базу данных любой версии до последней версии (или указанной версии, если вы используете **–TargetMigration**). Созданный скрипт содержит логику для проверки таблицы **\_\_MigrationsHistory** и применяет только изменения, которые еще не применены.

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a>Автоматическое обновление при запуске приложения (инициализатор MigrateDatabaseToLatestVersion)

Когда вы развертываете приложение, вы можете настроить автоматическое обновление базы данных (путем применения ожидающих миграций) при запуске приложения. Это можно сделать с помощью регистрации инициализатора базы данных **MigrateDatabaseToLatestVersion**. Инициализатор базы данных просто содержит определенную логику, которая используется для контроля правильной настройки базы данных. Эта логика выполняется при первом использовании контекста в процессе приложения (**AppDomain**).

Мы можем обновить файл **Program.cs**, как показано ниже, чтобы настроить инициализатор **MigrateDatabaseToLatestVersion** для BlogContext, прежде чем мы используем контекст (строка 14). Обратите внимание, что необходимо также добавить оператор using для пространства имен **System.Data.Entity** (строка 5).

*При создании экземпляра этого инициализатора нам нужно указать тип контекста (**BlogContext**) и конфигурацию миграций (**Configuration**). Конфигурация миграций — это класс, который был добавлен в нашу папку **Migrations** при включении Migrations.*

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Data.Entity;
    using MigrationsDemo.Migrations;

    namespace MigrationsDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                Database.SetInitializer(new MigrateDatabaseToLatestVersion\<BlogContext, Configuration>());

                using (var db = new BlogContext())
                {
                    db.Blogs.Add(new Blog { Name = "Another Blog " });
                    db.SaveChanges();

                    foreach (var blog in db.Blogs)
                    {
                        Console.WriteLine(blog.Name);
                    }
                }

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }
        }
    }
```

Теперь при каждом запуске приложение сначала будет проверять актуальность целевой базы данных и применять незавершенные миграции, если это необходимо.
