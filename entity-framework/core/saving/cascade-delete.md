---
title: Каскадное удаление — EF Core
description: Настройка каскадной реакции на события, активируемой при удалении сущности или разрыве ее связи с основной или родительской сущностью
author: ajcvickers
ms.date: 01/07/2021
uid: core/saving/cascade-delete
ms.openlocfilehash: 6e897be6b4f5e6550d9ed3590445df60bef4fb3c
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103023618"
---
# <a name="cascade-delete"></a><span data-ttu-id="3d318-103">Каскадное удаление</span><span class="sxs-lookup"><span data-stu-id="3d318-103">Cascade Delete</span></span>

<span data-ttu-id="3d318-104">В Entity Framework Core (EF Core) связи представлены с помощью внешних ключей.</span><span class="sxs-lookup"><span data-stu-id="3d318-104">Entity Framework Core (EF Core) represents relationships using foreign keys.</span></span> <span data-ttu-id="3d318-105">Сущность с внешним ключом является дочерней или зависимой в связи.</span><span class="sxs-lookup"><span data-stu-id="3d318-105">An entity with a foreign key is the child or dependent entity in the relationship.</span></span> <span data-ttu-id="3d318-106">Значение внешнего ключа этой сущности должно соответствовать значению первичного ключа (или альтернативному значению ключа) связанной основной или родительской сущности.</span><span class="sxs-lookup"><span data-stu-id="3d318-106">This entity's foreign key value must match the primary key value (or an alternate key value) of the related principal/parent entity.</span></span>

<span data-ttu-id="3d318-107">Если основная или родительская сущность удалена, значения внешних ключей зависимых и дочерних сущностей больше не соответствуют первичному или альтернативному ключу _какой-либо_ основной или родительской сущности.</span><span class="sxs-lookup"><span data-stu-id="3d318-107">If the principal/parent entity is deleted, then the foreign key values of the dependents/children will no longer match the primary or alternate key of _any_ principal/parent.</span></span> <span data-ttu-id="3d318-108">Это недопустимое состояние, которое в большинстве баз данных приведет к нарушению ссылочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="3d318-108">This is an invalid state, and will cause a referential constraint violation in most databases.</span></span>

<span data-ttu-id="3d318-109">Избежать этого нарушения можно двумя способами.</span><span class="sxs-lookup"><span data-stu-id="3d318-109">There are two options to avoid this referential constraint violation:</span></span>

1. <span data-ttu-id="3d318-110">Присвоить внешним ключам значения NULL.</span><span class="sxs-lookup"><span data-stu-id="3d318-110">Set the FK values to null</span></span>
2. <span data-ttu-id="3d318-111">Удалить соответствующие зависимые или дочерние сущности.</span><span class="sxs-lookup"><span data-stu-id="3d318-111">Also delete the dependent/child entities</span></span>

<span data-ttu-id="3d318-112">Первый способ подходит только для необязательных связей, в которых свойство внешнего ключа (и столбец базы данных, с которым оно сопоставлено), должно допускать значения NULL.</span><span class="sxs-lookup"><span data-stu-id="3d318-112">The first option in only valid for optional relationships where the foreign key property (and the database column to which it is mapped) must be nullable.</span></span>

<span data-ttu-id="3d318-113">Второй вариант допустим для любого типа связи и называется каскадным удалением.</span><span class="sxs-lookup"><span data-stu-id="3d318-113">The second option is valid for any kind of relationship and is known as "cascade delete".</span></span>

> [!TIP]
> <span data-ttu-id="3d318-114">В этом документе описывается каскадное удаление (и удаление потерянных объектов) в контексте обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="3d318-114">This document describes cascade deletes (and deleting orphans) from the perspective of updating the database.</span></span> <span data-ttu-id="3d318-115">При этом активно используются концепции, представленные в разделах [Отслеживание изменений в EF Core](xref:core/change-tracking/index) и [Изменение внешних ключей и свойств навигации](xref:core/change-tracking/relationship-changes).</span><span class="sxs-lookup"><span data-stu-id="3d318-115">It makes heavy use of concepts introduced in [Change Tracking in EF Core](xref:core/change-tracking/index) and [Changing Foreign Keys and Navigations](xref:core/change-tracking/relationship-changes).</span></span> <span data-ttu-id="3d318-116">Прежде чем приступать к изучению этого документа, обязательно ознакомьтесь с ними.</span><span class="sxs-lookup"><span data-stu-id="3d318-116">Make sure to fully understand these concepts before tackling the material here.</span></span>

> [!TIP]  
> <span data-ttu-id="3d318-117">Вы можете запустить и отладить весь код, используемый в этой документации, [скачав пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/CascadeDeletes).</span><span class="sxs-lookup"><span data-stu-id="3d318-117">You can run and debug into all the code in this document by [downloading the sample code from GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/CascadeDeletes).</span></span>

## <a name="when-cascading-behaviors-happen"></a><span data-ttu-id="3d318-118">Когда происходит каскадная реакция на события</span><span class="sxs-lookup"><span data-stu-id="3d318-118">When cascading behaviors happen</span></span>

<span data-ttu-id="3d318-119">Каскадное удаление требуется в тех случаях, когда зависимая или дочерняя сущность теряет связь со своей текущей основной или родительской сущностью.</span><span class="sxs-lookup"><span data-stu-id="3d318-119">Cascading deletes are needed when a dependent/child entity can no longer be associated with its current principal/parent.</span></span> <span data-ttu-id="3d318-120">Это может произойти в результате удаления основной или родительской сущности, а также в случае разрыва связи между ней и зависимой или дочерней сущностью.</span><span class="sxs-lookup"><span data-stu-id="3d318-120">This can happen because the principal/parent is deleted, or it can happen when the principal/parent still exists but the dependent/child is no longer associated with it.</span></span>

### <a name="deleting-a-principalparent"></a><span data-ttu-id="3d318-121">Удаление основной или родительской сущности</span><span class="sxs-lookup"><span data-stu-id="3d318-121">Deleting a principal/parent</span></span>

<span data-ttu-id="3d318-122">Рассмотрим простую модель, в которой `Blog` является основной или родительской сущностью в связи с дочерней или зависимой сущностью `Post`.</span><span class="sxs-lookup"><span data-stu-id="3d318-122">Consider this simple model where `Blog` is the principal/parent in a relationship with `Post`, which is the dependent/child.</span></span> <span data-ttu-id="3d318-123">В качестве внешнего ключа используется свойство `Post.BlogId`, значение которого должно соответствовать первичному ключу `Post.Id` блога (Blog), которому принадлежит запись (Post).</span><span class="sxs-lookup"><span data-stu-id="3d318-123">`Post.BlogId` is a foreign key property, the value of which must match the `Post.Id` primary key of the post to which the blog belongs.</span></span>

