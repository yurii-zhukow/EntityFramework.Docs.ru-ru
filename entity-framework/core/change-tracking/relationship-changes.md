---
title: Изменение внешних ключей и переходов — EF Core
description: Изменение связей между сущностями путем манипулирования внешними ключами и переходами
author: ajcvickers
ms.date: 12/30/2020
uid: core/change-tracking/relationship-changes
ms.openlocfilehash: b1ebe77ed29291beeef3708b603db026c38bbbec
ms.sourcegitcommit: 7700840119b1639275f3b64836e7abb59103f2e7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2021
ms.locfileid: "98983616"
---
# <a name="changing-foreign-keys-and-navigations"></a><span data-ttu-id="c29e6-103">Изменение внешних ключей и переходов</span><span class="sxs-lookup"><span data-stu-id="c29e6-103">Changing Foreign Keys and Navigations</span></span>

## <a name="overview-of-foreign-keys-and-navigations"></a><span data-ttu-id="c29e6-104">Общие сведения о внешних ключах и переходах</span><span class="sxs-lookup"><span data-stu-id="c29e6-104">Overview of foreign keys and navigations</span></span>

<span data-ttu-id="c29e6-105">Связи в модели Entity Framework Core (EF Core) представлены с помощью внешних ключей (внешние ключи).</span><span class="sxs-lookup"><span data-stu-id="c29e6-105">Relationships in an Entity Framework Core (EF Core) model are represented using foreign keys (FKs).</span></span> <span data-ttu-id="c29e6-106">FK состоит из одного или нескольких свойств в зависимой или дочерней сущности в связи.</span><span class="sxs-lookup"><span data-stu-id="c29e6-106">An FK consists of one or more properties on the dependent or child entity in the relationship.</span></span> <span data-ttu-id="c29e6-107">Эта зависимая или Дочерняя сущность связана с заданной основной или родительской сущностью, если значения свойств внешнего ключа в зависимом или дочернем элементе соответствуют значениям свойств альтернативного или первичного ключа (PK) основного или родительского ключей.</span><span class="sxs-lookup"><span data-stu-id="c29e6-107">This dependent/child entity is associated with a given principal/parent entity when the values of the foreign key properties on the dependent/child match the values of the alternate or primary key (PK) properties on the principal/parent.</span></span>

<span data-ttu-id="c29e6-108">Внешние ключи — это хороший способ хранения связей и управления ими в базе данных, но они не очень удобны при работе с несколькими связанными сущностями в коде приложения.</span><span class="sxs-lookup"><span data-stu-id="c29e6-108">Foreign keys are a good way to store and manipulate relationships in the database, but are not very friendly when working with multiple related entities in application code.</span></span> <span data-ttu-id="c29e6-109">Таким образом, большинство моделей EF Core также являются переходами по слоям в представлении FK.</span><span class="sxs-lookup"><span data-stu-id="c29e6-109">Therefore, most EF Core models also layer "navigations" over the FK representation.</span></span> <span data-ttu-id="c29e6-110">Переходы формы C#/.NET ссылки между экземплярами сущности, отражающими ассоциации, найденные соответствующими значениями внешних ключей и первичными или альтернативными значениями ключа.</span><span class="sxs-lookup"><span data-stu-id="c29e6-110">Navigations form C#/.NET references between entity instances that reflect the associations found by matching foreign key values to primary or alternate key values.</span></span>

<span data-ttu-id="c29e6-111">Переходы можно использовать на обеих сторонах связи, только на одной стороне, а не на всех, при этом остается только свойство FK.</span><span class="sxs-lookup"><span data-stu-id="c29e6-111">Navigations can be used on both sides of the relationship, on one side only, or not at all, leaving only the FK property.</span></span> <span data-ttu-id="c29e6-112">Свойство FK можно скрыть, сделав его [свойством Shadow](xref:core/modeling/shadow-properties).</span><span class="sxs-lookup"><span data-stu-id="c29e6-112">The FK property can be hidden by making it a [shadow property](xref:core/modeling/shadow-properties).</span></span> <span data-ttu-id="c29e6-113">Дополнительные сведения о связях между моделированием см. в разделе [связи](xref:core/modeling/relationships) .</span><span class="sxs-lookup"><span data-stu-id="c29e6-113">See [Relationships](xref:core/modeling/relationships) for more information on modelling relationships.</span></span>

> [!TIP]
> <span data-ttu-id="c29e6-114">В этом документе предполагается, что состояния сущностей и основы отслеживания изменений EF Core понятны.</span><span class="sxs-lookup"><span data-stu-id="c29e6-114">This document assumes that entity states and the basics of EF Core change tracking are understood.</span></span> <span data-ttu-id="c29e6-115">Дополнительные сведения по этим темам см. [в разделе Отслеживание изменений в EF Core](xref:core/change-tracking/index) .</span><span class="sxs-lookup"><span data-stu-id="c29e6-115">See [Change Tracking in EF Core](xref:core/change-tracking/index) for more information on these topics.</span></span>

> [!TIP]
> <span data-ttu-id="c29e6-116">Вы можете запустить и отладить весь код, используемый в этой документации, [скачав пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/ChangeTracking/ChangingFKsAndNavigations).</span><span class="sxs-lookup"><span data-stu-id="c29e6-116">You can run and debug into all the code in this document by [downloading the sample code from GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/ChangeTracking/ChangingFKsAndNavigations).</span></span>

### <a name="example-model"></a><span data-ttu-id="c29e6-117">Пример модели</span><span class="sxs-lookup"><span data-stu-id="c29e6-117">Example model</span></span>

<span data-ttu-id="c29e6-118">Следующая модель содержит четыре типа сущностей со связями между ними.</span><span class="sxs-lookup"><span data-stu-id="c29e6-118">The following model contains four entity types with relationships between them.</span></span> <span data-ttu-id="c29e6-119">Комментарии в коде указывают, какие свойства являются внешними ключами, первичными ключами и навигациями.</span><span class="sxs-lookup"><span data-stu-id="c29e6-119">The comments in the code indicate which properties are foreign keys, primary keys, and navigations.</span></span>

<!--
public class Blog
{
    public int Id { get; set; } // Primary key
    public string Name { get; set; }

    public IList<Post> Posts { get; } = new List<Post>(); // Collection navigation
    public BlogAssets Assets { get; set; } // Reference navigation
}

public class BlogAssets
{
    public int Id { get; set; } // Primary key
    public byte[] Banner { get; set; }

    public int BlogId { get; set; } // Foreign key
    public Blog Blog { get; set; } // Reference navigation
}

public class Post
{
    public int Id { get; set; } // Primary key
    public string Title { get; set; }
    public string Content { get; set; }

    public int? BlogId { get; set; } // Foreign key
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
[!code-csharp[Model](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Model)]

<span data-ttu-id="c29e6-120">В этой модели используются следующие три связи:</span><span class="sxs-lookup"><span data-stu-id="c29e6-120">The three relationships in this model are:</span></span>

- <span data-ttu-id="c29e6-121">Каждый блог может иметь много записей («один ко многим»):</span><span class="sxs-lookup"><span data-stu-id="c29e6-121">Each blog can have many posts (one-to-many):</span></span>
  - <span data-ttu-id="c29e6-122">`Blog` является участником или родителем.</span><span class="sxs-lookup"><span data-stu-id="c29e6-122">`Blog` is the principal/parent.</span></span>
  - <span data-ttu-id="c29e6-123">`Post` — Это зависимый/дочерний элемент.</span><span class="sxs-lookup"><span data-stu-id="c29e6-123">`Post` is the dependent/child.</span></span> <span data-ttu-id="c29e6-124">Он содержит свойство FK `Post.BlogId` , значение которого должно совпадать со `Blog.Id` значением PK соответствующего блога.</span><span class="sxs-lookup"><span data-stu-id="c29e6-124">It contains the FK property `Post.BlogId`, the value of which must match the `Blog.Id` PK value of the related blog.</span></span>
  - <span data-ttu-id="c29e6-125">`Post.Blog` — это навигационная ссылка из записи в связанный блог.</span><span class="sxs-lookup"><span data-stu-id="c29e6-125">`Post.Blog` is a reference navigation from a post to the associated blog.</span></span> <span data-ttu-id="c29e6-126">`Post.Blog` является обратной навигацией для `Blog.Posts` .</span><span class="sxs-lookup"><span data-stu-id="c29e6-126">`Post.Blog` is the inverse navigation for `Blog.Posts`.</span></span>
  - <span data-ttu-id="c29e6-127">`Blog.Posts` — Это Навигация коллекции из блога со всеми связанными записями.</span><span class="sxs-lookup"><span data-stu-id="c29e6-127">`Blog.Posts` is a collection navigation from a blog to all the associated posts.</span></span> <span data-ttu-id="c29e6-128">`Blog.Posts` является обратной навигацией для `Post.Blog` .</span><span class="sxs-lookup"><span data-stu-id="c29e6-128">`Blog.Posts` is the inverse navigation for `Post.Blog`.</span></span>
- <span data-ttu-id="c29e6-129">Каждый блог может иметь один ресурс (один к одному):</span><span class="sxs-lookup"><span data-stu-id="c29e6-129">Each blog can have one assets (one-to-one):</span></span>
  - <span data-ttu-id="c29e6-130">`Blog` является участником или родителем.</span><span class="sxs-lookup"><span data-stu-id="c29e6-130">`Blog` is the principal/parent.</span></span>
  - <span data-ttu-id="c29e6-131">`BlogAssets` — Это зависимый/дочерний элемент.</span><span class="sxs-lookup"><span data-stu-id="c29e6-131">`BlogAssets` is the dependent/child.</span></span> <span data-ttu-id="c29e6-132">Он содержит свойство FK `BlogAssets.BlogId` , значение которого должно совпадать со `Blog.Id` значением PK соответствующего блога.</span><span class="sxs-lookup"><span data-stu-id="c29e6-132">It contains the FK property `BlogAssets.BlogId`, the value of which must match the `Blog.Id` PK value of the related blog.</span></span>
  - <span data-ttu-id="c29e6-133">`BlogAssets.Blog` — Это ссылка на навигацию из ресурсов в связанный блог.</span><span class="sxs-lookup"><span data-stu-id="c29e6-133">`BlogAssets.Blog` is a reference navigation from the assets to the associated blog.</span></span> <span data-ttu-id="c29e6-134">`BlogAssets.Blog` является обратной навигацией для `Blog.Assets` .</span><span class="sxs-lookup"><span data-stu-id="c29e6-134">`BlogAssets.Blog` is the inverse navigation for `Blog.Assets`.</span></span>
  - <span data-ttu-id="c29e6-135">`Blog.Assets` — это навигационная ссылка из блога на связанные ресурсы.</span><span class="sxs-lookup"><span data-stu-id="c29e6-135">`Blog.Assets` is a reference navigation from the blog to the associated assets.</span></span> <span data-ttu-id="c29e6-136">`Blog.Assets` является обратной навигацией для `BlogAssets.Blog` .</span><span class="sxs-lookup"><span data-stu-id="c29e6-136">`Blog.Assets` is the inverse navigation for `BlogAssets.Blog`.</span></span>
- <span data-ttu-id="c29e6-137">Каждая запись может иметь много тегов, и каждый тег может иметь много записей («многие ко многим»):</span><span class="sxs-lookup"><span data-stu-id="c29e6-137">Each post can have many tags and each tag can have many posts (many-to-many):</span></span>
  - <span data-ttu-id="c29e6-138">Связи «многие ко многим» — это более детальный уровень связи между 2 1 и многими.</span><span class="sxs-lookup"><span data-stu-id="c29e6-138">Many-to-many relationships are a further layer over two one-to-many relationships.</span></span> <span data-ttu-id="c29e6-139">Связи «многие ко многим» рассматриваются далее в этом документе.</span><span class="sxs-lookup"><span data-stu-id="c29e6-139">Many-to-many relationships are covered later in this document.</span></span>
  - <span data-ttu-id="c29e6-140">`Post.Tags` — Это Навигация по коллекции от записи до всех связанных тегов.</span><span class="sxs-lookup"><span data-stu-id="c29e6-140">`Post.Tags` is a collection navigation from a post to all the associated tags.</span></span> <span data-ttu-id="c29e6-141">`Post.Tags` является обратной навигацией для `Tag.Posts` .</span><span class="sxs-lookup"><span data-stu-id="c29e6-141">`Post.Tags` is the inverse navigation for `Tag.Posts`.</span></span>
  - <span data-ttu-id="c29e6-142">`Tag.Posts` — Это Навигация в коллекции от тега ко всем связанным записям.</span><span class="sxs-lookup"><span data-stu-id="c29e6-142">`Tag.Posts` is a collection navigation from a tag to all the associated posts.</span></span> <span data-ttu-id="c29e6-143">`Tag.Posts` является обратной навигацией для `Post.Tags` .</span><span class="sxs-lookup"><span data-stu-id="c29e6-143">`Tag.Posts` is the inverse navigation for `Post.Tags`.</span></span>

<span data-ttu-id="c29e6-144">Дополнительные сведения о том, как моделировать и настраивать связи, см. в разделе [связи](xref:core/modeling/relationships) .</span><span class="sxs-lookup"><span data-stu-id="c29e6-144">See [Relationships](xref:core/modeling/relationships) for more information on how to model and configure relationships.</span></span>

## <a name="relationship-fixup"></a><span data-ttu-id="c29e6-145">Исправление связи</span><span class="sxs-lookup"><span data-stu-id="c29e6-145">Relationship fixup</span></span>

<span data-ttu-id="c29e6-146">EF Core поддерживает переходы по значениям внешних ключей и наоборот.</span><span class="sxs-lookup"><span data-stu-id="c29e6-146">EF Core keeps navigations in alignment with foreign key values and vice versa.</span></span> <span data-ttu-id="c29e6-147">То есть, если значение внешнего ключа изменяется таким образом, что теперь оно ссылается на другую сущность или родительский объект, переходы обновляются в соответствии с этим изменением.</span><span class="sxs-lookup"><span data-stu-id="c29e6-147">That is, if a foreign key value changes such that it now refers to a different principal/parent entity, then the navigations are updated to reflect this change.</span></span> <span data-ttu-id="c29e6-148">Аналогично, при изменении навигации значения внешних ключей участвующих сущностей обновляются в соответствии с этим изменением.</span><span class="sxs-lookup"><span data-stu-id="c29e6-148">Likewise, if a navigation is changed, then the foreign key values of the entities involved are updated to reflect this change.</span></span> <span data-ttu-id="c29e6-149">Это называется "исправлением связи".</span><span class="sxs-lookup"><span data-stu-id="c29e6-149">This is called "relationship fixup".</span></span>

### <a name="fixup-by-query"></a><span data-ttu-id="c29e6-150">Адресная привязка по запросу</span><span class="sxs-lookup"><span data-stu-id="c29e6-150">Fixup by query</span></span>

<span data-ttu-id="c29e6-151">Начальная адресная привязка происходит при запросе сущностей из базы данных.</span><span class="sxs-lookup"><span data-stu-id="c29e6-151">Fixup first occurs when entities are queried from the database.</span></span> <span data-ttu-id="c29e6-152">База данных содержит только значения внешнего ключа, поэтому, когда EF Core создает экземпляр сущности из базы данных, он использует значения внешнего ключа для установки ссылок на навигацию и добавления сущностей в области навигации по коллекции по мере необходимости.</span><span class="sxs-lookup"><span data-stu-id="c29e6-152">The database has only foreign key values, so when EF Core creates an entity instance from the database it uses the foreign key values to set reference navigations and add entities to collection navigations as appropriate.</span></span> <span data-ttu-id="c29e6-153">Например, рассмотрим запрос блогов и связанных с ним сообщений и ресурсов.</span><span class="sxs-lookup"><span data-stu-id="c29e6-153">For example, consider a query for blogs and its associated posts and assets:</span></span>

<!--
        using var context = new BlogsContext();

