---
title: Автоматическое Code First Migrations - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
ms.openlocfilehash: f358a4df04b03399e9e54ffdf0389e43d715af1c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996099"
---
# <a name="automatic-code-first-migrations"></a>Автоматическое Code First Migrations
Автоматический перенос позволяет использовать Code First Migrations без файл кода в проекте для каждого изменения, внесенные. Не все изменения, которые могут применяться автоматически — например переименование столбцов требуют использования миграции на основе кода.

> [!NOTE]
> В этой статье предполагается, что вы знаете, как использовать Code First Migrations в основных сценариях. Если этого не сделать, то нужно будет прочесть [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) перед продолжением.

## <a name="recommendation-for-team-environments"></a>Рекомендации для сред Team

Вы можете перемежать миграций автоматическое и на основе кода, но это не рекомендуется при коллективной разработки. Если вы работаете в команде разработчиков, использующих систему управления версиями следует использовать исключительно автоматической миграции или миграции исключительно на основе кода. Учитывая ограничения автоматический перенос рекомендуется использовать миграция на базе кода в командных средах.

## <a name="building-an-initial-model--database"></a>Привязка начальной модели и базы данных

Прежде чем мы начнем использовать миграции, нам потребуется проект и модель Code First, с которой мы будем работать. В этом пошаговом руководстве мы собираемся использовать каноническую модель **Blog** и **Post**.

-   Создайте новый **MigrationsAutomaticDemo** консольное приложение
-   Добавьте последнюю версию пакета NuGet **EntityFramework** в проект.
    -   **Средства —&gt; Диспетчер пакетов библиотеки —&gt; Консоль диспетчера пакетов**
    -   Запустите команду **EntityFramework Install-Package**
-   Добавьте файл **Model.cs** с кодом, показанным ниже. Этот код определяет один класс **Blog**, составляющий нашу модель предметной области, и класс **BlogContext**, который будет нашим контекстом EF Code First.

  ``` csharp
      using System.Data.Entity;
      using System.Collections.Generic;
      using System.ComponentModel.DataAnnotations;
      using System.Data.Entity.Infrastructure;

      namespace MigrationsAutomaticDemo
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

      namespace MigrationsAutomaticDemo
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

-   Запустите приложение и вы увидите, что **MigrationsAutomaticCodeDemo.BlogContext** база данных создается автоматически.

    ![DatabaseLocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Включение миграций

Пора внести дополнительные изменения в нашу модель.

-   Давайте внесем в класс Blog свойство Url.

``` csharp
    public string Url { get; set; }
```

Если бы вы запустили приложение снова, оно выдало бы исключение InvalidOperationException с сообщением: *Модель, поддерживающая контекст BlogContext, изменилась с момента создания базы данных. Попробуйте обновить базу данных с помощью Code First Migrations (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](http://go.microsoft.com/fwlink/?LinkId=238269)*).*

Как видно из исключения, пора приступить к использованию Code First Migrations. Поскольку мы хотим использовать автоматический перенос мы собираемся укажите **— EnableAutomaticMigrations** переключения.

-   Запустите **Enable-Migrations — EnableAutomaticMigrations** добавленные команды в консоли диспетчера пакетов эту команду **миграций** папку для нашего проекта. В этой новой папке содержит один файл:

-   **Класс конфигурации.** Этот класс позволяет настраивать поведение миграций для контекста. В этом пошаговом руководстве мы будем просто использовать конфигурацию по умолчанию.
    *Поскольку имеется только один контекст Code First в проекте, Enable-Migrations автоматически заполняет тип контекста, к которому относится эта конфигурация.*

 

## <a name="your-first-automatic-migration"></a>Первый автоматического переход

Вам нужно знать о двух основных командах Code First Migrations.

-   **Add-Migration** будет автоматически формировать следующую миграцию на основе изменений, внесенных в модель с момента создания последней миграции.
-   **Update-Database** будет применять ожидающие обработки миграции к базе данных.

Мы хотим избежать с помощью Add-Migration (если не нужно в действительности) и сосредоточиться на возможности Code First Migrations автоматически вычислить и применить изменения. Мы используем **Update-Database** для получения Code First Migrations для отправьте изменения в нашей модели (новый **Blog.Ur**свойство l) к базе данных.

-   Запустите **Update-Database** команду в консоли диспетчера пакетов.

**MigrationsAutomaticDemo.BlogContext** базы данных теперь обновляется для включения **URL-адрес** столбца в **блоги** таблицы.

 

## <a name="your-second-automatic-migration"></a>Второй автоматического переход

Давайте создадим другой изменения и позволить Code First Migrations автоматически отправьте изменения в базе данных для нас.

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

Теперь с помощью **Update-Database** перевести базу данных в актуальном состоянии. Давайте укажем флаг **–Verbose**, чтобы вы могли видеть SQL, который выполняет Code First Migrations.

-   Запустите команду **Update-Database –Verbose** в консоли диспетчера пакетов.

## <a name="adding-a-code-based-migration"></a>Добавление кода на основе миграции

Теперь давайте взглянем на что-то, возможно, мы хотим использовать миграции на основе кода для.

-   Давайте добавим **Оценка** свойства **блог** класса

``` csharp
    public int Rating { get; set; }
