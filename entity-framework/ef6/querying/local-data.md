---
title: Локальные данные - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
caps.latest.revision: 3
ms.openlocfilehash: 79f0d2175199780d41b43088832bab808ab2fff0
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122302"
---
# <a name="local-data"></a>Локальные данные
Выполняется запрос LINQ непосредственно к DbSet, всегда отправляет запрос к базе данных, но можно получить доступ к данных, который в данный момент в памяти с помощью свойства DbSet.Local. Можно также открыть нужные дополнительные данные отслеживания EF о сущности с помощью методов DbContext.Entry и DbContext.ChangeTracker.Entries. Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.  

## <a name="using-local-to-look-at-local-data"></a>Использовать локальную для просмотра локальных данных  

Локальное свойство DbSet предоставляет простой доступ к сущностей из набора, которые отслеживаются контекстом и не были помечены как удаленные. Доступ к локальному свойству никогда не вызывает запрос для отправки в базу данных. Это означает, что оно обычно используется после запроса уже была выполнена. Метода расширения Load может использоваться для выполнения запроса, чтобы контекст отслеживает результаты. Пример:  

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

Если у нас два блоги в базе данных - «ADO.NET блог» с BlogId 1 - и «Блоге по Visual Studio» с BlogId 2 мы могла бы ожидать следующие выходные данные:  

