---
title: Проверка — EF6
description: Проверка в Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 0e0f5c581681e7b21b6591ade1541d774e7280bc
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89619816"
---
# <a name="data-validation"></a><span data-ttu-id="7aacc-103">Проверка данных</span><span class="sxs-lookup"><span data-stu-id="7aacc-103">Data Validation</span></span>
> [!NOTE]
> <span data-ttu-id="7aacc-104">**EF 4.1 только в сторону** — функции, API и т. д., обсуждаемые на этой странице, появились в Entity Framework 4,1.</span><span class="sxs-lookup"><span data-stu-id="7aacc-104">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="7aacc-105">Если вы используете более раннюю версию, некоторые или все сведения не применяются</span><span class="sxs-lookup"><span data-stu-id="7aacc-105">If you are using an earlier version, some or all of the information does not apply</span></span>

<span data-ttu-id="7aacc-106">Содержимое этой страницы адаптируется из статьи, первоначально написанной Джулия Лерман ( [https://thedatafarm.com](https://thedatafarm.com) ).</span><span class="sxs-lookup"><span data-stu-id="7aacc-106">The content on this page is adapted from an article originally written by Julie Lerman ([https://thedatafarm.com](https://thedatafarm.com)).</span></span>

<span data-ttu-id="7aacc-107">Entity Framework предоставляет широкий спектр функций проверки, которые могут передавать в пользовательский интерфейс для проверки на стороне клиента или использоваться для проверки на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="7aacc-107">Entity Framework provides a great variety of validation features that can feed through to a user interface for client-side validation or be used for server-side validation.</span></span> <span data-ttu-id="7aacc-108">При первом использовании кода можно указать проверки с помощью заметок или конфигураций API-интерфейса Fluent.</span><span class="sxs-lookup"><span data-stu-id="7aacc-108">When using code first, you can specify validations using annotation or fluent API configurations.</span></span> <span data-ttu-id="7aacc-109">Дополнительные проверки и более сложные, могут быть указаны в коде и будут работать независимо от того, приехал модель сначала от кода, модель First или Database.</span><span class="sxs-lookup"><span data-stu-id="7aacc-109">Additional validations, and more complex, can be specified in code and will work whether your model hails from code first, model first or database first.</span></span>

## <a name="the-model"></a><span data-ttu-id="7aacc-110">Модель</span><span class="sxs-lookup"><span data-stu-id="7aacc-110">The model</span></span>

<span data-ttu-id="7aacc-111">Я продемонстрирую проверки с помощью простой пары классов: блог и POST.</span><span class="sxs-lookup"><span data-stu-id="7aacc-111">I'll demonstrate the validations with a simple pair of classes: Blog and Post.</span></span>

``` csharp
public class Blog
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string BloggerName { get; set; }
    public DateTime DateCreated { get; set; }
    public virtual ICollection<Post> Posts { get; set; }
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

## <a name="data-annotations"></a><span data-ttu-id="7aacc-112">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="7aacc-112">Data Annotations</span></span>

<span data-ttu-id="7aacc-113">Code First использует аннотации из `System.ComponentModel.DataAnnotations` сборки как одно из способов настройки первых классов кода.</span><span class="sxs-lookup"><span data-stu-id="7aacc-113">Code First uses annotations from the `System.ComponentModel.DataAnnotations` assembly as one means of configuring code first classes.</span></span> <span data-ttu-id="7aacc-114">К этим заметкам относятся те, которые предоставляют такие правила `Required` , как, `MaxLength` и `MinLength` .</span><span class="sxs-lookup"><span data-stu-id="7aacc-114">Among these annotations are those which provide rules such as the `Required`, `MaxLength` and `MinLength`.</span></span> <span data-ttu-id="7aacc-115">Некоторые клиентские приложения .NET также распознают эти заметки, например ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7aacc-115">A number of .NET client applications also recognize these annotations, for example, ASP.NET MVC.</span></span> <span data-ttu-id="7aacc-116">С этими заметками можно выполнять проверку на стороне клиента и на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="7aacc-116">You can achieve both client side and server side validation with these annotations.</span></span> <span data-ttu-id="7aacc-117">Например, можно принудительно задать свойство заголовка блога как обязательное свойство.</span><span class="sxs-lookup"><span data-stu-id="7aacc-117">For example, you can force the Blog Title property to be a required property.</span></span>

``` csharp
[Required]
public string Title { get; set; }
```

<span data-ttu-id="7aacc-118">При отсутствии в приложении дополнительного кода или изменений разметки существующее приложение MVC будет выполнять проверку на стороне клиента, даже динамически создавая сообщение, используя имена свойств и заметок.</span><span class="sxs-lookup"><span data-stu-id="7aacc-118">With no additional code or markup changes in the application, an existing MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![Рисунок 1](~/ef6/media/figure01.png)

<span data-ttu-id="7aacc-120">В методе обратного вызова этого представления создания Entity Framework используется для сохранения нового блога в базе данных, но проверка на стороне клиента MVC активируется до того, как приложение достигнет этого кода.</span><span class="sxs-lookup"><span data-stu-id="7aacc-120">In the post back method of this Create view, Entity Framework is used to save the new blog to the database, but MVC's client-side validation is triggered before the application reaches that code.</span></span>

<span data-ttu-id="7aacc-121">Однако проверка на стороне клиента не является маркированной.</span><span class="sxs-lookup"><span data-stu-id="7aacc-121">Client side validation is not bullet-proof however.</span></span> <span data-ttu-id="7aacc-122">Пользователи могут повлиять на функции браузера или, что еще хуже, злоумышленник может использовать некоторую хитрость во избежание проверки пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="7aacc-122">Users can impact features of their browser or worse yet, a hacker might use some trickery to avoid the UI validations.</span></span> <span data-ttu-id="7aacc-123">Но Entity Framework также будет распознавать `Required` заметку и проверять ее.</span><span class="sxs-lookup"><span data-stu-id="7aacc-123">But Entity Framework will also recognize the `Required` annotation and validate it.</span></span>

<span data-ttu-id="7aacc-124">Простой способ проверки заключается в отключении функции проверки на стороне клиента MVC.</span><span class="sxs-lookup"><span data-stu-id="7aacc-124">A simple way to test this is to disable MVC's client-side validation feature.</span></span> <span data-ttu-id="7aacc-125">Это можно сделать в файле web.config приложения MVC.</span><span class="sxs-lookup"><span data-stu-id="7aacc-125">You can do this in the MVC application's web.config file.</span></span> <span data-ttu-id="7aacc-126">Раздел appSettings содержит ключ для Клиентвалидатионенаблед.</span><span class="sxs-lookup"><span data-stu-id="7aacc-126">The appSettings section has a key for ClientValidationEnabled.</span></span> <span data-ttu-id="7aacc-127">Если задать для этого ключа значение false, Пользовательский интерфейс не сможет выполнять проверки.</span><span class="sxs-lookup"><span data-stu-id="7aacc-127">Setting this key to false will prevent the UI from performing validations.</span></span>

``` xml
<appSettings>
    <add key="ClientValidationEnabled"value="false"/>
    ...
</appSettings>
```

<span data-ttu-id="7aacc-128">Даже если проверка на стороне клиента отключена, вы получите тот же ответ в приложении.</span><span class="sxs-lookup"><span data-stu-id="7aacc-128">Even with the client-side validation disabled, you will get the same response in your application.</span></span> <span data-ttu-id="7aacc-129">Сообщение об ошибке "поле заголовка является обязательным" будет отображаться как ранее.</span><span class="sxs-lookup"><span data-stu-id="7aacc-129">The error message "The Title field is required" will be displayed as before.</span></span> <span data-ttu-id="7aacc-130">Кроме того, теперь это будет результатом проверки на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="7aacc-130">Except now it will be a result of server-side validation.</span></span> <span data-ttu-id="7aacc-131">Entity Framework будет выполнять проверку `Required` заметки (перед тем, как они смогут создать `INSERT` команду для отправки в базу данных) и вернуть ошибку в MVC, которая отобразит сообщение.</span><span class="sxs-lookup"><span data-stu-id="7aacc-131">Entity Framework will perform the validation on the `Required` annotation (before it even bothers to build an `INSERT` command to send to the database) and return the error to MVC which will display the message.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="7aacc-132">Текучий API</span><span class="sxs-lookup"><span data-stu-id="7aacc-132">Fluent API</span></span>

<span data-ttu-id="7aacc-133">Для получения той же клиентской стороны & проверку на стороне сервера можно использовать вместо заметок интерфейс API для первого кода.</span><span class="sxs-lookup"><span data-stu-id="7aacc-133">You can use code first's fluent API instead of annotations to get the same client side & server side validation.</span></span> <span data-ttu-id="7aacc-134">Вместо `Required` этого я покажу это с помощью проверки MaxLength.</span><span class="sxs-lookup"><span data-stu-id="7aacc-134">Rather than use `Required`, I'll show you this using a MaxLength validation.</span></span>

<span data-ttu-id="7aacc-135">Конфигурации API Fluent применяются в качестве кода сначала — это сборка модели из классов.</span><span class="sxs-lookup"><span data-stu-id="7aacc-135">Fluent API configurations are applied as code first is building the model from the classes.</span></span> <span data-ttu-id="7aacc-136">Можно внедрить конфигурации, переопределив метод OnModelCreating класса DbContext.</span><span class="sxs-lookup"><span data-stu-id="7aacc-136">You can inject the configurations by overriding the DbContext class' OnModelCreating  method.</span></span> <span data-ttu-id="7aacc-137">Ниже приведена конфигурация, указывающая, что свойство Блогжернаме может содержать не более 10 символов.</span><span class="sxs-lookup"><span data-stu-id="7aacc-137">Here is a configuration specifying that the BloggerName property can be no longer than 10 characters.</span></span>

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

<span data-ttu-id="7aacc-138">Ошибки проверки, возникающие на основе конфигураций API Fluent, не будут автоматически достигать пользовательского интерфейса, но вы можете записать его в код, а затем ответить на него соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="7aacc-138">Validation errors thrown based on the Fluent API configurations will not automatically reach the UI, but you can capture it in code and then respond to it accordingly.</span></span>

<span data-ttu-id="7aacc-139">Ниже приведен код ошибки обработки исключений в классе Блогконтроллер приложения, который фиксирует эту ошибку проверки, когда Entity Framework пытается сохранить блог с Блогжернаме, длина которого превышает максимальное значение в 10 символов.</span><span class="sxs-lookup"><span data-stu-id="7aacc-139">Here's some exception handling error code in the application's BlogController class that captures that validation error when Entity Framework attempts to save a blog with a BloggerName that exceeds the 10 character maximum.</span></span>

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
    catch (DbEntityValidationException ex)
    {
        var error = ex.EntityValidationErrors.First().ValidationErrors.First();
        this.ModelState.AddModelError(error.PropertyName, error.ErrorMessage);
        return View();
    }
}
```

<span data-ttu-id="7aacc-140">Проверка не автоматически передается обратно в представление, поэтому используется дополнительный код, используемый `ModelState.AddModelError` .</span><span class="sxs-lookup"><span data-stu-id="7aacc-140">The validation doesn't automatically get passed back into the view which is why the additional code that uses `ModelState.AddModelError` is being used.</span></span> <span data-ttu-id="7aacc-141">Это гарантирует, что сведения об ошибке будут внесены в представление, которое затем будет использовать `ValidationMessageFor` HtmlHelper для отображения ошибки.</span><span class="sxs-lookup"><span data-stu-id="7aacc-141">This ensures that the error details make it to the view which will then use the `ValidationMessageFor` Htmlhelper to display the error.</span></span>

``` csharp
@Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a><span data-ttu-id="7aacc-142">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="7aacc-142">IValidatableObject</span></span>

<span data-ttu-id="7aacc-143">`IValidatableObject` — Это интерфейс, который находится в `System.ComponentModel.DataAnnotations` .</span><span class="sxs-lookup"><span data-stu-id="7aacc-143">`IValidatableObject` is an interface that lives in `System.ComponentModel.DataAnnotations`.</span></span> <span data-ttu-id="7aacc-144">Хотя он не входит в Entity Framework API, вы по-прежнему можете использовать его для проверки на стороне сервера в классах Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7aacc-144">While it is not part of the Entity Framework API, you can still leverage it for server-side validation in your Entity Framework classes.</span></span> <span data-ttu-id="7aacc-145">`IValidatableObject` предоставляет `Validate` метод, который Entity Framework будет вызываться во время вызова SaveChanges, или вы можете вызвать его каждый раз, когда требуется проверить классы.</span><span class="sxs-lookup"><span data-stu-id="7aacc-145">`IValidatableObject` provides a `Validate` method that Entity Framework will call during SaveChanges or you can call yourself any time you want to validate the classes.</span></span>

<span data-ttu-id="7aacc-146">Конфигурации, такие как `Required` и, `MaxLength` выполняют проверку одного поля.</span><span class="sxs-lookup"><span data-stu-id="7aacc-146">Configurations such as `Required` and `MaxLength` perform validation on a single field.</span></span> <span data-ttu-id="7aacc-147">В `Validate` методе можно использовать еще более сложную логику, например сравнить два поля.</span><span class="sxs-lookup"><span data-stu-id="7aacc-147">In the `Validate` method you can have even more complex logic, for example, comparing two fields.</span></span>

<span data-ttu-id="7aacc-148">В следующем примере `Blog` класс был расширен для реализации, `IValidatableObject` а затем предоставляет правило, которое `Title` `BloggerName` не может совпадать с.</span><span class="sxs-lookup"><span data-stu-id="7aacc-148">In the following example, the `Blog` class has been extended to implement `IValidatableObject` and then provide a rule that the `Title` and `BloggerName` cannot match.</span></span>

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
            yield return new ValidationResult(
                "Blog Title cannot match Blogger Name",
                new[] { nameof(Title), nameof(BloggerName) });
        }
    }
}
```

<span data-ttu-id="7aacc-149">`ValidationResult`Конструктор принимает объект `string` , представляющий сообщение об ошибке, и массив `string` s, который представляет имена элементов, связанных с проверкой.</span><span class="sxs-lookup"><span data-stu-id="7aacc-149">The `ValidationResult` constructor takes a `string` that represents the error message and an array of `string`s that represent the member names that are associated with the validation.</span></span> <span data-ttu-id="7aacc-150">Так как эта проверка проверяет как `Title` , так и `BloggerName` , возвращаются оба имени свойства.</span><span class="sxs-lookup"><span data-stu-id="7aacc-150">Since this validation checks both the `Title` and the `BloggerName`, both property names are returned.</span></span>

<span data-ttu-id="7aacc-151">В отличие от проверки, предоставляемой API Fluent, этот результат проверки будет распознан представлением, а обработчик исключений, который я использовал ранее для добавления ошибки в `ModelState` , не нужен.</span><span class="sxs-lookup"><span data-stu-id="7aacc-151">Unlike the validation provided by the Fluent API, this validation result will be recognized by the View and the exception handler that I used earlier to add the error into `ModelState` is unnecessary.</span></span> <span data-ttu-id="7aacc-152">Так как я задали оба имени свойств в `ValidationResult` , MVC хтмлхелперс отображает сообщение об ошибке для обоих этих свойств.</span><span class="sxs-lookup"><span data-stu-id="7aacc-152">Because I set both property names in the `ValidationResult`, the MVC HtmlHelpers display the error message for both of those properties.</span></span>

![рис. 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a><span data-ttu-id="7aacc-154">DbContext. Валидатинтити</span><span class="sxs-lookup"><span data-stu-id="7aacc-154">DbContext.ValidateEntity</span></span>

<span data-ttu-id="7aacc-155">`DbContext` имеет вызванный метод Overridable `ValidateEntity` .</span><span class="sxs-lookup"><span data-stu-id="7aacc-155">`DbContext` has an overridable method called `ValidateEntity`.</span></span> <span data-ttu-id="7aacc-156">При вызове `SaveChanges` метода Entity Framework будет вызывать этот метод для каждой сущности в кэше, состояние которого не равно `Unchanged` .</span><span class="sxs-lookup"><span data-stu-id="7aacc-156">When you call `SaveChanges`, Entity Framework will call this method for each entity in its cache whose state is not `Unchanged`.</span></span> <span data-ttu-id="7aacc-157">Логику проверки можно прямо здесь или даже использовать этот метод для вызова, например, `Blog.Validate` метода, добавленного в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="7aacc-157">You can put validation logic directly in here or even use this method to call, for example, the `Blog.Validate` method added in the previous section.</span></span>

<span data-ttu-id="7aacc-158">Ниже приведен пример `ValidateEntity` переопределения, которое проверяет новые объекты, `Post` чтобы убедиться, что заголовок записи еще не используется.</span><span class="sxs-lookup"><span data-stu-id="7aacc-158">Here's an example of a `ValidateEntity` override that validates new `Post`s to ensure that the post title hasn't been used already.</span></span> <span data-ttu-id="7aacc-159">Сначала он проверяет, является ли сущность записью, и добавляет ее состояние.</span><span class="sxs-lookup"><span data-stu-id="7aacc-159">It first checks to see if the entity is a post and that its state is Added.</span></span> <span data-ttu-id="7aacc-160">Если это так, то он просматривает базу данных, чтобы проверить, существует ли запись с тем же названием.</span><span class="sxs-lookup"><span data-stu-id="7aacc-160">If that's the case, then it looks in the database to see if there is already a post with the same title.</span></span> <span data-ttu-id="7aacc-161">Если уже есть существующая публикация, `DbEntityValidationResult` создается новый.</span><span class="sxs-lookup"><span data-stu-id="7aacc-161">If there is an existing post already, then a new `DbEntityValidationResult` is created.</span></span>

<span data-ttu-id="7aacc-162">`DbEntityValidationResult` применяет `DbEntityEntry` и `ICollection<DbValidationErrors>` для одной сущности.</span><span class="sxs-lookup"><span data-stu-id="7aacc-162">`DbEntityValidationResult` houses a `DbEntityEntry` and an `ICollection<DbValidationErrors>` for a single entity.</span></span> <span data-ttu-id="7aacc-163">В начале этого метода создается экземпляр класса, `DbEntityValidationResult` а затем все обнаруженные ошибки добавляются в `ValidationErrors` коллекцию.</span><span class="sxs-lookup"><span data-stu-id="7aacc-163">At the start of this method, a `DbEntityValidationResult` is instantiated and then any errors that are discovered are added into its `ValidationErrors` collection.</span></span>

``` csharp
protected override DbEntityValidationResult ValidateEntity (
    System.Data.Entity.Infrastructure.DbEntityEntry entityEntry,
    IDictionary<object, object> items)
{
    var result = new DbEntityValidationResult(entityEntry, new List<DbValidationError>());

    if (entityEntry.Entity is Post post && entityEntry.State == EntityState.Added)
    {
        // Check for uniqueness of post title
        if (Posts.Where(p => p.Title == post.Title).Any())
        {
            result.ValidationErrors.Add(
                    new System.Data.Entity.Validation.DbValidationError(
                        nameof(Title),
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

## <a name="explicitly-triggering-validation"></a><span data-ttu-id="7aacc-164">Явная активация проверки</span><span class="sxs-lookup"><span data-stu-id="7aacc-164">Explicitly triggering validation</span></span>

<span data-ttu-id="7aacc-165">Вызов метода `SaveChanges` запускает все проверки, описанные в этой статье.</span><span class="sxs-lookup"><span data-stu-id="7aacc-165">A call to `SaveChanges` triggers all of the validations covered in this article.</span></span> <span data-ttu-id="7aacc-166">Но вам не нужно полагаться на `SaveChanges` .</span><span class="sxs-lookup"><span data-stu-id="7aacc-166">But you don't need to rely on `SaveChanges`.</span></span> <span data-ttu-id="7aacc-167">Вы можете предпочесть проверку в любом расположении приложения.</span><span class="sxs-lookup"><span data-stu-id="7aacc-167">You may prefer to validate elsewhere in your application.</span></span>

<span data-ttu-id="7aacc-168">`DbContext.GetValidationErrors` запустит все проверки, заданные заметками или API-интерфейсом Fluent, проверку, созданную в `IValidatableObject` (например, `Blog.Validate` ), и проверок, выполненных в `DbContext.ValidateEntity` методе.</span><span class="sxs-lookup"><span data-stu-id="7aacc-168">`DbContext.GetValidationErrors` will trigger all of the validations, those defined by annotations or the Fluent API, the validation created in `IValidatableObject` (for example, `Blog.Validate`), and the validations performed in the `DbContext.ValidateEntity` method.</span></span>

<span data-ttu-id="7aacc-169">Следующий код вызовет `GetValidationErrors` в текущем экземпляре `DbContext` .</span><span class="sxs-lookup"><span data-stu-id="7aacc-169">The following code will call `GetValidationErrors` on the current instance of a `DbContext`.</span></span> <span data-ttu-id="7aacc-170">`ValidationErrors` группируются по типу сущности в `DbEntityValidationResult` .</span><span class="sxs-lookup"><span data-stu-id="7aacc-170">`ValidationErrors` are grouped by entity type into `DbEntityValidationResult`.</span></span> <span data-ttu-id="7aacc-171">Код сначала проходит через `DbEntityValidationResult` s, возвращенный методом, а затем через каждый объект `DbValidationError` .</span><span class="sxs-lookup"><span data-stu-id="7aacc-171">The code iterates first through the `DbEntityValidationResult`s returned by the method and then through each `DbValidationError` inside.</span></span>

``` csharp
foreach (var validationResult in db.GetValidationErrors())
{
    foreach (var error in validationResult.ValidationErrors)
    {
        Debug.WriteLine(
            "Entity Property: {0}, Error {1}",
            error.PropertyName,
            error.ErrorMessage);
    }
}
```

## <a name="other-considerations-when-using-validation"></a><span data-ttu-id="7aacc-172">Другие соображения при использовании проверки</span><span class="sxs-lookup"><span data-stu-id="7aacc-172">Other considerations when using validation</span></span>

<span data-ttu-id="7aacc-173">Ниже приведены некоторые другие моменты, которые следует учитывать при использовании проверки Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7aacc-173">Here are a few other points to consider when using Entity Framework validation:</span></span>

- <span data-ttu-id="7aacc-174">Отложенная загрузка отключена во время проверки</span><span class="sxs-lookup"><span data-stu-id="7aacc-174">Lazy loading is disabled during validation</span></span>
- <span data-ttu-id="7aacc-175">EF будет проверять заметки данных на несопоставленных свойствах (свойства, которые не сопоставлены со столбцом в базе данных)</span><span class="sxs-lookup"><span data-stu-id="7aacc-175">EF will validate data annotations on non-mapped properties (properties that are not mapped to a column in the database)</span></span>
- <span data-ttu-id="7aacc-176">Проверка выполняется после обнаружения изменений во время `SaveChanges` .</span><span class="sxs-lookup"><span data-stu-id="7aacc-176">Validation is performed after changes are detected during `SaveChanges`.</span></span> <span data-ttu-id="7aacc-177">При внесении изменений во время проверки вы отвечаете за уведомление средства записи изменений.</span><span class="sxs-lookup"><span data-stu-id="7aacc-177">If you make changes during validation it is your responsibility to notify the change tracker</span></span>
- <span data-ttu-id="7aacc-178">`DbUnexpectedValidationException` возникает, если во время проверки возникают ошибки</span><span class="sxs-lookup"><span data-stu-id="7aacc-178">`DbUnexpectedValidationException` is thrown if errors occur during validation</span></span>
- <span data-ttu-id="7aacc-179">Аспекты, которые Entity Framework включают в модель (максимальная длина, обязательный и т. д.), будут вызывать проверку, даже если в классах нет заметок к данным и/или если вы использовали конструктор EF для создания модели.</span><span class="sxs-lookup"><span data-stu-id="7aacc-179">Facets that Entity Framework includes in the model (maximum length, required, etc.) will cause validation, even if there are no data annotations in your classes and/or you used the EF Designer to create your model</span></span>
- <span data-ttu-id="7aacc-180">Правила приоритета:</span><span class="sxs-lookup"><span data-stu-id="7aacc-180">Precedence rules:</span></span>
  - <span data-ttu-id="7aacc-181">Вызовы API Fluent переопределяют соответствующие заметки к данным</span><span class="sxs-lookup"><span data-stu-id="7aacc-181">Fluent API calls override the corresponding data annotations</span></span>
- <span data-ttu-id="7aacc-182">Порядок выполнения:</span><span class="sxs-lookup"><span data-stu-id="7aacc-182">Execution order:</span></span>
  - <span data-ttu-id="7aacc-183">Проверка свойства выполняется перед проверкой типа</span><span class="sxs-lookup"><span data-stu-id="7aacc-183">Property validation occurs before type validation</span></span>
  - <span data-ttu-id="7aacc-184">Проверка типа выполняется только в случае, если проверка свойства прошла</span><span class="sxs-lookup"><span data-stu-id="7aacc-184">Type validation only occurs if property validation succeeds</span></span>
- <span data-ttu-id="7aacc-185">Если свойство является сложным, его проверка также будет включать:</span><span class="sxs-lookup"><span data-stu-id="7aacc-185">If a property is complex, its validation will also include:</span></span>
  - <span data-ttu-id="7aacc-186">Проверка на уровне свойств для свойств сложного типа</span><span class="sxs-lookup"><span data-stu-id="7aacc-186">Property-level validation on the complex type properties</span></span>
  - <span data-ttu-id="7aacc-187">Проверка на уровне типа для сложного типа, включая `IValidatableObject` проверку сложного типа</span><span class="sxs-lookup"><span data-stu-id="7aacc-187">Type level validation on the complex type, including `IValidatableObject` validation on the complex type</span></span>

## <a name="summary"></a><span data-ttu-id="7aacc-188">Итоги</span><span class="sxs-lookup"><span data-stu-id="7aacc-188">Summary</span></span>

<span data-ttu-id="7aacc-189">API проверки в Entity Framework очень хорошо играет с проверкой на стороне клиента в MVC, но не нужно полагаться на проверку на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="7aacc-189">The validation API in Entity Framework plays very nicely with client side validation in MVC but you don't have to rely on client-side validation.</span></span> <span data-ttu-id="7aacc-190">Entity Framework позаботится о проверке на стороне сервера для заметок к данным или конфигураций, примененных с помощью API Code First Fluent.</span><span class="sxs-lookup"><span data-stu-id="7aacc-190">Entity Framework will take care of the validation on the server side for DataAnnotations or configurations you've applied with the code first Fluent API.</span></span>

<span data-ttu-id="7aacc-191">Вы также узнали о ряде точек расширения для настройки поведения при использовании `IValidatableObject` интерфейса или касании `DbContext.ValidateEntity` метода.</span><span class="sxs-lookup"><span data-stu-id="7aacc-191">You also saw a number of extensibility points for customizing the behavior whether you use the `IValidatableObject` interface or tap into the `DbContext.ValidateEntity` method.</span></span> <span data-ttu-id="7aacc-192">И эти два последних способа проверки доступны в `DbContext` , независимо от того, используется ли Code First, Model First или Database First рабочего процесса для описания концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="7aacc-192">And these last two means of validation are available through the `DbContext`, whether you use the Code First, Model First or Database First workflow to describe your conceptual model.</span></span>
