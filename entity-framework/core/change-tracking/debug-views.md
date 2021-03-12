---
title: Отладка инспектора изменений — EF Core
description: Использование ChangeTracker DebugView и сообщений журнала для отладки отслеживания изменений EF Core
author: ajcvickers
ms.date: 12/30/2020
uid: core/change-tracking/debug-views
ms.openlocfilehash: cda81728beb5ffbc8604c191e84b3553a7343ec0
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103023176"
---
# <a name="change-tracker-debugging"></a><span data-ttu-id="6efe0-103">Отладка инспектора изменений</span><span class="sxs-lookup"><span data-stu-id="6efe0-103">Change Tracker Debugging</span></span>

<span data-ttu-id="6efe0-104">Средство записи изменений Entity Framework Core (EF Core) создает два вида выходных данных, которые помогут в отладке:</span><span class="sxs-lookup"><span data-stu-id="6efe0-104">The Entity Framework Core (EF Core) change tracker generates two kinds of output to help with debugging:</span></span>

- <span data-ttu-id="6efe0-105"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DebugView%2A?displayProperty=nameWithType>Предоставляет удобное для чтения представление всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="6efe0-105">The <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DebugView%2A?displayProperty=nameWithType> provides a human-readable view of all entities being tracked</span></span>
- <span data-ttu-id="6efe0-106">Сообщения журнала уровня отладки создаются, когда средство отслеживания изменений обнаруживает состояние и устраняет связи.</span><span class="sxs-lookup"><span data-stu-id="6efe0-106">Debug-level log messages are generated when the change tracker detects state and fixes up relationships</span></span>

> [!TIP]
> <span data-ttu-id="6efe0-107">В этом документе предполагается, что состояния сущностей и основы отслеживания изменений EF Core понятны.</span><span class="sxs-lookup"><span data-stu-id="6efe0-107">This document assumes that entity states and the basics of EF Core change tracking are understood.</span></span> <span data-ttu-id="6efe0-108">Дополнительные сведения по этим темам см. [в разделе Отслеживание изменений в EF Core](xref:core/change-tracking/index) .</span><span class="sxs-lookup"><span data-stu-id="6efe0-108">See [Change Tracking in EF Core](xref:core/change-tracking/index) for more information on these topics.</span></span>

> [!TIP]
> <span data-ttu-id="6efe0-109">Вы можете запустить и отладить весь код, используемый в этой документации, [скачав пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/ChangeTracking/ChangeTrackerDebugging).</span><span class="sxs-lookup"><span data-stu-id="6efe0-109">You can run and debug into all the code in this document by [downloading the sample code from GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/ChangeTracking/ChangeTrackerDebugging).</span></span>

## <a name="change-tracker-debug-view"></a><span data-ttu-id="6efe0-110">Представление отладки для средства записи изменений</span><span class="sxs-lookup"><span data-stu-id="6efe0-110">Change tracker debug view</span></span>

<span data-ttu-id="6efe0-111">Доступ к представлению отладки изменений можно получить в отладчике интегрированной среды разработки.</span><span class="sxs-lookup"><span data-stu-id="6efe0-111">The change tracker debug view can be accessed in the debugger of your IDE.</span></span> <span data-ttu-id="6efe0-112">Например, в Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="6efe0-112">For example, with Visual Studio:</span></span>

![Доступ к представлению "Отладка" для средства записи изменений в отладчике Visual Studio](_static/debug-view.png)

<span data-ttu-id="6efe0-114">Доступ к нему также можно получить непосредственно из кода, например для отправки представления отладки в консоль:</span><span class="sxs-lookup"><span data-stu-id="6efe0-114">It can also be accessed directly from code, for example to send the debug view to the console:</span></span>

<!--
        Console.WriteLine(context.ChangeTracker.DebugView.ShortView);
-->
[!code-csharp[Change_tracker_debug_view_1b](../../../samples/core/ChangeTracking/ChangeTrackerDebugging/Samples.cs?name=Change_tracker_debug_view_1b)]

