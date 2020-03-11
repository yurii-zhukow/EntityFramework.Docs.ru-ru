---
title: Локальные данные — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
ms.openlocfilehash: efd646348d8a18bbeed2d0a0e708d4d36eb26eac
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414485"
---
# <a name="local-data"></a><span data-ttu-id="f3ea9-102">Локальные данные</span><span class="sxs-lookup"><span data-stu-id="f3ea9-102">Local Data</span></span>
<span data-ttu-id="f3ea9-103">При выполнении запроса LINQ непосредственно к DbSet будет всегда отправляться запрос к базе данных, но можно получить доступ к данным, находящихся в памяти, с помощью свойства DbSet. local.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-103">Running a LINQ query directly against a DbSet will always send a query to the database, but you can access the data that is currently in-memory using the DbSet.Local property.</span></span> <span data-ttu-id="f3ea9-104">Кроме того, можно получить доступ к дополнительной информации EF, которая отслеживает сущности с помощью методов DbContext. Entry и DbContext. ChangeTracker .s.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-104">You can also access the extra information EF is tracking about your entities using the DbContext.Entry and DbContext.ChangeTracker.Entries methods.</span></span> <span data-ttu-id="f3ea9-105">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="using-local-to-look-at-local-data"></a><span data-ttu-id="f3ea9-106">Использование Local для просмотра локальных данных</span><span class="sxs-lookup"><span data-stu-id="f3ea9-106">Using Local to look at local data</span></span>  

<span data-ttu-id="f3ea9-107">Свойство Local объекта DbSet предоставляет простой доступ к сущностям набора, которые в настоящее время отслеживается контекстом и не помечены как удаленные.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-107">The Local property of DbSet provides simple access to the entities of the set that are currently being tracked by the context and have not been marked as Deleted.</span></span> <span data-ttu-id="f3ea9-108">Обращение к локальному свойству никогда не приводит к отправке запроса в базу данных.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-108">Accessing the Local property never causes a query to be sent to the database.</span></span> <span data-ttu-id="f3ea9-109">Это означает, что он обычно используется после того, как запрос уже выполнен.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-109">This means that it is usually used after a query has already been performed.</span></span> <span data-ttu-id="f3ea9-110">Метод расширения Load можно использовать для выполнения запроса, чтобы контекст мог отслеживать результаты.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-110">The Load extension method can be used to execute a query so that the context tracks the results.</span></span> <span data-ttu-id="f3ea9-111">Пример:</span><span class="sxs-lookup"><span data-stu-id="f3ea9-111">For example:</span></span>  

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

<span data-ttu-id="f3ea9-112">Если у нас есть два блога в базе данных — "блог ADO.NET" с Блогид 1 и "блог Visual Studio" с Блогид 2-мы можем получить следующие выходные данные:</span><span class="sxs-lookup"><span data-stu-id="f3ea9-112">If we had two blogs in the database - 'ADO.NET Blog' with a BlogId of 1 and 'The Visual Studio Blog' with a BlogId of 2 - we could expect the following output:</span></span>  