<!--
    public class Blog
    {
        public int Id { get; set; }

        public string Name { get; set; }

        public IList<Post> Posts { get; } = new List<Post>();
    }

    public class Post
    {
        public int Id { get; set; }

        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
-->
[!code-csharp[Model](../../../samples/core/CascadeDeletes/IntroRequiredSamples.cs?name=Model)]

<span data-ttu-id="3d318-124">По соглашению эта связь настроена как обязательная, поскольку свойство внешнего ключа `Post.BlogId` не допускает значения NULL.</span><span class="sxs-lookup"><span data-stu-id="3d318-124">By convention, this relationship is configured as a required, since the `Post.BlogId` foreign key property is non-nullable.</span></span> <span data-ttu-id="3d318-125">Для обязательных связей по умолчанию используется каскадное удаление.</span><span class="sxs-lookup"><span data-stu-id="3d318-125">Required relationships are configured to use cascade deletes by default.</span></span> <span data-ttu-id="3d318-126">Дополнительные сведения о моделировании связей см. в разделе [Связи](xref:core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="3d318-126">See [Relationships](xref:core/modeling/relationships) for more information on modeling relationships.</span></span>

<span data-ttu-id="3d318-127">При удалении блога происходит каскадное удаление всех его записей.</span><span class="sxs-lookup"><span data-stu-id="3d318-127">When deleting a blog, all posts are cascade deleted.</span></span> <span data-ttu-id="3d318-128">Пример:</span><span class="sxs-lookup"><span data-stu-id="3d318-128">For example:</span></span>

<!--
            using var context = new BlogsContext();

            var blog = context.Blogs.OrderBy(e => e.Name).Include(e => e.Posts).First();

            context.Remove(blog);
            
            context.SaveChanges();
-->
[!code-csharp[Deleting_principal_parent_1](../../../samples/core/CascadeDeletes/IntroRequiredSamples.cs?name=Deleting_principal_parent_1)]

<span data-ttu-id="3d318-129">Метод SaveChanges создает следующий код SQL (на примере SQL Server).</span><span class="sxs-lookup"><span data-stu-id="3d318-129">SaveChanges generates the following SQL, using SQL Server as an example:</span></span>

```sql
-- Executed DbCommand (1ms) [Parameters=[@p0='1'], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
DELETE FROM [Posts]
WHERE [Id] = @p0;
SELECT @@ROWCOUNT;

-- Executed DbCommand (0ms) [Parameters=[@p0='2'], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
DELETE FROM [Posts]
WHERE [Id] = @p0;
SELECT @@ROWCOUNT;

-- Executed DbCommand (2ms) [Parameters=[@p1='1'], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
DELETE FROM [Blogs]
WHERE [Id] = @p1;
SELECT @@ROWCOUNT;
```

### <a name="severing-a-relationship"></a><span data-ttu-id="3d318-130">Разрыв связи</span><span class="sxs-lookup"><span data-stu-id="3d318-130">Severing a relationship</span></span>

<span data-ttu-id="3d318-131">Вместо удаления блога можно разорвать связь между ним и каждой его записью.</span><span class="sxs-lookup"><span data-stu-id="3d318-131">Rather than deleting the blog, we could instead sever the relationship between each post and its blog.</span></span> <span data-ttu-id="3d318-132">Это можно сделать, установив значение NULL для свойства навигации по ссылке `Post.Blog` для каждой записи:</span><span class="sxs-lookup"><span data-stu-id="3d318-132">This can be done by setting the reference navigation `Post.Blog` to null for each post:</span></span>

<!--
            using var context = new BlogsContext();

            var blog = context.Blogs.OrderBy(e => e.Name).Include(e => e.Posts).First();

            foreach (var post in blog.Posts)
            {
                post.Blog = null;
            }
            
            context.SaveChanges();
-->
[!code-csharp[Severing_a_relationship_1](../../../samples/core/CascadeDeletes/IntroRequiredSamples.cs?name=Severing_a_relationship_1)]

<span data-ttu-id="3d318-133">Связь также можно разорвать, удалив каждую запись из свойства навигации по коллекции `Blog.Posts`.</span><span class="sxs-lookup"><span data-stu-id="3d318-133">The relationship can also be severed by removing each post from the `Blog.Posts` collection navigation:</span></span>

<!--
            using var context = new BlogsContext();

            var blog = context.Blogs.OrderBy(e => e.Name).Include(e => e.Posts).First();

            blog.Posts.Clear();
            
            context.SaveChanges();
-->
[!code-csharp[Severing_a_relationship_2](../../../samples/core/CascadeDeletes/IntroRequiredSamples.cs?name=Severing_a_relationship_2)]

<span data-ttu-id="3d318-134">В обоих случаях будет достигнут одинаковый результат, то есть записи, которые больше не связаны ни с одним блогом, удаляются, а сам блог остается.</span><span class="sxs-lookup"><span data-stu-id="3d318-134">In either case the result is the same: the blog is not deleted, but the posts that are no longer associated with any blog are deleted:</span></span>

```sql
-- Executed DbCommand (1ms) [Parameters=[@p0='1'], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
DELETE FROM [Posts]
WHERE [Id] = @p0;
SELECT @@ROWCOUNT;

-- Executed DbCommand (0ms) [Parameters=[@p0='2'], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
DELETE FROM [Posts]
WHERE [Id] = @p0;
SELECT @@ROWCOUNT;
```

<span data-ttu-id="3d318-135">Удаление сущностей, которые больше не связаны ни с одной основной или родительской сущностью, называется удалением потерянных объектов.</span><span class="sxs-lookup"><span data-stu-id="3d318-135">Deleting entities that are no longer associated with any principal/dependent is known as "deleting orphans".</span></span>

> [!TIP]
> <span data-ttu-id="3d318-136">Концепции каскадного удаления и удаления потерянных объектов тесно взаимосвязаны.</span><span class="sxs-lookup"><span data-stu-id="3d318-136">Cascade delete and deleting orphans are closely related.</span></span> <span data-ttu-id="3d318-137">В обоих случаях при разрыве связи с основной или родительской сущностью ее зависимые и дочерние сущности удаляются.</span><span class="sxs-lookup"><span data-stu-id="3d318-137">Both result in deleting dependent/child entities when the relationship to their required principal/parent is severed.</span></span> <span data-ttu-id="3d318-138">При каскадном удалении это происходит вследствие удаления самой основной или родительской сущности.</span><span class="sxs-lookup"><span data-stu-id="3d318-138">For cascade delete, this severing happens because the principal/parent is itself deleted.</span></span> <span data-ttu-id="3d318-139">При удалении потерянных объектов основная или родительская сущность по-прежнему существует, но при этом больше не связана с зависимыми и дочерними сущностями.</span><span class="sxs-lookup"><span data-stu-id="3d318-139">For orphans, the principal/parent entity still exists, but is no longer related to the dependent/child entities.</span></span>  

## <a name="where-cascading-behaviors-happen"></a><span data-ttu-id="3d318-140">Где происходит каскадная реакция на события</span><span class="sxs-lookup"><span data-stu-id="3d318-140">Where cascading behaviors happen</span></span>

<span data-ttu-id="3d318-141">Каскадная реакция на события может применяться к следующим сущностям.</span><span class="sxs-lookup"><span data-stu-id="3d318-141">Cascading behaviors can be applied to:</span></span>

- <span data-ttu-id="3d318-142">Сущности, которые отслеживаются текущим экземпляром <xref:Microsoft.EntityFrameworkCore.DbContext>.</span><span class="sxs-lookup"><span data-stu-id="3d318-142">Entities tracked by the current <xref:Microsoft.EntityFrameworkCore.DbContext></span></span>
- <span data-ttu-id="3d318-143">Сущности в базе данных, которые не были загружены в контекст.</span><span class="sxs-lookup"><span data-stu-id="3d318-143">Entities in the database that have not been loaded into the context</span></span>

### <a name="cascade-delete-of-tracked-entities"></a><span data-ttu-id="3d318-144">Каскадное удаление отслеживаемых сущностей</span><span class="sxs-lookup"><span data-stu-id="3d318-144">Cascade delete of tracked entities</span></span>

<span data-ttu-id="3d318-145">EF Core всегда применяет к отслеживаемым сущностям каскадную реакцию на события.</span><span class="sxs-lookup"><span data-stu-id="3d318-145">EF Core always applies configured cascading behaviors to tracked entities.</span></span> <span data-ttu-id="3d318-146">Это значит, что, если приложение загружает все соответствующие зависимые или дочерние сущности в DbContext (как показано в приведенных выше примерах), каскадная реакция на события будет корректно применяться, независимо конфигурации базы данных.</span><span class="sxs-lookup"><span data-stu-id="3d318-146">This means that if the application loads all relevant dependent/child entities into the DbContext, as is shown in the examples above, then cascading behaviors will be correctly applied regardless of how the database is configured.</span></span>

> [!TIP]
> <span data-ttu-id="3d318-147">Точное время, когда происходит каскадная реакция для отслеживаемых сущностей, можно контролировать с помощью свойств <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.CascadeDeleteTiming?displayProperty=nameWithType> и <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DeleteOrphansTiming?displayProperty=nameWithType>.</span><span class="sxs-lookup"><span data-stu-id="3d318-147">The exact timing of when cascading behaviors happen to tracked entities can be controlled using <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.CascadeDeleteTiming?displayProperty=nameWithType> and <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DeleteOrphansTiming?displayProperty=nameWithType>.</span></span> <span data-ttu-id="3d318-148">Дополнительные сведения см. в разделе [Изменение внешних ключей и свойств навигации](xref:core/change-tracking/relationship-changes).</span><span class="sxs-lookup"><span data-stu-id="3d318-148">See [Changing Foreign Keys and Navigations](xref:core/change-tracking/relationship-changes) for more information.</span></span>

### <a name="cascade-delete-in-the-database"></a><span data-ttu-id="3d318-149">Каскадное удаление в базе данных</span><span class="sxs-lookup"><span data-stu-id="3d318-149">Cascade delete in the database</span></span>

<span data-ttu-id="3d318-150">Многие системы баз данных также реализуют каскадную реакцию на события, которая активируется при удалении сущности в базе данных.</span><span class="sxs-lookup"><span data-stu-id="3d318-150">Many database systems also offer cascading behaviors that are triggered when an entity is deleted in the database.</span></span> <span data-ttu-id="3d318-151">EF Core определяет конфигурацию такой реакции на основе настроек каскадного удаления в модели EF Core при создании базы данных с использованием метода <xref:Microsoft.EntityFrameworkCore.Infrastructure.DatabaseFacade.EnsureCreated%2A> или миграций EF Core.</span><span class="sxs-lookup"><span data-stu-id="3d318-151">EF Core configures these behaviors based on the cascade delete behavior in the EF Core model when a database is created using <xref:Microsoft.EntityFrameworkCore.Infrastructure.DatabaseFacade.EnsureCreated%2A> or EF Core migrations.</span></span> <span data-ttu-id="3d318-152">Например, в представленной выше модели при использовании SQL Server для записей создается следующая таблица.</span><span class="sxs-lookup"><span data-stu-id="3d318-152">For example, using the model above, the following table is created for posts when using SQL Server:</span></span>

```sql
CREATE TABLE [Posts] (
    [Id] int NOT NULL IDENTITY,
    [Title] nvarchar(max) NULL,
    [Content] nvarchar(max) NULL,
    [BlogId] int NOT NULL,
    CONSTRAINT [PK_Posts] PRIMARY KEY ([Id]),
    CONSTRAINT [FK_Posts_Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [Blogs] ([Id]) ON DELETE CASCADE
);
```

<span data-ttu-id="3d318-153">Обратите внимание, что ограничение внешнего ключа, определяющее связь между блогами и записями, настраивается с использованием `ON DELETE CASCADE`.</span><span class="sxs-lookup"><span data-stu-id="3d318-153">Notice that the foreign key constraint defining the relationship between blogs and posts is configured with `ON DELETE CASCADE`.</span></span>

<span data-ttu-id="3d318-154">Если мы знаем, что база данных настроена таким образом, можно удалить блог _без предварительной загрузки записей_. В этом случае база данных автоматически удалит все связанные с ним записи.</span><span class="sxs-lookup"><span data-stu-id="3d318-154">If we know that the database is configured like this, then we can delete a blog _without first loading posts_ and the database will take care of deleting all the posts that were related to that blog.</span></span> <span data-ttu-id="3d318-155">Пример:</span><span class="sxs-lookup"><span data-stu-id="3d318-155">For example:</span></span>

<!--
            using var context = new BlogsContext();

            var blog = context.Blogs.OrderBy(e => e.Name).First();

            context.Remove(blog);
            
            context.SaveChanges();
-->
[!code-csharp[Where_cascading_behaviors_happen_1](../../../samples/core/CascadeDeletes/IntroRequiredSamples.cs?name=Where_cascading_behaviors_happen_1)]

<span data-ttu-id="3d318-156">Обратите внимание, что для записей не используется `Include`, поэтому они не загружаются.</span><span class="sxs-lookup"><span data-stu-id="3d318-156">Notice that there is no `Include` for posts, so they are not loaded.</span></span> <span data-ttu-id="3d318-157">В этом случае метод SaveChanges удаляет только блог, поскольку это единственная отслеживаемая сущность.</span><span class="sxs-lookup"><span data-stu-id="3d318-157">SaveChanges in this case will delete just the blog, since that's the only entity being tracked:</span></span>

```sql
-- Executed DbCommand (6ms) [Parameters=[@p0='1'], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
DELETE FROM [Blogs]
WHERE [Id] = @p0;
SELECT @@ROWCOUNT;
```

<span data-ttu-id="3d318-158">Если ограничение внешнего ключа в базе данных не настроено для каскадного удаления, это приведет к возникновению исключения.</span><span class="sxs-lookup"><span data-stu-id="3d318-158">This would result in an exception if the foreign key constraint in the database is not configured for cascade deletes.</span></span> <span data-ttu-id="3d318-159">Тем не менее в этом случае записи будут удалены базой данных, так как при ее создании была задана настройка `ON DELETE CASCADE`.</span><span class="sxs-lookup"><span data-stu-id="3d318-159">However, in this case the posts are deleted by the database because it has been configured with `ON DELETE CASCADE` when it was created.</span></span>

> [!NOTE]
> <span data-ttu-id="3d318-160">Как правило, базы данных не поддерживают автоматическое удаление потерянных объектов.</span><span class="sxs-lookup"><span data-stu-id="3d318-160">Databases don't typically have any way to automatically delete orphans.</span></span> <span data-ttu-id="3d318-161">Это связано с тем, что в базах данных используются только внешние ключи и нет свойств навигации, тогда как в EF Core связи представляются с использованием и внешних ключей, и свойств навигации.</span><span class="sxs-lookup"><span data-stu-id="3d318-161">This is because while EF Core represents relationships using navigations as well of foreign keys, databases have only foreign keys and no navigations.</span></span> <span data-ttu-id="3d318-162">Это значит, что в большинстве случаев для разрыва связи обе ее стороны должны быть загружены в DbContext.</span><span class="sxs-lookup"><span data-stu-id="3d318-162">This means that it is usually not possible to sever a relationship without loading both sides into the DbContext.</span></span>

> [!NOTE]
> <span data-ttu-id="3d318-163">Выполняющаяся в памяти база данных EF Core в настоящее время не поддерживает каскадное удаление в базе данных.</span><span class="sxs-lookup"><span data-stu-id="3d318-163">The EF Core in-memory database does not currently support cascade deletes in the database.</span></span>

> [!WARNING]
> <span data-ttu-id="3d318-164">Не следует настраивать каскадное удаление в базе данных при обратимом удалении сущностей.</span><span class="sxs-lookup"><span data-stu-id="3d318-164">Do not configure cascade delete in the database when soft-deleting entities.</span></span> <span data-ttu-id="3d318-165">Это может привести к случайному полному (вместо обратимого) удалению сущностей.</span><span class="sxs-lookup"><span data-stu-id="3d318-165">This may cause entities to be accidentally really deleted instead of soft-deleted.</span></span>

### <a name="database-cascade-limitations"></a><span data-ttu-id="3d318-166">Ограничения для каскадной реакции на события в базе данных</span><span class="sxs-lookup"><span data-stu-id="3d318-166">Database cascade limitations</span></span>

<span data-ttu-id="3d318-167">Некоторые базы данных, особенно SQL Server, устанавливают ограничения для каскадных операций, которые приводят к возникновению циклов.</span><span class="sxs-lookup"><span data-stu-id="3d318-167">Some databases, most notably SQL Server, have limitations on the cascade behaviors that form cycles.</span></span> <span data-ttu-id="3d318-168">Рассмотрим следующую модель.</span><span class="sxs-lookup"><span data-stu-id="3d318-168">For example, consider the following model:</span></span>

<!--
    public class Blog
    {
        public int Id { get; set; }
        public string Name { get; set; }

        public IList<Post> Posts { get; } = new List<Post>();
        
        public int OwnerId { get; set; }
        public Person Owner { get; set; }
    }

    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
        
        public int AuthorId { get; set; }
        public Person Author { get; set; }
    }

    public class Person
    {
        public int Id { get; set; }
        public string Name { get; set; }
        
        public IList<Post> Posts { get; } = new List<Post>();

        public Blog OwnedBlog { get; set; }
    }
-->
[!code-csharp[Model](../../../samples/core/CascadeDeletes/WithDatabaseCycleSamples.cs?name=Model)]

<span data-ttu-id="3d318-169">В этой модели устанавливаются три связи, каждая из которых является обязательной и по соглашению настраивается для каскадного удаления.</span><span class="sxs-lookup"><span data-stu-id="3d318-169">This model has three relationships, all required and therefore configured to cascade delete by convention:</span></span>

- <span data-ttu-id="3d318-170">При удалении блога происходит каскадное удаление всех связанных с ним записей.</span><span class="sxs-lookup"><span data-stu-id="3d318-170">Deleting a blog will cascade delete all the related posts</span></span>
- <span data-ttu-id="3d318-171">При удалении автора записей происходит каскадное удаление созданных им записей.</span><span class="sxs-lookup"><span data-stu-id="3d318-171">Deleting the author of posts will cause the authored posts to be cascade deleted</span></span>
- <span data-ttu-id="3d318-172">При удалении владельца блога происходит каскадное удаление самого блога.</span><span class="sxs-lookup"><span data-stu-id="3d318-172">Deleting the owner of a blog will cause the blog to be cascade deleted</span></span>

<span data-ttu-id="3d318-173">Это вполне разумный, хотя и достаточно строгий подход к управлению блогом. Тем не менее при попытке создать базу данных SQL Server с этими настройками каскадной реакции на событий возникнет следующее исключение.</span><span class="sxs-lookup"><span data-stu-id="3d318-173">This is all reasonable (if a bit draconian in blog management policies!) but attempting to create a SQL Server database with these cascades configured results in the following exception:</span></span>

> <span data-ttu-id="3d318-174">Microsoft.Data.SqlClient.SqlException (0x80131904): Введение ограничения внешнего ключа (FOREIGN KEY) FK_Posts_Person_AuthorId для таблицы Posts может привести к появлению циклов или множественных каскадных путей.</span><span class="sxs-lookup"><span data-stu-id="3d318-174">Microsoft.Data.SqlClient.SqlException (0x80131904): Introducing FOREIGN KEY constraint 'FK_Posts_Person_AuthorId' on table 'Posts' may cause cycles or multiple cascade paths.</span></span> <span data-ttu-id="3d318-175">Укажите ON DELETE NO ACTION или ON UPDATE NO ACTION либо измените другие ограничения внешнего ключа (FOREIGN KEY).</span><span class="sxs-lookup"><span data-stu-id="3d318-175">Specify ON DELETE NO ACTION or ON UPDATE NO ACTION, or modify other FOREIGN KEY constraints.</span></span>

<span data-ttu-id="3d318-176">Эту проблему можно решить двумя способами.</span><span class="sxs-lookup"><span data-stu-id="3d318-176">There are two ways to handle this situation:</span></span>

1. <span data-ttu-id="3d318-177">Изменить одну связь или несколько, отключив их каскадное удаление.</span><span class="sxs-lookup"><span data-stu-id="3d318-177">Change one or more of the relationships to not cascade delete.</span></span>
2. <span data-ttu-id="3d318-178">Настроить базу данных, отключив одно или несколько из этих действий каскадного удаления, после чего убедиться, что все зависимые сущности загружены, чтобы позволить EF Core реализовывать каскадную реакцию на события.</span><span class="sxs-lookup"><span data-stu-id="3d318-178">Configure the database without one or more of these cascade deletes, then ensure all dependent entities are loaded so that EF Core can perform the cascading behavior.</span></span>

<span data-ttu-id="3d318-179">Применение первого подхода в нашем примере позволяет сделать связь между блогом и его владельцем необязательной, предоставив свойство внешнего ключа, допускающее значение NULL.</span><span class="sxs-lookup"><span data-stu-id="3d318-179">Taking the first approach with our example, we could make the blog-owner relationship optional by giving it a nullable foreign key property:</span></span>

<!--
            public int? BlogId { get; set; }
-->
[!code-csharp[NullableBlogId](../../../samples/core/CascadeDeletes/OptionalDependentsSamples.cs?name=NullableBlogId)]

<span data-ttu-id="3d318-180">Поскольку эта связь не является обязательной, блог может существовать без владельца, то есть каскадное удаление больше не будет настроено по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3d318-180">An optional relationship allows the blog to exist without an owner, which means cascade delete will no longer be configured by default.</span></span> <span data-ttu-id="3d318-181">Это значит, что каскадная реакция на события больше не будет приводить к возникновению циклов, благодаря чему база данных может быть создана в SQL Server без ошибок.</span><span class="sxs-lookup"><span data-stu-id="3d318-181">This means there is no longer a cycle in cascading actions, and the database can be created without error on SQL Server.</span></span>

<span data-ttu-id="3d318-182">Если использовать второй подход, связь между блогом и владельцем можно оставить обязательной и настроить для каскадного удаления, но при этом задать применение этой конфигурации только к отслеживаемым сущностям, а не к базе данных.</span><span class="sxs-lookup"><span data-stu-id="3d318-182">Taking the second approach instead, we can keep the blog-owner relationship required and configured for cascade delete, but make this configuration only apply to tracked entities, not the database:</span></span>

<!--
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder
                .Entity<Blog>()
                .HasOne(e => e.Owner)
                .WithOne(e => e.OwnedBlog)
                .OnDelete(DeleteBehavior.ClientCascade);
        }
