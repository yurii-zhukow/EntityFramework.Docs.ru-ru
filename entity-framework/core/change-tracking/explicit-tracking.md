---
title: Явная отслеживание сущностей — EF Core
description: Явная отслеживание сущностей с помощью DbContext с использованием добавления, присоединения, обновления и удаления
author: ajcvickers
ms.date: 12/30/2020
uid: core/change-tracking/explicit-tracking
ms.openlocfilehash: 1428096b362c8016f7924c72ec9ac3e2f9203ed6
ms.sourcegitcommit: 7700840119b1639275f3b64836e7abb59103f2e7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2021
ms.locfileid: "98983278"
---
# <a name="explicitly-tracking-entities"></a><span data-ttu-id="aa541-103">Явная отслеживание сущностей</span><span class="sxs-lookup"><span data-stu-id="aa541-103">Explicitly Tracking Entities</span></span>

<span data-ttu-id="aa541-104">Каждый экземпляр <xref:Microsoft.EntityFrameworkCore.DbContext> отслеживает изменения, внесенные в сущности.</span><span class="sxs-lookup"><span data-stu-id="aa541-104">Each <xref:Microsoft.EntityFrameworkCore.DbContext> instance tracks changes made to entities.</span></span> <span data-ttu-id="aa541-105">Эти отслеживаемые сущности, в свою очередь, записывают изменения в базу данных при вызове <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A>.</span><span class="sxs-lookup"><span data-stu-id="aa541-105">These tracked entities in turn drive the changes to the database when <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> is called.</span></span>

<span data-ttu-id="aa541-106">Отслеживание изменений Entity Framework Core (EF Core) лучше подходит, если один и тот же <xref:Microsoft.EntityFrameworkCore.DbContext> экземпляр используется для запроса сущностей и их обновления путем вызова <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> .</span><span class="sxs-lookup"><span data-stu-id="aa541-106">Entity Framework Core (EF Core) change tracking works best when the same <xref:Microsoft.EntityFrameworkCore.DbContext> instance is used to both query for entities and update them by calling <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A>.</span></span> <span data-ttu-id="aa541-107">Это происходит потому, что EF Core автоматически отслеживает состояние запрашиваемых сущностей и определяет изменения, внесенные в эти сущности при вызове SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="aa541-107">This is because EF Core automatically tracks the state of queried entities and then detects any changes made to these entities when SaveChanges is called.</span></span> <span data-ttu-id="aa541-108">Этот подход рассматривается в [Отслеживание изменений EF Core](xref:core/change-tracking/index).</span><span class="sxs-lookup"><span data-stu-id="aa541-108">This approach is covered in [Change Tracking in EF Core](xref:core/change-tracking/index).</span></span>

> [!TIP]
> <span data-ttu-id="aa541-109">В этом документе предполагается, что состояния сущностей и основы отслеживания изменений EF Core понятны.</span><span class="sxs-lookup"><span data-stu-id="aa541-109">This document assumes that entity states and the basics of EF Core change tracking are understood.</span></span> <span data-ttu-id="aa541-110">Дополнительные сведения по этим темам см. [в разделе Отслеживание изменений в EF Core](xref:core/change-tracking/index) .</span><span class="sxs-lookup"><span data-stu-id="aa541-110">See [Change Tracking in EF Core](xref:core/change-tracking/index) for more information on these topics.</span></span>

> [!TIP]
> <span data-ttu-id="aa541-111">Вы можете запустить и отладить весь код, используемый в этой документации, [скачав пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/ChangeTracking/ChangeTrackingInEFCore).</span><span class="sxs-lookup"><span data-stu-id="aa541-111">You can run and debug into all the code in this document by [downloading the sample code from GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/ChangeTracking/ChangeTrackingInEFCore).</span></span>

> [!TIP]
> <span data-ttu-id="aa541-112">Для простоты в этой документации используются и описываются синхронные методы, такие как <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A>, а не их асинхронные эквиваленты, такие как <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A>.</span><span class="sxs-lookup"><span data-stu-id="aa541-112">For simplicity, this document uses and references synchronous methods such as <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> rather their async equivalents such as <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A>.</span></span> <span data-ttu-id="aa541-113">Вызов и ожидание асинхронного метода можно заменить, если не указано иное.</span><span class="sxs-lookup"><span data-stu-id="aa541-113">Calling and awaiting the async method can be substituted unless otherwise noted.</span></span>

## <a name="introduction"></a><span data-ttu-id="aa541-114">Введение</span><span class="sxs-lookup"><span data-stu-id="aa541-114">Introduction</span></span>

<span data-ttu-id="aa541-115">Сущности могут быть явно «присоединены» к <xref:Microsoft.EntityFrameworkCore.DbContext> таким, что затем контекст отслеживает эти сущности.</span><span class="sxs-lookup"><span data-stu-id="aa541-115">Entities can be explicitly "attached" to a <xref:Microsoft.EntityFrameworkCore.DbContext> such that the context then tracks those entities.</span></span> <span data-ttu-id="aa541-116">Это в первую очередь полезно в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="aa541-116">This is primarily useful when:</span></span>

1. <span data-ttu-id="aa541-117">Создание новых сущностей, которые будут вставлены в базу данных.</span><span class="sxs-lookup"><span data-stu-id="aa541-117">Creating new entities that will be inserted into the database.</span></span>
2. <span data-ttu-id="aa541-118">Повторное подключение отключенных сущностей, которые ранее запрашивались _другим_ экземпляром DbContext.</span><span class="sxs-lookup"><span data-stu-id="aa541-118">Re-attaching disconnected entities that were previously queried by a _different_ DbContext instance.</span></span>

<span data-ttu-id="aa541-119">Первый из них потребуется большинству приложений, а является первичным, обрабатываемым <xref:Microsoft.EntityFrameworkCore.DbContext.Add%2A?displayProperty=nameWithType> методами.</span><span class="sxs-lookup"><span data-stu-id="aa541-119">The first of these will be needed by most applications, and is primary handled by the <xref:Microsoft.EntityFrameworkCore.DbContext.Add%2A?displayProperty=nameWithType> methods.</span></span>

<span data-ttu-id="aa541-120">Вторая необходима только приложениям, которые изменяют сущности или их связи, **_пока сущности не отслеживаемы_**.</span><span class="sxs-lookup"><span data-stu-id="aa541-120">The second is only needed by applications that change entities or their relationships **_while the entities are not being tracked_**.</span></span> <span data-ttu-id="aa541-121">Например, веб-приложение может отправлять сущности веб-клиенту, где пользователь вносит изменения и отправляет сущности обратно.</span><span class="sxs-lookup"><span data-stu-id="aa541-121">For example, a web application may send entities to the web client where the user makes changes and sends the entities back.</span></span> <span data-ttu-id="aa541-122">Эти сущности называются "отключенными", так как они были изначально запрошены из DbContext, но были отключены от этого контекста при отправке клиенту.</span><span class="sxs-lookup"><span data-stu-id="aa541-122">These entities are referred to as "disconnected" since they were originally queried from a DbContext, but were then disconnected from that context when sent to the client.</span></span>

<span data-ttu-id="aa541-123">Теперь веб-приложение должно повторно присоединить эти сущности, чтобы они снова отправляются и указывали внесенные изменения, которые <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> могут внести соответствующие обновления в базу данных.</span><span class="sxs-lookup"><span data-stu-id="aa541-123">The web application must now re-attach these entities so that they are again tracked and indicate the changes that have been made such that <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> can make appropriate updates to the database.</span></span> <span data-ttu-id="aa541-124">Это в первую очередь обрабатывается <xref:Microsoft.EntityFrameworkCore.DbContext.Attach%2A?displayProperty=nameWithType> <xref:Microsoft.EntityFrameworkCore.DbContext.Update%2A?displayProperty=nameWithType> методами и.</span><span class="sxs-lookup"><span data-stu-id="aa541-124">This is primarily handled by the <xref:Microsoft.EntityFrameworkCore.DbContext.Attach%2A?displayProperty=nameWithType> and <xref:Microsoft.EntityFrameworkCore.DbContext.Update%2A?displayProperty=nameWithType> methods.</span></span>

> [!TIP]
> <span data-ttu-id="aa541-125">Присоединение сущностей к тому _же экземпляру DbContext_ , из которого они были запрошены, не должен быть обычно нужен.</span><span class="sxs-lookup"><span data-stu-id="aa541-125">Attaching entities to the _same DbContext instance_ that they were queried from should not normally be needed.</span></span> <span data-ttu-id="aa541-126">Не осуществляйте регулярный запрос без отслеживания, а затем присоедините возвращенные сущности к тому же контексту.</span><span class="sxs-lookup"><span data-stu-id="aa541-126">Do not routinely perform a no-tracking query and then attach the returned entities to the same context.</span></span> <span data-ttu-id="aa541-127">Это будет медленнее, чем при использовании отслеживаемого запроса, а также может привести к таким проблемам, как отсутствие значений теневых свойств, что затрудняет его получение.</span><span class="sxs-lookup"><span data-stu-id="aa541-127">This will be slower than using a tracking query, and may also result in issues such as missing shadow property values, making it harder to get right.</span></span>

### <a name="generated-verses-explicit-key-values"></a><span data-ttu-id="aa541-128">Созданные обратные явные значения ключа</span><span class="sxs-lookup"><span data-stu-id="aa541-128">Generated verses explicit key values</span></span>

