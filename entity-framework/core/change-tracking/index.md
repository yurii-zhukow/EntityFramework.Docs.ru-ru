---
title: Отслеживание изменений — EF Core
description: Общие сведения об отслеживании изменений для EF Core
author: ajcvickers
ms.date: 12/30/2020
uid: core/change-tracking/index
ms.openlocfilehash: 8cfa4590af07ec1715eb48ec0c7acb3426b6a6b4
ms.sourcegitcommit: 7700840119b1639275f3b64836e7abb59103f2e7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2021
ms.locfileid: "98983265"
---
# <a name="change-tracking-in-ef-core"></a><span data-ttu-id="7a869-103">Отслеживание изменений в EF Core</span><span class="sxs-lookup"><span data-stu-id="7a869-103">Change Tracking in EF Core</span></span>

<span data-ttu-id="7a869-104">Каждый экземпляр <xref:Microsoft.EntityFrameworkCore.DbContext> отслеживает изменения, внесенные в сущности.</span><span class="sxs-lookup"><span data-stu-id="7a869-104">Each <xref:Microsoft.EntityFrameworkCore.DbContext> instance tracks changes made to entities.</span></span> <span data-ttu-id="7a869-105">Эти отслеживаемые сущности, в свою очередь, записывают изменения в базу данных при вызове <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A>.</span><span class="sxs-lookup"><span data-stu-id="7a869-105">These tracked entities in turn drive the changes to the database when <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> is called.</span></span>

<span data-ttu-id="7a869-106">В этом документе представлены общие сведения об отслеживании изменений Entity Framework Core (EF Core) и о том, как это связано с запросами и обновлениями.</span><span class="sxs-lookup"><span data-stu-id="7a869-106">This document presents an overview of Entity Framework Core (EF Core) change tracking and how it relates to queries and updates.</span></span>

> [!TIP]
> <span data-ttu-id="7a869-107">Вы можете запустить и отладить весь код, используемый в этой документации, [скачав пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/ChangeTracking/ChangeTrackingInEFCore).</span><span class="sxs-lookup"><span data-stu-id="7a869-107">You can run and debug into all the code in this document by [downloading the sample code from GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/ChangeTracking/ChangeTrackingInEFCore).</span></span>

> [!TIP]
> <span data-ttu-id="7a869-108">Для простоты в этой документации используются и описываются синхронные методы, такие как <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A>, а не их асинхронные эквиваленты, такие как <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A>.</span><span class="sxs-lookup"><span data-stu-id="7a869-108">For simplicity, this document uses and references synchronous methods such as <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> rather their async equivalents such as <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A>.</span></span> <span data-ttu-id="7a869-109">Вызов и ожидание асинхронного метода можно заменить, если не указано иное.</span><span class="sxs-lookup"><span data-stu-id="7a869-109">Calling and awaiting the async method can be substituted unless otherwise noted.</span></span>

## <a name="how-to-track-entities"></a><span data-ttu-id="7a869-110">Как отслеживать сущности</span><span class="sxs-lookup"><span data-stu-id="7a869-110">How to track entities</span></span>

<span data-ttu-id="7a869-111">Экземпляры сущностей отслеживаются, если они:</span><span class="sxs-lookup"><span data-stu-id="7a869-111">Entity instances become tracked when they are:</span></span>

- <span data-ttu-id="7a869-112">получены из запросов к базе данных;</span><span class="sxs-lookup"><span data-stu-id="7a869-112">Returned from a query executed against the database</span></span>
- <span data-ttu-id="7a869-113">явно присоединены к DbContext с помощью `Add`, `Attach`, `Update` или аналогичных методов;</span><span class="sxs-lookup"><span data-stu-id="7a869-113">Explicitly attached to the DbContext by `Add`, `Attach`, `Update`, or similar methods</span></span>
- <span data-ttu-id="7a869-114">обнаружены как новые сущности, подключенные к существующим отслеживаемым сущностям.</span><span class="sxs-lookup"><span data-stu-id="7a869-114">Detected as new entities connected to existing tracked entities</span></span>

