---
title: Типы сущностей с помощью конструкторов - EF Core
author: ajcvickers
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
ms.technology: entity-framework-core
uid: core/modeling/constructors
ms.openlocfilehash: 3f861d54c5bff637ae28f38b08da7aff7d0ea5c0
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2018
---
# <a name="entity-types-with-constructors"></a>Типы сущностей с помощью конструкторов

> [!NOTE]  
> Этот компонент впервые появился в EF Core 2.1.

Начиная с EF Core 2.1, возможна теперь определять конструктор с параметрами и Core EF вызвать этот конструктор при создании экземпляра сущности. Параметры конструктора могут быть привязаны к сопоставленного свойства или для различных типов служб для упрощения поведения, такие как отложенную загрузку.

> [!NOTE]  
> Начиная с версии 2.1 Core EF все привязки конструктор является соглашением. Конфигурация из определенных конструкторов для использования планируется в будущих выпусках.

## <a name="binding-to-mapped-properties"></a>Привязки для сопоставленного свойства

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

При EF Core создает экземпляры этих типов, таких как для результатов запроса, будет сначала вызывается конструктор по умолчанию без параметров и задайте для каждого свойства значение из базы данных. Однако если EF Core выполняет поиск конструктора с параметрами с имена параметров и типы, которые совпадают с сопоставлены свойства, то он вместо этого будет вызывать параметризованный конструктор со значениями этих свойств и не будет устанавливать каждое свойство явно. Пример:

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
Необходимо отметить следующее:
* Не все свойства должны иметь параметров конструктора. Например свойство Post.Content не задается любого параметра конструктора, поэтому EF Core будет задано после вызова конструктора обычным способом.
* Типы параметров и имена должны совпадать имена и типы свойств, за исключением того, что свойства могут быть стиле Pascal пока параметры находятся в стиле Camel.
* Основные EF не удалось установить свойства навигации (например, в блоге или выше в блогах) с помощью конструктора.
* Конструктор может быть открытым, закрытым, или иметь любой уровень доступа.

### <a name="read-only-properties"></a>Свойства только для чтения

После установки свойств через конструктор имеет смысл сделать некоторые из них только для чтения. Это поддерживает EF Core, но существуют некоторые особенности нужно обратить внимание:
* Свойства без задания не сопоставляются с соглашением. (Таким образом, как правило, для сопоставления свойства, которые не должны сопоставляться, например вычисляемого свойства.)
* С помощью автоматически создаваемых значений ключа требуется ключевое свойство, чтения и записи, поскольку значение ключа должно быть задано генератором ключа, при вставке новых сущностей.

Для использования закрытого задания — простой способ избежать следующие действия. Пример:
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
EF Core видит свойства с помощью закрытого setter чтения и записи, что означает, что все свойства сопоставляются как и раньше, и ключ можно по-прежнему создаваться хранилища.

Вместо использования закрытого задания является сделать свойства действительно только для чтения и Добавление более явное сопоставление в OnModelCreating. Аналогично некоторые свойства можно полностью удалены и заменены только поля. Например рассмотрим этих типов сущностей.

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
И эту настройку в OnModelCreating:
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
Необходимо отметить следующее:
* Теперь поле является ключ «свойство». Это не `readonly` таким образом, чтобы сформированные хранилищем ключи можно использовать. 
* Другие свойства являются свойства только для чтения только в конструкторе.
* Если значение первичного ключа является только задается EF или прочитать из базы данных, нет необходимости включать его в конструкторе. Это оставляет ключ «свойство» как простой поле и указывает на то, что его не следует задавать явным образом при создании новых блогах или сообщения.

> [!NOTE]  
> Этот код вызовет предупреждение "169", указывающее, что поле никогда не используется. Это сообщение можно проигнорировать, так как на самом деле EF Core, используя extralinguistic образом поле.

## <a name="injecting-services"></a>Добавление службы

EF Core также можно ввести «службы» в конструктор типа сущности. Например ниже могут быть добавлены:
* `DbContext` -текущего экземпляра контекста, также могут быть типизированы как вашего производного типа DbContext
* `ILazyLoader` -службы отложенная загрузка — в разделе [отложенная загрузка документации](../querying/related-data.md) Дополнительные сведения
* `Action<object, string>` -Делегат отложенная загрузка — в разделе [отложенная загрузка документации](../querying/related-data.md) Дополнительные сведения
* `IEntityType` -EF Core метаданные, связанные с этого типа сущностей

> [!NOTE]  
> Начиная с версии 2.1 Core EF могут быть добавлены только службы, известен EF ядром. Поддержка добавления служб приложений считается в будущих выпусках.

Например введенный DbContext можно выборочно доступа к базе данных для получения сведений о связанных сущностей без загрузки их все. В следующем примере это используется для получения количество записей в записи блога без загрузки сообщения:

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
* Конструктор закрыт, так как он вызывается только основных компонентов EF и другой конструктор, открытый для общего использования.
* Код, использующий службу подставляемого (т. е. контекст) защитного по нему, `null` для обработки случаев, где EF Core не создает экземпляр.
* Поскольку службы хранится в свойство чтения/записи, оно будет сброшено при присоединении к новому экземпляру контекста сущности.

> [!WARNING]  
> Вводится DbContext следующим образом часто считается антишаблон, так как оно прикрепляет к типам сущностей непосредственно к EF Core. Внимательно рассмотрите все параметры, прежде чем с помощью внесения службы следующим образом.