        var blogs = context.Blogs
            .Include(e => e.Posts)
            .Include(e => e.Assets)
            .ToList();

        Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Relationship_fixup_1](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Relationship_fixup_1)]

<span data-ttu-id="c29e6-154">Для каждого блога EF Core сначала создаст `Blog` экземпляр.</span><span class="sxs-lookup"><span data-stu-id="c29e6-154">For each blog, EF Core will first create a `Blog` instance.</span></span> <span data-ttu-id="c29e6-155">Затем, когда каждая запись загружается из базы данных, `Post.Blog` для нее устанавливается ссылка на соответствующий блог.</span><span class="sxs-lookup"><span data-stu-id="c29e6-155">Then, as each post is loaded from the database its `Post.Blog` reference navigation is set to point to the associated blog.</span></span> <span data-ttu-id="c29e6-156">Аналогичным образом запись добавляется в `Blog.Posts` навигацию по коллекции.</span><span class="sxs-lookup"><span data-stu-id="c29e6-156">Likewise, the post is added to the `Blog.Posts` collection navigation.</span></span> <span data-ttu-id="c29e6-157">То же самое происходит с `BlogAssets` , за исключением того, что в этом случае переходы являются ссылками.</span><span class="sxs-lookup"><span data-stu-id="c29e6-157">The same thing happens with `BlogAssets`, except in this case both navigations are references.</span></span> <span data-ttu-id="c29e6-158">`Blog.Assets`Навигация настраивается так, чтобы указывать на экземпляр Assets, а `BlogAsserts.Blog` Навигация установлена так, чтобы указывать на экземпляр блога.</span><span class="sxs-lookup"><span data-stu-id="c29e6-158">The `Blog.Assets` navigation is set to point to the assets instance, and the `BlogAsserts.Blog` navigation is set to point to the blog instance.</span></span>

