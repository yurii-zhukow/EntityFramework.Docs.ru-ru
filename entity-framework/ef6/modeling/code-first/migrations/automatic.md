---
title: Автоматическая Code First Migrations-EF6
description: Автоматическое Code First Migrations в Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
uid: ef6/modeling/code-first/migrations/automatic
ms.openlocfilehash: 541adf5aed517685b54d675730996c12ff0c824a
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89618084"
---
# <a name="automatic-code-first-migrations"></a><span data-ttu-id="ba159-103">Автоматическое Code First Migrations</span><span class="sxs-lookup"><span data-stu-id="ba159-103">Automatic Code First Migrations</span></span>
<span data-ttu-id="ba159-104">Автоматическая миграция позволяет использовать Code First Migrations без файла кода в проекте для каждого внесенного изменения.</span><span class="sxs-lookup"><span data-stu-id="ba159-104">Automatic Migrations allows you to use Code First Migrations without having a code file in your project for each change you make.</span></span> <span data-ttu-id="ba159-105">Не все изменения могут быть применены автоматически. Например, для переименования столбцов требуется использовать перенос на основе кода.</span><span class="sxs-lookup"><span data-stu-id="ba159-105">Not all changes can be applied automatically - for example column renames require the use of a code-based migration.</span></span>

> [!NOTE]
> <span data-ttu-id="ba159-106">В этой статье предполагается, что вы умеете использовать Code First Migrations в основных сценариях.</span><span class="sxs-lookup"><span data-stu-id="ba159-106">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="ba159-107">В противном случае необходимо прочитать [Code First migrations](xref:ef6/modeling/code-first/migrations/index) , прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="ba159-107">If you don’t, then you’ll need to read [Code First Migrations](xref:ef6/modeling/code-first/migrations/index) before continuing.</span></span>

## <a name="recommendation-for-team-environments"></a><span data-ttu-id="ba159-108">Рекомендации для сред группы</span><span class="sxs-lookup"><span data-stu-id="ba159-108">Recommendation for Team Environments</span></span>

<span data-ttu-id="ba159-109">Можно использовать автоматическую миграцию и перенос на основе кода, но это не рекомендуется делать в сценариях групповой разработки.</span><span class="sxs-lookup"><span data-stu-id="ba159-109">You can intersperse automatic and code-based migrations but this is not recommended in team development scenarios.</span></span> <span data-ttu-id="ba159-110">Если вы являетесь членом группы разработчиков, использующих систему управления версиями, следует использовать исключительно автоматическую миграцию или только переносы на основе кода.</span><span class="sxs-lookup"><span data-stu-id="ba159-110">If you are part of a team of developers that use source control you should either use purely automatic migrations or purely code-based migrations.</span></span> <span data-ttu-id="ba159-111">Учитывая ограничения автоматической миграции, мы рекомендуем использовать миграцию на основе кода в средах группы.</span><span class="sxs-lookup"><span data-stu-id="ba159-111">Given the limitations of automatic migrations we recommend using code-based migrations in team environments.</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="ba159-112">Привязка начальной модели и базы данных</span><span class="sxs-lookup"><span data-stu-id="ba159-112">Building an Initial Model & Database</span></span>

<span data-ttu-id="ba159-113">Прежде чем мы начнем использовать миграции, нам потребуется проект и модель Code First, с которой мы будем работать.</span><span class="sxs-lookup"><span data-stu-id="ba159-113">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="ba159-114">В этом пошаговом руководстве мы собираемся использовать каноническую модель **Blog** и **Post**.</span><span class="sxs-lookup"><span data-stu-id="ba159-114">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="ba159-115">Создание нового консольного приложения **мигратионсаутоматикдемо**</span><span class="sxs-lookup"><span data-stu-id="ba159-115">Create a new **MigrationsAutomaticDemo** Console application</span></span>
-   <span data-ttu-id="ba159-116">Добавьте последнюю версию пакета NuGet **EntityFramework** в проект.</span><span class="sxs-lookup"><span data-stu-id="ba159-116">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="ba159-117">**Средства —&gt; Диспетчер пакетов библиотеки —&gt; Консоль диспетчера пакетов**</span><span class="sxs-lookup"><span data-stu-id="ba159-117">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="ba159-118">Запустите команду **EntityFramework Install-Package**</span><span class="sxs-lookup"><span data-stu-id="ba159-118">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="ba159-119">Добавьте файл **Model.cs** с кодом, показанным ниже.</span><span class="sxs-lookup"><span data-stu-id="ba159-119">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="ba159-120">Этот код определяет один класс **Blog**, составляющий нашу модель предметной области, и класс **BlogContext**, который будет нашим контекстом EF Code First.</span><span class="sxs-lookup"><span data-stu-id="ba159-120">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

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

