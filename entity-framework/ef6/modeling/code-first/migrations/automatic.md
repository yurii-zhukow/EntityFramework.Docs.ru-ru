---
title: Автоматическая Code First Migrations-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
ms.openlocfilehash: 2713afaf09707b7696e90464aac9945c2d82d274
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415703"
---
# <a name="automatic-code-first-migrations"></a><span data-ttu-id="d5c3a-102">Автоматическое Code First Migrations</span><span class="sxs-lookup"><span data-stu-id="d5c3a-102">Automatic Code First Migrations</span></span>
<span data-ttu-id="d5c3a-103">Автоматическая миграция позволяет использовать Code First Migrations без файла кода в проекте для каждого внесенного изменения.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-103">Automatic Migrations allows you to use Code First Migrations without having a code file in your project for each change you make.</span></span> <span data-ttu-id="d5c3a-104">Не все изменения могут быть применены автоматически. Например, для переименования столбцов требуется использовать перенос на основе кода.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-104">Not all changes can be applied automatically - for example column renames require the use of a code-based migration.</span></span>

> [!NOTE]
> <span data-ttu-id="d5c3a-105">В этой статье предполагается, что вы умеете использовать Code First Migrations в основных сценариях.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="d5c3a-106">В противном случае необходимо прочитать [Code First migrations](~/ef6/modeling/code-first/migrations/index.md) , прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="recommendation-for-team-environments"></a><span data-ttu-id="d5c3a-107">Рекомендации для сред группы</span><span class="sxs-lookup"><span data-stu-id="d5c3a-107">Recommendation for Team Environments</span></span>

<span data-ttu-id="d5c3a-108">Можно использовать автоматическую миграцию и перенос на основе кода, но это не рекомендуется делать в сценариях групповой разработки.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-108">You can intersperse automatic and code-based migrations but this is not recommended in team development scenarios.</span></span> <span data-ttu-id="d5c3a-109">Если вы являетесь членом группы разработчиков, использующих систему управления версиями, следует использовать исключительно автоматическую миграцию или только переносы на основе кода.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-109">If you are part of a team of developers that use source control you should either use purely automatic migrations or purely code-based migrations.</span></span> <span data-ttu-id="d5c3a-110">Учитывая ограничения автоматической миграции, мы рекомендуем использовать миграцию на основе кода в средах группы.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-110">Given the limitations of automatic migrations we recommend using code-based migrations in team environments.</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="d5c3a-111">Привязка начальной модели и базы данных</span><span class="sxs-lookup"><span data-stu-id="d5c3a-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="d5c3a-112">Прежде чем мы начнем использовать миграции, нам потребуется проект и модель Code First, с которой мы будем работать.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="d5c3a-113">В этом пошаговом руководстве мы собираемся использовать каноническую модель **Blog** и **Post**.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="d5c3a-114">Создание нового консольного приложения **мигратионсаутоматикдемо**</span><span class="sxs-lookup"><span data-stu-id="d5c3a-114">Create a new **MigrationsAutomaticDemo** Console application</span></span>
-   <span data-ttu-id="d5c3a-115">Добавьте последнюю версию пакета NuGet **EntityFramework** в проект.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="d5c3a-116">**Средства —&gt; Диспетчер пакетов библиотеки —&gt; Консоль диспетчера пакетов**</span><span class="sxs-lookup"><span data-stu-id="d5c3a-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="d5c3a-117">Запустите команду **EntityFramework Install-Package**</span><span class="sxs-lookup"><span data-stu-id="d5c3a-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="d5c3a-118">Добавьте файл **Model.cs** с кодом, показанным ниже.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="d5c3a-119">Этот код определяет один класс **Blog**, составляющий нашу модель предметной области, и класс **BlogContext**, который будет нашим контекстом EF Code First.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

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

