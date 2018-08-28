---
title: Проверка - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: eec834888e2e3efaadc8acf9d4f64307f394ea4a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994449"
---
# <a name="data-validation"></a><span data-ttu-id="9cf85-102">Проверка данных</span><span class="sxs-lookup"><span data-stu-id="9cf85-102">Data Validation</span></span>
> [!NOTE]
> <span data-ttu-id="9cf85-103">**EF4.1 и более поздних версий только** -функции, интерфейсы API, и т.д., описанных на этой странице появились в версии 4.1 платформы Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9cf85-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="9cf85-104">При использовании более ранней версии, некоторые или все сведения неприменимо</span><span class="sxs-lookup"><span data-stu-id="9cf85-104">If you are using an earlier version, some or all of the information does not apply</span></span>

<span data-ttu-id="9cf85-105">Содержимое на этой странице написана по материалам, а статья задумано, Джули Лерман ([http://thedatafarm.com](http://thedatafarm.com)).</span><span class="sxs-lookup"><span data-stu-id="9cf85-105">The content on this page is adapted from and article originally written by Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).</span></span>

<span data-ttu-id="9cf85-106">Платформа Entity Framework предоставляет выполнения разнообразных функций проверки, которые можно веб-канал через пользовательский интерфейс для проверки на стороне клиента или использоваться для проверки на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="9cf85-106">Entity Framework provides a great variety of validation features that can feed through to a user interface for client-side validation or be used for server-side validation.</span></span> <span data-ttu-id="9cf85-107">При использовании кода, во-первых, можно указать с помощью заметки или fluent API конфигурации проверки.</span><span class="sxs-lookup"><span data-stu-id="9cf85-107">When using code first, you can specify validations using annotation or fluent API configurations.</span></span> <span data-ttu-id="9cf85-108">Дополнительные проверки и сложнее, можно указать в коде и будет работать ли модель приехал из кода, во-первых, сначала модели или базы данных сначала.</span><span class="sxs-lookup"><span data-stu-id="9cf85-108">Additional validations, and more complex, can be specified in code and will work whether your model hails from code first, model first or database first.</span></span>

## <a name="the-model"></a><span data-ttu-id="9cf85-109">Модель</span><span class="sxs-lookup"><span data-stu-id="9cf85-109">The model</span></span>

<span data-ttu-id="9cf85-110">Я продемонстрирую проверок с парой простых классов: блог и Post.</span><span class="sxs-lookup"><span data-stu-id="9cf85-110">I’ll demonstrate the validations with a simple pair of classes: Blog and Post.</span></span>

``` csharp
    public class Blog
      {
          public int Id { get; set; }
          public string Title { get; set; }
          public string BloggerName { get; set; }
          public DateTime DateCreated { get; set; }
          public virtual ICollection<Post> Posts { get; set; }
          }
      }

      public class Post
      {
          public int Id { get; set; }
          public string Title { get; set; }
          public DateTime DateCreated { get; set; }
          public string Content { get; set; }
          public int BlogId { get; set; }
          public ICollection<Comment> Comments { get; set; }
      }
```
## <a name="data-annotations"></a><span data-ttu-id="9cf85-111">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="9cf85-111">Data Annotations</span></span>

<span data-ttu-id="9cf85-112">Сначала в коде используется заметок, связанных с сборке System.ComponentModel.DataAnnotations как один из способов настройки классов первого кода.</span><span class="sxs-lookup"><span data-stu-id="9cf85-112">Code First uses annotations from the System.ComponentModel.DataAnnotations assembly as one means of configuring code first classes.</span></span> <span data-ttu-id="9cf85-113">Эти заметки относятся те, которые предоставляют правил, таких как обязательные, MaxLength и MinLength.</span><span class="sxs-lookup"><span data-stu-id="9cf85-113">Among these annotations are those which provide rules such as the Required, MaxLength and MinLength.</span></span> <span data-ttu-id="9cf85-114">Число клиентских приложениях .NET также распознает такие заметки, например, ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9cf85-114">A number of .NET client applications also recognize these annotations, for example, ASP.NET MVC.</span></span> <span data-ttu-id="9cf85-115">Вы можете достичь обе стороны и сервером проверки на стороне клиента с этими заметками.</span><span class="sxs-lookup"><span data-stu-id="9cf85-115">You can achieve both client side and server side validation with these annotations.</span></span> <span data-ttu-id="9cf85-116">Например можно принудительно свойство Title блог обязательное свойство.</span><span class="sxs-lookup"><span data-stu-id="9cf85-116">For example, you can force the Blog Title property to be a required property.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="9cf85-117">Без дополнительного кода или разметки изменений в приложение существующее приложение MVC будет выполнять проверки на стороне клиента, даже динамическое создание сообщения с использованием имени свойства и заметки.</span><span class="sxs-lookup"><span data-stu-id="9cf85-117">With no additional code or markup changes in the application, an existing MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![figure01](~/ef6/media/figure01.png)

<span data-ttu-id="9cf85-119">В блога back-метод этого создать представления, Entity Framework используется для сохранения в новой записи в базу данных, но проверка на стороне клиента в MVC запускается прежде, чем оно достигнет этого кода.</span><span class="sxs-lookup"><span data-stu-id="9cf85-119">In the post back method of this Create view, Entity Framework is used to save the new blog to the database, but MVC’s client-side validation is triggered before the application reaches that code.</span></span>

<span data-ttu-id="9cf85-120">Проверка на стороне клиента не пуленепробиваема тем не менее.</span><span class="sxs-lookup"><span data-stu-id="9cf85-120">Client side validation is not bullet-proof however.</span></span> <span data-ttu-id="9cf85-121">Пользователи могут повлиять на возможности браузера или что еще хуже, злоумышленник может использовать некоторые изобретательности во избежание проверки пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="9cf85-121">Users can impact features of their browser or worse yet, a hacker might use some trickery to avoid the UI validations.</span></span> <span data-ttu-id="9cf85-122">Но платформа Entity Framework также распознает необходимые заметки и его проверки.</span><span class="sxs-lookup"><span data-stu-id="9cf85-122">But Entity Framework will also recognize the Required annotation and validate it.</span></span>

<span data-ttu-id="9cf85-123">Чтобы отключить функцию проверки на стороне клиента в MVC является простой способ проверить это.</span><span class="sxs-lookup"><span data-stu-id="9cf85-123">A simple way to test this is to disable MVC’s client-side validation feature.</span></span> <span data-ttu-id="9cf85-124">Это можно сделать в файле web.config приложения MVC.</span><span class="sxs-lookup"><span data-stu-id="9cf85-124">You can do this in the MVC application’s web.config file.</span></span> <span data-ttu-id="9cf85-125">В разделе appSettings имеет ключ для ClientValidationEnabled.</span><span class="sxs-lookup"><span data-stu-id="9cf85-125">The appSettings section has a key for ClientValidationEnabled.</span></span> <span data-ttu-id="9cf85-126">Задавать этот параметр в значение false будет препятствовать выполнение проверки пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="9cf85-126">Setting this key to false will prevent the UI from performing validations.</span></span>

``` xml
    <appSettings>
        <add key="ClientValidationEnabled"value="false"/>
        ...
    </appSettings>
```

<span data-ttu-id="9cf85-127">Даже при проверке со стороны клиента отключен вы получите отклик в приложении.</span><span class="sxs-lookup"><span data-stu-id="9cf85-127">Even with the client-side validation disabled, you will get the same response in your application.</span></span> <span data-ttu-id="9cf85-128">Сообщение об ошибке «Title это обязательное поле» отображается как.</span><span class="sxs-lookup"><span data-stu-id="9cf85-128">The error message “The Title field is required” will be displayed as.</span></span> <span data-ttu-id="9cf85-129">За исключением того, теперь она будет результатом проверки на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="9cf85-129">Except now it will be a result of server-side validation.</span></span> <span data-ttu-id="9cf85-130">Платформа Entity Framework будет выполнять проверку необходимые заметки (прежде чем он даже идея сборки и команда INSERT для отправки в базу данных) и возвращает ошибку для MVC, которая будет отображаться сообщение.</span><span class="sxs-lookup"><span data-stu-id="9cf85-130">Entity Framework will perform the validation on the Required annotation (before it even bothers to build and INSERT command to send to the database) and return the error to MVC which will display the message.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="9cf85-131">Текучий API</span><span class="sxs-lookup"><span data-stu-id="9cf85-131">Fluent API</span></span>

<span data-ttu-id="9cf85-132">Вы можете использовать код проверки стороне текучий API вместо заметок для получения того же клиента на стороне & сервер первого элемента.</span><span class="sxs-lookup"><span data-stu-id="9cf85-132">You can use code first’s fluent API instead of annotations to get the same client side & server side validation.</span></span> <span data-ttu-id="9cf85-133">Вместо использования требуется, я покажу это с помощью проверки MaxLength.</span><span class="sxs-lookup"><span data-stu-id="9cf85-133">Rather than use Required, I’ll show you this using a MaxLength validation.</span></span>

<span data-ttu-id="9cf85-134">Fluent API конфигурации применяются в том случае, как код сначала создает модель от классов.</span><span class="sxs-lookup"><span data-stu-id="9cf85-134">Fluent API configurations are applied as code first is building the model from the classes.</span></span> <span data-ttu-id="9cf85-135">Вы можете внедрить конфигураций путем переопределения метода OnModelCreating класса DbContext.</span><span class="sxs-lookup"><span data-stu-id="9cf85-135">You can inject the configurations by overriding the DbContext class’ OnModelCreating  method.</span></span> <span data-ttu-id="9cf85-136">Здесь — это конфигурация, указывающего, что свойство BloggerName может быть не более 10 символов.</span><span class="sxs-lookup"><span data-stu-id="9cf85-136">Here is a configuration specifying that the BloggerName property can be no longer than 10 characters.</span></span>

``` csharp
    public class BlogContext : DbContext
      {
          public DbSet<Blog> Blogs { get; set; }
          public DbSet<Post> Posts { get; set; }
          public DbSet<Comment> Comments { get; set; }

          protected override void OnModelCreating(DbModelBuilder modelBuilder)
          {
              modelBuilder.Entity<Blog>().Property(p => p.BloggerName).HasMaxLength(10);
          }
        }
```

<span data-ttu-id="9cf85-137">Исключение ошибки проверки в зависимости от настроек Fluent API будет не автоматически охват пользовательского интерфейса, но можно записать его в код, а затем отреагировать на него соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="9cf85-137">Validation errors thrown based on the Fluent API configurations will not automatically reach the UI, but you can capture it in code and then respond to it accordingly.</span></span>

<span data-ttu-id="9cf85-138">Вот некоторые код обработки исключений ошибок в классе BlogController приложения, который фиксирует эту ошибку проверки, при попытке сохранить блог с BloggerName, превышает максимальный предел 10-значный Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9cf85-138">Here’s some exception handling error code in the application’s BlogController class that captures that validation error when Entity Framework attempts to save a blog with a BloggerName that exceeds the 10 character maximum.</span></span>

``` csharp
    [HttpPost]
    public ActionResult Edit(int id, Blog blog)
    {
        try
        {
            db.Entry(blog).State = EntityState.Modified;
            db.SaveChanges();
            return RedirectToAction("Index");
        }
        catch(DbEntityValidationException ex)
        {
            var error = ex.EntityValidationErrors.First().ValidationErrors.First();
            this.ModelState.AddModelError(error.PropertyName, error.ErrorMessage);
            return View();
        }
    }
```

<span data-ttu-id="9cf85-139">Проверка автоматически не был передан обратно в представление, поэтому используется дополнительный код, который использует ModelState.AddModelError.</span><span class="sxs-lookup"><span data-stu-id="9cf85-139">The validation doesn’t automatically get passed back into the view which is why the additional code that uses ModelState.AddModelError is being used.</span></span> <span data-ttu-id="9cf85-140">Это гарантирует, что сведения об ошибке сделать его в представление, в котором будет использовать ValidationMessageFor Htmlhelper для отображения ошибки.</span><span class="sxs-lookup"><span data-stu-id="9cf85-140">This ensures that the error details make it to the view which will then use the ValidationMessageFor Htmlhelper to display the error.</span></span>

``` csharp
    @Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a><span data-ttu-id="9cf85-141">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="9cf85-141">IValidatableObject</span></span>

<span data-ttu-id="9cf85-142">IValidatableObject — это интерфейс, живет в System.ComponentModel.DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="9cf85-142">IValidatableObject is an interface that lives in System.ComponentModel.DataAnnotations.</span></span> <span data-ttu-id="9cf85-143">Хотя он не является частью Entity Framework API, вы можете по-прежнему использовать его для проверки на стороне сервера в классах Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9cf85-143">While it is not part of the Entity Framework API, you can still leverage it for server-side validation in your Entity Framework classes.</span></span> <span data-ttu-id="9cf85-144">IValidatableObject предоставляет метод Validate, Entity Framework будет вызывать во время SaveChanges. также можно вызвать самостоятельно любое время, вы хотите проверить классы.</span><span class="sxs-lookup"><span data-stu-id="9cf85-144">IValidatableObject provides a Validate method that Entity Framework will call during SaveChanges or you can call yourself any time you want to validate the classes.</span></span>

<span data-ttu-id="9cf85-145">Конфигурации, такие как необходимые и MaxLength выполняют инициировать по одному полю.</span><span class="sxs-lookup"><span data-stu-id="9cf85-145">Configurations such as Required and MaxLength perform validaton on a single field.</span></span> <span data-ttu-id="9cf85-146">В метод Validate может иметь более сложную логику, например, сравнивая два поля.</span><span class="sxs-lookup"><span data-stu-id="9cf85-146">In the Validate method you can have even more complex logic, for example, comparing two fields.</span></span>

<span data-ttu-id="9cf85-147">В следующем примере класс блог была расширена для реализации IValidatableObject, а затем укажите правило, которое не может совпадать с заголовком и BloggerName.</span><span class="sxs-lookup"><span data-stu-id="9cf85-147">In the following example, the Blog class has been extended to implement IValidatableObject and then provide a rule that the Title and BloggerName cannot match.</span></span>

``` csharp
    public class Blog : IValidatableObject
     {
         public int Id { get; set; }
         [Required]
         public string Title { get; set; }
         public string BloggerName { get; set; }
         public DateTime DateCreated { get; set; }
         public virtual ICollection<Post> Posts { get; set; }

         public IEnumerable<ValidationResult> Validate(ValidationContext validationContext)
         {
             if (Title == BloggerName)
             {
                 yield return new ValidationResult
                  ("Blog Title cannot match Blogger Name", new[] { "Title", “BloggerName” });
             }
         }
     }
```

<span data-ttu-id="9cf85-148">Конструктор ValidationResult принимает строку, которая представляет сообщение об ошибке, а также массив строк, представляющих имена элементов, связанных с проверкой.</span><span class="sxs-lookup"><span data-stu-id="9cf85-148">The ValidationResult constructor takes a string that represents the error message and an array of strings that represent the member names that are associated with the validation.</span></span> <span data-ttu-id="9cf85-149">Так как эта проверка проверяет заголовок и BloggerName, возвращаются как имена свойств.</span><span class="sxs-lookup"><span data-stu-id="9cf85-149">Since this validation checks both the Title and the BloggerName, both property names are returned.</span></span>

<span data-ttu-id="9cf85-150">В отличие от проверки, предоставляемый Fluent API результат этой проверки будет распознаваться модулем представление и обработчик исключений, который я использовал выше, чтобы добавить ошибку в ModelState не требуется.</span><span class="sxs-lookup"><span data-stu-id="9cf85-150">Unlike the validation provided by the Fluent API, this validation result will be recognized by the View and the exception handler that I used earlier to add the error into ModelState is unnecessary.</span></span> <span data-ttu-id="9cf85-151">Поскольку я обоих имен свойств в ValidationResult, MVC HtmlHelpers отображает сообщение об ошибке для оба этих свойства.</span><span class="sxs-lookup"><span data-stu-id="9cf85-151">Because I set both property names in the ValidationResult, the MVC HtmlHelpers display the error message for both of those properties.</span></span>

![figure02](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a><span data-ttu-id="9cf85-153">DbContext.ValidateEntity</span><span class="sxs-lookup"><span data-stu-id="9cf85-153">DbContext.ValidateEntity</span></span>

<span data-ttu-id="9cf85-154">DbContext имеет переопределяемый метод ValidateEntity.</span><span class="sxs-lookup"><span data-stu-id="9cf85-154">DbContext has an Overridable method called ValidateEntity.</span></span> <span data-ttu-id="9cf85-155">При вызове метода SaveChanges, Entity Framework будет вызывать этот метод для каждой сущности в своем кэше, состояние которых отлично без изменений.</span><span class="sxs-lookup"><span data-stu-id="9cf85-155">When you call SaveChanges, Entity Framework will call this method for each entity in its cache whose state is not Unchanged.</span></span> <span data-ttu-id="9cf85-156">Логика проверки можно поместить непосредственно в здесь или даже использовать этот метод для вызова, например, метода Blog.Validate, добавленного в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="9cf85-156">You can put validation logic directly in here or even use this method to call, for example, the Blog.Validate method added in the previous section.</span></span>

<span data-ttu-id="9cf85-157">Ниже приведен пример переопределения ValidateEntity, которое проверяет новых записей, чтобы убедиться, что заголовок post не уже используется.</span><span class="sxs-lookup"><span data-stu-id="9cf85-157">Here’s an example of a ValidateEntity override that validates new Posts to ensure that the post title hasn’t been used already.</span></span> <span data-ttu-id="9cf85-158">Он сначала проверяет, является ли сущность post, и что добавляется свое состояние.</span><span class="sxs-lookup"><span data-stu-id="9cf85-158">It first checks to see if the entity is a post and that its state is Added.</span></span> <span data-ttu-id="9cf85-159">Если это так, он выполняет в базе данных, чтобы увидеть, если уже есть запись с тем же именем.</span><span class="sxs-lookup"><span data-stu-id="9cf85-159">If that’s the case, then it looks in the database to see if there is already a post with the same title.</span></span> <span data-ttu-id="9cf85-160">Если уже имеется сообщение, будет создан новый DbEntityValidationResult.</span><span class="sxs-lookup"><span data-stu-id="9cf85-160">If there is an existing post already, then a new DbEntityValidationResult is created.</span></span>

<span data-ttu-id="9cf85-161">DbEntityValidationResult содержит dbentityentry, который должен и DbValidationErrors из коллекции ICollection для одной сущности.</span><span class="sxs-lookup"><span data-stu-id="9cf85-161">DbEntityValidationResult houses a DbEntityEntry and an ICollection of DbValidationErrors for a single entity.</span></span> <span data-ttu-id="9cf85-162">В начале этого метода DbEntityValidationResult создается и затем добавляются все ошибки, обнаруженные в коллекции ValidationErrors.</span><span class="sxs-lookup"><span data-stu-id="9cf85-162">At the start of this method, a  DbEntityValidationResult is instantiated and then any errors that are discovered are added into its ValidationErrors collection.</span></span>

``` csharp
    protected override DbEntityValidationResult ValidateEntity (
        System.Data.Entity.Infrastructure.DbEntityEntry entityEntry,
        IDictionary\<object, object> items)
    {
        var result = new DbEntityValidationResult(entityEntry, new List<DbValidationError>());
        if (entityEntry.Entity is Post && entityEntry.State == EntityState.Added)
        {
            Post post = entityEntry.Entity as Post;
            //check for uniqueness of post title
            if (Posts.Where(p => p.Title == post.Title).Count() > 0)
            {
                result.ValidationErrors.Add(
                        new System.Data.Entity.Validation.DbValidationError("Title",
                        "Post title must be unique."));
            }
        }

        if (result.ValidationErrors.Count > 0)
        {
            return result;
        }
        else
        {
         return base.ValidateEntity(entityEntry, items);
        }
    }
```

## <a name="explicitly-triggering-validation"></a><span data-ttu-id="9cf85-163">Явного запуска проверки</span><span class="sxs-lookup"><span data-stu-id="9cf85-163">Explicitly triggering validation</span></span>

<span data-ttu-id="9cf85-164">Вызов SaveChanges запускает все проверки, описанные в этой статье.</span><span class="sxs-lookup"><span data-stu-id="9cf85-164">A call to SaveChanges triggers all of the validations covered in this article.</span></span> <span data-ttu-id="9cf85-165">Но не следует полагаться на SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="9cf85-165">But you don’t need to rely on SaveChanges.</span></span> <span data-ttu-id="9cf85-166">Можно проверить в другом месте в приложении.</span><span class="sxs-lookup"><span data-stu-id="9cf85-166">You may prefer to validate elsewhere in your application.</span></span>

<span data-ttu-id="9cf85-167">DbContext.GetValidationErrors запустит все проверки, определенные заметки или Fluent API, проверки, созданные в IValidatableObject (например, Blog.Validate) и либо в DbContext.ValidateEntity проверки метод.</span><span class="sxs-lookup"><span data-stu-id="9cf85-167">DbContext.GetValidationErrors will trigger all of the validations, those defined by annotations or the Fluent API, the validation created in IValidatableObject (for example, Blog.Validate), and the validations performed in the DbContext.ValidateEntity method.</span></span>

<span data-ttu-id="9cf85-168">Следующий код вызовет GetValidationErrors для текущего экземпляра DbContext.</span><span class="sxs-lookup"><span data-stu-id="9cf85-168">The following code will call GetValidationErrors on the current instance of a DbContext.</span></span> <span data-ttu-id="9cf85-169">ValidationErrors группируются по типу сущности в DbValidationRestuls.</span><span class="sxs-lookup"><span data-stu-id="9cf85-169">ValidationErrors are grouped by entity type into DbValidationRestuls.</span></span> <span data-ttu-id="9cf85-170">Код сначала проходит через DbValidationResults, возвращаемый методом, а затем проходят через каждый ValidationError внутри.</span><span class="sxs-lookup"><span data-stu-id="9cf85-170">The code iterates first through the DbValidationResults returned by the method and then through each ValidationError inside.</span></span>

``` csharp
    foreach (var validationResults in db.GetValidationErrors())
        {
            foreach (var error in validationResults.ValidationErrors)
            {
                Debug.WriteLine(
                                  "Entity Property: {0}, Error {1}",
                                  error.PropertyName,
                                  error.ErrorMessage);
            }
        }
```

## <a name="other-considerations-when-using-validation"></a><span data-ttu-id="9cf85-171">Дополнительные рекомендации при использовании проверки</span><span class="sxs-lookup"><span data-stu-id="9cf85-171">Other considerations when using validation</span></span>

<span data-ttu-id="9cf85-172">Ниже приведены несколько аспекты, которые следует учитывать при использовании Entity Framework проверки.</span><span class="sxs-lookup"><span data-stu-id="9cf85-172">Here are a few other points to consider when using Entity Framework validation:</span></span>

-   <span data-ttu-id="9cf85-173">Отложенная загрузка отключена во время проверки.</span><span class="sxs-lookup"><span data-stu-id="9cf85-173">Lazy loading is disabled during validation.</span></span>
-   <span data-ttu-id="9cf85-174">EF проверит заметок к данным на несопоставленные свойства, которые не сопоставлены со столбцом в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9cf85-174">EF will validate data annotations on non-mapped properties (properties that are not mapped to a column in the database).</span></span>
-   <span data-ttu-id="9cf85-175">Проверка выполняется после обнаружения изменений во время SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="9cf85-175">Validation is performed after changes are detected during SaveChanges.</span></span> <span data-ttu-id="9cf85-176">При внесении изменений во время проверки отвечает для уведомления объекта отслеживания изменений.</span><span class="sxs-lookup"><span data-stu-id="9cf85-176">If you make changes during validation it is your responsibility to notify the change tracker.</span></span>
-   <span data-ttu-id="9cf85-177">DbUnexpectedValidationException возникает в том случае, если возникают ошибки во время проверки.</span><span class="sxs-lookup"><span data-stu-id="9cf85-177">DbUnexpectedValidationException is thrown if errors occur during validation.</span></span>
-   <span data-ttu-id="9cf85-178">Аспекты, которые Entity Framework включаются в модель (максимальной длины, т. д.) вызовет проверку, даже в том случае, если нет заметок к данным в классах и (или) конструктор EF вы использовали для создания модели.</span><span class="sxs-lookup"><span data-stu-id="9cf85-178">Facets that Entity Framework includes in the model (maximum length, required, etc.) will cause validation, even if there are not data annotations on your classes and/or you used the EF Designer to create your model.</span></span>
-   <span data-ttu-id="9cf85-179">Правила приоритета:</span><span class="sxs-lookup"><span data-stu-id="9cf85-179">Precedence rules:</span></span>
    -   <span data-ttu-id="9cf85-180">Вызовы Fluent API переопределить соответствующие заметки к данным</span><span class="sxs-lookup"><span data-stu-id="9cf85-180">Fluent API calls override the corresponding data annotations</span></span>
-   <span data-ttu-id="9cf85-181">Порядок выполнения:</span><span class="sxs-lookup"><span data-stu-id="9cf85-181">Execution order:</span></span>
    -   <span data-ttu-id="9cf85-182">Проверка свойства предшествует проверки типа</span><span class="sxs-lookup"><span data-stu-id="9cf85-182">Property validation occurs before type validation</span></span>
    -   <span data-ttu-id="9cf85-183">Проверка типа происходят, если свойство проверка прошла успешно</span><span class="sxs-lookup"><span data-stu-id="9cf85-183">Type validation only occurs if property validation succeeds</span></span>
-   <span data-ttu-id="9cf85-184">Если свойство является сложным проверки также будет включать:</span><span class="sxs-lookup"><span data-stu-id="9cf85-184">If a property is complex its validation will also include:</span></span>
    -   <span data-ttu-id="9cf85-185">Проверка уровня свойств в свойства сложного типа</span><span class="sxs-lookup"><span data-stu-id="9cf85-185">Property-level validation on the complex type properties</span></span>
    -   <span data-ttu-id="9cf85-186">Тип уровня проверки в сложном типе, включая проверку IValidatableObject сложного типа</span><span class="sxs-lookup"><span data-stu-id="9cf85-186">Type level validation on the complex type, including IValidatableObject validation on the complex type</span></span>

## <a name="summary"></a><span data-ttu-id="9cf85-187">Сводка</span><span class="sxs-lookup"><span data-stu-id="9cf85-187">Summary</span></span>

<span data-ttu-id="9cf85-188">Проверка API на платформе Entity Framework играет очень хорошо с проверки на стороне клиента в MVC, но не нужно полагаться на проверку на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="9cf85-188">The validation API in Entity Framework plays very nicely with client side validation in MVC but you don't have to rely on client-side validation.</span></span> <span data-ttu-id="9cf85-189">Платформа Entity Framework позаботится проверки на стороне сервера для DataAnnotations или конфигураций, которые вы применили с Fluent API для code first.</span><span class="sxs-lookup"><span data-stu-id="9cf85-189">Entity Framework will take care of the validation on the server side for DataAnnotations or configurations you've applied with the code first Fluent API.</span></span>

<span data-ttu-id="9cf85-190">Вы также узнали ряд точек расширения для настройки поведения с помощью интерфейса IValidatableObject или коснитесь в метод DbContet.ValidateEntity.</span><span class="sxs-lookup"><span data-stu-id="9cf85-190">You also saw a number of extensibility points for customizing the behavior whether you use the IValidatableObject interface or tap into the DbContet.ValidateEntity method.</span></span> <span data-ttu-id="9cf85-191">И эти последние два средства для проверки доступны через DbContext, как при использовании Code First, Model First или Database First рабочего процесса для описания концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="9cf85-191">And these last two means of validation are available through the DbContext, whether you use the Code First, Model First or Database First workflow to describe your conceptual model.</span></span>
