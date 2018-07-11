---
title: Типы сущностей с помощью конструкторов — EF Core
author: ajcvickers
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
ms.technology: entity-framework-core
uid: core/modeling/constructors
ms.openlocfilehash: 80c7ee04d3bb0dd45b66ec7d6fec5ee7e3343026
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949209"
---
# <a name="entity-types-with-constructors"></a>Типы сущностей с помощью конструкторов

> [!NOTE]  
> Это новая возможность в EF Core 2.1.

Начиная с EF Core 2.1, стало возможным определять конструктор с параметрами и посредством вызова этого конструктора, при создании экземпляра сущности EF Core. Параметры конструктора могут быть привязаны к сопоставленным свойствам или на различные виды услуг, которые помогают такое поведение, как отложенной загрузки.

> [!NOTE]  
> В версии EF Core 2.1 все привязки конструктора является по соглашению. Конфигурация из определенных конструкторов для использования планируется в будущих выпусках.

## <a name="binding-to-mapped-properties"></a>Привязка к сопоставленным свойствам

Рассмотрим типичный модель сообщение в блоге:

```Csharp
public class Blog
{
    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```

Когда EF Core создает экземпляры этих типов, например для результатов запроса, он сначала вызвать конструктор без параметров по умолчанию и затем в базе данных присвоено значение каждого свойства. Тем не менее если EF Core находит параметризованный конструктор с имена параметров и типы, которые совпадают с сопоставлены свойства, то он вместо этого будет вызывать параметризованный конструктор со значениями этих свойств и не будет устанавливать каждое свойство явным образом. Пример:

```Csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```
Некоторые другие примечания:
* Не все свойства должны иметь параметры конструктора. Например свойство Post.Content не задано каким-либо конструктор параметром, поэтому EF Core установит его после вызова конструктора обычным способом.
* Типы параметров и имена должны совпадать имена и типы свойств, за исключением того, что свойства могут быть стиле Pascal хотя параметры являются в стиле Camel.
* EF Core не удалось установить свойства навигации (например, блогов или сообщений выше) с помощью конструктора.
* Конструктор может быть открытым, закрытым, или иметь любой уровень доступа.

### <a name="read-only-properties"></a>Свойства только для чтения

После установки свойств с помощью конструктора имеет смысл сделать некоторые из них только для чтения. EF Core поддерживает это, но есть несколько моментов, нужно обратить внимание:
* Свойства без методов задания не сопоставлены по соглашению. (Таким образом, как правило, для сопоставления свойства, которые не должны сопоставляться, например вычисляемых свойств.)
* С помощью автоматического формирования значений ключей требуется ключевое свойство, чтение и запись, так как значение ключа необходимо задать генератором ключа при вставке новых сущностей.

Для использования закрытых методов задания — простой способ избежать такие вещи. Пример:
```Csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; private set; }

    public string Name { get; private set; }
    public string Author { get; private set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; private set; }

    public string Title { get; private set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; private set; }

    public Blog Blog { get; set; }
}
```
EF Core воспринимает свойство с закрытый метод задания как чтения и записи, что означает, что все свойства сопоставляются как и раньше, и ключ можно по-прежнему создаваться хранилища.

Альтернативой использованию закрытых методов задания является сделать свойства действительно только для чтения и Добавление более явное сопоставление в OnModelCreating. Аналогично некоторые свойства можно полностью удалить и заменить только поля. Например рассмотрим эти типы сущностей:

```Csharp
public class Blog
{
    private int _id;

    public Blog(string name, string author)
    {
        Name = name;
        Author = author;
    }

    public string Name { get; }
    public string Author { get; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    private int _id;

    public Post(string title, DateTime postedOn)
    {
        Title = title;
        PostedOn = postedOn;
    }

    public string Title { get; }
    public string Content { get; set; }
    public DateTime PostedOn { get; }

    public Blog Blog { get; set; }
}
```
И эта конфигурация в OnModelCreating:
```Csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Author);
            b.Property(e => e.Name);
        });

    modelBuilder.Entity<Post>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Title);
            b.Property(e => e.PostedOn);
        });
}
```
Сведения:
* Теперь поле является ключ «свойство». Это не `readonly` таким образом, чтобы сформированные хранилищем ключи могут использоваться.
* Другие свойства являются только для чтения свойства, заданные только в конструкторе.
* Если значение первичного ключа является только заданные EF или прочитать из базы данных, нет необходимости включать его в конструкторе. Это оставляет ключ «свойство» как простое поле и позволяет понять, что он не должен задаваться явным образом при создании нового блогов или сообщений.

> [!NOTE]  
> Этот код приведет к компилятор выдает предупреждение "169", указывающее, что поле никогда не используется. Это сообщение можно проигнорировать, так как на самом деле EF Core использует поле extralinguistic образом.

## <a name="injecting-services"></a>Добавление служб

EF Core также можно внедрить «службы» в конструктор типа сущности. Например ниже можно ввести:
* `DbContext` -текущий экземпляр контекста, можно также ввести производный тип DbContext
* `ILazyLoader` -отложенная загрузка службы — см. в разделе [отложенной загрузки документации](../querying/related-data.md) подробности
* `Action<object, string>` -Делегат отложенной загрузки — см. в разделе [отложенной загрузки документации](../querying/related-data.md) подробности
* `IEntityType` — EF Core метаданные, связанные с этим типом сущности

> [!NOTE]  
> Начиная с EF Core 2.1 можно внедрить только те службы, которые известны EF Core. Поддержка добавления службы приложений будет рассмотрен в будущих выпусках.

Например можно использовать внедренного DbContext выборочный доступ к базе данных для получения сведений о связанных сущностей, не загружая их все. В следующем примере это используется для получения числа записей в блог без загрузки в записях:

```Csharp
public class Blog
{
    public Blog()
    {
    }

    private Blog(BloggingContext context)
    {
        Context = context;
    }

    private BloggingContext Context { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; set; }

    public int PostsCount
        => Posts?.Count
           ?? Context?.Set<Post>().Count(p => Id == EF.Property<int?>(p, "BlogId"))
           ?? 0;
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```
Несколько замечания об этом.
* Конструктор является закрытым, так как только вызывается с помощью EF Core, а другой открытый конструктор для общего использования.
* Код, с помощью внедренного службы (то есть контекст) имеет защищенный характер: на ее основе, `null` для обработки случаев, где EF Core не создает экземпляр.
* Так как службы хранится в свойстве чтения и записи будет изменено при присоединении сущности в новый экземпляр контекста.

> [!WARNING]  
> Внедрение DbContext, как это часто считается антишаблоном так, как он связывает типах сущностей непосредственно в EF Core. Внимательно рассмотрите все параметры, прежде чем с помощью внесения службы следующим образом.