<span data-ttu-id="6efe0-115">Представление Отладка имеет краткую форму и длинную форму.</span><span class="sxs-lookup"><span data-stu-id="6efe0-115">The debug view has a short form and a long form.</span></span> <span data-ttu-id="6efe0-116">В краткой форме показаны объекты отслеживания, их состояние и значения ключей.</span><span class="sxs-lookup"><span data-stu-id="6efe0-116">The short form shows tracked entities, their state, and key values.</span></span> <span data-ttu-id="6efe0-117">В длинную форму также входят все значения свойств и навигации и состояние.</span><span class="sxs-lookup"><span data-stu-id="6efe0-117">The long form also includes all property and navigation values and state.</span></span>

### <a name="the-short-view"></a><span data-ttu-id="6efe0-118">Короткое представление</span><span class="sxs-lookup"><span data-stu-id="6efe0-118">The short view</span></span>

<span data-ttu-id="6efe0-119">Рассмотрим пример представления отладки, используя модель, показанную в конце этого документа.</span><span class="sxs-lookup"><span data-stu-id="6efe0-119">Let's look at a debug view example using the model shown at the end of this document.</span></span> <span data-ttu-id="6efe0-120">Во-первых, мы будем отслеживать некоторые сущности и размещать их в различных состояниях, так что у нас есть хорошие данные отслеживания изменений для просмотра:</span><span class="sxs-lookup"><span data-stu-id="6efe0-120">First, we will track some entities and put them in some different states, just so we have good change tracking data to view:</span></span>

<!--
        using var context = new BlogsContext();

        var blogs = context.Blogs
            .Include(e => e.Posts).ThenInclude(e => e.Tags)
            .Include(e => e.Assets)
            .ToList();

        // Mark something Added
        blogs[0].Posts.Add(
            new Post
            {
                Title = "What’s next for System.Text.Json?",
                Content = ".NET 5.0 was released recently and has come with many new features and..."
            });

        // Mark something Deleted
        blogs[1].Posts.Remove(blogs[1].Posts[1]);

        // Make something Modified
        blogs[0].Name = ".NET Blog (All new!)";

        context.ChangeTracker.DetectChanges();
-->
[!code-csharp[Change_tracker_debug_view_1a](../../../samples/core/ChangeTracking/ChangeTrackerDebugging/Samples.cs?name=Change_tracker_debug_view_1a)]

<span data-ttu-id="6efe0-121">Печать короткого представления на этом этапе, как показано выше, приводит к следующим результатам:</span><span class="sxs-lookup"><span data-stu-id="6efe0-121">Printing the short view at this point, as shown above, results in the following output:</span></span>

```output
Blog {Id: 1} Modified AK {AssetsId: ed727978-1ffe-4709-baee-73913e8e44a0}
Blog {Id: 2} Unchanged AK {AssetsId: 3a54b880-2b9d-486b-9403-dc2e52d36d65}
BlogAssets {Id: 3a54b880-2b9d-486b-9403-dc2e52d36d65} Unchanged FK {Id: 3a54b880-2b9d-486b-9403-dc2e52d36d65}
BlogAssets {Id: ed727978-1ffe-4709-baee-73913e8e44a0} Unchanged FK {Id: ed727978-1ffe-4709-baee-73913e8e44a0}
Post {Id: -2147482643} Added FK {BlogId: 1}
Post {Id: 1} Unchanged FK {BlogId: 1}
Post {Id: 2} Unchanged FK {BlogId: 1}
Post {Id: 3} Unchanged FK {BlogId: 2}
Post {Id: 4} Deleted FK {BlogId: 2}
PostTag (Dictionary<string, object>) {PostsId: 1, TagsId: 1} Unchanged FK {PostsId: 1} FK {TagsId: 1}
PostTag (Dictionary<string, object>) {PostsId: 1, TagsId: 3} Unchanged FK {PostsId: 1} FK {TagsId: 3}
PostTag (Dictionary<string, object>) {PostsId: 2, TagsId: 1} Unchanged FK {PostsId: 2} FK {TagsId: 1}
PostTag (Dictionary<string, object>) {PostsId: 3, TagsId: 2} Unchanged FK {PostsId: 3} FK {TagsId: 2}
PostTag (Dictionary<string, object>) {PostsId: 4, TagsId: 2} Deleted FK {PostsId: 4} FK {TagsId: 2}
Tag {Id: 1} Unchanged
Tag {Id: 2} Unchanged
Tag {Id: 3} Unchanged
```