<span data-ttu-id="aa541-129">По умолчанию [Свойства ключа](xref:core/modeling/keys) Integer и GUID настраиваются для использования [автоматически создаваемых значений ключа](xref:core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="aa541-129">By default, integer and GUID [key properties](xref:core/modeling/keys) are configured to use [automatically generated key values](xref:core/modeling/generated-properties).</span></span> <span data-ttu-id="aa541-130">Это имеет **существенное преимущество для отслеживания изменений: неопределенное значение ключа указывает, что сущность является "новой"**.</span><span class="sxs-lookup"><span data-stu-id="aa541-130">This has a **major advantage for change tracking: an unset key value indicates that the entity is "new"**.</span></span> <span data-ttu-id="aa541-131">По «новому» мы имеем в виду, что он еще не вставлен в базу данных.</span><span class="sxs-lookup"><span data-stu-id="aa541-131">By "new", we mean that it has not yet been inserted into the database.</span></span>

<span data-ttu-id="aa541-132">В следующих разделах используются две модели.</span><span class="sxs-lookup"><span data-stu-id="aa541-132">Two models are used in the following sections.</span></span> <span data-ttu-id="aa541-133">Во-первых, настроено **не** использовать созданные значения ключа:</span><span class="sxs-lookup"><span data-stu-id="aa541-133">The first is configured to **not** use generated key values:</span></span>

<!--
    public class Blog
    {
        [DatabaseGenerated(DatabaseGeneratedOption.None)]
        public int Id { get; set; }

        public string Name { get; set; }

        public IList<Post> Posts { get; } = new List<Post>();
    }

    public class Post
    {
        [DatabaseGenerated(DatabaseGeneratedOption.None)]
        public int Id { get; set; }

        public string Title { get; set; }
        public string Content { get; set; }

        public int? BlogId { get; set; }
        public Blog Blog { get; set; }
    }
-->
[!code-csharp[Model](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysSamples.cs?name=Model)]

<span data-ttu-id="aa541-134">Несгенерированные (т. е. явно задаются) значения ключей отображаются первыми в каждом примере, так как все является очевидным и легко поддерживайтеся.</span><span class="sxs-lookup"><span data-stu-id="aa541-134">Non-generated (i.e. explicitly set) key values are shown first in each example because everything is very explicit and easy to follow.</span></span> <span data-ttu-id="aa541-135">Затем следует пример, в котором используются созданные значения ключа:</span><span class="sxs-lookup"><span data-stu-id="aa541-135">This is then followed by an example where generated key values are used:</span></span>

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

<span data-ttu-id="aa541-136">Обратите внимание, что ключевые свойства в этой модели не требуют дополнительной настройки, так как использование созданных значений ключа является [значением по умолчанию для простых целочисленных ключей](xref:core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="aa541-136">Notice that the key properties in this model need no additional configuration here since using generated key values is the [default for simple integer keys](xref:core/modeling/generated-properties).</span></span>

## <a name="inserting-new-entities"></a><span data-ttu-id="aa541-137">Вставка новых сущностей</span><span class="sxs-lookup"><span data-stu-id="aa541-137">Inserting new entities</span></span>

### <a name="explicit-key-values"></a><span data-ttu-id="aa541-138">Явные значения ключа</span><span class="sxs-lookup"><span data-stu-id="aa541-138">Explicit key values</span></span>

<span data-ttu-id="aa541-139">Сущность должна быть отслеживаниь в `Added` состоянии, которое будет вставлено <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> .</span><span class="sxs-lookup"><span data-stu-id="aa541-139">An entity must be tracked in the `Added` state to be inserted by <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A>.</span></span> <span data-ttu-id="aa541-140">Сущности обычно помещаются в добавленное состояние путем вызова одного из <xref:Microsoft.EntityFrameworkCore.DbContext.Add%2A?displayProperty=nameWithType> ,,, <xref:Microsoft.EntityFrameworkCore.DbContext.AddRange%2A?displayProperty=nameWithType> <xref:Microsoft.EntityFrameworkCore.DbContext.AddAsync%2A?displayProperty=nameWithType> <xref:Microsoft.EntityFrameworkCore.DbContext.AddRangeAsync%2A?displayProperty=nameWithType> или эквивалентных методов в <xref:Microsoft.EntityFrameworkCore.DbSet%601> .</span><span class="sxs-lookup"><span data-stu-id="aa541-140">Entities are typically put in the Added state by calling one of <xref:Microsoft.EntityFrameworkCore.DbContext.Add%2A?displayProperty=nameWithType>, <xref:Microsoft.EntityFrameworkCore.DbContext.AddRange%2A?displayProperty=nameWithType>, <xref:Microsoft.EntityFrameworkCore.DbContext.AddAsync%2A?displayProperty=nameWithType>, <xref:Microsoft.EntityFrameworkCore.DbContext.AddRangeAsync%2A?displayProperty=nameWithType>, or the equivalent methods on <xref:Microsoft.EntityFrameworkCore.DbSet%601>.</span></span>

> [!TIP]
> <span data-ttu-id="aa541-141">Все эти методы работают таким же образом в контексте отслеживания изменений.</span><span class="sxs-lookup"><span data-stu-id="aa541-141">These methods all work in the same way in the context of change tracking.</span></span> <span data-ttu-id="aa541-142">Дополнительные сведения см. в разделе [дополнительные отслеживание изменений функции](xref:core/change-tracking/miscellaneous) .</span><span class="sxs-lookup"><span data-stu-id="aa541-142">See [Additional Change Tracking Features](xref:core/change-tracking/miscellaneous) for more information.</span></span>

<span data-ttu-id="aa541-143">Например, чтобы начать отслеживание нового блога, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="aa541-143">For example, to start tracking a new blog:</span></span>

<!--
            context.Add(
                new Blog
                {
                    Id = 1,
                    Name = ".NET Blog",
                });
-->
[!code-csharp[Inserting_new_entities_1](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysSamples.cs?name=Inserting_new_entities_1)]

<span data-ttu-id="aa541-144">Проверка [представления отладки отслеживания изменений](xref:core/change-tracking/debug-views) после этого вызова показывает, что контекст отслеживает новую сущность в `Added` состоянии:</span><span class="sxs-lookup"><span data-stu-id="aa541-144">Inspecting the [change tracker debug view](xref:core/change-tracking/debug-views) following this call shows that the context is tracking the new entity in the `Added` state:</span></span>

```output
Blog {Id: 1} Added
  Id: 1 PK
  Name: '.NET Blog'
  Posts: []
```

<span data-ttu-id="aa541-145">Однако методы Add не просто работают с отдельной сущностью.</span><span class="sxs-lookup"><span data-stu-id="aa541-145">However, the Add methods don't just work on an individual entity.</span></span> <span data-ttu-id="aa541-146">Фактически они начинают отслеживать _весь граф связанных сущностей_, помещая их в `Added` состояние.</span><span class="sxs-lookup"><span data-stu-id="aa541-146">They actually start tracking an _entire graph of related entities_, putting them all to the `Added` state.</span></span> <span data-ttu-id="aa541-147">Например, чтобы вставить новый блог и связанные с ним новые записи:</span><span class="sxs-lookup"><span data-stu-id="aa541-147">For example, to insert a new blog and associated new posts:</span></span>

<!--
            context.Add(
                new Blog
                {
                    Id = 1,
                    Name = ".NET Blog",
                    Posts =
                    {
                        new Post
                        {
                            Id = 1,
                            Title = "Announcing the Release of EF Core 5.0",
                            Content = "Announcing the release of EF Core 5.0, a full featured cross-platform..."
                        },
                        new Post
                        {
                            Id = 2,
                            Title = "Announcing F# 5",
                            Content = "F# 5 is the latest version of F#, the functional programming language..."
                        }
                    }
                });
-->
[!code-csharp[Inserting_new_entities_2](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysSamples.cs?name=Inserting_new_entities_2)]

<span data-ttu-id="aa541-148">Теперь контекст отслеживает все эти сущности следующим образом `Added` :</span><span class="sxs-lookup"><span data-stu-id="aa541-148">The context is now tracking all these entities as `Added`:</span></span>

```output
Blog {Id: 1} Added
  Id: 1 PK
  Name: '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}]
Post {Id: 1} Added
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
Post {Id: 2} Added
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: {Id: 1}
```

<span data-ttu-id="aa541-149">Обратите внимание, что для `Id` ключевых свойств в приведенных выше примерах были заданы явные значения.</span><span class="sxs-lookup"><span data-stu-id="aa541-149">Notice that explicit values have been set for the `Id` key properties in the examples above.</span></span> <span data-ttu-id="aa541-150">Это обусловлено тем, что в модели настроено использование явно заданных значений ключей, а не автоматически создаваемых значений ключа.</span><span class="sxs-lookup"><span data-stu-id="aa541-150">This is because the model here has been configured to use explicitly set key values, rather than automatically generated key values.</span></span> <span data-ttu-id="aa541-151">Если созданные ключи не используются, _перед_ вызовом необходимо явно задать ключевые свойства `Add` .</span><span class="sxs-lookup"><span data-stu-id="aa541-151">When not using generated keys, the key properties must be explicitly set _before_ calling `Add`.</span></span> <span data-ttu-id="aa541-152">Эти ключевые значения затем вставляются при вызове SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="aa541-152">These key values are then inserted when SaveChanges is called.</span></span> <span data-ttu-id="aa541-153">Например, при использовании SQLite:</span><span class="sxs-lookup"><span data-stu-id="aa541-153">For example, when using SQLite:</span></span>

```sql
-- Executed DbCommand (0ms) [Parameters=[@p0='1' (DbType = String), @p1='.NET Blog' (Size = 9)], CommandType='Text', CommandTimeout='30']
INSERT INTO "Blogs" ("Id", "Name")
VALUES (@p0, @p1);

-- Executed DbCommand (0ms) [Parameters=[@p2='1' (DbType = String), @p3='1' (DbType = String), @p4='Announcing the release of EF Core 5.0, a full featured cross-platform...' (Size = 72), @p5='Announcing the Release of EF Core 5.0' (Size = 37)], CommandType='Text', CommandTimeout='30']
INSERT INTO "Posts" ("Id", "BlogId", "Content", "Title")
VALUES (@p2, @p3, @p4, @p5);

-- Executed DbCommand (0ms) [Parameters=[@p0='2' (DbType = String), @p1='1' (DbType = String), @p2='F# 5 is the latest version of F#, the functional programming language...' (Size = 72), @p3='Announcing F# 5' (Size = 15)], CommandType='Text', CommandTimeout='30']
INSERT INTO "Posts" ("Id", "BlogId", "Content", "Title")
VALUES (@p0, @p1, @p2, @p3);
```

<span data-ttu-id="aa541-154">Все эти сущности будут отслеживаниться в `Unchanged` состоянии после завершения SaveChanges, так как эти сущности теперь существуют в базе данных:</span><span class="sxs-lookup"><span data-stu-id="aa541-154">All of these entities are tracked in the `Unchanged` state after SaveChanges completes, since these entities now exist in the database:</span></span>

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}]
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

### <a name="generated-key-values"></a><span data-ttu-id="aa541-155">Созданные значения ключа</span><span class="sxs-lookup"><span data-stu-id="aa541-155">Generated key values</span></span>