```console
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

<span data-ttu-id="f3ea9-113">Это иллюстрирует три точки:</span><span class="sxs-lookup"><span data-stu-id="f3ea9-113">This illustrates three points:</span></span>  

- <span data-ttu-id="f3ea9-114">Новый блог "мой новый блог" включен в локальную коллекцию, несмотря на то, что он еще не сохранен в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-114">The new blog 'My New Blog' is included in the Local collection even though it has not yet been saved to the database.</span></span> <span data-ttu-id="f3ea9-115">Этот блог имеет первичный ключ, равный нулю, так как база данных еще не создала реальный ключ для сущности.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-115">This blog has a primary key of zero because the database has not yet generated a real key for the entity.</span></span>  
- <span data-ttu-id="f3ea9-116">Блог ADO.NET не включен в локальную коллекцию, несмотря на то, что он по-прежнему отслеживается контекстом.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-116">The 'ADO.NET Blog' is not included in the local collection even though it is still being tracked by the context.</span></span> <span data-ttu-id="f3ea9-117">Это вызвано тем, что мы удалили его из DbSet, пометив его как удаленный.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-117">This is because we removed it from the DbSet thereby marking it as deleted.</span></span>  
- <span data-ttu-id="f3ea9-118">Когда DbSet используется для выполнения запроса, помеченный для удаления (блог ADO.NET), включается в результаты и новый блог (мой новый блог), который еще не был сохранен в базе данных, не включается в результаты.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-118">When DbSet is used to perform a query the blog marked for deletion (ADO.NET Blog) is included in the results and the new blog (My New Blog) that has not yet been saved to the database is not included in the results.</span></span> <span data-ttu-id="f3ea9-119">Это обусловлено тем, что DbSet выполняет запрос к базе данных, и возвращаемые результаты всегда отражать то, что находится в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-119">This is because DbSet is performing a query against the database and the results returned always reflect what is in the database.</span></span>  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a><span data-ttu-id="f3ea9-120">Использование Local для добавления и удаления сущностей из контекста</span><span class="sxs-lookup"><span data-stu-id="f3ea9-120">Using Local to add and remove entities from the context</span></span>  

<span data-ttu-id="f3ea9-121">Локальное свойство в DbSet возвращает [коллекцию ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) с событиями, которые были подключены таким же, что они остаются синхронизированными с содержимым контекста.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-121">The Local property on DbSet returns an [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) with events hooked up such that it stays in sync with the contents of the context.</span></span> <span data-ttu-id="f3ea9-122">Это означает, что сущности можно добавлять или удалять либо из локальной коллекции, либо из DbSet.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-122">This means that entities can be added or removed from either the Local collection or the DbSet.</span></span> <span data-ttu-id="f3ea9-123">Это также означает, что запросы, которые переносят новые сущности в контекст, приведут к обновлению локальной коллекции этими сущностями.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-123">It also means that queries that bring new entities into the context will result in the Local collection being updated with those entities.</span></span> <span data-ttu-id="f3ea9-124">Пример:</span><span class="sxs-lookup"><span data-stu-id="f3ea9-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load some posts from the database into the context
    context.Posts.Where(p => p.Tags.Contains("entity-framework")).Load();  

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

<span data-ttu-id="f3ea9-125">При условии, что у нас есть несколько записей с тегами "Entity-Framework" и "asp.net", выходные данные могут выглядеть примерно так:</span><span class="sxs-lookup"><span data-stu-id="f3ea9-125">Assuming we had a few posts tagged with 'entity-framework' and 'asp.net' the output may look something like this:</span></span>  

```console
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

<span data-ttu-id="f3ea9-126">Это иллюстрирует три точки:</span><span class="sxs-lookup"><span data-stu-id="f3ea9-126">This illustrates three points:</span></span>  

- <span data-ttu-id="f3ea9-127">Новая запись New в EF, добавленная в локальную коллекцию, будет записана контекстом в добавленном состоянии.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-127">The new post 'What's New in EF' that was added to the Local collection becomes tracked by the context in the Added state.</span></span> <span data-ttu-id="f3ea9-128">Поэтому он будет вставлен в базу данных при вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-128">It will therefore be inserted into the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="f3ea9-129">Сообщение, которое было удалено из локальной коллекции (руководством по EF для начинающих), теперь помечено как удаленное в контексте.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-129">The post that was removed from the Local collection (EF Beginners Guide) is now marked as deleted in the context.</span></span> <span data-ttu-id="f3ea9-130">Поэтому он будет удален из базы данных при вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-130">It will therefore be deleted from the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="f3ea9-131">Дополнительная запись (ASP.NET для начинающих), загруженная в контекст со вторым запросом, автоматически добавляется в локальную коллекцию.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-131">The additional post (ASP.NET Beginners Guide) loaded into the context with the second query is automatically added to the Local collection.</span></span>  

<span data-ttu-id="f3ea9-132">Последнее, что нужно отметить, — это то, что производительность ObservableCollection не очень велика для большого количества сущностей.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-132">One final thing to note about Local is that because it is an ObservableCollection performance is not great for large numbers of entities.</span></span> <span data-ttu-id="f3ea9-133">Поэтому, если вы работаете с тысячами сущностей в контексте, не рекомендуется использовать локальную версию.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-133">Therefore if you are dealing with thousands of entities in your context it may not be advisable to use Local.</span></span>  

## <a name="using-local-for-wpf-data-binding"></a><span data-ttu-id="f3ea9-134">Использование Local для привязки данных WPF</span><span class="sxs-lookup"><span data-stu-id="f3ea9-134">Using Local for WPF data binding</span></span>  