-->
[!code-csharp[OnModelCreating](../../../samples/core/CascadeDeletes/WithDatabaseCycleSamples.cs?name=OnModelCreating)]

<span data-ttu-id="3d318-183">Что произойдет, если загрузить владельца блога и сам блог, а затем удалить владельца?</span><span class="sxs-lookup"><span data-stu-id="3d318-183">Now what happens if we load both a person and the blog they own, then delete the person?</span></span>

<!--
            using var context = new BlogsContext();

            var owner = context.People.Single(e => e.Name == "ajcvickers");
            var blog = context.Blogs.Single(e => e.Owner == owner);

            context.Remove(owner);
            
            context.SaveChanges();
-->
[!code-csharp[Database_cascade_limitations_1](../../../samples/core/CascadeDeletes/WithDatabaseCycleSamples.cs?name=Database_cascade_limitations_1)]

<span data-ttu-id="3d318-184">EF Core выполнит каскадное удаление владельца и принадлежащего ему блога.</span><span class="sxs-lookup"><span data-stu-id="3d318-184">EF Core will cascade the delete of the owner so that the blog is also deleted:</span></span>

```sql
-- Executed DbCommand (8ms) [Parameters=[@p0='1'], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
DELETE FROM [Blogs]
WHERE [Id] = @p0;
SELECT @@ROWCOUNT;

-- Executed DbCommand (2ms) [Parameters=[@p1='1'], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
DELETE FROM [People]
WHERE [Id] = @p1;
SELECT @@ROWCOUNT;
```