<span data-ttu-id="aa541-156">Как упоминалось выше, [Свойства ключа](xref:core/modeling/keys) Integer и GUID настраиваются для использования [автоматически создаваемых значений ключа](xref:core/modeling/generated-properties) по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="aa541-156">As mentioned above, integer and GUID [key properties](xref:core/modeling/keys) are configured to use [automatically generated key values](xref:core/modeling/generated-properties) by default.</span></span> <span data-ttu-id="aa541-157">Это означает, что приложение _не должно явно задавать значение ключа_.</span><span class="sxs-lookup"><span data-stu-id="aa541-157">This means that the application _must not set any key value explicitly_.</span></span> <span data-ttu-id="aa541-158">Например, чтобы вставить новый блог и опубликовать все записи с созданными значениями ключа:</span><span class="sxs-lookup"><span data-stu-id="aa541-158">For example, to insert a new blog and posts all with generated key values:</span></span>

<!--
            context.Add(
                new Blog
                {
                    Name = ".NET Blog",
                    Posts =
                    {
                        new Post
                        {
                            Title = "Announcing the Release of EF Core 5.0",
                            Content = "Announcing the release of EF Core 5.0, a full featured cross-platform..."
                        },
                        new Post
                        {
                            Title = "Announcing F# 5",
                            Content = "F# 5 is the latest version of F#, the functional programming language..."
                        }
                    }
                });
-->
[!code-csharp[Inserting_new_entities_3](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/GeneratedKeysSamples.cs?name=Inserting_new_entities_3)]

<span data-ttu-id="aa541-159">Как и с явными значениями ключа, контекст теперь отслеживает все эти сущности следующим образом `Added` :</span><span class="sxs-lookup"><span data-stu-id="aa541-159">As with explicit key values, the context is now tracking all these entities as `Added`:</span></span>

```output
Blog {Id: -2147482644} Added
  Id: -2147482644 PK Temporary
  Name: '.NET Blog'
  Posts: [{Id: -2147482637}, {Id: -2147482636}]
Post {Id: -2147482637} Added
  Id: -2147482637 PK Temporary
  BlogId: -2147482644 FK Temporary
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: -2147482644}
Post {Id: -2147482636} Added
  Id: -2147482636 PK Temporary
  BlogId: -2147482644 FK Temporary
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: {Id: -2147482644}
```

<span data-ttu-id="aa541-160">Обратите внимание, что в этом случае для каждой сущности были созданы [временные значения ключей](xref:core/change-tracking/miscellaneous#temporary-values) .</span><span class="sxs-lookup"><span data-stu-id="aa541-160">Notice in this case that [temporary key values](xref:core/change-tracking/miscellaneous#temporary-values) have been generated for each entity.</span></span> <span data-ttu-id="aa541-161">Эти значения используются EF Core до вызова SaveChanges, после чего значения реальных ключей считываются из базы данных.</span><span class="sxs-lookup"><span data-stu-id="aa541-161">These values are used by EF Core until SaveChanges is called, at which point real key values are read back from the database.</span></span> <span data-ttu-id="aa541-162">Например, при использовании SQLite:</span><span class="sxs-lookup"><span data-stu-id="aa541-162">For example, when using SQLite:</span></span>

```sql
-- Executed DbCommand (0ms) [Parameters=[@p0='.NET Blog' (Size = 9)], CommandType='Text', CommandTimeout='30']
INSERT INTO "Blogs" ("Name")
VALUES (@p0);
SELECT "Id"
FROM "Blogs"
WHERE changes() = 1 AND "rowid" = last_insert_rowid();

-- Executed DbCommand (0ms) [Parameters=[@p1='1' (DbType = String), @p2='Announcing the release of EF Core 5.0, a full featured cross-platform...' (Size = 72), @p3='Announcing the Release of EF Core 5.0' (Size = 37)], CommandType='Text', CommandTimeout='30']
INSERT INTO "Posts" ("BlogId", "Content", "Title")
VALUES (@p1, @p2, @p3);
SELECT "Id"
FROM "Posts"
WHERE changes() = 1 AND "rowid" = last_insert_rowid();

-- Executed DbCommand (0ms) [Parameters=[@p0='1' (DbType = String), @p1='F# 5 is the latest version of F#, the functional programming language...' (Size = 72), @p2='Announcing F# 5' (Size = 15)], CommandType='Text', CommandTimeout='30']
INSERT INTO "Posts" ("BlogId", "Content", "Title")
VALUES (@p0, @p1, @p2);
SELECT "Id"
FROM "Posts"
WHERE changes() = 1 AND "rowid" = last_insert_rowid();
```

<span data-ttu-id="aa541-163">После завершения SaveChanges все сущности были обновлены с использованием их реальных значений ключей и отправляются в `Unchanged` состояние, так как теперь они соответствуют состоянию в базе данных:</span><span class="sxs-lookup"><span data-stu-id="aa541-163">After SaveChanges completes, all of the entities have been updated with their real key values and are tracked in the `Unchanged` state since they now match the state in the database:</span></span>

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}]
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

<span data-ttu-id="aa541-164">Это точно такое же конечное состояние, как и в предыдущем примере, в котором использовались явные значения ключа.</span><span class="sxs-lookup"><span data-stu-id="aa541-164">This is exactly the same end-state as the previous example that used explicit key values.</span></span>

