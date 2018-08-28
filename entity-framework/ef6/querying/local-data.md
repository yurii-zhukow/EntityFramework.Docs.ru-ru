---
title: Локальные данные - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
ms.openlocfilehash: dac1a1de20398501c706b118443743d47970df17
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994278"
---
# <a name="local-data"></a><span data-ttu-id="6d5d1-102">Локальные данные</span><span class="sxs-lookup"><span data-stu-id="6d5d1-102">Local Data</span></span>
<span data-ttu-id="6d5d1-103">Выполняется запрос LINQ непосредственно к DbSet, всегда отправляет запрос к базе данных, но можно получить доступ к данных, который в данный момент в памяти с помощью свойства DbSet.Local.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-103">Running a LINQ query directly against a DbSet will always send a query to the database, but you can access the data that is currently in-memory using the DbSet.Local property.</span></span> <span data-ttu-id="6d5d1-104">Можно также открыть нужные дополнительные данные отслеживания EF о сущности с помощью методов DbContext.Entry и DbContext.ChangeTracker.Entries.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-104">You can also access the extra information EF is tracking about your entities using the DbContext.Entry and DbContext.ChangeTracker.Entries methods.</span></span> <span data-ttu-id="6d5d1-105">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="using-local-to-look-at-local-data"></a><span data-ttu-id="6d5d1-106">Использовать локальную для просмотра локальных данных</span><span class="sxs-lookup"><span data-stu-id="6d5d1-106">Using Local to look at local data</span></span>  

<span data-ttu-id="6d5d1-107">Локальное свойство DbSet предоставляет простой доступ к сущностей из набора, которые отслеживаются контекстом и не были помечены как удаленные.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-107">The Local property of DbSet provides simple access to the entities of the set that are currently being tracked by the context and have not been marked as Deleted.</span></span> <span data-ttu-id="6d5d1-108">Доступ к локальному свойству никогда не вызывает запрос для отправки в базу данных.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-108">Accessing the Local property never causes a query to be sent to the database.</span></span> <span data-ttu-id="6d5d1-109">Это означает, что оно обычно используется после запроса уже была выполнена.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-109">This means that it is usually used after a query has already been performed.</span></span> <span data-ttu-id="6d5d1-110">Метода расширения Load может использоваться для выполнения запроса, чтобы контекст отслеживает результаты.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-110">The Load extension method can be used to execute a query so that the context tracks the results.</span></span> <span data-ttu-id="6d5d1-111">Пример:</span><span class="sxs-lookup"><span data-stu-id="6d5d1-111">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs from the database into the context
    context.Blogs.Load();

    // Add a new blog to the context
    context.Blogs.Add(new Blog { Name = "My New Blog" });

    // Mark one of the existing blogs as Deleted
    context.Blogs.Remove(context.Blogs.Find(1));

    // Loop over the blogs in the context.
    Console.WriteLine("In Local: ");
    foreach (var blog in context.Blogs.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }

    // Perform a query against the database.
    Console.WriteLine("\nIn DbSet query: ");
    foreach (var blog in context.Blogs)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }
}
```  

<span data-ttu-id="6d5d1-112">Если у нас два блоги в базе данных - «ADO.NET блог» с BlogId 1 - и «Блоге по Visual Studio» с BlogId 2 мы могла бы ожидать следующие выходные данные:</span><span class="sxs-lookup"><span data-stu-id="6d5d1-112">If we had two blogs in the database - 'ADO.NET Blog' with a BlogId of 1 and 'The Visual Studio Blog' with a BlogId of 2 - we could expect the following output:</span></span>  

```  
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

<span data-ttu-id="6d5d1-113">Это иллюстрирует три точки:</span><span class="sxs-lookup"><span data-stu-id="6d5d1-113">This illustrates three points:</span></span>  

