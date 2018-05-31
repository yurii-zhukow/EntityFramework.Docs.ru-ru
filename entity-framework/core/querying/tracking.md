---
title: Сравнение запросов с отслеживанием и без отслеживания — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
ms.technology: entity-framework-core
uid: core/querying/tracking
ms.openlocfilehash: 9a22c893f3b1e9991560e25e0252287a2844b39e
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2017
ms.locfileid: "26053964"
---
# <a name="tracking-vs-no-tracking-queries"></a><span data-ttu-id="b9192-102">Сравнение запросов с отслеживанием и без отслеживания</span><span class="sxs-lookup"><span data-stu-id="b9192-102">Tracking vs. No-Tracking Queries</span></span>

<span data-ttu-id="b9192-103">Поведение отслеживания контролирует, будет ли Entity Framework Core хранить информацию об экземпляре сущности в своем средстве отслеживания изменений.</span><span class="sxs-lookup"><span data-stu-id="b9192-103">Tracking behavior controls whether or not Entity Framework Core will keep information about an entity instance in its change tracker.</span></span> <span data-ttu-id="b9192-104">Если сущность отслеживается, любые изменения, обнаруженные в сущности, будут сохраняться в базе данных во время операции `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="b9192-104">If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`.</span></span> <span data-ttu-id="b9192-105">Entity Framework Core также будет адаптировать свойства навигации между сущностями, полученными из запроса отслеживания, и сущностями, которые ранее были загружены в экземпляр DbContext.</span><span class="sxs-lookup"><span data-stu-id="b9192-105">Entity Framework Core will also fix-up navigation properties between entities that are obtained from a tracking query and entities that were previously loaded into the DbContext instance.</span></span>

> [!TIP]  
> <span data-ttu-id="b9192-106">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="b9192-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="tracking-queries"></a><span data-ttu-id="b9192-107">Отслеживания запросов</span><span class="sxs-lookup"><span data-stu-id="b9192-107">Tracking queries</span></span>

<span data-ttu-id="b9192-108">По умолчанию запросы, возвращающие типы сущностей, отслеживаются.</span><span class="sxs-lookup"><span data-stu-id="b9192-108">By default, queries that return entity types are tracking.</span></span> <span data-ttu-id="b9192-109">Это означает, что вы можете внести изменения в эти экземпляры сущностей и сохранить эти изменения с помощью операции `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="b9192-109">This means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.</span></span>

<span data-ttu-id="b9192-110">В следующем примере изменение рейтинга блогов будет обнаружено и сохранено в базе данных во время операции `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="b9192-110">In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a><span data-ttu-id="b9192-111">Отключение отслеживания запросов</span><span class="sxs-lookup"><span data-stu-id="b9192-111">No-tracking queries</span></span>

<span data-ttu-id="b9192-112">Никакие запросы отслеживания не полезны, когда результаты используются в сценарии только для чтения.</span><span class="sxs-lookup"><span data-stu-id="b9192-112">No tracking queries are useful when the results are used in a read-only scenario.</span></span> <span data-ttu-id="b9192-113">Они быстрее выполняются, потому что нет необходимости настраивать информацию об отслеживании изменений.</span><span class="sxs-lookup"><span data-stu-id="b9192-113">They are quicker to execute because there is no need to setup change tracking information.</span></span>

<span data-ttu-id="b9192-114">Вы можете отключить отслеживание для отдельного запроса:</span><span class="sxs-lookup"><span data-stu-id="b9192-114">You can swap an individual query to be no-tracking:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

<span data-ttu-id="b9192-115">Вы также можете изменить поведение отслеживания по умолчанию на уровне экземпляра контекста:</span><span class="sxs-lookup"><span data-stu-id="b9192-115">You can also change the default tracking behavior at the context instance level:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> <span data-ttu-id="b9192-116">Запросы отслеживания не выполняют разрешение идентичности в исполняемом запросе.</span><span class="sxs-lookup"><span data-stu-id="b9192-116">No tracking queries still perform identity resolution within the excuting query.</span></span> <span data-ttu-id="b9192-117">Если результирующий набор содержит одну и ту же сущность несколько раз, тот же экземпляр класса сущности будет возвращен для каждого вхождения в результирующем наборе.</span><span class="sxs-lookup"><span data-stu-id="b9192-117">If the result set contains the same entity multiple times, the same instance of the entity class will be returned for each occurrence in the result set.</span></span> <span data-ttu-id="b9192-118">Однако для отслеживания сущностей, которые уже были возвращены, используются слабые ссылки.</span><span class="sxs-lookup"><span data-stu-id="b9192-118">However, weak references are used to keep track of entities that have already been returned.</span></span> <span data-ttu-id="b9192-119">Если предыдущий результат с тем же идентификатором выходит за пределы области, а сборка мусора выполняется, вы можете получить новый экземпляр сущности.</span><span class="sxs-lookup"><span data-stu-id="b9192-119">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span> <span data-ttu-id="b9192-120">Подробнее см. в статье о [принципе работы запросов](overview.md).</span><span class="sxs-lookup"><span data-stu-id="b9192-120">For more information, see [How Query Works](overview.md).</span></span>

## <a name="tracking-and-projections"></a><span data-ttu-id="b9192-121">Отслеживание и проекции</span><span class="sxs-lookup"><span data-stu-id="b9192-121">Tracking and projections</span></span>

<span data-ttu-id="b9192-122">Даже если тип результата запроса не является типом сущности, если результат содержит типы сущностей, они по-прежнему будут отслеживаться по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b9192-122">Even if the result type of the query isn't an entity type, if the result contains entity types they will still be tracked by default.</span></span> <span data-ttu-id="b9192-123">В следующем запросе, который возвращает анонимный тип, будут отслеживаться экземпляры `Blog` в результирующем наборе.</span><span class="sxs-lookup"><span data-stu-id="b9192-123">In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.</span></span>

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

<span data-ttu-id="b9192-124">Если результирующий набор не содержит типов сущностей, то отслеживание не выполняется.</span><span class="sxs-lookup"><span data-stu-id="b9192-124">If the result set does not contain any entity types, then no tracking is performed.</span></span> <span data-ttu-id="b9192-125">В следующем запросе, который возвращает анонимный тип с некоторыми значениями из сущности (но не экземплярами фактического типа сущности), отслеживание не выполняется.</span><span class="sxs-lookup"><span data-stu-id="b9192-125">In the following query, which returns an anonymous type with some of the values from the entity (but no instances of the actual entity type), there is no tracking performed.</span></span>

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
