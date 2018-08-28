---
title: Загрузка связанных сущностей - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: 091493f60c732573ca63adb8c65bc28606453d34
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995929"
---
# <a name="loading-related-entities"></a><span data-ttu-id="c33af-102">Загрузка связанных сущностей</span><span class="sxs-lookup"><span data-stu-id="c33af-102">Loading Related Entities</span></span>
<span data-ttu-id="c33af-103">Платформа Entity Framework поддерживает три способа загрузки связанных данных — Безотложная загрузка, отложенная загрузка и явная загрузка.</span><span class="sxs-lookup"><span data-stu-id="c33af-103">Entity Framework supports three ways to load related data - eager loading, lazy loading and explicit loading.</span></span> <span data-ttu-id="c33af-104">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="c33af-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="eagerly-loading"></a><span data-ttu-id="c33af-105">Загрузка заранее</span><span class="sxs-lookup"><span data-stu-id="c33af-105">Eagerly Loading</span></span>  

<span data-ttu-id="c33af-106">Безотложная загрузка — это процесс, при котором запрос для одного типа сущности также загружает связанные сущности как часть запроса.</span><span class="sxs-lookup"><span data-stu-id="c33af-106">Eager loading is the process whereby a query for one type of entity also loads related entities as part of the query.</span></span> <span data-ttu-id="c33af-107">Безотложная загрузка достигается за счет применения метода Include.</span><span class="sxs-lookup"><span data-stu-id="c33af-107">Eager loading is achieved by use of the Include method.</span></span> <span data-ttu-id="c33af-108">Например приведенных ниже запросах загрузит блогов и записей, связанных с каждого блога.</span><span class="sxs-lookup"><span data-stu-id="c33af-108">For example, the queries below will load blogs and all the posts related to each blog.</span></span>  

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

<span data-ttu-id="c33af-109">Обратите внимание, что включить метод расширения в пространстве имен System.Data.Entity поэтому убедитесь, что при использовании этого пространства имен.</span><span class="sxs-lookup"><span data-stu-id="c33af-109">Note that Include is an extension method in the System.Data.Entity namespace so make sure you are using that namespace.</span></span>  

### <a name="eagerly-loading-multiple-levels"></a><span data-ttu-id="c33af-110">Заранее загрузки несколько уровней</span><span class="sxs-lookup"><span data-stu-id="c33af-110">Eagerly loading multiple levels</span></span>  

<span data-ttu-id="c33af-111">Можно также заранее загрузить несколько уровней связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="c33af-111">It is also possible to eagerly load multiple levels of related entities.</span></span> <span data-ttu-id="c33af-112">Приведенных ниже запросах приведены примеры того, как сделать это для коллекции и свойства навигации по ссылке.</span><span class="sxs-lookup"><span data-stu-id="c33af-112">The queries below show examples of how to do this for both collection and reference navigation properties.</span></span>  

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

<span data-ttu-id="c33af-113">Обратите внимание на то, что он не является в настоящее время можно фильтровать, какие связанные сущности загружаются.</span><span class="sxs-lookup"><span data-stu-id="c33af-113">Note that it is not currently possible to filter which related entities are loaded.</span></span> <span data-ttu-id="c33af-114">Будет всегда использовать в все связанные сущности.</span><span class="sxs-lookup"><span data-stu-id="c33af-114">Include will always bring in all related entities.</span></span>  

## <a name="lazy-loading"></a><span data-ttu-id="c33af-115">Отложенная загрузка</span><span class="sxs-lookup"><span data-stu-id="c33af-115">Lazy Loading</span></span>  

<span data-ttu-id="c33af-116">Отложенная загрузка — это процесс, при котором сущность или коллекцию сущностей автоматически загружается из базы данных при первом обращении к свойству, ссылки на сущности или сущностей.</span><span class="sxs-lookup"><span data-stu-id="c33af-116">Lazy loading is the process whereby an entity or collection of entities is automatically loaded from the database the first time that a property referring to the entity/entities is accessed.</span></span> <span data-ttu-id="c33af-117">При использовании типов сущностей POCO, отложенная загрузка достигается путем создания экземпляров типов производного прокси-сервера и затем переопределение виртуальных свойств для добавления обработчика загрузки.</span><span class="sxs-lookup"><span data-stu-id="c33af-117">When using POCO entity types, lazy loading is achieved by creating instances of derived proxy types and then overriding virtual properties to add the loading hook.</span></span> <span data-ttu-id="c33af-118">Например при использовании класса сущности блога, определенная ниже, связанных сообщений будут загружены при первом обращении к свойству навигации сообщения:</span><span class="sxs-lookup"><span data-stu-id="c33af-118">For example, when using the Blog entity class defined below, the related Posts will be loaded the first time the Posts navigation property is accessed:</span></span>  

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

