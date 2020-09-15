---
title: Запросы и поиск сущностей — EF6
description: Запрашивание и поиск сущностей в Entity Framework 6
author: divega
ms.date: 10/23/2016
uid: ef6/querying/index
ms.openlocfilehash: d55c88280bcf164457da89ec58c180e9aa1ad12d
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90072997"
---
# <a name="querying-and-finding-entities"></a><span data-ttu-id="11483-103">Запросы и поиск сущностей</span><span class="sxs-lookup"><span data-stu-id="11483-103">Querying and Finding Entities</span></span>
<span data-ttu-id="11483-104">В этом разделе рассматриваются различные способы запроса данных с помощью Entity Framework, включая LINQ и метод Find.</span><span class="sxs-lookup"><span data-stu-id="11483-104">This topic covers the various ways you can query for data using Entity Framework, including LINQ and the Find method.</span></span> <span data-ttu-id="11483-105">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="11483-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="finding-entities-using-a-query"></a><span data-ttu-id="11483-106">Поиск сущностей с помощью запроса</span><span class="sxs-lookup"><span data-stu-id="11483-106">Finding entities using a query</span></span>  

<span data-ttu-id="11483-107">DbSet и IDbSet реализуют IQueryable, поэтому их можно использовать как начальную точку для написания запроса LINQ к базе данных.</span><span class="sxs-lookup"><span data-stu-id="11483-107">DbSet and IDbSet implement IQueryable and so can be used as the starting point for writing a LINQ query against the database.</span></span> <span data-ttu-id="11483-108">Здесь мы не будем подробно рассматривать LINQ, но приведем несколько простых примеров:</span><span class="sxs-lookup"><span data-stu-id="11483-108">This is not the appropriate place for an in-depth discussion of LINQ, but here are a couple of simple examples:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs with names starting with B
    var blogs = from b in context.Blogs
                   where b.Name.StartsWith("B")
                   select b;

    // Query for the Blog named ADO.NET Blog
    var blog = context.Blogs
                    .Where(b => b.Name == "ADO.NET Blog")
                    .FirstOrDefault();
}
```  

<span data-ttu-id="11483-109">Обратите внимание, что DbSet и IDbSet всегда создают запросы к базе данных и всегда используют круговой путь к базе данных, даже если возвращаемые сущности уже присутствуют в контексте.</span><span class="sxs-lookup"><span data-stu-id="11483-109">Note that DbSet and IDbSet always create queries against the database and will always involve a round trip to the database even if the entities returned already exist in the context.</span></span> <span data-ttu-id="11483-110">Запрос выполняется в базе данных, если:</span><span class="sxs-lookup"><span data-stu-id="11483-110">A query is executed against the database when:</span></span>  

- <span data-ttu-id="11483-111">Он перечисляется с помощью инструкции **foreach** (C#) или **For Each** (Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="11483-111">It is enumerated by a **foreach** (C#) or **For Each** (Visual Basic) statement.</span></span>  
- <span data-ttu-id="11483-112">Он перечисляется операцией коллекции, например [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary) или [ToList](https://msdn.microsoft.com/library/bb342261).</span><span class="sxs-lookup"><span data-stu-id="11483-112">It is enumerated by a collection operation such as [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary), or [ToList](https://msdn.microsoft.com/library/bb342261).</span></span>  
- <span data-ttu-id="11483-113">Во внешней части запроса заданы операторы LINQ, например [First](https://msdn.microsoft.com/library/bb291976) или [Any](https://msdn.microsoft.com/library/bb337697).</span><span class="sxs-lookup"><span data-stu-id="11483-113">LINQ operators such as [First](https://msdn.microsoft.com/library/bb291976) or [Any](https://msdn.microsoft.com/library/bb337697) are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="11483-114">Вызываются следующие методы: метод расширения [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) в DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx) и Database.ExecuteSqlCommand.</span><span class="sxs-lookup"><span data-stu-id="11483-114">The following methods are called: the [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) extension method on a DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx), and Database.ExecuteSqlCommand.</span></span>  

<span data-ttu-id="11483-115">Когда из базы данных возвращаются результаты, объекты, отсутствующие в контексте, присоединяются к контексту.</span><span class="sxs-lookup"><span data-stu-id="11483-115">When results are returned from the database, objects that do not exist in the context are attached to the context.</span></span> <span data-ttu-id="11483-116">Если объект уже есть в контексте, возвращается существующий объект (текущие и исходные значения свойств объекта в записи **не** переписываются значениями из базы данных).</span><span class="sxs-lookup"><span data-stu-id="11483-116">If an object is already in the context, the existing object is returned (the current and original values of the object's properties in the entry are **not** overwritten with database values).</span></span>  

<span data-ttu-id="11483-117">Когда вы выполняете запрос, сущности, которые были добавлены в контекст, но еще не были сохранены в базе данных, не возвращаются в составе результирующего набора.</span><span class="sxs-lookup"><span data-stu-id="11483-117">When you perform a query, entities that have been added to the context but have not yet been saved to the database are not returned as part of the result set.</span></span> <span data-ttu-id="11483-118">Чтобы получить данные из контекста, см. раздел [Локальные данные](xref:ef6/querying/local-data).</span><span class="sxs-lookup"><span data-stu-id="11483-118">To get the data that is in the context, see [Local Data](xref:ef6/querying/local-data).</span></span>  

<span data-ttu-id="11483-119">Если запрос не возвращает строки из базы данных, результатом будет пустая коллекция, а не **NULL**.</span><span class="sxs-lookup"><span data-stu-id="11483-119">If a query returns no rows from the database, the result will be an empty collection, rather than **null**.</span></span>  

## <a name="finding-entities-using-primary-keys"></a><span data-ttu-id="11483-120">Поиск сущностей с помощью первичных ключей</span><span class="sxs-lookup"><span data-stu-id="11483-120">Finding entities using primary keys</span></span>  

<span data-ttu-id="11483-121">Метод Find в классе DbSet использует значение первичного ключа, чтобы найти сущность, отслеживаемую контекстом.</span><span class="sxs-lookup"><span data-stu-id="11483-121">The Find method on DbSet uses the primary key value to attempt to find an entity tracked by the context.</span></span> <span data-ttu-id="11483-122">Если сущность не найдена в контексте, запрос отправляется в базу данных для поиска сущности там.</span><span class="sxs-lookup"><span data-stu-id="11483-122">If the entity is not found in the context then a query will be sent to the database to find the entity there.</span></span> <span data-ttu-id="11483-123">Если сущность не найдена в контексте или в базе данных, возвращается значение NULL.</span><span class="sxs-lookup"><span data-stu-id="11483-123">Null is returned if the entity is not found in the context or in the database.</span></span>  

<span data-ttu-id="11483-124">Метод Find имеет два важных отличия от запроса:</span><span class="sxs-lookup"><span data-stu-id="11483-124">Find is different from using a query in two significant ways:</span></span>  

- <span data-ttu-id="11483-125">Круговой путь к базе данных будет использоваться только в том случае, если сущность с указанным ключом не найдена в контексте.</span><span class="sxs-lookup"><span data-stu-id="11483-125">A round-trip to the database will only be made if the entity with the given key is not found in the context.</span></span>  
- <span data-ttu-id="11483-126">Метод Find возвращает сущности в состоянии "Добавлено".</span><span class="sxs-lookup"><span data-stu-id="11483-126">Find will return entities that are in the Added state.</span></span> <span data-ttu-id="11483-127">Это значит, что метод Find возвращает сущности, которые были добавлены в контекст, но еще не были сохранены в базе данных.</span><span class="sxs-lookup"><span data-stu-id="11483-127">That is, Find will return entities that have been added to the context but have not yet been saved to the database.</span></span>  
### <a name="finding-an-entity-by-primary-key"></a><span data-ttu-id="11483-128">Поиск сущности по первичному ключу</span><span class="sxs-lookup"><span data-stu-id="11483-128">Finding an entity by primary key</span></span>  

<span data-ttu-id="11483-129">В коде ниже приведено несколько примеров использования метода Find.</span><span class="sxs-lookup"><span data-stu-id="11483-129">The following code shows some uses of Find:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Will hit the database
    var blog = context.Blogs.Find(3);

    // Will return the same instance without hitting the database
    var blogAgain = context.Blogs.Find(3);

    context.Blogs.Add(new Blog { Id = -1 });

    // Will find the new blog even though it does not exist in the database
    var newBlog = context.Blogs.Find(-1);

    // Will find a User which has a string primary key
    var user = context.Users.Find("johndoe1987");
}
```  