<span data-ttu-id="3d318-185">Тем не менее, если блог не был загружен, при удалении его владельца происходит следующее.</span><span class="sxs-lookup"><span data-stu-id="3d318-185">However, if the blog is not loaded when the owner is deleted:</span></span>

<!--
                using var context = new BlogsContext();

                var owner = context.People.Single(e => e.Name == "ajcvickers");

                context.Remove(owner);
            
                context.SaveChanges();
-->
[!code-csharp[Database_cascade_limitations_2](../../../samples/core/CascadeDeletes/WithDatabaseCycleSamples.cs?name=Database_cascade_limitations_2)]

<span data-ttu-id="3d318-186">После этого возникает исключение из-за нарушения ограничения внешнего ключа в базе данных.</span><span class="sxs-lookup"><span data-stu-id="3d318-186">Then an exception will be thrown due to violation of the foreign key constraint in the database:</span></span>

> <span data-ttu-id="3d318-187">Microsoft.Data.SqlClient.SqlException: Конфликт инструкции DELETE с ограничением REFERENCE FK_Blogs_People_OwnerId.</span><span class="sxs-lookup"><span data-stu-id="3d318-187">Microsoft.Data.SqlClient.SqlException: The DELETE statement conflicted with the REFERENCE constraint "FK_Blogs_People_OwnerId".</span></span> <span data-ttu-id="3d318-188">Конфликт произошел в базе данных Scratch, таблица dbo.Blogs, столбец OwnerId.</span><span class="sxs-lookup"><span data-stu-id="3d318-188">The conflict occurred in database "Scratch", table "dbo.Blogs", column 'OwnerId'.</span></span>
<span data-ttu-id="3d318-189">Выполнение данной инструкции было прервано.</span><span class="sxs-lookup"><span data-stu-id="3d318-189">The statement has been terminated.</span></span>

## <a name="cascading-nulls"></a><span data-ttu-id="3d318-190">Каскадное распространение значений NULL</span><span class="sxs-lookup"><span data-stu-id="3d318-190">Cascading nulls</span></span>

<span data-ttu-id="3d318-191">Для необязательных связей свойства внешнего ключа, допускающие значения NULL, сопоставлены со столбцами базы данных, также допускающими такие значения.</span><span class="sxs-lookup"><span data-stu-id="3d318-191">Optional relationships have nullable foreign key properties mapped to nullable database columns.</span></span> <span data-ttu-id="3d318-192">Это означает, что внешнему ключу может быть присвоено значение NULL, если текущая основная или родительская сущность удаляется или разрывается ее связь с зависимой или дочерней сущностью.</span><span class="sxs-lookup"><span data-stu-id="3d318-192">This means that the foreign key value can be set to null when the current principal/parent is deleted or is severed from the dependent/child.</span></span>

