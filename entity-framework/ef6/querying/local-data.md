---
title: Локальные данные — EF6
description: Локальные данные в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/querying/local-data
ms.openlocfilehash: cff3612072ad9caa057d8589d47cc75ae32be8db
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92065879"
---
# <a name="local-data"></a>Локальные данные
При выполнении запроса LINQ непосредственно к DbSet будет всегда отправляться запрос к базе данных, но можно получить доступ к данным, находящихся в памяти, с помощью свойства DbSet. local. Кроме того, можно получить доступ к дополнительной информации EF, которая отслеживает сущности с помощью методов DbContext. Entry и DbContext. ChangeTracker .s. Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.  

## <a name="using-local-to-look-at-local-data"></a>Использование Local для просмотра локальных данных  

Свойство Local объекта DbSet предоставляет простой доступ к сущностям набора, которые в настоящее время отслеживается контекстом и не помечены как удаленные. Обращение к локальному свойству никогда не приводит к отправке запроса в базу данных. Это означает, что он обычно используется после того, как запрос уже выполнен. Метод расширения Load можно использовать для выполнения запроса, чтобы контекст мог отслеживать результаты. Например.  

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

Если у нас есть два блога в базе данных — "блог ADO.NET" с Блогид 1 и "блог Visual Studio" с Блогид 2-мы можем получить следующие выходные данные:  

