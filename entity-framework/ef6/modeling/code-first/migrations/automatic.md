---
title: Автоматическое Code First Migrations - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
caps.latest.revision: 3
ms.openlocfilehash: 1f6ed728271e38d8e65fcf33fd45c74f085d9178
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122273"
---
# <a name="automatic-code-first-migrations"></a><span data-ttu-id="5e57b-102">Автоматическое Code First Migrations</span><span class="sxs-lookup"><span data-stu-id="5e57b-102">Automatic Code First Migrations</span></span>
<span data-ttu-id="5e57b-103">Автоматический перенос позволяет использовать Code First Migrations без файл кода в проекте для каждого изменения, внесенные.</span><span class="sxs-lookup"><span data-stu-id="5e57b-103">Automatic Migrations allows you to use Code First Migrations without having a code file in your project for each change you make.</span></span> <span data-ttu-id="5e57b-104">Не все изменения, которые могут применяться автоматически — например переименование столбцов требуют использования миграции на основе кода.</span><span class="sxs-lookup"><span data-stu-id="5e57b-104">Not all changes can be applied automatically - for example column renames require the use of a code-based migration.</span></span>

> [!NOTE]
> <span data-ttu-id="5e57b-105">В этой статье предполагается, что вы знаете, как использовать Code First Migrations в основных сценариях.</span><span class="sxs-lookup"><span data-stu-id="5e57b-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="5e57b-106">Если этого не сделать, то нужно будет прочесть [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) перед продолжением.</span><span class="sxs-lookup"><span data-stu-id="5e57b-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="recommendation-for-team-environments"></a><span data-ttu-id="5e57b-107">Рекомендации для сред Team</span><span class="sxs-lookup"><span data-stu-id="5e57b-107">Recommendation for Team Environments</span></span>

<span data-ttu-id="5e57b-108">Вы можете перемежать миграций автоматическое и на основе кода, но это не рекомендуется при коллективной разработки.</span><span class="sxs-lookup"><span data-stu-id="5e57b-108">You can intersperse automatic and code-based migrations but this is not recommended in team development scenarios.</span></span> <span data-ttu-id="5e57b-109">Если вы работаете в команде разработчиков, использующих систему управления версиями следует использовать исключительно автоматической миграции или миграции исключительно на основе кода.</span><span class="sxs-lookup"><span data-stu-id="5e57b-109">If you are part of a team of developers that use source control you should either use purely automatic migrations or purely code-based migrations.</span></span> <span data-ttu-id="5e57b-110">Учитывая ограничения автоматический перенос рекомендуется использовать миграция на базе кода в командных средах.</span><span class="sxs-lookup"><span data-stu-id="5e57b-110">Given the limitations of automatic migrations we recommend using code-based migrations in team environments.</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="5e57b-111">Привязка начальной модели и базы данных</span><span class="sxs-lookup"><span data-stu-id="5e57b-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="5e57b-112">Прежде чем мы начнем использовать миграции, нам потребуется проект и модель Code First, с которой мы будем работать.</span><span class="sxs-lookup"><span data-stu-id="5e57b-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="5e57b-113">В этом пошаговом руководстве мы собираемся использовать каноническую модель **Blog** и **Post**.</span><span class="sxs-lookup"><span data-stu-id="5e57b-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="5e57b-114">Создайте новый **MigrationsAutomaticDemo** консольное приложение</span><span class="sxs-lookup"><span data-stu-id="5e57b-114">Create a new **MigrationsAutomaticDemo** Console application</span></span>
-   <span data-ttu-id="5e57b-115">Добавьте последнюю версию пакета NuGet **EntityFramework** в проект.</span><span class="sxs-lookup"><span data-stu-id="5e57b-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="5e57b-116">**Средства —&gt; Диспетчер пакетов библиотеки —&gt; Консоль диспетчера пакетов**</span><span class="sxs-lookup"><span data-stu-id="5e57b-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="5e57b-117">Запустите команду **EntityFramework Install-Package**</span><span class="sxs-lookup"><span data-stu-id="5e57b-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="5e57b-118">Добавьте файл **Model.cs** с кодом, показанным ниже.</span><span class="sxs-lookup"><span data-stu-id="5e57b-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="5e57b-119">Этот код определяет один класс **Blog**, составляющий нашу модель предметной области, и класс **BlogContext**, который будет нашим контекстом EF Code First.</span><span class="sxs-lookup"><span data-stu-id="5e57b-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

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