<span data-ttu-id="f3ea9-135">Локальное свойство DbSet можно использовать непосредственно для привязки данных в приложении WPF, так как это экземпляр ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-135">The Local property on DbSet can be used directly for data binding in a WPF application because it is an instance of ObservableCollection.</span></span> <span data-ttu-id="f3ea9-136">Как описано в предыдущих разделах, это означает, что оно будет автоматически оставаться синхронизированным с содержимым контекста, и содержимое контекста будет автоматически синхронизировано с ним.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-136">As described in the previous sections this means that it will automatically stay in sync with the contents of the context and the contents of the context will automatically stay in sync with it.</span></span> <span data-ttu-id="f3ea9-137">Обратите внимание, что необходимо предварительно заполнить локальную коллекцию данными для привязки, так как локальный никогда не вызывает запрос к базе данных.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-137">Note that you do need to pre-populate the Local collection with data for there to be anything to bind to since Local never causes a database query.</span></span>  

<span data-ttu-id="f3ea9-138">Это не подходящее место для полного примера привязки данных WPF, но ключевыми элементами являются:</span><span class="sxs-lookup"><span data-stu-id="f3ea9-138">This is not an appropriate place for a full WPF data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="f3ea9-139">Настройка источника привязки</span><span class="sxs-lookup"><span data-stu-id="f3ea9-139">Setup a binding source</span></span>  
- <span data-ttu-id="f3ea9-140">Привяжите его к локальному свойству набора</span><span class="sxs-lookup"><span data-stu-id="f3ea9-140">Bind it to the Local property of your set</span></span>  
- <span data-ttu-id="f3ea9-141">Заполните локальную базу данных с помощью запроса.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-141">Populate Local using a query to the database.</span></span>  

## <a name="wpf-binding-to-navigation-properties"></a><span data-ttu-id="f3ea9-142">Привязка WPF к свойствам навигации</span><span class="sxs-lookup"><span data-stu-id="f3ea9-142">WPF binding to navigation properties</span></span>  

<span data-ttu-id="f3ea9-143">При выполнении привязки данных "основной/подробности" может потребоваться привязать подробное представление к свойству навигации одной из сущностей.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-143">If you are doing master/detail data binding you may want to bind the detail view to a navigation property of one of your entities.</span></span> <span data-ttu-id="f3ea9-144">Простой способ заставить эту работу — использовать ObservableCollection для свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-144">An easy way to make this work is to use an ObservableCollection for the navigation property.</span></span> <span data-ttu-id="f3ea9-145">Пример:</span><span class="sxs-lookup"><span data-stu-id="f3ea9-145">For example:</span></span>  

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

## <a name="using-local-to-clean-up-entities-in-savechanges"></a><span data-ttu-id="f3ea9-146">Использование Local для очистки сущностей в SaveChanges</span><span class="sxs-lookup"><span data-stu-id="f3ea9-146">Using Local to clean up entities in SaveChanges</span></span>  

<span data-ttu-id="f3ea9-147">В большинстве случаев сущности, удаленные из свойства навигации, не будут автоматически помечаться как удаленные в контексте.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-147">In most cases entities removed from a navigation property will not be automatically marked as deleted in the context.</span></span> <span data-ttu-id="f3ea9-148">Например, если удалить объект POST из коллекции блога. posts, то эта публикация не будет автоматически удалена при вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-148">For example, if you remove a Post object from the Blog.Posts collection then that post will not be automatically deleted when SaveChanges is called.</span></span> <span data-ttu-id="f3ea9-149">Если требуется удалить его, может потребоваться найти эти висячие сущности и пометить их как удаленные перед вызовом SaveChanges или в рамках переопределенного метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-149">If you need it to be deleted then you may need to find these dangling entities and mark them as deleted before calling SaveChanges or as part of an overridden SaveChanges.</span></span> <span data-ttu-id="f3ea9-150">Пример:</span><span class="sxs-lookup"><span data-stu-id="f3ea9-150">For example:</span></span>  

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

