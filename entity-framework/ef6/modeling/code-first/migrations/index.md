---
title: Code First Migrations — EF6
description: Использование Code First Migrations в Entity Framework 6
author: divega
ms.date: 10/23/2016
uid: ef6/modeling/code-first/migrations/index
ms.openlocfilehash: aec3a53816e41bf7ead696fd9edc30618d58b07a
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90072165"
---
# <a name="code-first-migrations"></a><span data-ttu-id="b3690-103">Code First Migrations</span><span class="sxs-lookup"><span data-stu-id="b3690-103">Code First Migrations</span></span>
<span data-ttu-id="b3690-104">Code First Migrations — это рекомендуемый способ разработки структуры базы данных в приложении при использовании рабочего процесса Code First.</span><span class="sxs-lookup"><span data-stu-id="b3690-104">Code First Migrations is the recommended way to evolve your application's database schema if you are using the Code First workflow.</span></span> <span data-ttu-id="b3690-105">Code First Migrations предоставляет набор средств со следующими функциями:</span><span class="sxs-lookup"><span data-stu-id="b3690-105">Migrations provide a set of tools that allow:</span></span>

1. <span data-ttu-id="b3690-106">Создание исходной базы данных, которая работает с моделью EF</span><span class="sxs-lookup"><span data-stu-id="b3690-106">Create an initial database that works with your EF model</span></span>
2. <span data-ttu-id="b3690-107">Создание миграций для отслеживания изменений в модели EF</span><span class="sxs-lookup"><span data-stu-id="b3690-107">Generating migrations to keep track of changes you make to your EF model</span></span>
2. <span data-ttu-id="b3690-108">Поддержание актуальности базы данных с учетом этих изменений</span><span class="sxs-lookup"><span data-stu-id="b3690-108">Keep your database up to date with those changes</span></span>

<span data-ttu-id="b3690-109">Следующее пошаговое руководство содержит обзор Code First Migrations в Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b3690-109">The following walkthrough will provide an overview Code First Migrations in Entity Framework.</span></span> <span data-ttu-id="b3690-110">Вы можете выполнить все пошаговое руководство или сразу перейти в нужный раздел.</span><span class="sxs-lookup"><span data-stu-id="b3690-110">You can either complete the entire walkthrough or skip to the topic you are interested in.</span></span> <span data-ttu-id="b3690-111">Рассматриваются следующие темы.</span><span class="sxs-lookup"><span data-stu-id="b3690-111">The following topics are covered:</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="b3690-112">Привязка начальной модели и базы данных</span><span class="sxs-lookup"><span data-stu-id="b3690-112">Building an Initial Model & Database</span></span>

<span data-ttu-id="b3690-113">Прежде чем мы начнем использовать миграции, нам потребуется проект и модель Code First, с которой мы будем работать.</span><span class="sxs-lookup"><span data-stu-id="b3690-113">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="b3690-114">В этом пошаговом руководстве мы собираемся использовать каноническую модель **Blog** и **Post**.</span><span class="sxs-lookup"><span data-stu-id="b3690-114">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="b3690-115">Создайте новое консольное приложение **MigrationsDemo**.</span><span class="sxs-lookup"><span data-stu-id="b3690-115">Create a new **MigrationsDemo** Console application</span></span>
-   <span data-ttu-id="b3690-116">Добавьте последнюю версию пакета NuGet **EntityFramework** в проект.</span><span class="sxs-lookup"><span data-stu-id="b3690-116">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="b3690-117">**Средства —&gt; Диспетчер пакетов библиотеки —&gt; Консоль диспетчера пакетов**</span><span class="sxs-lookup"><span data-stu-id="b3690-117">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="b3690-118">Запустите команду **EntityFramework Install-Package**</span><span class="sxs-lookup"><span data-stu-id="b3690-118">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="b3690-119">Добавьте файл **Model.cs** с кодом, показанным ниже.</span><span class="sxs-lookup"><span data-stu-id="b3690-119">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="b3690-120">Этот код определяет один класс **Blog**, составляющий нашу модель предметной области, и класс **BlogContext**, который будет нашим контекстом EF Code First.</span><span class="sxs-lookup"><span data-stu-id="b3690-120">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

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

