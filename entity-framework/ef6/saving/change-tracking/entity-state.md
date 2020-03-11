---
title: Работа с состояниями сущностей — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
ms.openlocfilehash: ef0e8d5a5a9d66adab7046088c49d8cd472edc8a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416255"
---
# <a name="working-with-entity-states"></a><span data-ttu-id="ff1a3-102">Работа с состояниями сущностей</span><span class="sxs-lookup"><span data-stu-id="ff1a3-102">Working with entity states</span></span>
<span data-ttu-id="ff1a3-103">В этом разделе рассматривается добавление и присоединение сущностей к контексту и то, как Entity Framework обрабатывает их во время вызова метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-103">This topic will cover how to add and attach entities to a context and how Entity Framework processes these during SaveChanges.</span></span>
<span data-ttu-id="ff1a3-104">Entity Framework следит за отслеживанием состояния сущностей, когда они подключены к контексту, но в отключенных или N-ярусных сценариях EF можно сообщить о том, в каком состоянии будут находиться ваши сущности.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-104">Entity Framework takes care of tracking the state of entities while they are connected to a context, but in disconnected or N-Tier scenarios you can let EF know what state your entities should be in.</span></span>
<span data-ttu-id="ff1a3-105">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="entity-states-and-savechanges"></a><span data-ttu-id="ff1a3-106">Состояния сущностей и SaveChanges</span><span class="sxs-lookup"><span data-stu-id="ff1a3-106">Entity states and SaveChanges</span></span>

<span data-ttu-id="ff1a3-107">Сущность может находиться в одном из пяти состояний, как определено в перечислении EntityState.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-107">An entity can be in one of five states as defined by the EntityState enumeration.</span></span> <span data-ttu-id="ff1a3-108">Возможны следующие состояния:</span><span class="sxs-lookup"><span data-stu-id="ff1a3-108">These states are:</span></span>  

- <span data-ttu-id="ff1a3-109">Добавлено: сущность отслеживается контекстом, но еще не существует в базе данных.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-109">Added: the entity is being tracked by the context but does not yet exist in the database</span></span>  
- <span data-ttu-id="ff1a3-110">Без изменений: сущность отслеживается контекстом и существует в базе данных, а значения ее свойств не изменяются из значений в базе данных.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-110">Unchanged: the entity is being tracked by the context and exists in the database, and its property values have not changed from the values in the database</span></span>  
- <span data-ttu-id="ff1a3-111">Изменено: сущность отслеживается контекстом и существует в базе данных, а некоторые или все значения ее свойств были изменены.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-111">Modified: the entity is being tracked by the context and exists in the database, and some or all of its property values have been modified</span></span>  
- <span data-ttu-id="ff1a3-112">Удалено: сущность отслеживается контекстом и существует в базе данных, но была помечена для удаления из базы данных при следующем вызове SaveChanges</span><span class="sxs-lookup"><span data-stu-id="ff1a3-112">Deleted: the entity is being tracked by the context and exists in the database, but has been marked for deletion from the database the next time SaveChanges is called</span></span>  
- <span data-ttu-id="ff1a3-113">Отсоединено: сущность не отслеживается контекстом</span><span class="sxs-lookup"><span data-stu-id="ff1a3-113">Detached: the entity is not being tracked by the context</span></span>  

<span data-ttu-id="ff1a3-114">SaveChanges выполняет различные операции с сущностями в различных состояниях:</span><span class="sxs-lookup"><span data-stu-id="ff1a3-114">SaveChanges does different things for entities in different states:</span></span>  

- <span data-ttu-id="ff1a3-115">Неизмененные сущности не затрагиваются функцией SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-115">Unchanged entities are not touched by SaveChanges.</span></span> <span data-ttu-id="ff1a3-116">Обновления не отправляются в базу данных для сущностей в неизмененном состоянии.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-116">Updates are not sent to the database for entities in the Unchanged state.</span></span>  
- <span data-ttu-id="ff1a3-117">Добавленные сущности вставляются в базу данных, а затем становятся неизменными при возврате SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-117">Added entities are inserted into the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="ff1a3-118">Измененные сущности обновляются в базе данных, а затем становятся неизменными при возврате SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-118">Modified entities are updated in the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="ff1a3-119">Удаленные сущности удаляются из базы данных и затем отсоединяются из контекста.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-119">Deleted entities are deleted from the database and are then detached from the context.</span></span>  

<span data-ttu-id="ff1a3-120">В следующих примерах показаны способы изменения состояния сущности или графа сущностей.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-120">The following examples show ways in which the state of an entity or an entity graph can be changed.</span></span>  

