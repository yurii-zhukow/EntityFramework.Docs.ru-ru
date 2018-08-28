---
title: Работа с состояния сущностей - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
ms.openlocfilehash: 1a415e7c6090365a66d58fc9a2dd3256984d7a8e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997951"
---
# <a name="working-with-entity-states"></a><span data-ttu-id="715b7-102">Работа с состояния сущностей</span><span class="sxs-lookup"><span data-stu-id="715b7-102">Working with entity states</span></span>
<span data-ttu-id="715b7-103">В этом разделе будут рассмотрены способы добавления и присоединить сущности к контексту и как платформа Entity Framework преобразует их во время SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="715b7-103">This topic will cover how to add and attach entities to a context and how Entity Framework processes these during SaveChanges.</span></span>
<span data-ttu-id="715b7-104">Платформа Entity Framework берет на себя отслеживания состояния сущностей, пока они подключены к контексту, но в отключенном или N-уровневых сценариях можно позволить платформе EF, какое состояние сущности должны находиться в.</span><span class="sxs-lookup"><span data-stu-id="715b7-104">Entity Framework takes care of tracking the state of entities while they are connected to a context, but in disconnected or N-Tier scenarios you can let EF know what state your entities should be in.</span></span>
<span data-ttu-id="715b7-105">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="715b7-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="entity-states-and-savechanges"></a><span data-ttu-id="715b7-106">Состояния сущностей и SaveChanges</span><span class="sxs-lookup"><span data-stu-id="715b7-106">Entity states and SaveChanges</span></span>

<span data-ttu-id="715b7-107">Сущность может быть в одном из пяти состояний, как определено в перечислении EntityState.</span><span class="sxs-lookup"><span data-stu-id="715b7-107">An entity can be in one of five states as defined by the EntityState enumeration.</span></span> <span data-ttu-id="715b7-108">Возможны следующие состояния:</span><span class="sxs-lookup"><span data-stu-id="715b7-108">These states are:</span></span>  

- <span data-ttu-id="715b7-109">Добавлена: сущность отслеживается контекстом, но еще не существует в базе данных</span><span class="sxs-lookup"><span data-stu-id="715b7-109">Added: the entity is being tracked by the context but does not yet exist in the database</span></span>  
- <span data-ttu-id="715b7-110">Без изменений: отслеживается контекстом и сущности в базе данных и значения его свойств, не отличаются от значений в базе данных</span><span class="sxs-lookup"><span data-stu-id="715b7-110">Unchanged: the entity is being tracked by the context and exists in the database, and its property values have not changed from the values in the database</span></span>  
- <span data-ttu-id="715b7-111">Изменен: сущность отслеживается контекстом и существует в базе данных, а также были изменены некоторые или все значения его свойств</span><span class="sxs-lookup"><span data-stu-id="715b7-111">Modified: the entity is being tracked by the context and exists in the database, and some or all of its property values have been modified</span></span>  
- <span data-ttu-id="715b7-112">Удаленные: сущности отслеживается контекстом в базе данных, но теперь помечено для удаления из базы данных в следующий раз в вызове метода SaveChanges</span><span class="sxs-lookup"><span data-stu-id="715b7-112">Deleted: the entity is being tracked by the context and exists in the database, but has been marked for deletion from the database the next time SaveChanges is called</span></span>  
- <span data-ttu-id="715b7-113">Отсоединенные: сущность не отслеживается контекстом</span><span class="sxs-lookup"><span data-stu-id="715b7-113">Detached: the entity is not being tracked by the context</span></span>  

<span data-ttu-id="715b7-114">SaveChanges выполняет различные действия для сущностей в различных состояниях.</span><span class="sxs-lookup"><span data-stu-id="715b7-114">SaveChanges does different things for entities in different states:</span></span>  

