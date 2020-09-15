---
title: Работа с состояниями сущностей — EF6
description: Работа с состояниями сущностей в Entity Framework 6
author: divega
ms.date: 10/23/2016
uid: ef6/saving/change-tracking/entity-state
ms.openlocfilehash: 88c1b67b3eda02e79f7d10d5e46fdd3566361634
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90073773"
---
# <a name="working-with-entity-states"></a><span data-ttu-id="4d4b8-103">Работа с состояниями сущностей</span><span class="sxs-lookup"><span data-stu-id="4d4b8-103">Working with entity states</span></span>
<span data-ttu-id="4d4b8-104">В этом разделе рассматривается добавление и присоединение сущностей к контексту и то, как Entity Framework обрабатывает их во время вызова метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-104">This topic will cover how to add and attach entities to a context and how Entity Framework processes these during SaveChanges.</span></span>
<span data-ttu-id="4d4b8-105">Entity Framework следит за отслеживанием состояния сущностей, когда они подключены к контексту, но в отключенных или N-ярусных сценариях EF можно сообщить о том, в каком состоянии будут находиться ваши сущности.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-105">Entity Framework takes care of tracking the state of entities while they are connected to a context, but in disconnected or N-Tier scenarios you can let EF know what state your entities should be in.</span></span>
<span data-ttu-id="4d4b8-106">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="entity-states-and-savechanges"></a><span data-ttu-id="4d4b8-107">Состояния сущностей и SaveChanges</span><span class="sxs-lookup"><span data-stu-id="4d4b8-107">Entity states and SaveChanges</span></span>

<span data-ttu-id="4d4b8-108">Сущность может находиться в одном из пяти состояний, как определено в перечислении EntityState.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-108">An entity can be in one of five states as defined by the EntityState enumeration.</span></span> <span data-ttu-id="4d4b8-109">Возможны следующие состояния:</span><span class="sxs-lookup"><span data-stu-id="4d4b8-109">These states are:</span></span>  

- <span data-ttu-id="4d4b8-110">Добавлено: сущность отслеживается контекстом, но еще не существует в базе данных.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-110">Added: the entity is being tracked by the context but does not yet exist in the database</span></span>  
- <span data-ttu-id="4d4b8-111">Без изменений: сущность отслеживается контекстом и существует в базе данных, а значения ее свойств не изменяются из значений в базе данных.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-111">Unchanged: the entity is being tracked by the context and exists in the database, and its property values have not changed from the values in the database</span></span>  
- <span data-ttu-id="4d4b8-112">Изменено: сущность отслеживается контекстом и существует в базе данных, а некоторые или все значения ее свойств были изменены.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-112">Modified: the entity is being tracked by the context and exists in the database, and some or all of its property values have been modified</span></span>  
- <span data-ttu-id="4d4b8-113">Удалено: сущность отслеживается контекстом и существует в базе данных, но была помечена для удаления из базы данных при следующем вызове SaveChanges</span><span class="sxs-lookup"><span data-stu-id="4d4b8-113">Deleted: the entity is being tracked by the context and exists in the database, but has been marked for deletion from the database the next time SaveChanges is called</span></span>  
- <span data-ttu-id="4d4b8-114">Отсоединено: сущность не отслеживается контекстом</span><span class="sxs-lookup"><span data-stu-id="4d4b8-114">Detached: the entity is not being tracked by the context</span></span>  

<span data-ttu-id="4d4b8-115">SaveChanges выполняет различные операции с сущностями в различных состояниях:</span><span class="sxs-lookup"><span data-stu-id="4d4b8-115">SaveChanges does different things for entities in different states:</span></span>  

- <span data-ttu-id="4d4b8-116">Неизмененные сущности не затрагиваются функцией SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-116">Unchanged entities are not touched by SaveChanges.</span></span> <span data-ttu-id="4d4b8-117">Обновления не отправляются в базу данных для сущностей в неизмененном состоянии.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-117">Updates are not sent to the database for entities in the Unchanged state.</span></span>  
- <span data-ttu-id="4d4b8-118">Добавленные сущности вставляются в базу данных, а затем становятся неизменными при возврате SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-118">Added entities are inserted into the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="4d4b8-119">Измененные сущности обновляются в базе данных, а затем становятся неизменными при возврате SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-119">Modified entities are updated in the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="4d4b8-120">Удаленные сущности удаляются из базы данных и затем отсоединяются из контекста.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-120">Deleted entities are deleted from the database and are then detached from the context.</span></span>  

<span data-ttu-id="4d4b8-121">В следующих примерах показаны способы изменения состояния сущности или графа сущностей.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-121">The following examples show ways in which the state of an entity or an entity graph can be changed.</span></span>  