```

Мы просто выполнить **Update-Database** отправлять эти изменения к базе данных. Тем не менее, мы добавляем не допускающим **Blogs.Rating** столбца, если все существующие данные в таблице он будет назначаются по умолчанию CLR типа данных для нового столбца (оценка — целое число, это будет **0**). Но нам нужно указать значение по умолчанию **3**, чтобы существующие строки в таблице **Blogs** начинались с неплохой оценки.
Для записи этого изменения out для миграции на основе кода, таким образом, чтобы его можно изменить мы используем команду Add-Migration. **Add-Migration** команда позволяет нам назовите эти переносы, давайте просто вызовите наши **AddBlogRating**.

-   Запустите **AddBlogRating Add-Migration** команду в консоли диспетчера пакетов.
-   В **миграций** папки теперь у нас есть новый **AddBlogRating** миграции. Имя файла миграции предварительно исправлена с меткой времени для облегчения упорядочения. Давайте изменить созданный код, чтобы указать значение по умолчанию 3 для Blog.Rating (строка 10 в приведенном ниже коде)

*Миграции также содержит файл кода, в котором регистрируются некоторые метаданные. Эти метаданные позволит Code First Migrations для репликации автоматический перенос, которые мы выполнили перед этой миграции на основе кода. Это важно в том случае, если другой разработчик хочет выполните наши миграции, или когда придет время, чтобы развернуть наше приложение.*

``` csharp
    namespace MigrationsAutomaticDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddBlogRating : DbMigration
        {
            public override void Up()
            {
                AddColumn("Blogs", "Rating", c => c.Int(nullable: false, defaultValue: 3));
            }

            public override void Down()
            {
                DropColumn("Blogs", "Rating");
            }
        }
    }
```

Измененная миграция выглядит прекрасно, так что давайте обновим ее с помощью команды **Update-Database**.

-   Запустите **Update-Database** команду в консоли диспетчера пакетов.

## <a name="back-to-automatic-migrations"></a>К автоматический перенос

Теперь мы бесплатны переключиться обратно на автоматический перенос для нашей простой изменения. Code First Migrations позаботится о выполнении миграции автоматическое и на основе кода в правильном порядке на основе метаданных, хранящиеся в файле кода для каждого миграции на основе кода.

-   Давайте добавим свойство Post.Abstract для нашей модели

``` csharp
    public string Abstract { get; set; }
```

Теперь мы можем использовать **Update-Database** для получения Code First Migrations, чтобы передать изменения в базу данных, с помощью автоматической миграции.

-   Запустите **Update-Database** команду в консоли диспетчера пакетов.

## <a name="summary"></a>Сводка

В этом пошаговом руководстве вы узнали, как использовать автоматический перенос для передачи в базу данных изменения модели. Вы также узнали, как сформировать шаблон и выполнения миграции на основе кода между автоматический перенос в том случае, когда требуется более строгий контроль.
