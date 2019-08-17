---
title: Проверка — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 457af0c32f0fe4804dbfe6e348664efb1af517c9
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565374"
---
# <a name="data-validation"></a>Проверка данных
> [!NOTE]
> **EF 4.1 только в сторону** — функции, API и т. д., обсуждаемые на этой странице, появились в Entity Framework 4,1. Если вы используете более раннюю версию, некоторые или все сведения не применяются

Содержимое этой страницы адаптируется из статьи, первоначально написанной Джулия Лерман ([http://thedatafarm.com](http://thedatafarm.com)).

Entity Framework предоставляет широкий спектр функций проверки, которые могут передавать в пользовательский интерфейс для проверки на стороне клиента или использоваться для проверки на стороне сервера. При первом использовании кода можно указать проверки с помощью заметок или конфигураций API-интерфейса Fluent. Дополнительные проверки и более сложные, могут быть указаны в коде и будут работать независимо от того, приехал модель сначала от кода, модель First или Database.

## <a name="the-model"></a>Модель

Я продемонстрирую проверки с помощью простой пары классов: Блог и POST.

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

## <a name="data-annotations"></a>Заметки к данным

Code First использует аннотации из `System.ComponentModel.DataAnnotations` сборки как одно из способов настройки первых классов кода. К этим заметкам относятся те `Required`, которые предоставляют такие правила, как, `MaxLength` и `MinLength`. Некоторые клиентские приложения .NET также распознают эти заметки, например ASP.NET MVC. С этими заметками можно выполнять проверку на стороне клиента и на стороне сервера. Например, можно принудительно задать свойство заголовка блога как обязательное свойство.

``` csharp
[Required]
public string Title { get; set; }
```

При отсутствии в приложении дополнительного кода или изменений разметки существующее приложение MVC будет выполнять проверку на стороне клиента, даже динамически создавая сообщение, используя имена свойств и заметок.

![рис. 1](~/ef6/media/figure01.png)

В методе обратного вызова этого представления создания Entity Framework используется для сохранения нового блога в базе данных, но проверка на стороне клиента MVC активируется до того, как приложение достигнет этого кода.

Однако проверка на стороне клиента не является маркированной. Пользователи могут повлиять на функции браузера или, что еще хуже, злоумышленник может использовать некоторую хитрость во избежание проверки пользовательского интерфейса. Но Entity Framework также будет распознавать `Required` заметку и проверять ее.

Простой способ проверки заключается в отключении функции проверки на стороне клиента MVC. Это можно сделать в файле Web. config приложения MVC. Раздел appSettings содержит ключ для Клиентвалидатионенаблед. Если задать для этого ключа значение false, Пользовательский интерфейс не сможет выполнять проверки.

``` xml
<appSettings>
    <add key="ClientValidationEnabled"value="false"/>
    ...
</appSettings>
```

Даже если проверка на стороне клиента отключена, вы получите тот же ответ в приложении. Сообщение об ошибке "поле заголовка является обязательным" будет отображаться как ранее. Кроме того, теперь это будет результатом проверки на стороне сервера. Entity Framework будет выполнять проверку `Required` заметки (перед тем, как они смогут `INSERT` создать команду для отправки в базу данных) и вернуть ошибку в MVC, которая отобразит сообщение.

## <a name="fluent-api"></a>Текучий API

Для получения той же клиентской стороны & проверку на стороне сервера можно использовать вместо заметок интерфейс API для первого кода. `Required`Вместо этого я покажу это с помощью проверки MaxLength.

Конфигурации API Fluent применяются в качестве кода сначала — это сборка модели из классов. Можно внедрить конфигурации, переопределив метод OnModelCreating класса DbContext. Ниже приведена конфигурация, указывающая, что свойство Блогжернаме может содержать не более 10 символов.

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

Ошибки проверки, возникающие на основе конфигураций API Fluent, не будут автоматически достигать пользовательского интерфейса, но вы можете записать его в код, а затем ответить на него соответствующим образом.

Ниже приведен код ошибки обработки исключений в классе Блогконтроллер приложения, который фиксирует эту ошибку проверки, когда Entity Framework пытается сохранить блог с Блогжернаме, длина которого превышает максимальное значение в 10 символов.

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

Проверка не автоматически передается обратно в представление, поэтому используется дополнительный код, `ModelState.AddModelError` используемый. Это гарантирует, что сведения об ошибке будут внесены в представление, которое затем `ValidationMessageFor` будет использовать HtmlHelper для отображения ошибки.

``` csharp
@Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a>IValidatableObject

`IValidatableObject`— Это интерфейс, который находится `System.ComponentModel.DataAnnotations`в. Хотя он не входит в Entity Framework API, вы по-прежнему можете использовать его для проверки на стороне сервера в классах Entity Framework. `IValidatableObject``Validate` предоставляет метод, который Entity Framework будет вызываться во время вызова SaveChanges, или вы можете вызвать его каждый раз, когда требуется проверить классы.

Конфигурации, такие `Required` как `MaxLength` и, выполняют проверку одного поля. `Validate` В методе можно использовать еще более сложную логику, например сравнить два поля.

В следующем примере `Blog` класс был расширен для реализации `IValidatableObject` , а затем `Title` предоставляет правило, которое `BloggerName` не может совпадать с.

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

Конструктор принимает объект `string` , представляющий `string`сообщение об ошибке, и массив s, который представляет имена элементов, связанных с проверкой. `ValidationResult` Так как эта проверка проверяет как `Title` `BloggerName`, так и, возвращаются оба имени свойства.

В отличие от проверки, предоставляемой API Fluent, этот результат проверки будет распознан представлением, а обработчик исключений, который я использовал ранее для добавления ошибки в `ModelState` , не нужен. Так как я задали оба имени свойств `ValidationResult`в, MVC хтмлхелперс отображает сообщение об ошибке для обоих этих свойств.

![рис. 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a>DbContext. Валидатинтити

`DbContext`имеет вызванный `ValidateEntity`метод Overridable. При вызове `SaveChanges`метода Entity Framework будет вызывать этот метод для каждой сущности в кэше, состояние которого не `Unchanged`равно. Логику проверки можно прямо здесь или даже использовать этот метод для вызова, `Blog.Validate` например, метода, добавленного в предыдущем разделе.

Ниже приведен пример `ValidateEntity` переопределения, которое проверяет новые `Post`объекты, чтобы убедиться, что заголовок записи еще не используется. Сначала он проверяет, является ли сущность записью, и добавляет ее состояние. Если это так, то он просматривает базу данных, чтобы проверить, существует ли запись с тем же названием. Если уже есть существующая публикация, создается новый `DbEntityValidationResult` .

`DbEntityValidationResult`применяет `ICollection<DbValidationErrors>` и для одной сущности. `DbEntityEntry` В начале этого метода создается экземпляр класса, `DbEntityValidationResult` а затем все обнаруженные ошибки добавляются `ValidationErrors` в коллекцию.

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

## <a name="explicitly-triggering-validation"></a>Явная активация проверки

Вызов метода `SaveChanges` запускает все проверки, описанные в этой статье. Но вам не нужно полагаться `SaveChanges`на. Вы можете предпочесть проверку в любом расположении приложения.

`DbContext.GetValidationErrors`запустит все проверки, заданные заметками или API-интерфейсом Fluent, проверку, созданную `IValidatableObject` в (например, `Blog.Validate`), и `DbContext.ValidateEntity` проверок, выполненных в методе.

Следующий код вызовет `GetValidationErrors` в текущем экземпляре `DbContext`. `ValidationErrors`группируются по типу сущности в `DbEntityValidationResult`. Код сначала проходит через s, возвращенный методом, а затем через каждый `DbEntityValidationResult` `DbValidationError` объект.

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

## <a name="other-considerations-when-using-validation"></a>Другие соображения при использовании проверки

Ниже приведены некоторые другие моменты, которые следует учитывать при использовании проверки Entity Framework.

- Отложенная загрузка отключена во время проверки
- EF будет проверять заметки данных на несопоставленных свойствах (свойства, которые не сопоставлены со столбцом в базе данных)
- Проверка выполняется после обнаружения изменений во время `SaveChanges`. При внесении изменений во время проверки вы отвечаете за уведомление средства записи изменений.
- `DbUnexpectedValidationException`возникает, если во время проверки возникают ошибки
- Аспекты, которые Entity Framework включают в модель (максимальная длина, обязательный и т. д.), будут вызывать проверку, даже если в классах нет заметок к данным и/или если вы использовали конструктор EF для создания модели.
- Правила приоритета:
  - Вызовы API Fluent переопределяют соответствующие заметки к данным
- Порядок выполнения:
  - Проверка свойства выполняется перед проверкой типа
  - Проверка типа выполняется только в случае, если проверка свойства прошла
- Если свойство является сложным, его проверка также будет включать:
  - Проверка на уровне свойств для свойств сложного типа
  - Проверка на уровне типа для сложного типа, `IValidatableObject` включая проверку сложного типа

## <a name="summary"></a>Сводка

API проверки в Entity Framework очень хорошо играет с проверкой на стороне клиента в MVC, но не нужно полагаться на проверку на стороне клиента. Entity Framework позаботится о проверке на стороне сервера для заметок к данным или конфигураций, примененных с помощью API Code First Fluent.

Вы также узнали о ряде точек расширения для настройки поведения при использовании `IValidatableObject` интерфейса или касании `DbContext.ValidateEntity` метода. И эти два последних способа проверки доступны в `DbContext`, независимо от того, используется ли Code First, Model First или Database First рабочего процесса для описания концептуальной модели.