> [!TIP]
> <span data-ttu-id="aa541-165">Явное значение ключа по-прежнему можно задать даже при использовании созданных значений ключа.</span><span class="sxs-lookup"><span data-stu-id="aa541-165">An explicit key value can still be set even when using generated key values.</span></span> <span data-ttu-id="aa541-166">После этого EF Core попытается вставить с использованием этого значения ключа.</span><span class="sxs-lookup"><span data-stu-id="aa541-166">EF Core will then attempt to insert using this key value.</span></span> <span data-ttu-id="aa541-167">Некоторые конфигурации базы данных, в том числе SQL Server со столбцами идентификаторов, не поддерживают такие вставки и выдадут ([см. Эти документы в качестве обходного решения](xref:core/providers/sql-server/value-generation#inserting-explicit-values-into-identity-columns)).</span><span class="sxs-lookup"><span data-stu-id="aa541-167">Some database configurations, including SQL Server with Identity columns, do not support such inserts and will throw ([see these docs for a workaround](xref:core/providers/sql-server/value-generation#inserting-explicit-values-into-identity-columns)).</span></span>

## <a name="attaching-existing-entities"></a><span data-ttu-id="aa541-168">Присоединение существующих сущностей</span><span class="sxs-lookup"><span data-stu-id="aa541-168">Attaching existing entities</span></span>

### <a name="explicit-key-values"></a><span data-ttu-id="aa541-169">Явные значения ключа</span><span class="sxs-lookup"><span data-stu-id="aa541-169">Explicit key values</span></span>

<span data-ttu-id="aa541-170">Сущности, возвращаемые из запросов, прописываются в `Unchanged` состоянии.</span><span class="sxs-lookup"><span data-stu-id="aa541-170">Entities returned from queries are tracked in the `Unchanged` state.</span></span> <span data-ttu-id="aa541-171">`Unchanged`Состояние означает, что сущность не была изменена с момента запроса.</span><span class="sxs-lookup"><span data-stu-id="aa541-171">The `Unchanged` state means that the entity has not been modified since it was queried.</span></span> <span data-ttu-id="aa541-172">Отсоединенная сущность, которая, возможно, возвращенная веб-клиентом в запросе HTTP, может быть переведена в это состояние с помощью <xref:Microsoft.EntityFrameworkCore.DbContext.Attach%2A?displayProperty=nameWithType> , <xref:Microsoft.EntityFrameworkCore.DbContext.AttachRange%2A?displayProperty=nameWithType> или эквивалентных методов в <xref:Microsoft.EntityFrameworkCore.DbSet%601> .</span><span class="sxs-lookup"><span data-stu-id="aa541-172">A disconnected entity, perhaps returned from a web client in an HTTP request, can be put into this state using either <xref:Microsoft.EntityFrameworkCore.DbContext.Attach%2A?displayProperty=nameWithType>, <xref:Microsoft.EntityFrameworkCore.DbContext.AttachRange%2A?displayProperty=nameWithType>, or the equivalent methods on <xref:Microsoft.EntityFrameworkCore.DbSet%601>.</span></span> <span data-ttu-id="aa541-173">Например, чтобы начать отслеживание существующего блога, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="aa541-173">For example, to start tracking an existing blog:</span></span>

<!--
            context.Attach(
                new Blog
                {
                    Id = 1,
                    Name = ".NET Blog",
                });
-->
[!code-csharp[Attaching_existing_entities_1](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysSamples.cs?name=Attaching_existing_entities_1)]

> [!NOTE]
> <span data-ttu-id="aa541-174">В примерах здесь явно создаются сущности с `new` целью простоты.</span><span class="sxs-lookup"><span data-stu-id="aa541-174">The examples here are creating entities explicitly with `new` for simplicity.</span></span> <span data-ttu-id="aa541-175">Обычно экземпляры сущностей поступают из другого источника, например, десериализованы из клиента или создаются из данных HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="aa541-175">Normally the entity instances will have come from another source, such as being deserialized from a client, or being created from data in an HTTP Post.</span></span>

<span data-ttu-id="aa541-176">Проверка [представления отладки отслеживания изменений](xref:core/change-tracking/debug-views) после этого вызова показывает, что сущность будет отслеживаться в `Unchanged` состоянии:</span><span class="sxs-lookup"><span data-stu-id="aa541-176">Inspecting the [change tracker debug view](xref:core/change-tracking/debug-views) following this call shows that the entity is tracked in the `Unchanged` state:</span></span>

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Posts: []
```

<span data-ttu-id="aa541-177">Точно так же `Add` , как `Attach` факт, устанавливает весь граф подключенных сущностей в `Unchanged` состояние.</span><span class="sxs-lookup"><span data-stu-id="aa541-177">Just like `Add`, `Attach` actually sets an entire graph of connected entities to the `Unchanged` state.</span></span> <span data-ttu-id="aa541-178">Например, чтобы присоединить существующий блог и связать его с существующими записями:</span><span class="sxs-lookup"><span data-stu-id="aa541-178">For example, to attach an existing blog and associated existing posts:</span></span>

<!--
            context.Attach(
                new Blog
                {
                    Id = 1,
                    Name = ".NET Blog",
                    Posts =
                    {
                        new Post
                        {
                            Id = 1,
                            Title = "Announcing the Release of EF Core 5.0",
                            Content = "Announcing the release of EF Core 5.0, a full featured cross-platform..."
                        },
                        new Post
                        {
                            Id = 2,
                            Title = "Announcing F# 5",
                            Content = "F# 5 is the latest version of F#, the functional programming language..."
                        }
                    }
                });
-->
[!code-csharp[Attaching_existing_entities_2](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysSamples.cs?name=Attaching_existing_entities_2)]

<span data-ttu-id="aa541-179">Теперь контекст отслеживает все эти сущности следующим образом `Unchanged` :</span><span class="sxs-lookup"><span data-stu-id="aa541-179">The context is now tracking all these entities as `Unchanged`:</span></span>

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}]
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

<span data-ttu-id="aa541-180">Вызов метода SaveChanges на этом этапе не будет действовать.</span><span class="sxs-lookup"><span data-stu-id="aa541-180">Calling SaveChanges at this point will have no effect.</span></span> <span data-ttu-id="aa541-181">Все сущности помечены как `Unchanged` , поэтому в базе данных нет никаких обновлений.</span><span class="sxs-lookup"><span data-stu-id="aa541-181">All the entities are marked as `Unchanged`, so there is nothing to update in the database.</span></span>

### <a name="generated-key-values"></a><span data-ttu-id="aa541-182">Созданные значения ключа</span><span class="sxs-lookup"><span data-stu-id="aa541-182">Generated key values</span></span>

<span data-ttu-id="aa541-183">Как упоминалось выше, [Свойства ключа](xref:core/modeling/keys) Integer и GUID настраиваются для использования [автоматически создаваемых значений ключа](xref:core/modeling/generated-properties) по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="aa541-183">As mentioned above, integer and GUID [key properties](xref:core/modeling/keys) are configured to use [automatically generated key values](xref:core/modeling/generated-properties) by default.</span></span> <span data-ttu-id="aa541-184">Это имеет существенное преимущество при работе с отключенными сущностями: значение неопределенного ключа указывает, что сущность еще не вставлена в базу данных.</span><span class="sxs-lookup"><span data-stu-id="aa541-184">This has a major advantage when working with disconnected entities: an unset key value indicates that the entity has not yet been inserted into the database.</span></span> <span data-ttu-id="aa541-185">Это позволяет средству отслеживания изменений автоматически обнаруживать новые сущности и размещать их в `Added` состоянии.</span><span class="sxs-lookup"><span data-stu-id="aa541-185">This allows the change tracker to automatically detect new entities and put them in the `Added` state.</span></span> <span data-ttu-id="aa541-186">Например, можно присоединить этот график блога и записей:</span><span class="sxs-lookup"><span data-stu-id="aa541-186">For example, consider attaching this graph of a blog and posts:</span></span>

<!--
            context.Attach(
                new Blog
                {
                    Id = 1,
                    Name = ".NET Blog",
                    Posts =
                    {
                        new Post
                        {
                            Id = 1,
                            Title = "Announcing the Release of EF Core 5.0",
                            Content = "Announcing the release of EF Core 5.0, a full featured cross-platform..."
                        },
                        new Post
                        {
                            Id = 2,
                            Title = "Announcing F# 5",
                            Content = "F# 5 is the latest version of F#, the functional programming language..."
                        },
                        new Post
                        {
                            Title = "Announcing .NET 5.0",
                            Content = ".NET 5.0 includes many enhancements, including single file applications, more..."
                        },
                    }
                });
-->
[!code-csharp[Attaching_existing_entities_3](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/GeneratedKeysSamples.cs?name=Attaching_existing_entities_3)]

<span data-ttu-id="aa541-187">В блоге имеется значение ключа 1, указывающее, что он уже существует в базе данных.</span><span class="sxs-lookup"><span data-stu-id="aa541-187">The blog has a key value of 1, indicating that it already exists in the database.</span></span> <span data-ttu-id="aa541-188">В двух записях также заданы ключевые значения, но третья — нет.</span><span class="sxs-lookup"><span data-stu-id="aa541-188">Two of the posts also have key values set, but the third does not.</span></span> <span data-ttu-id="aa541-189">EF Core увидите это значение ключа как 0, значение CLR по умолчанию для целого числа.</span><span class="sxs-lookup"><span data-stu-id="aa541-189">EF Core will see this key value as 0, the CLR default for an integer.</span></span> <span data-ttu-id="aa541-190">Это приводит к EF Core пометке новой сущности `Added` вместо `Unchanged` :</span><span class="sxs-lookup"><span data-stu-id="aa541-190">This results in EF Core marking the new entity as `Added` instead of `Unchanged`:</span></span>

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}, {Id: -2147482636}]
Post {Id: -2147482636} Added
  Id: -2147482636 PK Temporary
  BlogId: 1 FK
  Content: '.NET 5.0 includes many enhancements, including single file a...'
  Title: 'Announcing .NET 5.0'
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
```

<span data-ttu-id="aa541-191">Вызов метода SaveChanges в этом этапе не выполняет никаких действий с `Unchanged` сущностями, но вставляет новую сущность в базу данных.</span><span class="sxs-lookup"><span data-stu-id="aa541-191">Calling SaveChanges at this point does nothing with the `Unchanged` entities, but inserts the new entity into the database.</span></span> <span data-ttu-id="aa541-192">Например, при использовании SQLite:</span><span class="sxs-lookup"><span data-stu-id="aa541-192">For example, when using SQLite:</span></span>

```sql
-- Executed DbCommand (0ms) [Parameters=[@p0='1' (DbType = String), @p1='.NET 5.0 includes many enhancements, including single file applications, more...' (Size = 80), @p2='Announcing .NET 5.0' (Size = 19)], CommandType='Text', CommandTimeout='30']
INSERT INTO "Posts" ("BlogId", "Content", "Title")
VALUES (@p0, @p1, @p2);
SELECT "Id"
FROM "Posts"
WHERE changes() = 1 AND "rowid" = last_insert_rowid();
```

<span data-ttu-id="aa541-193">Важно отметить, что с помощью созданных значений ключа EF Core может **автоматически отличать новый от существующих сущностей в отключенном графе**.</span><span class="sxs-lookup"><span data-stu-id="aa541-193">The important point to notice here is that, with generated key values, EF Core is able to **automatically distinguish new from existing entities in a disconnected graph**.</span></span> <span data-ttu-id="aa541-194">В двух словах, при использовании созданных ключей EF Core всегда будет вставлять сущность, если в этой сущности не задано значение ключа.</span><span class="sxs-lookup"><span data-stu-id="aa541-194">In a nutshell, when using generated keys, EF Core will always insert an entity when that entity has no key value set.</span></span>

## <a name="updating-existing-entities"></a><span data-ttu-id="aa541-195">Обновление существующих сущностей</span><span class="sxs-lookup"><span data-stu-id="aa541-195">Updating existing entities</span></span>

### <a name="explicit-key-values"></a><span data-ttu-id="aa541-196">Явные значения ключа</span><span class="sxs-lookup"><span data-stu-id="aa541-196">Explicit key values</span></span>

<span data-ttu-id="aa541-197"><xref:Microsoft.EntityFrameworkCore.DbContext.Update%2A?displayProperty=nameWithType>, <xref:Microsoft.EntityFrameworkCore.DbContext.UpdateRange%2A?displayProperty=nameWithType> и эквивалентные методы <xref:Microsoft.EntityFrameworkCore.DbSet%601> ведут себя точно так же `Attach` , как описанные выше методы, за исключением того, что сущности помещаются в, `Modfied` а не в `Unchanged` состояние.</span><span class="sxs-lookup"><span data-stu-id="aa541-197"><xref:Microsoft.EntityFrameworkCore.DbContext.Update%2A?displayProperty=nameWithType>, <xref:Microsoft.EntityFrameworkCore.DbContext.UpdateRange%2A?displayProperty=nameWithType>, and the equivalent methods on <xref:Microsoft.EntityFrameworkCore.DbSet%601> behave exactly as the `Attach` methods described above, except that entities are put into the `Modfied` instead of the `Unchanged` state.</span></span> <span data-ttu-id="aa541-198">Например, чтобы начать отслеживание существующего блога `Modified` :</span><span class="sxs-lookup"><span data-stu-id="aa541-198">For example, to start tracking an existing blog as `Modified`:</span></span>

<!--
            context.Update(
                new Blog
                {
                    Id = 1,
                    Name = ".NET Blog",
                });
-->
[!code-csharp[Updating_existing_entities_1](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysSamples.cs?name=Updating_existing_entities_1)]

<span data-ttu-id="aa541-199">Проверка [представления отладки отслеживания изменений](xref:core/change-tracking/debug-views) после этого вызова показывает, что контекст отслеживает эту сущность в `Modified` состоянии:</span><span class="sxs-lookup"><span data-stu-id="aa541-199">Inspecting the [change tracker debug view](xref:core/change-tracking/debug-views) following this call shows that the context is tracking this entity in the `Modified` state:</span></span>

```output
Blog {Id: 1} Modified
  Id: 1 PK
  Name: '.NET Blog' Modified
  Posts: []
```

<span data-ttu-id="aa541-200">Как и в `Add` `Attach` , `Update` фактически помечает _весь граф_ связанных сущностей как `Modified` .</span><span class="sxs-lookup"><span data-stu-id="aa541-200">Just like with `Add` and `Attach`, `Update` actually marks an _entire graph_ of related entities as `Modified`.</span></span> <span data-ttu-id="aa541-201">Например, чтобы присоединить существующий блог и связать его с существующими записями как `Modified` :</span><span class="sxs-lookup"><span data-stu-id="aa541-201">For example, to attach an existing blog and associated existing posts as `Modified`:</span></span>

<!--
            context.Update(
                new Blog
                {
                    Id = 1,
                    Name = ".NET Blog",
                    Posts =
                    {
                        new Post
                        {
                            Id = 1,
                            Title = "Announcing the Release of EF Core 5.0",
                            Content = "Announcing the release of EF Core 5.0, a full featured cross-platform..."
                        },
                        new Post
                        {
                            Id = 2,
                            Title = "Announcing F# 5",
                            Content = "F# 5 is the latest version of F#, the functional programming language..."
                        }
                    }
                });
-->
[!code-csharp[Updating_existing_entities_2](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysSamples.cs?name=Updating_existing_entities_2)]

<span data-ttu-id="aa541-202">Теперь контекст отслеживает все эти сущности следующим образом `Modified` :</span><span class="sxs-lookup"><span data-stu-id="aa541-202">The context is now tracking all these entities as `Modified`:</span></span>

```output
Blog {Id: 1} Modified
  Id: 1 PK
  Name: '.NET Blog' Modified
  Posts: [{Id: 1}, {Id: 2}]
Post {Id: 1} Modified
  Id: 1 PK
  BlogId: 1 FK Modified Originally <null>
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...' Modified
  Title: 'Announcing the Release of EF Core 5.0' Modified
  Blog: {Id: 1}
Post {Id: 2} Modified
  Id: 2 PK
  BlogId: 1 FK Modified Originally <null>
  Content: 'F# 5 is the latest version of F#, the functional programming...' Modified
  Title: 'Announcing F# 5' Modified
  Blog: {Id: 1}
```

<span data-ttu-id="aa541-203">Вызов метода SaveChanges на этом этапе приведет к тому, что обновления будут отправляться в базу данных для всех этих сущностей.</span><span class="sxs-lookup"><span data-stu-id="aa541-203">Calling SaveChanges at this point will cause updates to be sent to the database for all these entities.</span></span> <span data-ttu-id="aa541-204">Например, при использовании SQLite:</span><span class="sxs-lookup"><span data-stu-id="aa541-204">For example, when using SQLite:</span></span>

```sql
-- Executed DbCommand (0ms) [Parameters=[@p1='1' (DbType = String), @p0='.NET Blog' (Size = 9)], CommandType='Text', CommandTimeout='30']
UPDATE "Blogs" SET "Name" = @p0
WHERE "Id" = @p1;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p3='1' (DbType = String), @p0='1' (DbType = String), @p1='Announcing the release of EF Core 5.0, a full featured cross-platform...' (Size = 72), @p2='Announcing the Release of EF Core 5.0' (Size = 37)], CommandType='Text', CommandTimeout='30']
UPDATE "Posts" SET "BlogId" = @p0, "Content" = @p1, "Title" = @p2
WHERE "Id" = @p3;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p3='2' (DbType = String), @p0='1' (DbType = String), @p1='F# 5 is the latest version of F#, the functional programming language...' (Size = 72), @p2='Announcing F# 5' (Size = 15)], CommandType='Text', CommandTimeout='30']
UPDATE "Posts" SET "BlogId" = @p0, "Content" = @p1, "Title" = @p2
WHERE "Id" = @p3;
SELECT changes();
```

### <a name="generated-key-values"></a><span data-ttu-id="aa541-205">Созданные значения ключа</span><span class="sxs-lookup"><span data-stu-id="aa541-205">Generated key values</span></span>

<span data-ttu-id="aa541-206">Как и в `Attach` случае, созданные значения ключей имеют одинаковое основное преимущество `Update` : значение неопределенного ключа указывает, что сущность является новой и еще не вставлена в базу данных.</span><span class="sxs-lookup"><span data-stu-id="aa541-206">As with `Attach`, generated key values have the same major benefit for `Update`: an unset key value indicates that the entity is new and has not yet been inserted into the database.</span></span> <span data-ttu-id="aa541-207">Как и в `Attach` случае, DbContext позволяет автоматически обнаруживать новые сущности и размещать их в `Added` состоянии.</span><span class="sxs-lookup"><span data-stu-id="aa541-207">As with `Attach`, this allows the DbContext to automatically detect new entities and put them in the `Added` state.</span></span> <span data-ttu-id="aa541-208">Например, рассмотрим вызов `Update` с помощью этого графа блога и записей:</span><span class="sxs-lookup"><span data-stu-id="aa541-208">For example, consider calling `Update` with this graph of a blog and posts:</span></span>

<!--
            context.Update(
                new Blog
                {
                    Id = 1,
                    Name = ".NET Blog",
                    Posts =
                    {
                        new Post
                        {
                            Id = 1,
                            Title = "Announcing the Release of EF Core 5.0",
                            Content = "Announcing the release of EF Core 5.0, a full featured cross-platform..."
                        },
                        new Post
                        {
                            Id = 2,
                            Title = "Announcing F# 5",
                            Content = "F# 5 is the latest version of F#, the functional programming language..."
                        }
                    }
                });
-->
[!code-csharp[Updating_existing_entities_3](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/GeneratedKeysSamples.cs?name=Updating_existing_entities_3)]

<span data-ttu-id="aa541-209">Как и в `Attach` примере, POST со значением ключа не обнаруживается как New и устанавливается в `Added` состояние.</span><span class="sxs-lookup"><span data-stu-id="aa541-209">As with the `Attach` example, the post with no key value is detected as new and set to the `Added` state.</span></span> <span data-ttu-id="aa541-210">Другие сущности помечаются как `Modified` :</span><span class="sxs-lookup"><span data-stu-id="aa541-210">The other entities are marked as `Modified`:</span></span>

```output
Blog {Id: 1} Modified
  Id: 1 PK
  Name: '.NET Blog' Modified
  Posts: [{Id: 1}, {Id: 2}, {Id: -2147482633}]
Post {Id: -2147482633} Added
  Id: -2147482633 PK Temporary
  BlogId: 1 FK
  Content: '.NET 5.0 includes many enhancements, including single file a...'
  Title: 'Announcing .NET 5.0'
  Blog: {Id: 1}
Post {Id: 1} Modified
  Id: 1 PK
  BlogId: 1 FK Modified Originally <null>
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...' Modified
  Title: 'Announcing the Release of EF Core 5.0' Modified
  Blog: {Id: 1}
Post {Id: 2} Modified
  Id: 2 PK
  BlogId: 1 FK Modified Originally <null>
  Content: 'F# 5 is the latest version of F#, the functional programming...' Modified
  Title: 'Announcing F# 5' Modified
  Blog: {Id: 1}
```

<span data-ttu-id="aa541-211">Вызов `SaveChanges` в этой точке приведет к тому, что обновления будут отправляться в базу данных для всех существующих сущностей, а новая сущность будет вставлена.</span><span class="sxs-lookup"><span data-stu-id="aa541-211">Calling `SaveChanges` at this point will cause updates to be sent to the database for all the existing entities, while the new entity is inserted.</span></span> <span data-ttu-id="aa541-212">Например, при использовании SQLite:</span><span class="sxs-lookup"><span data-stu-id="aa541-212">For example, when using SQLite:</span></span>

```sql
-- Executed DbCommand (0ms) [Parameters=[@p1='1' (DbType = String), @p0='.NET Blog' (Size = 9)], CommandType='Text', CommandTimeout='30']
UPDATE "Blogs" SET "Name" = @p0
WHERE "Id" = @p1;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p3='1' (DbType = String), @p0='1' (DbType = String), @p1='Announcing the release of EF Core 5.0, a full featured cross-platform...' (Size = 72), @p2='Announcing the Release of EF Core 5.0' (Size = 37)], CommandType='Text', CommandTimeout='30']
UPDATE "Posts" SET "BlogId" = @p0, "Content" = @p1, "Title" = @p2
WHERE "Id" = @p3;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p3='2' (DbType = String), @p0='1' (DbType = String), @p1='F# 5 is the latest version of F#, the functional programming language...' (Size = 72), @p2='Announcing F# 5' (Size = 15)], CommandType='Text', CommandTimeout='30']
UPDATE "Posts" SET "BlogId" = @p0, "Content" = @p1, "Title" = @p2
WHERE "Id" = @p3;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p0='1' (DbType = String), @p1='.NET 5.0 includes many enhancements, including single file applications, more...' (Size = 80), @p2='Announcing .NET 5.0' (Size = 19)], CommandType='Text', CommandTimeout='30']
INSERT INTO "Posts" ("BlogId", "Content", "Title")
VALUES (@p0, @p1, @p2);
SELECT "Id"
FROM "Posts"
WHERE changes() = 1 AND "rowid" = last_insert_rowid();
```

<span data-ttu-id="aa541-213">Это очень простой способ создания обновлений и вставок из отключенного графа.</span><span class="sxs-lookup"><span data-stu-id="aa541-213">This is a very easy way to generate updates and inserts from a disconnected graph.</span></span> <span data-ttu-id="aa541-214">Однако это приводит к отправке обновлений или вставок в базу данных для каждого свойства отслеживаемой сущности, даже если некоторые значения свойств могут быть не изменены.</span><span class="sxs-lookup"><span data-stu-id="aa541-214">However, it results in updates or inserts being sent to the database for every property of every tracked entity, even when some property values may not have been changed.</span></span> <span data-ttu-id="aa541-215">Не страх. для многих приложений с небольшими графами это может быть простой и практичный способ создания обновлений.</span><span class="sxs-lookup"><span data-stu-id="aa541-215">Don't be too scared by this; for many applications with small graphs, this can be an easy and pragmatic way of generating updates.</span></span> <span data-ttu-id="aa541-216">С другой стороны, другие более сложные шаблоны могут привести к более эффективным обновлениям, как описано в разделе [разрешение идентификации в EF Core](xref:core/change-tracking/identity-resolution).</span><span class="sxs-lookup"><span data-stu-id="aa541-216">That being said, other more complex patterns can sometimes result in more efficient updates, as described in [Identity Resolution in EF Core](xref:core/change-tracking/identity-resolution).</span></span>

## <a name="deleting-existing-entities"></a><span data-ttu-id="aa541-217">Удаление существующих сущностей</span><span class="sxs-lookup"><span data-stu-id="aa541-217">Deleting existing entities</span></span>

<span data-ttu-id="aa541-218">Чтобы сущность, удаляемая методом SaveChanges, была удалена, она должна быть записана в `Deleted` состояние.</span><span class="sxs-lookup"><span data-stu-id="aa541-218">For an entity to be deleted by SaveChanges it must be tracked in the `Deleted` state.</span></span> <span data-ttu-id="aa541-219">Сущности обычно помещаются в `Deleted` состояние путем вызова одного из <xref:Microsoft.EntityFrameworkCore.DbContext.Remove%2A?displayProperty=nameWithType> , <xref:Microsoft.EntityFrameworkCore.DbContext.RemoveRange%2A?displayProperty=nameWithType> или эквивалентных методов в <xref:Microsoft.EntityFrameworkCore.DbSet%601> .</span><span class="sxs-lookup"><span data-stu-id="aa541-219">Entities are typically put in the `Deleted` state by calling one of <xref:Microsoft.EntityFrameworkCore.DbContext.Remove%2A?displayProperty=nameWithType>, <xref:Microsoft.EntityFrameworkCore.DbContext.RemoveRange%2A?displayProperty=nameWithType>, or the equivalent methods on <xref:Microsoft.EntityFrameworkCore.DbSet%601>.</span></span> <span data-ttu-id="aa541-220">Например, чтобы пометить существующий объект Post как `Deleted` :</span><span class="sxs-lookup"><span data-stu-id="aa541-220">For example, to mark an existing post as `Deleted`:</span></span>

<!--
            context.Remove(
                new Post
                {
                    Id = 2
                });
-->
[!code-csharp[Deleting_existing_entities_1](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysSamples.cs?name=Deleting_existing_entities_1)]

<span data-ttu-id="aa541-221">Проверка [представления отладки отслеживания изменений](xref:core/change-tracking/debug-views) после этого вызова показывает, что контекст отслеживает сущность в `Deleted` состоянии:</span><span class="sxs-lookup"><span data-stu-id="aa541-221">Inspecting the [change tracker debug view](xref:core/change-tracking/debug-views) following this call shows that the context is tracking the entity in the `Deleted` state:</span></span>

```output
Post {Id: 2} Deleted
  Id: 2 PK
  BlogId: <null> FK
  Content: <null>
  Title: <null>
  Blog: <null>
```

<span data-ttu-id="aa541-222">Эта сущность будет удалена при вызове SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="aa541-222">This entity will be deleted when SaveChanges is called.</span></span> <span data-ttu-id="aa541-223">Например, при использовании SQLite:</span><span class="sxs-lookup"><span data-stu-id="aa541-223">For example, when using SQLite:</span></span>

```sql
-- Executed DbCommand (0ms) [Parameters=[@p0='2' (DbType = String)], CommandType='Text', CommandTimeout='30']
DELETE FROM "Posts"
WHERE "Id" = @p0;
SELECT changes();
```

<span data-ttu-id="aa541-224">После завершения SaveChanges Удаленная сущность отсоединяется от DbContext, так как она больше не существует в базе данных.</span><span class="sxs-lookup"><span data-stu-id="aa541-224">After SaveChanges completes, the deleted entity is detached from the DbContext since it no longer exists in the database.</span></span> <span data-ttu-id="aa541-225">Поэтому представление отладки пусто, так как нет отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="aa541-225">The debug view is therefore empty because no entities are being tracked.</span></span>

## <a name="deleting-dependentchild-entities"></a><span data-ttu-id="aa541-226">Удаление зависимых или дочерних сущностей</span><span class="sxs-lookup"><span data-stu-id="aa541-226">Deleting dependent/child entities</span></span>

<span data-ttu-id="aa541-227">Удаление зависимых и дочерних сущностей из графа является более простым, чем удаление основных и родительских сущностей.</span><span class="sxs-lookup"><span data-stu-id="aa541-227">Deleting dependent/child entities from a graph is more straightforward than deleting principal/parent entities.</span></span> <span data-ttu-id="aa541-228">Дополнительные сведения см. в следующем разделе и об [изменении внешних ключей и переходов](xref:core/change-tracking/relationship-changes) .</span><span class="sxs-lookup"><span data-stu-id="aa541-228">See the next section and [Changing Foreign Keys and Navigations](xref:core/change-tracking/relationship-changes) for more information.</span></span>

<span data-ttu-id="aa541-229">Вызывать `Remove` для сущности, созданной с помощью, нередко `new` .</span><span class="sxs-lookup"><span data-stu-id="aa541-229">It is unusual to call `Remove` on an entity created with `new`.</span></span> <span data-ttu-id="aa541-230">Кроме того, в отличие от `Add` `Attach` и `Update` , вызов `Remove` для сущности, которая еще не была зарегистрирована в состоянии или, нераспространена `Unchanged` `Modified` .</span><span class="sxs-lookup"><span data-stu-id="aa541-230">Further, unlike `Add`, `Attach` and `Update`, it is uncommon to call `Remove` on an entity that isn't already tracked in the `Unchanged` or `Modified` state.</span></span> <span data-ttu-id="aa541-231">Вместо этого обычно используется отслеживание отдельной сущности или графа связанных сущностей, а затем вызов `Remove` для сущностей, которые необходимо удалить.</span><span class="sxs-lookup"><span data-stu-id="aa541-231">Instead it is typical to track a single entity or graph of related entities, and then call `Remove` on the entities that should be deleted.</span></span> <span data-ttu-id="aa541-232">Этот граф отслеживания сущностей обычно создается одним из следующих:</span><span class="sxs-lookup"><span data-stu-id="aa541-232">This graph of tracked entities is typically created by either:</span></span>

1. <span data-ttu-id="aa541-233">Выполнение запроса для сущностей</span><span class="sxs-lookup"><span data-stu-id="aa541-233">Running a query for the entities</span></span>
2. <span data-ttu-id="aa541-234">Использование `Attach` методов или `Update` на графе отключенных сущностей, как описано в предыдущих разделах.</span><span class="sxs-lookup"><span data-stu-id="aa541-234">Using the `Attach` or `Update` methods on a graph of disconnected entities, as described in the preceding sections.</span></span>

<span data-ttu-id="aa541-235">Например, код в предыдущем разделе более вероятно получает сообщение от клиента, а затем делает что-то подобное:</span><span class="sxs-lookup"><span data-stu-id="aa541-235">For example, the code in the previous section is more likely obtain a post from a client and then do something like this:</span></span>

<!--
            context.Attach(post);
            context.Remove(post);
-->
[!code-csharp[Deleting_dependent_child_entities_1](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysSamples.cs?name=Deleting_dependent_child_entities_1)]

<span data-ttu-id="aa541-236">Это ведет себя точно так же, как в предыдущем примере, поскольку вызов `Remove` для неотслеживающей сущности приводит к тому, что он сначала присоединяется, а затем помечается как `Deleted` .</span><span class="sxs-lookup"><span data-stu-id="aa541-236">This behaves exactly the same way as the previous example, since calling `Remove` on an un-tracked entity causes it to first be attached and then marked as `Deleted`.</span></span>

<span data-ttu-id="aa541-237">В более реалистичных примерах сначала присоединяется граф сущностей, а затем некоторые из этих сущностей помечаются как удаленные.</span><span class="sxs-lookup"><span data-stu-id="aa541-237">In more realistic examples, a graph of entities is first attached, and then some of those entities are marked as deleted.</span></span> <span data-ttu-id="aa541-238">Пример:</span><span class="sxs-lookup"><span data-stu-id="aa541-238">For example:</span></span>

<!--
            // Attach a blog and associated posts
            context.Attach(blog);

            // Mark one post as Deleted
            context.Remove(blog.Posts[1]);
-->
[!code-csharp[Deleting_dependent_child_entities_2](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysSamples.cs?name=Deleting_dependent_child_entities_2)]

<span data-ttu-id="aa541-239">Все сущности помечены как `Unchanged` , за исключением того, в котором `Remove` был вызван:</span><span class="sxs-lookup"><span data-stu-id="aa541-239">All entities are marked as `Unchanged`, except the one on which `Remove` was called:</span></span>

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}]
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

