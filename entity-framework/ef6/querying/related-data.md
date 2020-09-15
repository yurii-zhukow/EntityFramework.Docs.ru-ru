---
title: Загрузка связанных сущностей — EF6
description: Загрузка связанных сущностей в Entity Framework 6
author: divega
ms.date: 10/23/2016
uid: ef6/querying/related-data
ms.openlocfilehash: d1bf04f9d9017291ef3f5ec0809095df856329cd
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90073877"
---
# <a name="loading-related-entities"></a><span data-ttu-id="c4790-103">Загрузка связанных сущностей</span><span class="sxs-lookup"><span data-stu-id="c4790-103">Loading Related Entities</span></span>

<span data-ttu-id="c4790-104">Entity Framework поддерживает три способа загрузки связанной с данными загрузки, отложенной загрузки и явной загрузки.</span><span class="sxs-lookup"><span data-stu-id="c4790-104">Entity Framework supports three ways to load related data - eager loading, lazy loading and explicit loading.</span></span> <span data-ttu-id="c4790-105">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="c4790-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>

## <a name="eagerly-loading"></a><span data-ttu-id="c4790-106">Упреждающая загрузка</span><span class="sxs-lookup"><span data-stu-id="c4790-106">Eagerly Loading</span></span>

<span data-ttu-id="c4790-107">Упреждающая загрузка — это процесс, при котором запрос для одного типа сущности также загружает связанные сущности в составе запроса.</span><span class="sxs-lookup"><span data-stu-id="c4790-107">Eager loading is the process whereby a query for one type of entity also loads related entities as part of the query.</span></span> <span data-ttu-id="c4790-108">Упреждающая загрузка достигается с помощью метода Include.</span><span class="sxs-lookup"><span data-stu-id="c4790-108">Eager loading is achieved by use of the Include method.</span></span> <span data-ttu-id="c4790-109">Например, приведенные ниже запросы будут загружать блоги и все записи, связанные с каждым блогом.</span><span class="sxs-lookup"><span data-stu-id="c4790-109">For example, the queries below will load blogs and all the posts related to each blog.</span></span>

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts.
    var blogs1 = context.Blogs
                        .Include(b => b.Posts)
                        .ToList();

    // Load one blog and its related posts.
    var blog1 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include(b => b.Posts)
                       .FirstOrDefault();

    // Load all blogs and related posts
    // using a string to specify the relationship.
    var blogs2 = context.Blogs
                        .Include("Posts")
                        .ToList();

    // Load one blog and its related posts
    // using a string to specify the relationship.
    var blog2 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include("Posts")
                       .FirstOrDefault();
}
```

> [!NOTE]
> <span data-ttu-id="c4790-110">Include — это метод расширения в пространстве имен System. Data. Entity, поэтому убедитесь, что вы используете это пространство имен.</span><span class="sxs-lookup"><span data-stu-id="c4790-110">Include is an extension method in the System.Data.Entity namespace so make sure you are using that namespace.</span></span>

### <a name="eagerly-loading-multiple-levels"></a><span data-ttu-id="c4790-111">Упреждающая загрузка нескольких уровней</span><span class="sxs-lookup"><span data-stu-id="c4790-111">Eagerly loading multiple levels</span></span>

<span data-ttu-id="c4790-112">Также можно заранее загрузить несколько уровней связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="c4790-112">It is also possible to eagerly load multiple levels of related entities.</span></span> <span data-ttu-id="c4790-113">В запросах ниже приведены примеры того, как это сделать для свойств навигации коллекции и ссылки.</span><span class="sxs-lookup"><span data-stu-id="c4790-113">The queries below show examples of how to do this for both collection and reference navigation properties.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments.
    var blogs1 = context.Blogs
                        .Include(b => b.Posts.Select(p => p.Comments))
                        .ToList();

    // Load all users, their related profiles, and related avatar.
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships.
    var blogs2 = context.Blogs
                        .Include("Posts.Comments")
                        .ToList();

    // Load all users, their related profiles, and related avatar  
    // using a string to specify the relationships.
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

> [!NOTE]
> <span data-ttu-id="c4790-114">В настоящее время невозможно отфильтровать, какие связанные сущности загружаются.</span><span class="sxs-lookup"><span data-stu-id="c4790-114">It is not currently possible to filter which related entities are loaded.</span></span> <span data-ttu-id="c4790-115">Include всегда будет переноситься во все связанные сущности.</span><span class="sxs-lookup"><span data-stu-id="c4790-115">Include will always bring in all related entities.</span></span>  

## <a name="lazy-loading"></a><span data-ttu-id="c4790-116">Отложенная загрузка</span><span class="sxs-lookup"><span data-stu-id="c4790-116">Lazy Loading</span></span>

<span data-ttu-id="c4790-117">Отложенная загрузка — это процесс, при котором сущность или коллекция сущностей автоматически загружается из базы данных при первом обращении к свойству, ссылающимся на сущность или сущности.</span><span class="sxs-lookup"><span data-stu-id="c4790-117">Lazy loading is the process whereby an entity or collection of entities is automatically loaded from the database the first time that a property referring to the entity/entities is accessed.</span></span> <span data-ttu-id="c4790-118">При использовании типов сущностей POCO отложенная загрузка достигается путем создания экземпляров производных прокси-типов и последующего переопределения виртуальных свойств для добавления обработчика загрузки.</span><span class="sxs-lookup"><span data-stu-id="c4790-118">When using POCO entity types, lazy loading is achieved by creating instances of derived proxy types and then overriding virtual properties to add the loading hook.</span></span> <span data-ttu-id="c4790-119">Например, при использовании класса сущности блога, определенного ниже, связанные записи будут загружаться при первом обращении к свойству навигации posts.</span><span class="sxs-lookup"><span data-stu-id="c4790-119">For example, when using the Blog entity class defined below, the related Posts will be loaded the first time the Posts navigation property is accessed:</span></span>

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

### <a name="turn-lazy-loading-off-for-serialization"></a><span data-ttu-id="c4790-120">Отключить отложенную загрузку для сериализации</span><span class="sxs-lookup"><span data-stu-id="c4790-120">Turn lazy loading off for serialization</span></span>

<span data-ttu-id="c4790-121">Отложенная загрузка и сериализация не сочетаются хорошо, и если вы не следите за тем, чтобы выполнять запросы для всей базы данных, просто потому, что включена отложенная загрузка.</span><span class="sxs-lookup"><span data-stu-id="c4790-121">Lazy loading and serialization don’t mix well, and if you aren’t careful you can end up querying for your entire database just because lazy loading is enabled.</span></span> <span data-ttu-id="c4790-122">Большинство сериализаторов работают путем доступа к каждому свойству в экземпляре типа.</span><span class="sxs-lookup"><span data-stu-id="c4790-122">Most serializers work by accessing each property on an instance of a type.</span></span> <span data-ttu-id="c4790-123">Доступ к свойствам активирует отложенную загрузку, поэтому сериализуются другие сущности.</span><span class="sxs-lookup"><span data-stu-id="c4790-123">Property access triggers lazy loading, so more entities get serialized.</span></span> <span data-ttu-id="c4790-124">Для доступа к этим свойствам сущностей и загружается еще несколько сущностей.</span><span class="sxs-lookup"><span data-stu-id="c4790-124">On those entities properties are accessed, and even more entities are loaded.</span></span> <span data-ttu-id="c4790-125">Перед сериализацией сущности рекомендуется отключить отложенную загрузку.</span><span class="sxs-lookup"><span data-stu-id="c4790-125">It’s a good practice to turn lazy loading off before you serialize an entity.</span></span> <span data-ttu-id="c4790-126">В следующих разделах показано, как это сделать.</span><span class="sxs-lookup"><span data-stu-id="c4790-126">The following sections show how to do this.</span></span>

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a><span data-ttu-id="c4790-127">Отключение отложенной загрузки для конкретных свойств навигации</span><span class="sxs-lookup"><span data-stu-id="c4790-127">Turning off lazy loading for specific navigation properties</span></span>

<span data-ttu-id="c4790-128">Отложенная загрузка коллекции posts может быть отключена путем создания свойства posts, не являющегося виртуальным:</span><span class="sxs-lookup"><span data-stu-id="c4790-128">Lazy loading of the Posts collection can be turned off by making the Posts property non-virtual:</span></span>

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

<span data-ttu-id="c4790-129">Загрузка коллекции posts может быть достигнута с помощью безотлагательной загрузки (см. статью « *безотлагательная загрузка* выше») или метода Load (см. раздел *явная загрузка* ниже).</span><span class="sxs-lookup"><span data-stu-id="c4790-129">Loading of the Posts collection can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>

### <a name="turn-off-lazy-loading-for-all-entities"></a><span data-ttu-id="c4790-130">Отключить отложенную загрузку для всех сущностей</span><span class="sxs-lookup"><span data-stu-id="c4790-130">Turn off lazy loading for all entities</span></span>

<span data-ttu-id="c4790-131">Отложенную загрузку можно отключить для всех сущностей в контексте, установив флаг для свойства конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c4790-131">Lazy loading can be turned off for all entities in the context by setting a flag on the Configuration property.</span></span> <span data-ttu-id="c4790-132">Пример:</span><span class="sxs-lookup"><span data-stu-id="c4790-132">For example:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```

