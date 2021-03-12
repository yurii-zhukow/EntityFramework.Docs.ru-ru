---
title: Обнаружение изменений и уведомления — EF Core
description: Обнаружение изменений свойств и связей с помощью DetectChanges или уведомлений
author: ajcvickers
ms.date: 12/30/2020
uid: core/change-tracking/change-detection
ms.openlocfilehash: fae8bdb1a89478531535b377f4ba8b02d1c848f4
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103023189"
---
# <a name="change-detection-and-notifications"></a><span data-ttu-id="edff4-103">Обнаружение изменений и уведомления</span><span class="sxs-lookup"><span data-stu-id="edff4-103">Change Detection and Notifications</span></span>

<span data-ttu-id="edff4-104">Каждый экземпляр <xref:Microsoft.EntityFrameworkCore.DbContext> отслеживает изменения, внесенные в сущности.</span><span class="sxs-lookup"><span data-stu-id="edff4-104">Each <xref:Microsoft.EntityFrameworkCore.DbContext> instance tracks changes made to entities.</span></span> <span data-ttu-id="edff4-105">Эти отслеживаемые сущности, в свою очередь, записывают изменения в базу данных при вызове <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A>.</span><span class="sxs-lookup"><span data-stu-id="edff4-105">These tracked entities in turn drive the changes to the database when <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> is called.</span></span> <span data-ttu-id="edff4-106">Это рассматривается в [Отслеживание изменений в EF Core](xref:core/change-tracking/index), и в этом документе предполагается, что состояния сущностей и основы отслеживания изменений Entity Framework Core (EF Core) понятны.</span><span class="sxs-lookup"><span data-stu-id="edff4-106">This is covered in [Change Tracking in EF Core](xref:core/change-tracking/index), and this document assumes that entity states and the basics of Entity Framework Core (EF Core) change tracking are understood.</span></span>

<span data-ttu-id="edff4-107">Для отслеживания изменений свойств и связей требуется, чтобы DbContext мог обнаруживать эти изменения.</span><span class="sxs-lookup"><span data-stu-id="edff4-107">Tracking property and relationship changes requires that the DbContext is able to detect these changes.</span></span> <span data-ttu-id="edff4-108">В этом документе описано, как происходит это обнаружение, а также как использовать уведомления о свойствах или прокси-серверы отслеживания изменений для немедленного обнаружения изменений.</span><span class="sxs-lookup"><span data-stu-id="edff4-108">This document covers how this detection happens, as well as how to use property notifications or change-tracking proxies to force immediate detection of changes.</span></span>

> [!TIP]
> <span data-ttu-id="edff4-109">Вы можете запустить и отладить весь код, используемый в этой документации, [скачав пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/ChangeTracking/ChangeDetectionAndNotifications).</span><span class="sxs-lookup"><span data-stu-id="edff4-109">You can run and debug into all the code in this document by [downloading the sample code from GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/ChangeTracking/ChangeDetectionAndNotifications).</span></span>

## <a name="snapshot-change-tracking"></a><span data-ttu-id="edff4-110">Отслеживание изменений по моментальному снимку</span><span class="sxs-lookup"><span data-stu-id="edff4-110">Snapshot change tracking</span></span>

<span data-ttu-id="edff4-111">По умолчанию EF Core создает моментальный снимок значений свойств каждой сущности при ее первой отслеживании экземпляром DbContext.</span><span class="sxs-lookup"><span data-stu-id="edff4-111">By default, EF Core creates a snapshot of every entity's property values when it is first tracked by a DbContext instance.</span></span> <span data-ttu-id="edff4-112">Значения, хранящиеся в этом снимке, затем сравниваются с текущими значениями сущности, чтобы определить, какие значения свойств изменились.</span><span class="sxs-lookup"><span data-stu-id="edff4-112">The values stored in this snapshot are then compared against the current values of the entity in order to determine which property values have changed.</span></span>

<span data-ttu-id="edff4-113">Это обнаружение изменений происходит при вызове SaveChanges, чтобы гарантировать, что все измененные значения будут обнаружены перед отправкой обновлений в базу данных.</span><span class="sxs-lookup"><span data-stu-id="edff4-113">This detection of changes happens when SaveChanges is called to ensure all changed values are detected before sending updates to the database.</span></span> <span data-ttu-id="edff4-114">Однако обнаружение изменений также происходит в других случаях, чтобы обеспечить работу приложения с актуальными данными отслеживания.</span><span class="sxs-lookup"><span data-stu-id="edff4-114">However, the detection of changes also happens at other times to ensure the application is working with up-to-date tracking information.</span></span> <span data-ttu-id="edff4-115">Обнаружение изменений можно принудительно выполнить в любое время, вызвав <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges?displayProperty=nameWithType> .</span><span class="sxs-lookup"><span data-stu-id="edff4-115">Detection of changes can be forced at any time by calling <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges?displayProperty=nameWithType>.</span></span>

### <a name="when-change-detection-is-needed"></a><span data-ttu-id="edff4-116">Когда требуется обнаружение изменений</span><span class="sxs-lookup"><span data-stu-id="edff4-116">When change detection is needed</span></span>

<span data-ttu-id="edff4-117">Обнаружение изменений необходимо при изменении свойства или навигации _без использования EF Core для внесения этого изменения_.</span><span class="sxs-lookup"><span data-stu-id="edff4-117">Detection of changes is needed when a property or navigation has been changed _without using EF Core to make this change_.</span></span> <span data-ttu-id="edff4-118">Например, можно загрузить блоги и записи, а затем внести изменения в эти сущности:</span><span class="sxs-lookup"><span data-stu-id="edff4-118">For example, consider loading blogs and posts and then making changes to these entities:</span></span>

<!--
        using var context = new BlogsContext();
        var blog = context.Blogs.Include(e => e.Posts).First(e => e.Name == ".NET Blog");

        // Change a property value
        blog.Name = ".NET Blog (Updated!)";

        // Add a new entity to a navigation
        blog.Posts.Add(new Post
        {
            Title = "What’s next for System.Text.Json?",
            Content = ".NET 5.0 was released recently and has come with many..."
        });

        Console.WriteLine(context.ChangeTracker.DebugView.LongView);
        context.ChangeTracker.DetectChanges();
        Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Snapshot_change_tracking_1](../../../samples/core/ChangeTracking/ChangeDetectionAndNotifications/SnapshotSamples.cs?name=Snapshot_change_tracking_1)]