<span data-ttu-id="6efe0-122">Примечание:</span><span class="sxs-lookup"><span data-stu-id="6efe0-122">Notice:</span></span>

- <span data-ttu-id="6efe0-123">Каждая отслеживающая сущность отображается со значением первичного ключа (PK).</span><span class="sxs-lookup"><span data-stu-id="6efe0-123">Each tracked entity is listed with its primary key (PK) value.</span></span> <span data-ttu-id="6efe0-124">Например, `Blog {Id: 1}`.</span><span class="sxs-lookup"><span data-stu-id="6efe0-124">For example, `Blog {Id: 1}`.</span></span>
- <span data-ttu-id="6efe0-125">Если сущность является типом сущности общего типа, то также отображается тип CLR.</span><span class="sxs-lookup"><span data-stu-id="6efe0-125">If the entity is a shared-type entity type, then it's CLR type is also shown.</span></span> <span data-ttu-id="6efe0-126">Например, `PostTag (Dictionary<string, object>)`.</span><span class="sxs-lookup"><span data-stu-id="6efe0-126">For example, `PostTag (Dictionary<string, object>)`.</span></span>
- <span data-ttu-id="6efe0-127"><xref:Microsoft.EntityFrameworkCore.EntityState>Показан далее.</span><span class="sxs-lookup"><span data-stu-id="6efe0-127">The <xref:Microsoft.EntityFrameworkCore.EntityState> is shown next.</span></span> <span data-ttu-id="6efe0-128">Это будет одно из `Unchanged` ,, `Added` `Modified` или `Deleted` .</span><span class="sxs-lookup"><span data-stu-id="6efe0-128">This will be one of `Unchanged`, `Added`, `Modified`, or `Deleted`.</span></span>
- <span data-ttu-id="6efe0-129">Далее приведены значения для любых альтернативных ключей (AKs).</span><span class="sxs-lookup"><span data-stu-id="6efe0-129">Values for any alternate keys (AKs) are shown next.</span></span> <span data-ttu-id="6efe0-130">Например, `AK {AssetsId: ed727978-1ffe-4709-baee-73913e8e44a0}`.</span><span class="sxs-lookup"><span data-stu-id="6efe0-130">For example, `AK {AssetsId: ed727978-1ffe-4709-baee-73913e8e44a0}`.</span></span>
- <span data-ttu-id="6efe0-131">Наконец, отображаются значения для всех внешних ключей (внешние ключи).</span><span class="sxs-lookup"><span data-stu-id="6efe0-131">Finally, values for any foreign keys (FKs) are shown.</span></span> <span data-ttu-id="6efe0-132">Например, `FK {PostsId: 4} FK {TagsId: 2}`.</span><span class="sxs-lookup"><span data-stu-id="6efe0-132">For example, `FK {PostsId: 4} FK {TagsId: 2}`.</span></span>

### <a name="the-long-view"></a><span data-ttu-id="6efe0-133">Длинное представление</span><span class="sxs-lookup"><span data-stu-id="6efe0-133">The long view</span></span>

<span data-ttu-id="6efe0-134">Длинное представление можно отправить на консоль так же, как в кратком представлении:</span><span class="sxs-lookup"><span data-stu-id="6efe0-134">The long view can be sent to the console in the same way as the short view:</span></span>

<!--
        Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Change_tracker_debug_view_1c](../../../samples/core/ChangeTracking/ChangeTrackerDebugging/Samples.cs?name=Change_tracker_debug_view_1c)]

<span data-ttu-id="6efe0-135">Выходные данные для того же состояния, что и в кратком представлении выше, — это:</span><span class="sxs-lookup"><span data-stu-id="6efe0-135">The output for the same state as the short view above is:</span></span>