<span data-ttu-id="3d318-193">Еще раз возьмем примеры из раздела [Когда происходит каскадная реакция на события](#when-cascading-behaviors-happen). На этот раз рассмотрим необязательную связь, которая представлена допускающим значение NULL свойством внешнего ключа `Post.BlogId`:</span><span class="sxs-lookup"><span data-stu-id="3d318-193">Let's look again at the examples from [When cascading behaviors happen](#when-cascading-behaviors-happen), but this time with an optional relationship represented by a nullable `Post.BlogId` foreign key property:</span></span>

<!--
            public int? BlogId { get; set; }
-->
[!code-csharp[NullableBlogId](../../../samples/core/CascadeDeletes/OptionalDependentsSamples.cs?name=NullableBlogId)]

<span data-ttu-id="3d318-194">Этому свойству внешнего ключа будет присваиваться значение NULL для каждой записи при удалении связанного с ней блога.</span><span class="sxs-lookup"><span data-stu-id="3d318-194">This foreign key property will be set to null for each post when its related blog is deleted.</span></span> <span data-ttu-id="3d318-195">Например, следующий код аналогичен приведенному ранее.</span><span class="sxs-lookup"><span data-stu-id="3d318-195">For example, this code, which is the same as before:</span></span>

<!--
            using var context = new BlogsContext();

            var blog = context.Blogs.OrderBy(e => e.Name).Include(e => e.Posts).First();

            context.Remove(blog);
            
            context.SaveChanges();
-->
[!code-csharp[Deleting_principal_parent_1b](../../../samples/core/CascadeDeletes/IntroOptionalSamples.cs?name=Deleting_principal_parent_1b)]

<span data-ttu-id="3d318-196">Тем не менее теперь при вызове метода SaveChanges будут выполнены следующие обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="3d318-196">Will now result in the following database updates when SaveChanges is called:</span></span>

```sql
-- Executed DbCommand (2ms) [Parameters=[@p1='1', @p0=NULL (DbType = Int32)], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
UPDATE [Posts] SET [BlogId] = @p0
WHERE [Id] = @p1;
SELECT @@ROWCOUNT;

-- Executed DbCommand (0ms) [Parameters=[@p1='2', @p0=NULL (DbType = Int32)], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
UPDATE [Posts] SET [BlogId] = @p0
WHERE [Id] = @p1;
SELECT @@ROWCOUNT;

-- Executed DbCommand (1ms) [Parameters=[@p2='1'], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
DELETE FROM [Blogs]
WHERE [Id] = @p2;
SELECT @@ROWCOUNT;
```

<span data-ttu-id="3d318-197">Аналогичным образом, при разрыве связи в любом из приведенных выше примеров:</span><span class="sxs-lookup"><span data-stu-id="3d318-197">Likewise, if the relationship is severed using either of the examples from above:</span></span>

<!--
            using var context = new BlogsContext();

            var blog = context.Blogs.OrderBy(e => e.Name).Include(e => e.Posts).First();

            foreach (var post in blog.Posts)
            {
                post.Blog = null;
            }
            
            context.SaveChanges();
-->
[!code-csharp[Severing_a_relationship_1b](../../../samples/core/CascadeDeletes/IntroOptionalSamples.cs?name=Severing_a_relationship_1b)]

<span data-ttu-id="3d318-198">Или сделайте так:</span><span class="sxs-lookup"><span data-stu-id="3d318-198">Or:</span></span>

<!--
            using var context = new BlogsContext();

            var blog = context.Blogs.OrderBy(e => e.Name).Include(e => e.Posts).First();

            blog.Posts.Clear();
            
            context.SaveChanges();
-->
[!code-csharp[Severing_a_relationship_2b](../../../samples/core/CascadeDeletes/IntroOptionalSamples.cs?name=Severing_a_relationship_2b)]

<span data-ttu-id="3d318-199">При вызове метода SaveChanges записи будут обновлены с присвоением внешнему ключу значения NULL.</span><span class="sxs-lookup"><span data-stu-id="3d318-199">Then the posts are updated with null foreign key values when SaveChanges is called:</span></span>

```sql
-- Executed DbCommand (2ms) [Parameters=[@p1='1', @p0=NULL (DbType = Int32)], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
UPDATE [Posts] SET [BlogId] = @p0
WHERE [Id] = @p1;
SELECT @@ROWCOUNT;

-- Executed DbCommand (0ms) [Parameters=[@p1='2', @p0=NULL (DbType = Int32)], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
UPDATE [Posts] SET [BlogId] = @p0
WHERE [Id] = @p1;
SELECT @@ROWCOUNT;
```

<span data-ttu-id="3d318-200">Дополнительные сведения о том, как EF Core управляет внешними ключами и свойствами навигации при изменении их значений, см. в разделе [Изменение внешних ключей и свойств навигации](xref:core/change-tracking/relationship-changes).</span><span class="sxs-lookup"><span data-stu-id="3d318-200">See [Changing Foreign Keys and Navigations](xref:core/change-tracking/relationship-changes) for more information on how EF Core manages foreign keys and navigations as their values are changed.</span></span>

> [!NOTE]
> <span data-ttu-id="3d318-201">По умолчанию, начиная с первой версии Entity Framework 2008 года, выполнялась адресная привязка таких связей.</span><span class="sxs-lookup"><span data-stu-id="3d318-201">The fixup of relationships like this has been the default behavior of Entity Framework since the first version in 2008.</span></span> <span data-ttu-id="3d318-202">До появления EF Core у такого подхода не было своего названия и его нельзя было изменить.</span><span class="sxs-lookup"><span data-stu-id="3d318-202">Prior to EF Core it didn't have a name and was not possible to change.</span></span> <span data-ttu-id="3d318-203">Сейчас он называется `ClientSetNull` и описывается в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="3d318-203">It is now known as `ClientSetNull` as described in the next section.</span></span>

<span data-ttu-id="3d318-204">Базы данных также можно настроить для подобного каскадного распространения значений NULL при удалении основной или родительской сущности в необязательной связи.</span><span class="sxs-lookup"><span data-stu-id="3d318-204">Databases can also be configured to cascade nulls like this when a principal/parent in an optional relationship is deleted.</span></span> <span data-ttu-id="3d318-205">Тем не менее этот подход применяется гораздо реже операций каскадного удаления в базе данных.</span><span class="sxs-lookup"><span data-stu-id="3d318-205">However, this is much less common than using cascading deletes in the database.</span></span> <span data-ttu-id="3d318-206">Одновременное выполнение каскадного удаления и каскадного распространения значений NULL в базе данных SQL Server почти всегда будет приводить к возникновению циклов связей.</span><span class="sxs-lookup"><span data-stu-id="3d318-206">Using cascading deletes and cascading nulls in the database at the same time will almost always result in relationship cycles when using SQL Server.</span></span> <span data-ttu-id="3d318-207">Дополнительные сведения о настройке каскадного распространения значений NULL см. в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="3d318-207">See the next section for more information on configuring cascading nulls.</span></span>

## <a name="configuring-cascading-behaviors"></a><span data-ttu-id="3d318-208">Настройка каскадной реакции на события</span><span class="sxs-lookup"><span data-stu-id="3d318-208">Configuring cascading behaviors</span></span>

> [!TIP]
> <span data-ttu-id="3d318-209">Прежде чем переходить к этому разделу, ознакомьтесь с приведенными выше.</span><span class="sxs-lookup"><span data-stu-id="3d318-209">Be sure to read sections above before coming here.</span></span> <span data-ttu-id="3d318-210">Для понимания параметров конфигурации потребуется знание представленных ранее материалов.</span><span class="sxs-lookup"><span data-stu-id="3d318-210">The configuration options will likely not make sense if the preceding material is not understood.</span></span>

<span data-ttu-id="3d318-211">Настройка каскадной реакции на события для отдельных связей осуществляется с помощью метода <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.ReferenceCollectionBuilder.OnDelete%2A> в <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A>.</span><span class="sxs-lookup"><span data-stu-id="3d318-211">Cascade behaviors are configured per relationship using the <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.ReferenceCollectionBuilder.OnDelete%2A> method in <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A>.</span></span> <span data-ttu-id="3d318-212">Пример:</span><span class="sxs-lookup"><span data-stu-id="3d318-212">For example:</span></span>

<!--
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder
                .Entity<Blog>()
                .HasOne(e => e.Owner)
                .WithOne(e => e.OwnedBlog)
                .OnDelete(DeleteBehavior.ClientCascade);
        }
-->
[!code-csharp[OnModelCreating](../../../samples/core/CascadeDeletes/WithDatabaseCycleSamples.cs?name=OnModelCreating)]