- <span data-ttu-id="6d5d1-114">Новый блог «Мой новый блог» входит в локальную коллекцию, несмотря на то, что он еще не были сохранены в базе данных.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-114">The new blog 'My New Blog' is included in the Local collection even though it has not yet been saved to the database.</span></span> <span data-ttu-id="6d5d1-115">Этот блог имеет первичный ключ нулевой, так как в базе данных еще не создал фактическую клавишу для сущности.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-115">This blog has a primary key of zero because the database has not yet generated a real key for the entity.</span></span>  
- <span data-ttu-id="6d5d1-116">В «записи ADO.NET» отсутствует в локальной коллекции, несмотря на то, что он по-прежнему отслеживается контекстом.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-116">The 'ADO.NET Blog' is not included in the local collection even though it is still being tracked by the context.</span></span> <span data-ttu-id="6d5d1-117">Это так, как мы удалили из DbSet, тем самым пометив его как удаленные.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-117">This is because we removed it from the DbSet thereby marking it as deleted.</span></span>  
- <span data-ttu-id="6d5d1-118">Когда DbSet, используется для выполнения запроса блога, помечен для удаления (блог ADO.NET) включается в результаты и нового блога (Мой новый блог), который еще не были сохранены в базе данных не включается в результаты.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-118">When DbSet is used to perform a query the blog marked for deletion (ADO.NET Blog) is included in the results and the new blog (My New Blog) that has not yet been saved to the database is not included in the results.</span></span> <span data-ttu-id="6d5d1-119">Это обусловлено DbSet выполняет запрос к базе данных и результатов, возвращаемых всегда оно совпадало с указанным в базе данных.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-119">This is because DbSet is performing a query against the database and the results returned always reflect what is in the database.</span></span>  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a><span data-ttu-id="6d5d1-120">Использовать локальную для добавления и удаления сущностей из контекста</span><span class="sxs-lookup"><span data-stu-id="6d5d1-120">Using Local to add and remove entities from the context</span></span>  