<span data-ttu-id="edff4-119">[Просмотр представления отладки средства записи изменений](xref:core/change-tracking/debug-views) перед вызовом <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges?displayProperty=nameWithType> показывает, что внесенные изменения не были обнаружены и, следовательно, не отражаются в состояниях сущностей и измененных данных свойств:</span><span class="sxs-lookup"><span data-stu-id="edff4-119">Looking at the [change tracker debug view](xref:core/change-tracking/debug-views) before calling <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges?displayProperty=nameWithType> shows that the changes made have not been detected and hence are not reflected in the entity states and modified property data:</span></span>

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog (Updated!)' Originally '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}, <not found>]
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
Post {Id: 2} Unchanged
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: {Id: 1}
```

<span data-ttu-id="edff4-120">В частности, состояние записи в блоге остается `Unchanged` , а новая запись не отображается в виде отслеживающей сущности.</span><span class="sxs-lookup"><span data-stu-id="edff4-120">Specifically, the state of the blog entry is still `Unchanged`, and the new post does not appear as a tracked entity.</span></span> <span data-ttu-id="edff4-121">(Внимательный заметит, что свойства сообщают о своих новых значениях, даже если эти изменения еще не обнаружены EF Core.</span><span class="sxs-lookup"><span data-stu-id="edff4-121">(The astute will notice properties report their new values, even though these changes have not yet been detected by EF Core.</span></span> <span data-ttu-id="edff4-122">Это происходит потому, что представление отладки считывает текущие значения непосредственно из экземпляра сущности.)</span><span class="sxs-lookup"><span data-stu-id="edff4-122">This is because the debug view is reading current values directly from the entity instance.)</span></span>

<span data-ttu-id="edff4-123">Сравните это с представлением отладки после вызова DetectChanges:</span><span class="sxs-lookup"><span data-stu-id="edff4-123">Contrast this with the debug view after calling DetectChanges:</span></span>

```output
Blog {Id: 1} Modified
  Id: 1 PK
  Name: '.NET Blog (Updated!)' Modified Originally '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}, {Id: -2147482643}]