-   <span data-ttu-id="ba159-121">Теперь, когда у нас есть модель, пора использовать ее для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="ba159-121">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="ba159-122">Внесите в файл **Program.cs** приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="ba159-122">Update the **Program.cs** file with the code shown below.</span></span>

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

-   <span data-ttu-id="ba159-123">Запустите приложение, и вы увидите, что база данных **мигратионсаутоматиккодедемо. BlogContext** создана.</span><span class="sxs-lookup"><span data-stu-id="ba159-123">Run your application and you will see that a **MigrationsAutomaticCodeDemo.BlogContext** database is created for you.</span></span>

    ![LocalDB базы данных](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="ba159-125">Включение миграций</span><span class="sxs-lookup"><span data-stu-id="ba159-125">Enabling Migrations</span></span>

<span data-ttu-id="ba159-126">Пора внести дополнительные изменения в нашу модель.</span><span class="sxs-lookup"><span data-stu-id="ba159-126">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="ba159-127">Давайте внесем в класс Blog свойство Url.</span><span class="sxs-lookup"><span data-stu-id="ba159-127">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="ba159-128">Если бы вы выполняли приложение еще раз, вы получите исключение InvalidOperationException, сообщающее *, что модель резервного копирования контекста "BlogContext" изменилась с момента создания базы данных. Для обновления базы данных () рекомендуется использовать Code First Migrations* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269) *.*</span><span class="sxs-lookup"><span data-stu-id="ba159-128">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="ba159-129">Как видно из исключения, пора приступить к использованию Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="ba159-129">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="ba159-130">Поскольку мы хотим использовать автоматические миграции, мы будем указывать параметр **– енаблеаутоматикмигратионс** .</span><span class="sxs-lookup"><span data-stu-id="ba159-130">Because we want to use automatic migrations we’re going to specify the **–EnableAutomaticMigrations** switch.</span></span>

-   <span data-ttu-id="ba159-131">Выполните команду **Enable-migrations – енаблеаутоматикмигратионс** в консоли диспетчера пакетов. Эта команда добавила в наш проект папку **migrations** .</span><span class="sxs-lookup"><span data-stu-id="ba159-131">Run the **Enable-Migrations –EnableAutomaticMigrations** command in Package Manager Console This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="ba159-132">Эта новая папка содержит один файл:</span><span class="sxs-lookup"><span data-stu-id="ba159-132">This new folder contains one file:</span></span>

-   <span data-ttu-id="ba159-133">**Класс конфигурации.**</span><span class="sxs-lookup"><span data-stu-id="ba159-133">**The Configuration class.**</span></span> <span data-ttu-id="ba159-134">Этот класс позволяет настраивать поведение миграций для контекста.</span><span class="sxs-lookup"><span data-stu-id="ba159-134">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="ba159-135">В этом пошаговом руководстве мы будем просто использовать конфигурацию по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="ba159-135">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="ba159-136">*Поскольку имеется только один контекст Code First в проекте, Enable-Migrations автоматически заполняет тип контекста, к которому относится эта конфигурация.*</span><span class="sxs-lookup"><span data-stu-id="ba159-136">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>

 

## <a name="your-first-automatic-migration"></a><span data-ttu-id="ba159-137">Первая автоматическая миграция</span><span class="sxs-lookup"><span data-stu-id="ba159-137">Your First Automatic Migration</span></span>

<span data-ttu-id="ba159-138">Вам нужно знать о двух основных командах Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="ba159-138">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="ba159-139">**Add-Migration** будет автоматически формировать следующую миграцию на основе изменений, внесенных в модель с момента создания последней миграции.</span><span class="sxs-lookup"><span data-stu-id="ba159-139">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="ba159-140">**Update-Database** будет применять ожидающие обработки миграции к базе данных.</span><span class="sxs-lookup"><span data-stu-id="ba159-140">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="ba159-141">Мы будем избегать использования добавления-миграции (если это не требуется) и сосредоточиться на том, чтобы Code First Migrations автоматически вычислять и применять изменения.</span><span class="sxs-lookup"><span data-stu-id="ba159-141">We are going to avoid using Add-Migration (unless we really need to) and focus on letting Code First Migrations automatically calculate and apply the changes.</span></span> <span data-ttu-id="ba159-142">Мы используем **Обновление базы данных** , чтобы получить Code First migrations для отправки изменений в нашу модель (новое свойство **Blog. ur**l) в базу данных.</span><span class="sxs-lookup"><span data-stu-id="ba159-142">Let’s use **Update-Database** to get Code First Migrations to push the changes to our model (the new **Blog.Ur**l property) to the database.</span></span>