<span data-ttu-id="c29e6-159">[Просмотр представления отладки "средство записи изменений](xref:core/change-tracking/debug-views) " после того, как этот запрос показывает два блога, каждый с одним активом и двумя отслеживаемыми записями:</span><span class="sxs-lookup"><span data-stu-id="c29e6-159">Looking at the [change tracker debug view](xref:core/change-tracking/debug-views) after this query shows two blogs, each with one assets and two posts being tracked:</span></span>

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Assets: {Id: 1}
  Posts: [{Id: 1}, {Id: 2}]
Blog {Id: 2} Unchanged
  Id: 2 PK
  Name: 'Visual Studio Blog'
  Assets: {Id: 2}
  Posts: [{Id: 3}, {Id: 4}]
BlogAssets {Id: 1} Unchanged
  Id: 1 PK
  Banner: <null>
  BlogId: 1 FK
  Blog: {Id: 1}
BlogAssets {Id: 2} Unchanged
  Id: 2 PK
  Banner: <null>
  BlogId: 2 FK
  Blog: {Id: 2}
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
  Tags: []
Post {Id: 2} Unchanged
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: {Id: 1}
  Tags: []
Post {Id: 3} Unchanged
  Id: 3 PK
  BlogId: 2 FK
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: {Id: 2}
  Tags: []
Post {Id: 4} Unchanged
  Id: 4 PK
  BlogId: 2 FK
  Content: 'Examine when database queries were executed and measure how ...'
  Title: 'Database Profiling with Visual Studio'
  Blog: {Id: 2}
  Tags: []
```

<span data-ttu-id="c29e6-160">В представлении Отладка отображаются ключевые значения и переходы.</span><span class="sxs-lookup"><span data-stu-id="c29e6-160">The debug view shows both key values and navigations.</span></span> <span data-ttu-id="c29e6-161">Переходы отображаются с использованием значений первичного ключа связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="c29e6-161">Navigations are shown using the primary key values of the related entities.</span></span> <span data-ttu-id="c29e6-162">Например, `Posts: [{Id: 1}, {Id: 2}]` в приведенном выше выводе указывает, что `Blog.Posts` Навигация по коллекциям содержит две связанные записи с первичными ключами 1 и 2 соответственно.</span><span class="sxs-lookup"><span data-stu-id="c29e6-162">For example, `Posts: [{Id: 1}, {Id: 2}]` in the output above indicates that the `Blog.Posts` collection navigation contains two related posts with primary keys 1 and 2 respectively.</span></span> <span data-ttu-id="c29e6-163">Аналогичным образом для каждой записи, связанной с первым блогом, `Blog: {Id: 1}` строка указывает на то, что `Post.Blog` Переход ссылается на блог с первичным ключом 1.</span><span class="sxs-lookup"><span data-stu-id="c29e6-163">Similarly, for each post associated with the first blog, the `Blog: {Id: 1}` line indicates that the `Post.Blog` navigation references the Blog with primary key 1.</span></span>

### <a name="fixup-to-locally-tracked-entities"></a><span data-ttu-id="c29e6-164">Исправление для локально записанных сущностей</span><span class="sxs-lookup"><span data-stu-id="c29e6-164">Fixup to locally tracked entities</span></span>

<span data-ttu-id="c29e6-165">Исправление связи также происходит между сущностями, возвращаемыми запросом отслеживания, и сущностями, которые уже отслеживаются DbContext.</span><span class="sxs-lookup"><span data-stu-id="c29e6-165">Relationship fixup also happens between entities returned from a tracking query and entities already tracked by the DbContext.</span></span> <span data-ttu-id="c29e6-166">Например, можно выполнить три отдельных запроса для блогов, записей и ресурсов:</span><span class="sxs-lookup"><span data-stu-id="c29e6-166">For example, consider executing three separate queries for blogs, posts, and assets:</span></span>

<!--
        using var context = new BlogsContext();

        var blogs = context.Blogs.ToList();
        Console.WriteLine(context.ChangeTracker.DebugView.LongView);

        var assets = context.Assets.ToList();
        Console.WriteLine(context.ChangeTracker.DebugView.LongView);

        var posts = context.Posts.ToList();
        Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Relationship_fixup_2](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Relationship_fixup_2)]
<span data-ttu-id="c29e6-167">Еще раз взгляните на представления отладки, после того как будет записан первый запрос только двух блогов:</span><span class="sxs-lookup"><span data-stu-id="c29e6-167">Looking again at the debug views, after the first query only the two blogs are tracked:</span></span>

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Assets: <null>
  Posts: []
Blog {Id: 2} Unchanged
  Id: 2 PK
  Name: 'Visual Studio Blog'
  Assets: <null>
  Posts: []
```

<span data-ttu-id="c29e6-168">`Blog.Assets`Переходы по ссылке имеют значение null, а `Blog.Posts` навигационные коллекции пусты, так как в данный момент в контексте не отслеживается ни одна из связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="c29e6-168">The `Blog.Assets` reference navigations are null, and the `Blog.Posts` collection navigations are empty because no associated entities are currently being tracked by the context.</span></span>

<span data-ttu-id="c29e6-169">После второго запроса `Blogs.Assets` были исправлены навигационные ссылки, указывающие на новые записанные `BlogAsset` экземпляры.</span><span class="sxs-lookup"><span data-stu-id="c29e6-169">After the second query, the `Blogs.Assets` reference navigations have been fixed up to point to the newly tracked `BlogAsset` instances.</span></span> <span data-ttu-id="c29e6-170">Аналогичным образом, `BlogAssets.Blog` навигационные ссылки указывают на соответствующий уже записанный `Blog` экземпляр.</span><span class="sxs-lookup"><span data-stu-id="c29e6-170">Likewise, the `BlogAssets.Blog` reference navigations are set to point to the appropriate already tracked `Blog` instance.</span></span>

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Assets: {Id: 1}
  Posts: []
Blog {Id: 2} Unchanged
  Id: 2 PK
  Name: 'Visual Studio Blog'
  Assets: {Id: 2}
  Posts: []
BlogAssets {Id: 1} Unchanged
  Id: 1 PK
  Banner: <null>
  BlogId: 1 FK
  Blog: {Id: 1}
BlogAssets {Id: 2} Unchanged
  Id: 2 PK
  Banner: <null>
  BlogId: 2 FK
  Blog: {Id: 2}
```

<span data-ttu-id="c29e6-171">Наконец, после третьего запроса `Blog.Posts` навигационные коллекции теперь содержат все связанные записи, а `Post.Blog` ссылки указывают на соответствующий `Blog` экземпляр:</span><span class="sxs-lookup"><span data-stu-id="c29e6-171">Finally, after the third query, the `Blog.Posts` collection navigations now contain all related posts, and the `Post.Blog` references point to the appropriate `Blog` instance:</span></span>

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Assets: {Id: 1}
  Posts: [{Id: 1}, {Id: 2}]
Blog {Id: 2} Unchanged
  Id: 2 PK
  Name: 'Visual Studio Blog'
  Assets: {Id: 2}
  Posts: [{Id: 3}, {Id: 4}]
BlogAssets {Id: 1} Unchanged
  Id: 1 PK
  Banner: <null>
  BlogId: 1 FK
  Blog: {Id: 1}
BlogAssets {Id: 2} Unchanged
  Id: 2 PK
  Banner: <null>
  BlogId: 2 FK
  Blog: {Id: 2}
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
  Tags: []
Post {Id: 2} Unchanged
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: {Id: 1}
  Tags: []
Post {Id: 3} Unchanged
  Id: 3 PK
  BlogId: 2 FK
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: {Id: 2}
  Tags: []
Post {Id: 4} Unchanged
  Id: 4 PK
  BlogId: 2 FK
  Content: 'Examine when database queries were executed and measure how ...'
  Title: 'Database Profiling with Visual Studio'
  Blog: {Id: 2}
  Tags: []
```

<span data-ttu-id="c29e6-172">Это то же самое, что и в исходном единственном запросе, поскольку EF Core исправлены переходы по мере отслеживания сущностей, даже если они поступают из нескольких разных запросов.</span><span class="sxs-lookup"><span data-stu-id="c29e6-172">This is the same end-state as was achieved with the original single query, since EF Core fixed up navigations as entities were tracked, even when coming from multiple different queries.</span></span>

> [!NOTE]
> <span data-ttu-id="c29e6-173">Адресная привязка никогда не приводит к возврату данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="c29e6-173">Fixup never causes more data to be returned from the database.</span></span> <span data-ttu-id="c29e6-174">Он соединяет только сущности, которые уже возвращены запросом или уже были зарегистрированы DbContext.</span><span class="sxs-lookup"><span data-stu-id="c29e6-174">It only connects entities that are already returned by the query or already tracked by the DbContext.</span></span> <span data-ttu-id="c29e6-175">Сведения об обработке дубликатов при сериализации сущностей см. [в разделе Разрешение удостоверений в EF Core](xref:core/change-tracking/identity-resolution) .</span><span class="sxs-lookup"><span data-stu-id="c29e6-175">See [Identity Resolution in EF Core](xref:core/change-tracking/identity-resolution) for information about handling duplicates when serializing entities.</span></span>

## <a name="changing-relationships-using-navigations"></a><span data-ttu-id="c29e6-176">Изменение связей с помощью переходов</span><span class="sxs-lookup"><span data-stu-id="c29e6-176">Changing relationships using navigations</span></span>

<span data-ttu-id="c29e6-177">Самый простой способ изменить связь между двумя сущностями — управлять навигацией, оставляя EF Core для правильной привязки обратной навигации и значений FK.</span><span class="sxs-lookup"><span data-stu-id="c29e6-177">The easiest way to change the relationship between two entities is by manipulating a navigation, while leaving EF Core to fixup the inverse navigation and FK values appropriately.</span></span> <span data-ttu-id="c29e6-178">Как это можно сделать:</span><span class="sxs-lookup"><span data-stu-id="c29e6-178">This can be done by:</span></span>

- <span data-ttu-id="c29e6-179">Добавление или удаление сущности из навигации по коллекции.</span><span class="sxs-lookup"><span data-stu-id="c29e6-179">Adding or removing an entity from a collection navigation.</span></span>
- <span data-ttu-id="c29e6-180">Изменение ссылки на навигацию для указания другой сущности или присвоения ей значения NULL.</span><span class="sxs-lookup"><span data-stu-id="c29e6-180">Changing a reference navigation to point to a different entity, or setting it to null.</span></span>

### <a name="adding-or-removing-from-collection-navigations"></a><span data-ttu-id="c29e6-181">Добавление или удаление из навигации по коллекциям</span><span class="sxs-lookup"><span data-stu-id="c29e6-181">Adding or removing from collection navigations</span></span>

<span data-ttu-id="c29e6-182">Например, давайте перейдем одну из записей из блога Visual Studio в блог .NET.</span><span class="sxs-lookup"><span data-stu-id="c29e6-182">For example, let's move one of the posts from the Visual Studio blog to the .NET blog.</span></span> <span data-ttu-id="c29e6-183">Для этого необходимо сначала загрузить блоги и записи, а затем переместить запись из коллекции навигации в одном блоге в коллекцию навигации в другом блоге:</span><span class="sxs-lookup"><span data-stu-id="c29e6-183">This requires first loading the blogs and posts, and then moving the post from the navigation collection on one blog to the navigation collection on the other blog:</span></span>

<!--
        using var context = new BlogsContext();

        var dotNetBlog = context.Blogs.Include(e => e.Posts).Single(e => e.Name == ".NET Blog");
        var vsBlog = context.Blogs.Include(e => e.Posts).Single(e => e.Name == "Visual Studio Blog");

        Console.WriteLine(context.ChangeTracker.DebugView.LongView);

        var post = vsBlog.Posts.Single(e => e.Title.StartsWith("Disassembly improvements"));
        vsBlog.Posts.Remove(post);
        dotNetBlog.Posts.Add(post);

        context.ChangeTracker.DetectChanges();
        Console.WriteLine(context.ChangeTracker.DebugView.LongView);

        context.SaveChanges();
-->
[!code-csharp[Changing_relationships_using_navigations_1](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Changing_relationships_using_navigations_1)]

> [!TIP]
> <span data-ttu-id="c29e6-184">Вызов требуется, <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges?displayProperty=nameWithType> так как доступ к представлению отладки не приводит к [автоматическому обнаружению изменений](xref:core/change-tracking/change-detection).</span><span class="sxs-lookup"><span data-stu-id="c29e6-184">A call to <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges?displayProperty=nameWithType> is needed here because accessing the debug view does not cause [automatic detection of changes](xref:core/change-tracking/change-detection).</span></span>

<span data-ttu-id="c29e6-185">Это представление отладки печатается после выполнения приведенного выше кода:</span><span class="sxs-lookup"><span data-stu-id="c29e6-185">This is the debug view printed after running the code above:</span></span>

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Assets: <null>
  Posts: [{Id: 1}, {Id: 2}, {Id: 3}]
Blog {Id: 2} Unchanged
  Id: 2 PK
  Name: 'Visual Studio Blog'
  Assets: <null>
  Posts: [{Id: 4}]
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
  Tags: []
Post {Id: 2} Unchanged
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: {Id: 1}
  Tags: []
Post {Id: 3} Modified
  Id: 3 PK
  BlogId: 1 FK Modified Originally 2
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: {Id: 1}
  Tags: []
Post {Id: 4} Unchanged
  Id: 4 PK
  BlogId: 2 FK
  Content: 'Examine when database queries were executed and measure how ...'
  Title: 'Database Profiling with Visual Studio'
  Blog: {Id: 2}
  Tags: []
```

<span data-ttu-id="c29e6-186">`Blog.Posts`Теперь в блоге .NET есть три записи ( `Posts: [{Id: 1}, {Id: 2}, {Id: 3}]` ).</span><span class="sxs-lookup"><span data-stu-id="c29e6-186">The `Blog.Posts` navigation on the .NET Blog now has three posts (`Posts: [{Id: 1}, {Id: 2}, {Id: 3}]`).</span></span> <span data-ttu-id="c29e6-187">Аналогичным образом, `Blog.Posts` Навигация в блоге Visual Studio имеет только одну запись ( `Posts: [{Id: 4}]` ).</span><span class="sxs-lookup"><span data-stu-id="c29e6-187">Likewise, the `Blog.Posts` navigation on the Visual Studio blog only has one post (`Posts: [{Id: 4}]`).</span></span> <span data-ttu-id="c29e6-188">Это должно быть ожидаемым, так как в коде явным образом изменяются эти коллекции.</span><span class="sxs-lookup"><span data-stu-id="c29e6-188">This is to be expected since the code explicitly changed these collections.</span></span>

<span data-ttu-id="c29e6-189">Более интересно, даже несмотря на то, что код не был явно изменил `Post.Blog` навигацию, он был исправлен до того, как он указывал на блог Visual Studio ( `Blog: {Id: 1}` ).</span><span class="sxs-lookup"><span data-stu-id="c29e6-189">More interestingly, even though the code did not explicitly change the `Post.Blog` navigation, it has been fixed-up to point to the Visual Studio blog (`Blog: {Id: 1}`).</span></span> <span data-ttu-id="c29e6-190">Кроме того, `Post.BlogId` значение внешнего ключа было Обновлено в соответствии со значением первичного ключа в блоге .NET.</span><span class="sxs-lookup"><span data-stu-id="c29e6-190">Also, the `Post.BlogId` foreign key value has been updated to match the primary key value of the .NET blog.</span></span> <span data-ttu-id="c29e6-191">Это изменение значения FK в затем сохраняется в базе данных при вызове SaveChanges:</span><span class="sxs-lookup"><span data-stu-id="c29e6-191">This change to the FK value in then persisted to the database when SaveChanges is called:</span></span>

```sql
-- Executed DbCommand (0ms) [Parameters=[@p1='3' (DbType = String), @p0='1' (Nullable = true) (DbType = String)], CommandType='Text', CommandTimeout='30']
UPDATE "Posts" SET "BlogId" = @p0
WHERE "Id" = @p1;
SELECT changes();
```

### <a name="changing-reference-navigations"></a><span data-ttu-id="c29e6-192">Изменение переходов по ссылке</span><span class="sxs-lookup"><span data-stu-id="c29e6-192">Changing reference navigations</span></span>

<span data-ttu-id="c29e6-193">В предыдущем примере запись была перемещена из одного блога в другой путем управления навигацией по записям в каждом блоге.</span><span class="sxs-lookup"><span data-stu-id="c29e6-193">In the previous example, a post was moved from one blog to another by manipulating the collection navigation of posts on each blog.</span></span> <span data-ttu-id="c29e6-194">То же самое можно сделать, вместо этого измените `Post.Blog` навигацию по ссылке, чтобы она указывала на новый блог.</span><span class="sxs-lookup"><span data-stu-id="c29e6-194">The same thing can be achieved by instead changing the `Post.Blog` reference navigation to point to the new blog.</span></span> <span data-ttu-id="c29e6-195">Пример:</span><span class="sxs-lookup"><span data-stu-id="c29e6-195">For example:</span></span>

<!--
        var post = vsBlog.Posts.Single(e => e.Title.StartsWith("Disassembly improvements"));
        post.Blog = dotNetBlog;
-->
[!code-csharp[Changing_relationships_using_navigations_2](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Changing_relationships_using_navigations_2)]

<span data-ttu-id="c29e6-196">Представление отладки после этого изменения будет _точно таким же_ , как и в предыдущем примере.</span><span class="sxs-lookup"><span data-stu-id="c29e6-196">The debug view after this change is _exactly the same_ as it was in the previous example.</span></span> <span data-ttu-id="c29e6-197">Это связано с тем, что EF Core обнаружил изменение ссылочной навигации, а затем предисправил навигацию по коллекции и значение FK для сопоставления.</span><span class="sxs-lookup"><span data-stu-id="c29e6-197">This because EF Core detected the reference navigation change and then fixed up the collection navigations and FK value to match.</span></span>

## <a name="changing-relationships-using-foreign-key-values"></a><span data-ttu-id="c29e6-198">Изменение связей с использованием значений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="c29e6-198">Changing relationships using foreign key values</span></span>

<span data-ttu-id="c29e6-199">В предыдущем разделе связи были изменены с помощью переходов, при этом значения внешних ключей обновляются автоматически.</span><span class="sxs-lookup"><span data-stu-id="c29e6-199">In the previous section, relationships were manipulated by navigations leaving foreign key values to be updated automatically.</span></span> <span data-ttu-id="c29e6-200">Это рекомендуемый способ управления связями в EF Core.</span><span class="sxs-lookup"><span data-stu-id="c29e6-200">This is the recommended way to manipulate relationships in EF Core.</span></span> <span data-ttu-id="c29e6-201">Однако можно также управлять значениями FK напрямую.</span><span class="sxs-lookup"><span data-stu-id="c29e6-201">However, it is also possible to manipulate FK values directly.</span></span> <span data-ttu-id="c29e6-202">Например, можно переместить запись из одного блога в другой, изменив `Post.BlogId` значение внешнего ключа:</span><span class="sxs-lookup"><span data-stu-id="c29e6-202">For example, we can move a post from one blog to another by changing the `Post.BlogId` foreign key value:</span></span>

<!--
        var post = vsBlog.Posts.Single(e => e.Title.StartsWith("Disassembly improvements"));
        post.BlogId = dotNetBlog.Id;
-->
[!code-csharp[Changing_relationships_using_foreign_key_values_1](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Changing_relationships_using_foreign_key_values_1)]

<span data-ttu-id="c29e6-203">Обратите внимание, что это очень похоже на изменение навигации по ссылке, как показано в предыдущем примере.</span><span class="sxs-lookup"><span data-stu-id="c29e6-203">Notice how this is very similar to changing the reference navigation, as shown in the previous example.</span></span>

<span data-ttu-id="c29e6-204">Представление отладки после этого изменения опять _же,_ как и в случае двух предыдущих примеров.</span><span class="sxs-lookup"><span data-stu-id="c29e6-204">The debug view after this change is again _exactly the same_ as was the case for the previous two examples.</span></span> <span data-ttu-id="c29e6-205">Это связано с тем, что EF Core обнаружил изменение значения FK, а затем исправил ссылки и навигацию коллекции для сопоставления.</span><span class="sxs-lookup"><span data-stu-id="c29e6-205">This because EF Core detected the FK value change and then fixed up both the reference and collection navigations to match.</span></span>

> [!TIP]
> <span data-ttu-id="c29e6-206">Не следует писать код для обработки всех переходов и значений FK при каждом изменении связи.</span><span class="sxs-lookup"><span data-stu-id="c29e6-206">Do not write code to manipulate all navigations and FK values each time a relationship changes.</span></span> <span data-ttu-id="c29e6-207">Такой код более сложен и должен обеспечивать единообразное изменение внешних ключей и переходов во всех случаях.</span><span class="sxs-lookup"><span data-stu-id="c29e6-207">Such code is more complicated and must ensure consistent changes to foreign keys and navigations in every case.</span></span> <span data-ttu-id="c29e6-208">Если возможно, просто обработайте одну навигацию или, возможно, и навигацию.</span><span class="sxs-lookup"><span data-stu-id="c29e6-208">If possible, just manipulate a single navigation, or maybe both navigations.</span></span> <span data-ttu-id="c29e6-209">При необходимости просто измените значения FK.</span><span class="sxs-lookup"><span data-stu-id="c29e6-209">If needed, just manipulate FK values.</span></span> <span data-ttu-id="c29e6-210">Избегайте манипулирования как навигацией, так и значениями FK.</span><span class="sxs-lookup"><span data-stu-id="c29e6-210">Avoid manipulating both navigations and FK values.</span></span>

## <a name="fixup-for-added-or-deleted-entities"></a><span data-ttu-id="c29e6-211">Исправление для добавленных или удаленных сущностей</span><span class="sxs-lookup"><span data-stu-id="c29e6-211">Fixup for added or deleted entities</span></span>

### <a name="adding-to-a-collection-navigation"></a><span data-ttu-id="c29e6-212">Добавление в навигацию по коллекциям</span><span class="sxs-lookup"><span data-stu-id="c29e6-212">Adding to a collection navigation</span></span>

<span data-ttu-id="c29e6-213">EF Core выполняет следующие действия, когда [обнаруживает](xref:core/change-tracking/change-detection) , что в навигацию коллекции добавлена новая зависимая или Дочерняя сущность:</span><span class="sxs-lookup"><span data-stu-id="c29e6-213">EF Core performs the following actions when it [detects](xref:core/change-tracking/change-detection) that a new dependent/child entity has been added to a collection navigation:</span></span>

- <span data-ttu-id="c29e6-214">Если сущность не будет отслеживанием, она будет отслеживанием.</span><span class="sxs-lookup"><span data-stu-id="c29e6-214">If the entity is not tracked, then it is tracked.</span></span> <span data-ttu-id="c29e6-215">(Сущность обычно находится в `Added` состоянии.</span><span class="sxs-lookup"><span data-stu-id="c29e6-215">(The entity will usually be in the `Added` state.</span></span> <span data-ttu-id="c29e6-216">Однако если для типа сущности настроено использование созданных ключей и задано значение первичного ключа, то сущность будет отслеживанием в `Unchanged` состоянии.)</span><span class="sxs-lookup"><span data-stu-id="c29e6-216">However, if the entity type is configured to use generated keys and the primary key value is set, then the entity is tracked in the `Unchanged` state.)</span></span>
- <span data-ttu-id="c29e6-217">Если сущность связана с другим участником или родителем, то эта связь является серьезной.</span><span class="sxs-lookup"><span data-stu-id="c29e6-217">If the entity is associated with a different principal/parent, then that relationship is severed.</span></span>
- <span data-ttu-id="c29e6-218">Сущность становится связанной с участником/родителем, которому принадлежит Навигация по коллекции.</span><span class="sxs-lookup"><span data-stu-id="c29e6-218">The entity becomes associated with the principal/parent that owns the collection navigation.</span></span>
- <span data-ttu-id="c29e6-219">Переходы и значения внешних ключей фиксируются для всех участвующих сущностей.</span><span class="sxs-lookup"><span data-stu-id="c29e6-219">Navigations and foreign key values are fixed up for all entities involved.</span></span>

<span data-ttu-id="c29e6-220">На основе этого мы видим, что для перемещения записи из одного блога в другой нам не нужно удалять его из старой структуры коллекции, прежде чем добавлять в новый.</span><span class="sxs-lookup"><span data-stu-id="c29e6-220">Based on this we can see that to move a post from one blog to another we don't actually need to remove it from the old collection navigation before adding it to the new.</span></span> <span data-ttu-id="c29e6-221">Поэтому код из приведенного выше примера можно изменить с:</span><span class="sxs-lookup"><span data-stu-id="c29e6-221">So the code from the example above can be changed from:</span></span>

<!--
        var post = vsBlog.Posts.Single(e => e.Title.StartsWith("Disassembly improvements"));
        vsBlog.Posts.Remove(post);
        dotNetBlog.Posts.Add(post);
-->
[!code-csharp[Fixup_for_added_or_deleted_entities_1](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Fixup_for_added_or_deleted_entities_1)]

<span data-ttu-id="c29e6-222">на:</span><span class="sxs-lookup"><span data-stu-id="c29e6-222">To:</span></span>

<!--
        var post = vsBlog.Posts.Single(e => e.Title.StartsWith("Disassembly improvements"));
        dotNetBlog.Posts.Add(post);
-->
[!code-csharp[Fixup_for_added_or_deleted_entities_2](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Fixup_for_added_or_deleted_entities_2)]

<span data-ttu-id="c29e6-223">EF Core видит, что запись была добавлена в новый блог, и автоматически удаляет ее из коллекции в первом блоге.</span><span class="sxs-lookup"><span data-stu-id="c29e6-223">EF Core sees that the post has been added to a new blog and automatically removes it from the collection on the first blog.</span></span>

### <a name="removing-from-a-collection-navigation"></a><span data-ttu-id="c29e6-224">Удаление из структуры навигации коллекции</span><span class="sxs-lookup"><span data-stu-id="c29e6-224">Removing from a collection navigation</span></span>

<span data-ttu-id="c29e6-225">Удаление зависимой или дочерней сущности из структуры навигации субъекта-родителя или родительского объекта приводит к появлению серьезности отношения к этому участнику или родительскому объекту.</span><span class="sxs-lookup"><span data-stu-id="c29e6-225">Removing a dependent/child entity from the collection navigation of the principal/parent causes severing of the relationship to that principal/parent.</span></span> <span data-ttu-id="c29e6-226">Дальнейшие действия зависят от того, является ли связь необязательной или необязательной.</span><span class="sxs-lookup"><span data-stu-id="c29e6-226">What happens next depends on whether the relationship is optional or required.</span></span>

#### <a name="optional-relationships"></a><span data-ttu-id="c29e6-227">Необязательные связи</span><span class="sxs-lookup"><span data-stu-id="c29e6-227">Optional relationships</span></span>

<span data-ttu-id="c29e6-228">По умолчанию для необязательных связей значение внешнего ключа устанавливается равным null.</span><span class="sxs-lookup"><span data-stu-id="c29e6-228">By default for optional relationships, the foreign key value is set to null.</span></span> <span data-ttu-id="c29e6-229">Это означает, что зависимый/дочерний элемент больше не связан _ни с одним_ участником/родителем.</span><span class="sxs-lookup"><span data-stu-id="c29e6-229">This means that the dependent/child is no longer associated with _any_ principal/parent.</span></span> <span data-ttu-id="c29e6-230">Например, можно загрузить блог и записи, а затем удалить одну из записей из `Blog.Posts` навигации по коллекции:</span><span class="sxs-lookup"><span data-stu-id="c29e6-230">For example, let's load a blog and posts and then remove one of the posts from the `Blog.Posts` collection navigation:</span></span>

<!--
        var post = dotNetBlog.Posts.Single(e => e.Title == "Announcing F# 5");
        dotNetBlog.Posts.Remove(post);
-->
[!code-csharp[Fixup_for_added_or_deleted_entities_3](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Fixup_for_added_or_deleted_entities_3)]

<span data-ttu-id="c29e6-231">[Просмотрев представление Отладка отслеживания изменений](xref:core/change-tracking/debug-views) после этого изменения показывает, что:</span><span class="sxs-lookup"><span data-stu-id="c29e6-231">Looking at the [change tracking debug view](xref:core/change-tracking/debug-views) after this change shows that:</span></span>

- <span data-ttu-id="c29e6-232">`Post.BlogId`Для FK задано значение null ( `BlogId: <null> FK Modified Originally 1` )</span><span class="sxs-lookup"><span data-stu-id="c29e6-232">The `Post.BlogId` FK has been set to null (`BlogId: <null> FK Modified Originally 1`)</span></span>
- <span data-ttu-id="c29e6-233">`Post.Blog`Для навигации по ссылке задано значение null ( `Blog: <null>` )</span><span class="sxs-lookup"><span data-stu-id="c29e6-233">The `Post.Blog` reference navigation has been set to null (`Blog: <null>`)</span></span>
- <span data-ttu-id="c29e6-234">Запись была удалена из `Blog.Posts` навигации по коллекции ( `Posts: [{Id: 1}]` )</span><span class="sxs-lookup"><span data-stu-id="c29e6-234">The post has been removed from `Blog.Posts` collection navigation (`Posts: [{Id: 1}]`)</span></span>

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Assets: <null>
  Posts: [{Id: 1}]
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
  Tags: []
Post {Id: 2} Modified
  Id: 2 PK
  BlogId: <null> FK Modified Originally 1
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: <null>
  Tags: []
```

<span data-ttu-id="c29e6-235">Обратите внимание, что запись _не_ помечена как `Deleted` .</span><span class="sxs-lookup"><span data-stu-id="c29e6-235">Notice that the post is _not_ marked as `Deleted`.</span></span> <span data-ttu-id="c29e6-236">Он помечается как, `Modified` так что при вызове SaveChanges значение FK в базе данных будет равно null.</span><span class="sxs-lookup"><span data-stu-id="c29e6-236">It is marked as `Modified` so that the FK value in the database will be set to null when SaveChanges is called.</span></span>

#### <a name="required-relationships"></a><span data-ttu-id="c29e6-237">Обязательные связи</span><span class="sxs-lookup"><span data-stu-id="c29e6-237">Required relationships</span></span>

<span data-ttu-id="c29e6-238">Установка значения FK в NULL не допускается (и обычно невозможна) для требуемых связей.</span><span class="sxs-lookup"><span data-stu-id="c29e6-238">Setting the FK value to null is not allowed (and is usually not possible) for required relationships.</span></span> <span data-ttu-id="c29e6-239">Таким образом, важность необходимой связи означает, что зависимая или Дочерняя сущность должна быть либо повторно родительской, либо удалена из базы данных при вызове метода SaveChanges, чтобы избежать нарушения ссылочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="c29e6-239">Therefore, severing a required relationship means that the dependent/child entity must be either re-parented to a new principal/parent, or removed from the database when SaveChanges is called to avoid a referential constraint violation.</span></span> <span data-ttu-id="c29e6-240">Это называется удалением потерянных объектов и является поведением по умолчанию в EF Core для необходимых связей.</span><span class="sxs-lookup"><span data-stu-id="c29e6-240">This is known as "deleting orphans", and is the default behavior in EF Core for required relationships.</span></span>

<span data-ttu-id="c29e6-241">Например, измените связь между блогом и записями, которые должны быть обязательными, а затем выполните тот же код, что и в предыдущем примере:</span><span class="sxs-lookup"><span data-stu-id="c29e6-241">For example, let's change the relationship between blog and posts to be required and then run the same code as in the previous example:</span></span>

<!--
        var post = dotNetBlog.Posts.Single(e => e.Title == "Announcing F# 5");
        dotNetBlog.Posts.Remove(post);
-->
[!code-csharp[Fixup_for_added_or_deleted_entities_4](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/RequiredRelationshipsSamples.cs?name=Fixup_for_added_or_deleted_entities_4)]

<span data-ttu-id="c29e6-242">Просмотрев представление Отладка после этого изменения показывает, что:</span><span class="sxs-lookup"><span data-stu-id="c29e6-242">Looking at the debug view after this change shows that:</span></span>

- <span data-ttu-id="c29e6-243">Запись была помечена таким образом `Deleted` , что она будет удалена из базы данных при вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="c29e6-243">The post has been marked as `Deleted` such that it will be deleted from the database when SaveChanges is called.</span></span>
- <span data-ttu-id="c29e6-244">`Post.Blog`Для навигации по ссылке задано значение null ( `Blog: <null>` ).</span><span class="sxs-lookup"><span data-stu-id="c29e6-244">The `Post.Blog` reference navigation has been set to null (`Blog: <null>`).</span></span>
- <span data-ttu-id="c29e6-245">Запись была удалена из `Blog.Posts` навигации по коллекции ( `Posts: [{Id: 1}]` ).</span><span class="sxs-lookup"><span data-stu-id="c29e6-245">The post has been removed from `Blog.Posts` collection navigation (`Posts: [{Id: 1}]`).</span></span>

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Assets: <null>
  Posts: [{Id: 1}]
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
  Tags: []
Post {Id: 2} Deleted
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: <null>
  Tags: []
```

<span data-ttu-id="c29e6-246">Обратите внимание, что объект `Post.BlogId` остается неизменным, так как для него не может быть задано значение null.</span><span class="sxs-lookup"><span data-stu-id="c29e6-246">Notice that the `Post.BlogId` remains unchanged since for a required relationship it cannot be set to null.</span></span>

<span data-ttu-id="c29e6-247">Вызов метода SaveChanges приводит к удалению потерянной записи:</span><span class="sxs-lookup"><span data-stu-id="c29e6-247">Calling SaveChanges results in the orphaned post being deleted:</span></span>

```sql
-- Executed DbCommand (0ms) [Parameters=[@p0='2' (DbType = String)], CommandType='Text', CommandTimeout='30']
DELETE FROM "Posts"
WHERE "Id" = @p0;
SELECT changes();
```

#### <a name="delete-orphans-timing-and-re-parenting"></a><span data-ttu-id="c29e6-248">Удаление потерянных значений времени и повторного создания родительских объектов</span><span class="sxs-lookup"><span data-stu-id="c29e6-248">Delete orphans timing and re-parenting</span></span>

<span data-ttu-id="c29e6-249">По умолчанию пометка потерянных объектов как `Deleted` произойдет, как только будет [обнаружено](xref:core/change-tracking/change-detection)изменение связи.</span><span class="sxs-lookup"><span data-stu-id="c29e6-249">By default, marking orphans as `Deleted` happens as soon as the relationship change is [detected](xref:core/change-tracking/change-detection).</span></span> <span data-ttu-id="c29e6-250">Однако этот процесс можно отложить до фактического вызова SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="c29e6-250">However, this process can be delayed until SaveChanges is actually called.</span></span> <span data-ttu-id="c29e6-251">Это может быть полезно, чтобы избежать удаления потерянных сущностей, удаленных из одного участника или родителя, но при этом будет выполнено повторное создание родительского объекта с новым участником или родительским объектом перед вызовом метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="c29e6-251">This can be useful to avoid making orphans of entities that have been removed from one principal/parent, but will be re-parented with a new principal/parent before SaveChanges is called.</span></span> <span data-ttu-id="c29e6-252"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DeleteOrphansTiming?displayProperty=nameWithType> используется для задания этого времени.</span><span class="sxs-lookup"><span data-stu-id="c29e6-252"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DeleteOrphansTiming?displayProperty=nameWithType> is used to set this timing.</span></span> <span data-ttu-id="c29e6-253">Пример:</span><span class="sxs-lookup"><span data-stu-id="c29e6-253">For example:</span></span>

<!--
        context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;

        var post = vsBlog.Posts.Single(e => e.Title.StartsWith("Disassembly improvements"));
        vsBlog.Posts.Remove(post);

        context.ChangeTracker.DetectChanges();
        Console.WriteLine(context.ChangeTracker.DebugView.LongView);

        dotNetBlog.Posts.Add(post);

        context.ChangeTracker.DetectChanges();
        Console.WriteLine(context.ChangeTracker.DebugView.LongView);

        context.SaveChanges();
-->
[!code-csharp[Fixup_for_added_or_deleted_entities_5](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/RequiredRelationshipsSamples.cs?name=Fixup_for_added_or_deleted_entities_5)]

<span data-ttu-id="c29e6-254">После удаления записи из первой коллекции объект не помечается как `Deleted` в предыдущем примере.</span><span class="sxs-lookup"><span data-stu-id="c29e6-254">After removing the post from the first collection the object is not marked as `Deleted` as it was in the previous example.</span></span> <span data-ttu-id="c29e6-255">Вместо этого EF Core отслеживает, что связь является серьезной, несмотря на то, что _это обязательная связь_.</span><span class="sxs-lookup"><span data-stu-id="c29e6-255">Instead, EF Core is tracking that the relationship is severed _even though this is a required relationship_.</span></span> <span data-ttu-id="c29e6-256">(Значение FK считается нулевым, EF Core несмотря на то, что оно не может действительно иметь значение null, поскольку тип не допускает значения NULL.</span><span class="sxs-lookup"><span data-stu-id="c29e6-256">(The FK value is considered null by EF Core even though it cannot really be null because the type is not nullable.</span></span> <span data-ttu-id="c29e6-257">Это называется «концептуальным значением NULL».)</span><span class="sxs-lookup"><span data-stu-id="c29e6-257">This is known as a "conceptual null".)</span></span>

```output
Post {Id: 3} Modified
  Id: 3 PK
  BlogId: <null> FK Modified Originally 2
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: <null>
  Tags: []
```

<span data-ttu-id="c29e6-258">Вызов метода SaveChanges в это время приведет к удалению потерянной записи.</span><span class="sxs-lookup"><span data-stu-id="c29e6-258">Calling SaveChanges at this time would result in the orphaned post being deleted.</span></span> <span data-ttu-id="c29e6-259">Однако, если, как в приведенном выше примере, POST связан с новым блогом перед вызовом SaveChanges, то он будет исправлена соответствующим образом для нового блога и больше не считается потерянным:</span><span class="sxs-lookup"><span data-stu-id="c29e6-259">However, if as in the example above, post is associated with a new blog before SaveChanges is called, then it will be fixed up appropriately to that new blog and is no longer considered an orphan:</span></span>

```output
Post {Id: 3} Modified
  Id: 3 PK
  BlogId: 1 FK Modified Originally 2
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: {Id: 1}
  Tags: []
```

<span data-ttu-id="c29e6-260">Метод SaveChanges, вызываемый в этой точке, обновляет запись в базе данных, а не удаляет ее.</span><span class="sxs-lookup"><span data-stu-id="c29e6-260">SaveChanges called at this point will update the post in the database rather than deleting it.</span></span>

<span data-ttu-id="c29e6-261">Также можно отключить автоматическое удаление потерянных элементов.</span><span class="sxs-lookup"><span data-stu-id="c29e6-261">It is also possible to turn off automatic deletion of orphans.</span></span> <span data-ttu-id="c29e6-262">Это приведет к исключению, если метод SaveChanges вызывается при отслеживании потерянных объектов.</span><span class="sxs-lookup"><span data-stu-id="c29e6-262">This will result in an exception if SaveChanges is called while an orphan is being tracked.</span></span> <span data-ttu-id="c29e6-263">Например, следующий код:</span><span class="sxs-lookup"><span data-stu-id="c29e6-263">For example, this code:</span></span>

<!--
                var dotNetBlog = context.Blogs.Include(e => e.Posts).Single(e => e.Name == ".NET Blog");

                context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.Never;

                var post = dotNetBlog.Posts.Single(e => e.Title == "Announcing F# 5");
                dotNetBlog.Posts.Remove(post);

                context.SaveChanges(); // Throws
-->
[!code-csharp[Fixup_for_added_or_deleted_entities_6](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/RequiredRelationshipsSamples.cs?name=Fixup_for_added_or_deleted_entities_6)]

<span data-ttu-id="c29e6-264">Вызовет это исключение:</span><span class="sxs-lookup"><span data-stu-id="c29e6-264">Will throw this exception:</span></span>

> <span data-ttu-id="c29e6-265">System. InvalidOperationException: связь между сущностями "Blog" и "Post" со значением ключа "{Блогид: 1}" было разорвано, но связь либо помечена как обязательная, либо неявно является обязательной, поскольку внешний ключ не допускает значения NULL.</span><span class="sxs-lookup"><span data-stu-id="c29e6-265">System.InvalidOperationException: The association between entities 'Blog' and 'Post' with the key value '{BlogId: 1}' has been severed, but the relationship is either marked as required or is implicitly required because the foreign key is not nullable.</span></span> <span data-ttu-id="c29e6-266">Если зависимая или Дочерняя сущность должна быть удалена при серьезном отношении необходимой связи, настройте связь для использования каскадных удалений.</span><span class="sxs-lookup"><span data-stu-id="c29e6-266">If the dependent/child entity should be deleted when a required relationship is severed, configure the relationship to use cascade deletes.</span></span>

<span data-ttu-id="c29e6-267">Удаление потерянных и каскадных удалений может быть принудительно применено в любое время путем вызова <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.CascadeChanges?displayProperty=nameWithType> .</span><span class="sxs-lookup"><span data-stu-id="c29e6-267">Deletion of orphans, as well as cascade deletes, can be forced at any time by calling <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.CascadeChanges?displayProperty=nameWithType>.</span></span> <span data-ttu-id="c29e6-268">Если присвоить этому параметру значение времени удаления без сохранения, то все `Never` потерянные фрагменты не будут удаляться, пока не будет явно указана EF Core.</span><span class="sxs-lookup"><span data-stu-id="c29e6-268">Combining this with setting the delete orphan timing to `Never` will ensure orphans are never deleted unless EF Core is explicitly instructed to do so.</span></span>

### <a name="changing-a-reference-navigation"></a><span data-ttu-id="c29e6-269">Изменение навигации по ссылке</span><span class="sxs-lookup"><span data-stu-id="c29e6-269">Changing a reference navigation</span></span>

<span data-ttu-id="c29e6-270">Изменение навигации по ссылке для связи «один ко многим» оказывает тот же результат, что и изменение навигации по коллекции на другом конце связи.</span><span class="sxs-lookup"><span data-stu-id="c29e6-270">Changing the reference navigation of a one-to-many relationship has the same effect as changing the collection navigation on the other end of the relationship.</span></span> <span data-ttu-id="c29e6-271">Настройка навигации по ссылке для зависимого или дочернего элемента на null эквивалентна удалению сущности из структуры навигации по основному или родительскому элементу коллекции.</span><span class="sxs-lookup"><span data-stu-id="c29e6-271">Setting the reference navigation of dependent/child to null is equivalent to removing the entity from the collection navigation of the principal/parent.</span></span> <span data-ttu-id="c29e6-272">Все изменения в исправлениях и базах данных происходят, как описано в предыдущем разделе, в том числе при необходимости сделать сущность потерянной.</span><span class="sxs-lookup"><span data-stu-id="c29e6-272">All fixup and database changes happen as described in the previous section, including making the entity an orphan if the relationship is required.</span></span>

#### <a name="optional-one-to-one-relationships"></a><span data-ttu-id="c29e6-273">Необязательные связи "один к одному"</span><span class="sxs-lookup"><span data-stu-id="c29e6-273">Optional one-to-one relationships</span></span>

<span data-ttu-id="c29e6-274">Для связей «один к одному» изменение навигации по ссылке приводит к тому, что все предыдущие связи будут разорваны.</span><span class="sxs-lookup"><span data-stu-id="c29e6-274">For one-to-one relationships, changing a reference navigation causes any previous relationship to be severed.</span></span> <span data-ttu-id="c29e6-275">Для необязательных связей это означает, что значение FK для ранее связанного зависимого или дочернего элемента установлено в NULL.</span><span class="sxs-lookup"><span data-stu-id="c29e6-275">For optional relationships, this means that the FK value on the previously related dependent/child is set to null.</span></span> <span data-ttu-id="c29e6-276">Пример:</span><span class="sxs-lookup"><span data-stu-id="c29e6-276">For example:</span></span>

<!--
        using var context = new BlogsContext();

        var dotNetBlog = context.Blogs.Include(e => e.Assets).Single(e => e.Name == ".NET Blog");
        dotNetBlog.Assets = new BlogAssets();

        context.ChangeTracker.DetectChanges();
        Console.WriteLine(context.ChangeTracker.DebugView.LongView);

        context.SaveChanges();
-->
[!code-csharp[Fixup_for_added_or_deleted_entities_7](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Fixup_for_added_or_deleted_entities_7)]

<span data-ttu-id="c29e6-277">Представление Отладка перед вызовом SaveChanges показывает, что новые ресурсы заменили существующие ресурсы, которые теперь отмечены как `Modified` имеющие значение ключа FK, равное NULL `BlogAssets.BlogId` :</span><span class="sxs-lookup"><span data-stu-id="c29e6-277">The debug view before calling SaveChanges shows that the new assets has replaced the existing assets, which is now marked as `Modified` with a null `BlogAssets.BlogId` FK value:</span></span>

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Assets: {Id: -2147482629}
  Posts: []
BlogAssets {Id: -2147482629} Added
  Id: -2147482629 PK Temporary
  Banner: <null>
  BlogId: 1 FK
  Blog: {Id: 1}
BlogAssets {Id: 1} Modified
  Id: 1 PK
  Banner: <null>
  BlogId: <null> FK Modified Originally 1
  Blog: <null>
```

<span data-ttu-id="c29e6-278">Это приводит к обновлению и вставке при вызове SaveChanges:</span><span class="sxs-lookup"><span data-stu-id="c29e6-278">This results in an update and an insert when SaveChanges is called:</span></span>

```sql
-- Executed DbCommand (0ms) [Parameters=[@p1='1' (DbType = String), @p0=NULL], CommandType='Text', CommandTimeout='30']
UPDATE "Assets" SET "BlogId" = @p0
WHERE "Id" = @p1;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p2=NULL, @p3='1' (Nullable = true) (DbType = String)], CommandType='Text', CommandTimeout='30']
INSERT INTO "Assets" ("Banner", "BlogId")
VALUES (@p2, @p3);
SELECT "Id"
FROM "Assets"
WHERE changes() = 1 AND "rowid" = last_insert_rowid();
```

#### <a name="required-one-to-one-relationships"></a><span data-ttu-id="c29e6-279">Необходимые связи "один к одному"</span><span class="sxs-lookup"><span data-stu-id="c29e6-279">Required one-to-one relationships</span></span>

<span data-ttu-id="c29e6-280">Выполнение того же кода, что и в предыдущем примере, но на этот раз с требуемой связью "один к одному" показывается, что ранее связанная связь `BlogAssets` теперь помечена как `Deleted` , так как она становится потерянной после того, как новая система примет `BlogAssets` следующий вид:</span><span class="sxs-lookup"><span data-stu-id="c29e6-280">Running the same code as in the previous example, but this time with a required one-to-one relationship, shows that the previously associated `BlogAssets` is now marked as `Deleted`, since it becomes an orphan when the new `BlogAssets` takes its place:</span></span>

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Assets: {Id: -2147482639}
  Posts: []
BlogAssets {Id: -2147482639} Added
  Id: -2147482639 PK Temporary
  Banner: <null>
  BlogId: 1 FK
  Blog: {Id: 1}
BlogAssets {Id: 1} Deleted
  Id: 1 PK
  Banner: <null>
  BlogId: 1 FK
  Blog: <null>
```

<span data-ttu-id="c29e6-281">После этого вызывается удаление и вставка при вызове SaveChanges:</span><span class="sxs-lookup"><span data-stu-id="c29e6-281">This then results in an delete an and insert when SaveChanges is called:</span></span>

```sql
-- Executed DbCommand (0ms) [Parameters=[@p0='1' (DbType = String)], CommandType='Text', CommandTimeout='30']
DELETE FROM "Assets"
WHERE "Id" = @p0;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p1=NULL, @p2='1' (DbType = String)], CommandType='Text', CommandTimeout='30']
INSERT INTO "Assets" ("Banner", "BlogId")
VALUES (@p1, @p2);
SELECT "Id"
FROM "Assets"
WHERE changes() = 1 AND "rowid" = last_insert_rowid();
```

<span data-ttu-id="c29e6-282">Время пометки потерянных элементов как удаленных можно изменить так же, как показано для навигации по коллекциям и имеет те же эффекты.</span><span class="sxs-lookup"><span data-stu-id="c29e6-282">The timing of marking orphans as deleted can be changed in the same way as shown for collection navigations and has the same effects.</span></span>

### <a name="deleting-an-entity"></a><span data-ttu-id="c29e6-283">Удаление сущности</span><span class="sxs-lookup"><span data-stu-id="c29e6-283">Deleting an entity</span></span>

#### <a name="optional-relationships"></a><span data-ttu-id="c29e6-284">Необязательные связи</span><span class="sxs-lookup"><span data-stu-id="c29e6-284">Optional relationships</span></span>

<span data-ttu-id="c29e6-285">Если сущность помечена как `Deleted` , например, путем вызова <xref:Microsoft.EntityFrameworkCore.DbContext.Remove%2A?displayProperty=nameWithType> , то ссылки на удаленную сущность удаляются из переходов других сущностей.</span><span class="sxs-lookup"><span data-stu-id="c29e6-285">When an entity is marked as `Deleted`, for example by calling <xref:Microsoft.EntityFrameworkCore.DbContext.Remove%2A?displayProperty=nameWithType>, then references to the deleted entity are removed from the navigations of other entities.</span></span> <span data-ttu-id="c29e6-286">Для необязательных связей значения FK в зависимых сущностях устанавливаются в значение null.</span><span class="sxs-lookup"><span data-stu-id="c29e6-286">For optional relationships, the FK values in dependent entities are set to null.</span></span>

<span data-ttu-id="c29e6-287">Например, давайте пометив блог Visual Studio как `Deleted` :</span><span class="sxs-lookup"><span data-stu-id="c29e6-287">For example, let's mark the Visual Studio blog as `Deleted`:</span></span>

<!--
        using var context = new BlogsContext();

        var vsBlog = context.Blogs
            .Include(e => e.Posts)
            .Include(e => e.Assets)
            .Single(e => e.Name == "Visual Studio Blog");

        context.Remove(vsBlog);

        Console.WriteLine(context.ChangeTracker.DebugView.LongView);

        context.SaveChanges();
-->
[!code-csharp[Deleting_an_entity_1](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Deleting_an_entity_1)]

<span data-ttu-id="c29e6-288">[Просмотр представления отладки средства записи изменений](xref:core/change-tracking/debug-views) перед вызовом SaveChanges показывает:</span><span class="sxs-lookup"><span data-stu-id="c29e6-288">Looking at the [change tracker debug view](xref:core/change-tracking/debug-views) before calling SaveChanges shows:</span></span>

```output
Blog {Id: 2} Deleted
  Id: 2 PK
  Name: 'Visual Studio Blog'
  Assets: {Id: 2}
  Posts: [{Id: 3}, {Id: 4}]