<span data-ttu-id="7a869-115">Экземпляры сущностей больше не отслеживаются, если:</span><span class="sxs-lookup"><span data-stu-id="7a869-115">Entity instances are no longer tracked when:</span></span>

- <span data-ttu-id="7a869-116">экземпляр DbContext удален;</span><span class="sxs-lookup"><span data-stu-id="7a869-116">The DbContext is disposed</span></span>
- <span data-ttu-id="7a869-117">средство отслеживания изменений очищено (EF Core 5.0 и более поздних версий);</span><span class="sxs-lookup"><span data-stu-id="7a869-117">The change tracker is cleared (EF Core 5.0 and later)</span></span>
- <span data-ttu-id="7a869-118">сущности явно окончательно удалены.</span><span class="sxs-lookup"><span data-stu-id="7a869-118">The entities are explicitly detached</span></span>

<span data-ttu-id="7a869-119">DbContext используется для представления кратковременной единицы работы, как описано в статье [Время существования, настройка и инициализация DbContext](xref:core/dbcontext-configuration/index).</span><span class="sxs-lookup"><span data-stu-id="7a869-119">DbContext is designed to represent a short-lived unit-of-work, as described in [DbContext Initialization and Configuration](xref:core/dbcontext-configuration/index).</span></span> <span data-ttu-id="7a869-120">Это означает, что удаление DbContext — это _обычный способ_ остановки отслеживания сущностей.</span><span class="sxs-lookup"><span data-stu-id="7a869-120">This means that disposing the DbContext is _the normal way_ to stop tracking entities.</span></span> <span data-ttu-id="7a869-121">Иными словами, время существования DbContext должно быть следующим:</span><span class="sxs-lookup"><span data-stu-id="7a869-121">In other words, the lifetime of a DbContext should be:</span></span>

1. <span data-ttu-id="7a869-122">Создание экземпляра DbContext.</span><span class="sxs-lookup"><span data-stu-id="7a869-122">Create the DbContext instance</span></span>
2. <span data-ttu-id="7a869-123">Отслеживание определенных сущностей.</span><span class="sxs-lookup"><span data-stu-id="7a869-123">Track some entities</span></span>
3. <span data-ttu-id="7a869-124">Внесение определенных изменений в сущности.</span><span class="sxs-lookup"><span data-stu-id="7a869-124">Make some changes to the entities</span></span>
4. <span data-ttu-id="7a869-125">Вызов метода SaveChanges для обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="7a869-125">Call SaveChanges to update the database</span></span>
5. <span data-ttu-id="7a869-126">Удаление экземпляра DbContext.</span><span class="sxs-lookup"><span data-stu-id="7a869-126">Dispose the DbContext instance</span></span>

> [!TIP]
> <span data-ttu-id="7a869-127">Вам не нужно очищать средство отслеживания изменений или явно окончательно удалять экземпляры сущностей при использовании этого подхода.</span><span class="sxs-lookup"><span data-stu-id="7a869-127">It is not necessary to clear the change tracker or explicitly detach entity instances when taking this approach.</span></span> <span data-ttu-id="7a869-128">Но, если необходимо окончательно удалить сущности, более эффективным будет вызов <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Clear%2A?displayProperty=nameWithType>, чем удаление сущностей по одной.</span><span class="sxs-lookup"><span data-stu-id="7a869-128">However, if you do need to detach entities, then calling <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Clear%2A?displayProperty=nameWithType> is more efficient than detaching entities one-by-one.</span></span>

## <a name="entity-states"></a><span data-ttu-id="7a869-129">Состояния сущностей</span><span class="sxs-lookup"><span data-stu-id="7a869-129">Entity states</span></span>

<span data-ttu-id="7a869-130">Каждая сущность связана с заданным состоянием <xref:Microsoft.EntityFrameworkCore.EntityState>:</span><span class="sxs-lookup"><span data-stu-id="7a869-130">Every entity is associated with a given <xref:Microsoft.EntityFrameworkCore.EntityState>:</span></span>