## <a name="adding-a-new-entity-to-the-context"></a><span data-ttu-id="ff1a3-121">Добавление новой сущности в контекст</span><span class="sxs-lookup"><span data-stu-id="ff1a3-121">Adding a new entity to the context</span></span>  

<span data-ttu-id="ff1a3-122">Новую сущность можно добавить в контекст, вызвав метод Add для DbSet.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-122">A new entity can be added to the context by calling the Add method on DbSet.</span></span>
<span data-ttu-id="ff1a3-123">При этом сущность помещается в добавленное состояние, что означает, что она будет вставлена в базу данных при следующем вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-123">This puts the entity into the Added state, meaning that it will be inserted into the database the next time that SaveChanges is called.</span></span>
<span data-ttu-id="ff1a3-124">Пример:</span><span class="sxs-lookup"><span data-stu-id="ff1a3-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

<span data-ttu-id="ff1a3-125">Другим способом добавления новой сущности в контекст является изменение ее состояния на "Добавлено".</span><span class="sxs-lookup"><span data-stu-id="ff1a3-125">Another way to add a new entity to the context is to change its state to Added.</span></span> <span data-ttu-id="ff1a3-126">Пример:</span><span class="sxs-lookup"><span data-stu-id="ff1a3-126">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

<span data-ttu-id="ff1a3-127">Наконец, можно добавить новую сущность в контекст, применив ее к другой сущности, которая уже отслеживается.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-127">Finally, you can add a new entity to the context by hooking it up to another entity that is already being tracked.</span></span>
<span data-ttu-id="ff1a3-128">Это может быть вызвано добавлением новой сущности к свойству навигации коллекции другой сущности или установкой свойства навигации по ссылке другой сущности для указания на новую сущность.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-128">This could be by adding the new entity to the collection navigation property of another entity or by setting a reference navigation property of another entity to point to the new entity.</span></span> <span data-ttu-id="ff1a3-129">Пример:</span><span class="sxs-lookup"><span data-stu-id="ff1a3-129">For example:</span></span>  

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

<span data-ttu-id="ff1a3-130">Обратите внимание, что для всех этих примеров, если добавляемая сущность содержит ссылки на другие сущности, которые еще не были отслеживаемы, эти новые сущности также будут добавлены в контекст и будут вставлены в базу данных при следующем вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-130">Note that for all of these examples if the entity being added has references to other entities that are not yet tracked then these new entities will also be added to the context and will be inserted into the database the next time that SaveChanges is called.</span></span>  

## <a name="attaching-an-existing-entity-to-the-context"></a><span data-ttu-id="ff1a3-131">Присоединение существующей сущности к контексту</span><span class="sxs-lookup"><span data-stu-id="ff1a3-131">Attaching an existing entity to the context</span></span>  

<span data-ttu-id="ff1a3-132">Если у вас есть сущность, которая уже существует в базе данных, но в настоящий момент не отслеживается контекстом, можно указать контексту отслеживание сущности с помощью метода Attach в DbSet.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-132">If you have an entity that you know already exists in the database but which is not currently being tracked by the context then you can tell the context to track the entity using the Attach method on DbSet.</span></span> <span data-ttu-id="ff1a3-133">Сущность будет находиться в неизмененном состоянии в контексте.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-133">The entity will be in the Unchanged state in the context.</span></span> <span data-ttu-id="ff1a3-134">Пример:</span><span class="sxs-lookup"><span data-stu-id="ff1a3-134">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="ff1a3-135">Обратите внимание, что в базу данных не вносятся изменения, если метод SaveChanges вызывается без выполнения других манипуляций с присоединенной сущностью.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-135">Note that no changes will be made to the database if SaveChanges is called without doing any other manipulation of the attached entity.</span></span> <span data-ttu-id="ff1a3-136">Это вызвано тем, что сущность находится в неизмененном состоянии.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-136">This is because the entity is in the Unchanged state.</span></span>  

<span data-ttu-id="ff1a3-137">Еще один способ присоединить существующую сущность к контексту — изменить его состояние на без изменений.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-137">Another way to attach an existing entity to the context is to change its state to Unchanged.</span></span> <span data-ttu-id="ff1a3-138">Пример:</span><span class="sxs-lookup"><span data-stu-id="ff1a3-138">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="ff1a3-139">Обратите внимание, что в обоих примерах, если присоединяемая сущность имеет ссылки на другие сущности, которые еще не отслеживаемы, эти новые сущности также будут присоединены к контексту в неизмененном состоянии.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-139">Note that for both of these examples if the entity being attached has references to other entities that are not yet tracked then these new entities will also attached to the context in the Unchanged state.</span></span>  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a><span data-ttu-id="ff1a3-140">Присоединение существующей, но измененной сущности к контексту</span><span class="sxs-lookup"><span data-stu-id="ff1a3-140">Attaching an existing but modified entity to the context</span></span>  