BlogAssets {Id: 2} Modified
  Id: 2 PK
  Banner: <null>
  BlogId: <null> FK Modified Originally 2
  Blog: <null>
Post {Id: 3} Modified
  Id: 3 PK
  BlogId: <null> FK Modified Originally 2
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: <null>
  Tags: []
Post {Id: 4} Modified
  Id: 4 PK
  BlogId: <null> FK Modified Originally 2
  Content: 'Examine when database queries were executed and measure how ...'
  Title: 'Database Profiling with Visual Studio'
  Blog: <null>
  Tags: []
```

<span data-ttu-id="c29e6-289">Обратите внимание на указанные ниже моменты.</span><span class="sxs-lookup"><span data-stu-id="c29e6-289">Notice that:</span></span>

- <span data-ttu-id="c29e6-290">Блог помечен как `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="c29e6-290">The blog is marked as `Deleted`.</span></span>
- <span data-ttu-id="c29e6-291">Ресурсы, связанные с удаленным блогом, имеют нулевое значение FK ( `BlogId: <null> FK Modified Originally 2` ) и навигацию по ссылке NULL ( `Blog: <null>` ).</span><span class="sxs-lookup"><span data-stu-id="c29e6-291">The assets related to the deleted blog has a null FK value (`BlogId: <null> FK Modified Originally 2`) and a null reference navigation (`Blog: <null>`)</span></span>
- <span data-ttu-id="c29e6-292">Каждая запись, связанная с удаленным блогом, содержит значение FK со значением NULL ( `BlogId: <null> FK Modified Originally 2` ) и навигацию по ссылке NULL ( `Blog: <null>` ).</span><span class="sxs-lookup"><span data-stu-id="c29e6-292">Each post related to the deleted blog has a null FK value (`BlogId: <null> FK Modified Originally 2`) and a null reference navigation (`Blog: <null>`)</span></span>