### <a name="turn-lazy-loading-off-for-serialization"></a><span data-ttu-id="c33af-119">Включить отложенная загрузка для сериализации</span><span class="sxs-lookup"><span data-stu-id="c33af-119">Turn lazy loading off for serialization</span></span>  

<span data-ttu-id="c33af-120">Отложенная загрузка и сериализация не следует смешивать хорошо, и если не соблюдать осторожность можно в итоге запросов для всей базы данных, только потому, что отложенная загрузка включена.</span><span class="sxs-lookup"><span data-stu-id="c33af-120">Lazy loading and serialization don’t mix well, and if you aren’t careful you can end up querying for your entire database just because lazy loading is enabled.</span></span> <span data-ttu-id="c33af-121">Большинство сериализаторов работать с помощью каждого свойства в экземпляре типа.</span><span class="sxs-lookup"><span data-stu-id="c33af-121">Most serializers work by accessing each property on an instance of a type.</span></span> <span data-ttu-id="c33af-122">Доступ к свойству активирует отложенной загрузки, поэтому сериализуются больше сущностей.</span><span class="sxs-lookup"><span data-stu-id="c33af-122">Property access triggers lazy loading, so more entities get serialized.</span></span> <span data-ttu-id="c33af-123">В этих сущностях доступа к свойствам и даже дополнительные сущности загружаются.</span><span class="sxs-lookup"><span data-stu-id="c33af-123">On those entities properties are accessed, and even more entities are loaded.</span></span> <span data-ttu-id="c33af-124">Рекомендуется включить отложенной загрузки из системы, прежде чем сериализации сущности.</span><span class="sxs-lookup"><span data-stu-id="c33af-124">It’s a good practice to turn lazy loading off before you serialize an entity.</span></span> <span data-ttu-id="c33af-125">Ниже показано, как это сделать.</span><span class="sxs-lookup"><span data-stu-id="c33af-125">The following sections show how to do this.</span></span>  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a><span data-ttu-id="c33af-126">Отключение отложенная загрузка для свойств конкретного навигации</span><span class="sxs-lookup"><span data-stu-id="c33af-126">Turning off lazy loading for specific navigation properties</span></span>  

<span data-ttu-id="c33af-127">Можно отключить отложенную загрузку набор Posts, сделав невиртуальный свойства сообщения:</span><span class="sxs-lookup"><span data-stu-id="c33af-127">Lazy loading of the Posts collection can be turned off by making the Posts property non-virtual:</span></span>  

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

<span data-ttu-id="c33af-128">Загрузка в записях коллекции можно по-прежнему добиться, используя безотложную загрузку (см. в разделе *заранее Загрузка* выше) или метода Load (см. в разделе *явная загрузка* ниже).</span><span class="sxs-lookup"><span data-stu-id="c33af-128">Loading of the Posts collection can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

### <a name="turn-off-lazy-loading-for-all-entities"></a><span data-ttu-id="c33af-129">Отключить отложенную загрузку для всех сущностей</span><span class="sxs-lookup"><span data-stu-id="c33af-129">Turn off lazy loading for all entities</span></span>  

<span data-ttu-id="c33af-130">Отложенная загрузка можно отключить для всех сущностей в контексте, установив флаг в свойстве конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c33af-130">Lazy loading can be turned off for all entities in the context by setting a flag on the Configuration property.</span></span> <span data-ttu-id="c33af-131">Пример:</span><span class="sxs-lookup"><span data-stu-id="c33af-131">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

<span data-ttu-id="c33af-132">Загрузкой связанных сущностей по-прежнему может осуществляться с помощью Безотложная загрузка (см. в разделе *заранее Загрузка* выше) или метода Load (см. в разделе *явная загрузка* ниже).</span><span class="sxs-lookup"><span data-stu-id="c33af-132">Loading of related entities can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

## <a name="explicitly-loading"></a><span data-ttu-id="c33af-133">Явная загрузка</span><span class="sxs-lookup"><span data-stu-id="c33af-133">Explicitly Loading</span></span>  