Post {Id: -2147482643} Added
  Id: -2147482643 PK Temporary
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
Post {Id: 2} Unchanged
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: {Id: 1}
```

<span data-ttu-id="edff4-124">Теперь блог правильно помечен как `Modified` , и обнаружена новая запись, которая будет записана как `Added` .</span><span class="sxs-lookup"><span data-stu-id="edff4-124">Now the blog is correctly marked as `Modified` and the new post has been detected and is tracked as `Added`.</span></span>

<span data-ttu-id="edff4-125">В начале этого раздела мы упоминали, что при _использовании EF Core для внесения_ изменений необходимо обнаружить изменения.</span><span class="sxs-lookup"><span data-stu-id="edff4-125">At the start of this section we stated that detecting changes is needed when not using _using EF Core to make the change_.</span></span> <span data-ttu-id="edff4-126">Это происходит в приведенном выше коде.</span><span class="sxs-lookup"><span data-stu-id="edff4-126">This is what is happening in the code above.</span></span> <span data-ttu-id="edff4-127">То есть изменения свойства и навигации вносятся _непосредственно в экземпляры сущности_, а не с помощью каких-либо методов EF Core.</span><span class="sxs-lookup"><span data-stu-id="edff4-127">That is, the changes to the property and navigation are made _directly on the entity instances_, and not by using any EF Core methods.</span></span>

<span data-ttu-id="edff4-128">Сравните это с приведенным ниже кодом, который изменяет сущности таким же образом, но на этот раз с помощью EF Core методов:</span><span class="sxs-lookup"><span data-stu-id="edff4-128">Contrast this to the following code which modifies the entities in the same way, but this time using EF Core methods:</span></span>

<!--
        using var context = new BlogsContext();
        var blog = context.Blogs.Include(e => e.Posts).First(e => e.Name == ".NET Blog");

        // Change a property value
        context.Entry(blog).Property(e => e.Name).CurrentValue = ".NET Blog (Updated!)";

        // Add a new entity to the DbContext
        context.Add(
            new Post
            {
                Blog = blog,
                Title = "What’s next for System.Text.Json?",
                Content = ".NET 5.0 was released recently and has come with many..."
            });

        Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Snapshot_change_tracking_2](../../../samples/core/ChangeTracking/ChangeDetectionAndNotifications/SnapshotSamples.cs?name=Snapshot_change_tracking_2)]

<span data-ttu-id="edff4-129">В этом случае представление отладка изменений Tracker показывает, что все состояния сущностей и изменения свойств известны, даже если обнаружение изменений не произошло.</span><span class="sxs-lookup"><span data-stu-id="edff4-129">In this case the change tracker debug view shows that all entity states and property modifications are known, even though detection of changes has not happened.</span></span> <span data-ttu-id="edff4-130">Это обусловлено тем <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.CurrentValue?displayProperty=nameWithType> , что является методом EF Core, что означает, что EF Core немедленно знает об изменениях, внесенных этим методом.</span><span class="sxs-lookup"><span data-stu-id="edff4-130">This is because <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.CurrentValue?displayProperty=nameWithType> is an EF Core method, which means that EF Core immediately knows about the change made by this method.</span></span> <span data-ttu-id="edff4-131">Аналогичным образом, вызов <xref:Microsoft.EntityFrameworkCore.DbContext.Add%2A?displayProperty=nameWithType> позволяет EF Core немедленно узнать о новой сущности и отвести ее соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="edff4-131">Likewise, calling <xref:Microsoft.EntityFrameworkCore.DbContext.Add%2A?displayProperty=nameWithType> allows EF Core to immediately know about the new entity and track it appropriately.</span></span>

> [!TIP]
> <span data-ttu-id="edff4-132">Не пытайтесь обнаружить изменения, всегда используя методы EF Core для внесения изменений в сущности.</span><span class="sxs-lookup"><span data-stu-id="edff4-132">Don't attempt to avoid detecting changes by always using EF Core methods to make entity changes.</span></span> <span data-ttu-id="edff4-133">Это часто бывает более громоздким и менее хорошо, чем внесение изменений в сущности обычным способом.</span><span class="sxs-lookup"><span data-stu-id="edff4-133">Doing so is often more cumbersome and performs less well than making changes to entities in the normal way.</span></span> <span data-ttu-id="edff4-134">Цель этого документа — сообщить о том, когда нужно обнаружить изменения, а когда — нет.</span><span class="sxs-lookup"><span data-stu-id="edff4-134">The intention of this document is to inform as to when detecting changes is needed and when it is not.</span></span> <span data-ttu-id="edff4-135">Намерение не порекомендую избегать обнаружения изменений.</span><span class="sxs-lookup"><span data-stu-id="edff4-135">The intention is not to encourage avoidance of change detection.</span></span>

### <a name="methods-that-automatically-detect-changes"></a><span data-ttu-id="edff4-136">Методы, которые автоматически обнаруживают изменения</span><span class="sxs-lookup"><span data-stu-id="edff4-136">Methods that automatically detect changes</span></span>

<span data-ttu-id="edff4-137"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges> вызывается автоматически методами, где это может повлиять на результаты.</span><span class="sxs-lookup"><span data-stu-id="edff4-137"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges> is called automatically by methods where doing so is likely to impact the results.</span></span> <span data-ttu-id="edff4-138">Этими методами являются:</span><span class="sxs-lookup"><span data-stu-id="edff4-138">These methods are:</span></span>

- <span data-ttu-id="edff4-139"><xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A?displayProperty=nameWithType> и <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A?displayProperty=nameWithType> , чтобы убедиться, что все изменения обнаружены перед обновлением базы данных.</span><span class="sxs-lookup"><span data-stu-id="edff4-139"><xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A?displayProperty=nameWithType> and <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A?displayProperty=nameWithType>, to ensure that all changes are detected before updating the database.</span></span>
- <span data-ttu-id="edff4-140"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Entries?displayProperty=nameWithType> и <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Entries%60%601?displayProperty=nameWithType> , чтобы обеспечить актуальность состояний сущностей и измененных свойств.</span><span class="sxs-lookup"><span data-stu-id="edff4-140"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Entries?displayProperty=nameWithType> and <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Entries%60%601?displayProperty=nameWithType>, to ensure entity states and modified properties are up-to-date.</span></span>
- <span data-ttu-id="edff4-141"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.HasChanges?displayProperty=nameWithType>, чтобы убедиться, что результат является точным.</span><span class="sxs-lookup"><span data-stu-id="edff4-141"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.HasChanges?displayProperty=nameWithType>, to ensure that the result is accurate.</span></span>
- <span data-ttu-id="edff4-142"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.CascadeChanges?displayProperty=nameWithType>, чтобы обеспечить правильное состояние сущности для сущностей Principal/Parent перед каскадным.</span><span class="sxs-lookup"><span data-stu-id="edff4-142"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.CascadeChanges?displayProperty=nameWithType>, to ensure correct entity states for principal/parent entities before cascading.</span></span>
- <span data-ttu-id="edff4-143"><xref:Microsoft.EntityFrameworkCore.DbSet%601.Local?displayProperty=nameWithType>, чтобы обеспечить актуальность отслеживающего графа.</span><span class="sxs-lookup"><span data-stu-id="edff4-143"><xref:Microsoft.EntityFrameworkCore.DbSet%601.Local?displayProperty=nameWithType>, to ensure that the tracked graph is up-to-date.</span></span>

<span data-ttu-id="edff4-144">Кроме того, в некоторых случаях обнаружение изменений происходит только в одном экземпляре сущности, а не на графе отслеживаний сущностей.</span><span class="sxs-lookup"><span data-stu-id="edff4-144">There are also some places where detection of changes happens on only a single entity instance, rather than on the entire graph of tracked entities.</span></span> <span data-ttu-id="edff4-145">Это следующие места:</span><span class="sxs-lookup"><span data-stu-id="edff4-145">These places are:</span></span>

- <span data-ttu-id="edff4-146">При использовании <xref:System.Data.Entity.DbContext.Entry%2A?displayProperty=nameWithType> для обеспечения актуальности состояния и измененных свойств сущности.</span><span class="sxs-lookup"><span data-stu-id="edff4-146">When using <xref:System.Data.Entity.DbContext.Entry%2A?displayProperty=nameWithType>, to ensure that the entity's state and modified properties are up-to-date.</span></span>
- <span data-ttu-id="edff4-147">При использовании <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry> таких методов `Property` , как `Collection` , `Reference` или, `Member` чтобы обеспечить актуальность изменений свойств, текущих значений и т. д.</span><span class="sxs-lookup"><span data-stu-id="edff4-147">When using <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry> methods such as `Property`, `Collection`, `Reference` or `Member` to ensure property modifications, current values, etc. are up-to-date.</span></span>
- <span data-ttu-id="edff4-148">Если зависимая или Дочерняя сущность будет удалена из-за того, что необходимая связь была разорвана.</span><span class="sxs-lookup"><span data-stu-id="edff4-148">When a dependent/child entity is going to be deleted because a required relationship has been severed.</span></span> <span data-ttu-id="edff4-149">Это обнаруживает, что сущность не должна удаляться, так как она была повторно дочерней.</span><span class="sxs-lookup"><span data-stu-id="edff4-149">This detects when an entity should not be deleted because it has been re-parented.</span></span>

<span data-ttu-id="edff4-150">Локальное обнаружение изменений для одной сущности может быть инициировано явным образом путем вызова <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.DetectChanges?displayProperty=nameWithType> .</span><span class="sxs-lookup"><span data-stu-id="edff4-150">Local detection of changes for a single entity can be triggered explicitly by calling <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.DetectChanges?displayProperty=nameWithType>.</span></span>

> [!NOTE]
> <span data-ttu-id="edff4-151">Локальное обнаружение изменений может пропускать некоторые изменения, обнаруженные в результате полного обнаружения.</span><span class="sxs-lookup"><span data-stu-id="edff4-151">Local detect changes can miss some changes that a full detection would find.</span></span> <span data-ttu-id="edff4-152">Это происходит, когда каскадные действия, являющиеся результатом необнаруженных изменений в других сущностях, оказывают влияние на рассматриваемую сущность.</span><span class="sxs-lookup"><span data-stu-id="edff4-152">This happens when cascading actions resulting from undetected changes to other entities have an impact on the entity in question.</span></span> <span data-ttu-id="edff4-153">В таких ситуациях приложению может потребоваться принудительно выполнить полную проверку всех сущностей путем явного вызова <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges?displayProperty=nameWithType> .</span><span class="sxs-lookup"><span data-stu-id="edff4-153">In such situations the application may need to force a full scan of all entities by explicitly calling <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges?displayProperty=nameWithType>.</span></span>

### <a name="disabling-automatic-change-detection"></a><span data-ttu-id="edff4-154">Отключение автоматического обнаружения изменений</span><span class="sxs-lookup"><span data-stu-id="edff4-154">Disabling automatic change detection</span></span>

<span data-ttu-id="edff4-155">Производительность обнаружения изменений не является узким местом для большинства приложений.</span><span class="sxs-lookup"><span data-stu-id="edff4-155">The performance of detecting changes is not a bottleneck for most applications.</span></span> <span data-ttu-id="edff4-156">Однако обнаружение изменений может стать проблемой с производительностью для некоторых приложений, которые отслеживают тысячи сущностей.</span><span class="sxs-lookup"><span data-stu-id="edff4-156">However, detecting changes can become a performance problem for some applications that track thousands of entities.</span></span> <span data-ttu-id="edff4-157">(Точное число зависит от многих вещей, например от количества свойств в сущности.) По этой причине автоматическое обнаружение изменений можно отключить с помощью <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.AutoDetectChangesEnabled?displayProperty=nameWithType> .</span><span class="sxs-lookup"><span data-stu-id="edff4-157">(The exact number will dependent on many things, such as the number of properties in the entity.) For this reason the automatic detection of changes can be disabled using <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.AutoDetectChangesEnabled?displayProperty=nameWithType>.</span></span> <span data-ttu-id="edff4-158">Например, рассмотрим обработку сущностей JOIN в связи «многие ко многим» с полезной нагрузкой.</span><span class="sxs-lookup"><span data-stu-id="edff4-158">For example, consider processing join entities in a many-to-many relationship with payloads:</span></span>

<!--
        public override int SaveChanges()
        {
            foreach (var entityEntry in ChangeTracker.Entries<PostTag>()) // Detects changes automatically
            {
                if (entityEntry.State == EntityState.Added)
                {
                    entityEntry.Entity.TaggedBy = "ajcvickers";
                    entityEntry.Entity.TaggedOn = DateTime.Now;
                }
            }

            try
            {
                ChangeTracker.AutoDetectChangesEnabled = false;
                return base.SaveChanges(); // Avoid automatically detecting changes again here
            }
            finally
            {
                ChangeTracker.AutoDetectChangesEnabled = true;
            }
        }
-->
[!code-csharp[SaveChanges](../../../samples/core/ChangeTracking/ChangeDetectionAndNotifications/SnapshotSamples.cs?name=SaveChanges)]

<span data-ttu-id="edff4-159">Как мы узнали из предыдущего раздела, <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Entries%60%601?displayProperty=nameWithType> и <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A?displayProperty=nameWithType> автоматически обнаруживают изменения.</span><span class="sxs-lookup"><span data-stu-id="edff4-159">As we know from the previous section, both <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Entries%60%601?displayProperty=nameWithType> and <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A?displayProperty=nameWithType> automatically detect changes.</span></span> <span data-ttu-id="edff4-160">Однако после вызова записей код не вносит изменения в состояние сущности или свойства.</span><span class="sxs-lookup"><span data-stu-id="edff4-160">However, after calling Entries, the code does not then make any entity or property state changes.</span></span> <span data-ttu-id="edff4-161">(Установка обычных значений свойств для добавленных сущностей не приводит к изменениям состояния.) Таким образом, код отключает ненужное автоматическое обнаружение изменений при вызове базового метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="edff4-161">(Setting normal property values on Added entities does not cause any state changes.) The code therefore disables unnecessary automatic change detection when calling down into the base SaveChanges method.</span></span> <span data-ttu-id="edff4-162">Код также использует блок try/finally, чтобы гарантировать восстановление значения по умолчанию, даже если команда SaveChanges завершается неудачно.</span><span class="sxs-lookup"><span data-stu-id="edff4-162">The code also makes use of a try/finally block to ensure that the default setting is restored even if SaveChanges fails.</span></span>

> [!TIP]
> <span data-ttu-id="edff4-163">Не следует рассчитывать на то, что код должен отключать автоматическое обнаружение изменений для выполнения.</span><span class="sxs-lookup"><span data-stu-id="edff4-163">Do not assume that your code must disable automatic change detection to to perform well.</span></span> <span data-ttu-id="edff4-164">Это необходимо только в том случае, если профилирование приложения с отслеживанием многих сущностей указывает на то, что производительность обнаружения изменений является проблемой.</span><span class="sxs-lookup"><span data-stu-id="edff4-164">This is only needed when profiling an application tracking many entities indicates that performance of change detection is an issue.</span></span>

### <a name="detecting-changes-and-value-conversions"></a><span data-ttu-id="edff4-165">Обнаружение изменений и преобразований значений</span><span class="sxs-lookup"><span data-stu-id="edff4-165">Detecting changes and value conversions</span></span>

<span data-ttu-id="edff4-166">Чтобы использовать отслеживание изменений моментальных снимков с типом сущности, EF Core должен иметь возможность:</span><span class="sxs-lookup"><span data-stu-id="edff4-166">To use snapshot change tracking with an entity type, EF Core must be able to:</span></span>

- <span data-ttu-id="edff4-167">Создание моментального снимка каждого значения свойства при отслеживании сущности</span><span class="sxs-lookup"><span data-stu-id="edff4-167">Make a snapshot of each property value when the entity is tracked</span></span>
- <span data-ttu-id="edff4-168">Сравните это значение с текущим значением свойства</span><span class="sxs-lookup"><span data-stu-id="edff4-168">Compare this value to the current value of the property</span></span>
- <span data-ttu-id="edff4-169">Создание хэш-кода для значения</span><span class="sxs-lookup"><span data-stu-id="edff4-169">Generate a hash code for the value</span></span>

<span data-ttu-id="edff4-170">Это автоматически обрабатывается EF Core для типов, которые могут быть непосредственно сопоставлены с базой данных.</span><span class="sxs-lookup"><span data-stu-id="edff4-170">This is handled automatically by EF Core for types that can be directly mapped to the database.</span></span> <span data-ttu-id="edff4-171">Однако если [для отображения свойства используется преобразователь значений](xref:core/modeling/value-conversions), то этот преобразователь должен указать, как выполнять эти действия.</span><span class="sxs-lookup"><span data-stu-id="edff4-171">However, when a [value converter is used to map a property](xref:core/modeling/value-conversions), then that converter must specify how to perform these actions.</span></span> <span data-ttu-id="edff4-172">Это достигается с помощью компаратора значений, и подробно описывается в документации по [компараторам значений](xref:core/modeling/value-comparers) .</span><span class="sxs-lookup"><span data-stu-id="edff4-172">This is achieved with a value comparer, and is described in detail in the [Value Comparers](xref:core/modeling/value-comparers) documentation.</span></span>

## <a name="notification-entities"></a><span data-ttu-id="edff4-173">Сущности уведомления</span><span class="sxs-lookup"><span data-stu-id="edff4-173">Notification entities</span></span>

<span data-ttu-id="edff4-174">Для большинства приложений рекомендуется отслеживание изменений моментальных снимков.</span><span class="sxs-lookup"><span data-stu-id="edff4-174">Snapshot change tracking is recommended for most applications.</span></span> <span data-ttu-id="edff4-175">Однако приложения, которые отправляют много сущностей и (или) делают множество изменений в этих сущностях, могут использовать преимущества реализации сущностей, которые автоматически уведомляют EF Core при изменении их свойств и значений навигации.</span><span class="sxs-lookup"><span data-stu-id="edff4-175">However, applications that track many entities and/or make many changes to those entities may benefit from implementing entities that automatically notify EF Core when their property and navigation values change.</span></span> <span data-ttu-id="edff4-176">Они называются "сущностями уведомлений".</span><span class="sxs-lookup"><span data-stu-id="edff4-176">These are known as "notification entities".</span></span>

### <a name="implementing-notification-entities"></a><span data-ttu-id="edff4-177">Реализация сущностей уведомления</span><span class="sxs-lookup"><span data-stu-id="edff4-177">Implementing notification entities</span></span>

<span data-ttu-id="edff4-178">В сущностях уведомления используются <xref:System.ComponentModel.INotifyPropertyChanging> интерфейсы и <xref:System.ComponentModel.INotifyPropertyChanged> , которые являются частью библиотеки базовых классов (BCL) .NET.</span><span class="sxs-lookup"><span data-stu-id="edff4-178">Notification entities make use of the <xref:System.ComponentModel.INotifyPropertyChanging> and <xref:System.ComponentModel.INotifyPropertyChanged> interfaces, which are part of the .NET base class library (BCL).</span></span> <span data-ttu-id="edff4-179">Эти интерфейсы определяют события, которые должны срабатывать до и после изменения значения свойства.</span><span class="sxs-lookup"><span data-stu-id="edff4-179">These interfaces define events that must be fired before and after changing a property value.</span></span> <span data-ttu-id="edff4-180">Например:</span><span class="sxs-lookup"><span data-stu-id="edff4-180">For example:</span></span>

<!--
    public class Blog : INotifyPropertyChanging, INotifyPropertyChanged
    {
        public event PropertyChangingEventHandler PropertyChanging;
        public event PropertyChangedEventHandler PropertyChanged;

        private int _id;
        public int Id
        {
            get => _id;
            set
            {
                PropertyChanging?.Invoke(this, new PropertyChangingEventArgs(nameof(Id)));
                _id = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(Id)));
            }
        }

        private string _name;
        public string Name
        {
            get => _name;
            set
            {
                PropertyChanging?.Invoke(this, new PropertyChangingEventArgs(nameof(Name)));
                _name = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(Name)));
            }
        }

        public IList<Post> Posts { get; } = new ObservableCollection<Post>();
    }
-->
[!code-csharp[Model](../../../samples/core/ChangeTracking/ChangeDetectionAndNotifications/NotificationEntitiesSamples.cs?name=Model)]

<span data-ttu-id="edff4-181">Кроме того, все переходы по коллекциям должны реализовываться `INotifyCollectionChanged` ; в приведенном выше примере это выполнено с помощью <xref:System.Collections.ObjectModel.ObservableCollection%601> набора записей.</span><span class="sxs-lookup"><span data-stu-id="edff4-181">In addition, any collection navigations must implement `INotifyCollectionChanged`; in the example above this satisfied by using an <xref:System.Collections.ObjectModel.ObservableCollection%601> of posts.</span></span> <span data-ttu-id="edff4-182">EF Core также поставляется с <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ObservableHashSet%601> реализацией, которая имеет более эффективные Просмотры за счет стабильного упорядочения.</span><span class="sxs-lookup"><span data-stu-id="edff4-182">EF Core also ships with an <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ObservableHashSet%601> implementation that has more efficient lookups at the expense of stable ordering.</span></span>

<span data-ttu-id="edff4-183">Большая часть этого кода уведомления обычно перемещается в Несопоставленный базовый класс.</span><span class="sxs-lookup"><span data-stu-id="edff4-183">Most of this notification code is typically moved into an unmapped base class.</span></span> <span data-ttu-id="edff4-184">Например:</span><span class="sxs-lookup"><span data-stu-id="edff4-184">For example:</span></span>

<!--
    public class Blog : NotifyingEntity
    {
        private int _id;
        public int Id
        {
            get => _id;
            set => SetWithNotify(value, out _id);
        }

        private string _name;
        public string Name
        {
            get => _name;
            set => SetWithNotify(value, out _name);
        }

        public IList<Post> Posts { get; } = new ObservableCollection<Post>();
    }

    public abstract class NotifyingEntity : INotifyPropertyChanging, INotifyPropertyChanged
    {
        protected void SetWithNotify<T>(T value, out T field, [CallerMemberName] string propertyName = "")
        {
            NotifyChanging(propertyName);
            field = value;
            NotifyChanged(propertyName);
        }

        public event PropertyChangingEventHandler PropertyChanging;
        public event PropertyChangedEventHandler PropertyChanged;

        private void NotifyChanged(string propertyName)
            => PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));

        private void NotifyChanging(string propertyName)
            => PropertyChanging?.Invoke(this, new PropertyChangingEventArgs(propertyName));
    }
-->
[!code-csharp[Model](../../../samples/core/ChangeTracking/ChangeDetectionAndNotifications/NotificationWithBaseSamples.cs?name=Model)]

### <a name="configuring-notification-entities"></a><span data-ttu-id="edff4-185">Настройка сущностей уведомления</span><span class="sxs-lookup"><span data-stu-id="edff4-185">Configuring notification entities</span></span>

<span data-ttu-id="edff4-186">Не существует способа EF Core проверки того, что `INotifyPropertyChanging` или `INotifyPropertyChanged` полностью реализовано для использования с EF Core.</span><span class="sxs-lookup"><span data-stu-id="edff4-186">There is no way for EF Core to validate that `INotifyPropertyChanging` or `INotifyPropertyChanged` are fully implemented for use with EF Core.</span></span> <span data-ttu-id="edff4-187">В частности, некоторые способы использования этих интерфейсов выполняют с уведомлениями только определенные свойства, а не все свойства (включая навигацию) в соответствии с требованиями EF Core.</span><span class="sxs-lookup"><span data-stu-id="edff4-187">In particular, some uses of these interfaces do so with notifications only on certain properties, rather than on all properties (including navigations) as required by EF Core.</span></span> <span data-ttu-id="edff4-188">По этой причине EF Core не выполняет автоматическое подключение к этим событиям.</span><span class="sxs-lookup"><span data-stu-id="edff4-188">For this reason, EF Core does not automatically hook into these events.</span></span>

<span data-ttu-id="edff4-189">Вместо этого EF Core должны быть настроены для использования этих сущностей уведомлений.</span><span class="sxs-lookup"><span data-stu-id="edff4-189">Instead, EF Core must be configured to use these notification entities.</span></span> <span data-ttu-id="edff4-190">Обычно это делается для всех типов сущностей путем вызова <xref:Microsoft.EntityFrameworkCore.ModelBuilder.HasChangeTrackingStrategy%2A?displayProperty=nameWithType> .</span><span class="sxs-lookup"><span data-stu-id="edff4-190">This is usually done for all entity types by calling <xref:Microsoft.EntityFrameworkCore.ModelBuilder.HasChangeTrackingStrategy%2A?displayProperty=nameWithType>.</span></span> <span data-ttu-id="edff4-191">Например:</span><span class="sxs-lookup"><span data-stu-id="edff4-191">For example:</span></span>

<!--
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.HasChangeTrackingStrategy(ChangeTrackingStrategy.ChangingAndChangedNotifications);
        }
-->
[!code-csharp[OnModelCreating](../../../samples/core/ChangeTracking/ChangeDetectionAndNotifications/NotificationWithBaseSamples.cs?name=OnModelCreating)]

<span data-ttu-id="edff4-192">(Стратегия также может быть задана по-разному для разных типов сущностей с помощью <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasChangeTrackingStrategy%2A?displayProperty=nameWithType> , но обычно это понизит производительность, так как DetectChanges все еще требуется для тех типов, которые не являются сущностями уведомлений.)</span><span class="sxs-lookup"><span data-stu-id="edff4-192">(The strategy can also be set differently for different entity types using <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasChangeTrackingStrategy%2A?displayProperty=nameWithType>, but this is usually counterproductive since DetectChanges is still required for those types that are not notification entities.)</span></span>

<span data-ttu-id="edff4-193">Для отслеживания изменений полного уведомления необходимо, `INotifyPropertyChanging` чтобы `INotifyPropertyChanged` были реализованы и, и.</span><span class="sxs-lookup"><span data-stu-id="edff4-193">Full notification change tracking requires that both `INotifyPropertyChanging` and `INotifyPropertyChanged` are implemented.</span></span> <span data-ttu-id="edff4-194">Это позволяет сохранять исходные значения непосредственно перед изменением значения свойства, избегая необходимости EF Core создавать моментальные снимки при отслеживании сущности.</span><span class="sxs-lookup"><span data-stu-id="edff4-194">This allows original values to be saved just before the property value is changed, avoiding the need for EF Core to create a snapshot when tracking the entity.</span></span> <span data-ttu-id="edff4-195">Типы сущностей, реализующие только реализацию, `INotifyPropertyChanged` можно также использовать с EF Core.</span><span class="sxs-lookup"><span data-stu-id="edff4-195">Entity types that implement only `INotifyPropertyChanged` can also be used with EF Core.</span></span> <span data-ttu-id="edff4-196">В этом случае EF по-прежнему создает моментальный снимок при отслеживании сущности для отслеживания исходных значений, а затем использует уведомления для немедленного обнаружения изменений, а не требует вызова DetectChanges.</span><span class="sxs-lookup"><span data-stu-id="edff4-196">In this case, EF still creates a snapshot when tracking an entity to keep track of original values, but then uses the notifications to detect changes immediately, rather than needing DetectChanges to be called.</span></span>

<span data-ttu-id="edff4-197">В <xref:Microsoft.EntityFrameworkCore.ChangeTrackingStrategy> следующей таблице приведены различные значения.</span><span class="sxs-lookup"><span data-stu-id="edff4-197">The different <xref:Microsoft.EntityFrameworkCore.ChangeTrackingStrategy> values are summarized in the the following table.</span></span>

| <span data-ttu-id="edff4-198">чанжетраккингстратеги</span><span class="sxs-lookup"><span data-stu-id="edff4-198">ChangeTrackingStrategy</span></span>                              | <span data-ttu-id="edff4-199">Необходимые интерфейсы</span><span class="sxs-lookup"><span data-stu-id="edff4-199">Interfaces needed</span></span>                                      | <span data-ttu-id="edff4-200">Требуется DetectChanges</span><span class="sxs-lookup"><span data-stu-id="edff4-200">Needs DetectChanges</span></span> | <span data-ttu-id="edff4-201">Исходные значения моментальных снимков</span><span class="sxs-lookup"><span data-stu-id="edff4-201">Snapshots original values</span></span>
|:----------------------------------------------------|--------------------------------------------------------|---------------------|--------------------------
| <span data-ttu-id="edff4-202">Моментальный снимок</span><span class="sxs-lookup"><span data-stu-id="edff4-202">Snapshot</span></span>                                            | <span data-ttu-id="edff4-203">Нет</span><span class="sxs-lookup"><span data-stu-id="edff4-203">None</span></span>                                                   | <span data-ttu-id="edff4-204">Да</span><span class="sxs-lookup"><span data-stu-id="edff4-204">Yes</span></span>                 | <span data-ttu-id="edff4-205">Да</span><span class="sxs-lookup"><span data-stu-id="edff4-205">Yes</span></span>
| <span data-ttu-id="edff4-206">чанжеднотификатионс</span><span class="sxs-lookup"><span data-stu-id="edff4-206">ChangedNotifications</span></span>                                | <span data-ttu-id="edff4-207">INotifyPropertyChanged</span><span class="sxs-lookup"><span data-stu-id="edff4-207">INotifyPropertyChanged</span></span>                                 | <span data-ttu-id="edff4-208">Нет</span><span class="sxs-lookup"><span data-stu-id="edff4-208">No</span></span>                  | <span data-ttu-id="edff4-209">Да</span><span class="sxs-lookup"><span data-stu-id="edff4-209">Yes</span></span>
| <span data-ttu-id="edff4-210">чангингандчанжеднотификатионс</span><span class="sxs-lookup"><span data-stu-id="edff4-210">ChangingAndChangedNotifications</span></span>                     | <span data-ttu-id="edff4-211">INotifyPropertyChanged и Инотифипропертичангинг</span><span class="sxs-lookup"><span data-stu-id="edff4-211">INotifyPropertyChanged and INotifyPropertyChanging</span></span>     | <span data-ttu-id="edff4-212">Нет</span><span class="sxs-lookup"><span data-stu-id="edff4-212">No</span></span>                  | <span data-ttu-id="edff4-213">Нет</span><span class="sxs-lookup"><span data-stu-id="edff4-213">No</span></span>
| <span data-ttu-id="edff4-214">чангингандчанжеднотификатионсвисоригиналвалуес</span><span class="sxs-lookup"><span data-stu-id="edff4-214">ChangingAndChangedNotificationsWithOriginalValues</span></span>   | <span data-ttu-id="edff4-215">INotifyPropertyChanged и Инотифипропертичангинг</span><span class="sxs-lookup"><span data-stu-id="edff4-215">INotifyPropertyChanged and INotifyPropertyChanging</span></span>     | <span data-ttu-id="edff4-216">Нет</span><span class="sxs-lookup"><span data-stu-id="edff4-216">No</span></span>                  | <span data-ttu-id="edff4-217">Да</span><span class="sxs-lookup"><span data-stu-id="edff4-217">Yes</span></span>

### <a name="using-notification-entities"></a><span data-ttu-id="edff4-218">Использование сущностей уведомления</span><span class="sxs-lookup"><span data-stu-id="edff4-218">Using notification entities</span></span>

<span data-ttu-id="edff4-219">Сущности уведомлений ведут себя так же, как и любые другие сущности, за исключением того, что внесение изменений в экземпляры сущности не требует вызова <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges?displayProperty=nameWithType> для обнаружения этих изменений.</span><span class="sxs-lookup"><span data-stu-id="edff4-219">Notification entities behave like any other entities, except that making changes to the entity instances do not require a call to <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges?displayProperty=nameWithType> to detect these changes.</span></span> <span data-ttu-id="edff4-220">Например:</span><span class="sxs-lookup"><span data-stu-id="edff4-220">For example:</span></span>

<!--
            using var context = new BlogsContext();
            var blog = context.Blogs.Include(e => e.Posts).First(e => e.Name == ".NET Blog");

            // Change a property value
            blog.Name = ".NET Blog (Updated!)";

            // Add a new entity to a navigation
            blog.Posts.Add(new Post
            {
                Title = "What’s next for System.Text.Json?",
                Content = ".NET 5.0 was released recently and has come with many..."
            });

            Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Notification_entities_2](../../../samples/core/ChangeTracking/ChangeDetectionAndNotifications/NotificationWithBaseSamples.cs?name=Notification_entities_2)]

<span data-ttu-id="edff4-221">При работе с обычными сущностями [представление "Отладка" средства записи изменений](xref:core/change-tracking/debug-views) показало, что эти изменения не были обнаружены до вызова DetectChanges.</span><span class="sxs-lookup"><span data-stu-id="edff4-221">With normal entities, the [change tracker debug view](xref:core/change-tracking/debug-views) showed that these changes were not detected until DetectChanges was called.</span></span> <span data-ttu-id="edff4-222">Просмотр представления Отладка при использовании сущностей уведомления показывает, что эти изменения были обнаружены немедленно:</span><span class="sxs-lookup"><span data-stu-id="edff4-222">Looking at the debug view when notification entities are used shows that these changes have been detected immediately:</span></span>

```output
Blog {Id: 1} Modified
  Id: 1 PK
  Name: '.NET Blog (Updated!)' Modified
  Posts: [{Id: 1}, {Id: 2}, {Id: -2147482643}]