#### <a name="required-relationships"></a><span data-ttu-id="c29e6-293">Обязательные связи</span><span class="sxs-lookup"><span data-stu-id="c29e6-293">Required relationships</span></span>

<span data-ttu-id="c29e6-294">Поведение адресной привязки для требуемых связей аналогично поведению для необязательных связей, за исключением того, что зависимые/дочерние сущности помечены как, `Deleted` так как они не могут существовать без принципала или родителя и должны быть удалены из базы данных при вызове метода SaveChanges, чтобы избежать исключения ссылочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="c29e6-294">The fixup behavior for required relationships is the same as for optional relationships except that the dependent/child entities are marked as `Deleted` since they cannot exist without a principal/parent and must be removed from the database when SaveChanges is called to avoid a referential constraint exception.</span></span> <span data-ttu-id="c29e6-295">Это называется каскадным удалением и является поведением по умолчанию в EF Core для необходимых связей.</span><span class="sxs-lookup"><span data-stu-id="c29e6-295">This is known as "cascade delete", and is the default behavior in EF Core for required relationships.</span></span> <span data-ttu-id="c29e6-296">Например, выполнение того же кода, что и в предыдущем примере, но при наличии обязательной связи приводит к представлению в следующем представлении отладки перед вызовом SaveChanges:</span><span class="sxs-lookup"><span data-stu-id="c29e6-296">For example, running the same code as in the previous example but with a required relationship results in the following debug view before SaveChanges is called:</span></span>