```output
Blog {Id: 1} Modified
  Id: 1 PK
  AssetsId: 'ed727978-1ffe-4709-baee-73913e8e44a0' AK
  Name: '.NET Blog (All new!)' Modified Originally '.NET Blog'
  Assets: {Id: ed727978-1ffe-4709-baee-73913e8e44a0}
  Posts: [{Id: 1}, {Id: 2}, {Id: -2147482643}]
Blog {Id: 2} Unchanged
  Id: 2 PK
  AssetsId: '3a54b880-2b9d-486b-9403-dc2e52d36d65' AK
  Name: 'Visual Studio Blog'
  Assets: {Id: 3a54b880-2b9d-486b-9403-dc2e52d36d65}
  Posts: [{Id: 3}]
BlogAssets {Id: 3a54b880-2b9d-486b-9403-dc2e52d36d65} Unchanged
  Id: '3a54b880-2b9d-486b-9403-dc2e52d36d65' PK FK
  Banner: <null>
  Blog: {Id: 2}
BlogAssets {Id: ed727978-1ffe-4709-baee-73913e8e44a0} Unchanged
  Id: 'ed727978-1ffe-4709-baee-73913e8e44a0' PK FK
  Banner: <null>
  Blog: {Id: 1}
Post {Id: -2147482643} Added
  Id: -2147482643 PK Temporary
  BlogId: 1 FK
  Content: '.NET 5.0 was released recently and has come with many new fe...'
  Title: 'What's next for System.Text.Json?'
  Blog: {Id: 1}
  Tags: []
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
  Tags: [{Id: 1}, {Id: 3}]
Post {Id: 2} Unchanged
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: {Id: 1}
  Tags: [{Id: 1}]
Post {Id: 3} Unchanged
  Id: 3 PK
  BlogId: 2 FK
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: {Id: 2}
  Tags: [{Id: 2}]
Post {Id: 4} Deleted
  Id: 4 PK
  BlogId: 2 FK
  Content: 'Examine when database queries were executed and measure how ...'
  Title: 'Database Profiling with Visual Studio'
  Blog: <null>
  Tags: [{Id: 2}]
PostTag (Dictionary<string, object>) {PostsId: 1, TagsId: 1} Unchanged
  PostsId: 1 PK FK
  TagsId: 1 PK FK
PostTag (Dictionary<string, object>) {PostsId: 1, TagsId: 3} Unchanged
  PostsId: 1 PK FK
  TagsId: 3 PK FK
PostTag (Dictionary<string, object>) {PostsId: 2, TagsId: 1} Unchanged
  PostsId: 2 PK FK
  TagsId: 1 PK FK
PostTag (Dictionary<string, object>) {PostsId: 3, TagsId: 2} Unchanged
  PostsId: 3 PK FK
  TagsId: 2 PK FK
PostTag (Dictionary<string, object>) {PostsId: 4, TagsId: 2} Deleted
  PostsId: 4 PK FK
  TagsId: 2 PK FK
Tag {Id: 1} Unchanged
  Id: 1 PK
  Text: '.NET'
  Posts: [{Id: 1}, {Id: 2}]
Tag {Id: 2} Unchanged
  Id: 2 PK
  Text: 'Visual Studio'
  Posts: [{Id: 3}, {Id: 4}]
Tag {Id: 3} Unchanged
  Id: 3 PK
  Text: 'EF Core'
  Posts: [{Id: 1}]
```

<span data-ttu-id="6efe0-136">Каждая отслеживающая сущность и ее состояние отображаются как и прежде.</span><span class="sxs-lookup"><span data-stu-id="6efe0-136">Each tracked entity and its state is shown as before.</span></span> <span data-ttu-id="6efe0-137">Однако в длинном представлении также отображаются значения свойств и навигации.</span><span class="sxs-lookup"><span data-stu-id="6efe0-137">However, the long view also shows property and navigation values.</span></span>

#### <a name="property-values"></a><span data-ttu-id="6efe0-138">Значения свойств</span><span class="sxs-lookup"><span data-stu-id="6efe0-138">Property values</span></span>