Post {Id: -2147482643} Added
  Id: -2147482643 PK Temporary
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
Post {Id: 2} Unchanged
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: {Id: 1}
```

## <a name="change-tracking-proxies"></a><span data-ttu-id="edff4-223">Прокси-серверы отслеживания изменений</span><span class="sxs-lookup"><span data-stu-id="edff4-223">Change-tracking proxies</span></span>

> [!NOTE]
> <span data-ttu-id="edff4-224">Прокси-серверы отслеживания изменений были введены в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="edff4-224">Change-tracking proxies were introduced in EF Core 5.0.</span></span>

<span data-ttu-id="edff4-225">EF Core могут динамически создавать типы прокси, реализующие <xref:System.ComponentModel.INotifyPropertyChanging> и <xref:System.ComponentModel.INotifyPropertyChanged> .</span><span class="sxs-lookup"><span data-stu-id="edff4-225">EF Core can dynamically generate proxy types that implement <xref:System.ComponentModel.INotifyPropertyChanging> and <xref:System.ComponentModel.INotifyPropertyChanged>.</span></span> <span data-ttu-id="edff4-226">Для этого необходимо установить пакет NuGet [Microsoft. EntityFrameworkCore. прокси](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) и включить учетные записи-посредники с отслеживанием изменений, <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseChangeTrackingProxies%2A> например:</span><span class="sxs-lookup"><span data-stu-id="edff4-226">This requires installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) NuGet package, and enabling change-tracking proxies with <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseChangeTrackingProxies%2A> For example:</span></span>

<!--
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            => optionsBuilder.UseChangeTrackingProxies();
-->
[!code-csharp[OnConfiguring](../../../samples/core/ChangeTracking/ChangeDetectionAndNotifications/ChangeTrackingProxiesSamples.cs?name=OnConfiguring)]

<span data-ttu-id="edff4-227">Создание динамического прокси-сервера включает создание нового динамического типа .NET (с использованием реализации прокси [Castle. Core](https://www.nuget.org/packages/Castle.Core/) ), который наследует от типа сущности, а затем переопределяет все методы задания свойств.</span><span class="sxs-lookup"><span data-stu-id="edff4-227">Creating a dynamic proxy involves creating a new, dynamic .NET type (using the [Castle.Core](https://www.nuget.org/packages/Castle.Core/) proxies implementation), which inherits from the entity type and then overrides all property setters.</span></span> <span data-ttu-id="edff4-228">Поэтому типы сущностей для прокси-серверов должны быть типами, которые могут быть унаследованы от и должны иметь свойства, которые могут быть переопределены.</span><span class="sxs-lookup"><span data-stu-id="edff4-228">Entity types for proxies must therefore be types that can be inherited from and must have properties that can be overridden.</span></span> <span data-ttu-id="edff4-229">Кроме того, структуры навигации по коллекциям, созданные явным образом, должны реализовывать, <xref:System.Collections.Specialized.INotifyCollectionChanged> например:</span><span class="sxs-lookup"><span data-stu-id="edff4-229">Also, collection navigations created explicitly must implement <xref:System.Collections.Specialized.INotifyCollectionChanged> For example:</span></span>

<!--
    public class Blog
    {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }

        public virtual IList<Post> Posts { get; } = new ObservableCollection<Post>();
    }

    public class Post
    {
        public virtual int Id { get; set; }
        public virtual string Title { get; set; }
        public virtual string Content { get; set; }

        public virtual int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }
-->
[!code-csharp[Model](../../../samples/core/ChangeTracking/ChangeDetectionAndNotifications/ChangeTrackingProxiesSamples.cs?name=Model)]

<span data-ttu-id="edff4-230">Одним из значащих недостатков для прокси-серверов отслеживания изменений является то, что EF Core должны всегда отслеживать экземпляры прокси-сервера, никогда не экземпляры базового типа сущности.</span><span class="sxs-lookup"><span data-stu-id="edff4-230">One significant downside to change-tracking proxies is that EF Core must always track instances of the proxy, never instances of the underlying entity type.</span></span> <span data-ttu-id="edff4-231">Это происходит потому, что экземпляры базового типа сущности не будут создавать уведомления, что означает, что изменения, внесенные в эти сущности, будут пропущены.</span><span class="sxs-lookup"><span data-stu-id="edff4-231">This is because instances of the underlying entity type will not generate notifications, which means changes made to these entities will be missed.</span></span>

<span data-ttu-id="edff4-232">EF Core создает экземпляры прокси-сервера автоматически при запросе к базе данных, поэтому эта негативная сторона обычно ограничена отслеживанием новых экземпляров сущностей.</span><span class="sxs-lookup"><span data-stu-id="edff4-232">EF Core creates proxy instances automatically when querying the database, so this downside is generally limited to tracking new entity instances.</span></span> <span data-ttu-id="edff4-233">Эти экземпляры должны быть созданы с помощью <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.CreateProxy%2A> методов расширения, а **не** обычным способом использования `new` .</span><span class="sxs-lookup"><span data-stu-id="edff4-233">These instances must be created using the <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.CreateProxy%2A> extension methods, and **not** in the normal way using `new`.</span></span> <span data-ttu-id="edff4-234">Это означает, что теперь код из предыдущих примеров должен использовать `CreateProxy` :</span><span class="sxs-lookup"><span data-stu-id="edff4-234">This means the code from the previous examples must now make use of `CreateProxy`:</span></span>

<!--
            using var context = new BlogsContext();
            var blog = context.Blogs.Include(e => e.Posts).First(e => e.Name == ".NET Blog");

            // Change a property value
            blog.Name = ".NET Blog (Updated!)";

            // Add a new entity to a navigation
            blog.Posts.Add(
                context.CreateProxy<Post>(
                    p =>
                        {
                            p.Title = "What’s next for System.Text.Json?";
                            p.Content = ".NET 5.0 was released recently and has come with many...";
                        }));

            Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Change_tracking_proxies_1](../../../samples/core/ChangeTracking/ChangeDetectionAndNotifications/ChangeTrackingProxiesSamples.cs?name=Change_tracking_proxies_1)]

## <a name="change-tracking-events"></a><span data-ttu-id="edff4-235">События отслеживания изменений</span><span class="sxs-lookup"><span data-stu-id="edff4-235">Change tracking events</span></span>

<span data-ttu-id="edff4-236">EF Core запускает <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Tracked?displayProperty=nameWithType> событие при первой отслеживании сущности.</span><span class="sxs-lookup"><span data-stu-id="edff4-236">EF Core fires the <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Tracked?displayProperty=nameWithType> event when an entity is tracked for the first time.</span></span> <span data-ttu-id="edff4-237">Будущие изменения состояния сущности приводят к <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.StateChanged?displayProperty=nameWithType> событиям.</span><span class="sxs-lookup"><span data-stu-id="edff4-237">Future entity state changes result in <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.StateChanged?displayProperty=nameWithType> events.</span></span> <span data-ttu-id="edff4-238">Дополнительные сведения см. в статье о [событиях .NET в EF Core](xref:core/logging-events-diagnostics/events).</span><span class="sxs-lookup"><span data-stu-id="edff4-238">See [.NET Events in EF Core](xref:core/logging-events-diagnostics/events) for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="edff4-239">`StateChanged`Событие не запускается при первой отслеживании сущности, несмотря на то, что состояние изменилось с `Detached` на одно из других состояний.</span><span class="sxs-lookup"><span data-stu-id="edff4-239">The `StateChanged` event is not fired when an entity is first tracked, even though the state has changed from `Detached` to one of the other states.</span></span> <span data-ttu-id="edff4-240">Убедитесь, что прослушиваете `StateChanged` события и, `Tracked` чтобы получить все соответствующие уведомления.</span><span class="sxs-lookup"><span data-stu-id="edff4-240">Make sure to listen for both `StateChanged` and `Tracked` events to get all relevant notifications.</span></span>