```output
Blog {Id: 2} Deleted
  Id: 2 PK
  Name: 'Visual Studio Blog'
  Assets: {Id: 2}
  Posts: [{Id: 3}, {Id: 4}]
BlogAssets {Id: 2} Deleted
  Id: 2 PK
  Banner: <null>
  BlogId: 2 FK
  Blog: {Id: 2}
Post {Id: 3} Deleted
  Id: 3 PK
  BlogId: 2 FK
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: {Id: 2}
  Tags: []
Post {Id: 4} Deleted
  Id: 4 PK
  BlogId: 2 FK
  Content: 'Examine when database queries were executed and measure how ...'
  Title: 'Database Profiling with Visual Studio'
  Blog: {Id: 2}
  Tags: []
```

<span data-ttu-id="c29e6-297">Как и ожидалось, зависимые элементы/потомки теперь помечены как `Deleted` .</span><span class="sxs-lookup"><span data-stu-id="c29e6-297">As expected, the dependents/children are now marked as `Deleted`.</span></span> <span data-ttu-id="c29e6-298">Однако обратите внимание, что переходы по удаленным сущностям _не_ изменились.</span><span class="sxs-lookup"><span data-stu-id="c29e6-298">However, notice that the navigations on the deleted entities have _not_ changed.</span></span> <span data-ttu-id="c29e6-299">Это может показаться странным, но позволяет избежать полного разделения удаленных графов сущностей путем очистки всех переходов.</span><span class="sxs-lookup"><span data-stu-id="c29e6-299">This may seem strange, but it avoids completely shredding a deleted graph of entities by clearing all navigations.</span></span> <span data-ttu-id="c29e6-300">Это значит, что блог, ресурс и записи по-прежнему формируют граф сущностей даже после удаления.</span><span class="sxs-lookup"><span data-stu-id="c29e6-300">That is, the blog, asset, and posts still form a graph of entities even after having been deleted.</span></span> <span data-ttu-id="c29e6-301">Это значительно упрощает отмену удаления графа сущностей, чем было в EF6, где был разбираться граф.</span><span class="sxs-lookup"><span data-stu-id="c29e6-301">This makes it much easier to un-delete a graph of entities than was the case in EF6 where the graph was shredded.</span></span>

#### <a name="cascade-delete-timing-and-re-parenting"></a><span data-ttu-id="c29e6-302">Каскадное удаление и повторное наследование</span><span class="sxs-lookup"><span data-stu-id="c29e6-302">Cascade delete timing and re-parenting</span></span>

<span data-ttu-id="c29e6-303">По умолчанию каскадное удаление происходит сразу после того, как родительский объект или участник помечается как `Deleted` .</span><span class="sxs-lookup"><span data-stu-id="c29e6-303">By default, cascade delete happens as soon as the parent/principal is marked as `Deleted`.</span></span> <span data-ttu-id="c29e6-304">Это то же самое, что и при удалении потерянных объектов, как описано выше.</span><span class="sxs-lookup"><span data-stu-id="c29e6-304">This is the same as for deleting orphans, as described previously.</span></span> <span data-ttu-id="c29e6-305">Как и при удалении потерянных объектов, этот процесс можно отложить до вызова метода SaveChanges или даже полностью отключить, установив <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.CascadeDeleteTiming?displayProperty=nameWithType> соответствующий параметр.</span><span class="sxs-lookup"><span data-stu-id="c29e6-305">As with deleting orphans, this process can be delayed until SaveChanges is called, or even disabled entirely, by setting <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.CascadeDeleteTiming?displayProperty=nameWithType> appropriately.</span></span> <span data-ttu-id="c29e6-306">Это полезно так же, как и удаление потерянных объектов, в том числе для повторного наследования потомков или зависимых элементов после удаления субъекта-родителя.</span><span class="sxs-lookup"><span data-stu-id="c29e6-306">This is useful in the same way as it is for deleting orphans, including for re-parenting children/dependents after deletion of a principal/parent.</span></span>

<span data-ttu-id="c29e6-307">Каскадные удаления, а также удаление потерянных объектов могут быть принудительно запущены в любое время путем вызова <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.CascadeChanges?displayProperty=nameWithType> .</span><span class="sxs-lookup"><span data-stu-id="c29e6-307">Cascade deletes, as well as deleting orphans, can be forced at any time by calling <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.CascadeChanges?displayProperty=nameWithType>.</span></span> <span data-ttu-id="c29e6-308">Если присвоить этому параметру значение времени каскадного удаления, `Never` оно не будет выполняться каскадным удалением, если только EF Core явно не получит указания.</span><span class="sxs-lookup"><span data-stu-id="c29e6-308">Combining this with setting the cascade delete timing to `Never` will ensure cascade deletes never happen unless EF Core is explicitly instructed to do so.</span></span>

> [!TIP]
> <span data-ttu-id="c29e6-309">Концепции каскадного удаления и удаления потерянных объектов тесно взаимосвязаны.</span><span class="sxs-lookup"><span data-stu-id="c29e6-309">Cascade delete and deleting orphans are closely related.</span></span> <span data-ttu-id="c29e6-310">В обоих случаях при разрыве связи с основной или родительской сущностью ее зависимые и дочерние сущности удаляются.</span><span class="sxs-lookup"><span data-stu-id="c29e6-310">Both result in deleting dependent/child entities when the relationship to their required principal/parent is severed.</span></span> <span data-ttu-id="c29e6-311">При каскадном удалении это происходит вследствие удаления самой основной или родительской сущности.</span><span class="sxs-lookup"><span data-stu-id="c29e6-311">For cascade delete, this severing happens because the principal/parent is itself deleted.</span></span> <span data-ttu-id="c29e6-312">При удалении потерянных объектов основная или родительская сущность по-прежнему существует, но при этом больше не связана с зависимыми и дочерними сущностями.</span><span class="sxs-lookup"><span data-stu-id="c29e6-312">For orphans, the principal/parent entity still exists, but is no longer related to the dependent/child entities.</span></span>

## <a name="many-to-many-relationships"></a><span data-ttu-id="c29e6-313">Связи "много ко многим".</span><span class="sxs-lookup"><span data-stu-id="c29e6-313">Many-to-many relationships</span></span>

<span data-ttu-id="c29e6-314">Связи «многие ко многим» в EF Core реализуются с помощью сущности JOIN.</span><span class="sxs-lookup"><span data-stu-id="c29e6-314">Many-to-many relationships in EF Core are implemented using a join entity.</span></span> <span data-ttu-id="c29e6-315">Каждая сторона связи "многие ко многим" связана с этой сущностью объединения со связью "один ко многим".</span><span class="sxs-lookup"><span data-stu-id="c29e6-315">Each side the many-to-many relationship is related to this join entity with a one-to-many relationship.</span></span> <span data-ttu-id="c29e6-316">До EF Core 5,0 эта сущность объединения должна быть явно определена и сопоставлена.</span><span class="sxs-lookup"><span data-stu-id="c29e6-316">Before EF Core 5.0, this join entity had to explicitly defined and mapped.</span></span> <span data-ttu-id="c29e6-317">Начиная с EF Core 5,0, его можно создать неявно и скрыть.</span><span class="sxs-lookup"><span data-stu-id="c29e6-317">Starting with EF Core 5.0, it can be created implicitly and hidden.</span></span> <span data-ttu-id="c29e6-318">Однако в обоих случаях базовое поведение одинаково.</span><span class="sxs-lookup"><span data-stu-id="c29e6-318">However, in both cases the underlying behavior is the same.</span></span> <span data-ttu-id="c29e6-319">Сначала мы рассмотрим базовое поведение, чтобы понять, как работает отслеживание связей "многие ко многим".</span><span class="sxs-lookup"><span data-stu-id="c29e6-319">We will look at this underlying behavior first to understand how tracking of many-to-many relationships works.</span></span>

### <a name="how-many-to-many-relationships-work"></a><span data-ttu-id="c29e6-320">Как работают связи «многие ко многим»</span><span class="sxs-lookup"><span data-stu-id="c29e6-320">How many-to-many relationships work</span></span>

<span data-ttu-id="c29e6-321">Рассмотрим эту модель EF Core, которая создает связь "многие ко многим" между записями и тегами, используя явно определенный тип сущности Join:</span><span class="sxs-lookup"><span data-stu-id="c29e6-321">Consider this EF Core model that creates a many-to-many relationship between posts and tags using an explicitly defined join entity type:</span></span>

<!--
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int? BlogId { get; set; }
        public Blog Blog { get; set; }

        public IList<PostTag> PostTags { get; } = new List<PostTag>(); // Collection navigation
    }

    public class Tag
    {
        public int Id { get; set; }
        public string Text { get; set; }

        public IList<PostTag> PostTags { get; } = new List<PostTag>(); // Collection navigation
    }

    public class PostTag
    {
        public int PostId { get; set; } // First part of composite PK; FK to Post
        public int TagId { get; set; } // Second part of composite PK; FK to Tag

        public Post Post { get; set; } // Reference navigation
        public Tag Tag { get; set; } // Reference navigation
    }
    -->
[!code-csharp[Model](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntitySamples.cs?name=Model)]

<span data-ttu-id="c29e6-322">Обратите внимание, что `PostTag` тип сущности Join содержит два свойства внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="c29e6-322">Notice that the `PostTag` join entity type contains two foreign key properties.</span></span> <span data-ttu-id="c29e6-323">В этой модели для того, чтобы запись была связана с тегом, должна существовать сущность Посттаг JOIN, где `PostTag.PostId` значение внешнего ключа соответствует `Post.Id` значению первичного ключа и где `PostTag.TagId` значение внешнего ключа соответствует `Tag.Id` значению первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="c29e6-323">In this model, for a post to be related to a tag, there must be a PostTag join entity where the `PostTag.PostId` foreign key value matches the `Post.Id` primary key value, and where the `PostTag.TagId` foreign key value matches the `Tag.Id` primary key value.</span></span> <span data-ttu-id="c29e6-324">Пример:</span><span class="sxs-lookup"><span data-stu-id="c29e6-324">For example:</span></span>