```  
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

Это иллюстрирует три точки:  

- Новый блог «Мой новый блог» входит в локальную коллекцию, несмотря на то, что он еще не были сохранены в базе данных. Этот блог имеет первичный ключ нулевой, так как в базе данных еще не создал фактическую клавишу для сущности.  
- В «записи ADO.NET» отсутствует в локальной коллекции, несмотря на то, что он по-прежнему отслеживается контекстом. Это так, как мы удалили из DbSet, тем самым пометив его как удаленные.  
- Когда DbSet, используется для выполнения запроса блога, помечен для удаления (блог ADO.NET) включается в результаты и нового блога (Мой новый блог), который еще не были сохранены в базе данных не включается в результаты. Это обусловлено DbSet выполняет запрос к базе данных и результатов, возвращаемых всегда оно совпадало с указанным в базе данных.  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a>Использовать локальную для добавления и удаления сущностей из контекста  

Возвращает свойства локально DbSet [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) с событиями, привязываем ее таким образом, что он остается синхронизированной с содержимое контекста. Это означает, что сущности можно добавлять или удалять из локальной коллекции или DbSet. Это также означает, что запросы, которые привносят новые сущности в контекст, приведет к локальной коллекции, обновляемой с этими сущностями. Пример:  

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

При условии, что у нас было несколько помеченные с «entity framework» и «asp.net» выходные данные записи может выглядеть следующим образом:  

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

Это иллюстрирует три точки:  

- «Новые возможности в EF» новую запись, добавленный к локальной коллекции становится отслеживаемая в контексте в добавленном состоянии. Она будет таким образом вставлена в базу данных при вызове метода SaveChanges.  
- Запрос post, который был удален из локальной коллекции (руководство по профилированию EF) теперь помечены как удаленные в контексте. Его таким образом удаляются из базы данных при вызове метода SaveChanges.  
- Дополнительные post (руководство по профилированию ASP.NET), загруженной в контекст, с помощью второй запрос автоматически добавляется к локальной коллекции.  

Заключительную следует помнить о локальной обусловлено тем, что ObservableCollection производительности не очень удобно для большого количества сущностей. Поэтому если вы имеете дело с тысячами сущностей в контексте может оказаться рекомендуется использовать локальный.  

## <a name="using-local-for-wpf-data-binding"></a>Использовать локальную для привязки данных WPF  

Свойства DbSet локально можно использовать непосредственно для привязки данных в приложении WPF, так как он является экземпляром ObservableCollection. Как описано в предыдущих разделах, это означает, что он будет автоматически синхронизироваться с содержимое контекста и содержимое контекста будут автоматически синхронизированы с ним. Обратите внимание на то, что вам нужно предварительно заполнить локальную коллекцию с данными не исключена действий, которые необходимо привязать к, так как локальный никогда не вызывает запрос к базе данных.  

Это не является подходящим местом для полный пример привязки данных WPF, но являются ключевые элементы:  

- Настройка источника привязки  
- Привяжите его к локальному свойству набора параметров  
- Заполнение локальной, с помощью запроса к базе данных.  

## <a name="wpf-binding-to-navigation-properties"></a>Привязки WPF для свойств навигации  

Если вы выполняете данных «основной/подробности», то может потребоваться привязать к свойству навигации, одной из сущностей представлении «Подробности». Простой способ выполнения этой работы является использование коллекции ObservableCollection для свойства навигации. Пример:  

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

## <a name="using-local-to-clean-up-entities-in-savechanges"></a>Использовать локальную для очистки сущностей в SaveChanges  

В большинстве случаев сущности удалены из свойства навигации не помечается, автоматически как удаленный в контексте. Например при удалении объекта Post из коллекции Blog.Posts, затем выполните, не удаляются автоматически при вызове метода SaveChanges. Если это необходимо для удаления затем может потребоваться найти эти несвязанные сущности и пометить их как удаленные, перед вызовом метода SaveChanges, или как часть переопределенного метода SaveChanges. Пример:  

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

Приведенный выше код использует локальную коллекцию, чтобы найти все сообщения и метки, которые не имеют ссылку на блог как удаленные. Вызов ToList является обязательным, поскольку в противном случае коллекции будет изменена Remove вызвать во время его перечисления. В большинстве случаев вы можете запрашивать непосредственно с локального свойства без использования ToList сначала.  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a>Использование привязки данных для локальных и ToBindingList for Windows Forms  

Windows Forms не поддерживает привязку данных полноценной непосредственно с помощью ObservableCollection. Тем не менее чтобы получить все преимущества, описанные в предыдущих разделах по-прежнему можно использовать DbSet локального свойства для привязки данных. Это достигается с помощью метода расширения ToBindingList, который создает [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) реализации поддерживаемый локальной коллекции ObservableCollection.  

Это не является подходящим местом для полный пример привязки данных Windows Forms, но являются ключевые элементы:  

- Настройка объекта источника привязки  
- Привяжите его к локальному свойству с помощью Local.ToBindingList() набора  
- Заполнение локальной, с помощью запроса к базе данных  

## <a name="getting-detailed-information-about-tracked-entities"></a>Получение подробных сведений о отслеживаемые сущности  

Во многих примерах в этой серии запись метод позволяет вернуть экземпляр dbentityentry, который должен для сущности. Затем этот объект записи выступает в качестве отправной точки для сбора сведений о сущности, например текущего состояния, а также для выполнения операций с сущностями, например явная загрузка связанных сущностей.  

Методы операции возвращают объекты dbentityentry, который должен для нескольких или всех сущностей, отслеживается контекстом. Это позволяет собирать информацию или выполнить операции на множество сущностей, а не только одна запись. Пример:  

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

Можно заметить, мы представляем класс автора и чтения в пример — оба эти класса реализуют интерфейс IPerson.  

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

Предположим, что у нас есть следующие данные в базе данных:

Блог с BlogId = 1 "и" имя = «Блог ADO.NET»  
Блог с BlogId = 2, а также имя = «Блог Visual Studio»  
Блог с BlogId = 3 "и" имя = «Блог по .NET Framework»  
Разработка с помощью AuthorId = 1 "и" имя = «Joe Bloggs»  
Средства чтения с ReaderId = 1 "и" имя = «John Doe»  

В результате выполнения кода будет следующим:  

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

В следующих примерах показаны несколько точек:  

- Методы записи, возвращают записи для сущности во всех состояниях, включая удаленные. Сравните это для локального параметра, который исключает удаление сущностей.  
- При использовании метода нестандартную записей, возвращаются записи для всех типов сущностей. При использовании метода универсального записи только возвращаются записи для сущностей, которые являются экземплярами универсального типа. Использовался выше для получения записи для всех блогах. Он также использовался для получения записи для всех сущностей, которые реализуют IPerson. Это показывает, что универсальный тип имеет тип фактического объекта.  
- LINQ к объектам можно использовать для фильтрации возвращаемых результатов. Использовался выше для поиска объектов любого типа, до тех пор, пока они будут изменены.  

Обратите внимание, что экземпляры dbentityentry, который должен всегда содержат сущности отличное от null. Записей отношений и записи заглушки не представлены в виде экземпляров dbentityentry, который должен, нет необходимости для фильтрации для них.