<span data-ttu-id="6efe0-139">Для каждого свойства длинное представление показывает, является ли свойство частью первичного ключа (PK), альтернативного ключа (AK) или внешнего ключа (FK).</span><span class="sxs-lookup"><span data-stu-id="6efe0-139">For each property, the long view shows whether or not the property is part of a primary key (PK), alternate key (AK), or foreign key (FK).</span></span> <span data-ttu-id="6efe0-140">Например:</span><span class="sxs-lookup"><span data-stu-id="6efe0-140">For example:</span></span>

- <span data-ttu-id="6efe0-141">`Blog.Id` является свойством первичного ключа: `Id: 1 PK`</span><span class="sxs-lookup"><span data-stu-id="6efe0-141">`Blog.Id` is a primary key property: `Id: 1 PK`</span></span>
- <span data-ttu-id="6efe0-142">`Blog.AssetsId` является альтернативным свойством ключа: `AssetsId: 'ed727978-1ffe-4709-baee-73913e8e44a0' AK`</span><span class="sxs-lookup"><span data-stu-id="6efe0-142">`Blog.AssetsId` is an alternate key property: `AssetsId: 'ed727978-1ffe-4709-baee-73913e8e44a0' AK`</span></span>
- <span data-ttu-id="6efe0-143">`Post.BlogId` является внешним ключевым свойством: `BlogId: 2 FK`</span><span class="sxs-lookup"><span data-stu-id="6efe0-143">`Post.BlogId` is a foreign key property: `BlogId: 2 FK`</span></span>
- <span data-ttu-id="6efe0-144">`BlogAssets.Id` является первичным ключом и свойством внешнего ключа: `Id: '3a54b880-2b9d-486b-9403-dc2e52d36d65' PK FK`</span><span class="sxs-lookup"><span data-stu-id="6efe0-144">`BlogAssets.Id` is both a primary key and a foreign key property: `Id: '3a54b880-2b9d-486b-9403-dc2e52d36d65' PK FK`</span></span>

<span data-ttu-id="6efe0-145">Измененные значения свойств помечаются как такие, а также отображается исходное значение свойства.</span><span class="sxs-lookup"><span data-stu-id="6efe0-145">Property values that have been modified are marked as such, and the original value of the property is also shown.</span></span> <span data-ttu-id="6efe0-146">Например, `Name: '.NET Blog (All new!)' Modified Originally '.NET Blog'`.</span><span class="sxs-lookup"><span data-stu-id="6efe0-146">For example, `Name: '.NET Blog (All new!)' Modified Originally '.NET Blog'`.</span></span>

<span data-ttu-id="6efe0-147">Наконец, `Added` сущности с временными значениями ключа указывают, что это значение является временным.</span><span class="sxs-lookup"><span data-stu-id="6efe0-147">Finally, `Added` entities with temporary key values indicate that the value is temporary.</span></span> <span data-ttu-id="6efe0-148">Например, `Id: -2147482643 PK Temporary`.</span><span class="sxs-lookup"><span data-stu-id="6efe0-148">For example, `Id: -2147482643 PK Temporary`.</span></span>

#### <a name="navigation-values"></a><span data-ttu-id="6efe0-149">Значения навигации</span><span class="sxs-lookup"><span data-stu-id="6efe0-149">Navigation values</span></span>

<span data-ttu-id="6efe0-150">Значения навигации отображаются с использованием значений первичного ключа сущностей, на которые ссылается Навигация.</span><span class="sxs-lookup"><span data-stu-id="6efe0-150">Navigation values are displayed using the primary key values of the entities that the navigations reference.</span></span> <span data-ttu-id="6efe0-151">Например, в приведенных выше выходных данных запись 3 связана с блогом 2.</span><span class="sxs-lookup"><span data-stu-id="6efe0-151">For example, in the output above, post 3 is related to blog 2.</span></span> <span data-ttu-id="6efe0-152">Это означает, что `Post.Blog` Переход указывает на `Blog` экземпляр с идентификатором 2.</span><span class="sxs-lookup"><span data-stu-id="6efe0-152">This means that the `Post.Blog` navigation points to the `Blog` instance with ID 2.</span></span> <span data-ttu-id="6efe0-153">Это показано в виде `Blog: {Id: 2}` .</span><span class="sxs-lookup"><span data-stu-id="6efe0-153">This is shown as `Blog: {Id: 2}`.</span></span>

