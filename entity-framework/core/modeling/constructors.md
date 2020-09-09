---
title: Типы сущностей с конструкторами — EF Core
description: Использование конструкторов для привязки данных с помощью модели Entity Framework Core
author: ajcvickers
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
uid: core/modeling/constructors
ms.openlocfilehash: 58529a3a68e69a31249460d402027274404dce45
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89617541"
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="00ae0-103">Типы сущностей с конструкторами</span><span class="sxs-lookup"><span data-stu-id="00ae0-103">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="00ae0-104">Это новая возможность в EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="00ae0-104">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="00ae0-105">Начиная с EF Core 2,1, теперь можно определить конструктор с параметрами и EF Core вызвать этот конструктор при создании экземпляра сущности.</span><span class="sxs-lookup"><span data-stu-id="00ae0-105">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="00ae0-106">Параметры конструктора можно привязать к сопоставленным свойствам или к различным типам служб, чтобы упростить такие действия, как отложенная загрузка.</span><span class="sxs-lookup"><span data-stu-id="00ae0-106">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="00ae0-107">Начиная с EF Core 2,1, все привязки конструктора имеют соглашение.</span><span class="sxs-lookup"><span data-stu-id="00ae0-107">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="00ae0-108">Настройка конкретных конструкторов для использования планируется в будущем выпуске.</span><span class="sxs-lookup"><span data-stu-id="00ae0-108">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="00ae0-109">Привязка к сопоставленным свойствам</span><span class="sxs-lookup"><span data-stu-id="00ae0-109">Binding to mapped properties</span></span>

<span data-ttu-id="00ae0-110">Рассмотрим типичную модель блога или публикации:</span><span class="sxs-lookup"><span data-stu-id="00ae0-110">Consider a typical Blog/Post model:</span></span>

``` csharp
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

<span data-ttu-id="00ae0-111">Когда EF Core создает экземпляры этих типов, например, для результатов запроса, сначала вызывается конструктор без параметров по умолчанию, а затем каждому свойству присваивается значение из базы данных.</span><span class="sxs-lookup"><span data-stu-id="00ae0-111">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="00ae0-112">Однако если EF Core находит параметризованный конструктор с именами и типами параметров, совпадающими с сопоставленными свойствами, то вместо этого он вызывает параметризованный конструктор со значениями для этих свойств и не задаст каждое свойство явным образом.</span><span class="sxs-lookup"><span data-stu-id="00ae0-112">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="00ae0-113">Пример:</span><span class="sxs-lookup"><span data-stu-id="00ae0-113">For example:</span></span>

``` csharp
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

<span data-ttu-id="00ae0-114">Обратите внимание на следующие моменты.</span><span class="sxs-lookup"><span data-stu-id="00ae0-114">Some things to note:</span></span>

* <span data-ttu-id="00ae0-115">Не все свойства должны иметь параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="00ae0-115">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="00ae0-116">Например, свойство POST. Content не задается ни одним из параметров конструктора, поэтому EF Core задаст его после вызова конструктора обычным способом.</span><span class="sxs-lookup"><span data-stu-id="00ae0-116">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="00ae0-117">Типы и имена параметров должны совпадать с типами и именами свойств, за исключением того, что свойства могут быть в стиле Pascal, а параметры — в стиле Camel.</span><span class="sxs-lookup"><span data-stu-id="00ae0-117">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="00ae0-118">EF Core не может задать свойства навигации (например, блог или записи выше) с помощью конструктора.</span><span class="sxs-lookup"><span data-stu-id="00ae0-118">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="00ae0-119">Конструктор может быть общедоступным, закрытым или иметь другие специальные возможности.</span><span class="sxs-lookup"><span data-stu-id="00ae0-119">The constructor can be public, private, or have any other accessibility.</span></span> <span data-ttu-id="00ae0-120">Однако для прокси-серверов с отложенной загрузкой требуется, чтобы конструктор был доступен из наследуемого прокси-класса.</span><span class="sxs-lookup"><span data-stu-id="00ae0-120">However, lazy-loading proxies require that the constructor is accessible from the inheriting proxy class.</span></span> <span data-ttu-id="00ae0-121">Обычно это означает, что он является общедоступным или защищенным.</span><span class="sxs-lookup"><span data-stu-id="00ae0-121">Usually this means making it either public or protected.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="00ae0-122">Свойства только для чтения</span><span class="sxs-lookup"><span data-stu-id="00ae0-122">Read-only properties</span></span>