- <span data-ttu-id="7a869-131">Сущности `Detached` не отслеживаются <xref:Microsoft.EntityFrameworkCore.DbContext>.</span><span class="sxs-lookup"><span data-stu-id="7a869-131">`Detached` entities are not being tracked by the <xref:Microsoft.EntityFrameworkCore.DbContext>.</span></span>
- <span data-ttu-id="7a869-132">Сущности `Added` являются новыми и еще не вставлены в базу данных.</span><span class="sxs-lookup"><span data-stu-id="7a869-132">`Added` entities are new and have not yet been inserted into the database.</span></span> <span data-ttu-id="7a869-133">Это означает, что они будут вставлены при вызове <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A>.</span><span class="sxs-lookup"><span data-stu-id="7a869-133">This means they will be inserted when <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> is called.</span></span>
- <span data-ttu-id="7a869-134">Сущности `Unchanged` _не_ изменены, так как они были запрошены из базы данных.</span><span class="sxs-lookup"><span data-stu-id="7a869-134">`Unchanged` entities have _not_ been changed since they were queried from the database.</span></span> <span data-ttu-id="7a869-135">Все сущности, получаемые из запросов, изначально находятся в этом состоянии.</span><span class="sxs-lookup"><span data-stu-id="7a869-135">All entities returned from queries are initially in this state.</span></span>
- <span data-ttu-id="7a869-136">Сущности `Modified` были изменены с момента запроса из базы данных.</span><span class="sxs-lookup"><span data-stu-id="7a869-136">`Modified` entities have been changed since they were queried from the database.</span></span> <span data-ttu-id="7a869-137">Это означает, что они будут обновлены при вызове SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="7a869-137">This means they will be updated when SaveChanges is called.</span></span>
- <span data-ttu-id="7a869-138">Сущности `Deleted` существуют в базе данных, но отмечены для удаления при вызове SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="7a869-138">`Deleted` entities exist in the database, but are marked to be deleted when SaveChanges is called.</span></span>

<span data-ttu-id="7a869-139">EF Core отслеживает изменения на уровне свойств.</span><span class="sxs-lookup"><span data-stu-id="7a869-139">EF Core tracks changes at the property level.</span></span> <span data-ttu-id="7a869-140">Например, если изменяется только одно значение свойства, обновление базы данных изменит только это значение.</span><span class="sxs-lookup"><span data-stu-id="7a869-140">For example, if only a single property value is modified, then a database update will change only that value.</span></span> <span data-ttu-id="7a869-141">При этом свойства помечаются как измененные, только если сущность находится в состоянии Modified.</span><span class="sxs-lookup"><span data-stu-id="7a869-141">However, properties can only be marked as modified when the entity itself is in the Modified state.</span></span> <span data-ttu-id="7a869-142">(С другой стороны, состояние Modified означает, что хотя бы одно значение свойства помечено как измененное.)</span><span class="sxs-lookup"><span data-stu-id="7a869-142">(Or, from an alternate perspective, the Modified state means that at least one property value has been marked as modified.)</span></span>

<span data-ttu-id="7a869-143">В следующей таблице описаны различия между состояниями.</span><span class="sxs-lookup"><span data-stu-id="7a869-143">The following table summarizes the different states:</span></span>