<span data-ttu-id="6efe0-154">То же самое происходит при переходе по коллекции, за исключением того, что в этом случае может существовать несколько связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="6efe0-154">The same thing happens for collection navigations, except that in this case there can be multiple related entities.</span></span> <span data-ttu-id="6efe0-155">Например, Навигация по коллекции `Blog.Posts` содержит три сущности со значениями ключа 1, 2 и-2147482643 соответственно.</span><span class="sxs-lookup"><span data-stu-id="6efe0-155">For example, the collection navigation `Blog.Posts` contains three entities, with key values 1, 2, and -2147482643 respectively.</span></span> <span data-ttu-id="6efe0-156">Это показано в виде `[{Id: 1}, {Id: 2}, {Id: -2147482643}]` .</span><span class="sxs-lookup"><span data-stu-id="6efe0-156">This is shown as `[{Id: 1}, {Id: 2}, {Id: -2147482643}]`.</span></span>

## <a name="change-tracker-logging"></a><span data-ttu-id="6efe0-157">Ведение журнала изменений средства записи</span><span class="sxs-lookup"><span data-stu-id="6efe0-157">Change tracker logging</span></span>

<span data-ttu-id="6efe0-158">Средство записи изменений записывает сообщения в журнал `Debug` <xref:Microsoft.Extensions.Logging.LogLevel> при каждом [обнаружении изменений свойств или навигации](xref:core/change-tracking/debug-views).</span><span class="sxs-lookup"><span data-stu-id="6efe0-158">The change tracker logs messages at the `Debug` <xref:Microsoft.Extensions.Logging.LogLevel> whenever it [detects property or navigation changes](xref:core/change-tracking/debug-views).</span></span> <span data-ttu-id="6efe0-159">Например, если <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges?displayProperty=nameWithType> вызывается в коде в верхней части этого документа, а [ведение журнала отладки включено](xref:core/logging-events-diagnostics/index), то создаются следующие журналы:</span><span class="sxs-lookup"><span data-stu-id="6efe0-159">For example, when <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges?displayProperty=nameWithType> is called in the code at the top of this document and [debug logging is enabled](xref:core/logging-events-diagnostics/index), then the following logs are generated:</span></span>