<span data-ttu-id="00ae0-123">Когда свойства задаются с помощью конструктора, может быть целесообразно сделать некоторые из них только для чтения.</span><span class="sxs-lookup"><span data-stu-id="00ae0-123">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="00ae0-124">EF Core поддерживает это, но необходимо обратить внимание на следующие моменты:</span><span class="sxs-lookup"><span data-stu-id="00ae0-124">EF Core supports this, but there are some things to look out for:</span></span>

* <span data-ttu-id="00ae0-125">Свойства без методов задания не сопоставляются по соглашению.</span><span class="sxs-lookup"><span data-stu-id="00ae0-125">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="00ae0-126">(Это предполагает сопоставление свойств, которые не должны сопоставляться, например, вычисленные свойства.)</span><span class="sxs-lookup"><span data-stu-id="00ae0-126">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="00ae0-127">Для использования автоматически создаваемых значений ключа требуется ключевое свойство, которое доступно для чтения и записи, поскольку при вставке новых сущностей значение ключа должно быть задано генератором ключей.</span><span class="sxs-lookup"><span data-stu-id="00ae0-127">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="00ae0-128">Простой способ избежать этих проблем — использовать частные методы задания.</span><span class="sxs-lookup"><span data-stu-id="00ae0-128">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="00ae0-129">Пример:</span><span class="sxs-lookup"><span data-stu-id="00ae0-129">For example:</span></span>

``` csharp
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

<span data-ttu-id="00ae0-130">EF Core видит свойство с закрытым методом задания как доступное для чтения и записи. Это означает, что все свойства сопоставляются как и ранее, а ключ все еще может быть создан хранилищем.</span><span class="sxs-lookup"><span data-stu-id="00ae0-130">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="00ae0-131">Вместо использования частных методов задания свойств можно сделать свойства только для чтения и добавить более явное сопоставление в OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="00ae0-131">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="00ae0-132">Аналогичным образом некоторые свойства могут быть полностью удалены и заменены только полями.</span><span class="sxs-lookup"><span data-stu-id="00ae0-132">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="00ae0-133">Например, рассмотрим следующие типы сущностей:</span><span class="sxs-lookup"><span data-stu-id="00ae0-133">For example, consider these entity types:</span></span>

``` csharp
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

<span data-ttu-id="00ae0-134">И эта конфигурация в OnModelCreating:</span><span class="sxs-lookup"><span data-stu-id="00ae0-134">And this configuration in OnModelCreating:</span></span>