<span data-ttu-id="f3ea9-151">Приведенный выше код использует локальную коллекцию для поиска всех записей и помечает все, у которых нет ссылки на блог как удаленные.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-151">The code above uses the Local collection to find all posts and marks any that do not have a blog reference as deleted.</span></span> <span data-ttu-id="f3ea9-152">Вызов ToList является обязательным, так как в противном случае коллекция будет изменена вызовом Remove во время перечисления.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-152">The ToList call is required because otherwise the collection will be modified by the Remove call while it is being enumerated.</span></span> <span data-ttu-id="f3ea9-153">В большинстве других ситуаций можно выполнить запрос непосредственно к локальному свойству без использования ToList.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-153">In most other situations you can query directly against the Local property without using ToList first.</span></span>  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a><span data-ttu-id="f3ea9-154">Использование локальных и Тобиндинглист для привязки данных Windows Forms</span><span class="sxs-lookup"><span data-stu-id="f3ea9-154">Using Local and ToBindingList for Windows Forms data binding</span></span>  

<span data-ttu-id="f3ea9-155">Windows Forms не поддерживает полную точность привязки данных с помощью ObservableCollection напрямую.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-155">Windows Forms does not support full fidelity data binding using ObservableCollection directly.</span></span> <span data-ttu-id="f3ea9-156">Однако вы по-прежнему можете использовать локальное свойство DbSet для привязки данных, чтобы получить все преимущества, описанные в предыдущих разделах.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-156">However, you can still use the DbSet Local property for data binding to get all the benefits described in the previous sections.</span></span> <span data-ttu-id="f3ea9-157">Это достигается с помощью метода расширения Тобиндинглист, который создает реализацию [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) , которая поддерживается локальным ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-157">This is achieved through the ToBindingList extension method which creates an [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementation backed by the Local ObservableCollection.</span></span>  

<span data-ttu-id="f3ea9-158">Это не подходящее место для полного Windows Formsного примера привязки данных, но ключевыми элементами являются:</span><span class="sxs-lookup"><span data-stu-id="f3ea9-158">This is not an appropriate place for a full Windows Forms data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="f3ea9-159">Настройка источника привязки объектов</span><span class="sxs-lookup"><span data-stu-id="f3ea9-159">Setup an object binding source</span></span>  
- <span data-ttu-id="f3ea9-160">Привяжите его к локальному свойству набора с помощью Local. Тобиндинглист ()</span><span class="sxs-lookup"><span data-stu-id="f3ea9-160">Bind it to the Local property of your set using Local.ToBindingList()</span></span>  
- <span data-ttu-id="f3ea9-161">Заполнение локальной базы данных с помощью запроса</span><span class="sxs-lookup"><span data-stu-id="f3ea9-161">Populate Local using a query to the database</span></span>  

## <a name="getting-detailed-information-about-tracked-entities"></a><span data-ttu-id="f3ea9-162">Получение подробных сведений о отслеживании сущностей</span><span class="sxs-lookup"><span data-stu-id="f3ea9-162">Getting detailed information about tracked entities</span></span>  

<span data-ttu-id="f3ea9-163">Во многих примерах в этой серии используется метод записи для возврата экземпляра Дбентитентри для сущности.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-163">Many of the examples in this series use the Entry method to return a DbEntityEntry instance for an entity.</span></span> <span data-ttu-id="f3ea9-164">Этот объект записи затем выступает в качестве отправной точки для сбора сведений о сущности, например ее текущего состояния, а также для выполнения операций с сущностью, например для явной загрузки связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-164">This entry object then acts as the starting point for gathering information about the entity such as its current state, as well as for performing operations on the entity such as explicitly loading a related entity.</span></span>  

<span data-ttu-id="f3ea9-165">Методы записи возвращают объекты Дбентитентри для многих или всех сущностей, отслеживаемых контекстом.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-165">The Entries methods return DbEntityEntry objects for many or all entities being tracked by the context.</span></span> <span data-ttu-id="f3ea9-166">Это позволяет собирать информацию или выполнять операции с множеством сущностей, а не только с одной записью.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-166">This allows you to gather information or perform operations on many entities rather than just a single entry.</span></span> <span data-ttu-id="f3ea9-167">Пример:</span><span class="sxs-lookup"><span data-stu-id="f3ea9-167">For example:</span></span>  

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

<span data-ttu-id="f3ea9-168">Обратите внимание, что мы представляем класс Author и Reader в примере. оба этих класса реализуют интерфейс Иперсон.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-168">You'll notice we are introducing a Author and Reader class into the example - both of these classes implement the IPerson interface.</span></span>  

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