## <a name="adding-a-new-entity-to-the-context"></a><span data-ttu-id="4d4b8-122">Добавление новой сущности в контекст</span><span class="sxs-lookup"><span data-stu-id="4d4b8-122">Adding a new entity to the context</span></span>  

<span data-ttu-id="4d4b8-123">Новую сущность можно добавить в контекст, вызвав метод Add для DbSet.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-123">A new entity can be added to the context by calling the Add method on DbSet.</span></span>
<span data-ttu-id="4d4b8-124">При этом сущность помещается в добавленное состояние, что означает, что она будет вставлена в базу данных при следующем вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-124">This puts the entity into the Added state, meaning that it will be inserted into the database the next time that SaveChanges is called.</span></span>
<span data-ttu-id="4d4b8-125">Пример:</span><span class="sxs-lookup"><span data-stu-id="4d4b8-125">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

<span data-ttu-id="4d4b8-126">Другим способом добавления новой сущности в контекст является изменение ее состояния на "Добавлено".</span><span class="sxs-lookup"><span data-stu-id="4d4b8-126">Another way to add a new entity to the context is to change its state to Added.</span></span> <span data-ttu-id="4d4b8-127">Пример:</span><span class="sxs-lookup"><span data-stu-id="4d4b8-127">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

<span data-ttu-id="4d4b8-128">Наконец, можно добавить новую сущность в контекст, применив ее к другой сущности, которая уже отслеживается.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-128">Finally, you can add a new entity to the context by hooking it up to another entity that is already being tracked.</span></span>
<span data-ttu-id="4d4b8-129">Это может быть вызвано добавлением новой сущности к свойству навигации коллекции другой сущности или установкой свойства навигации по ссылке другой сущности для указания на новую сущность.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-129">This could be by adding the new entity to the collection navigation property of another entity or by setting a reference navigation property of another entity to point to the new entity.</span></span> <span data-ttu-id="4d4b8-130">Пример:</span><span class="sxs-lookup"><span data-stu-id="4d4b8-130">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Add a new User by setting a reference from a tracked Blog
    var blog = context.Blogs.Find(1);
    blog.Owner = new User { UserName = "johndoe1987" };

    // Add a new Post by adding to the collection of a tracked Blog
    blog.Posts.Add(new Post { Name = "How to Add Entities" });

    context.SaveChanges();
}
```  

<span data-ttu-id="4d4b8-131">Обратите внимание, что для всех этих примеров, если добавляемая сущность содержит ссылки на другие сущности, которые еще не были отслеживаемы, эти новые сущности также будут добавлены в контекст и будут вставлены в базу данных при следующем вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-131">Note that for all of these examples if the entity being added has references to other entities that are not yet tracked then these new entities will also be added to the context and will be inserted into the database the next time that SaveChanges is called.</span></span>  

## <a name="attaching-an-existing-entity-to-the-context"></a><span data-ttu-id="4d4b8-132">Присоединение существующей сущности к контексту</span><span class="sxs-lookup"><span data-stu-id="4d4b8-132">Attaching an existing entity to the context</span></span>  

<span data-ttu-id="4d4b8-133">Если у вас есть сущность, которая уже существует в базе данных, но в настоящий момент не отслеживается контекстом, можно указать контексту отслеживание сущности с помощью метода Attach в DbSet.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-133">If you have an entity that you know already exists in the database but which is not currently being tracked by the context then you can tell the context to track the entity using the Attach method on DbSet.</span></span> <span data-ttu-id="4d4b8-134">Сущность будет находиться в неизмененном состоянии в контексте.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-134">The entity will be in the Unchanged state in the context.</span></span> <span data-ttu-id="4d4b8-135">Пример:</span><span class="sxs-lookup"><span data-stu-id="4d4b8-135">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="4d4b8-136">Обратите внимание, что в базу данных не вносятся изменения, если метод SaveChanges вызывается без выполнения других манипуляций с присоединенной сущностью.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-136">Note that no changes will be made to the database if SaveChanges is called without doing any other manipulation of the attached entity.</span></span> <span data-ttu-id="4d4b8-137">Это вызвано тем, что сущность находится в неизмененном состоянии.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-137">This is because the entity is in the Unchanged state.</span></span>  

<span data-ttu-id="4d4b8-138">Еще один способ присоединить существующую сущность к контексту — изменить его состояние на без изменений.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-138">Another way to attach an existing entity to the context is to change its state to Unchanged.</span></span> <span data-ttu-id="4d4b8-139">Пример:</span><span class="sxs-lookup"><span data-stu-id="4d4b8-139">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="4d4b8-140">Обратите внимание, что в обоих примерах, если присоединяемая сущность имеет ссылки на другие сущности, которые еще не отслеживаемы, эти новые сущности также будут присоединены к контексту в неизмененном состоянии.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-140">Note that for both of these examples if the entity being attached has references to other entities that are not yet tracked then these new entities will also attached to the context in the Unchanged state.</span></span>  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a><span data-ttu-id="4d4b8-141">Присоединение существующей, но измененной сущности к контексту</span><span class="sxs-lookup"><span data-stu-id="4d4b8-141">Attaching an existing but modified entity to the context</span></span>  

<span data-ttu-id="4d4b8-142">Если у вас есть сущность, которая уже существует в базе данных, но изменения могли быть внесены, можно указать контексту присоединить сущность и присвоить ей состояние Modified.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-142">If you have an entity that you know already exists in the database but to which changes may have been made then you can tell the context to attach the entity and set its state to Modified.</span></span>
<span data-ttu-id="4d4b8-143">Пример:</span><span class="sxs-lookup"><span data-stu-id="4d4b8-143">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="4d4b8-144">При изменении состояния на измененные все свойства сущности будут помечены как измененные, а все значения свойств будут отправляться в базу данных при вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-144">When you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  

<span data-ttu-id="4d4b8-145">Обратите внимание, что если присоединяемая сущность содержит ссылки на другие сущности, которые еще не отслеживаемы, то эти новые сущности будут присоединены к контексту в неизмененном состоянии — они не будут автоматически изменены.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-145">Note that if the entity being attached has references to other entities that are not yet tracked, then these new entities will attached to the context in the Unchanged state—they will not automatically be made Modified.</span></span>
<span data-ttu-id="4d4b8-146">При наличии нескольких сущностей, которые необходимо пометить как измененные, необходимо задать состояние для каждой из этих сущностей по отдельности.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-146">If you have multiple entities that need to be marked Modified you should set the state for each of these entities individually.</span></span>  

## <a name="changing-the-state-of-a-tracked-entity"></a><span data-ttu-id="4d4b8-147">Изменение состояния отслеживающей сущности</span><span class="sxs-lookup"><span data-stu-id="4d4b8-147">Changing the state of a tracked entity</span></span>  

<span data-ttu-id="4d4b8-148">Состояние сущности, которая уже отслеживается, можно изменить, задав свойство State для его записи.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-148">You can change the state of an entity that is already being tracked by setting the State property on its entry.</span></span> <span data-ttu-id="4d4b8-149">Пример:</span><span class="sxs-lookup"><span data-stu-id="4d4b8-149">For example:</span></span>  

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

<span data-ttu-id="4d4b8-150">Обратите внимание, что вызов Add или Attach для сущности, которая уже зарегистрирована, может также использоваться для изменения состояния сущности.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-150">Note that calling Add or Attach for an entity that is already tracked can also be used to change the entity state.</span></span> <span data-ttu-id="4d4b8-151">Например, вызов attach для сущности, которая в данный момент находится в состоянии добавления, изменит свое состояние на без изменений.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-151">For example, calling Attach for an entity that is currently in the Added state will change its state to Unchanged.</span></span>  

## <a name="insert-or-update-pattern"></a><span data-ttu-id="4d4b8-152">Вставка или обновление шаблона</span><span class="sxs-lookup"><span data-stu-id="4d4b8-152">Insert or update pattern</span></span>  

<span data-ttu-id="4d4b8-153">Распространенным шаблоном для некоторых приложений является добавление сущности как новой (в результате вставки базы данных) или присоединение сущности в качестве существующей и пометка ее как измененной (в результате обновления базы данных) в зависимости от значения первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-153">A common pattern for some applications is to either Add an entity as new (resulting in a database insert) or Attach an entity as existing and mark it as modified (resulting in a database update) depending on the value of the primary key.</span></span>
<span data-ttu-id="4d4b8-154">Например, при использовании сформированных в базе данных целочисленных первичных ключей обычно считается, что сущность с нулевым ключом не является новой и сущность с ненулевым ключом уже существует.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-154">For example, when using database generated integer primary keys it is common to treat an entity with a zero key as new and an entity with a non-zero key as existing.</span></span>
<span data-ttu-id="4d4b8-155">Этот шаблон можно достичь, задав состояние сущности на основе проверки значения первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-155">This pattern can be achieved by setting the entity state based on a check of the primary key value.</span></span> <span data-ttu-id="4d4b8-156">Пример:</span><span class="sxs-lookup"><span data-stu-id="4d4b8-156">For example:</span></span>  

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

<span data-ttu-id="4d4b8-157">Обратите внимание, что при изменении состояния на измененные все свойства сущности будут помечены как измененные, а все значения свойств будут отправляться в базу данных при вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="4d4b8-157">Note that when you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  