| <span data-ttu-id="7a869-144">Состояние сущностей</span><span class="sxs-lookup"><span data-stu-id="7a869-144">Entity state</span></span>     | <span data-ttu-id="7a869-145">Отслеживается DbContext</span><span class="sxs-lookup"><span data-stu-id="7a869-145">Tracked by DbContext</span></span> | <span data-ttu-id="7a869-146">Существует в базе данных</span><span class="sxs-lookup"><span data-stu-id="7a869-146">Exists in database</span></span> | <span data-ttu-id="7a869-147">Измененные свойства</span><span class="sxs-lookup"><span data-stu-id="7a869-147">Properties modified</span></span> | <span data-ttu-id="7a869-148">Действие при вызове SaveChanges</span><span class="sxs-lookup"><span data-stu-id="7a869-148">Action on SaveChanges</span></span>
|:-----------------|----------------------|--------------------|---------------------|-----------------------
| `Detached`       | <span data-ttu-id="7a869-149">Нет</span><span class="sxs-lookup"><span data-stu-id="7a869-149">No</span></span>                   | -                  | -                   | -
| `Added`          | <span data-ttu-id="7a869-150">Да</span><span class="sxs-lookup"><span data-stu-id="7a869-150">Yes</span></span>                  | <span data-ttu-id="7a869-151">Нет</span><span class="sxs-lookup"><span data-stu-id="7a869-151">No</span></span>                 | -                   | <span data-ttu-id="7a869-152">Вставить</span><span class="sxs-lookup"><span data-stu-id="7a869-152">Insert</span></span>
| `Unchanged`      | <span data-ttu-id="7a869-153">Да</span><span class="sxs-lookup"><span data-stu-id="7a869-153">Yes</span></span>                  | <span data-ttu-id="7a869-154">Да</span><span class="sxs-lookup"><span data-stu-id="7a869-154">Yes</span></span>                | <span data-ttu-id="7a869-155">Нет</span><span class="sxs-lookup"><span data-stu-id="7a869-155">No</span></span>                  | -
| `Modified`       | <span data-ttu-id="7a869-156">Да</span><span class="sxs-lookup"><span data-stu-id="7a869-156">Yes</span></span>                  | <span data-ttu-id="7a869-157">Да</span><span class="sxs-lookup"><span data-stu-id="7a869-157">Yes</span></span>                | <span data-ttu-id="7a869-158">Да</span><span class="sxs-lookup"><span data-stu-id="7a869-158">Yes</span></span>                 | <span data-ttu-id="7a869-159">Update</span><span class="sxs-lookup"><span data-stu-id="7a869-159">Update</span></span>
| `Deleted`        | <span data-ttu-id="7a869-160">Да</span><span class="sxs-lookup"><span data-stu-id="7a869-160">Yes</span></span>                  | <span data-ttu-id="7a869-161">Да</span><span class="sxs-lookup"><span data-stu-id="7a869-161">Yes</span></span>                | -                   | <span data-ttu-id="7a869-162">Удалить</span><span class="sxs-lookup"><span data-stu-id="7a869-162">Delete</span></span>

> [!NOTE]
> <span data-ttu-id="7a869-163">В этом тексте для ясности используются термины, связанные с реляционной базой данных.</span><span class="sxs-lookup"><span data-stu-id="7a869-163">This text uses relational database terms for clarity.</span></span> <span data-ttu-id="7a869-164">Базы данных NoSQL обычно поддерживают аналогичные операции, но, возможно, с другими именами.</span><span class="sxs-lookup"><span data-stu-id="7a869-164">NoSQL databases typically support similar operations but possibly with different names.</span></span> <span data-ttu-id="7a869-165">Дополнительные сведения см. в документации поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="7a869-165">Consult your database provider documentation for more information.</span></span>

## <a name="tracking-from-queries"></a><span data-ttu-id="7a869-166">Отслеживание из запросов</span><span class="sxs-lookup"><span data-stu-id="7a869-166">Tracking from queries</span></span>

<span data-ttu-id="7a869-167">Отслеживание изменений EF Core наиболее эффективно, когда один и тот же экземпляр <xref:Microsoft.EntityFrameworkCore.DbContext> используется для запрашивания сущностей и их обновления путем вызова <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A>.</span><span class="sxs-lookup"><span data-stu-id="7a869-167">EF Core change tracking works best when the same <xref:Microsoft.EntityFrameworkCore.DbContext> instance is used to both query for entities and update them by calling <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A>.</span></span> <span data-ttu-id="7a869-168">Это происходит потому, что EF Core автоматически отслеживает состояние запрашиваемых сущностей и определяет изменения, внесенные в эти сущности при вызове SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="7a869-168">This is because EF Core automatically tracks the state of queried entities and then detects any changes made to these entities when SaveChanges is called.</span></span>