-   <span data-ttu-id="ba159-143">Выполните команду **обновления базы данных** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="ba159-143">Run the **Update-Database** command in Package Manager Console.</span></span>

<span data-ttu-id="ba159-144">База данных **мигратионсаутоматикдемо. BlogContext** теперь обновляется для включения столбца **URL-адреса** в таблицу **blogs** .</span><span class="sxs-lookup"><span data-stu-id="ba159-144">The **MigrationsAutomaticDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

 

## <a name="your-second-automatic-migration"></a><span data-ttu-id="ba159-145">Вторая автоматическая миграция</span><span class="sxs-lookup"><span data-stu-id="ba159-145">Your Second Automatic Migration</span></span>

<span data-ttu-id="ba159-146">Сделаем еще одно изменение и позвольте Code First Migrations автоматически отправлять изменения в базу данных для нас.</span><span class="sxs-lookup"><span data-stu-id="ba159-146">Let’s make another change and let Code First Migrations automatically push the changes to the database for us.</span></span>

-   <span data-ttu-id="ba159-147">Также добавим новый класс **Post**.</span><span class="sxs-lookup"><span data-stu-id="ba159-147">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="ba159-148">Еще мы добавим коллекцию **Posts** в класс **Blog**, чтобы создать второй конец связи между **Blog** и **Post**.</span><span class="sxs-lookup"><span data-stu-id="ba159-148">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="ba159-149">Теперь используйте **Обновление базы данных** , чтобы обновить базу данных в актуальном состоянии.</span><span class="sxs-lookup"><span data-stu-id="ba159-149">Now use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="ba159-150">Давайте укажем флаг **–Verbose**, чтобы вы могли видеть SQL, который выполняет Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="ba159-150">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="ba159-151">Запустите команду **Update-Database –Verbose** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="ba159-151">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="adding-a-code-based-migration"></a><span data-ttu-id="ba159-152">Добавление переноса на основе кода</span><span class="sxs-lookup"><span data-stu-id="ba159-152">Adding a Code Based Migration</span></span>

<span data-ttu-id="ba159-153">Теперь давайте взглянем на то, что мы можем использовать для переноса на основе кода.</span><span class="sxs-lookup"><span data-stu-id="ba159-153">Now let’s look at something we might want to use a code-based migration for.</span></span>

-   <span data-ttu-id="ba159-154">Давайте добавим свойство **рейтинга** к классу **Blog** .</span><span class="sxs-lookup"><span data-stu-id="ba159-154">Let’s add a **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

<span data-ttu-id="ba159-155">Для принудительной отправки этих изменений в базу данных можно просто выполнить **Обновление базы данных** .</span><span class="sxs-lookup"><span data-stu-id="ba159-155">We could just run **Update-Database** to push these changes to the database.</span></span> <span data-ttu-id="ba159-156">Тем не менее мы добавляем в столбец, не допускающий значения NULL, в столбце **рейтинг** , если в таблице есть данные, им будет назначено значение CLR по умолчанию для нового столбца (оценка — целое число, то есть значение **0**).</span><span class="sxs-lookup"><span data-stu-id="ba159-156">However, we're adding a non-nullable **Blogs.Rating** column, if there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="ba159-157">Но нам нужно указать значение по умолчанию **3**, чтобы существующие строки в таблице **Blogs** начинались с неплохой оценки.</span><span class="sxs-lookup"><span data-stu-id="ba159-157">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
<span data-ttu-id="ba159-158">Воспользуемся командой Add-Migration, чтобы записать это изменение в перенос на основе кода, чтобы мы могли изменить его.</span><span class="sxs-lookup"><span data-stu-id="ba159-158">Let’s use the Add-Migration command to write this change out to a code-based migration so that we can edit it.</span></span> <span data-ttu-id="ba159-159">С помощью команды **Add-Migration** можно присвоить имя миграции. Давайте просто вызываем наш **аддблогратинг**.</span><span class="sxs-lookup"><span data-stu-id="ba159-159">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogRating**.</span></span>