``` csharp
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

<span data-ttu-id="00ae0-135">Примечания.</span><span class="sxs-lookup"><span data-stu-id="00ae0-135">Things to note:</span></span>

* <span data-ttu-id="00ae0-136">Ключ "свойство" теперь является полем.</span><span class="sxs-lookup"><span data-stu-id="00ae0-136">The key "property" is now a field.</span></span> <span data-ttu-id="00ae0-137">Это не `readonly` поле, поэтому можно использовать ключи, созданные в магазине.</span><span class="sxs-lookup"><span data-stu-id="00ae0-137">It is not a `readonly` field so that store-generated keys can be used.</span></span>
* <span data-ttu-id="00ae0-138">Другие свойства доступны только для чтения и задаются только в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="00ae0-138">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="00ae0-139">Если значение первичного ключа задается только EF или считывается из базы данных, нет необходимости включать его в конструктор.</span><span class="sxs-lookup"><span data-stu-id="00ae0-139">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="00ae0-140">Это позволяет оставить ключевое поле "свойство" простым и ясно, что его не следует задавать явно при создании новых блогов или записей.</span><span class="sxs-lookup"><span data-stu-id="00ae0-140">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="00ae0-141">Этот код приведет к выдаче предупреждения "169" компилятора, указывающего, что поле никогда не используется.</span><span class="sxs-lookup"><span data-stu-id="00ae0-141">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="00ae0-142">Это можно проигнорировать, так как в реальности EF Core использует поле екстралингуистик способом.</span><span class="sxs-lookup"><span data-stu-id="00ae0-142">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="00ae0-143">Внедрение служб</span><span class="sxs-lookup"><span data-stu-id="00ae0-143">Injecting services</span></span>

<span data-ttu-id="00ae0-144">EF Core также может внедрять "Services" в конструктор типа сущности.</span><span class="sxs-lookup"><span data-stu-id="00ae0-144">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="00ae0-145">Например, можно внедрить следующее:</span><span class="sxs-lookup"><span data-stu-id="00ae0-145">For example, the following can be injected:</span></span>

* <span data-ttu-id="00ae0-146">`DbContext` — текущий экземпляр контекста, который также может быть типизирован как производный тип DbContext</span><span class="sxs-lookup"><span data-stu-id="00ae0-146">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="00ae0-147">`ILazyLoader` — Служба отложенной загрузки — дополнительные сведения см. в [документации по отложенной загрузке](xref:core/querying/related-data) .</span><span class="sxs-lookup"><span data-stu-id="00ae0-147">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](xref:core/querying/related-data) for more details</span></span>
* <span data-ttu-id="00ae0-148">`Action<object, string>` — делегат с отложенной загрузкой — дополнительные сведения см. в [документации по отложенной загрузке](xref:core/querying/related-data) .</span><span class="sxs-lookup"><span data-stu-id="00ae0-148">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](xref:core/querying/related-data) for more details</span></span>
* <span data-ttu-id="00ae0-149">`IEntityType` — EF Core метаданные, связанные с этим типом сущности.</span><span class="sxs-lookup"><span data-stu-id="00ae0-149">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="00ae0-150">Начиная с EF Core 2,1, можно внедрять только службы, известные EF Core.</span><span class="sxs-lookup"><span data-stu-id="00ae0-150">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="00ae0-151">Поддержка внедрения служб приложений рассматривается в будущем выпуске.</span><span class="sxs-lookup"><span data-stu-id="00ae0-151">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="00ae0-152">Например, внедренный DbContext можно использовать для выборочного доступа к базе данных, чтобы получить сведения о связанных сущностях, не загружая их все.</span><span class="sxs-lookup"><span data-stu-id="00ae0-152">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="00ae0-153">В приведенном ниже примере используется для получения числа записей в блоге без загрузки записей.</span><span class="sxs-lookup"><span data-stu-id="00ae0-153">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

``` csharp
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

<span data-ttu-id="00ae0-154">Обратите внимание на некоторые моменты:</span><span class="sxs-lookup"><span data-stu-id="00ae0-154">A few things to notice about this:</span></span>

* <span data-ttu-id="00ae0-155">Конструктор является закрытым, так как он вызывается только EF Core, и существует еще один открытый конструктор для общего использования.</span><span class="sxs-lookup"><span data-stu-id="00ae0-155">The constructor is private, since it is only ever called by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="00ae0-156">Код, использующий внедренную службу (то есть контекст), защищается от него `null` в случаях, когда EF Core не создает экземпляр.</span><span class="sxs-lookup"><span data-stu-id="00ae0-156">The code using the injected service (that is, the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="00ae0-157">Поскольку служба хранится в свойстве для чтения и записи, она будет сброшена при присоединении сущности к новому экземпляру контекста.</span><span class="sxs-lookup"><span data-stu-id="00ae0-157">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="00ae0-158">Внедрение DbContext, подобно этому, часто считается антишаблоном, так как он связывает типы сущностей непосредственно с EF Core.</span><span class="sxs-lookup"><span data-stu-id="00ae0-158">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="00ae0-159">Прежде чем использовать подобную службу, внимательно рассмотрите все параметры.</span><span class="sxs-lookup"><span data-stu-id="00ae0-159">Carefully consider all options before using service injection like this.</span></span>
