---
title: События .NET — EF Core
description: События .NET, определяемые EF Core
author: ajcvickers
ms.date: 10/15/2020
uid: core/logging-events-diagnostics/events
ms.openlocfilehash: 21ee65b7a2c5155c4d5b45350f3f47bdcee22921
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94431292"
---
# <a name="net-events-in-ef-core"></a><span data-ttu-id="6101b-103">События .NET в EF Core</span><span class="sxs-lookup"><span data-stu-id="6101b-103">.NET Events in EF Core</span></span>

> [!TIP]  
> <span data-ttu-id="6101b-104">[Пример событий можно загрузить](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Events) с сайта GitHub.</span><span class="sxs-lookup"><span data-stu-id="6101b-104">You can [download the events sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Events) from GitHub.</span></span>

<span data-ttu-id="6101b-105">Entity Framework Core (EF Core) предоставляет [события .NET](/dotnet/standard/events/) для выполнения в качестве обратных вызовов при возникновении определенных моментов в коде EF Core.</span><span class="sxs-lookup"><span data-stu-id="6101b-105">Entity Framework Core (EF Core) exposes [.NET events](/dotnet/standard/events/) to act as callbacks when certain things happen in the EF Core code.</span></span> <span data-ttu-id="6101b-106">События проще, чем [перехватчики](xref:core/logging-events-diagnostics/interceptors) , и обеспечивают более гибкую регистрацию.</span><span class="sxs-lookup"><span data-stu-id="6101b-106">Events are simpler than [interceptors](xref:core/logging-events-diagnostics/interceptors) and allow more flexible registration.</span></span> <span data-ttu-id="6101b-107">Однако они только синхронизируются и поэтому не могут выполнять неблокирующие асинхронные операции ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="6101b-107">However, they are sync only and so cannot perform non-blocking async I/O.</span></span>

<span data-ttu-id="6101b-108">События регистрируются для каждого `DbContext` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="6101b-108">Events are registered per `DbContext` instance.</span></span> <span data-ttu-id="6101b-109">Используйте [прослушиватель диагностики](xref:core/logging-events-diagnostics/diagnostic-listeners) для получения одних и тех же сведений, но для всех экземпляров DbContext в процессе.</span><span class="sxs-lookup"><span data-stu-id="6101b-109">Use a [diagnostic listener](xref:core/logging-events-diagnostics/diagnostic-listeners) to get the same information but for all DbContext instances in the process.</span></span>

## <a name="events-raised-by-ef-core"></a><span data-ttu-id="6101b-110">События, вызванные EF Core</span><span class="sxs-lookup"><span data-stu-id="6101b-110">Events raised by EF Core</span></span>

<span data-ttu-id="6101b-111">EF Core вызывает следующие события:</span><span class="sxs-lookup"><span data-stu-id="6101b-111">The following events are raised by EF Core:</span></span>

| <span data-ttu-id="6101b-112">Событие</span><span class="sxs-lookup"><span data-stu-id="6101b-112">Event</span></span> | <span data-ttu-id="6101b-113">Представленная версия</span><span class="sxs-lookup"><span data-stu-id="6101b-113">Version introduced</span></span> | <span data-ttu-id="6101b-114">При возникновении</span><span class="sxs-lookup"><span data-stu-id="6101b-114">When raised</span></span>
|:------|--------------------|-------
| `DbContext.SavingChanges` <!-- Issue #2748 -->| <span data-ttu-id="6101b-115">5,0</span><span class="sxs-lookup"><span data-stu-id="6101b-115">5.0</span></span> | <span data-ttu-id="6101b-116">В начале <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> или <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A></span><span class="sxs-lookup"><span data-stu-id="6101b-116">At the start of <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> or <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A></span></span>
| `DbContext.SavedChanges`  <!-- Issue #2748 -->| <span data-ttu-id="6101b-117">5,0</span><span class="sxs-lookup"><span data-stu-id="6101b-117">5.0</span></span> | <span data-ttu-id="6101b-118">В конце успешного <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> или <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A></span><span class="sxs-lookup"><span data-stu-id="6101b-118">At the end of a successful <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> or <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A></span></span>
| `DbContext.SaveChangesFailed`  <!-- Issue #2748 -->| <span data-ttu-id="6101b-119">5,0</span><span class="sxs-lookup"><span data-stu-id="6101b-119">5.0</span></span> | <span data-ttu-id="6101b-120">В конце сбоя <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> или <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A></span><span class="sxs-lookup"><span data-stu-id="6101b-120">At the end of a failed <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> or <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A></span></span>
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Tracked?displayProperty=nameWithType> | <span data-ttu-id="6101b-121">2.1</span><span class="sxs-lookup"><span data-stu-id="6101b-121">2.1</span></span> | <span data-ttu-id="6101b-122">При отслеживании сущности в контексте</span><span class="sxs-lookup"><span data-stu-id="6101b-122">When an entity is tracked by the context</span></span>
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.StateChanged?displayProperty=nameWithType> | <span data-ttu-id="6101b-123">2.1</span><span class="sxs-lookup"><span data-stu-id="6101b-123">2.1</span></span> | <span data-ttu-id="6101b-124">Изменение состояния отслеживающей сущности</span><span class="sxs-lookup"><span data-stu-id="6101b-124">When a tracked entity changes its state</span></span>