<!--
            using var context = new BlogsContext();

            var post = context.Posts.Single(e => e.Id == 3);
            var tag = context.Tags.Single(e => e.Id == 1);

            context.Add(new PostTag { PostId = post.Id, TagId = tag.Id });

            Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Many_to_many_relationships_1](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntitySamples.cs?name=Many_to_many_relationships_1)]

<span data-ttu-id="c29e6-325">[Просмотр представления отладки "средство записи изменений](xref:core/change-tracking/debug-views) " после выполнения этого кода показывает, что запись и тег связаны с новой `PostTag` сущностью Join:</span><span class="sxs-lookup"><span data-stu-id="c29e6-325">Looking at the [change tracker debug view](xref:core/change-tracking/debug-views) after running this code shows that the post and tag are related by the new `PostTag` join entity:</span></span>

```output
Post {Id: 3} Unchanged
  Id: 3 PK
  BlogId: 2 FK
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: <null>
  PostTags: [{PostId: 3, TagId: 1}]
PostTag {PostId: 3, TagId: 1} Added
  PostId: 3 PK FK
  TagId: 1 PK FK
  Post: {Id: 3}
  Tag: {Id: 1}
Tag {Id: 1} Unchanged
  Id: 1 PK
  Text: '.NET'
  PostTags: [{PostId: 3, TagId: 1}]
```

<span data-ttu-id="c29e6-326">Обратите внимание, что структуры навигации по коллекции `Post` и были `Tag` исправлены, как и ссылки на `PostTag` .</span><span class="sxs-lookup"><span data-stu-id="c29e6-326">Notice that the collection navigations on `Post` and `Tag` have been fixed up, as have the reference navigations on `PostTag`.</span></span> <span data-ttu-id="c29e6-327">Этими связями можно управлять с помощью переходов вместо значений FK, как и во всех предыдущих примерах.</span><span class="sxs-lookup"><span data-stu-id="c29e6-327">These relationships can be manipulated by navigations instead of FK values, just as in all the preceding examples.</span></span> <span data-ttu-id="c29e6-328">Например, приведенный выше код можно изменить, чтобы добавить связь, задав ссылки для навигации по ссылке в сущности JOIN.</span><span class="sxs-lookup"><span data-stu-id="c29e6-328">For example, the code above can be modified to add the relationship by setting the reference navigations on the join entity:</span></span>

<!--
            context.Add(new PostTag { Post = post, Tag = tag });
-->
[!code-csharp[Many_to_many_relationships_2](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntitySamples.cs?name=Many_to_many_relationships_2)]

<span data-ttu-id="c29e6-329">Это приводит к тому же изменению для внешние ключи и переходов, как в предыдущем примере.</span><span class="sxs-lookup"><span data-stu-id="c29e6-329">This results in exactly the same change to FKs and navigations as in the previous example.</span></span>

### <a name="skip-navigations"></a><span data-ttu-id="c29e6-330">Пропустить навигацию</span><span class="sxs-lookup"><span data-stu-id="c29e6-330">Skip navigations</span></span>

> [!NOTE]
> <span data-ttu-id="c29e6-331">Переходы по пропуску были введены в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="c29e6-331">Skip navigations were introduced in EF Core 5.0.</span></span>

<span data-ttu-id="c29e6-332">Работа с таблицей соединений вручную может быть утомительной.</span><span class="sxs-lookup"><span data-stu-id="c29e6-332">Manipulating the join table manually can be cumbersome.</span></span> <span data-ttu-id="c29e6-333">Начиная с EF Core 5,0, связи "многие ко многим" можно манипулировать напрямую с помощью специальных переходов по коллекции, которые пропускают сущность JOIN.</span><span class="sxs-lookup"><span data-stu-id="c29e6-333">Starting with EF Core 5.0, many-to-many relationships can be manipulated directly using special collection navigations that "skip over" the join entity.</span></span> <span data-ttu-id="c29e6-334">Например, в приведенной выше модели можно добавить два перехода с пропуском. один из записей в теги, а другой — от тега к записям:</span><span class="sxs-lookup"><span data-stu-id="c29e6-334">For example, two skip navigations can be added to the model above; one from Post to Tags, and the other from Tag to Posts:</span></span>

<!--
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int? BlogId { get; set; }
        public Blog Blog { get; set; }

        public IList<Tag> Tags { get; } = new List<Tag>(); // Skip collection navigation
        public IList<PostTag> PostTags { get; } = new List<PostTag>(); // Collection navigation
    }

    public class Tag
    {
        public int Id { get; set; }
        public string Text { get; set; }

        public IList<Post> Posts { get; } = new List<Post>(); // Skip collection navigation
        public IList<PostTag> PostTags { get; } = new List<PostTag>(); // Collection navigation
    }

    public class PostTag
    {
        public int PostId { get; set; } // First part of composite PK; FK to Post
        public int TagId { get; set; } // Second part of composite PK; FK to Tag

        public Post Post { get; set; } // Reference navigation
        public Tag Tag { get; set; } // Reference navigation
    }
-->
[!code-csharp[Model](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityAndSkipsSamples.cs?name=Model)]

<span data-ttu-id="c29e6-335">Для этой связи "многие ко многим" требуется следующая конфигурация, чтобы убедиться, что Навигация по пропуску и обычные переходы используются для одной и той же связи "многие ко многим":</span><span class="sxs-lookup"><span data-stu-id="c29e6-335">This many-to-many relationship requires the following configuration to ensure the skip navigations and normal navigations are all used for the same many-to-many relationship:</span></span>

<!--
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Post>()
                .HasMany(p => p.Tags)
                .WithMany(p => p.Posts)
                .UsingEntity<PostTag>(
                    j => j.HasOne(t => t.Tag).WithMany(p => p.PostTags),
                    j => j.HasOne(t => t.Post).WithMany(p => p.PostTags));
        }
-->
[!code-csharp[OnModelCreating](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityAndSkipsSamples.cs?name=OnModelCreating)]

<span data-ttu-id="c29e6-336">Дополнительные сведения о сопоставлении отношений «многие ко многим» см. в разделе [связи](xref:core/modeling/relationships) .</span><span class="sxs-lookup"><span data-stu-id="c29e6-336">See [Relationships](xref:core/modeling/relationships) for more information on mapping many-to-many relationships.</span></span>

<span data-ttu-id="c29e6-337">Пропуски переходов пропускаются и ведут себя как обычные навигационные коллекции.</span><span class="sxs-lookup"><span data-stu-id="c29e6-337">Skip navigations look and behave like normal collection navigations.</span></span> <span data-ttu-id="c29e6-338">Однако способ их работы с внешними ключами отличается.</span><span class="sxs-lookup"><span data-stu-id="c29e6-338">However, the way they work with foreign key values is different.</span></span> <span data-ttu-id="c29e6-339">Давайте связав запись с тегом, но на этот раз используя навигацию Skip:</span><span class="sxs-lookup"><span data-stu-id="c29e6-339">Let's associate a post with a tag, but this time using a skip navigation:</span></span>

<!--
            using var context = new BlogsContext();

            var post = context.Posts.Single(e => e.Id == 3);
            var tag = context.Tags.Single(e => e.Id == 1);

            post.Tags.Add(tag);

            context.ChangeTracker.DetectChanges();
            Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Many_to_many_relationships_3](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityAndSkipsSamples.cs?name=Many_to_many_relationships_3)]

<span data-ttu-id="c29e6-340">Обратите внимание, что этот код не использует сущность JOIN.</span><span class="sxs-lookup"><span data-stu-id="c29e6-340">Notice that this code doesn't use the join entity.</span></span> <span data-ttu-id="c29e6-341">Вместо этого он просто добавляет сущность в коллекцию навигации точно так же, как если бы это была связь «один ко многим».</span><span class="sxs-lookup"><span data-stu-id="c29e6-341">It instead just adds an entity to a navigation collection in the same way as would be done if this were a one-to-many relationship.</span></span> <span data-ttu-id="c29e6-342">Результирующее представление отладки, по сути, такое же, как и раньше:</span><span class="sxs-lookup"><span data-stu-id="c29e6-342">The resulting debug view is essentially the same as before:</span></span>

```output
Post {Id: 3} Unchanged
  Id: 3 PK
  BlogId: 2 FK
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: <null>
  PostTags: [{PostId: 3, TagId: 1}]
  Tags: [{Id: 1}]
PostTag {PostId: 3, TagId: 1} Added
  PostId: 3 PK FK
  TagId: 1 PK FK
  Post: {Id: 3}
  Tag: {Id: 1}
Tag {Id: 1} Unchanged
  Id: 1 PK
  Text: '.NET'
  PostTags: [{PostId: 3, TagId: 1}]
  Posts: [{Id: 3}]
```

<span data-ttu-id="c29e6-343">Обратите внимание, что экземпляр `PostTag` сущности Join был создан автоматически с ЗНАЧЕНИЯМИ FK, для которых заданы значения ключа PK тега и POST, которые теперь связаны.</span><span class="sxs-lookup"><span data-stu-id="c29e6-343">Notice that an instance of the `PostTag` join entity was created automatically with FK values set to the PK values of the tag and post that are now associated.</span></span> <span data-ttu-id="c29e6-344">Все обычные переходы по ссылке и коллекции были исправлены в соответствии с этими значениями FK.</span><span class="sxs-lookup"><span data-stu-id="c29e6-344">All the normal reference and collection navigations have been fixed up to match these FK values.</span></span> <span data-ttu-id="c29e6-345">Кроме того, поскольку эта модель содержит переходы с пропуском, они также были исправлены.</span><span class="sxs-lookup"><span data-stu-id="c29e6-345">Also, since this model contains skip navigations, these have also been fixed up.</span></span> <span data-ttu-id="c29e6-346">В частности, несмотря на то, что мы добавили тег к `Post.Tags` навигации Skip, `Tag.Posts` обратная Навигация по другой стороне этой связи также была исправлена, чтобы содержать связанную запись.</span><span class="sxs-lookup"><span data-stu-id="c29e6-346">Specifically, even though we added the tag to the `Post.Tags` skip navigation, the `Tag.Posts` inverse skip navigation on the other side of this relationship has also been fixed up to contain the associated post.</span></span>

<span data-ttu-id="c29e6-347">Стоит отметить, что базовые связи "многие ко многим" все еще можно манипулировать напрямую, даже если пропускная Навигация пропускается сверху.</span><span class="sxs-lookup"><span data-stu-id="c29e6-347">It is worth noting that the underlying many-to-many relationships can still be manipulated directly even when skip navigations have been layered on top.</span></span> <span data-ttu-id="c29e6-348">Например, тег и POST могут быть связаны, как мы делали до представления пропускаемых переходов:</span><span class="sxs-lookup"><span data-stu-id="c29e6-348">For example, the tag and Post could be associated as we did before introducing skip navigations:</span></span>

<!--
            context.Add(new PostTag { Post = post, Tag = tag });
-->
[!code-csharp[Many_to_many_relationships_4](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityAndSkipsSamples.cs?name=Many_to_many_relationships_4)]

<span data-ttu-id="c29e6-349">Или используйте значения FK:</span><span class="sxs-lookup"><span data-stu-id="c29e6-349">Or using FK values:</span></span>

<!--
            context.Add(new PostTag { PostId = post.Id, TagId = tag.Id });
-->
[!code-csharp[Many_to_many_relationships_5](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityAndSkipsSamples.cs?name=Many_to_many_relationships_5)]

<span data-ttu-id="c29e6-350">Это по-прежнему приведет к правильному исправлению переходов по пропускам, что приведет к тому же результату представления отладки, что и в предыдущем примере.</span><span class="sxs-lookup"><span data-stu-id="c29e6-350">This will still result in the skip navigations being fixed up correctly, resulting in the same debug view output as in the previous example.</span></span>

### <a name="skip-navigations-only"></a><span data-ttu-id="c29e6-351">Пропустить только навигацию</span><span class="sxs-lookup"><span data-stu-id="c29e6-351">Skip navigations only</span></span>

<span data-ttu-id="c29e6-352">В предыдущем разделе мы добавили пропустить навигацию _в дополнение к_ полному определению двух базовых связей «один ко многим».</span><span class="sxs-lookup"><span data-stu-id="c29e6-352">In the previous section we added skip navigations _in addition to_ fully defining the two underlying one-to-many relationships.</span></span> <span data-ttu-id="c29e6-353">Это полезно для иллюстрации того, что происходит со значениями FK, но часто не требуется.</span><span class="sxs-lookup"><span data-stu-id="c29e6-353">This is useful to illustrate what happens to FK values, but is often unnecessary.</span></span> <span data-ttu-id="c29e6-354">Вместо этого связь «многие ко многим» может быть определена только с помощью _навигации по пропустить_.</span><span class="sxs-lookup"><span data-stu-id="c29e6-354">Instead, the many-to-many relationship can be defined using _only skip navigations_.</span></span> <span data-ttu-id="c29e6-355">Таким способом отношение «многие ко многим» определяется в модели в самом верху этого документа.</span><span class="sxs-lookup"><span data-stu-id="c29e6-355">This is how the many-to-many relationship is defined in the model at the very top of this document.</span></span> <span data-ttu-id="c29e6-356">С помощью этой модели можно снова связать запись и тег, добавив запись в `Tag.Posts` навигацию по пропуску (или, как вариант, добавить тег в `Post.Tags` навигацию Skip):</span><span class="sxs-lookup"><span data-stu-id="c29e6-356">Using this model, we can again associate a Post and a Tag by adding a post to the `Tag.Posts` skip navigation (or, alternately, adding a tag to the `Post.Tags` skip navigation):</span></span>

