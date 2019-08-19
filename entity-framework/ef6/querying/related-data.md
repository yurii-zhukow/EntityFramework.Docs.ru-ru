---
title: Загрузка связанных сущностей — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: f40034475ed6659b60ab4317605fd1d802218d69
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565322"
---
# <a name="loading-related-entities"></a>Загрузка связанных сущностей
Entity Framework поддерживает три способа загрузки связанной с данными загрузки, отложенной загрузки и явной загрузки. Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.  

## <a name="eagerly-loading"></a>Упреждающая загрузка  

Упреждающая загрузка — это процесс, при котором запрос для одного типа сущности также загружает связанные сущности в составе запроса. Упреждающая загрузка достигается с помощью метода Include. Например, приведенные ниже запросы будут загружать блоги и все записи, связанные с каждым блогом.  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts
    var blogs1 = context.Blogs
                        .Include(b => b.Posts)
                        .ToList();

    // Load one blog and its related posts
    var blog1 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include(b => b.Posts)
                       .FirstOrDefault();

    // Load all blogs and related posts  
    // using a string to specify the relationship
    var blogs2 = context.Blogs
                        .Include("Posts")
                        .ToList();

    // Load one blog and its related posts  
    // using a string to specify the relationship
    var blog2 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include("Posts")
                       .FirstOrDefault();
}
```  

Обратите внимание, что include — это метод расширения в пространстве имен System. Data. Entity, поэтому убедитесь, что вы используете это пространство имен.  

### <a name="eagerly-loading-multiple-levels"></a>Упреждающая загрузка нескольких уровней  

Также можно заранее загрузить несколько уровней связанных сущностей. В запросах ниже приведены примеры того, как это сделать для свойств навигации коллекции и ссылки.  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments
    var blogs1 = context.Blogs
                        .Include(b => b.Posts.Select(p => p.Comments))
                        .ToList();

    // Load all users, their related profiles, and related avatar
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships
    var blogs2 = context.Blogs
                        .Include("Posts.Comments")
                        .ToList();

    // Load all users, their related profiles, and related avatar  
    // using a string to specify the relationships
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

Обратите внимание, что в настоящее время невозможно отфильтровать, какие связанные сущности загружаются. Include всегда будет переноситься во все связанные сущности.  

## <a name="lazy-loading"></a>Отложенная загрузка  

Отложенная загрузка — это процесс, при котором сущность или коллекция сущностей автоматически загружается из базы данных при первом обращении к свойству, ссылающимся на сущность или сущности. При использовании типов сущностей POCO отложенная загрузка достигается путем создания экземпляров производных прокси-типов и последующего переопределения виртуальных свойств для добавления обработчика загрузки. Например, при использовании класса сущности блога, определенного ниже, связанные записи будут загружаться при первом обращении к свойству навигации posts.  

``` csharp
public class Blog
{  
    public int BlogId { get; set; }  
    public string Name { get; set; }  
    public string Url { get; set; }  
    public string Tags { get; set; }  

    public virtual ICollection<Post> Posts { get; set; }  
}
```  

### <a name="turn-lazy-loading-off-for-serialization"></a>Отключить отложенную загрузку для сериализации  

Отложенная загрузка и сериализация не сочетаются хорошо, и если вы не следите за тем, чтобы выполнять запросы для всей базы данных, просто потому, что включена отложенная загрузка. Большинство сериализаторов работают путем доступа к каждому свойству в экземпляре типа. Доступ к свойствам активирует отложенную загрузку, поэтому сериализуются другие сущности. Для доступа к этим свойствам сущностей и загружается еще несколько сущностей. Перед сериализацией сущности рекомендуется отключить отложенную загрузку. В следующих разделах показано, как это сделать.  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a>Отключение отложенной загрузки для конкретных свойств навигации  

Отложенная загрузка коллекции posts может быть отключена путем создания свойства posts, не являющегося виртуальным:  

``` csharp
public class Blog
{  
    public int BlogId { get; set; }  
    public string Name { get; set; }  
    public string Url { get; set; }  
    public string Tags { get; set; }  

    public ICollection<Post> Posts { get; set; }  
}
```  

Загрузка коллекции posts может быть достигнута с помощью безотлагательной загрузки (см. статью « *безотлагательная загрузка* выше») или метода Load (см. раздел *явная загрузка* ниже).  

### <a name="turn-off-lazy-loading-for-all-entities"></a>Отключить отложенную загрузку для всех сущностей  

Отложенную загрузку можно отключить для всех сущностей в контексте, установив флаг для свойства конфигурации. Например:  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

Загрузка связанных сущностей по-прежнему может быть достигнута с помощью безотлагательной загрузки (см. статью « *безотлагательная загрузка* » выше) или метода Load (см. раздел *явная загрузка* ниже).  

## <a name="explicitly-loading"></a>Явная загрузка  

Даже при отключенной отложенной загрузке все равно можно загрузить связанные сущности с задержкой, но необходимо выполнить явный вызов. Чтобы сделать это, используйте метод Load для записи связанной сущности. Например:  

``` csharp
using (var context = new BloggingContext())
{
    var post = context.Posts.Find(2);

    // Load the blog related to a given post
    context.Entry(post).Reference(p => p.Blog).Load();

    // Load the blog related to a given post using a string  
    context.Entry(post).Reference("Blog").Load();

    var blog = context.Blogs.Find(1);

    // Load the posts related to a given blog
    context.Entry(blog).Collection(p => p.Posts).Load();

    // Load the posts related to a given blog  
    // using a string to specify the relationship
    context.Entry(blog).Collection("Posts").Load();
}
```  

Обратите внимание, что метод Reference следует использовать, когда сущность имеет свойство навигации для другой отдельной сущности. С другой стороны, метод Collection следует использовать, когда сущность имеет свойство навигации для коллекции других сущностей.  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a>Применение фильтров при явной загрузке связанных сущностей  

Метод Query предоставляет доступ к базовому запросу, который Entity Framework будет использовать при загрузке связанных сущностей. Затем можно использовать LINQ для применения фильтров к запросу перед его выполнением с вызовом метода расширения LINQ, такого как ToList, Load и т. д. Метод Query можно использовать со свойствами навигации по ссылке и коллекции, но наиболее удобен для коллекций, где его можно использовать для загрузки только части коллекции. Например:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
           .Collection(b => b.Posts)
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog  
    // using a string to specify the relationship  
    context.Entry(blog)
           .Collection("Posts")
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();
}
```  

При использовании метода запроса обычно лучше отключить отложенную загрузку для свойства навигации. Это происходит потому, что в противном случае вся коллекция может загружаться автоматически механизмом отложенной загрузки либо до, либо после выполнения фильтрованного запроса.  

Обратите внимание, что хотя связь может быть указана как строка, а не лямбда-выражение, возвращаемый IQueryable не является универсальным, если используется строка, поэтому метод Cast обычно необходим, прежде чем можно будет выполнить с ним все полезное.  

## <a name="using-query-to-count-related-entities-without-loading-them"></a>Использование запроса для подсчета связанных сущностей без их загрузки  

Иногда полезно иметь представление о количестве сущностей, связанных с другой сущностью в базе данных, не тратя на фактическую стоимость загрузки всех этих сущностей. Для этого можно использовать метод запроса с методом Count LINQ. Например:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Count how many posts the blog has  
    var postCount = context.Entry(blog)
                           .Collection(b => b.Posts)
                           .Query()
                           .Count();
}
```  
