---
title: Типы сущностей с конструкторами — EF Core
description: Использование конструкторов для привязки данных с помощью модели Entity Framework Core
author: ajcvickers
ms.date: 02/23/2018
uid: core/modeling/constructors
ms.openlocfilehash: 9502d75072eebb80c37cf1805e21f7d112269ba1
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92063716"
---
# <a name="entity-types-with-constructors"></a>Типы сущностей с конструкторами

Можно определить конструктор с параметрами и EF Core вызвать этот конструктор при создании экземпляра сущности. Параметры конструктора можно привязать к сопоставленным свойствам или к различным типам служб, чтобы упростить такие действия, как отложенная загрузка.

> [!NOTE]
> В настоящее время все привязки конструктора имеют соглашение. Настройка конкретных конструкторов для использования планируется в будущем выпуске.

## <a name="binding-to-mapped-properties"></a>Привязка к сопоставленным свойствам

Рассмотрим типичную модель блога или публикации:

```csharp
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

Когда EF Core создает экземпляры этих типов, например, для результатов запроса, сначала вызывается конструктор без параметров по умолчанию, а затем каждому свойству присваивается значение из базы данных. Однако если EF Core находит параметризованный конструктор с именами и типами параметров, совпадающими с сопоставленными свойствами, то вместо этого он вызывает параметризованный конструктор со значениями для этих свойств и не задаст каждое свойство явным образом. Например.

```csharp
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

Обратите внимание на следующие моменты.

* Не все свойства должны иметь параметры конструктора. Например, свойство POST. Content не задается ни одним из параметров конструктора, поэтому EF Core задаст его после вызова конструктора обычным способом.
* Типы и имена параметров должны совпадать с типами и именами свойств, за исключением того, что свойства могут быть в стиле Pascal, а параметры — в стиле Camel.
* EF Core не может задать свойства навигации (например, блог или записи выше) с помощью конструктора.
* Конструктор может быть общедоступным, закрытым или иметь другие специальные возможности. Однако для прокси-серверов с отложенной загрузкой требуется, чтобы конструктор был доступен из наследуемого прокси-класса. Обычно это означает, что он является общедоступным или защищенным.

### <a name="read-only-properties"></a>Свойства только для чтения

Когда свойства задаются с помощью конструктора, может быть целесообразно сделать некоторые из них только для чтения. EF Core поддерживает это, но необходимо обратить внимание на следующие моменты:

* Свойства без методов задания не сопоставляются по соглашению. (Это предполагает сопоставление свойств, которые не должны сопоставляться, например, вычисленные свойства.)
* Для использования автоматически создаваемых значений ключа требуется ключевое свойство, которое доступно для чтения и записи, поскольку при вставке новых сущностей значение ключа должно быть задано генератором ключей.

Простой способ избежать этих проблем — использовать частные методы задания. Например.

```csharp
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

EF Core видит свойство с закрытым методом задания как доступное для чтения и записи. Это означает, что все свойства сопоставляются как и ранее, а ключ все еще может быть создан хранилищем.

Вместо использования частных методов задания свойств можно сделать свойства только для чтения и добавить более явное сопоставление в OnModelCreating. Аналогичным образом некоторые свойства могут быть полностью удалены и заменены только полями. Например, рассмотрим такие типы сущностей:

```csharp
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

```csharp
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

Примечания.

* Ключ "свойство" теперь является полем. Это не `readonly` поле, поэтому можно использовать ключи, созданные в магазине.
* Другие свойства доступны только для чтения и задаются только в конструкторе.
* Если значение первичного ключа задается только EF или считывается из базы данных, нет необходимости включать его в конструктор. Это позволяет оставить ключевое поле "свойство" простым и ясно, что его не следует задавать явно при создании новых блогов или записей.

> [!NOTE]
> Этот код приведет к выдаче предупреждения "169" компилятора, указывающего, что поле никогда не используется. Это можно проигнорировать, так как в реальности EF Core использует поле екстралингуистик способом.

## <a name="injecting-services"></a>Внедрение служб

EF Core также может внедрять "Services" в конструктор типа сущности. Например, можно внедрить следующее:

* `DbContext` — текущий экземпляр контекста, который также может быть типизирован как производный тип DbContext
* `ILazyLoader` — Служба отложенной загрузки — дополнительные сведения см. в [документации по отложенной загрузке](xref:core/querying/related-data) .
* `Action<object, string>` — делегат с отложенной загрузкой — дополнительные сведения см. в [документации по отложенной загрузке](xref:core/querying/related-data) .
* `IEntityType` — EF Core метаданные, связанные с этим типом сущности.

> [!NOTE]
> В настоящее время можно внедрять только службы, известные EF Core. Поддержка внедрения служб приложений рассматривается в будущем выпуске.

Например, внедренный DbContext можно использовать для выборочного доступа к базе данных, чтобы получить сведения о связанных сущностях, не загружая их все. В приведенном ниже примере используется для получения числа записей в блоге без загрузки записей.

```csharp
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

Обратите внимание на некоторые моменты:

* Конструктор является закрытым, так как он вызывается только EF Core, и существует еще один открытый конструктор для общего использования.
* Код, использующий внедренную службу (то есть контекст), защищается от него `null` в случаях, когда EF Core не создает экземпляр.
* Поскольку служба хранится в свойстве для чтения и записи, она будет сброшена при присоединении сущности к новому экземпляру контекста.

> [!WARNING]
> Внедрение DbContext, подобно этому, часто считается антишаблоном, так как он связывает типы сущностей непосредственно с EF Core. Прежде чем использовать подобную службу, внимательно рассмотрите все параметры.