<span data-ttu-id="6d5d1-121">Возвращает свойства локально DbSet [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) с событиями, привязываем ее таким образом, что он остается синхронизированной с содержимое контекста.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-121">The Local property on DbSet returns an [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) with events hooked up such that it stays in sync with the contents of the context.</span></span> <span data-ttu-id="6d5d1-122">Это означает, что сущности можно добавлять или удалять из локальной коллекции или DbSet.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-122">This means that entities can be added or removed from either the Local collection or the DbSet.</span></span> <span data-ttu-id="6d5d1-123">Это также означает, что запросы, которые привносят новые сущности в контекст, приведет к локальной коллекции, обновляемой с этими сущностями.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-123">It also means that queries that bring new entities into the context will result in the Local collection being updated with those entities.</span></span> <span data-ttu-id="6d5d1-124">Пример:</span><span class="sxs-lookup"><span data-stu-id="6d5d1-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load some posts from the database into the context
    context.Posts.Where(p => p.Tags.Contains("entity-framework").Load();  

    // Get the local collection and make some changes to it
    var localPosts = context.Posts.Local;
    localPosts.Add(new Post { Name = "What's New in EF" });
    localPosts.Remove(context.Posts.Find(1));  

    // Loop over the posts in the context.
    Console.WriteLine("In Local after entity-framework query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }

    var post1 = context.Posts.Find(1);
    Console.WriteLine(
        "State of post 1: {0} is {1}",
        post1.Name,  
        context.Entry(post1).State);  

    // Query some more posts from the database
    context.Posts.Where(p => p.Tags.Contains("asp.net").Load();  

    // Loop over the posts in the context again.
    Console.WriteLine("\nIn Local after asp.net query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }
}
```  

<span data-ttu-id="6d5d1-125">При условии, что у нас было несколько помеченные с «entity framework» и «asp.net» выходные данные записи может выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="6d5d1-125">Assuming we had a few posts tagged with 'entity-framework' and 'asp.net' the output may look something like this:</span></span>  

```  
In Local after entity-framework query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
State of post 1: EF Beginners Guide is Deleted

In Local after asp.net query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
Found 4: ASP.NET Beginners Guide with state Unchanged
```  

<span data-ttu-id="6d5d1-126">Это иллюстрирует три точки:</span><span class="sxs-lookup"><span data-stu-id="6d5d1-126">This illustrates three points:</span></span>  

- <span data-ttu-id="6d5d1-127">«Новые возможности в EF» новую запись, добавленный к локальной коллекции становится отслеживаемая в контексте в добавленном состоянии.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-127">The new post 'What's New in EF' that was added to the Local collection becomes tracked by the context in the Added state.</span></span> <span data-ttu-id="6d5d1-128">Она будет таким образом вставлена в базу данных при вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-128">It will therefore be inserted into the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="6d5d1-129">Запрос post, который был удален из локальной коллекции (руководство по профилированию EF) теперь помечены как удаленные в контексте.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-129">The post that was removed from the Local collection (EF Beginners Guide) is now marked as deleted in the context.</span></span> <span data-ttu-id="6d5d1-130">Его таким образом удаляются из базы данных при вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-130">It will therefore be deleted from the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="6d5d1-131">Дополнительные post (руководство по профилированию ASP.NET), загруженной в контекст, с помощью второй запрос автоматически добавляется к локальной коллекции.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-131">The additional post (ASP.NET Beginners Guide) loaded into the context with the second query is automatically added to the Local collection.</span></span>  

<span data-ttu-id="6d5d1-132">Заключительную следует помнить о локальной обусловлено тем, что ObservableCollection производительности не очень удобно для большого количества сущностей.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-132">One final thing to note about Local is that because it is an ObservableCollection performance is not great for large numbers of entities.</span></span> <span data-ttu-id="6d5d1-133">Поэтому если вы имеете дело с тысячами сущностей в контексте может оказаться рекомендуется использовать локальный.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-133">Therefore if you are dealing with thousands of entities in your context it may not be advisable to use Local.</span></span>  

## <a name="using-local-for-wpf-data-binding"></a><span data-ttu-id="6d5d1-134">Использовать локальную для привязки данных WPF</span><span class="sxs-lookup"><span data-stu-id="6d5d1-134">Using Local for WPF data binding</span></span>  

<span data-ttu-id="6d5d1-135">Свойства DbSet локально можно использовать непосредственно для привязки данных в приложении WPF, так как он является экземпляром ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-135">The Local property on DbSet can be used directly for data binding in a WPF application because it is an instance of ObservableCollection.</span></span> <span data-ttu-id="6d5d1-136">Как описано в предыдущих разделах, это означает, что он будет автоматически синхронизироваться с содержимое контекста и содержимое контекста будут автоматически синхронизированы с ним.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-136">As described in the previous sections this means that it will automatically stay in sync with the contents of the context and the contents of the context will automatically stay in sync with it.</span></span> <span data-ttu-id="6d5d1-137">Обратите внимание на то, что вам нужно предварительно заполнить локальную коллекцию с данными не исключена действий, которые необходимо привязать к, так как локальный никогда не вызывает запрос к базе данных.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-137">Note that you do need to pre-populate the Local collection with data for there to be anything to bind to since Local never causes a database query.</span></span>  

<span data-ttu-id="6d5d1-138">Это не является подходящим местом для полный пример привязки данных WPF, но являются ключевые элементы:</span><span class="sxs-lookup"><span data-stu-id="6d5d1-138">This is not an appropriate place for a full WPF data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="6d5d1-139">Настройка источника привязки</span><span class="sxs-lookup"><span data-stu-id="6d5d1-139">Setup a binding source</span></span>  
- <span data-ttu-id="6d5d1-140">Привяжите его к локальному свойству набора параметров</span><span class="sxs-lookup"><span data-stu-id="6d5d1-140">Bind it to the Local property of your set</span></span>  
- <span data-ttu-id="6d5d1-141">Заполнение локальной, с помощью запроса к базе данных.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-141">Populate Local using a query to the database.</span></span>  

## <a name="wpf-binding-to-navigation-properties"></a><span data-ttu-id="6d5d1-142">Привязки WPF для свойств навигации</span><span class="sxs-lookup"><span data-stu-id="6d5d1-142">WPF binding to navigation properties</span></span>  

<span data-ttu-id="6d5d1-143">Если вы выполняете данных «основной/подробности», то может потребоваться привязать к свойству навигации, одной из сущностей представлении «Подробности».</span><span class="sxs-lookup"><span data-stu-id="6d5d1-143">If you are doing master/detail data binding you may want to bind the detail view to a navigation property of one of your entities.</span></span> <span data-ttu-id="6d5d1-144">Простой способ выполнения этой работы является использование коллекции ObservableCollection для свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-144">An easy way to make this work is to use an ObservableCollection for the navigation property.</span></span> <span data-ttu-id="6d5d1-145">Пример:</span><span class="sxs-lookup"><span data-stu-id="6d5d1-145">For example:</span></span>  

``` csharp
public class Blog
{
    private readonly ObservableCollection<Post> _posts =
        new ObservableCollection<Post>();

    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual ObservableCollection<Post> Posts
    {
        get { return _posts; }
    }
}
```  

## <a name="using-local-to-clean-up-entities-in-savechanges"></a><span data-ttu-id="6d5d1-146">Использовать локальную для очистки сущностей в SaveChanges</span><span class="sxs-lookup"><span data-stu-id="6d5d1-146">Using Local to clean up entities in SaveChanges</span></span>  

<span data-ttu-id="6d5d1-147">В большинстве случаев сущности удалены из свойства навигации не помечается, автоматически как удаленный в контексте.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-147">In most cases entities removed from a navigation property will not be automatically marked as deleted in the context.</span></span> <span data-ttu-id="6d5d1-148">Например при удалении объекта Post из коллекции Blog.Posts, затем выполните, не удаляются автоматически при вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-148">For example, if you remove a Post object from the Blog.Posts collection then that post will not be automatically deleted when SaveChanges is called.</span></span> <span data-ttu-id="6d5d1-149">Если это необходимо для удаления затем может потребоваться найти эти несвязанные сущности и пометить их как удаленные, перед вызовом метода SaveChanges, или как часть переопределенного метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-149">If you need it to be deleted then you may need to find these dangling entities and mark them as deleted before calling SaveChanges or as part of an overridden SaveChanges.</span></span> <span data-ttu-id="6d5d1-150">Пример:</span><span class="sxs-lookup"><span data-stu-id="6d5d1-150">For example:</span></span>  

``` csharp
public override int SaveChanges()
{
    foreach (var post in this.Posts.Local.ToList())
    {
        if (post.Blog == null)
        {
            this.Posts.Remove(post);
        }
    }

    return base.SaveChanges();
}
```  

<span data-ttu-id="6d5d1-151">Приведенный выше код использует локальную коллекцию, чтобы найти все сообщения и метки, которые не имеют ссылку на блог как удаленные.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-151">The code above uses the Local collection to find all posts and marks any that do not have a blog reference as deleted.</span></span> <span data-ttu-id="6d5d1-152">Вызов ToList является обязательным, поскольку в противном случае коллекции будет изменена Remove вызвать во время его перечисления.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-152">The ToList call is required because otherwise the collection will be modified by the Remove call while it is being enumerated.</span></span> <span data-ttu-id="6d5d1-153">В большинстве случаев вы можете запрашивать непосредственно с локального свойства без использования ToList сначала.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-153">In most other situations you can query directly against the Local property without using ToList first.</span></span>  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a><span data-ttu-id="6d5d1-154">Использование привязки данных для локальных и ToBindingList for Windows Forms</span><span class="sxs-lookup"><span data-stu-id="6d5d1-154">Using Local and ToBindingList for Windows Forms data binding</span></span>  

<span data-ttu-id="6d5d1-155">Windows Forms не поддерживает привязку данных полноценной непосредственно с помощью ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-155">Windows Forms does not support full fidelity data binding using ObservableCollection directly.</span></span> <span data-ttu-id="6d5d1-156">Тем не менее чтобы получить все преимущества, описанные в предыдущих разделах по-прежнему можно использовать DbSet локального свойства для привязки данных.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-156">However, you can still use the DbSet Local property for data binding to get all the benefits described in the previous sections.</span></span> <span data-ttu-id="6d5d1-157">Это достигается с помощью метода расширения ToBindingList, который создает [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) реализации поддерживаемый локальной коллекции ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-157">This is achieved through the ToBindingList extension method which creates an [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementation backed by the Local ObservableCollection.</span></span>  

<span data-ttu-id="6d5d1-158">Это не является подходящим местом для полный пример привязки данных Windows Forms, но являются ключевые элементы:</span><span class="sxs-lookup"><span data-stu-id="6d5d1-158">This is not an appropriate place for a full Windows Forms data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="6d5d1-159">Настройка объекта источника привязки</span><span class="sxs-lookup"><span data-stu-id="6d5d1-159">Setup an object binding source</span></span>  
- <span data-ttu-id="6d5d1-160">Привяжите его к локальному свойству с помощью Local.ToBindingList() набора</span><span class="sxs-lookup"><span data-stu-id="6d5d1-160">Bind it to the Local property of your set using Local.ToBindingList()</span></span>  
- <span data-ttu-id="6d5d1-161">Заполнение локальной, с помощью запроса к базе данных</span><span class="sxs-lookup"><span data-stu-id="6d5d1-161">Populate Local using a query to the database</span></span>  

## <a name="getting-detailed-information-about-tracked-entities"></a><span data-ttu-id="6d5d1-162">Получение подробных сведений о отслеживаемые сущности</span><span class="sxs-lookup"><span data-stu-id="6d5d1-162">Getting detailed information about tracked entities</span></span>  

<span data-ttu-id="6d5d1-163">Во многих примерах в этой серии запись метод позволяет вернуть экземпляр dbentityentry, который должен для сущности.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-163">Many of the examples in this series use the Entry method to return a DbEntityEntry instance for an entity.</span></span> <span data-ttu-id="6d5d1-164">Затем этот объект записи выступает в качестве отправной точки для сбора сведений о сущности, например текущего состояния, а также для выполнения операций с сущностями, например явная загрузка связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-164">This entry object then acts as the starting point for gathering information about the entity such as its current state, as well as for performing operations on the entity such as explicitly loading a related entity.</span></span>  

<span data-ttu-id="6d5d1-165">Методы операции возвращают объекты dbentityentry, который должен для нескольких или всех сущностей, отслеживается контекстом.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-165">The Entries methods return DbEntityEntry objects for many or all entities being tracked by the context.</span></span> <span data-ttu-id="6d5d1-166">Это позволяет собирать информацию или выполнить операции на множество сущностей, а не только одна запись.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-166">This allows you to gather information or perform operations on many entities rather than just a single entry.</span></span> <span data-ttu-id="6d5d1-167">Пример:</span><span class="sxs-lookup"><span data-stu-id="6d5d1-167">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load some entities into the context
    context.Blogs.Load();
    context.Authors.Load();
    context.Readers.Load();

    // Make some changes
    context.Blogs.Find(1).Title = "The New ADO.NET Blog";
    context.Blogs.Remove(context.Blogs.Find(2));
    context.Authors.Add(new Author { Name = "Jane Doe" });
    context.Readers.Find(1).Username = "johndoe1987";

    // Look at the state of all entities in the context
    Console.WriteLine("All tracked entities: ");
    foreach (var entry in context.ChangeTracker.Entries())
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Find modified entities of any type
    Console.WriteLine("\nAll modified entities: ");
    foreach (var entry in context.ChangeTracker.Entries()
                              .Where(e => e.State == EntityState.Modified))
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Get some information about just the tracked blogs
    Console.WriteLine("\nTracked blogs: ");
    foreach (var entry in context.ChangeTracker.Entries<Blog>())
    {
        Console.WriteLine(
            "Found Blog {0}: {1} with original Name {2}",
            entry.Entity.BlogId,  
            entry.Entity.Name,
            entry.Property(p => p.Name).OriginalValue);
    }

    // Find all people (author or reader)
    Console.WriteLine("\nPeople: ");
    foreach (var entry in context.ChangeTracker.Entries<IPerson>())
    {
        Console.WriteLine("Found Person {0}", entry.Entity.Name);
    }
}
```  

<span data-ttu-id="6d5d1-168">Можно заметить, мы представляем класс автора и чтения в пример — оба эти класса реализуют интерфейс IPerson.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-168">You'll notice we are introducing a Author and Reader class into the example - both of these classes implement the IPerson interface.</span></span>  

``` csharp
public class Author : IPerson
{
    public int AuthorId { get; set; }
    public string Name { get; set; }
    public string Biography { get; set; }
}

public class Reader : IPerson
{
    public int ReaderId { get; set; }
    public string Name { get; set; }
    public string Username { get; set; }
}

public interface IPerson
{
    string Name { get; }
}
```  

<span data-ttu-id="6d5d1-169">Предположим, что у нас есть следующие данные в базе данных:</span><span class="sxs-lookup"><span data-stu-id="6d5d1-169">Let's assume we have the following data in the database:</span></span>

<span data-ttu-id="6d5d1-170">Блог с BlogId = 1 "и" имя = «Блог ADO.NET»</span><span class="sxs-lookup"><span data-stu-id="6d5d1-170">Blog with BlogId = 1 and Name = 'ADO.NET Blog'</span></span>  
<span data-ttu-id="6d5d1-171">Блог с BlogId = 2, а также имя = «Блог Visual Studio»</span><span class="sxs-lookup"><span data-stu-id="6d5d1-171">Blog with BlogId = 2 and Name = 'The Visual Studio Blog'</span></span>  
<span data-ttu-id="6d5d1-172">Блог с BlogId = 3 "и" имя = «Блог по .NET Framework»</span><span class="sxs-lookup"><span data-stu-id="6d5d1-172">Blog with BlogId = 3 and Name = '.NET Framework Blog'</span></span>  
<span data-ttu-id="6d5d1-173">Разработка с помощью AuthorId = 1 "и" имя = «Joe Bloggs»</span><span class="sxs-lookup"><span data-stu-id="6d5d1-173">Author with AuthorId = 1 and Name = 'Joe Bloggs'</span></span>  
<span data-ttu-id="6d5d1-174">Средства чтения с ReaderId = 1 "и" имя = «John Doe»</span><span class="sxs-lookup"><span data-stu-id="6d5d1-174">Reader with ReaderId = 1 and Name = 'John Doe'</span></span>  

<span data-ttu-id="6d5d1-175">В результате выполнения кода будет следующим:</span><span class="sxs-lookup"><span data-stu-id="6d5d1-175">The output from running the code would be:</span></span>  

```  
All tracked entities:
Found entity of type Blog with state Modified
Found entity of type Blog with state Deleted
Found entity of type Blog with state Unchanged
Found entity of type Author with state Unchanged
Found entity of type Author with state Added
Found entity of type Reader with state Modified

All modified entities:
Found entity of type Blog with state Modified
Found entity of type Reader with state Modified

Tracked blogs:
Found Blog 1: The New ADO.NET Blog with original Name ADO.NET Blog
Found Blog 2: The Visual Studio Blog with original Name The Visual Studio Blog
Found Blog 3: .NET Framework Blog with original Name .NET Framework Blog

People:
Found Person John Doe
Found Person Joe Bloggs
Found Person Jane Doe
```  

<span data-ttu-id="6d5d1-176">В следующих примерах показаны несколько точек:</span><span class="sxs-lookup"><span data-stu-id="6d5d1-176">These examples illustrate several points:</span></span>  

- <span data-ttu-id="6d5d1-177">Методы записи, возвращают записи для сущности во всех состояниях, включая удаленные.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-177">The Entries methods return entries for entities in all states, including Deleted.</span></span> <span data-ttu-id="6d5d1-178">Сравните это для локального параметра, который исключает удаление сущностей.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-178">Compare this to Local which excludes Deleted entities.</span></span>  
- <span data-ttu-id="6d5d1-179">При использовании метода нестандартную записей, возвращаются записи для всех типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-179">Entries for all entity types are returned when the non-generic Entries method is used.</span></span> <span data-ttu-id="6d5d1-180">При использовании метода универсального записи только возвращаются записи для сущностей, которые являются экземплярами универсального типа.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-180">When the generic entries method is used entries are only returned for entities that are instances of the generic type.</span></span> <span data-ttu-id="6d5d1-181">Использовался выше для получения записи для всех блогах.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-181">This was used above to get entries for all blogs.</span></span> <span data-ttu-id="6d5d1-182">Он также использовался для получения записи для всех сущностей, которые реализуют IPerson.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-182">It was also used to get entries for all entities that implement IPerson.</span></span> <span data-ttu-id="6d5d1-183">Это показывает, что универсальный тип имеет тип фактического объекта.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-183">This demonstrates that the generic type does not have to be an actual entity type.</span></span>  
- <span data-ttu-id="6d5d1-184">LINQ к объектам можно использовать для фильтрации возвращаемых результатов.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-184">LINQ to Objects can be used to filter the results returned.</span></span> <span data-ttu-id="6d5d1-185">Использовался выше для поиска объектов любого типа, до тех пор, пока они будут изменены.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-185">This was used above to find entities of any type as long as they are modified.</span></span>  

<span data-ttu-id="6d5d1-186">Обратите внимание, что экземпляры dbentityentry, который должен всегда содержат сущности отличное от null.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-186">Note that DbEntityEntry instances always contain a non-null Entity.</span></span> <span data-ttu-id="6d5d1-187">Записей отношений и записи заглушки не представлены в виде экземпляров dbentityentry, который должен, нет необходимости для фильтрации для них.</span><span class="sxs-lookup"><span data-stu-id="6d5d1-187">Relationship entries and stub entries are not represented as DbEntityEntry instances so there is no need to filter for these.</span></span>