### <a name="finding-an-entity-by-composite-primary-key"></a><span data-ttu-id="11483-130">Поиск сущности по составному первичному ключу</span><span class="sxs-lookup"><span data-stu-id="11483-130">Finding an entity by composite primary key</span></span>  

<span data-ttu-id="11483-131">Платформа Entity Framework позволяет сущностям иметь составные ключи, то есть ключи, состоящие из нескольких свойств.</span><span class="sxs-lookup"><span data-stu-id="11483-131">Entity Framework allows your entities to have composite keys - that's a key that is made up of more than one property.</span></span> <span data-ttu-id="11483-132">Например, вы можете создать сущность BlogSettings, которая представляет собой параметры пользователей для конкретного блога.</span><span class="sxs-lookup"><span data-stu-id="11483-132">For example, you could have a BlogSettings entity that represents a users settings for a particular blog.</span></span> <span data-ttu-id="11483-133">Так как пользователю необходима только одна сущность BlogSettings для каждого блога, первичный ключ для BlogSettings может состоять из комбинации идентификатора блога и имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="11483-133">Because a user would only ever have one BlogSettings for each blog you could chose to make the primary key of BlogSettings a combination of BlogId and Username.</span></span> <span data-ttu-id="11483-134">Следующий код пытается найти BlogSettings по идентификатору = 3 и имени пользователя = johndoe1987:</span><span class="sxs-lookup"><span data-stu-id="11483-134">The following code attempts to find the BlogSettings with BlogId = 3 and Username = "johndoe1987":</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

<span data-ttu-id="11483-135">Если у вас есть составные ключи, вам нужно использовать ColumnAttribute или текучий API, чтобы указать порядок свойств составного ключа.</span><span class="sxs-lookup"><span data-stu-id="11483-135">Note that when you have composite keys you need to use ColumnAttribute or the fluent API to specify an ordering for the properties of the composite key.</span></span> <span data-ttu-id="11483-136">В вызове метода Find эти значения ключа должны указываться в том же порядке.</span><span class="sxs-lookup"><span data-stu-id="11483-136">The call to Find must use this order when specifying the values that form the key.</span></span>  