-   <span data-ttu-id="ba159-160">Выполните команду **Add-Migration аддблогратинг** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="ba159-160">Run the **Add-Migration AddBlogRating** command in Package Manager Console.</span></span>
-   <span data-ttu-id="ba159-161">В папке **migrations** теперь появилась новая **аддблогратинг** миграция.</span><span class="sxs-lookup"><span data-stu-id="ba159-161">In the **Migrations** folder we now have a new **AddBlogRating** migration.</span></span> <span data-ttu-id="ba159-162">Имя файла миграции предварительно исправлено с меткой времени, помогающей в упорядочении.</span><span class="sxs-lookup"><span data-stu-id="ba159-162">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="ba159-163">Давайте изменим созданный код, указав значение по умолчанию 3 для блога. рейтинг (строка 10 в приведенном ниже коде).</span><span class="sxs-lookup"><span data-stu-id="ba159-163">Let’s edit the generated code to specify a default value of 3 for Blog.Rating (Line 10 in the code below)</span></span>

<span data-ttu-id="ba159-164">*Миграция также имеет файл кода программной части, который фиксирует некоторые метаданные. Эти метаданные позволяют Code First Migrations реплицировать автоматические миграции, выполненные до этой миграции на основе кода. Это важно, если другой разработчик хочет выполнить наши миграции или время развертывания приложения.*</span><span class="sxs-lookup"><span data-stu-id="ba159-164">*The migration also has a code-behind file that captures some metadata. This metadata will allow Code First Migrations to replicate the automatic migrations we performed before this code-based migration. This is important if another developer wants to run our migrations or when it’s time to deploy our application.*</span></span>

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

<span data-ttu-id="ba159-165">Измененная миграция выглядит прекрасно, так что давайте обновим ее с помощью команды **Update-Database**.</span><span class="sxs-lookup"><span data-stu-id="ba159-165">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span>

-   <span data-ttu-id="ba159-166">Выполните команду **обновления базы данных** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="ba159-166">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="back-to-automatic-migrations"></a><span data-ttu-id="ba159-167">Назад к автоматическим миграции</span><span class="sxs-lookup"><span data-stu-id="ba159-167">Back to Automatic Migrations</span></span>

<span data-ttu-id="ba159-168">Теперь мы можем вернуться к автоматическому переходу на более простые изменения.</span><span class="sxs-lookup"><span data-stu-id="ba159-168">We are now free to switch back to automatic migrations for our simpler changes.</span></span> <span data-ttu-id="ba159-169">Code First Migrations будет выполнять автоматические миграции и перенос на основе кода в правильном порядке на основе метаданных, которые он хранит в файле кода программной части для каждой миграции на основе кода.</span><span class="sxs-lookup"><span data-stu-id="ba159-169">Code First Migrations will take care of performing the automatic and code-based migrations in the correct order based on the metadata it is storing in the code-behind file for each code-based migration.</span></span>

-   <span data-ttu-id="ba159-170">Давайте добавим в нашу модель свойство POST. abstract.</span><span class="sxs-lookup"><span data-stu-id="ba159-170">Let’s add a Post.Abstract property to our model</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="ba159-171">Теперь можно использовать **Обновление базы данных** , чтобы получить Code First migrations для отправки этого изменения в базу данных с помощью автоматической миграции.</span><span class="sxs-lookup"><span data-stu-id="ba159-171">Now we can use **Update-Database** to get Code First Migrations to push this change to the database using an automatic migration.</span></span>

-   <span data-ttu-id="ba159-172">Выполните команду **обновления базы данных** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="ba159-172">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="summary"></a><span data-ttu-id="ba159-173">Итоги</span><span class="sxs-lookup"><span data-stu-id="ba159-173">Summary</span></span>

<span data-ttu-id="ba159-174">В этом пошаговом руководстве вы узнали, как использовать автоматические миграции для отправки изменений модели в базу данных.</span><span class="sxs-lookup"><span data-stu-id="ba159-174">In this walkthrough you saw how to use automatic migrations to push model changes to the database.</span></span> <span data-ttu-id="ba159-175">Кроме того, вы узнали, как сформировать шаблоны и выполнять переносы на основе кода между автоматической миграцией, когда требуется больший контроль.</span><span class="sxs-lookup"><span data-stu-id="ba159-175">You also saw how to scaffold and run code-based migrations in between automatic migrations when you need more control.</span></span>