<span data-ttu-id="f3ea9-169">Предположим, что в базе данных есть следующие данные:</span><span class="sxs-lookup"><span data-stu-id="f3ea9-169">Let's assume we have the following data in the database:</span></span>

<span data-ttu-id="f3ea9-170">Блог с Блогид = 1 и именем = "блог ADO.NET"</span><span class="sxs-lookup"><span data-stu-id="f3ea9-170">Blog with BlogId = 1 and Name = 'ADO.NET Blog'</span></span>  
<span data-ttu-id="f3ea9-171">Блог с Блогид = 2 и именем = ' блог Visual Studio '</span><span class="sxs-lookup"><span data-stu-id="f3ea9-171">Blog with BlogId = 2 and Name = 'The Visual Studio Blog'</span></span>  
<span data-ttu-id="f3ea9-172">Блог с Блогид = 3 и именем = ' .NET Framework блоге '</span><span class="sxs-lookup"><span data-stu-id="f3ea9-172">Blog with BlogId = 3 and Name = '.NET Framework Blog'</span></span>  
<span data-ttu-id="f3ea9-173">Автор с Аусорид = 1 и название = ' Джо блоггеры '</span><span class="sxs-lookup"><span data-stu-id="f3ea9-173">Author with AuthorId = 1 and Name = 'Joe Bloggs'</span></span>  
<span data-ttu-id="f3ea9-174">Читатель с Реадерид = 1 и именем = ' Джон Петров '</span><span class="sxs-lookup"><span data-stu-id="f3ea9-174">Reader with ReaderId = 1 and Name = 'John Doe'</span></span>  

<span data-ttu-id="f3ea9-175">Результат выполнения кода будет следующим:</span><span class="sxs-lookup"><span data-stu-id="f3ea9-175">The output from running the code would be:</span></span>  

```console
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

<span data-ttu-id="f3ea9-176">В этих примерах показано несколько точек:</span><span class="sxs-lookup"><span data-stu-id="f3ea9-176">These examples illustrate several points:</span></span>  

- <span data-ttu-id="f3ea9-177">Методы записи возвращают записи для сущностей во всех состояниях, включая Deleted.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-177">The Entries methods return entries for entities in all states, including Deleted.</span></span> <span data-ttu-id="f3ea9-178">Сравните это с локальным, исключая удаленные сущности.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-178">Compare this to Local which excludes Deleted entities.</span></span>  
- <span data-ttu-id="f3ea9-179">Записи для всех типов сущностей возвращаются при использовании метода неуниверсальных записей.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-179">Entries for all entity types are returned when the non-generic Entries method is used.</span></span> <span data-ttu-id="f3ea9-180">Если метод универсальных записей используется для сущностей, являющихся экземплярами универсального типа, возвращаются только записи.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-180">When the generic entries method is used entries are only returned for entities that are instances of the generic type.</span></span> <span data-ttu-id="f3ea9-181">Это было использовано для получения записей обо всех блогах.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-181">This was used above to get entries for all blogs.</span></span> <span data-ttu-id="f3ea9-182">Он также использовался для получения записей для всех сущностей, реализующих Иперсон.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-182">It was also used to get entries for all entities that implement IPerson.</span></span> <span data-ttu-id="f3ea9-183">Это показывает, что универсальный тип не обязательно должен быть фактическим типом сущности.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-183">This demonstrates that the generic type does not have to be an actual entity type.</span></span>  
- <span data-ttu-id="f3ea9-184">LINQ to Objects можно использовать для фильтрации возвращаемых результатов.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-184">LINQ to Objects can be used to filter the results returned.</span></span> <span data-ttu-id="f3ea9-185">Это было использовано для поиска сущностей любого типа при условии, что они были изменены.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-185">This was used above to find entities of any type as long as they are modified.</span></span>  

<span data-ttu-id="f3ea9-186">Обратите внимание, что экземпляры Дбентитентри всегда содержат сущность, отличную от NULL.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-186">Note that DbEntityEntry instances always contain a non-null Entity.</span></span> <span data-ttu-id="f3ea9-187">Записи связей и заглушки не представлены как экземпляры Дбентитентри, поэтому их не нужно фильтровать.</span><span class="sxs-lookup"><span data-stu-id="f3ea9-187">Relationship entries and stub entries are not represented as DbEntityEntry instances so there is no need to filter for these.</span></span>