### <a name="example-timestamp-state-changes"></a><span data-ttu-id="6101b-125">Пример: изменение состояния метки времени</span><span class="sxs-lookup"><span data-stu-id="6101b-125">Example: Timestamp state changes</span></span>

<span data-ttu-id="6101b-126">Каждая сущность, прослеживаниющая DbContext, имеет <xref:Microsoft.EntityFrameworkCore.EntityState> .</span><span class="sxs-lookup"><span data-stu-id="6101b-126">Each entity tracked by a DbContext has an <xref:Microsoft.EntityFrameworkCore.EntityState>.</span></span> <span data-ttu-id="6101b-127">Например, `Added` состояние указывает, что сущность будет вставлена в базу данных.</span><span class="sxs-lookup"><span data-stu-id="6101b-127">For example, the `Added` state indicates that the entity will be inserted into the database.</span></span>

<span data-ttu-id="6101b-128">В этом примере используются <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Tracked> <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.StateChanged> события и для обнаружения изменения состояния сущности.</span><span class="sxs-lookup"><span data-stu-id="6101b-128">This example uses the <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Tracked> and <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.StateChanged> events to detect when an entity changes state.</span></span> <span data-ttu-id="6101b-129">Затем она помечает сущность на текущее время, указывающее, когда произошло это изменение.</span><span class="sxs-lookup"><span data-stu-id="6101b-129">It then stamps the entity with the current time indicating when this change happened.</span></span> <span data-ttu-id="6101b-130">В результате отмечаются метки времени, указывающие, когда сущность была вставлена, удалена или последний раз обновлялась.</span><span class="sxs-lookup"><span data-stu-id="6101b-130">This results in timestamps indicating when the entity was inserted, deleted, and/or last updated.</span></span>

<span data-ttu-id="6101b-131">Типы сущностей в этом примере реализуют интерфейс, который определяет свойства метки времени:</span><span class="sxs-lookup"><span data-stu-id="6101b-131">The entity types in this example implement an interface that defines the timestamp properties:</span></span>

<!--
public interface IHasTimestamps
{
    DateTime? Added { get; set; }
    DateTime? Deleted { get; set; }
    DateTime? Modified { get; set; }
}
-->
[!code-csharp[IHasTimestamps](../../../samples/core/Miscellaneous/Events/Program.cs?name=IHasTimestamps)]

<span data-ttu-id="6101b-132">Затем метод в DbContext приложения может установить метки времени для любой сущности, реализующей этот интерфейс:</span><span class="sxs-lookup"><span data-stu-id="6101b-132">A method on the application's DbContext can then set timestamps for any entity that implements this interface:</span></span>

<!--
    private static void UpdateTimestamps(object sender, EntityEntryEventArgs e)
    {
        if (e.Entry.Entity is IHasTimestamps entityWithTimestamps)
        {
            switch (e.Entry.State)
            {
                case EntityState.Deleted:
                    entityWithTimestamps.Deleted = DateTime.UtcNow;
                    Console.WriteLine($"Stamped for delete: {e.Entry.Entity}");
                    break;
                case EntityState.Modified:
                    entityWithTimestamps.Modified = DateTime.UtcNow;
                    Console.WriteLine($"Stamped for update: {e.Entry.Entity}");
                    break;
                case EntityState.Added:
                    entityWithTimestamps.Added = DateTime.UtcNow;
                    Console.WriteLine($"Stamped for insert: {e.Entry.Entity}");
                    break;
            }
        }
    }