<!--
            using var context = new BlogsContext();

            var post = context.Posts.Single(e => e.Id == 3);
            var tag = context.Tags.Single(e => e.Id == 1);

            post.Tags.Add(tag);

            context.ChangeTracker.DetectChanges();
            Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Many_to_many_relationships_6](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Many_to_many_relationships_6)]

<span data-ttu-id="c29e6-357">Просмотр представления отладки после внесения этого изменения показывает, что EF Core создал экземпляр `Dictionary<string, object>` для представления сущности JOIN.</span><span class="sxs-lookup"><span data-stu-id="c29e6-357">Looking at the debug view after making this change reveals that EF Core has created an instance of `Dictionary<string, object>` to represent the join entity.</span></span> <span data-ttu-id="c29e6-358">Эта сущность Join содержит как `PostsId` , так и `TagsId` Свойства внешнего ключа, которые были заданы в соответствии со значениями PK, которые связаны с разделом POST и тегом.</span><span class="sxs-lookup"><span data-stu-id="c29e6-358">This join entity contains both `PostsId` and `TagsId` foreign key properties which have been set to match the PK values of the post and tag that are associated.</span></span>

```output
Post {Id: 3} Unchanged
  Id: 3 PK
  BlogId: 2 FK
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: <null>
  Tags: [{Id: 1}]
Tag {Id: 1} Unchanged
  Id: 1 PK
  Text: '.NET'
  Posts: [{Id: 3}]
PostTag (Dictionary<string, object>) {PostsId: 3, TagsId: 1} Added
  PostsId: 3 PK FK
  TagsId: 1 PK FK
```

<span data-ttu-id="c29e6-359">Дополнительные [](xref:core/modeling/relationships) сведения о неявных сущностях Join и использовании `Dictionary<string, object>` типов сущностей см. в разделе связи.</span><span class="sxs-lookup"><span data-stu-id="c29e6-359">See [Relationships](xref:core/modeling/relationships) for more information about implicit join entities and the use of `Dictionary<string, object>` entity types.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c29e6-360">Тип CLR, используемый для объединения типов сущностей по соглашению, может измениться в будущих выпусках для повышения производительности.</span><span class="sxs-lookup"><span data-stu-id="c29e6-360">The CLR type used for join entity types by convention may change in future releases to improve performance.</span></span> <span data-ttu-id="c29e6-361">Не зависят от типа объединения, `Dictionary<string, object>` если только он не был явно настроен.</span><span class="sxs-lookup"><span data-stu-id="c29e6-361">Do not depend on the join type being `Dictionary<string, object>` unless this has been explicitly configured.</span></span>

### <a name="join-entities-with-payloads"></a><span data-ttu-id="c29e6-362">Соединение сущностей с полезными данными</span><span class="sxs-lookup"><span data-stu-id="c29e6-362">Join entities with payloads</span></span>

<span data-ttu-id="c29e6-363">Пока все примеры использовали тип сущности Join (явный или неявный), который содержит только два свойства внешнего ключа, необходимые для связи «многие ко многим».</span><span class="sxs-lookup"><span data-stu-id="c29e6-363">So far all the examples have used a join entity type (whether explicit or implicit) that contains only the two foreign key properties needed for the many-to-many relationship.</span></span> <span data-ttu-id="c29e6-364">Ни одно из этих значений FK не должно быть явно задано приложением при управлении связями, поскольку их значения берутся из свойств первичного ключа связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="c29e6-364">Neither of these FK values need to be explicitly set by the application when manipulating relationships because their values come from the primary key properties of the related entities.</span></span> <span data-ttu-id="c29e6-365">Это позволяет EF Core создавать экземпляры сущности JOIN без отсутствующих данных.</span><span class="sxs-lookup"><span data-stu-id="c29e6-365">This allows EF Core to create instances of the join entity without missing data.</span></span>

#### <a name="payloads-with-generated-values"></a><span data-ttu-id="c29e6-366">Полезные данные с созданными значениями</span><span class="sxs-lookup"><span data-stu-id="c29e6-366">Payloads with generated values</span></span>

<span data-ttu-id="c29e6-367">EF Core поддерживает добавление дополнительных свойств к типу сущности JOIN.</span><span class="sxs-lookup"><span data-stu-id="c29e6-367">EF Core supports adding additional properties to the join entity type.</span></span> <span data-ttu-id="c29e6-368">Это называется предоставлением сущности для объединения полезных данных.</span><span class="sxs-lookup"><span data-stu-id="c29e6-368">This is known as giving the join entity a "payload".</span></span> <span data-ttu-id="c29e6-369">Например, добавим `TaggedOn` свойство в `PostTag` сущность Join:</span><span class="sxs-lookup"><span data-stu-id="c29e6-369">For example, let's add `TaggedOn` property to the `PostTag` join entity:</span></span>

<!--
    public class PostTag
    {
        public int PostId { get; set; } // Foreign key to Post
        public int TagId { get; set; } // Foreign key to Tag

        public DateTime TaggedOn { get; set; } // Payload
    }
-->
[!code-csharp[Model](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityWithPayloadSamples.cs?name=Model)]

<span data-ttu-id="c29e6-370">Это свойство полезной нагрузки не будет задано, когда EF Core создает экземпляр сущности JOIN.</span><span class="sxs-lookup"><span data-stu-id="c29e6-370">This payload property will not be set when EF Core creates a join entity instance.</span></span> <span data-ttu-id="c29e6-371">Наиболее распространенный способ решения этой проблемы — использование свойств полезных данных с автоматически формируемыми значениями.</span><span class="sxs-lookup"><span data-stu-id="c29e6-371">The most common way to deal with this is to use payload properties with automatically generated values.</span></span> <span data-ttu-id="c29e6-372">Например, `TaggedOn` свойство может быть настроено для использования создаваемой хранилищем метки времени при вставке каждой новой сущности:</span><span class="sxs-lookup"><span data-stu-id="c29e6-372">For example, the `TaggedOn` property can be configured to use a store-generated timestamp when each new entity is inserted:</span></span>

<!--
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Post>()
                .HasMany(p => p.Tags)
                .WithMany(p => p.Posts)
                .UsingEntity<PostTag>(
                    j => j.HasOne<Tag>().WithMany(),
                    j => j.HasOne<Post>().WithMany(),
                    j => j.Property(e => e.TaggedOn).HasDefaultValueSql("CURRENT_TIMESTAMP"));
        }
-->
[!code-csharp[OnModelCreating](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityWithPayloadSamples.cs?name=OnModelCreating)]

<span data-ttu-id="c29e6-373">Теперь запись может быть помечена таким же образом, как и раньше:</span><span class="sxs-lookup"><span data-stu-id="c29e6-373">A post can now be tagged in the same way as before:</span></span>

<!--
            using var context = new BlogsContext();

            var post = context.Posts.Single(e => e.Id == 3);
            var tag = context.Tags.Single(e => e.Id == 1);

            post.Tags.Add(tag);

            context.SaveChanges();

            Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Many_to_many_relationships_7](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityWithPayloadSamples.cs?name=Many_to_many_relationships_7)]

<span data-ttu-id="c29e6-374">При [просмотре представления отладки средства записи изменений](xref:core/change-tracking/debug-views) после вызова SaveChanges показывается, что свойство полезной нагрузки установлено соответствующим образом:</span><span class="sxs-lookup"><span data-stu-id="c29e6-374">Looking at the [change tracker debug view](xref:core/change-tracking/debug-views) after calling SaveChanges shows that the payload property has been set appropriately:</span></span>

```output
Post {Id: 3} Unchanged
  Id: 3 PK
  BlogId: 2 FK
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: <null>
  Tags: [{Id: 1}]
PostTag {PostId: 3, TagId: 1} Unchanged
  PostId: 3 PK FK
  TagId: 1 PK FK
  TaggedOn: '12/29/2020 8:13:21 PM'
Tag {Id: 1} Unchanged
  Id: 1 PK
  Text: '.NET'
  Posts: [{Id: 3}]
```

#### <a name="explicitly-setting-payload-values"></a><span data-ttu-id="c29e6-375">Явное задание значений полезных данных</span><span class="sxs-lookup"><span data-stu-id="c29e6-375">Explicitly setting payload values</span></span>

<span data-ttu-id="c29e6-376">Далее в предыдущем примере мы добавим свойство полезной нагрузки, которое не использует автоматически созданное значение:</span><span class="sxs-lookup"><span data-stu-id="c29e6-376">Following on from the previous example, let's add a payload property that does not use an automatically generated value:</span></span>

<!--
    public class PostTag
    {
        public int PostId { get; set; } // First part of composite PK; FK to Post
        public int TagId { get; set; } // Second part of composite PK; FK to Tag

        public DateTime TaggedOn { get; set; } // Auto-generated payload property
        public string TaggedBy { get; set; } // Not-generated payload property
    }
-->
[!code-csharp[Model](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityWithStringPayloadSamples.cs?name=Model)]

<span data-ttu-id="c29e6-377">Теперь запись может быть помечена таким же образом, как и раньше, и сущность JOIN будет по-прежнему создана автоматически.</span><span class="sxs-lookup"><span data-stu-id="c29e6-377">A post can now be tagged in the same way as before, and the join entity will still be created automatically.</span></span> <span data-ttu-id="c29e6-378">Доступ к этой сущности можно получить с помощью одного из механизмов, описанных в статье [доступ к отслеживающим сущностям](xref:core/change-tracking/entity-entries).</span><span class="sxs-lookup"><span data-stu-id="c29e6-378">This entity can then be accessed using one of the mechanisms described in [Accessing Tracked Entities](xref:core/change-tracking/entity-entries).</span></span> <span data-ttu-id="c29e6-379">Например, приведенный ниже код использует <xref:Microsoft.EntityFrameworkCore.DbSet%601.Find%2A?displayProperty=nameWithType> для доступа к экземпляру сущности Join:</span><span class="sxs-lookup"><span data-stu-id="c29e6-379">For example, the code below uses <xref:Microsoft.EntityFrameworkCore.DbSet%601.Find%2A?displayProperty=nameWithType> to access the join entity instance:</span></span>

<!--
            using var context = new BlogsContext();

            var post = context.Posts.Single(e => e.Id == 3);
            var tag = context.Tags.Single(e => e.Id == 1);

            post.Tags.Add(tag);

            context.ChangeTracker.DetectChanges();

            var joinEntity = context.Set<PostTag>().Find(post.Id, tag.Id);

            joinEntity.TaggedBy = "ajcvickers";

            context.SaveChanges();

            Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Many_to_many_relationships_8](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityWithStringPayloadSamples.cs?name=Many_to_many_relationships_8)]

<span data-ttu-id="c29e6-380">После того как сущность JOIN будет найдена, ее можно обменять обычным способом — в этом примере — задать `TaggedBy` свойство полезной нагрузки перед вызовом метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="c29e6-380">Once the join entity has been located it can be manipulated in the normal way--in this example, to set the `TaggedBy` payload property before calling SaveChanges.</span></span>

> [!NOTE]
> <span data-ttu-id="c29e6-381">Обратите внимание, что <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges?displayProperty=nameWithType> здесь требуется вызов, чтобы предоставить EF Core возможность обнаружить изменение свойства навигации и создать экземпляр сущности Join перед `Find` использованием.</span><span class="sxs-lookup"><span data-stu-id="c29e6-381">Note that a call to <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges?displayProperty=nameWithType> is required here to give EF Core a chance to detect the navigation property change and create the join entity instance before `Find` is used.</span></span> <span data-ttu-id="c29e6-382">Дополнительные сведения см. в разделе [Обнаружение изменений и уведомления](xref:core/change-tracking/change-detection) .</span><span class="sxs-lookup"><span data-stu-id="c29e6-382">See [Change Detection and Notifications](xref:core/change-tracking/change-detection) for more information.</span></span>

<span data-ttu-id="c29e6-383">Кроме того, сущность Join можно создать явным образом, чтобы связать запись с тегом.</span><span class="sxs-lookup"><span data-stu-id="c29e6-383">Alternately, the join entity can be created explicitly to associate a post with a tag.</span></span> <span data-ttu-id="c29e6-384">Пример:</span><span class="sxs-lookup"><span data-stu-id="c29e6-384">For example:</span></span>

<!--
            using var context = new BlogsContext();

            var post = context.Posts.Single(e => e.Id == 3);
            var tag = context.Tags.Single(e => e.Id == 1);

            context.Add(new PostTag()
            {
                PostId = post.Id,
                TagId = tag.Id,
                TaggedBy = "ajcvickers"
            });

            context.SaveChanges();

            Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Many_to_many_relationships_9](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityWithStringPayloadSamples.cs?name=Many_to_many_relationships_9)]

<span data-ttu-id="c29e6-385">Наконец, еще один способ задать данные полезных данных — это переопределение <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> или использование <xref:Microsoft.EntityFrameworkCore.DbContext.SavingChanges?displayProperty=nameWithType> события для обработки сущностей перед обновлением базы данных.</span><span class="sxs-lookup"><span data-stu-id="c29e6-385">Finally, another way to set payload data is by either overriding <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> or using the <xref:Microsoft.EntityFrameworkCore.DbContext.SavingChanges?displayProperty=nameWithType> event to process entities before updating the database.</span></span> <span data-ttu-id="c29e6-386">Пример:</span><span class="sxs-lookup"><span data-stu-id="c29e6-386">For example:</span></span>

<!--
        public override int SaveChanges()
        {
            foreach (var entityEntry in ChangeTracker.Entries<PostTag>())
            {
                if (entityEntry.State == EntityState.Added)
                {
                    entityEntry.Entity.TaggedBy = "ajcvickers";
                }
            }

            return base.SaveChanges();
        }
-->
[!code-csharp[SaveChanges](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityWithStringPayloadSamples.cs?name=SaveChanges)]