<span data-ttu-id="aa541-240">Эта сущность будет удалена при вызове SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="aa541-240">This entity will be deleted when SaveChanges is called.</span></span> <span data-ttu-id="aa541-241">Например, при использовании SQLite:</span><span class="sxs-lookup"><span data-stu-id="aa541-241">For example, when using SQLite:</span></span>

```sql
-- Executed DbCommand (0ms) [Parameters=[@p0='2' (DbType = String)], CommandType='Text', CommandTimeout='30']
DELETE FROM "Posts"
WHERE "Id" = @p0;
SELECT changes();
```

<span data-ttu-id="aa541-242">После завершения SaveChanges Удаленная сущность отсоединяется от DbContext, так как она больше не существует в базе данных.</span><span class="sxs-lookup"><span data-stu-id="aa541-242">After SaveChanges completes, the deleted entity is detached from the DbContext since it no longer exists in the database.</span></span> <span data-ttu-id="aa541-243">Другие сущности остаются в `Unchanged` состоянии:</span><span class="sxs-lookup"><span data-stu-id="aa541-243">Other entities remain in the `Unchanged` state:</span></span>

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Posts: [{Id: 1}]
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
```

## <a name="deleting-principalparent-entities"></a><span data-ttu-id="aa541-244">Удаление субъектов или родительских сущностей</span><span class="sxs-lookup"><span data-stu-id="aa541-244">Deleting principal/parent entities</span></span>

<span data-ttu-id="aa541-245">Каждое отношение, соединяющее два типа сущностей, имеет основной или родительский элемент, а также зависимый или дочерний элемент.</span><span class="sxs-lookup"><span data-stu-id="aa541-245">Each relationship that connects two entity types has a principal or parent end, and a dependent or child end.</span></span> <span data-ttu-id="aa541-246">Зависимая или Дочерняя сущность — это свойство внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="aa541-246">The dependent/child entity is the one with the foreign key property.</span></span> <span data-ttu-id="aa541-247">В связи «один ко многим» основной или родительский элемент находится на стороне «один», а зависимый/дочерний элемент находится на стороне «многие».</span><span class="sxs-lookup"><span data-stu-id="aa541-247">In a one-to-many relationship, the principal/parent is on the "one" side, and the dependent/child is on the "many" side.</span></span> <span data-ttu-id="aa541-248">Дополнительные сведения см. в разделе [связи](xref:core/modeling/relationships) .</span><span class="sxs-lookup"><span data-stu-id="aa541-248">See [Relationships](xref:core/modeling/relationships) for more information.</span></span>

<span data-ttu-id="aa541-249">В предыдущих примерах мы выполняли удаление записи, которая является зависимой или дочерней сущностью в блоге — публикует связь «один ко многим».</span><span class="sxs-lookup"><span data-stu-id="aa541-249">In the preceding examples we were deleting a post, which is a dependent/child entity in the blog-posts one-to-many relationship.</span></span> <span data-ttu-id="aa541-250">Это относительно простая задача, так как удаление зависимой или дочерней сущности не влияет на другие сущности.</span><span class="sxs-lookup"><span data-stu-id="aa541-250">This is relatively straightforward since removal of a dependent/child entity does not have any impact on other entities.</span></span> <span data-ttu-id="aa541-251">С другой стороны, удаление основной или родительской сущности также должно повлиять на все зависимые и дочерние сущности.</span><span class="sxs-lookup"><span data-stu-id="aa541-251">On the other hand, deleting a principal/parent entity must also impact any dependent/child entities.</span></span> <span data-ttu-id="aa541-252">В противном случае значение внешнего ключа будет ссылаться на значение первичного ключа, которое больше не существует.</span><span class="sxs-lookup"><span data-stu-id="aa541-252">Not doing so would leave a foreign key value referencing a primary key value that no longer exists.</span></span> <span data-ttu-id="aa541-253">Это недопустимое состояние модели, которое приводит к ошибке ссылочного ограничения в большинстве баз данных.</span><span class="sxs-lookup"><span data-stu-id="aa541-253">This is an invalid model state and results in a referential constraint error in most databases.</span></span>

<span data-ttu-id="aa541-254">Это недопустимое состояние модели может быть обработано двумя способами:</span><span class="sxs-lookup"><span data-stu-id="aa541-254">This invalid model state can be handled in two ways:</span></span>

1. <span data-ttu-id="aa541-255">Установка значения FK в значение null.</span><span class="sxs-lookup"><span data-stu-id="aa541-255">Setting FK values to null.</span></span> <span data-ttu-id="aa541-256">Это означает, что зависимые и дочерние элементы больше не связаны ни с одним участником/родителем.</span><span class="sxs-lookup"><span data-stu-id="aa541-256">This indicates that the dependents/children are no longer related to any principal/parent.</span></span> <span data-ttu-id="aa541-257">Это значение по умолчанию для необязательных связей, в которых внешний ключ должен допускать значение null.</span><span class="sxs-lookup"><span data-stu-id="aa541-257">This is the default for optional relationships where the foreign key must be nullable.</span></span> <span data-ttu-id="aa541-258">Установка FK в NULL недопустима для обязательных связей, где внешний ключ обычно не допускает значения NULL.</span><span class="sxs-lookup"><span data-stu-id="aa541-258">Setting the FK to null is not valid for required relationships, where the foreign key is typically non-nullable.</span></span>
2. <span data-ttu-id="aa541-259">Удаление зависимых объектов и дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="aa541-259">Deleting the the dependents/children.</span></span> <span data-ttu-id="aa541-260">Это значение по умолчанию для обязательных связей, которое также допустимо для необязательных связей.</span><span class="sxs-lookup"><span data-stu-id="aa541-260">This is the default for required relationships, and is also valid for optional relationships.</span></span>

<span data-ttu-id="aa541-261">Подробные сведения об отслеживании изменений и связях см. в разделе [изменение внешних ключей и переходов](xref:core/change-tracking/relationship-changes) .</span><span class="sxs-lookup"><span data-stu-id="aa541-261">See [Changing Foreign Keys and Navigations](xref:core/change-tracking/relationship-changes) for detailed information on change tracking and relationships.</span></span>

### <a name="optional-relationships"></a><span data-ttu-id="aa541-262">Необязательные связи</span><span class="sxs-lookup"><span data-stu-id="aa541-262">Optional relationships</span></span>

<span data-ttu-id="aa541-263">`Post.BlogId`Свойство внешнего ключа допускает значение NULL в модели, которую мы использовали.</span><span class="sxs-lookup"><span data-stu-id="aa541-263">The `Post.BlogId` foreign key property is nullable in the model we have been using.</span></span> <span data-ttu-id="aa541-264">Это означает, что связь является необязательной, поэтому поведением по умолчанию EF Core является установка `BlogId` свойств внешнего ключа со значением NULL при удалении блога.</span><span class="sxs-lookup"><span data-stu-id="aa541-264">This means the relationship is optional, and hence the default behavior of EF Core is to set `BlogId` foreign key properties to null when the blog is deleted.</span></span> <span data-ttu-id="aa541-265">Пример:</span><span class="sxs-lookup"><span data-stu-id="aa541-265">For example:</span></span>

<!--
            // Attach a blog and associated posts
            context.Attach(blog);

            // Mark one post as Deleted
            context.Remove(blog.Posts[1]);
-->
[!code-csharp[Deleting_principal_parent_entities_1](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysSamples.cs?name=Deleting_principal_parent_entities_1)]

<span data-ttu-id="aa541-266">Проверка [представления отладки отслеживания изменений](xref:core/change-tracking/debug-views) после вызова `Remove` показывает, что, как и ожидалось, блог теперь помечен как `Deleted` :</span><span class="sxs-lookup"><span data-stu-id="aa541-266">Inspecting the [change tracker debug view](xref:core/change-tracking/debug-views) following the call to `Remove` shows that, as expected, the blog is now marked as `Deleted`:</span></span>

```output
Blog {Id: 1} Deleted
  Id: 1 PK
  Name: '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}]