<span data-ttu-id="7a869-169">Этот подход имеет несколько преимуществ по сравнению с [явным отслеживанием экземпляров сущностей](xref:core/change-tracking/explicit-tracking):</span><span class="sxs-lookup"><span data-stu-id="7a869-169">This approach has several advantages over [explicitly tracking entity instances](xref:core/change-tracking/explicit-tracking):</span></span>

- <span data-ttu-id="7a869-170">Он является простым.</span><span class="sxs-lookup"><span data-stu-id="7a869-170">It is simple.</span></span> <span data-ttu-id="7a869-171">Состояниями сущностей редко требуется управлять явным образом — EF Core следит за изменениями состояния.</span><span class="sxs-lookup"><span data-stu-id="7a869-171">Entity states rarely need to be manipulated explicitly--EF Core takes care of state changes.</span></span>
- <span data-ttu-id="7a869-172">Обновления ограничиваются только теми значениями, которые действительно изменились.</span><span class="sxs-lookup"><span data-stu-id="7a869-172">Updates are limited to only those values that have actually changed.</span></span>
- <span data-ttu-id="7a869-173">Значения [теневых свойств](xref:core/modeling/shadow-properties) сохраняются и используются по мере необходимости.</span><span class="sxs-lookup"><span data-stu-id="7a869-173">The values of [shadow properties](xref:core/modeling/shadow-properties) are preserved and used as needed.</span></span> <span data-ttu-id="7a869-174">Это особенно важно, если внешние ключи хранятся в теневом состоянии.</span><span class="sxs-lookup"><span data-stu-id="7a869-174">This is especially relevant when foreign keys are stored in shadow state.</span></span>
- <span data-ttu-id="7a869-175">Исходные значения свойств сохраняются автоматически и используются для эффективного обновления.</span><span class="sxs-lookup"><span data-stu-id="7a869-175">The original values of properties are preserved automatically and used for efficient updates.</span></span>

## <a name="simple-query-and-update"></a><span data-ttu-id="7a869-176">Простой запрос и обновление</span><span class="sxs-lookup"><span data-stu-id="7a869-176">Simple query and update</span></span>

<span data-ttu-id="7a869-177">Например, рассмотрим простую модель с участием блогов и публикаций:</span><span class="sxs-lookup"><span data-stu-id="7a869-177">For example, consider a simple blog/posts model:</span></span>

<!--
public class Blog
{
    public int Id { get; set; }

    public string Name { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }

    public int? BlogId { get; set; }
    public Blog Blog { get; set; }
}
-->
[!code-csharp[Model](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/GeneratedKeysSamples.cs?name=Model)]

<span data-ttu-id="7a869-178">Эту модель можно использовать для запрашивания блогов и публикаций с последующим внесением определенных обновлений в базу данных:</span><span class="sxs-lookup"><span data-stu-id="7a869-178">We can use this model to query for blogs and posts and then make some updates to the database:</span></span>

<!--
            using var context = new BlogsContext();

            var blog = context.Blogs.Include(e => e.Posts).First(e => e.Name == ".NET Blog");

            blog.Name = ".NET Blog (Updated!)";

            foreach (var post in blog.Posts.Where(e => !e.Title.Contains("5.0")))
            {
                post.Title = post.Title.Replace("5", "5.0");
            }

            context.SaveChanges();
-->
[!code-csharp[Simple_query_and_update_1](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/GeneratedKeysSamples.cs?name=Simple_query_and_update_1)]

<span data-ttu-id="7a869-179">Вызов SaveChanges приводит к следующим обновлениям базы данных с использованием SQLite в качестве примера базы данных:</span><span class="sxs-lookup"><span data-stu-id="7a869-179">Calling SaveChanges results in the following database updates, using SQLite as an example database:</span></span>