<span data-ttu-id="c33af-134">Даже с отложенная загрузка отключена возможность медленно загрузить связанные сущности, но это должно быть сделано с помощью явного вызова.</span><span class="sxs-lookup"><span data-stu-id="c33af-134">Even with lazy loading disabled it is still possible to lazily load related entities, but it must be done with an explicit call.</span></span> <span data-ttu-id="c33af-135">Для этого использовать метод Load в связанной сущности операции.</span><span class="sxs-lookup"><span data-stu-id="c33af-135">To do so you use the Load method on the related entity’s entry.</span></span> <span data-ttu-id="c33af-136">Пример:</span><span class="sxs-lookup"><span data-stu-id="c33af-136">For example:</span></span>  

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

<span data-ttu-id="c33af-137">Обратите внимание на то, что метода ссылки следует использовать, если сущность имеет свойство навигации, чтобы другой одной сущности.</span><span class="sxs-lookup"><span data-stu-id="c33af-137">Note that the Reference method should be used when an entity has a navigation property to another single entity.</span></span> <span data-ttu-id="c33af-138">С другой стороны метод сбора следует использовать, если сущность имеет свойство навигации, чтобы коллекция других сущностей.</span><span class="sxs-lookup"><span data-stu-id="c33af-138">On the other hand, the Collection method should be used when an entity has a navigation property to a collection of other entities.</span></span>  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a><span data-ttu-id="c33af-139">Применение фильтров, если явная загрузка связанных сущностей</span><span class="sxs-lookup"><span data-stu-id="c33af-139">Applying filters when explicitly loading related entities</span></span>  

<span data-ttu-id="c33af-140">Метод запроса предоставляет доступ к базовый запрос, который будет использовать Entity Framework, при загрузке связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="c33af-140">The Query method provides access to the underlying query that Entity Framework will use when loading related entities.</span></span> <span data-ttu-id="c33af-141">Затем можно использовать LINQ для фильтрации запроса перед его выполнением с помощью вызова в метод расширения LINQ, вроде ToList, нагрузки и т. д. Метод запроса может использоваться со свойствами навигации как ссылки, так и коллекцию, но чрезвычайно полезна для коллекций, где его можно использовать для загрузки только частью коллекции.</span><span class="sxs-lookup"><span data-stu-id="c33af-141">You can then use LINQ to apply filters to the query before executing it with a call to a LINQ extension method such as ToList, Load, etc. The Query method can be used with both reference and collection navigation properties but is most useful for collections where it can be used to load only part of the collection.</span></span> <span data-ttu-id="c33af-142">Пример:</span><span class="sxs-lookup"><span data-stu-id="c33af-142">For example:</span></span>  

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

<span data-ttu-id="c33af-143">При использовании метода Query обычно лучше отключить отложенную загрузку для свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="c33af-143">When using the Query method it is usually best to turn off lazy loading for the navigation property.</span></span> <span data-ttu-id="c33af-144">Это обусловлено тем, в противном случае вся коллекция может получить автоматически загружать механизм отложенной загрузки до или после выполнения отфильтрованного запроса.</span><span class="sxs-lookup"><span data-stu-id="c33af-144">This is because otherwise the entire collection may get loaded automatically by the lazy loading mechanism either before or after the filtered query has been executed.</span></span>  

<span data-ttu-id="c33af-145">Обратите внимание, что хотя связи должно лежать в виде строки вместо лямбда-выражение, возвращаемое IQueryable не универсального когда строка используется в том случае, и поэтому метод приведения обычно требуется, чтобы сделать ничего полезного с ним.</span><span class="sxs-lookup"><span data-stu-id="c33af-145">Note that while the relationship can be specified as a string instead of a lambda expression, the returned IQueryable is not generic when a string is used and so the Cast method is usually needed before anything useful can be done with it.</span></span>  

## <a name="using-query-to-count-related-entities-without-loading-them"></a><span data-ttu-id="c33af-146">С помощью запроса для подсчета без их загрузки связанных сущностей</span><span class="sxs-lookup"><span data-stu-id="c33af-146">Using Query to count related entities without loading them</span></span>  

<span data-ttu-id="c33af-147">Иногда бывает полезно знать, сколько сущности связаны с другой сущностью в базе данных без фактически затрат на загрузку этих сущностей.</span><span class="sxs-lookup"><span data-stu-id="c33af-147">Sometimes it is useful to know how many entities are related to another entity in the database without actually incurring the cost of loading all those entities.</span></span> <span data-ttu-id="c33af-148">Метод запроса с помощью метода LINQ счетчик может использоваться для этого.</span><span class="sxs-lookup"><span data-stu-id="c33af-148">The Query method with the LINQ Count method can be used to do this.</span></span> <span data-ttu-id="c33af-149">Пример:</span><span class="sxs-lookup"><span data-stu-id="c33af-149">For example:</span></span>  

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