Post {Id: 1} Modified
  Id: 1 PK
  BlogId: <null> FK Modified Originally 1
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: <null>
Post {Id: 2} Modified
  Id: 2 PK
  BlogId: <null> FK Modified Originally 1
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: <null>
```

<span data-ttu-id="aa541-267">Более интересно, что все связанные записи теперь помечены как `Modified` .</span><span class="sxs-lookup"><span data-stu-id="aa541-267">More interestingly, all the related posts are now marked as `Modified`.</span></span> <span data-ttu-id="aa541-268">Это связано с тем, что свойство внешнего ключа в каждой сущности имеет значение null.</span><span class="sxs-lookup"><span data-stu-id="aa541-268">This is because the foreign key property in each entity has been set to null.</span></span> <span data-ttu-id="aa541-269">Вызов метода SaveChanges обновляет значение внешнего ключа для каждой записи в базе данных до значения NULL, а затем удаляет блог:</span><span class="sxs-lookup"><span data-stu-id="aa541-269">Calling SaveChanges updates the foreign key value for each post to null in the database, before then deleting the blog:</span></span>

```sql
-- Executed DbCommand (0ms) [Parameters=[@p1='1' (DbType = String), @p0=NULL], CommandType='Text', CommandTimeout='30']
UPDATE "Posts" SET "BlogId" = @p0
WHERE "Id" = @p1;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p1='2' (DbType = String), @p0=NULL], CommandType='Text', CommandTimeout='30']
UPDATE "Posts" SET "BlogId" = @p0
WHERE "Id" = @p1;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p2='1' (DbType = String)], CommandType='Text', CommandTimeout='30']
DELETE FROM "Blogs"
WHERE "Id" = @p2;
SELECT changes();
```

<span data-ttu-id="aa541-270">После завершения SaveChanges Удаленная сущность отсоединяется от DbContext, так как она больше не существует в базе данных.</span><span class="sxs-lookup"><span data-stu-id="aa541-270">After SaveChanges completes, the deleted entity is detached from the DbContext since it no longer exists in the database.</span></span> <span data-ttu-id="aa541-271">Другие сущности теперь помечены `Unchanged` значениями внешнего ключа NULL, которые соответствуют состоянию базы данных:</span><span class="sxs-lookup"><span data-stu-id="aa541-271">Other entities are now marked as `Unchanged` with null foreign key values, which matches the state of the database:</span></span>

```output
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: <null> FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: <null>
Post {Id: 2} Unchanged
  Id: 2 PK
  BlogId: <null> FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: <null>