```sql
-- Executed DbCommand (0ms) [Parameters=[@p1='1' (DbType = String), @p0='.NET Blog (Updated!)' (Size = 20)], CommandType='Text', CommandTimeout='30']
UPDATE "Blogs" SET "Name" = @p0
WHERE "Id" = @p1;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p1='2' (DbType = String), @p0='Announcing F# 5.0' (Size = 17)], CommandType='Text', CommandTimeout='30']
UPDATE "Posts" SET "Title" = @p0
WHERE "Id" = @p1;
SELECT changes();
```

<span data-ttu-id="7a869-180">[Представление отладки средства отслеживания изменений](xref:core/change-tracking/debug-views) — это удобный способ визуализации отслеживаемых сущностей и их состояния.</span><span class="sxs-lookup"><span data-stu-id="7a869-180">The [change tracker debug view](xref:core/change-tracking/debug-views) is a great way visualize which entities are being tracked and what their states are.</span></span> <span data-ttu-id="7a869-181">Например, перед вызовом SaveChanges вставьте следующий код в приведенный выше пример:</span><span class="sxs-lookup"><span data-stu-id="7a869-181">For example, inserting the following code into the sample above before calling SaveChanges:</span></span>

<!--
                context.ChangeTracker.DetectChanges();
                Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Simple_query_and_update_2](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/GeneratedKeysSamples.cs?name=Simple_query_and_update_2)]

<span data-ttu-id="7a869-182">Будут получены следующие выходные данные:</span><span class="sxs-lookup"><span data-stu-id="7a869-182">Generates the following output:</span></span>

```output
Blog {Id: 1} Modified
  Id: 1 PK
  Name: '.NET Blog (Updated!)' Modified Originally '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}, {Id: 3}]
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
Post {Id: 2} Modified
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5.0' Modified Originally 'Announcing F# 5'
  Blog: {Id: 1}
```

<span data-ttu-id="7a869-183">Учитывайте следующее:</span><span class="sxs-lookup"><span data-stu-id="7a869-183">Notice specifically:</span></span>

- <span data-ttu-id="7a869-184">Свойство `Blog.Name` помечено как измененное (`Name: '.NET Blog (Updated!)' Modified Originally '.NET Blog'`), что приводит к тому, что состояние блога изменяется на `Modified`.</span><span class="sxs-lookup"><span data-stu-id="7a869-184">The `Blog.Name` property is marked as modified (`Name: '.NET Blog (Updated!)' Modified Originally '.NET Blog'`), and this results in the blog being in the `Modified` state.</span></span>
- <span data-ttu-id="7a869-185">Свойство `Post.Title` публикации 2 помечено как измененное (`Title: 'Announcing F# 5.0' Modified Originally 'Announcing F# 5'`), что приводит к тому, что состояние публикации изменяется на `Modified`.</span><span class="sxs-lookup"><span data-stu-id="7a869-185">The `Post.Title` property of post 2 is marked as modified (`Title: 'Announcing F# 5.0' Modified Originally 'Announcing F# 5'`), and this results in this post being in the `Modified` state.</span></span>
- <span data-ttu-id="7a869-186">Другие значения свойств публикации 2 не изменились и поэтому не помечаются как измененные.</span><span class="sxs-lookup"><span data-stu-id="7a869-186">The other property values of post 2 have not changed and are therefore not marked as modified.</span></span> <span data-ttu-id="7a869-187">Именно поэтому эти значения не включаются в обновление базы данных.</span><span class="sxs-lookup"><span data-stu-id="7a869-187">This is why these values are not included in the database update.</span></span>
- <span data-ttu-id="7a869-188">Другая публикация вообще не была изменена.</span><span class="sxs-lookup"><span data-stu-id="7a869-188">The other post was not modified in any way.</span></span> <span data-ttu-id="7a869-189">Именно поэтому она по-прежнему находится в состоянии `Unchanged` и не включается в обновление базы данных.</span><span class="sxs-lookup"><span data-stu-id="7a869-189">This is why it is still in the `Unchanged` state and is not included in the database update.</span></span>