```console
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

Это иллюстрирует три точки:  

- Новый блог "мой новый блог" включен в локальную коллекцию, несмотря на то, что он еще не сохранен в базе данных. Этот блог имеет первичный ключ, равный нулю, так как база данных еще не создала реальный ключ для сущности.  
- Блог ADO.NET не включен в локальную коллекцию, несмотря на то, что он по-прежнему отслеживается контекстом. Это вызвано тем, что мы удалили его из DbSet, пометив его как удаленный.  
- Когда DbSet используется для выполнения запроса, помеченный для удаления (блог ADO.NET), включается в результаты и новый блог (мой новый блог), который еще не был сохранен в базе данных, не включается в результаты. Это обусловлено тем, что DbSet выполняет запрос к базе данных, и возвращаемые результаты всегда отражать то, что находится в базе данных.  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a>Использование Local для добавления и удаления сущностей из контекста  

Локальное свойство в DbSet возвращает [коллекцию ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) с событиями, которые были подключены таким же, что они остаются синхронизированными с содержимым контекста. Это означает, что сущности можно добавлять или удалять либо из локальной коллекции, либо из DbSet. Это также означает, что запросы, которые переносят новые сущности в контекст, приведут к обновлению локальной коллекции этими сущностями. Например.  

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

При условии, что у нас есть несколько записей с тегами "Entity-Framework" и "asp.net", выходные данные могут выглядеть примерно так:  

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

Это иллюстрирует три точки:  

- Новая запись New в EF, добавленная в локальную коллекцию, будет записана контекстом в добавленном состоянии. Поэтому он будет вставлен в базу данных при вызове метода SaveChanges.  
- Сообщение, которое было удалено из локальной коллекции (руководством по EF для начинающих), теперь помечено как удаленное в контексте. Поэтому он будет удален из базы данных при вызове метода SaveChanges.  
- Дополнительная запись (ASP.NET для начинающих), загруженная в контекст со вторым запросом, автоматически добавляется в локальную коллекцию.  

Последнее, что нужно отметить, — это то, что производительность ObservableCollection не очень велика для большого количества сущностей. Поэтому, если вы работаете с тысячами сущностей в контексте, не рекомендуется использовать локальную версию.  

## <a name="using-local-for-wpf-data-binding"></a>Использование Local для привязки данных WPF  

Локальное свойство DbSet можно использовать непосредственно для привязки данных в приложении WPF, так как это экземпляр ObservableCollection. Как описано в предыдущих разделах, это означает, что оно будет автоматически оставаться синхронизированным с содержимым контекста, и содержимое контекста будет автоматически синхронизировано с ним. Обратите внимание, что необходимо предварительно заполнить локальную коллекцию данными для привязки, так как локальный никогда не вызывает запрос к базе данных.  

Это не подходящее место для полного примера привязки данных WPF, но ключевыми элементами являются:  

- Настройка источника привязки  
- Привяжите его к локальному свойству набора  
- Заполните локальную базу данных с помощью запроса.  

## <a name="wpf-binding-to-navigation-properties"></a>Привязка WPF к свойствам навигации  

При выполнении привязки данных "основной/подробности" может потребоваться привязать подробное представление к свойству навигации одной из сущностей. Простой способ заставить эту работу — использовать ObservableCollection для свойства навигации. Например.  

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

## <a name="using-local-to-clean-up-entities-in-savechanges"></a>Использование Local для очистки сущностей в SaveChanges  

В большинстве случаев сущности, удаленные из свойства навигации, не будут автоматически помечаться как удаленные в контексте. Например, если удалить объект POST из коллекции блога. posts, то эта публикация не будет автоматически удалена при вызове метода SaveChanges. Если требуется удалить его, может потребоваться найти эти висячие сущности и пометить их как удаленные перед вызовом SaveChanges или в рамках переопределенного метода SaveChanges. Например.  

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

Приведенный выше код использует локальную коллекцию для поиска всех записей и помечает все, у которых нет ссылки на блог как удаленные. Вызов ToList является обязательным, так как в противном случае коллекция будет изменена вызовом Remove во время перечисления. В большинстве других ситуаций можно выполнить запрос непосредственно к локальному свойству без использования ToList.  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a>Использование локальных и Тобиндинглист для привязки данных Windows Forms  

Windows Forms не поддерживает полную точность привязки данных с помощью ObservableCollection напрямую. Однако вы по-прежнему можете использовать локальное свойство DbSet для привязки данных, чтобы получить все преимущества, описанные в предыдущих разделах. Это достигается с помощью метода расширения Тобиндинглист, который создает реализацию [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) , которая поддерживается локальным ObservableCollection.  

Это не подходящее место для полного Windows Formsного примера привязки данных, но ключевыми элементами являются:  

- Настройка источника привязки объектов  
- Привяжите его к локальному свойству набора с помощью Local. Тобиндинглист ()  
- Заполнение локальной базы данных с помощью запроса  

## <a name="getting-detailed-information-about-tracked-entities"></a>Получение подробных сведений о отслеживании сущностей  

Во многих примерах в этой серии используется метод записи для возврата экземпляра Дбентитентри для сущности. Этот объект записи затем выступает в качестве отправной точки для сбора сведений о сущности, например ее текущего состояния, а также для выполнения операций с сущностью, например для явной загрузки связанной сущности.  

Методы записи возвращают объекты Дбентитентри для многих или всех сущностей, отслеживаемых контекстом. Это позволяет собирать информацию или выполнять операции с множеством сущностей, а не только с одной записью. Например.  

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

Обратите внимание, что мы представляем класс Author и Reader в примере. оба этих класса реализуют интерфейс Иперсон.  

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

Предположим, что в базе данных есть следующие данные:

Блог с Блогид = 1 и именем = "блог ADO.NET"  
Блог с Блогид = 2 и именем = ' блог Visual Studio '  
Блог с Блогид = 3 и именем = ' .NET Framework блоге '  
Автор с Аусорид = 1 и название = ' Джо блоггеры '  
Читатель с Реадерид = 1 и именем = ' Джон Петров '  

Результат выполнения кода будет следующим:  

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

В этих примерах показано несколько точек:  

- Методы записи возвращают записи для сущностей во всех состояниях, включая Deleted. Сравните это с локальным, исключая удаленные сущности.  
- Записи для всех типов сущностей возвращаются при использовании метода неуниверсальных записей. Если метод универсальных записей используется для сущностей, являющихся экземплярами универсального типа, возвращаются только записи. Это было использовано для получения записей обо всех блогах. Он также использовался для получения записей для всех сущностей, реализующих Иперсон. Это показывает, что универсальный тип не обязательно должен быть фактическим типом сущности.  
- LINQ to Objects можно использовать для фильтрации возвращаемых результатов. Это было использовано для поиска сущностей любого типа при условии, что они были изменены.  

Обратите внимание, что экземпляры Дбентитентри всегда содержат сущность, отличную от NULL. Записи связей и заглушки не представлены как экземпляры Дбентитентри, поэтому их не нужно фильтровать.