<span data-ttu-id="3d318-213">Дополнительные сведения о настройке связей между типами сущностей см. в разделе [Связи](xref:core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="3d318-213">See [Relationships](xref:core/modeling/relationships) for more information on configuring relationships between entity types.</span></span>

<span data-ttu-id="3d318-214">Метод `OnDelete` принимает значение из перечисления <xref:Microsoft.EntityFrameworkCore.DeleteBehavior>, с использованием которого связаны определенные сложности.</span><span class="sxs-lookup"><span data-stu-id="3d318-214">`OnDelete` accepts a value from the, admittedly confusing, <xref:Microsoft.EntityFrameworkCore.DeleteBehavior> enum.</span></span> <span data-ttu-id="3d318-215">В этом перечислении определяется поведение EF Core для отслеживаемых сущностей и задается конфигурация каскадного удаления в базе данных в случаях, когда для создания схемы используется EF.</span><span class="sxs-lookup"><span data-stu-id="3d318-215">This enum defines both the behavior of EF Core on tracked entities, and the configuration of cascade delete in the database when EF is used to create the schema.</span></span>

### <a name="impact-on-database-schema"></a><span data-ttu-id="3d318-216">Влияние на схему базы данных</span><span class="sxs-lookup"><span data-stu-id="3d318-216">Impact on database schema</span></span>

<span data-ttu-id="3d318-217">В следующей таблице показан результат применения каждого значения метода `OnDelete` для ограничения внешнего ключа, создаваемого с помощью миграций EF Core или метода <xref:Microsoft.EntityFrameworkCore.Infrastructure.DatabaseFacade.EnsureCreated%2A>.</span><span class="sxs-lookup"><span data-stu-id="3d318-217">The following table shows the result of each `OnDelete` value on the foreign key constraint created by EF Core migrations or <xref:Microsoft.EntityFrameworkCore.Infrastructure.DatabaseFacade.EnsureCreated%2A>.</span></span>

| <span data-ttu-id="3d318-218">Значение перечисления DeleteBehavior</span><span class="sxs-lookup"><span data-stu-id="3d318-218">DeleteBehavior</span></span>        | <span data-ttu-id="3d318-219">Влияние на схему базы данных</span><span class="sxs-lookup"><span data-stu-id="3d318-219">Impact on database schema</span></span>
|:----------------------|--------------------------
| <span data-ttu-id="3d318-220">Cascade</span><span class="sxs-lookup"><span data-stu-id="3d318-220">Cascade</span></span>               | <span data-ttu-id="3d318-221">ON DELETE CASCADE</span><span class="sxs-lookup"><span data-stu-id="3d318-221">ON DELETE CASCADE</span></span>
| <span data-ttu-id="3d318-222">Ограничение</span><span class="sxs-lookup"><span data-stu-id="3d318-222">Restrict</span></span>              | <span data-ttu-id="3d318-223">ON DELETE NO ACTION</span><span class="sxs-lookup"><span data-stu-id="3d318-223">ON DELETE NO ACTION</span></span>
| <span data-ttu-id="3d318-224">NoAction</span><span class="sxs-lookup"><span data-stu-id="3d318-224">NoAction</span></span>              | <span data-ttu-id="3d318-225">database default</span><span class="sxs-lookup"><span data-stu-id="3d318-225">database default</span></span>
| <span data-ttu-id="3d318-226">SetNull</span><span class="sxs-lookup"><span data-stu-id="3d318-226">SetNull</span></span>               | <span data-ttu-id="3d318-227">ON DELETE SET NULL</span><span class="sxs-lookup"><span data-stu-id="3d318-227">ON DELETE SET NULL</span></span>
| <span data-ttu-id="3d318-228">ClientSetNull</span><span class="sxs-lookup"><span data-stu-id="3d318-228">ClientSetNull</span></span>         | <span data-ttu-id="3d318-229">ON DELETE NO ACTION</span><span class="sxs-lookup"><span data-stu-id="3d318-229">ON DELETE NO ACTION</span></span>
| <span data-ttu-id="3d318-230">ClientCascade</span><span class="sxs-lookup"><span data-stu-id="3d318-230">ClientCascade</span></span>         | <span data-ttu-id="3d318-231">ON DELETE NO ACTION</span><span class="sxs-lookup"><span data-stu-id="3d318-231">ON DELETE NO ACTION</span></span>
| <span data-ttu-id="3d318-232">ClientNoAction</span><span class="sxs-lookup"><span data-stu-id="3d318-232">ClientNoAction</span></span>        | <span data-ttu-id="3d318-233">database default</span><span class="sxs-lookup"><span data-stu-id="3d318-233">database default</span></span>

> [!NOTE]
> <span data-ttu-id="3d318-234">Эта таблица определена неоднозначно и в ближайшем выпуске будет пересмотрена.</span><span class="sxs-lookup"><span data-stu-id="3d318-234">This table is confusing and we plan to revisit this in a future release.</span></span> <span data-ttu-id="3d318-235">См. [описание проблемы 21252 на сайте GitHub](https://github.com/dotnet/efcore/issues/21252).</span><span class="sxs-lookup"><span data-stu-id="3d318-235">See [GitHub Issue #21252](https://github.com/dotnet/efcore/issues/21252).</span></span>

<span data-ttu-id="3d318-236">Действия `ON DELETE NO ACTION` и `ON DELETE RESTRICT` в реляционных базах данных, как правило, идентичны или очень близки.</span><span class="sxs-lookup"><span data-stu-id="3d318-236">The behaviors of `ON DELETE NO ACTION` and `ON DELETE RESTRICT` in relational databases are typically either identical or very similar.</span></span> <span data-ttu-id="3d318-237">Несмотря на название `NO ACTION`, оба эти параметра приводят к принудительному применению ссылочных ограничений.</span><span class="sxs-lookup"><span data-stu-id="3d318-237">Despite what `NO ACTION` may imply, both of these options cause referential constraints to be enforced.</span></span> <span data-ttu-id="3d318-238">Единственное различие между ними заключается в том, _когда_ база данных выполняет проверку ограничений.</span><span class="sxs-lookup"><span data-stu-id="3d318-238">The difference, when there is one, is _when_ the database checks the constraints.</span></span>  <span data-ttu-id="3d318-239">Сведения о конкретных различиях между параметрами `ON DELETE NO ACTION` и `ON DELETE RESTRICT` в вашей системе базы данных следует искать в документации по ней.</span><span class="sxs-lookup"><span data-stu-id="3d318-239">Check your database documentation for the specific differences between `ON DELETE NO ACTION` and `ON DELETE RESTRICT` on your database system.</span></span>

<span data-ttu-id="3d318-240">Каскадная реакция на события в базе данных реализуется только при использовании значений `Cascade` и `SetNull`.</span><span class="sxs-lookup"><span data-stu-id="3d318-240">The only values that will cause cascading behaviors on the database are `Cascade` and `SetNull`.</span></span> <span data-ttu-id="3d318-241">При любых других значениях каскадное распространение изменений в базе данных не осуществляется.</span><span class="sxs-lookup"><span data-stu-id="3d318-241">All other values will configure the database to not cascade any changes.</span></span>

### <a name="impact-on-savechanges-behavior"></a><span data-ttu-id="3d318-242">Влияние на поведение метода SaveChanges</span><span class="sxs-lookup"><span data-stu-id="3d318-242">Impact on SaveChanges behavior</span></span>

<span data-ttu-id="3d318-243">В следующих разделах приводятся таблицы, в которых описывается, что происходит с зависимыми или дочерними сущностями при удалении основной или родительской сущности, а также при разрыве ее связи с зависимой или дочерней сущностью.</span><span class="sxs-lookup"><span data-stu-id="3d318-243">The tables in the following sections cover what happens to dependent/child entities when the principal/parent is deleted, or its relationship to the dependent/child entities is severed.</span></span> <span data-ttu-id="3d318-244">Каждая таблица описывает одну из следующих ситуаций.</span><span class="sxs-lookup"><span data-stu-id="3d318-244">Each table covers one of:</span></span>

- <span data-ttu-id="3d318-245">Необязательные (внешний ключ, допускающий значения NULL) и обязательные (не допускающий значения NULL внешний ключ) связи</span><span class="sxs-lookup"><span data-stu-id="3d318-245">Optional (nullable FK) and required (non-nullable FK) relationships</span></span>
- <span data-ttu-id="3d318-246">Зависимые или дочерние сущности загружаются в DbContext и отслеживаются в нем (либо существуют только в базе данных).</span><span class="sxs-lookup"><span data-stu-id="3d318-246">When dependents/children are loaded and tracked by the DbContext and when they exist only in the database</span></span>

#### <a name="required-relationship-with-dependentschildren-loaded"></a><span data-ttu-id="3d318-247">Обязательная связь с загруженными зависимыми или дочерними сущностями</span><span class="sxs-lookup"><span data-stu-id="3d318-247">Required relationship with dependents/children loaded</span></span>

| <span data-ttu-id="3d318-248">Значение перечисления DeleteBehavior</span><span class="sxs-lookup"><span data-stu-id="3d318-248">DeleteBehavior</span></span>    | <span data-ttu-id="3d318-249">Поведение при удалении основной или родительской сущности</span><span class="sxs-lookup"><span data-stu-id="3d318-249">On deleting principal/parent</span></span>             | <span data-ttu-id="3d318-250">Поведение при разрыве связи с основной или родительской сущностью</span><span class="sxs-lookup"><span data-stu-id="3d318-250">On severing from principal/parent</span></span>
|:------------------|------------------------------------------|----------------------------------------
| <span data-ttu-id="3d318-251">Cascade</span><span class="sxs-lookup"><span data-stu-id="3d318-251">Cascade</span></span>           | <span data-ttu-id="3d318-252">EF Core удаляет зависимые сущности</span><span class="sxs-lookup"><span data-stu-id="3d318-252">Dependents deleted by EF Core</span></span>            | <span data-ttu-id="3d318-253">EF Core удаляет зависимые сущности</span><span class="sxs-lookup"><span data-stu-id="3d318-253">Dependents deleted by EF Core</span></span>
| <span data-ttu-id="3d318-254">Ограничение</span><span class="sxs-lookup"><span data-stu-id="3d318-254">Restrict</span></span>          | `InvalidOperationException`              | `InvalidOperationException`
| <span data-ttu-id="3d318-255">NoAction</span><span class="sxs-lookup"><span data-stu-id="3d318-255">NoAction</span></span>          | `InvalidOperationException`              | `InvalidOperationException`
| <span data-ttu-id="3d318-256">SetNull</span><span class="sxs-lookup"><span data-stu-id="3d318-256">SetNull</span></span>           | <span data-ttu-id="3d318-257">При создании базы данных возникает исключение `SqlException`</span><span class="sxs-lookup"><span data-stu-id="3d318-257">`SqlException` on creating database</span></span>      | <span data-ttu-id="3d318-258">При создании базы данных возникает исключение `SqlException`</span><span class="sxs-lookup"><span data-stu-id="3d318-258">`SqlException` on creating database</span></span>
| <span data-ttu-id="3d318-259">ClientSetNull</span><span class="sxs-lookup"><span data-stu-id="3d318-259">ClientSetNull</span></span>     | `InvalidOperationException`              | `InvalidOperationException`
| <span data-ttu-id="3d318-260">ClientCascade</span><span class="sxs-lookup"><span data-stu-id="3d318-260">ClientCascade</span></span>     | <span data-ttu-id="3d318-261">EF Core удаляет зависимые сущности</span><span class="sxs-lookup"><span data-stu-id="3d318-261">Dependents deleted by EF Core</span></span>            | <span data-ttu-id="3d318-262">EF Core удаляет зависимые сущности</span><span class="sxs-lookup"><span data-stu-id="3d318-262">Dependents deleted by EF Core</span></span>
| <span data-ttu-id="3d318-263">ClientNoAction</span><span class="sxs-lookup"><span data-stu-id="3d318-263">ClientNoAction</span></span>    | `DbUpdateException`                      | `InvalidOperationException`

<span data-ttu-id="3d318-264">Примечания.</span><span class="sxs-lookup"><span data-stu-id="3d318-264">Notes:</span></span>

- <span data-ttu-id="3d318-265">По умолчанию для таких обязательных связей задается значение `Cascade`.</span><span class="sxs-lookup"><span data-stu-id="3d318-265">The default for required relationships like this is `Cascade`.</span></span>
- <span data-ttu-id="3d318-266">Применение любого другого значения для каскадного удаления обязательных связей приведет к возникновению исключения при вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="3d318-266">Using anything other than cascade delete for required relationships will result in an exception when SaveChanges is called.</span></span>
  - <span data-ttu-id="3d318-267">Как правило, в этом случае возникает исключение EF Core `InvalidOperationException`, так как в загруженных дочерних или зависимых сущностях обнаруживается недопустимое состояние.</span><span class="sxs-lookup"><span data-stu-id="3d318-267">Typically, this is an `InvalidOperationException` from EF Core since the invalid state is detected in the loaded children/dependents.</span></span>
  - <span data-ttu-id="3d318-268">Если используется значение `ClientNoAction`, EF Core не проверяет зависимые сущности адресной привязки перед их отправкой в базу данных, поэтому в этом случае база данных создает исключение, которое затем упаковывается в исключение `DbUpdateException` методом SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="3d318-268">`ClientNoAction` forces EF Core to not check fixup dependents before sending them to the database, so in this case the database throws an exception, which is then wrapped in a `DbUpdateException` by SaveChanges.</span></span>
  - <span data-ttu-id="3d318-269">Значение `SetNull` при создании базы данных отклоняется, поскольку столбец внешнего ключа не допускает значения NULL.</span><span class="sxs-lookup"><span data-stu-id="3d318-269">`SetNull` is rejected when creating the database since the foreign key column is not nullable.</span></span>
- <span data-ttu-id="3d318-270">Поскольку зависимые и дочерние сущности загружаются, они всегда будут удаляться с помощью EF Core и никогда не удаляются базой данных.</span><span class="sxs-lookup"><span data-stu-id="3d318-270">Since dependents/children are loaded, they are always deleted by EF Core, and never left for the database to delete.</span></span>

#### <a name="required-relationship-with-dependentschildren-not-loaded"></a><span data-ttu-id="3d318-271">Обязательная связь, для которой не загружены зависимые или дочерние сущности</span><span class="sxs-lookup"><span data-stu-id="3d318-271">Required relationship with dependents/children not loaded</span></span>

| <span data-ttu-id="3d318-272">Значение перечисления DeleteBehavior</span><span class="sxs-lookup"><span data-stu-id="3d318-272">DeleteBehavior</span></span>    | <span data-ttu-id="3d318-273">Поведение при удалении основной или родительской сущности</span><span class="sxs-lookup"><span data-stu-id="3d318-273">On deleting principal/parent</span></span>             | <span data-ttu-id="3d318-274">Поведение при разрыве связи с основной или родительской сущностью</span><span class="sxs-lookup"><span data-stu-id="3d318-274">On severing from principal/parent</span></span>
|:------------------|------------------------------------------|----------------------------------------
| <span data-ttu-id="3d318-275">Cascade</span><span class="sxs-lookup"><span data-stu-id="3d318-275">Cascade</span></span>           | <span data-ttu-id="3d318-276">База данных удаляет зависимые сущности</span><span class="sxs-lookup"><span data-stu-id="3d318-276">Dependents deleted by database</span></span>           | <span data-ttu-id="3d318-277">Н/Д</span><span class="sxs-lookup"><span data-stu-id="3d318-277">N/A</span></span>
| <span data-ttu-id="3d318-278">Ограничение</span><span class="sxs-lookup"><span data-stu-id="3d318-278">Restrict</span></span>          | `DbUpdateException`                      | <span data-ttu-id="3d318-279">Н/Д</span><span class="sxs-lookup"><span data-stu-id="3d318-279">N/A</span></span>
| <span data-ttu-id="3d318-280">NoAction</span><span class="sxs-lookup"><span data-stu-id="3d318-280">NoAction</span></span>          | `DbUpdateException`                      | <span data-ttu-id="3d318-281">Н/Д</span><span class="sxs-lookup"><span data-stu-id="3d318-281">N/A</span></span>
| <span data-ttu-id="3d318-282">SetNull</span><span class="sxs-lookup"><span data-stu-id="3d318-282">SetNull</span></span>           | <span data-ttu-id="3d318-283">При создании базы данных возникает исключение `SqlException`</span><span class="sxs-lookup"><span data-stu-id="3d318-283">`SqlException` on creating database</span></span>      | <span data-ttu-id="3d318-284">Н/Д</span><span class="sxs-lookup"><span data-stu-id="3d318-284">N/A</span></span>
| <span data-ttu-id="3d318-285">ClientSetNull</span><span class="sxs-lookup"><span data-stu-id="3d318-285">ClientSetNull</span></span>     | `DbUpdateException`                      | <span data-ttu-id="3d318-286">Н/Д</span><span class="sxs-lookup"><span data-stu-id="3d318-286">N/A</span></span>
| <span data-ttu-id="3d318-287">ClientCascade</span><span class="sxs-lookup"><span data-stu-id="3d318-287">ClientCascade</span></span>     | `DbUpdateException`                      | <span data-ttu-id="3d318-288">Н/Д</span><span class="sxs-lookup"><span data-stu-id="3d318-288">N/A</span></span>
| <span data-ttu-id="3d318-289">ClientNoAction</span><span class="sxs-lookup"><span data-stu-id="3d318-289">ClientNoAction</span></span>    | `DbUpdateException`                      | <span data-ttu-id="3d318-290">Н/Д</span><span class="sxs-lookup"><span data-stu-id="3d318-290">N/A</span></span>

<span data-ttu-id="3d318-291">Примечания.</span><span class="sxs-lookup"><span data-stu-id="3d318-291">Notes:</span></span>

- <span data-ttu-id="3d318-292">Разрыв связи в этом случае невозможен, поскольку зависимые или дочерние сущности не загружаются.</span><span class="sxs-lookup"><span data-stu-id="3d318-292">Severing a relationship is not valid here since the dependents/children are not loaded.</span></span>
- <span data-ttu-id="3d318-293">По умолчанию для таких обязательных связей задается значение `Cascade`.</span><span class="sxs-lookup"><span data-stu-id="3d318-293">The default for required relationships like this is `Cascade`.</span></span>
- <span data-ttu-id="3d318-294">Применение любого другого значения для каскадного удаления обязательных связей приведет к возникновению исключения при вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="3d318-294">Using anything other than cascade delete for required relationships will result in an exception when SaveChanges is called.</span></span>
  - <span data-ttu-id="3d318-295">Как правило, это исключение`DbUpdateException`, поскольку зависимые или дочерние сущности не загружаются, поэтому недопустимое состояние может быть обнаружено только базой данных.</span><span class="sxs-lookup"><span data-stu-id="3d318-295">Typically, this is a `DbUpdateException` because the dependents/children are not loaded, and hence the invalid state can only be detected by the database.</span></span> <span data-ttu-id="3d318-296">После этого метод SaveChanges упаковывает исключение базы данных в `DbUpdateException`.</span><span class="sxs-lookup"><span data-stu-id="3d318-296">SaveChanges then wraps the database exception in a `DbUpdateException`.</span></span>
  - <span data-ttu-id="3d318-297">Значение `SetNull` при создании базы данных отклоняется, поскольку столбец внешнего ключа не допускает значения NULL.</span><span class="sxs-lookup"><span data-stu-id="3d318-297">`SetNull` is rejected when creating the database since the foreign key column is not nullable.</span></span>

#### <a name="optional-relationship-with-dependentschildren-loaded"></a><span data-ttu-id="3d318-298">Необязательная связь с загруженными зависимыми или дочерними сущностями</span><span class="sxs-lookup"><span data-stu-id="3d318-298">Optional relationship with dependents/children loaded</span></span>

| <span data-ttu-id="3d318-299">Значение перечисления DeleteBehavior</span><span class="sxs-lookup"><span data-stu-id="3d318-299">DeleteBehavior</span></span>    | <span data-ttu-id="3d318-300">Поведение при удалении основной или родительской сущности</span><span class="sxs-lookup"><span data-stu-id="3d318-300">On deleting principal/parent</span></span>             | <span data-ttu-id="3d318-301">Поведение при разрыве связи с основной или родительской сущностью</span><span class="sxs-lookup"><span data-stu-id="3d318-301">On severing from principal/parent</span></span>
|:------------------|------------------------------------------|----------------------------------------
| <span data-ttu-id="3d318-302">Cascade</span><span class="sxs-lookup"><span data-stu-id="3d318-302">Cascade</span></span>           | <span data-ttu-id="3d318-303">EF Core удаляет зависимые сущности</span><span class="sxs-lookup"><span data-stu-id="3d318-303">Dependents deleted by EF Core</span></span>            | <span data-ttu-id="3d318-304">EF Core удаляет зависимые сущности</span><span class="sxs-lookup"><span data-stu-id="3d318-304">Dependents deleted by EF Core</span></span>
| <span data-ttu-id="3d318-305">Ограничение</span><span class="sxs-lookup"><span data-stu-id="3d318-305">Restrict</span></span>          | <span data-ttu-id="3d318-306">EF Core присваивает значения NULL внешним ключам зависимых сущностей</span><span class="sxs-lookup"><span data-stu-id="3d318-306">Dependent FKs set to null by EF Core</span></span>     | <span data-ttu-id="3d318-307">EF Core присваивает значения NULL внешним ключам зависимых сущностей</span><span class="sxs-lookup"><span data-stu-id="3d318-307">Dependent FKs set to null by EF Core</span></span>
| <span data-ttu-id="3d318-308">NoAction</span><span class="sxs-lookup"><span data-stu-id="3d318-308">NoAction</span></span>          | <span data-ttu-id="3d318-309">EF Core присваивает значения NULL внешним ключам зависимых сущностей</span><span class="sxs-lookup"><span data-stu-id="3d318-309">Dependent FKs set to null by EF Core</span></span>     | <span data-ttu-id="3d318-310">EF Core присваивает значения NULL внешним ключам зависимых сущностей</span><span class="sxs-lookup"><span data-stu-id="3d318-310">Dependent FKs set to null by EF Core</span></span>
| <span data-ttu-id="3d318-311">SetNull</span><span class="sxs-lookup"><span data-stu-id="3d318-311">SetNull</span></span>           | <span data-ttu-id="3d318-312">EF Core присваивает значения NULL внешним ключам зависимых сущностей</span><span class="sxs-lookup"><span data-stu-id="3d318-312">Dependent FKs set to null by EF Core</span></span>     | <span data-ttu-id="3d318-313">EF Core присваивает значения NULL внешним ключам зависимых сущностей</span><span class="sxs-lookup"><span data-stu-id="3d318-313">Dependent FKs set to null by EF Core</span></span>
| <span data-ttu-id="3d318-314">ClientSetNull</span><span class="sxs-lookup"><span data-stu-id="3d318-314">ClientSetNull</span></span>     | <span data-ttu-id="3d318-315">EF Core присваивает значения NULL внешним ключам зависимых сущностей</span><span class="sxs-lookup"><span data-stu-id="3d318-315">Dependent FKs set to null by EF Core</span></span>     | <span data-ttu-id="3d318-316">EF Core присваивает значения NULL внешним ключам зависимых сущностей</span><span class="sxs-lookup"><span data-stu-id="3d318-316">Dependent FKs set to null by EF Core</span></span>
| <span data-ttu-id="3d318-317">ClientCascade</span><span class="sxs-lookup"><span data-stu-id="3d318-317">ClientCascade</span></span>     | <span data-ttu-id="3d318-318">EF Core удаляет зависимые сущности</span><span class="sxs-lookup"><span data-stu-id="3d318-318">Dependents deleted by EF Core</span></span>            | <span data-ttu-id="3d318-319">EF Core удаляет зависимые сущности</span><span class="sxs-lookup"><span data-stu-id="3d318-319">Dependents deleted by EF Core</span></span>
| <span data-ttu-id="3d318-320">ClientNoAction</span><span class="sxs-lookup"><span data-stu-id="3d318-320">ClientNoAction</span></span>    | `DbUpdateException`                      | <span data-ttu-id="3d318-321">EF Core присваивает значения NULL внешним ключам зависимых сущностей</span><span class="sxs-lookup"><span data-stu-id="3d318-321">Dependent FKs set to null by EF Core</span></span>

<span data-ttu-id="3d318-322">Примечания.</span><span class="sxs-lookup"><span data-stu-id="3d318-322">Notes:</span></span>

- <span data-ttu-id="3d318-323">По умолчанию для таких необязательных связей задается значение `ClientSetNull`.</span><span class="sxs-lookup"><span data-stu-id="3d318-323">The default for optional relationships like this is `ClientSetNull`.</span></span>
- <span data-ttu-id="3d318-324">Зависимые и дочерние сущности никогда не удаляются, если не задано значение `Cascade` или `ClientCascade`.</span><span class="sxs-lookup"><span data-stu-id="3d318-324">Dependents/children are never deleted unless `Cascade` or `ClientCascade` are configured.</span></span>
- <span data-ttu-id="3d318-325">При любых других значениях EF Core присваивает значения NULL внешним ключам зависимых сущностей.</span><span class="sxs-lookup"><span data-stu-id="3d318-325">All other values cause the dependent FKs to be set to null by EF Core...</span></span>
  - <span data-ttu-id="3d318-326">Исключением является значение `ClientNoAction`, при котором EF Core не затрагивает внешние ключи зависимых или дочерних сущностей при удалении основной или родительской сущности.</span><span class="sxs-lookup"><span data-stu-id="3d318-326">...except `ClientNoAction` which tells EF Core not to touch the foreign keys of dependents/children when the principal/parent is deleted.</span></span> <span data-ttu-id="3d318-327">Таким образом, база данных создает исключение, которое упаковывается в `DbUpdateException` методом SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="3d318-327">The database therefore throws an exception, which is wrapped as a `DbUpdateException` by SaveChanges.</span></span>

#### <a name="optional-relationship-with-dependentschildren-not-loaded"></a><span data-ttu-id="3d318-328">Необязательная связь, для которой не загружены зависимые или дочерние сущности</span><span class="sxs-lookup"><span data-stu-id="3d318-328">Optional relationship with dependents/children not loaded</span></span>

| <span data-ttu-id="3d318-329">Значение перечисления DeleteBehavior</span><span class="sxs-lookup"><span data-stu-id="3d318-329">DeleteBehavior</span></span>    | <span data-ttu-id="3d318-330">Поведение при удалении основной или родительской сущности</span><span class="sxs-lookup"><span data-stu-id="3d318-330">On deleting principal/parent</span></span>             | <span data-ttu-id="3d318-331">Поведение при разрыве связи с основной или родительской сущностью</span><span class="sxs-lookup"><span data-stu-id="3d318-331">On severing from principal/parent</span></span>
|:------------------|------------------------------------------|----------------------------------------
| <span data-ttu-id="3d318-332">Cascade</span><span class="sxs-lookup"><span data-stu-id="3d318-332">Cascade</span></span>           | <span data-ttu-id="3d318-333">База данных удаляет зависимые сущности</span><span class="sxs-lookup"><span data-stu-id="3d318-333">Dependents deleted by database</span></span>           | <span data-ttu-id="3d318-334">Н/Д</span><span class="sxs-lookup"><span data-stu-id="3d318-334">N/A</span></span>
| <span data-ttu-id="3d318-335">Ограничение</span><span class="sxs-lookup"><span data-stu-id="3d318-335">Restrict</span></span>          | `DbUpdateException`                      | <span data-ttu-id="3d318-336">Н/Д</span><span class="sxs-lookup"><span data-stu-id="3d318-336">N/A</span></span>
| <span data-ttu-id="3d318-337">NoAction</span><span class="sxs-lookup"><span data-stu-id="3d318-337">NoAction</span></span>          | `DbUpdateException`                      | <span data-ttu-id="3d318-338">Н/Д</span><span class="sxs-lookup"><span data-stu-id="3d318-338">N/A</span></span>
| <span data-ttu-id="3d318-339">SetNull</span><span class="sxs-lookup"><span data-stu-id="3d318-339">SetNull</span></span>           | <span data-ttu-id="3d318-340">База данных присваивает значения NULL внешним ключам зависимых сущностей</span><span class="sxs-lookup"><span data-stu-id="3d318-340">Dependent FKs set to null by database</span></span>    | <span data-ttu-id="3d318-341">Н/Д</span><span class="sxs-lookup"><span data-stu-id="3d318-341">N/A</span></span>
| <span data-ttu-id="3d318-342">ClientSetNull</span><span class="sxs-lookup"><span data-stu-id="3d318-342">ClientSetNull</span></span>     | `DbUpdateException`                      | <span data-ttu-id="3d318-343">Н/Д</span><span class="sxs-lookup"><span data-stu-id="3d318-343">N/A</span></span>
| <span data-ttu-id="3d318-344">ClientCascade</span><span class="sxs-lookup"><span data-stu-id="3d318-344">ClientCascade</span></span>     | `DbUpdateException`                      | <span data-ttu-id="3d318-345">Н/Д</span><span class="sxs-lookup"><span data-stu-id="3d318-345">N/A</span></span>
| <span data-ttu-id="3d318-346">ClientNoAction</span><span class="sxs-lookup"><span data-stu-id="3d318-346">ClientNoAction</span></span>    | `DbUpdateException`                      | <span data-ttu-id="3d318-347">Н/Д</span><span class="sxs-lookup"><span data-stu-id="3d318-347">N/A</span></span>

<span data-ttu-id="3d318-348">Примечания.</span><span class="sxs-lookup"><span data-stu-id="3d318-348">Notes:</span></span>

- <span data-ttu-id="3d318-349">Разрыв связи в этом случае невозможен, поскольку зависимые или дочерние сущности не загружаются.</span><span class="sxs-lookup"><span data-stu-id="3d318-349">Severing a relationship is not valid here since the dependents/children are not loaded.</span></span>
- <span data-ttu-id="3d318-350">По умолчанию для таких необязательных связей задается значение `ClientSetNull`.</span><span class="sxs-lookup"><span data-stu-id="3d318-350">The default for optional relationships like this is `ClientSetNull`.</span></span>
- <span data-ttu-id="3d318-351">Чтобы избежать возникновения исключения базы данных, зависимые и дочерние сущности должны загружаться, если только база данных не настроена для каскадного удаления или распространения значений NULL.</span><span class="sxs-lookup"><span data-stu-id="3d318-351">Dependents/children must be loaded to avoid a database exception unless the database has been configured to cascade either deletes or nulls.</span></span>