```output
dbug: 12/30/2020 13:52:44.815 CoreEventId.DetectChangesStarting[10800] (Microsoft.EntityFrameworkCore.ChangeTracking)
      DetectChanges starting for 'BlogsContext'.
dbug: 12/30/2020 13:52:44.818 CoreEventId.PropertyChangeDetected[10802] (Microsoft.EntityFrameworkCore.ChangeTracking)
      The unchanged property 'Blog.Name' was detected as changed from '.NET Blog' to '.NET Blog (All new!)' and will be marked as modified for entity with key '{Id: 1}'.
dbug: 12/30/2020 13:52:44.820 CoreEventId.StateChanged[10807] (Microsoft.EntityFrameworkCore.ChangeTracking)
      The 'Blog' entity with key '{Id: 1}' tracked by 'BlogsContext' changed state from 'Unchanged' to 'Modified'.
dbug: 12/30/2020 13:52:44.821 CoreEventId.CollectionChangeDetected[10804] (Microsoft.EntityFrameworkCore.ChangeTracking)
      1 entities were added and 0 entities were removed from navigation 'Blog.Posts' on entity with key '{Id: 1}'.
dbug: 12/30/2020 13:52:44.822 CoreEventId.ValueGenerated[10808] (Microsoft.EntityFrameworkCore.ChangeTracking)
      'BlogsContext' generated temporary value '-2147482638' for the property 'Id.Post'.
dbug: 12/30/2020 13:52:44.822 CoreEventId.StartedTracking[10806] (Microsoft.EntityFrameworkCore.ChangeTracking)
      Context 'BlogsContext' started tracking 'Post' entity with key '{Id: -2147482638}'.
dbug: 12/30/2020 13:52:44.827 CoreEventId.CollectionChangeDetected[10804] (Microsoft.EntityFrameworkCore.ChangeTracking)
      0 entities were added and 1 entities were removed from navigation 'Blog.Posts' on entity with key '{Id: 2}'.
dbug: 12/30/2020 13:52:44.827 CoreEventId.StateChanged[10807] (Microsoft.EntityFrameworkCore.ChangeTracking)
      The 'Post' entity with key '{Id: 4}' tracked by 'BlogsContext' changed state from 'Unchanged' to 'Modified'.
dbug: 12/30/2020 13:52:44.829 CoreEventId.CascadeDeleteOrphan[10003] (Microsoft.EntityFrameworkCore.Update)
      An entity of type 'Post' with key '{Id: 4}' changed to 'Deleted' state due to severed required relationship to its parent entity of type 'Blog'.
dbug: 12/30/2020 13:52:44.829 CoreEventId.StateChanged[10807] (Microsoft.EntityFrameworkCore.ChangeTracking)
      The 'Post' entity with key '{Id: 4}' tracked by 'BlogsContext' changed state from 'Modified' to 'Deleted'.
dbug: 12/30/2020 13:52:44.829 CoreEventId.CollectionChangeDetected[10804] (Microsoft.EntityFrameworkCore.ChangeTracking)
      0 entities were added and 1 entities were removed from navigation 'Blog.Posts' on entity with key '{Id: 2}'.
dbug: 12/30/2020 13:52:44.831 CoreEventId.CascadeDelete[10002] (Microsoft.EntityFrameworkCore.Update)
      A cascade state change of an entity of type 'PostTag' with key '{PostsId: 4, TagsId: 2}' to 'Deleted' occurred due to the deletion of its parent entity of type 'Post' with key '{Id: 4}'.
dbug: 12/30/2020 13:52:44.831 CoreEventId.StateChanged[10807] (Microsoft.EntityFrameworkCore.ChangeTracking)
      The 'PostTag' entity with key '{PostsId: 4, TagsId: 2}' tracked by 'BlogsContext' changed state from 'Unchanged' to 'Deleted'.
dbug: 12/30/2020 13:52:44.831 CoreEventId.DetectChangesCompleted[10801] (Microsoft.EntityFrameworkCore.ChangeTracking)
      DetectChanges completed for 'BlogsContext'.
```

<span data-ttu-id="6efe0-160">В следующей таблице перечислены сообщения журнала изменений для записи в журнал.</span><span class="sxs-lookup"><span data-stu-id="6efe0-160">The following table summaries the change tracker logging messages:</span></span>