- <span data-ttu-id="715b7-115">Без изменений сущности, не менялись с SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="715b7-115">Unchanged entities are not touched by SaveChanges.</span></span> <span data-ttu-id="715b7-116">Обновления не отправляются в базу данных для сущностей в неизмененном состоянии.</span><span class="sxs-lookup"><span data-stu-id="715b7-116">Updates are not sent to the database for entities in the Unchanged state.</span></span>  
- <span data-ttu-id="715b7-117">Добавлены сущности вставляются в базу данных и затем становятся без изменений при возвращает SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="715b7-117">Added entities are inserted into the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="715b7-118">Измененных сущностей обновляются в базе данных и затем становятся не изменяются при возвращает SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="715b7-118">Modified entities are updated in the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="715b7-119">Удаленные сущности будут удалены из базы данных и затем отсоединяются от контекста.</span><span class="sxs-lookup"><span data-stu-id="715b7-119">Deleted entities are deleted from the database and are then detached from the context.</span></span>  

<span data-ttu-id="715b7-120">Ниже приведены примеры способов, в котором можно изменить состояние объекта или графа объекта.</span><span class="sxs-lookup"><span data-stu-id="715b7-120">The following examples show ways in which the state of an entity or an entity graph can be changed.</span></span>  

## <a name="adding-a-new-entity-to-the-context"></a><span data-ttu-id="715b7-121">Добавление новой сущности в контекст</span><span class="sxs-lookup"><span data-stu-id="715b7-121">Adding a new entity to the context</span></span>  

<span data-ttu-id="715b7-122">Можно добавить новую сущность в контекст путем вызова метода Add для DbSet.</span><span class="sxs-lookup"><span data-stu-id="715b7-122">A new entity can be added to the context by calling the Add method on DbSet.</span></span>
<span data-ttu-id="715b7-123">Этот запрос помещает сущность в добавленном состоянии, это означает, что она будет вставлена в базу данных во время следующего вызова метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="715b7-123">This puts the entity into the Added state, meaning that it will be inserted into the database the next time that SaveChanges is called.</span></span>
<span data-ttu-id="715b7-124">Пример:</span><span class="sxs-lookup"><span data-stu-id="715b7-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

<span data-ttu-id="715b7-125">Чтобы изменить его состояние на добавленное является другим способом добавления новой сущности в контекст.</span><span class="sxs-lookup"><span data-stu-id="715b7-125">Another way to add a new entity to the context is to change its state to Added.</span></span> <span data-ttu-id="715b7-126">Пример:</span><span class="sxs-lookup"><span data-stu-id="715b7-126">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