-   <span data-ttu-id="b3690-121">Теперь, когда у нас есть модель, пора использовать ее для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="b3690-121">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="b3690-122">Внесите в файл **Program.cs** приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="b3690-122">Update the **Program.cs** file with the code shown below.</span></span>

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

-   <span data-ttu-id="b3690-123">Запустите приложение, и вы увидите, что база данных **MigrationsCodeDemo.BlogContext** создается автоматически.</span><span class="sxs-lookup"><span data-stu-id="b3690-123">Run your application and you will see that a **MigrationsCodeDemo.BlogContext** database is created for you.</span></span>

    ![База данных LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="b3690-125">Включение миграций</span><span class="sxs-lookup"><span data-stu-id="b3690-125">Enabling Migrations</span></span>

<span data-ttu-id="b3690-126">Пора внести дополнительные изменения в нашу модель.</span><span class="sxs-lookup"><span data-stu-id="b3690-126">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="b3690-127">Давайте внесем в класс Blog свойство Url.</span><span class="sxs-lookup"><span data-stu-id="b3690-127">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="b3690-128">Если бы вы запустили приложение снова, оно выдало бы исключение InvalidOperationException с сообщением: *Модель, поддерживающая контекст BlogContext, изменилась с момента создания базы данных. Попробуйте обновить базу данных с помощью Code First Migrations (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](https://go.microsoft.com/fwlink/?LinkId=238269) *).*</span><span class="sxs-lookup"><span data-stu-id="b3690-128">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="b3690-129">Как видно из исключения, пора приступить к использованию Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="b3690-129">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="b3690-130">Сначала нужно включить миграции для нашего контекста.</span><span class="sxs-lookup"><span data-stu-id="b3690-130">The first step is to enable migrations for our context.</span></span>

-   <span data-ttu-id="b3690-131">Запустите команду **Enable-Migrations** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="b3690-131">Run the **Enable-Migrations** command in Package Manager Console</span></span>

    <span data-ttu-id="b3690-132">В результате этой команды в проект будет добавлена папка **Migrations**.</span><span class="sxs-lookup"><span data-stu-id="b3690-132">This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="b3690-133">В новой папке два файла:</span><span class="sxs-lookup"><span data-stu-id="b3690-133">This new folder contains two files:</span></span>

-   <span data-ttu-id="b3690-134">**Класс конфигурации.**</span><span class="sxs-lookup"><span data-stu-id="b3690-134">**The Configuration class.**</span></span> <span data-ttu-id="b3690-135">Этот класс позволяет настраивать поведение миграций для контекста.</span><span class="sxs-lookup"><span data-stu-id="b3690-135">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="b3690-136">В этом пошаговом руководстве мы будем просто использовать конфигурацию по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b3690-136">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="b3690-137">*Поскольку имеется только один контекст Code First в проекте, Enable-Migrations автоматически заполняет тип контекста, к которому относится эта конфигурация.*</span><span class="sxs-lookup"><span data-stu-id="b3690-137">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>
-   <span data-ttu-id="b3690-138">**Миграция InitialCreate**.</span><span class="sxs-lookup"><span data-stu-id="b3690-138">**An InitialCreate migration**.</span></span> <span data-ttu-id="b3690-139">Эта миграция создана, так как мы уже использовали Code First для создания базы данных, прежде чем включили миграции.</span><span class="sxs-lookup"><span data-stu-id="b3690-139">This migration was generated because we already had Code First create a database for us, before we enabled migrations.</span></span> <span data-ttu-id="b3690-140">Код в этой созданной по шаблону миграции представляет объекты, которые уже были созданы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="b3690-140">The code in this scaffolded migration represents the objects that have already been created in the database.</span></span> <span data-ttu-id="b3690-141">В нашем случае это таблица **Blog** со столбцами **BlogId** и **Name**.</span><span class="sxs-lookup"><span data-stu-id="b3690-141">In our case that is the **Blog** table with a **BlogId** and **Name** columns.</span></span> <span data-ttu-id="b3690-142">Имя файла содержит метку времени для удобства упорядочения.</span><span class="sxs-lookup"><span data-stu-id="b3690-142">The filename includes a timestamp to help with ordering.</span></span>
    <span data-ttu-id="b3690-143">*Если бы база данных еще не была создана, миграция InitialCreate не была бы добавлена в проект. Вместо этого, когда мы впервые вызвали бы Add-Migration, код для создания этих таблиц был бы перенесен в новую миграцию.*</span><span class="sxs-lookup"><span data-stu-id="b3690-143">*If the database had not already been created this InitialCreate migration would not have been added to the project. Instead, the first time we call Add-Migration the code to create these tables would be scaffolded to a new migration.*</span></span>

### <a name="multiple-models-targeting-the-same-database"></a><span data-ttu-id="b3690-144">Несколько моделей для одной целевой базы данных</span><span class="sxs-lookup"><span data-stu-id="b3690-144">Multiple Models Targeting the Same Database</span></span>

<span data-ttu-id="b3690-145">При использовании версий до EF6 можно было использовать только одну модель Code First для создания схемы базы данных и управления ей.</span><span class="sxs-lookup"><span data-stu-id="b3690-145">When using versions prior to EF6, only one Code First model could be used to generate/manage the schema of a database.</span></span> <span data-ttu-id="b3690-146">Это было связано с наличием всего одной таблицы **\_\_MigrationsHistory** на базу данных, когда невозможно было определить, какая запись какой модели принадлежит.</span><span class="sxs-lookup"><span data-stu-id="b3690-146">This is the result of a single **\_\_MigrationsHistory** table per database with no way to identify which entries belong to which model.</span></span>

<span data-ttu-id="b3690-147">Начиная с EF6, класс **Configuration** включает свойство **ContextKey**.</span><span class="sxs-lookup"><span data-stu-id="b3690-147">Starting with EF6, the **Configuration** class includes a **ContextKey** property.</span></span> <span data-ttu-id="b3690-148">Это уникальный идентификатор для каждой модели Code First.</span><span class="sxs-lookup"><span data-stu-id="b3690-148">This acts as a unique identifier for each Code First model.</span></span> <span data-ttu-id="b3690-149">В соответствующий столбец в таблице **\_\_MigrationsHistory** вносятся записи из нескольких моделей, которые используют таблицу совместно.</span><span class="sxs-lookup"><span data-stu-id="b3690-149">A corresponding column in the **\_\_MigrationsHistory** table allows entries from multiple models to share the table.</span></span> <span data-ttu-id="b3690-150">По умолчанию этому свойству присвоено полное имя контекста.</span><span class="sxs-lookup"><span data-stu-id="b3690-150">By default, this property is set to the fully qualified name of your context.</span></span>

## <a name="generating--running-migrations"></a><span data-ttu-id="b3690-151">Создание и запуск миграций</span><span class="sxs-lookup"><span data-stu-id="b3690-151">Generating & Running Migrations</span></span>

<span data-ttu-id="b3690-152">Вам нужно знать о двух основных командах Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="b3690-152">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="b3690-153">**Add-Migration** будет автоматически формировать следующую миграцию на основе изменений, внесенных в модель с момента создания последней миграции.</span><span class="sxs-lookup"><span data-stu-id="b3690-153">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="b3690-154">**Update-Database** будет применять ожидающие обработки миграции к базе данных.</span><span class="sxs-lookup"><span data-stu-id="b3690-154">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="b3690-155">Нам нужно сформировать шаблон миграции для нового свойства Url.</span><span class="sxs-lookup"><span data-stu-id="b3690-155">We need to scaffold a migration to take care of the new Url property we have added.</span></span> <span data-ttu-id="b3690-156">Команда **Add-Migration** позволяет нам давать этим миграциям имя. Давайте назовем нашу **AddBlogUrl**.</span><span class="sxs-lookup"><span data-stu-id="b3690-156">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogUrl**.</span></span>

-   <span data-ttu-id="b3690-157">Запустите команду **Add-Migration AddBlogUrl** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="b3690-157">Run the **Add-Migration AddBlogUrl** command in Package Manager Console</span></span>
-   <span data-ttu-id="b3690-158">В папке **Migrations** у нас теперь есть новая миграция **AddBlogUrl**.</span><span class="sxs-lookup"><span data-stu-id="b3690-158">In the **Migrations** folder we now have a new **AddBlogUrl** migration.</span></span> <span data-ttu-id="b3690-159">Имя файла миграции имеет метку времени в целях упорядочения.</span><span class="sxs-lookup"><span data-stu-id="b3690-159">The migration filename is pre-fixed with a timestamp to help with ordering</span></span>

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

<span data-ttu-id="b3690-160">Теперь мы могли бы изменить или дополнить эту миграцию, но все и так в порядке.</span><span class="sxs-lookup"><span data-stu-id="b3690-160">We could now edit or add to this migration but everything looks pretty good.</span></span> <span data-ttu-id="b3690-161">Мы используем **Update-Database**, чтобы применить эту миграцию к базе данных.</span><span class="sxs-lookup"><span data-stu-id="b3690-161">Let’s use **Update-Database** to apply this migration to the database.</span></span>

-   <span data-ttu-id="b3690-162">Запустите команду **Update-Database** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="b3690-162">Run the **Update-Database** command in Package Manager Console</span></span>
-   <span data-ttu-id="b3690-163">Code First Migrations сравнит миграции из папки **Migrations** с теми, которые были применены к базе данных.</span><span class="sxs-lookup"><span data-stu-id="b3690-163">Code First Migrations will compare the migrations in our **Migrations** folder with the ones that have been applied to the database.</span></span> <span data-ttu-id="b3690-164">Вы увидите, что необходимо применить и запустить миграцию **AddBlogUrl**.</span><span class="sxs-lookup"><span data-stu-id="b3690-164">It will see that the **AddBlogUrl** migration needs to be applied, and run it.</span></span>

<span data-ttu-id="b3690-165">База данных **MigrationsDemo.BlogContext** обновлена и включает столбец **Url** в таблице **Blog**.</span><span class="sxs-lookup"><span data-stu-id="b3690-165">The **MigrationsDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

## <a name="customizing-migrations"></a><span data-ttu-id="b3690-166">Настройка миграции</span><span class="sxs-lookup"><span data-stu-id="b3690-166">Customizing Migrations</span></span>

<span data-ttu-id="b3690-167">Пока мы создавали и запускали миграции без изменений.</span><span class="sxs-lookup"><span data-stu-id="b3690-167">So far we’ve generated and run a migration without making any changes.</span></span> <span data-ttu-id="b3690-168">Теперь давайте рассмотрим редактирование кода, который создается по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b3690-168">Now let’s look at editing the code that gets generated by default.</span></span>

-   <span data-ttu-id="b3690-169">Пора внести некоторые изменения в нашу модель. Давайте добавим новое свойство **Rating** в класс **Blog**.</span><span class="sxs-lookup"><span data-stu-id="b3690-169">It’s time to make some more changes to our model, let’s add a new **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

-   <span data-ttu-id="b3690-170">Также добавим новый класс **Post**.</span><span class="sxs-lookup"><span data-stu-id="b3690-170">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="b3690-171">Еще мы добавим коллекцию **Posts** в класс **Blog**, чтобы создать второй конец связи между **Blog** и **Post**.</span><span class="sxs-lookup"><span data-stu-id="b3690-171">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="b3690-172">Мы будем использовать команду **Add-Migration**, чтобы с помощью Code First Migrations автоматически создать миграцию.</span><span class="sxs-lookup"><span data-stu-id="b3690-172">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span> <span data-ttu-id="b3690-173">Мы назовем эту миграцию **AddPostClass**.</span><span class="sxs-lookup"><span data-stu-id="b3690-173">We’re going to call this migration **AddPostClass**.</span></span>

-   <span data-ttu-id="b3690-174">Запустите команду **Add-Migration AddPostClass** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="b3690-174">Run the **Add-Migration AddPostClass** command in Package Manager Console.</span></span>

<span data-ttu-id="b3690-175">С помощью Code First Migrations прекрасно удалось создать эти изменения по шаблону, но мы хотим изменить кое-что еще.</span><span class="sxs-lookup"><span data-stu-id="b3690-175">Code First Migrations did a pretty good job of scaffolding these changes, but there are some things we might want to change:</span></span>

1.  <span data-ttu-id="b3690-176">Сначала давайте добавим уникальный индекс в столбец **Posts.Title** (строки 22 и 29 в приведенном ниже коде).</span><span class="sxs-lookup"><span data-stu-id="b3690-176">First up, let’s add a unique index to **Posts.Title** column (Adding in line 22 & 29 in the code below).</span></span>
2.  <span data-ttu-id="b3690-177">Мы также добавим столбец **Blogs.Rating**, не допускающий значение null.</span><span class="sxs-lookup"><span data-stu-id="b3690-177">We’re also adding a non-nullable **Blogs.Rating** column.</span></span> <span data-ttu-id="b3690-178">Если в таблице есть данные, новому столбцу будет назначено значение CLR по умолчанию в зависимости от типа данных (оценка — это целое число, так что это будет **0**).</span><span class="sxs-lookup"><span data-stu-id="b3690-178">If there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="b3690-179">Но нам нужно указать значение по умолчанию **3**, чтобы существующие строки в таблице **Blogs** начинались с неплохой оценки.</span><span class="sxs-lookup"><span data-stu-id="b3690-179">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
    <span data-ttu-id="b3690-180">(Значение по умолчанию присваивается в строке 24 приведенного ниже кода.)</span><span class="sxs-lookup"><span data-stu-id="b3690-180">(You can see the default value specified on line 24 of the code below)</span></span>

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

<span data-ttu-id="b3690-181">Измененная миграция готова к запуску, так что давайте обновим ее с помощью команды **Update-Database**.</span><span class="sxs-lookup"><span data-stu-id="b3690-181">Our edited migration is ready to go, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="b3690-182">Давайте укажем флаг **–Verbose**, чтобы вы могли видеть SQL, который выполняет Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="b3690-182">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="b3690-183">Запустите команду **Update-Database –Verbose** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="b3690-183">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="data-motion--custom-sql"></a><span data-ttu-id="b3690-184">Перемещение данных или настраиваемый SQL</span><span class="sxs-lookup"><span data-stu-id="b3690-184">Data Motion / Custom SQL</span></span>

<span data-ttu-id="b3690-185">До сих пор мы рассматривали операции миграции, которые не меняют и не перемещают данные. Теперь давайте посмотрим на примеры, где мы перемещаем данные.</span><span class="sxs-lookup"><span data-stu-id="b3690-185">So far we have looked at migration operations that don’t change or move any data, now let’s look at something that needs to move some data around.</span></span> <span data-ttu-id="b3690-186">Для перемещения данных пока нет собственной поддержки, но мы можем запустить произвольные команды SQL в любой момент в нашем сценарии.</span><span class="sxs-lookup"><span data-stu-id="b3690-186">There is no native support for data motion yet, but we can run some arbitrary SQL commands at any point in our script.</span></span>

-   <span data-ttu-id="b3690-187">Давайте добавим свойство **Post.Abstract** в нашу модель.</span><span class="sxs-lookup"><span data-stu-id="b3690-187">Let’s add a **Post.Abstract** property to our model.</span></span> <span data-ttu-id="b3690-188">Позже мы заполним свойство **Abstract** для существующих записей текстом из начала столбца **Content**.</span><span class="sxs-lookup"><span data-stu-id="b3690-188">Later, we’re going to pre-populate the **Abstract** for existing posts using some text from the start of the **Content** column.</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="b3690-189">Мы будем использовать команду **Add-Migration**, чтобы с помощью Code First Migrations автоматически создать миграцию.</span><span class="sxs-lookup"><span data-stu-id="b3690-189">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span>

-   <span data-ttu-id="b3690-190">Запустите команду **Add-Migration AddPostAbstract** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="b3690-190">Run the **Add-Migration AddPostAbstract** command in Package Manager Console.</span></span>
-   <span data-ttu-id="b3690-191">Созданная миграция обрабатывает изменения схемы, но мы хотим предварительно заполнить столбец **Abstract** первыми 100 символами содержимого для каждой записи.</span><span class="sxs-lookup"><span data-stu-id="b3690-191">The generated migration takes care of the schema changes but we also want to pre-populate the **Abstract** column using the first 100 characters of content for each post.</span></span> <span data-ttu-id="b3690-192">Для этого мы можем перейти к SQL и выполнить оператор **UPDATE** после добавления столбца.</span><span class="sxs-lookup"><span data-stu-id="b3690-192">We can do this by dropping down to SQL and running an **UPDATE** statement after the column is added.</span></span>
    <span data-ttu-id="b3690-193">(Добавление в строке 12 в приведенном ниже коде)</span><span class="sxs-lookup"><span data-stu-id="b3690-193">(Adding in line 12 in the code below)</span></span>

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

<span data-ttu-id="b3690-194">Измененная миграция выглядит прекрасно, так что давайте обновим ее с помощью команды **Update-Database**.</span><span class="sxs-lookup"><span data-stu-id="b3690-194">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="b3690-195">Необходимо указать флаг **–Verbose**, чтобы видеть выполнение SQL в базе данных.</span><span class="sxs-lookup"><span data-stu-id="b3690-195">We’ll specify the **–Verbose** flag so that we can see the SQL being run against the database.</span></span>

-   <span data-ttu-id="b3690-196">Запустите команду **Update-Database –Verbose** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="b3690-196">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="migrate-to-a-specific-version-including-downgrade"></a><span data-ttu-id="b3690-197">Перенос в определенную версию (в том числе более раннюю)</span><span class="sxs-lookup"><span data-stu-id="b3690-197">Migrate to a Specific Version (Including Downgrade)</span></span>

<span data-ttu-id="b3690-198">Пока мы всегда выполняли обновление до последней миграции, но иногда могут возникнуть ситуации, когда вы хотите перейти к более поздней или более ранней версии миграции.</span><span class="sxs-lookup"><span data-stu-id="b3690-198">So far we have always upgraded to the latest migration, but there may be times when you want upgrade/downgrade to a specific migration.</span></span>

<span data-ttu-id="b3690-199">Предположим, мы хотим перенести базу данных в состояние до запуска миграции **AddBlogUrl**.</span><span class="sxs-lookup"><span data-stu-id="b3690-199">Let’s say we want to migrate our database to the state it was in after running our **AddBlogUrl** migration.</span></span> <span data-ttu-id="b3690-200">Мы можем использовать переключатель **–TargetMigration**, чтобы перейти на эту более раннюю версию миграции.</span><span class="sxs-lookup"><span data-stu-id="b3690-200">We can use the **–TargetMigration** switch to downgrade to this migration.</span></span>

-   <span data-ttu-id="b3690-201">Запустите команду **Update-Database –TargetMigration: AddBlogUrl** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="b3690-201">Run the **Update-Database –TargetMigration: AddBlogUrl** command in Package Manager Console.</span></span>

<span data-ttu-id="b3690-202">Эта команда запустит скрипт понижения версии для миграций **AddBlogAbstract** и **AddPostClass**.</span><span class="sxs-lookup"><span data-stu-id="b3690-202">This command will run the Down script for our **AddBlogAbstract** and **AddPostClass** migrations.</span></span>

<span data-ttu-id="b3690-203">Если вы хотите выполнить откат до пустой базы данных, используйте команду **Update-Database –TargetMigration: $InitialDatabase**.</span><span class="sxs-lookup"><span data-stu-id="b3690-203">If you want to roll all the way back to an empty database then you can use the **Update-Database –TargetMigration: $InitialDatabase** command.</span></span>

## <a name="getting-a-sql-script"></a><span data-ttu-id="b3690-204">Получение скрипта SQL</span><span class="sxs-lookup"><span data-stu-id="b3690-204">Getting a SQL Script</span></span>

<span data-ttu-id="b3690-205">Если другой разработчик хочет использовать эти изменения на своем компьютере, он может просто синхронизироваться, когда мы вернем наши изменения в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="b3690-205">If another developer wants these changes on their machine they can just sync once we check our changes into source control.</span></span> <span data-ttu-id="b3690-206">Когда он получит наши новые миграции, он может выполнить команду Update-Database, чтобы применить эти изменения локально.</span><span class="sxs-lookup"><span data-stu-id="b3690-206">Once they have our new migrations they can just run the Update-Database command to have the changes applied locally.</span></span> <span data-ttu-id="b3690-207">Но если мы хотим передать эти изменения на тестовый сервер, а затем и в рабочую среду, возможно, нам потребуется скрипт SQL, который мы сможем передать нашему администратору базы данных.</span><span class="sxs-lookup"><span data-stu-id="b3690-207">However if we want to push these changes out to a test server, and eventually production, we probably want a SQL script we can hand off to our DBA.</span></span>

-   <span data-ttu-id="b3690-208">Запустите команду **Update-Database**, но на этот раз укажите флаг **–Script**, чтобы записать изменения в скрипт, а не применить.</span><span class="sxs-lookup"><span data-stu-id="b3690-208">Run the **Update-Database** command but this time specify the **–Script** flag so that changes are written to a script rather than applied.</span></span> <span data-ttu-id="b3690-209">Мы также укажем исходную и целевую миграцию, для которой необходимо создать скрипт.</span><span class="sxs-lookup"><span data-stu-id="b3690-209">We’ll also specify a source and target migration to generate the script for.</span></span> <span data-ttu-id="b3690-210">Мы хотим, чтобы скрипт переходил от пустой базы данных (**$InitialDatabase**) до последней версии (миграция **AddPostAbstract**).</span><span class="sxs-lookup"><span data-stu-id="b3690-210">We want a script to go from an empty database (**$InitialDatabase**) to the latest version (migration **AddPostAbstract**).</span></span>
    <span data-ttu-id="b3690-211">*Если вы не укажете целевую миграцию, Code First Migrations будет использовать последнюю версию миграции в качестве целевого объекта. Если вы не укажете исходную миграцию, Code First Migrations будет использовать текущее состояние базы данных.*</span><span class="sxs-lookup"><span data-stu-id="b3690-211">*If you don’t specify a target migration, Migrations will use the latest migration as the target. If you don't specify a source migrations, Migrations will use the current state of the database.*</span></span>
-   <span data-ttu-id="b3690-212">Запустите команду **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: AddPostAbstract** в консоли диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="b3690-212">Run the **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: AddPostAbstract** command in Package Manager Console</span></span>

<span data-ttu-id="b3690-213">Code First Migrations будет выполнять конвейер миграций, но будет не применять изменения, а записывать их в SQL-файл.</span><span class="sxs-lookup"><span data-stu-id="b3690-213">Code First Migrations will run the migration pipeline but instead of actually applying the changes it will write them out to a .sql file for you.</span></span> <span data-ttu-id="b3690-214">Созданный скрипт автоматически открывается в Visual Studio, готовый для просмотра или сохранения.</span><span class="sxs-lookup"><span data-stu-id="b3690-214">Once the script is generated, it is opened for you in Visual Studio, ready for you to view or save.</span></span>

### <a name="generating-idempotent-scripts"></a><span data-ttu-id="b3690-215">Создание идемпотентных скриптов</span><span class="sxs-lookup"><span data-stu-id="b3690-215">Generating Idempotent Scripts</span></span>

<span data-ttu-id="b3690-216">Начиная с EF6, если вы укажете **–SourceMigration $InitialDatabase**, создаваемый скрипт будет идемпотентным.</span><span class="sxs-lookup"><span data-stu-id="b3690-216">Starting with EF6, if you specify **–SourceMigration $InitialDatabase** then the generated script will be ‘idempotent’.</span></span> <span data-ttu-id="b3690-217">Идемпотентные скрипты могут обновить базу данных любой версии до последней версии (или указанной версии, если вы используете **–TargetMigration**).</span><span class="sxs-lookup"><span data-stu-id="b3690-217">Idempotent scripts can upgrade a database currently at any version to the latest version (or the specified version if you use **–TargetMigration**).</span></span> <span data-ttu-id="b3690-218">Созданный скрипт содержит логику для проверки таблицы **\_\_MigrationsHistory** и применяет только изменения, которые еще не применены.</span><span class="sxs-lookup"><span data-stu-id="b3690-218">The generated script includes logic to check the **\_\_MigrationsHistory** table and only apply changes that haven't been previously applied.</span></span>

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a><span data-ttu-id="b3690-219">Автоматическое обновление при запуске приложения (инициализатор MigrateDatabaseToLatestVersion)</span><span class="sxs-lookup"><span data-stu-id="b3690-219">Automatically Upgrading on Application Startup (MigrateDatabaseToLatestVersion Initializer)</span></span>

<span data-ttu-id="b3690-220">Когда вы развертываете приложение, вы можете настроить автоматическое обновление базы данных (путем применения ожидающих миграций) при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="b3690-220">If you are deploying your application you may want it to automatically upgrade the database (by applying any pending migrations) when the application launches.</span></span> <span data-ttu-id="b3690-221">Это можно сделать с помощью регистрации инициализатора базы данных **MigrateDatabaseToLatestVersion**.</span><span class="sxs-lookup"><span data-stu-id="b3690-221">You can do this by registering the **MigrateDatabaseToLatestVersion** database initializer.</span></span> <span data-ttu-id="b3690-222">Инициализатор базы данных просто содержит определенную логику, которая используется для контроля правильной настройки базы данных.</span><span class="sxs-lookup"><span data-stu-id="b3690-222">A database initializer simply contains some logic that is used to make sure the database is setup correctly.</span></span> <span data-ttu-id="b3690-223">Эта логика выполняется при первом использовании контекста в процессе приложения (**AppDomain**).</span><span class="sxs-lookup"><span data-stu-id="b3690-223">This logic is run the first time the context is used within the application process (**AppDomain**).</span></span>

<span data-ttu-id="b3690-224">Мы можем обновить файл **Program.cs**, как показано ниже, чтобы настроить инициализатор **MigrateDatabaseToLatestVersion** для BlogContext, прежде чем мы используем контекст (строка 14).</span><span class="sxs-lookup"><span data-stu-id="b3690-224">We can update the **Program.cs** file, as shown below, to set the **MigrateDatabaseToLatestVersion** initializer for BlogContext before we use the context (Line 14).</span></span> <span data-ttu-id="b3690-225">Обратите внимание, что необходимо также добавить оператор using для пространства имен **System.Data.Entity** (строка 5).</span><span class="sxs-lookup"><span data-stu-id="b3690-225">Note that you also need to add a using statement for the **System.Data.Entity** namespace (Line 5).</span></span>

<span data-ttu-id="b3690-226">*При создании экземпляра этого инициализатора нам нужно указать тип контекста (**BlogContext**) и конфигурацию миграций (**Configuration**). Конфигурация миграций — это класс, который был добавлен в нашу папку **Migrations** при включении Migrations.*</span><span class="sxs-lookup"><span data-stu-id="b3690-226">*When we create an instance of this initializer we need to specify the context type (**BlogContext**) and the migrations configuration (**Configuration**) - the migrations configuration is the class that got added to our **Migrations** folder when we enabled Migrations.*</span></span>

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
                Database.SetInitializer(new MigrateDatabaseToLatestVersion<BlogContext, Configuration>());

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

<span data-ttu-id="b3690-227">Теперь при каждом запуске приложение сначала будет проверять актуальность целевой базы данных и применять незавершенные миграции, если это необходимо.</span><span class="sxs-lookup"><span data-stu-id="b3690-227">Now whenever our application runs it will first check if the database it is targeting is up-to-date, and apply any pending migrations if it is not.</span></span>