<span data-ttu-id="c4790-133">Загрузка связанных сущностей по-прежнему может быть достигнута с помощью безотлагательной загрузки (см. статью « *безотлагательная загрузка* » выше) или метода Load (см. раздел *явная загрузка* ниже).</span><span class="sxs-lookup"><span data-stu-id="c4790-133">Loading of related entities can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>

## <a name="explicitly-loading"></a><span data-ttu-id="c4790-134">Явная загрузка</span><span class="sxs-lookup"><span data-stu-id="c4790-134">Explicitly Loading</span></span>

<span data-ttu-id="c4790-135">Даже при отключенной отложенной загрузке все равно можно загрузить связанные сущности с задержкой, но необходимо выполнить явный вызов.</span><span class="sxs-lookup"><span data-stu-id="c4790-135">Even with lazy loading disabled it is still possible to lazily load related entities, but it must be done with an explicit call.</span></span> <span data-ttu-id="c4790-136">Чтобы сделать это, используйте метод Load для записи связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="c4790-136">To do so you use the Load method on the related entity’s entry.</span></span> <span data-ttu-id="c4790-137">Пример:</span><span class="sxs-lookup"><span data-stu-id="c4790-137">For example:</span></span>

``` csharp
using (var context = new BloggingContext())
{
    var post = context.Posts.Find(2);

    // Load the blog related to a given post.
    context.Entry(post).Reference(p => p.Blog).Load();

    // Load the blog related to a given post using a string.
    context.Entry(post).Reference("Blog").Load();

    var blog = context.Blogs.Find(1);

    // Load the posts related to a given blog.
    context.Entry(blog).Collection(p => p.Posts).Load();

    // Load the posts related to a given blog
    // using a string to specify the relationship.
    context.Entry(blog).Collection("Posts").Load();
}
```