-->
[!code-csharp[UpdateTimestamps](../../../samples/core/Miscellaneous/Events/Program.cs?name=UpdateTimestamps)]

<span data-ttu-id="6101b-133">Этот метод имеет соответствующую сигнатуру для использования в качестве обработчика событий для `Tracked` событий и `StateChanged` .</span><span class="sxs-lookup"><span data-stu-id="6101b-133">This method has the appropriate signature to use as an event handler for both the `Tracked` and `StateChanged` events.</span></span> <span data-ttu-id="6101b-134">Обработчик регистрируется для обоих событий в конструкторе DbContext.</span><span class="sxs-lookup"><span data-stu-id="6101b-134">The handler is registered for both events in the DbContext constructor.</span></span> <span data-ttu-id="6101b-135">Обратите внимание, что события могут быть присоединены к DbContext в любое время; Это не обязательно, если это происходит в конструкторе контекста.</span><span class="sxs-lookup"><span data-stu-id="6101b-135">Note that events can be attached to a DbContext at any time; it is not required that this happen in the context constructor.</span></span>

<!--
    public BlogsContext()
    {
        ChangeTracker.StateChanged += UpdateTimestamps;
        ChangeTracker.Tracked += UpdateTimestamps;
    }
-->
[!code-csharp[ContextConstructor](../../../samples/core/Miscellaneous/Events/Program.cs?name=ContextConstructor)]

<span data-ttu-id="6101b-136">Оба события необходимы, так как новые сущности инициируют `Tracked` события при первой отслеживании.</span><span class="sxs-lookup"><span data-stu-id="6101b-136">Both events are needed because new entities fire `Tracked` events when they are first tracked.</span></span> <span data-ttu-id="6101b-137">`StateChanged` события срабатывают только для сущностей, которые изменяют состояние, пока они _уже_ отслеживается.</span><span class="sxs-lookup"><span data-stu-id="6101b-137">`StateChanged` events are only fired for entities that change state while they are _already_ being tracked.</span></span>

<span data-ttu-id="6101b-138">[Пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Events) для этого примера содержит простое консольное приложение, которое вносит изменения в базу данных блогов:</span><span class="sxs-lookup"><span data-stu-id="6101b-138">The [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Events) for this example contains a simple console application that makes changes to the blogging database:</span></span>

<!--
        using (var context = new BlogsContext())
        {
            context.Database.EnsureDeleted();
            context.Database.EnsureCreated();
            
            context.Add(
                new Blog
                {
                    Id = 1,
                    Name = "EF Blog",
                    Posts =
                    {
                        new Post { Id = 1, Title = "EF Core 3.1!" },
                        new Post { Id = 2, Title = "EF Core 5.0!" }
                    }
                });

            context.SaveChanges();
        }

        using (var context = new BlogsContext())
        {
            var blog = context.Blogs.Include(e => e.Posts).Single();

            blog.Name = "EF Core Blog";
            context.Remove(blog.Posts.First());
            blog.Posts.Add(new Post { Id = 3, Title = "EF Core 6.0!" });

            context.SaveChanges();
        }
-->
[!code-csharp[Demonstration](../../../samples/core/Miscellaneous/Events/Program.cs?name=Demonstration)]

<span data-ttu-id="6101b-139">Выходные данные этого кода показывают изменения состояния и применяемые отметки времени:</span><span class="sxs-lookup"><span data-stu-id="6101b-139">The output from this code shows the state changes happening and the timestamps being applied:</span></span>

```output
Stamped for insert: Blog 1 Added on: 10/15/2020 11:01:26 PM
Stamped for insert: Post 1 Added on: 10/15/2020 11:01:26 PM
Stamped for insert: Post 2 Added on: 10/15/2020 11:01:26 PM
Stamped for delete: Post 1 Added on: 10/15/2020 11:01:26 PM Deleted on: 10/15/2020 11:01:26 PM
Stamped for update: Blog 1 Added on: 10/15/2020 11:01:26 PM Modified on: 10/15/2020 11:01:26 PM
Stamped for insert: Post 3 Added on: 10/15/2020 11:01:26 PM
```