## <a name="query-then-insert-update-and-delete"></a><span data-ttu-id="7a869-190">Запрос, вставка, обновление и удаление</span><span class="sxs-lookup"><span data-stu-id="7a869-190">Query then insert, update, and delete</span></span>

<span data-ttu-id="7a869-191">Такие обновления, как в предыдущем примере, можно сочетать с операциями вставки и удаления в одной единице работы.</span><span class="sxs-lookup"><span data-stu-id="7a869-191">Updates like those in the previous example can be combined with inserts and deletes in the same unit-of-work.</span></span> <span data-ttu-id="7a869-192">Пример:</span><span class="sxs-lookup"><span data-stu-id="7a869-192">For example:</span></span>

<!--
            using var context = new BlogsContext();

            var blog = context.Blogs.Include(e => e.Posts).First(e => e.Name == ".NET Blog");

            // Modify property values
            blog.Name = ".NET Blog (Updated!)";

            // Insert a new Post
            blog.Posts.Add(new Post
            {
                Title = "What’s next for System.Text.Json?",
                Content = ".NET 5.0 was released recently and has come with many..."
            });

            // Mark an existing Post as Deleted
            var postToDelete = blog.Posts.Single(e => e.Title == "Announcing F# 5");
            context.Remove(postToDelete);

            context.ChangeTracker.DetectChanges();
            Console.WriteLine(context.ChangeTracker.DebugView.LongView);

            context.SaveChanges();
-->
[!code-csharp[Query_then_insert_update_and_delete_1](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/GeneratedKeysSamples.cs?name=Query_then_insert_update_and_delete_1)]

<span data-ttu-id="7a869-193">В этом примере:</span><span class="sxs-lookup"><span data-stu-id="7a869-193">In this example:</span></span>

- <span data-ttu-id="7a869-194">Блог и связанные с ним публикации запрашиваются из базы данных и отслеживаются.</span><span class="sxs-lookup"><span data-stu-id="7a869-194">A blog and related posts are queried from the database and tracked</span></span>
- <span data-ttu-id="7a869-195">Свойство `Blog.Name` изменяется.</span><span class="sxs-lookup"><span data-stu-id="7a869-195">The `Blog.Name` property is changed</span></span>
- <span data-ttu-id="7a869-196">В коллекцию существующих записей блога добавляется новая публикация.</span><span class="sxs-lookup"><span data-stu-id="7a869-196">A new post is added to the collection of existing posts for the blog</span></span>
- <span data-ttu-id="7a869-197">Существующая публикация помечается для удаления путем вызова <xref:Microsoft.EntityFrameworkCore.DbContext.Remove%2A?displayProperty=nameWithType>.</span><span class="sxs-lookup"><span data-stu-id="7a869-197">An existing post is marked for deletion by calling <xref:Microsoft.EntityFrameworkCore.DbContext.Remove%2A?displayProperty=nameWithType></span></span>

<span data-ttu-id="7a869-198">Еще раз взгляните на [представление отладки средства отслеживания изменений](xref:core/change-tracking/debug-views) перед вызовом команды SaveChanges. Вы увидите, как EF Core отслеживает эти изменения:</span><span class="sxs-lookup"><span data-stu-id="7a869-198">Looking again at the [change tracker debug view](xref:core/change-tracking/debug-views) before calling SaveChanges shows how EF Core is tracking these changes:</span></span>

```output
Blog {Id: 1} Modified
  Id: 1 PK
  Name: '.NET Blog (Updated!)' Modified Originally '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}, {Id: 3}, {Id: -2147482638}]
Post {Id: -2147482638} Added
  Id: -2147482638 PK Temporary
  BlogId: 1 FK
  Content: '.NET 5.0 was released recently and has come with many...'
  Title: 'What's next for System.Text.Json?'
  Blog: {Id: 1}
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
Post {Id: 2} Deleted
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: {Id: 1}
```