<span data-ttu-id="715b7-127">Наконец можно добавить новую сущность в контекст путем ее прикрепления к до другой сущности, которая уже отслеживается.</span><span class="sxs-lookup"><span data-stu-id="715b7-127">Finally, you can add a new entity to the context by hooking it up to another entity that is already being tracked.</span></span>
<span data-ttu-id="715b7-128">Это может быть путем добавления новой сущности к другой сущности свойство навигации по коллекции, или установив свойства навигации ссылки другой сущности, чтобы он указывал на новую сущность.</span><span class="sxs-lookup"><span data-stu-id="715b7-128">This could be by adding the new entity to the collection navigation property of another entity or by setting a reference navigation property of another entity to point to the new entity.</span></span> <span data-ttu-id="715b7-129">Пример:</span><span class="sxs-lookup"><span data-stu-id="715b7-129">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Add a new User by setting a reference from a tracked Blog
    var blog = context.Blogs.Find(1);
    blog.Owner = new User { UserName = "johndoe1987" };

    // Add a new Post by adding to the collection of a tracked Blog
    var blog = context.Blogs.Find(2);
    blog.Posts.Add(new Post { Name = "How to Add Entities" });

    context.SaveChanges();
}
```  

<span data-ttu-id="715b7-130">Обратите внимание, что для всех этих примерах, если добавляемый сущность имеет ссылки на другие сущности, которые не отслеживаются затем еще эти новые сущности также будут добавляться к контексту и будет вставлен в базе данных при последующем вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="715b7-130">Note that for all of these examples if the entity being added has references to other entities that are not yet tracked then these new entities will also be added to the context and will be inserted into the database the next time that SaveChanges is called.</span></span>  

## <a name="attaching-an-existing-entity-to-the-context"></a><span data-ttu-id="715b7-131">Присоединение существующей сущности к контексту</span><span class="sxs-lookup"><span data-stu-id="715b7-131">Attaching an existing entity to the context</span></span>  

<span data-ttu-id="715b7-132">При наличии сущности, вы уже знаете, существует в базе данных, но которой не сейчас отслеживается контекстом можно указать контекст для отслеживания использует метод присоединения на DbSet сущности.</span><span class="sxs-lookup"><span data-stu-id="715b7-132">If you have an entity that you know already exists in the database but which is not currently being tracked by the context then you can tell the context to track the entity using the Attach method on DbSet.</span></span> <span data-ttu-id="715b7-133">Сущность будет в неизмененном состоянии, в контексте.</span><span class="sxs-lookup"><span data-stu-id="715b7-133">The entity will be in the Unchanged state in the context.</span></span> <span data-ttu-id="715b7-134">Пример:</span><span class="sxs-lookup"><span data-stu-id="715b7-134">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="715b7-135">Обратите внимание на то, что изменения не будут внесены в базу данных при вызове метода SaveChanges, не выполняя другие операции вложенные сущности.</span><span class="sxs-lookup"><span data-stu-id="715b7-135">Note that no changes will be made to the database if SaveChanges is called without doing any other manipulation of the attached entity.</span></span> <span data-ttu-id="715b7-136">Это обусловлено сущность находится в неизмененном состоянии.</span><span class="sxs-lookup"><span data-stu-id="715b7-136">This is because the entity is in the Unchanged state.</span></span>  

<span data-ttu-id="715b7-137">Другой способ Присоединение существующей сущности к контексту — изменяет свое состояние на неизмененное.</span><span class="sxs-lookup"><span data-stu-id="715b7-137">Another way to attach an existing entity to the context is to change its state to Unchanged.</span></span> <span data-ttu-id="715b7-138">Пример:</span><span class="sxs-lookup"><span data-stu-id="715b7-138">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="715b7-139">Обратите внимание, что для обоих этих примерах Если присоединяемый объект имеет ссылки на другие сущности, которые не отслеживаются затем эти новые сущности будет также присоединен к контексту в неизмененном состоянии.</span><span class="sxs-lookup"><span data-stu-id="715b7-139">Note that for both of these examples if the entity being attached has references to other entities that are not yet tracked then these new entities will also attached to the context in the Unchanged state.</span></span>  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a><span data-ttu-id="715b7-140">Присоединение существующего но измененная сущность к контексту</span><span class="sxs-lookup"><span data-stu-id="715b7-140">Attaching an existing but modified entity to the context</span></span>  

<span data-ttu-id="715b7-141">При наличии сущности, которые известны как уже существует в базе данных, но какие изменения внесены можно указать контекст, чтобы вложить сущность и задайте для его состояние Modified.</span><span class="sxs-lookup"><span data-stu-id="715b7-141">If you have an entity that you know already exists in the database but to which changes may have been made then you can tell the context to attach the entity and set its state to Modified.</span></span>
<span data-ttu-id="715b7-142">Пример:</span><span class="sxs-lookup"><span data-stu-id="715b7-142">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="715b7-143">При изменении состояния для изменения все свойства сущности будут помечены как измененные и все значения свойств будут отправляться в базу данных при вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="715b7-143">When you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  

<span data-ttu-id="715b7-144">Обратите внимание, что если присоединяемый объект имеет ссылки на другие сущности, которые не отслеживаются, затем эти новые сущности будет присоединен к контексту в неизмененном состоянии, они не автоматически будут внесены изменения.</span><span class="sxs-lookup"><span data-stu-id="715b7-144">Note that if the entity being attached has references to other entities that are not yet tracked, then these new entities will attached to the context in the Unchanged state—they will not automatically be made Modified.</span></span>
<span data-ttu-id="715b7-145">При наличии нескольких сущностей, которые должны быть помечены Modified необходимо задать состояние для каждого из этих сущностей по отдельности.</span><span class="sxs-lookup"><span data-stu-id="715b7-145">If you have multiple entities that need to be marked Modified you should set the state for each of these entities individually.</span></span>  

## <a name="changing-the-state-of-a-tracked-entity"></a><span data-ttu-id="715b7-146">Изменение состояния для отслеживаемой сущности</span><span class="sxs-lookup"><span data-stu-id="715b7-146">Changing the state of a tracked entity</span></span>  

<span data-ttu-id="715b7-147">Можно изменить состояние сущности, которая уже отслеживается, задав свойство состояния на соответствующую запись.</span><span class="sxs-lookup"><span data-stu-id="715b7-147">You can change the state of an entity that is already being tracked by setting the State property on its entry.</span></span> <span data-ttu-id="715b7-148">Пример:</span><span class="sxs-lookup"><span data-stu-id="715b7-148">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="715b7-149">Обратите внимание на то, что обращаясь добавить или присоединить сущность, которая уже отслеживается может также использоваться для изменения состояния сущности.</span><span class="sxs-lookup"><span data-stu-id="715b7-149">Note that calling Add or Attach for an entity that is already tracked can also be used to change the entity state.</span></span> <span data-ttu-id="715b7-150">Например вызов присоединения для сущности, которая в настоящее время находится в добавленном состоянии изменит свое состояние на неизмененное.</span><span class="sxs-lookup"><span data-stu-id="715b7-150">For example, calling Attach for an entity that is currently in the Added state will change its state to Unchanged.</span></span>  

## <a name="insert-or-update-pattern"></a><span data-ttu-id="715b7-151">Вставить или обновить шаблон</span><span class="sxs-lookup"><span data-stu-id="715b7-151">Insert or update pattern</span></span>  

<span data-ttu-id="715b7-152">Распространенный подход для некоторых приложений является либо добавить сущность в качестве нового (в результате чего вставки баз данных), или вложить сущность как существующих и пометить его как измененный (что обновления базы данных) в зависимости от значения первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="715b7-152">A common pattern for some applications is to either Add an entity as new (resulting in a database insert) or Attach an entity as existing and mark it as modified (resulting in a database update) depending on the value of the primary key.</span></span>
<span data-ttu-id="715b7-153">Например при использовании сформированный базой данных целочисленных первичных ключей довольно часто рассматривать сущность с нуля ключ как новый и сущность с ключом ненулевое значение, как существующий.</span><span class="sxs-lookup"><span data-stu-id="715b7-153">For example, when using database generated integer primary keys it is common to treat an entity with a zero key as new and an entity with a non-zero key as existing.</span></span>
<span data-ttu-id="715b7-154">Этот шаблон можно сделать путем установки состояние сущности на основе проверки значения первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="715b7-154">This pattern can be achieved by setting the entity state based on a check of the primary key value.</span></span> <span data-ttu-id="715b7-155">Пример:</span><span class="sxs-lookup"><span data-stu-id="715b7-155">For example:</span></span>  

``` csharp
public void InsertOrUpdate(Blog blog)
{
    using (var context = new BloggingContext())
    {
        context.Entry(blog).State = blog.BlogId == 0 ?
                                   EntityState.Added :
                                   EntityState.Modified;

        context.SaveChanges();
    }
}
```  

<span data-ttu-id="715b7-156">Обратите внимание, что при изменении состояния для изменения все свойства сущности будут помечены как измененные, и все значения свойств будут отправляться в базу данных при вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="715b7-156">Note that when you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  