-   <span data-ttu-id="d5c3a-120">Теперь, когда у нас есть модель, пора использовать ее для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="d5c3a-121">Внесите в файл **Program.cs** приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-121">Update the **Program.cs** file with the code shown below.</span></span>

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

-   <span data-ttu-id="d5c3a-122">Запустите приложение, и вы увидите, что база данных **мигратионсаутоматиккодедемо. BlogContext** создана.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-122">Run your application and you will see that a **MigrationsAutomaticCodeDemo.BlogContext** database is created for you.</span></span>

    ![LocalDB базы данных](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="d5c3a-124">Включение миграций</span><span class="sxs-lookup"><span data-stu-id="d5c3a-124">Enabling Migrations</span></span>

<span data-ttu-id="d5c3a-125">Пора внести дополнительные изменения в нашу модель.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="d5c3a-126">Давайте внесем в класс Blog свойство Url.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="d5c3a-127">Если бы вы выполняли приложение еще раз, вы получите исключение InvalidOperationException, сообщающее *, что модель резервного копирования контекста "BlogContext" изменилась с момента создания базы данных. Рекомендуется использовать Code First Migrations для обновления базы данных (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](https://go.microsoft.com/fwlink/?LinkId=238269) *).*</span><span class="sxs-lookup"><span data-stu-id="d5c3a-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="d5c3a-128">Как видно из исключения, пора приступить к использованию Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="d5c3a-129">Поскольку мы хотим использовать автоматические миграции, мы будем указывать параметр **– енаблеаутоматикмигратионс** .</span><span class="sxs-lookup"><span data-stu-id="d5c3a-129">Because we want to use automatic migrations we’re going to specify the **–EnableAutomaticMigrations** switch.</span></span>

-   <span data-ttu-id="d5c3a-130">Выполните команду **Enable-migrations – енаблеаутоматикмигратионс** в консоли диспетчера пакетов. Эта команда добавила в наш проект папку **migrations** .</span><span class="sxs-lookup"><span data-stu-id="d5c3a-130">Run the **Enable-Migrations –EnableAutomaticMigrations** command in Package Manager Console This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="d5c3a-131">Эта новая папка содержит один файл:</span><span class="sxs-lookup"><span data-stu-id="d5c3a-131">This new folder contains one file:</span></span>

-   <span data-ttu-id="d5c3a-132">**Класс конфигурации.**</span><span class="sxs-lookup"><span data-stu-id="d5c3a-132">**The Configuration class.**</span></span> <span data-ttu-id="d5c3a-133">Этот класс позволяет настраивать поведение миграций для контекста.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-133">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="d5c3a-134">В этом пошаговом руководстве мы будем просто использовать конфигурацию по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-134">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="d5c3a-135">*Поскольку имеется только один контекст Code First в проекте, Enable-Migrations автоматически заполняет тип контекста, к которому относится эта конфигурация.*</span><span class="sxs-lookup"><span data-stu-id="d5c3a-135">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>

 

## <a name="your-first-automatic-migration"></a><span data-ttu-id="d5c3a-136">Первая автоматическая миграция</span><span class="sxs-lookup"><span data-stu-id="d5c3a-136">Your First Automatic Migration</span></span>

<span data-ttu-id="d5c3a-137">Вам нужно знать о двух основных командах Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-137">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="d5c3a-138">**Add-Migration** будет автоматически формировать следующую миграцию на основе изменений, внесенных в модель с момента создания последней миграции.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-138">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="d5c3a-139">**Update-Database** будет применять ожидающие обработки миграции к базе данных.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-139">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="d5c3a-140">Мы будем избегать использования добавления-миграции (если это не требуется) и сосредоточиться на том, чтобы Code First Migrations автоматически вычислять и применять изменения.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-140">We are going to avoid using Add-Migration (unless we really need to) and focus on letting Code First Migrations automatically calculate and apply the changes.</span></span> <span data-ttu-id="d5c3a-141">Мы используем **Обновление базы данных** , чтобы получить Code First migrations для отправки изменений в нашу модель (новое свойство **Blog. ur**l) в базу данных.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-141">Let’s use **Update-Database** to get Code First Migrations to push the changes to our model (the new **Blog.Ur**l property) to the database.</span></span>

-   <span data-ttu-id="d5c3a-142">Выполните команду **обновления базы данных** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-142">Run the **Update-Database** command in Package Manager Console.</span></span>

<span data-ttu-id="d5c3a-143">База данных **мигратионсаутоматикдемо. BlogContext** теперь обновляется для включения столбца **URL-адреса** в таблицу **blogs** .</span><span class="sxs-lookup"><span data-stu-id="d5c3a-143">The **MigrationsAutomaticDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

 

## <a name="your-second-automatic-migration"></a><span data-ttu-id="d5c3a-144">Вторая автоматическая миграция</span><span class="sxs-lookup"><span data-stu-id="d5c3a-144">Your Second Automatic Migration</span></span>

<span data-ttu-id="d5c3a-145">Сделаем еще одно изменение и позвольте Code First Migrations автоматически отправлять изменения в базу данных для нас.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-145">Let’s make another change and let Code First Migrations automatically push the changes to the database for us.</span></span>

-   <span data-ttu-id="d5c3a-146">Также добавим новый класс **Post**.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-146">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="d5c3a-147">Еще мы добавим коллекцию **Posts** в класс **Blog**, чтобы создать второй конец связи между **Blog** и **Post**.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-147">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="d5c3a-148">Теперь используйте **Обновление базы данных** , чтобы обновить базу данных в актуальном состоянии.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-148">Now use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="d5c3a-149">Давайте укажем флаг **–Verbose**, чтобы вы могли видеть SQL, который выполняет Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-149">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="d5c3a-150">Запустите команду **Update-Database –Verbose** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-150">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="adding-a-code-based-migration"></a><span data-ttu-id="d5c3a-151">Добавление переноса на основе кода</span><span class="sxs-lookup"><span data-stu-id="d5c3a-151">Adding a Code Based Migration</span></span>

<span data-ttu-id="d5c3a-152">Теперь давайте взглянем на то, что мы можем использовать для переноса на основе кода.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-152">Now let’s look at something we might want to use a code-based migration for.</span></span>

-   <span data-ttu-id="d5c3a-153">Давайте добавим свойство **рейтинга** к классу **Blog** .</span><span class="sxs-lookup"><span data-stu-id="d5c3a-153">Let’s add a **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

<span data-ttu-id="d5c3a-154">Для принудительной отправки этих изменений в базу данных можно просто выполнить **Обновление базы данных** .</span><span class="sxs-lookup"><span data-stu-id="d5c3a-154">We could just run **Update-Database** to push these changes to the database.</span></span> <span data-ttu-id="d5c3a-155">Тем не менее мы добавляем в столбец, не допускающий значения NULL, в столбце **рейтинг** , если в таблице есть данные, им будет назначено значение CLR по умолчанию для нового столбца (оценка — целое число, то есть значение **0**).</span><span class="sxs-lookup"><span data-stu-id="d5c3a-155">However, we're adding a non-nullable **Blogs.Rating** column, if there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="d5c3a-156">Но нам нужно указать значение по умолчанию **3**, чтобы существующие строки в таблице **Blogs** начинались с неплохой оценки.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-156">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
<span data-ttu-id="d5c3a-157">Воспользуемся командой Add-Migration, чтобы записать это изменение в перенос на основе кода, чтобы мы могли изменить его.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-157">Let’s use the Add-Migration command to write this change out to a code-based migration so that we can edit it.</span></span> <span data-ttu-id="d5c3a-158">С помощью команды **Add-Migration** можно присвоить имя миграции. Давайте просто вызываем наш **аддблогратинг**.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-158">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogRating**.</span></span>

-   <span data-ttu-id="d5c3a-159">Выполните команду **Add-Migration аддблогратинг** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-159">Run the **Add-Migration AddBlogRating** command in Package Manager Console.</span></span>
-   <span data-ttu-id="d5c3a-160">В папке **migrations** теперь появилась новая **аддблогратинг** миграция.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-160">In the **Migrations** folder we now have a new **AddBlogRating** migration.</span></span> <span data-ttu-id="d5c3a-161">Имя файла миграции предварительно исправлено с меткой времени, помогающей в упорядочении.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-161">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="d5c3a-162">Давайте изменим созданный код, указав значение по умолчанию 3 для блога. рейтинг (строка 10 в приведенном ниже коде).</span><span class="sxs-lookup"><span data-stu-id="d5c3a-162">Let’s edit the generated code to specify a default value of 3 for Blog.Rating (Line 10 in the code below)</span></span>

<span data-ttu-id="d5c3a-163">*Миграция также имеет файл кода программной части, который фиксирует некоторые метаданные. Эти метаданные позволяют Code First Migrations реплицировать автоматические миграции, выполненные до этой миграции на основе кода. Это важно, если другой разработчик хочет выполнить наши миграции или время развертывания приложения.*</span><span class="sxs-lookup"><span data-stu-id="d5c3a-163">*The migration also has a code-behind file that captures some metadata. This metadata will allow Code First Migrations to replicate the automatic migrations we performed before this code-based migration. This is important if another developer wants to run our migrations or when it’s time to deploy our application.*</span></span>

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

<span data-ttu-id="d5c3a-164">Измененная миграция выглядит прекрасно, так что давайте обновим ее с помощью команды **Update-Database**.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-164">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span>

-   <span data-ttu-id="d5c3a-165">Выполните команду **обновления базы данных** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-165">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="back-to-automatic-migrations"></a><span data-ttu-id="d5c3a-166">Назад к автоматическим миграции</span><span class="sxs-lookup"><span data-stu-id="d5c3a-166">Back to Automatic Migrations</span></span>

<span data-ttu-id="d5c3a-167">Теперь мы можем вернуться к автоматическому переходу на более простые изменения.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-167">We are now free to switch back to automatic migrations for our simpler changes.</span></span> <span data-ttu-id="d5c3a-168">Code First Migrations будет выполнять автоматические миграции и перенос на основе кода в правильном порядке на основе метаданных, которые он хранит в файле кода программной части для каждой миграции на основе кода.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-168">Code First Migrations will take care of performing the automatic and code-based migrations in the correct order based on the metadata it is storing in the code-behind file for each code-based migration.</span></span>

-   <span data-ttu-id="d5c3a-169">Давайте добавим в нашу модель свойство POST. abstract.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-169">Let’s add a Post.Abstract property to our model</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="d5c3a-170">Теперь можно использовать **Обновление базы данных** , чтобы получить Code First migrations для отправки этого изменения в базу данных с помощью автоматической миграции.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-170">Now we can use **Update-Database** to get Code First Migrations to push this change to the database using an automatic migration.</span></span>

-   <span data-ttu-id="d5c3a-171">Выполните команду **обновления базы данных** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-171">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="summary"></a><span data-ttu-id="d5c3a-172">Сводка</span><span class="sxs-lookup"><span data-stu-id="d5c3a-172">Summary</span></span>

<span data-ttu-id="d5c3a-173">В этом пошаговом руководстве вы узнали, как использовать автоматические миграции для отправки изменений модели в базу данных.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-173">In this walkthrough you saw how to use automatic migrations to push model changes to the database.</span></span> <span data-ttu-id="d5c3a-174">Кроме того, вы узнали, как сформировать шаблоны и выполнять переносы на основе кода между автоматической миграцией, когда требуется больший контроль.</span><span class="sxs-lookup"><span data-stu-id="d5c3a-174">You also saw how to scaffold and run code-based migrations in between automatic migrations when you need more control.</span></span>