<span data-ttu-id="7a869-199">Обратите внимание на указанные ниже моменты.</span><span class="sxs-lookup"><span data-stu-id="7a869-199">Notice that:</span></span>

- <span data-ttu-id="7a869-200">Блог помечен как `Modified`.</span><span class="sxs-lookup"><span data-stu-id="7a869-200">The blog is marked as `Modified`.</span></span> <span data-ttu-id="7a869-201">Это вызовет обновление базы данных.</span><span class="sxs-lookup"><span data-stu-id="7a869-201">This will generate a database update.</span></span>
- <span data-ttu-id="7a869-202">Публикация 2 помечена как `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="7a869-202">Post 2 is marked as `Deleted`.</span></span> <span data-ttu-id="7a869-203">Это вызовет удаление базы данных.</span><span class="sxs-lookup"><span data-stu-id="7a869-203">This will generate a database delete.</span></span>
- <span data-ttu-id="7a869-204">Новая публикация с временным идентификатором связана с блогом 1 и помечена как `Added`.</span><span class="sxs-lookup"><span data-stu-id="7a869-204">A new post with a temporary ID is associated with blog 1 and is marked as `Added`.</span></span> <span data-ttu-id="7a869-205">Это вызовет вставку в базу данных.</span><span class="sxs-lookup"><span data-stu-id="7a869-205">This will generate a database insert.</span></span>

<span data-ttu-id="7a869-206">Это приводит к выполнению следующих команд базы данных (при использовании SQLite) при вызове SaveChanges:</span><span class="sxs-lookup"><span data-stu-id="7a869-206">This results in the following database commands (using SQLite) when SaveChanges is called:</span></span>

```sql
-- Executed DbCommand (0ms) [Parameters=[@p1='1' (DbType = String), @p0='.NET Blog (Updated!)' (Size = 20)], CommandType='Text', CommandTimeout='30']
UPDATE "Blogs" SET "Name" = @p0
WHERE "Id" = @p1;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p0='2' (DbType = String)], CommandType='Text', CommandTimeout='30']
DELETE FROM "Posts"
WHERE "Id" = @p0;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p0='1' (DbType = String), @p1='.NET 5.0 was released recently and has come with many...' (Size = 56), @p2='What's next for System.Text.Json?' (Size = 33)], CommandType='Text', CommandTimeout='30']
INSERT INTO "Posts" ("BlogId", "Content", "Title")
VALUES (@p0, @p1, @p2);
SELECT "Id"
FROM "Posts"
WHERE changes() = 1 AND "rowid" = last_insert_rowid();
```

<span data-ttu-id="7a869-207">Дополнительные сведения о вставке и удалении сущностей см. в статье [Явное отслеживание сущностей](xref:core/change-tracking/explicit-tracking).</span><span class="sxs-lookup"><span data-stu-id="7a869-207">See [Explicitly Tracking Entities](xref:core/change-tracking/explicit-tracking) for more information on inserting and deleting entities.</span></span> <span data-ttu-id="7a869-208">Дополнительные сведения о том, как EF Core автоматически обнаруживает подобные изменения, см. в статье [Обнаружение изменений и уведомления](xref:core/change-tracking/change-detection).</span><span class="sxs-lookup"><span data-stu-id="7a869-208">See [Change Detection and Notifications](xref:core/change-tracking/change-detection) for more information on how EF Core automatically detects changes like this.</span></span>

> [!TIP]
> <span data-ttu-id="7a869-209">Вызовите <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.HasChanges?displayProperty=nameWithType>, чтобы определить, были ли внесены какие-либо изменения, которые приведут к обновлению базы данных с помощью SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="7a869-209">Call <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.HasChanges?displayProperty=nameWithType> to determine whether any changes have been made that will cause SaveChanges to make updates to the database.</span></span> <span data-ttu-id="7a869-210">Если HasChanges возвращает значение false, SaveChanges не будет выполняться.</span><span class="sxs-lookup"><span data-stu-id="7a869-210">If HasChanges return false, then SaveChanges will be a no-op.</span></span>