<span data-ttu-id="ff1a3-141">Если у вас есть сущность, которая уже существует в базе данных, но изменения могли быть внесены, можно указать контексту присоединить сущность и присвоить ей состояние Modified.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-141">If you have an entity that you know already exists in the database but to which changes may have been made then you can tell the context to attach the entity and set its state to Modified.</span></span>
<span data-ttu-id="ff1a3-142">Пример:</span><span class="sxs-lookup"><span data-stu-id="ff1a3-142">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="ff1a3-143">При изменении состояния на измененные все свойства сущности будут помечены как измененные, а все значения свойств будут отправляться в базу данных при вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-143">When you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  

<span data-ttu-id="ff1a3-144">Обратите внимание, что если присоединяемая сущность содержит ссылки на другие сущности, которые еще не отслеживаемы, то эти новые сущности будут присоединены к контексту в неизмененном состоянии — они не будут автоматически изменены.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-144">Note that if the entity being attached has references to other entities that are not yet tracked, then these new entities will attached to the context in the Unchanged state—they will not automatically be made Modified.</span></span>
<span data-ttu-id="ff1a3-145">При наличии нескольких сущностей, которые необходимо пометить как измененные, необходимо задать состояние для каждой из этих сущностей по отдельности.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-145">If you have multiple entities that need to be marked Modified you should set the state for each of these entities individually.</span></span>  

## <a name="changing-the-state-of-a-tracked-entity"></a><span data-ttu-id="ff1a3-146">Изменение состояния отслеживающей сущности</span><span class="sxs-lookup"><span data-stu-id="ff1a3-146">Changing the state of a tracked entity</span></span>  

<span data-ttu-id="ff1a3-147">Состояние сущности, которая уже отслеживается, можно изменить, задав свойство State для его записи.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-147">You can change the state of an entity that is already being tracked by setting the State property on its entry.</span></span> <span data-ttu-id="ff1a3-148">Пример:</span><span class="sxs-lookup"><span data-stu-id="ff1a3-148">For example:</span></span>  

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

<span data-ttu-id="ff1a3-149">Обратите внимание, что вызов Add или Attach для сущности, которая уже зарегистрирована, может также использоваться для изменения состояния сущности.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-149">Note that calling Add or Attach for an entity that is already tracked can also be used to change the entity state.</span></span> <span data-ttu-id="ff1a3-150">Например, вызов attach для сущности, которая в данный момент находится в состоянии добавления, изменит свое состояние на без изменений.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-150">For example, calling Attach for an entity that is currently in the Added state will change its state to Unchanged.</span></span>  

## <a name="insert-or-update-pattern"></a><span data-ttu-id="ff1a3-151">Вставка или обновление шаблона</span><span class="sxs-lookup"><span data-stu-id="ff1a3-151">Insert or update pattern</span></span>  

<span data-ttu-id="ff1a3-152">Распространенным шаблоном для некоторых приложений является добавление сущности как новой (в результате вставки базы данных) или присоединение сущности в качестве существующей и пометка ее как измененной (в результате обновления базы данных) в зависимости от значения первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-152">A common pattern for some applications is to either Add an entity as new (resulting in a database insert) or Attach an entity as existing and mark it as modified (resulting in a database update) depending on the value of the primary key.</span></span>
<span data-ttu-id="ff1a3-153">Например, при использовании сформированных в базе данных целочисленных первичных ключей обычно считается, что сущность с нулевым ключом не является новой и сущность с ненулевым ключом уже существует.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-153">For example, when using database generated integer primary keys it is common to treat an entity with a zero key as new and an entity with a non-zero key as existing.</span></span>
<span data-ttu-id="ff1a3-154">Этот шаблон можно достичь, задав состояние сущности на основе проверки значения первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-154">This pattern can be achieved by setting the entity state based on a check of the primary key value.</span></span> <span data-ttu-id="ff1a3-155">Пример:</span><span class="sxs-lookup"><span data-stu-id="ff1a3-155">For example:</span></span>  

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

<span data-ttu-id="ff1a3-156">Обратите внимание, что при изменении состояния на измененные все свойства сущности будут помечены как измененные, а все значения свойств будут отправляться в базу данных при вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="ff1a3-156">Note that when you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  