```

### <a name="required-relationships"></a><span data-ttu-id="aa541-272">Обязательные связи</span><span class="sxs-lookup"><span data-stu-id="aa541-272">Required relationships</span></span>

<span data-ttu-id="aa541-273">Если `Post.BlogId` свойство внешнего ключа не допускает значения NULL, то связь между блогами и записями становится "обязательной".</span><span class="sxs-lookup"><span data-stu-id="aa541-273">If the `Post.BlogId` foreign key property is non-nullable, then the relationship between blogs and posts becomes "required".</span></span> <span data-ttu-id="aa541-274">В этом случае EF Core будет по умолчанию удалять зависимые и дочерние сущности при удалении основного или родительского объекта.</span><span class="sxs-lookup"><span data-stu-id="aa541-274">In this situation, EF Core will, by default, delete dependent/child entities when the principal/parent is deleted.</span></span> <span data-ttu-id="aa541-275">Например, при удалении блога со связанными записями, как в предыдущем примере:</span><span class="sxs-lookup"><span data-stu-id="aa541-275">For example, deleting a blog with related posts as in the previous example:</span></span>

<!--
            // Attach a blog and associated posts
            context.Attach(blog);

            // Mark one post as Deleted
            context.Remove(blog.Posts[1]);
-->
[!code-csharp[Deleting_principal_parent_entities_1](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysRequiredSamples.cs?name=Deleting_principal_parent_entities_1)]

<span data-ttu-id="aa541-276">Проверка [представления отладки отслеживания изменений](xref:core/change-tracking/debug-views) после вызова `Remove` показывает, что, как и ожидалось, блог снова помечен как `Deleted` :</span><span class="sxs-lookup"><span data-stu-id="aa541-276">Inspecting the [change tracker debug view](xref:core/change-tracking/debug-views) following the call to `Remove` shows that, as expected, the blog is again marked as `Deleted`:</span></span>

```output
Blog {Id: 1} Deleted
  Id: 1 PK
  Name: '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}]
Post {Id: 1} Deleted
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

<span data-ttu-id="aa541-277">Более интересно в этом случае, так как все связанные записи также помечены как `Deleted` .</span><span class="sxs-lookup"><span data-stu-id="aa541-277">More interestingly in this case is that all related posts have also been marked as `Deleted`.</span></span> <span data-ttu-id="aa541-278">Вызов метода SaveChanges приводит к удалению блога и всех связанных с ним записей из базы данных:</span><span class="sxs-lookup"><span data-stu-id="aa541-278">Calling SaveChanges causes the blog and all related posts to be deleted from the database:</span></span>

```sql
-- Executed DbCommand (0ms) [Parameters=[@p0='1' (DbType = String)], CommandType='Text', CommandTimeout='30']
DELETE FROM "Posts"
WHERE "Id" = @p0;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p0='2' (DbType = String)], CommandType='Text', CommandTimeout='30']
DELETE FROM "Posts"
WHERE "Id" = @p0;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p1='1' (DbType = String)], CommandType='Text', CommandTimeout='30']
DELETE FROM "Blogs"
WHERE "Id" = @p1;
```

