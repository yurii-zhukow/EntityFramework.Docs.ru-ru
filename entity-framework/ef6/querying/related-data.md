---
title: Загрузка связанных сущностей - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
caps.latest.revision: 3
ms.openlocfilehash: e7adc9aea11a7a8e9b87b4f9e9120aa7316588db
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122318"
---
# <a name="loading-related-entities"></a>Загрузка связанных сущностей
Платформа Entity Framework поддерживает три способа загрузки связанных данных — Безотложная загрузка, отложенная загрузка и явная загрузка. Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.  

## <a name="eagerly-loading"></a>Загрузка заранее  

Безотложная загрузка — это процесс, при котором запрос для одного типа сущности также загружает связанные сущности как часть запроса. Безотложная загрузка достигается за счет применения метода Include. Например приведенных ниже запросах загрузит блогов и записей, связанных с каждого блога.  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts
    var blogs1 = context.Blogs
                          .Include(b => b.Posts)
                          .ToList();

    // Load one blogs and its related posts
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

Обратите внимание, что включить метод расширения в пространстве имен System.Data.Entity поэтому убедитесь, что при использовании этого пространства имен.  

### <a name="eagerly-loading-multiple-levels"></a>Заранее загрузки несколько уровней  

Можно также заранее загрузить несколько уровней связанных сущностей. Приведенных ниже запросах приведены примеры того, как сделать это для коллекции и свойства навигации по ссылке.  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments
    var blogs1 = context.Blogs
                       .Include(b => b.Posts.Select(p => p.Comments))
                       .ToList();

    // Load all users their related profiles, and related avatar
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships
    var blogs2 = context.Blogs
                       .Include("Posts.Comments")
                       .ToList();

    // Load all users their related profiles, and related avatar  
    // using a string to specify the relationships
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

Обратите внимание на то, что он не является в настоящее время можно фильтровать, какие связанные сущности загружаются. Будет всегда использовать в все связанные сущности.  

## <a name="lazy-loading"></a>Отложенная загрузка  

Отложенная загрузка — это процесс, при котором сущность или коллекцию сущностей автоматически загружается из базы данных при первом обращении к свойству, ссылки на сущности или сущностей. При использовании типов сущностей POCO, отложенная загрузка достигается путем создания экземпляров типов производного прокси-сервера и затем переопределение виртуальных свойств для добавления обработчика загрузки. Например при использовании класса сущности блога, определенная ниже, связанных сообщений будут загружены при первом обращении к свойству навигации сообщения:  

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

### <a name="turn-lazy-loading-off-for-serialization"></a>Включить отложенная загрузка для сериализации  

Отложенная загрузка и сериализация не следует смешивать хорошо, и если не соблюдать осторожность можно в итоге запросов для всей базы данных, только потому, что отложенная загрузка включена. Большинство сериализаторов работать с помощью каждого свойства в экземпляре типа. Доступ к свойству активирует отложенной загрузки, поэтому сериализуются больше сущностей. В этих сущностях доступа к свойствам и даже дополнительные сущности загружаются. Рекомендуется включить отложенной загрузки из системы, прежде чем сериализации сущности. Ниже показано, как это сделать.  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a>Отключение отложенная загрузка для свойств конкретного навигации  

Можно отключить отложенную загрузку набор Posts, сделав невиртуальный свойства сообщения:  

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

Загрузка в записях коллекции можно по-прежнему добиться, используя безотложную загрузку (см. в разделе *заранее Загрузка* выше) или метода Load (см. в разделе *явная загрузка* ниже).  

### <a name="turn-off-lazy-loading-for-all-entities"></a>Отключить отложенную загрузку для всех сущностей  

Отложенная загрузка можно отключить для всех сущностей в контексте, установив флаг в свойстве конфигурации. Пример:  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

Загрузкой связанных сущностей по-прежнему может осуществляться с помощью Безотложная загрузка (см. в разделе *заранее Загрузка* выше) или метода Load (см. в разделе *явная загрузка* ниже).  

## <a name="explicitly-loading"></a>Явная загрузка  

Даже с отложенная загрузка отключена возможность медленно загрузить связанные сущности, но это должно быть сделано с помощью явного вызова. Для этого использовать метод Load в связанной сущности операции. Пример:  

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

Обратите внимание на то, что метода ссылки следует использовать, если сущность имеет свойство навигации, чтобы другой одной сущности. С другой стороны метод сбора следует использовать, если сущность имеет свойство навигации, чтобы коллекция других сущностей.  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a>Применение фильтров, если явная загрузка связанных сущностей  

Метод запроса предоставляет доступ к базовый запрос, который будет использовать Entity Framework, при загрузке связанных сущностей. Затем можно использовать LINQ для фильтрации запроса перед его выполнением с помощью вызова в метод расширения LINQ, вроде ToList, нагрузки и т. д. Метод запроса может использоваться со свойствами навигации как ссылки, так и коллекцию, но чрезвычайно полезна для коллекций, где его можно использовать для загрузки только частью коллекции. Пример:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog  
    // using a string to specify the relationship  
    context.Entry(blog)
        .Collection("Posts")
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  

При использовании метода Query обычно лучше отключить отложенную загрузку для свойства навигации. Это обусловлено тем, в противном случае вся коллекция может получить автоматически загружать механизм отложенной загрузки до или после выполнения отфильтрованного запроса.  

Обратите внимание, что хотя связи должно лежать в виде строки вместо лямбда-выражение, возвращаемое IQueryable не универсального когда строка используется в том случае, и поэтому метод приведения обычно требуется, чтобы сделать ничего полезного с ним.  

## <a name="using-query-to-count-related-entities-without-loading-them"></a>С помощью запроса для подсчета без их загрузки связанных сущностей  

Иногда бывает полезно знать, сколько сущности связаны с другой сущностью в базе данных без фактически затрат на загрузку этих сущностей. Метод запроса с помощью метода LINQ счетчик может использоваться для этого. Пример:  

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