> [!NOTE]
> <span data-ttu-id="c4790-138">Метод Reference следует использовать, когда сущность имеет свойство навигации для другой отдельной сущности.</span><span class="sxs-lookup"><span data-stu-id="c4790-138">The Reference method should be used when an entity has a navigation property to another single entity.</span></span> <span data-ttu-id="c4790-139">С другой стороны, метод Collection следует использовать, когда сущность имеет свойство навигации для коллекции других сущностей.</span><span class="sxs-lookup"><span data-stu-id="c4790-139">On the other hand, the Collection method should be used when an entity has a navigation property to a collection of other entities.</span></span>

### <a name="applying-filters-when-explicitly-loading-related-entities"></a><span data-ttu-id="c4790-140">Применение фильтров при явной загрузке связанных сущностей</span><span class="sxs-lookup"><span data-stu-id="c4790-140">Applying filters when explicitly loading related entities</span></span>

<span data-ttu-id="c4790-141">Метод Query предоставляет доступ к базовому запросу, который Entity Framework будет использовать при загрузке связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="c4790-141">The Query method provides access to the underlying query that Entity Framework will use when loading related entities.</span></span> <span data-ttu-id="c4790-142">Затем можно использовать LINQ для применения фильтров к запросу перед его выполнением с вызовом метода расширения LINQ, такого как ToList, Load и т. д. Метод Query можно использовать со свойствами навигации по ссылке и коллекции, но наиболее удобен для коллекций, где его можно использовать для загрузки только части коллекции.</span><span class="sxs-lookup"><span data-stu-id="c4790-142">You can then use LINQ to apply filters to the query before executing it with a call to a LINQ extension method such as ToList, Load, etc. The Query method can be used with both reference and collection navigation properties but is most useful for collections where it can be used to load only part of the collection.</span></span> <span data-ttu-id="c4790-143">Пример:</span><span class="sxs-lookup"><span data-stu-id="c4790-143">For example:</span></span>

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog.
    context.Entry(blog)
           .Collection(b => b.Posts)
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog
    // using a string to specify the relationship.
    context.Entry(blog)
           .Collection("Posts")
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();
}
```

<span data-ttu-id="c4790-144">При использовании метода запроса обычно лучше отключить отложенную загрузку для свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="c4790-144">When using the Query method it is usually best to turn off lazy loading for the navigation property.</span></span> <span data-ttu-id="c4790-145">Это происходит потому, что в противном случае вся коллекция может загружаться автоматически механизмом отложенной загрузки либо до, либо после выполнения фильтрованного запроса.</span><span class="sxs-lookup"><span data-stu-id="c4790-145">This is because otherwise the entire collection may get loaded automatically by the lazy loading mechanism either before or after the filtered query has been executed.</span></span>

> [!NOTE]
> <span data-ttu-id="c4790-146">Несмотря на то, что связь может быть указана в виде строки, а не лямбда-выражения, возвращаемый IQueryable не является универсальным, если используется строка, поэтому метод Cast обычно необходим, прежде чем можно будет выполнить с ним какое-либо полезное.</span><span class="sxs-lookup"><span data-stu-id="c4790-146">While the relationship can be specified as a string instead of a lambda expression, the returned IQueryable is not generic when a string is used and so the Cast method is usually needed before anything useful can be done with it.</span></span>

## <a name="using-query-to-count-related-entities-without-loading-them"></a><span data-ttu-id="c4790-147">Использование запроса для подсчета связанных сущностей без их загрузки</span><span class="sxs-lookup"><span data-stu-id="c4790-147">Using Query to count related entities without loading them</span></span>

<span data-ttu-id="c4790-148">Иногда полезно иметь представление о количестве сущностей, связанных с другой сущностью в базе данных, не тратя на фактическую стоимость загрузки всех этих сущностей.</span><span class="sxs-lookup"><span data-stu-id="c4790-148">Sometimes it is useful to know how many entities are related to another entity in the database without actually incurring the cost of loading all those entities.</span></span> <span data-ttu-id="c4790-149">Для этого можно использовать метод запроса с методом Count LINQ.</span><span class="sxs-lookup"><span data-stu-id="c4790-149">The Query method with the LINQ Count method can be used to do this.</span></span> <span data-ttu-id="c4790-150">Пример:</span><span class="sxs-lookup"><span data-stu-id="c4790-150">For example:</span></span>

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Count how many posts the blog has.
    var postCount = context.Entry(blog)
                           .Collection(b => b.Posts)
                           .Query()
                           .Count();
}
```
