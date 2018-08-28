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
# <a name="data-validation"></a>Проверка данных
> [!NOTE]
> **EF4.1 и более поздних версий только** -функции, интерфейсы API, и т.д., описанных на этой странице появились в версии 4.1 платформы Entity Framework. При использовании более ранней версии, некоторые или все сведения неприменимо

Содержимое на этой странице написана по материалам, а статья задумано, Джули Лерман ([http://thedatafarm.com](http://thedatafarm.com)).

Платформа Entity Framework предоставляет выполнения разнообразных функций проверки, которые можно веб-канал через пользовательский интерфейс для проверки на стороне клиента или использоваться для проверки на стороне сервера. При использовании кода, во-первых, можно указать с помощью заметки или fluent API конфигурации проверки. Дополнительные проверки и сложнее, можно указать в коде и будет работать ли модель приехал из кода, во-первых, сначала модели или базы данных сначала.

## <a name="the-model"></a>Модель

Я продемонстрирую проверок с парой простых классов: блог и Post.

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
## <a name="data-annotations"></a>Заметки к данным

Сначала в коде используется заметок, связанных с сборке System.ComponentModel.DataAnnotations как один из способов настройки классов первого кода. Эти заметки относятся те, которые предоставляют правил, таких как обязательные, MaxLength и MinLength. Число клиентских приложениях .NET также распознает такие заметки, например, ASP.NET MVC. Вы можете достичь обе стороны и сервером проверки на стороне клиента с этими заметками. Например можно принудительно свойство Title блог обязательное свойство.

``` csharp
    [Required]
    public string Title { get; set; }
```

Без дополнительного кода или разметки изменений в приложение существующее приложение MVC будет выполнять проверки на стороне клиента, даже динамическое создание сообщения с использованием имени свойства и заметки.

![figure01](~/ef6/media/figure01.png)

В блога back-метод этого создать представления, Entity Framework используется для сохранения в новой записи в базу данных, но проверка на стороне клиента в MVC запускается прежде, чем оно достигнет этого кода.

Проверка на стороне клиента не пуленепробиваема тем не менее. Пользователи могут повлиять на возможности браузера или что еще хуже, злоумышленник может использовать некоторые изобретательности во избежание проверки пользовательского интерфейса. Но платформа Entity Framework также распознает необходимые заметки и его проверки.

Чтобы отключить функцию проверки на стороне клиента в MVC является простой способ проверить это. Это можно сделать в файле web.config приложения MVC. В разделе appSettings имеет ключ для ClientValidationEnabled. Задавать этот параметр в значение false будет препятствовать выполнение проверки пользовательского интерфейса.

``` xml
    <appSettings>
        <add key="ClientValidationEnabled"value="false"/>
        ...
    </appSettings>
```

Даже при проверке со стороны клиента отключен вы получите отклик в приложении. Сообщение об ошибке «Title это обязательное поле» отображается как. За исключением того, теперь она будет результатом проверки на стороне сервера. Платформа Entity Framework будет выполнять проверку необходимые заметки (прежде чем он даже идея сборки и команда INSERT для отправки в базу данных) и возвращает ошибку для MVC, которая будет отображаться сообщение.

## <a name="fluent-api"></a>Текучий API

Вы можете использовать код проверки стороне текучий API вместо заметок для получения того же клиента на стороне & сервер первого элемента. Вместо использования требуется, я покажу это с помощью проверки MaxLength.

Fluent API конфигурации применяются в том случае, как код сначала создает модель от классов. Вы можете внедрить конфигураций путем переопределения метода OnModelCreating класса DbContext. Здесь — это конфигурация, указывающего, что свойство BloggerName может быть не более 10 символов.

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

Исключение ошибки проверки в зависимости от настроек Fluent API будет не автоматически охват пользовательского интерфейса, но можно записать его в код, а затем отреагировать на него соответствующим образом.

Вот некоторые код обработки исключений ошибок в классе BlogController приложения, который фиксирует эту ошибку проверки, при попытке сохранить блог с BloggerName, превышает максимальный предел 10-значный Entity Framework.

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

Проверка автоматически не был передан обратно в представление, поэтому используется дополнительный код, который использует ModelState.AddModelError. Это гарантирует, что сведения об ошибке сделать его в представление, в котором будет использовать ValidationMessageFor Htmlhelper для отображения ошибки.

``` csharp
    @Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a>IValidatableObject

IValidatableObject — это интерфейс, живет в System.ComponentModel.DataAnnotations. Хотя он не является частью Entity Framework API, вы можете по-прежнему использовать его для проверки на стороне сервера в классах Entity Framework. IValidatableObject предоставляет метод Validate, Entity Framework будет вызывать во время SaveChanges. также можно вызвать самостоятельно любое время, вы хотите проверить классы.

Конфигурации, такие как необходимые и MaxLength выполняют инициировать по одному полю. В метод Validate может иметь более сложную логику, например, сравнивая два поля.

В следующем примере класс блог была расширена для реализации IValidatableObject, а затем укажите правило, которое не может совпадать с заголовком и BloggerName.

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

Конструктор ValidationResult принимает строку, которая представляет сообщение об ошибке, а также массив строк, представляющих имена элементов, связанных с проверкой. Так как эта проверка проверяет заголовок и BloggerName, возвращаются как имена свойств.

В отличие от проверки, предоставляемый Fluent API результат этой проверки будет распознаваться модулем представление и обработчик исключений, который я использовал выше, чтобы добавить ошибку в ModelState не требуется. Поскольку я обоих имен свойств в ValidationResult, MVC HtmlHelpers отображает сообщение об ошибке для оба этих свойства.

![figure02](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a>DbContext.ValidateEntity

DbContext имеет переопределяемый метод ValidateEntity. При вызове метода SaveChanges, Entity Framework будет вызывать этот метод для каждой сущности в своем кэше, состояние которых отлично без изменений. Логика проверки можно поместить непосредственно в здесь или даже использовать этот метод для вызова, например, метода Blog.Validate, добавленного в предыдущем разделе.

Ниже приведен пример переопределения ValidateEntity, которое проверяет новых записей, чтобы убедиться, что заголовок post не уже используется. Он сначала проверяет, является ли сущность post, и что добавляется свое состояние. Если это так, он выполняет в базе данных, чтобы увидеть, если уже есть запись с тем же именем. Если уже имеется сообщение, будет создан новый DbEntityValidationResult.

DbEntityValidationResult содержит dbentityentry, который должен и DbValidationErrors из коллекции ICollection для одной сущности. В начале этого метода DbEntityValidationResult создается и затем добавляются все ошибки, обнаруженные в коллекции ValidationErrors.

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

## <a name="explicitly-triggering-validation"></a>Явного запуска проверки

Вызов SaveChanges запускает все проверки, описанные в этой статье. Но не следует полагаться на SaveChanges. Можно проверить в другом месте в приложении.

DbContext.GetValidationErrors запустит все проверки, определенные заметки или Fluent API, проверки, созданные в IValidatableObject (например, Blog.Validate) и либо в DbContext.ValidateEntity проверки метод.

Следующий код вызовет GetValidationErrors для текущего экземпляра DbContext. ValidationErrors группируются по типу сущности в DbValidationRestuls. Код сначала проходит через DbValidationResults, возвращаемый методом, а затем проходят через каждый ValidationError внутри.

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

## <a name="other-considerations-when-using-validation"></a>Дополнительные рекомендации при использовании проверки

Ниже приведены несколько аспекты, которые следует учитывать при использовании Entity Framework проверки.

-   Отложенная загрузка отключена во время проверки.
-   EF проверит заметок к данным на несопоставленные свойства, которые не сопоставлены со столбцом в базе данных.
-   Проверка выполняется после обнаружения изменений во время SaveChanges. При внесении изменений во время проверки отвечает для уведомления объекта отслеживания изменений.
-   DbUnexpectedValidationException возникает в том случае, если возникают ошибки во время проверки.
-   Аспекты, которые Entity Framework включаются в модель (максимальной длины, т. д.) вызовет проверку, даже в том случае, если нет заметок к данным в классах и (или) конструктор EF вы использовали для создания модели.
-   Правила приоритета:
    -   Вызовы Fluent API переопределить соответствующие заметки к данным
-   Порядок выполнения:
    -   Проверка свойства предшествует проверки типа
    -   Проверка типа происходят, если свойство проверка прошла успешно
-   Если свойство является сложным проверки также будет включать:
    -   Проверка уровня свойств в свойства сложного типа
    -   Тип уровня проверки в сложном типе, включая проверку IValidatableObject сложного типа

## <a name="summary"></a>Сводка

Проверка API на платформе Entity Framework играет очень хорошо с проверки на стороне клиента в MVC, но не нужно полагаться на проверку на стороне клиента. Платформа Entity Framework позаботится проверки на стороне сервера для DataAnnotations или конфигураций, которые вы применили с Fluent API для code first.

Вы также узнали ряд точек расширения для настройки поведения с помощью интерфейса IValidatableObject или коснитесь в метод DbContet.ValidateEntity. И эти последние два средства для проверки доступны через DbContext, как при использовании Code First, Model First или Database First рабочего процесса для описания концептуальной модели.