-   <span data-ttu-id="5e57b-120">Теперь, когда у нас есть модель, пора использовать ее для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="5e57b-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="5e57b-121">Внесите в файл **Program.cs** приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="5e57b-121">Update the **Program.cs** file with the code shown below.</span></span>

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

-   <span data-ttu-id="5e57b-122">Запустите приложение и вы увидите, что **MigrationsAutomaticCodeDemo.BlogContext** база данных создается автоматически.</span><span class="sxs-lookup"><span data-stu-id="5e57b-122">Run your application and you will see that a **MigrationsAutomaticCodeDemo.BlogContext** database is created for you.</span></span>

    ![DatabaseLocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="5e57b-124">Включение миграций</span><span class="sxs-lookup"><span data-stu-id="5e57b-124">Enabling Migrations</span></span>

<span data-ttu-id="5e57b-125">Пора внести дополнительные изменения в нашу модель.</span><span class="sxs-lookup"><span data-stu-id="5e57b-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="5e57b-126">Давайте внесем в класс Blog свойство Url.</span><span class="sxs-lookup"><span data-stu-id="5e57b-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="5e57b-127">Если бы вы запустили приложение снова, оно выдало бы исключение InvalidOperationException с сообщением: *Модель, поддерживающая контекст BlogContext, изменилась с момента создания базы данных. Попробуйте обновить базу данных с помощью Code First Migrations (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](http://go.microsoft.com/fwlink/?LinkId=238269)*).*</span><span class="sxs-lookup"><span data-stu-id="5e57b-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](http://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="5e57b-128">Как видно из исключения, пора приступить к использованию Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="5e57b-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="5e57b-129">Поскольку мы хотим использовать автоматический перенос мы собираемся укажите **— EnableAutomaticMigrations** переключения.</span><span class="sxs-lookup"><span data-stu-id="5e57b-129">Because we want to use automatic migrations we’re going to specify the **–EnableAutomaticMigrations** switch.</span></span>

-   <span data-ttu-id="5e57b-130">Запустите **Enable-Migrations — EnableAutomaticMigrations** добавленные команды в консоли диспетчера пакетов эту команду **миграций** папку для нашего проекта.</span><span class="sxs-lookup"><span data-stu-id="5e57b-130">Run the **Enable-Migrations –EnableAutomaticMigrations** command in Package Manager Console This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="5e57b-131">В этой новой папке содержит один файл:</span><span class="sxs-lookup"><span data-stu-id="5e57b-131">This new folder contains one file:</span></span>

-   <span data-ttu-id="5e57b-132">**Класс конфигурации.**</span><span class="sxs-lookup"><span data-stu-id="5e57b-132">**The Configuration class.**</span></span> <span data-ttu-id="5e57b-133">Этот класс позволяет настраивать поведение миграций для контекста.</span><span class="sxs-lookup"><span data-stu-id="5e57b-133">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="5e57b-134">В этом пошаговом руководстве мы будем просто использовать конфигурацию по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5e57b-134">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="5e57b-135">*Поскольку имеется только один контекст Code First в проекте, Enable-Migrations автоматически заполняет тип контекста, к которому относится эта конфигурация.*</span><span class="sxs-lookup"><span data-stu-id="5e57b-135">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>

 

## <a name="your-first-automatic-migration"></a><span data-ttu-id="5e57b-136">Первый автоматического переход</span><span class="sxs-lookup"><span data-stu-id="5e57b-136">Your First Automatic Migration</span></span>

<span data-ttu-id="5e57b-137">Вам нужно знать о двух основных командах Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="5e57b-137">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="5e57b-138">**Add-Migration** будет автоматически формировать следующую миграцию на основе изменений, внесенных в модель с момента создания последней миграции.</span><span class="sxs-lookup"><span data-stu-id="5e57b-138">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="5e57b-139">**Update-Database** будет применять ожидающие обработки миграции к базе данных.</span><span class="sxs-lookup"><span data-stu-id="5e57b-139">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="5e57b-140">Мы хотим избежать с помощью Add-Migration (если не нужно в действительности) и сосредоточиться на возможности Code First Migrations автоматически вычислить и применить изменения.</span><span class="sxs-lookup"><span data-stu-id="5e57b-140">We are going to avoid using Add-Migration (unless we really need to) and focus on letting Code First Migrations automatically calculate and apply the changes.</span></span> <span data-ttu-id="5e57b-141">Мы используем **Update-Database** для получения Code First Migrations для отправьте изменения в нашей модели (новый **Blog.Ur**свойство l) к базе данных.</span><span class="sxs-lookup"><span data-stu-id="5e57b-141">Let’s use **Update-Database** to get Code First Migrations to push the changes to our model (the new **Blog.Ur**l property) to the database.</span></span>

-   <span data-ttu-id="5e57b-142">Запустите **Update-Database** команду в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="5e57b-142">Run the **Update-Database** command in Package Manager Console.</span></span>

<span data-ttu-id="5e57b-143">**MigrationsAutomaticDemo.BlogContext** базы данных теперь обновляется для включения **URL-адрес** столбца в **блоги** таблицы.</span><span class="sxs-lookup"><span data-stu-id="5e57b-143">The **MigrationsAutomaticDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

 

## <a name="your-second-automatic-migration"></a><span data-ttu-id="5e57b-144">Второй автоматического переход</span><span class="sxs-lookup"><span data-stu-id="5e57b-144">Your Second Automatic Migration</span></span>

<span data-ttu-id="5e57b-145">Давайте создадим другой изменения и позволить Code First Migrations автоматически отправьте изменения в базе данных для нас.</span><span class="sxs-lookup"><span data-stu-id="5e57b-145">Let’s make another change and let Code First Migrations automatically push the changes to the database for us.</span></span>

-   <span data-ttu-id="5e57b-146">Также добавим новый класс **Post**.</span><span class="sxs-lookup"><span data-stu-id="5e57b-146">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="5e57b-147">Еще мы добавим коллекцию **Posts** в класс **Blog**, чтобы создать второй конец связи между **Blog** и **Post**.</span><span class="sxs-lookup"><span data-stu-id="5e57b-147">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="5e57b-148">Теперь с помощью **Update-Database** перевести базу данных в актуальном состоянии.</span><span class="sxs-lookup"><span data-stu-id="5e57b-148">Now use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="5e57b-149">Давайте укажем флаг **–Verbose**, чтобы вы могли видеть SQL, который выполняет Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="5e57b-149">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="5e57b-150">Запустите команду **Update-Database –Verbose** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="5e57b-150">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="adding-a-code-based-migration"></a><span data-ttu-id="5e57b-151">Добавление кода на основе миграции</span><span class="sxs-lookup"><span data-stu-id="5e57b-151">Adding a Code Based Migration</span></span>

<span data-ttu-id="5e57b-152">Теперь давайте взглянем на что-то, возможно, мы хотим использовать миграции на основе кода для.</span><span class="sxs-lookup"><span data-stu-id="5e57b-152">Now let’s look at something we might want to use a code-based migration for.</span></span>

-   <span data-ttu-id="5e57b-153">Давайте добавим **Оценка** свойства **блог** класса</span><span class="sxs-lookup"><span data-stu-id="5e57b-153">Let’s add a **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

<span data-ttu-id="5e57b-154">Мы просто выполнить **Update-Database** отправлять эти изменения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="5e57b-154">We could just run **Update-Database** to push these changes to the database.</span></span> <span data-ttu-id="5e57b-155">Тем не менее, мы добавляем не допускающим **Blogs.Rating** столбца, если все существующие данные в таблице он будет назначаются по умолчанию CLR типа данных для нового столбца (оценка — целое число, это будет **0**).</span><span class="sxs-lookup"><span data-stu-id="5e57b-155">However, we're adding a non-nullable **Blogs.Rating** column, if there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="5e57b-156">Но нам нужно указать значение по умолчанию **3**, чтобы существующие строки в таблице **Blogs** начинались с неплохой оценки.</span><span class="sxs-lookup"><span data-stu-id="5e57b-156">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
<span data-ttu-id="5e57b-157">Для записи этого изменения out для миграции на основе кода, таким образом, чтобы его можно изменить мы используем команду Add-Migration.</span><span class="sxs-lookup"><span data-stu-id="5e57b-157">Let’s use the Add-Migration command to write this change out to a code-based migration so that we can edit it.</span></span> <span data-ttu-id="5e57b-158">**Add-Migration** команда позволяет нам назовите эти переносы, давайте просто вызовите наши **AddBlogRating**.</span><span class="sxs-lookup"><span data-stu-id="5e57b-158">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogRating**.</span></span>

-   <span data-ttu-id="5e57b-159">Запустите **AddBlogRating Add-Migration** команду в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="5e57b-159">Run the **Add-Migration AddBlogRating** command in Package Manager Console.</span></span>
-   <span data-ttu-id="5e57b-160">В **миграций** папки теперь у нас есть новый **AddBlogRating** миграции.</span><span class="sxs-lookup"><span data-stu-id="5e57b-160">In the **Migrations** folder we now have a new **AddBlogRating** migration.</span></span> <span data-ttu-id="5e57b-161">Имя файла миграции предварительно исправлена с меткой времени для облегчения упорядочения.</span><span class="sxs-lookup"><span data-stu-id="5e57b-161">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="5e57b-162">Давайте изменить созданный код, чтобы указать значение по умолчанию 3 для Blog.Rating (строка 10 в приведенном ниже коде)</span><span class="sxs-lookup"><span data-stu-id="5e57b-162">Let’s edit the generated code to specify a default value of 3 for Blog.Rating (Line 10 in the code below)</span></span>

<span data-ttu-id="5e57b-163">*Миграции также содержит файл кода, в котором регистрируются некоторые метаданные. Эти метаданные позволит Code First Migrations для репликации автоматический перенос, которые мы выполнили перед этой миграции на основе кода. Это важно в том случае, если другой разработчик хочет выполните наши миграции, или когда придет время, чтобы развернуть наше приложение.*</span><span class="sxs-lookup"><span data-stu-id="5e57b-163">*The migration also has a code-behind file that captures some metadata. This metadata will allow Code First Migrations to replicate the automatic migrations we performed before this code-based migration. This is important if another developer wants to run our migrations or when it’s time to deploy our application.*</span></span>

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

<span data-ttu-id="5e57b-164">Измененная миграция выглядит прекрасно, так что давайте обновим ее с помощью команды **Update-Database**.</span><span class="sxs-lookup"><span data-stu-id="5e57b-164">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span>

-   <span data-ttu-id="5e57b-165">Запустите **Update-Database** команду в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="5e57b-165">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="back-to-automatic-migrations"></a><span data-ttu-id="5e57b-166">К автоматический перенос</span><span class="sxs-lookup"><span data-stu-id="5e57b-166">Back to Automatic Migrations</span></span>

<span data-ttu-id="5e57b-167">Теперь мы бесплатны переключиться обратно на автоматический перенос для нашей простой изменения.</span><span class="sxs-lookup"><span data-stu-id="5e57b-167">We are now free to switch back to automatic migrations for our simpler changes.</span></span> <span data-ttu-id="5e57b-168">Code First Migrations позаботится о выполнении миграции автоматическое и на основе кода в правильном порядке на основе метаданных, хранящиеся в файле кода для каждого миграции на основе кода.</span><span class="sxs-lookup"><span data-stu-id="5e57b-168">Code First Migrations will take care of performing the automatic and code-based migrations in the correct order based on the metadata it is storing in the code-behind file for each code-based migration.</span></span>

-   <span data-ttu-id="5e57b-169">Давайте добавим свойство Post.Abstract для нашей модели</span><span class="sxs-lookup"><span data-stu-id="5e57b-169">Let’s add a Post.Abstract property to our model</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="5e57b-170">Теперь мы можем использовать **Update-Database** для получения Code First Migrations, чтобы передать изменения в базу данных, с помощью автоматической миграции.</span><span class="sxs-lookup"><span data-stu-id="5e57b-170">Now we can use **Update-Database** to get Code First Migrations to push this change to the database using an automatic migration.</span></span>

-   <span data-ttu-id="5e57b-171">Запустите **Update-Database** команду в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="5e57b-171">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="summary"></a><span data-ttu-id="5e57b-172">Сводка</span><span class="sxs-lookup"><span data-stu-id="5e57b-172">Summary</span></span>

<span data-ttu-id="5e57b-173">В этом пошаговом руководстве вы узнали, как использовать автоматический перенос для передачи в базу данных изменения модели.</span><span class="sxs-lookup"><span data-stu-id="5e57b-173">In this walkthrough you saw how to use automatic migrations to push model changes to the database.</span></span> <span data-ttu-id="5e57b-174">Вы также узнали, как сформировать шаблон и выполнения миграции на основе кода между автоматический перенос в том случае, когда требуется более строгий контроль.</span><span class="sxs-lookup"><span data-stu-id="5e57b-174">You also saw how to scaffold and run code-based migrations in between automatic migrations when you need more control.</span></span>
