---
title: "Отслеживание vs. Нет отслеживания запросов - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
ms.technology: entity-framework-core
uid: core/querying/tracking
ms.openlocfilehash: 9a22c893f3b1e9991560e25e0252287a2844b39e
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2017
---
# <a name="tracking-vs-no-tracking-queries"></a><span data-ttu-id="01924-102">Отслеживание vs. Нет отслеживания запросов</span><span class="sxs-lookup"><span data-stu-id="01924-102">Tracking vs. No-Tracking Queries</span></span>

<span data-ttu-id="01924-103">Отслеживание поведение элементов управления, независимо от того, имеется ли Entity Framework Core будет хранить сведения об экземпляре сущности в его отслеживания изменений.</span><span class="sxs-lookup"><span data-stu-id="01924-103">Tracking behavior controls whether or not Entity Framework Core will keep information about an entity instance in its change tracker.</span></span> <span data-ttu-id="01924-104">Если отслеживаются сущности, любые изменения, которые обнаружены в сущности будут сохраняться в базе данных во время `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="01924-104">If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`.</span></span> <span data-ttu-id="01924-105">Entity Framework Core будет свойства навигации также исправление вверх между сущностями, полученные в результате запрос отслеживания и сущностей, которые были ранее загружены в экземпляр класса DbContext.</span><span class="sxs-lookup"><span data-stu-id="01924-105">Entity Framework Core will also fix-up navigation properties between entities that are obtained from a tracking query and entities that were previously loaded into the DbContext instance.</span></span>

> [!TIP]  
> <span data-ttu-id="01924-106">Можно просмотреть в этой статье [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) на GitHub.</span><span class="sxs-lookup"><span data-stu-id="01924-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="tracking-queries"></a><span data-ttu-id="01924-107">Запросы отслеживания</span><span class="sxs-lookup"><span data-stu-id="01924-107">Tracking queries</span></span>

<span data-ttu-id="01924-108">По умолчанию выполняется трассировка запросов, возвращающих типы сущностей.</span><span class="sxs-lookup"><span data-stu-id="01924-108">By default, queries that return entity types are tracking.</span></span> <span data-ttu-id="01924-109">Это означает, можно внести изменения для этих экземпляров сущности и имеют эти изменения сохраняются `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="01924-109">This means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.</span></span>

<span data-ttu-id="01924-110">В следующем примере изменений с оценкой блоги будет определяться и сохраняются в базе данных во время `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="01924-110">In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a><span data-ttu-id="01924-111">Нет отслеживания запросов</span><span class="sxs-lookup"><span data-stu-id="01924-111">No-tracking queries</span></span>

<span data-ttu-id="01924-112">Отсутствуют запросы отслеживания полезны в тех случаях, когда результаты передаются в сценарий только для чтения.</span><span class="sxs-lookup"><span data-stu-id="01924-112">No tracking queries are useful when the results are used in a read-only scenario.</span></span> <span data-ttu-id="01924-113">Они выполняются быстрее, поскольку нет необходимости в сведения об отслеживании изменений для установки.</span><span class="sxs-lookup"><span data-stu-id="01924-113">They are quicker to execute because there is no need to setup change tracking information.</span></span>

<span data-ttu-id="01924-114">Можно менять отдельного запроса, чтобы быть нет отслеживания:</span><span class="sxs-lookup"><span data-stu-id="01924-114">You can swap an individual query to be no-tracking:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

<span data-ttu-id="01924-115">Можно также изменить поведение на уровне экземпляра контекста отслеживания по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="01924-115">You can also change the default tracking behavior at the context instance level:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> <span data-ttu-id="01924-116">Нет отслеживания запросов по-прежнему выполнения разрешения удостоверения в уточнить запрос.</span><span class="sxs-lookup"><span data-stu-id="01924-116">No tracking queries still perform identity resolution within the excuting query.</span></span> <span data-ttu-id="01924-117">Если результирующий набор содержит ту же сущность несколько раз, для каждого вхождения в результирующем наборе возвращается экземпляр этого класса сущностей.</span><span class="sxs-lookup"><span data-stu-id="01924-117">If the result set contains the same entity multiple times, the same instance of the entity class will be returned for each occurrence in the result set.</span></span> <span data-ttu-id="01924-118">Тем не менее слабые ссылки используются для отслеживания объектов, которые уже были возвращены.</span><span class="sxs-lookup"><span data-stu-id="01924-118">However, weak references are used to keep track of entities that have already been returned.</span></span> <span data-ttu-id="01924-119">Если предыдущий результат с тем же удостоверением выходит за пределы области, а сборка мусора выполняется, может получить новый экземпляр сущности.</span><span class="sxs-lookup"><span data-stu-id="01924-119">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span> <span data-ttu-id="01924-120">Дополнительные сведения см. в разделе [принцип работы запросов](overview.md).</span><span class="sxs-lookup"><span data-stu-id="01924-120">For more information, see [How Query Works](overview.md).</span></span>

## <a name="tracking-and-projections"></a><span data-ttu-id="01924-121">Отслеживание и проекции</span><span class="sxs-lookup"><span data-stu-id="01924-121">Tracking and projections</span></span>

<span data-ttu-id="01924-122">Даже если тип результата запроса не тип сущности, если результат содержит типы сущностей будет по-прежнему отслеживаются по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="01924-122">Even if the result type of the query isn't an entity type, if the result contains entity types they will still be tracked by default.</span></span> <span data-ttu-id="01924-123">В следующем запросе, который возвращает анонимный тип, экземпляры `Blog` в результирующем наборе будут отслеживаться.</span><span class="sxs-lookup"><span data-stu-id="01924-123">In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=7)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Blog = b,
                Posts = b.Posts.Count()
            });
}
```

<span data-ttu-id="01924-124">Если результирующий набор не содержит все типы сущностей, отслеживание производиться не будет выполнен.</span><span class="sxs-lookup"><span data-stu-id="01924-124">If the result set does not contain any entity types, then no tracking is performed.</span></span> <span data-ttu-id="01924-125">В следующем запросе, который возвращает анонимный тип с различными значениями из сущности (но не экземпляров фактический тип сущности) нет отслеживания не выполнено.</span><span class="sxs-lookup"><span data-stu-id="01924-125">In the following query, which returns an anonymous type with some of the values from the entity (but no instances of the actual entity type), there is no tracking performed.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Id = b.BlogId,
                Url = b.Url
            });
}
```