<span data-ttu-id="aa541-279">После завершения SaveChanges все удаленные сущности отсоединяются от DbContext, так как они больше не существуют в базе данных.</span><span class="sxs-lookup"><span data-stu-id="aa541-279">After SaveChanges completes, all the deleted entities are detached from the DbContext since they no longer exist in the database.</span></span> <span data-ttu-id="aa541-280">Поэтому выходные данные представления отладка не заполнены.</span><span class="sxs-lookup"><span data-stu-id="aa541-280">Output from the debug view is therefore empty.</span></span>

> [!NOTE]
> <span data-ttu-id="aa541-281">В этом документе только поверхность работы с связями в EF Core.</span><span class="sxs-lookup"><span data-stu-id="aa541-281">This document only scratches the surface on working with relationships in EF Core.</span></span> <span data-ttu-id="aa541-282">Дополнительные сведения об обновлении и удалении зависимых и дочерних сущностей при вызове SaveChanges см. в разделе [связи](xref:core/modeling/relationships) для получения дополнительных сведений о связях моделирования и [изменении внешних ключей и переходов](xref:core/change-tracking/relationship-changes) .</span><span class="sxs-lookup"><span data-stu-id="aa541-282">See [Relationships](xref:core/modeling/relationships) for more information on modeling relationships, and [Changing Foreign Keys and Navigations](xref:core/change-tracking/relationship-changes) for more information on updating/deleting dependent/child entities when calling SaveChanges.</span></span>

## <a name="custom-tracking-with-trackgraph"></a><span data-ttu-id="aa541-283">Настраиваемое отслеживание с помощью Траккграф</span><span class="sxs-lookup"><span data-stu-id="aa541-283">Custom tracking with TrackGraph</span></span>

<span data-ttu-id="aa541-284"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.TrackGraph%2A?displayProperty=nameWithType> работает так же `Add` , как `Attach` и, `Update` за исключением того, что он создает обратный вызов для каждого экземпляра сущности перед его отслеживанием.</span><span class="sxs-lookup"><span data-stu-id="aa541-284"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.TrackGraph%2A?displayProperty=nameWithType> works like `Add`, `Attach` and `Update` except that it generates a callback for every entity instance before tracking it.</span></span> <span data-ttu-id="aa541-285">Это позволяет использовать пользовательскую логику при определении способа трассировки отдельных сущностей в графе.</span><span class="sxs-lookup"><span data-stu-id="aa541-285">This allows custom logic to be used when determining how to track individual entities in a graph.</span></span>

<span data-ttu-id="aa541-286">Например, рассмотрим правило, которое EF Core использует при отслеживании сущностей с созданными значениями ключа: Если значение ключа равно нулю, то сущность является новой и должна быть вставлена.</span><span class="sxs-lookup"><span data-stu-id="aa541-286">For example, consider the rule EF Core uses when tracking entities with generated key values: if the key value is zero, then the entity is new and should be inserted.</span></span> <span data-ttu-id="aa541-287">Добавим это правило, чтобы сказать, что значение ключа отрицательное, сущность следует удалить.</span><span class="sxs-lookup"><span data-stu-id="aa541-287">Let's extend this rule to say if the key value is negative, then the entity should be deleted.</span></span> <span data-ttu-id="aa541-288">Это позволяет нам изменить значения первичного ключа в сущностях отключенного графа, чтобы пометить удаленные сущности:</span><span class="sxs-lookup"><span data-stu-id="aa541-288">This allows us to change the primary key values in entities of a disconnected graph to mark deleted entities:</span></span>

<!--
            blog.Posts.Add(
                new Post
                {
                    Title = "Announcing .NET 5.0",
                    Content = ".NET 5.0 includes many enhancements, including single file applications, more..."
                }
            );

            var toDelete = blog.Posts.Single(e => e.Title == "Announcing F# 5");
            toDelete.Id = -toDelete.Id;
-->
[!code-csharp[Custom_tracking_with_TrackGraph_1a](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/GeneratedKeysSamples.cs?name=Custom_tracking_with_TrackGraph_1a)]

<span data-ttu-id="aa541-289">После этого отключенный граф можно будет отслеживанию с помощью Траккграф:</span><span class="sxs-lookup"><span data-stu-id="aa541-289">This disconnected graph can then be tracked using TrackGraph:</span></span>

<!--
        public static void UpdateBlog(Blog blog)
        {
            using var context = new BlogsContext();

            context.ChangeTracker.TrackGraph(
                blog, node =>
                    {
                        var propertyEntry = node.Entry.Property("Id");
                        var keyValue = (int)propertyEntry.CurrentValue;

                        if (keyValue == 0)
                        {
                            node.Entry.State = EntityState.Added;
                        }
                        else if (keyValue < 0)
                        {
                            propertyEntry.CurrentValue = -keyValue;
                            node.Entry.State = EntityState.Deleted;
                        }
                        else
                        {
                            node.Entry.State = EntityState.Modified;
                        }

                        Console.WriteLine($"Tracking {node.Entry.Metadata.DisplayName()} with key value {keyValue} as {node.Entry.State}");

                    });

            context.SaveChanges();
        }
-->
[!code-csharp[Custom_tracking_with_TrackGraph_1b](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/GeneratedKeysSamples.cs?name=Custom_tracking_with_TrackGraph_1b)]

<span data-ttu-id="aa541-290">Для каждой сущности в графе приведенный выше код проверяет значение первичного ключа _перед отслеживанием сущности_.</span><span class="sxs-lookup"><span data-stu-id="aa541-290">For each entity in the graph, the code above checks the primary key value _before tracking the entity_.</span></span> <span data-ttu-id="aa541-291">Для нестандартных (нулевых) значений ключа код выполняет действия, выполняемые EF Core.</span><span class="sxs-lookup"><span data-stu-id="aa541-291">For unset (zero) key values, the code does what EF Core would normally do.</span></span> <span data-ttu-id="aa541-292">То есть если ключ не задан, сущность помечается как `Added` .</span><span class="sxs-lookup"><span data-stu-id="aa541-292">That is, if the key is not set, then the entity is marked as `Added`.</span></span> <span data-ttu-id="aa541-293">Если ключ задан и значение не отрицательное, то сущность помечается как `Modified` .</span><span class="sxs-lookup"><span data-stu-id="aa541-293">If the key is set and the value is non-negative, then the entity is marked as `Modified`.</span></span> <span data-ttu-id="aa541-294">Однако при обнаружении отрицательного значения ключа восстанавливается его вещественное неотрицательное значение, а сущность — как `Deleted` .</span><span class="sxs-lookup"><span data-stu-id="aa541-294">However, if a negative key value is found, then its real, non-negative value is restored and the entity is tracked as `Deleted`.</span></span>

<span data-ttu-id="aa541-295">Результат выполнения этого кода:</span><span class="sxs-lookup"><span data-stu-id="aa541-295">The output from running this code is:</span></span>

```output
Tracking Blog with key value 1 as Modified
Tracking Post with key value 1 as Modified
Tracking Post with key value -2 as Deleted
Tracking Post with key value 0 as Added
```

> [!NOTE]
> <span data-ttu-id="aa541-296">Для простоты в этом коде предполагается, что каждая сущность имеет целочисленное свойство первичного ключа с именем `Id` .</span><span class="sxs-lookup"><span data-stu-id="aa541-296">For simplicity, this code assumes each entity has an integer primary key property called `Id`.</span></span> <span data-ttu-id="aa541-297">Это можно кодифицированные в абстрактный базовый класс или интерфейс.</span><span class="sxs-lookup"><span data-stu-id="aa541-297">This could be codified into an abstract base class or interface.</span></span> <span data-ttu-id="aa541-298">Кроме того, свойство или свойства первичного ключа можно получить из <xref:Microsoft.EntityFrameworkCore.Metadata.IEntityType> метаданных таким образом, чтобы этот код работал с любым типом сущности.</span><span class="sxs-lookup"><span data-stu-id="aa541-298">Alternately, the primary key property or properties could be obtained from the <xref:Microsoft.EntityFrameworkCore.Metadata.IEntityType> metadata such that this code would work with any type of entity.</span></span>

<span data-ttu-id="aa541-299">Траккграф имеет две перегрузки.</span><span class="sxs-lookup"><span data-stu-id="aa541-299">TrackGraph has two overloads.</span></span> <span data-ttu-id="aa541-300">В простой перегрузке, используемой выше, EF Core определяет, когда следует прерывать обход графа.</span><span class="sxs-lookup"><span data-stu-id="aa541-300">In the simple overload used above, EF Core determines when to stop traversing the graph.</span></span> <span data-ttu-id="aa541-301">В частности, он останавливает посещение новых связанных сущностей из заданной сущности, когда эта сущность уже отслеживается или если обратный вызов не начинает отслеживание сущности.</span><span class="sxs-lookup"><span data-stu-id="aa541-301">Specifically, it stops visiting new related entities from a given entity when that entity is either already tracked, or when the callback does not start tracking the entity.</span></span>

<span data-ttu-id="aa541-302">Расширенная перегрузка, <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.TrackGraph%60%601(System.Object,%60%600,System.Func{Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntryGraphNode{%60%600},System.Boolean})?displayProperty=nameWithType> , имеет обратный вызов, возвращающий логическое значение.</span><span class="sxs-lookup"><span data-stu-id="aa541-302">The advanced overload, <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.TrackGraph%60%601(System.Object,%60%600,System.Func{Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntryGraphNode{%60%600},System.Boolean})?displayProperty=nameWithType>, has a callback that returns a bool.</span></span> <span data-ttu-id="aa541-303">Если обратный вызов возвращает значение false, обход графа останавливается, в противном случае — продолжение.</span><span class="sxs-lookup"><span data-stu-id="aa541-303">If the callback returns false, then graph traversal stops, otherwise it continues.</span></span> <span data-ttu-id="aa541-304">Необходимо соблюдать осторожность, чтобы избежать бесконечных циклов при использовании этой перегрузки.</span><span class="sxs-lookup"><span data-stu-id="aa541-304">Care must be taken to avoid infinite loops when using this overload.</span></span>

<span data-ttu-id="aa541-305">Расширенная перегрузка также позволяет передавать состояние в Траккграф, а это состояние затем передается каждому обратному вызову.</span><span class="sxs-lookup"><span data-stu-id="aa541-305">The advanced overload also allows state to be supplied to TrackGraph and this state is then passed to each callback.</span></span>