| <span data-ttu-id="6efe0-161">Идентификатор события</span><span class="sxs-lookup"><span data-stu-id="6efe0-161">Event ID</span></span>                                                                                                               | <span data-ttu-id="6efe0-162">Описание</span><span class="sxs-lookup"><span data-stu-id="6efe0-162">Description</span></span>
|:-----------------------------------------------------------------------------------------------------------------------|----
| <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId.DetectChangesStarting?displayProperty=nameWithType>        | <span data-ttu-id="6efe0-163"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges> запускается</span><span class="sxs-lookup"><span data-stu-id="6efe0-163"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges> is starting</span></span>
| <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId.DetectChangesCompleted?displayProperty=nameWithType>       | <span data-ttu-id="6efe0-164"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges> завершено</span><span class="sxs-lookup"><span data-stu-id="6efe0-164"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges> has completed</span></span>
| <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId.PropertyChangeDetected?displayProperty=nameWithType>       | <span data-ttu-id="6efe0-165">Изменено значение обычного свойства</span><span class="sxs-lookup"><span data-stu-id="6efe0-165">A normal property value has changed</span></span>
| <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId.ForeignKeyChangeDetected?displayProperty=nameWithType>     | <span data-ttu-id="6efe0-166">Изменено значение свойства внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="6efe0-166">A foreign key property value has changed</span></span>
| <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId.CollectionChangeDetected?displayProperty=nameWithType>     | <span data-ttu-id="6efe0-167">Для навигации по коллекции без пропуска были добавлены или удалены связанные сущности.</span><span class="sxs-lookup"><span data-stu-id="6efe0-167">A non-skip collection navigation has had related entities added or removed.</span></span>
| <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId.ReferenceChangeDetected?displayProperty=nameWithType>      | <span data-ttu-id="6efe0-168">Переход по ссылке изменен для указания на другую сущность или установлен в значение null</span><span class="sxs-lookup"><span data-stu-id="6efe0-168">A reference navigation has been changed to point to another entity, or set to null</span></span>
| <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId.StartedTracking?displayProperty=nameWithType>              | <span data-ttu-id="6efe0-169">EF Core начать отслеживание сущности</span><span class="sxs-lookup"><span data-stu-id="6efe0-169">EF Core started tracking an entity</span></span>
| <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId.StateChanged?displayProperty=nameWithType>                 | <span data-ttu-id="6efe0-170"><xref:Microsoft.EntityFrameworkCore.EntityState>Изменена сущность</span><span class="sxs-lookup"><span data-stu-id="6efe0-170">The <xref:Microsoft.EntityFrameworkCore.EntityState> of an entity has changed</span></span>
| <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId.ValueGenerated?displayProperty=nameWithType>               | <span data-ttu-id="6efe0-171">Для свойства было создано значение</span><span class="sxs-lookup"><span data-stu-id="6efe0-171">A value was generated for a property</span></span>
| <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId.SkipCollectionChangeDetected?displayProperty=nameWithType> | <span data-ttu-id="6efe0-172">В навигации по пропускной коллекции были добавлены или удалены связанные сущности</span><span class="sxs-lookup"><span data-stu-id="6efe0-172">A skip collection navigation has had related entities added or removed</span></span>

## <a name="the-model"></a><span data-ttu-id="6efe0-173">Модель</span><span class="sxs-lookup"><span data-stu-id="6efe0-173">The model</span></span>

<span data-ttu-id="6efe0-174">Модель, используемая для приведенных выше примеров, содержит следующие типы сущностей:</span><span class="sxs-lookup"><span data-stu-id="6efe0-174">The model used for the examples above contains the following entity types:</span></span>

<!--
public class Blog
{
    public int Id { get; set; } // Primary key
    public Guid AssetsId { get; set; } // Alternate key
    public string Name { get; set; }

    public IList<Post> Posts { get; } = new List<Post>(); // Collection navigation
    public BlogAssets Assets { get; set; } // Reference navigation
}

public class BlogAssets
{
    public Guid Id { get; set; } // Primary key and foreign key
    public byte[] Banner { get; set; }

    public Blog Blog { get; set; } // Reference navigation
}

public class Post
{
    public int Id { get; set; } // Primary key
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; } // Foreign key
    public Blog Blog { get; set; } // Reference navigation

    public IList<Tag> Tags { get; } = new List<Tag>(); // Skip collection navigation
}

public class Tag
{
    public int Id { get; set; } // Primary key
    public string Text { get; set; }

    public IList<Post> Posts { get; } = new List<Post>(); // Skip collection navigation
}
-->
[!code-csharp[Model](../../../samples/core/ChangeTracking/ChangeTrackerDebugging/Samples.cs?name=Model)]

<span data-ttu-id="6efe0-175">Модель в основном настраивается по соглашению, всего за несколько строк в OnModelCreating:</span><span class="sxs-lookup"><span data-stu-id="6efe0-175">The model is mostly configured by convention, with just a few lines in OnModelCreating:</span></span>

<!--
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder
            .Entity<Blog>()
            .Property(e => e.AssetsId)
            .ValueGeneratedOnAdd();

        modelBuilder
            .Entity<BlogAssets>()
            .HasOne(e => e.Blog)
            .WithOne(e => e.Assets)
            .HasForeignKey<BlogAssets>(e => e.Id)
            .HasPrincipalKey<Blog>(e => e.AssetsId);
    }
-->
[!code-csharp[OnModelCreating](../../../samples/core/ChangeTracking/ChangeTrackerDebugging/Samples.cs?name=OnModelCreating)]
