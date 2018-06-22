---
title: Типы сущностей с помощью конструкторов - EF Core
author: ajcvickers
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
ms.technology: entity-framework-core
uid: core/modeling/constructors
ms.openlocfilehash: 8cea624c295f99ef54cb8b4758642eade03c235e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812499"
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="40d56-102">Типы сущностей с помощью конструкторов</span><span class="sxs-lookup"><span data-stu-id="40d56-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="40d56-103">Этот компонент впервые появился в EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="40d56-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="40d56-104">Начиная с EF Core 2.1, возможна теперь определять конструктор с параметрами и Core EF вызвать этот конструктор при создании экземпляра сущности.</span><span class="sxs-lookup"><span data-stu-id="40d56-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="40d56-105">Параметры конструктора могут быть привязаны к сопоставленного свойства или для различных типов служб для упрощения поведения, такие как отложенную загрузку.</span><span class="sxs-lookup"><span data-stu-id="40d56-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="40d56-106">Начиная с версии 2.1 Core EF все привязки конструктор является соглашением.</span><span class="sxs-lookup"><span data-stu-id="40d56-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="40d56-107">Конфигурация из определенных конструкторов для использования планируется в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="40d56-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="40d56-108">Привязки для сопоставленного свойства</span><span class="sxs-lookup"><span data-stu-id="40d56-108">Binding to mapped properties</span></span>

<span data-ttu-id="40d56-109">Рассмотрим типичный модель сообщение в блоге:</span><span class="sxs-lookup"><span data-stu-id="40d56-109">Consider a typical Blog/Post model:</span></span>

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

<span data-ttu-id="40d56-110">При EF Core создает экземпляры этих типов, таких как для результатов запроса, будет сначала вызывается конструктор по умолчанию без параметров и задайте для каждого свойства значение из базы данных.</span><span class="sxs-lookup"><span data-stu-id="40d56-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="40d56-111">Однако если EF Core выполняет поиск конструктора с параметрами с имена параметров и типы, которые совпадают с сопоставлены свойства, то он вместо этого будет вызывать параметризованный конструктор со значениями этих свойств и не будет устанавливать каждое свойство явно.</span><span class="sxs-lookup"><span data-stu-id="40d56-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="40d56-112">Пример:</span><span class="sxs-lookup"><span data-stu-id="40d56-112">For example:</span></span>

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
<span data-ttu-id="40d56-113">Необходимо отметить следующее:</span><span class="sxs-lookup"><span data-stu-id="40d56-113">Some things to note:</span></span>
* <span data-ttu-id="40d56-114">Не все свойства должны иметь параметров конструктора.</span><span class="sxs-lookup"><span data-stu-id="40d56-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="40d56-115">Например свойство Post.Content не задается любого параметра конструктора, поэтому EF Core будет задано после вызова конструктора обычным способом.</span><span class="sxs-lookup"><span data-stu-id="40d56-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="40d56-116">Типы параметров и имена должны совпадать имена и типы свойств, за исключением того, что свойства могут быть стиле Pascal пока параметры находятся в стиле Camel.</span><span class="sxs-lookup"><span data-stu-id="40d56-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="40d56-117">Основные EF не удалось установить свойства навигации (например, в блоге или выше в блогах) с помощью конструктора.</span><span class="sxs-lookup"><span data-stu-id="40d56-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="40d56-118">Конструктор может быть открытым, закрытым, или иметь любой уровень доступа.</span><span class="sxs-lookup"><span data-stu-id="40d56-118">The constructor can be public, private, or have any other accessibility.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="40d56-119">Свойства только для чтения</span><span class="sxs-lookup"><span data-stu-id="40d56-119">Read-only properties</span></span>

<span data-ttu-id="40d56-120">После установки свойств через конструктор имеет смысл сделать некоторые из них только для чтения.</span><span class="sxs-lookup"><span data-stu-id="40d56-120">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="40d56-121">Это поддерживает EF Core, но существуют некоторые особенности нужно обратить внимание:</span><span class="sxs-lookup"><span data-stu-id="40d56-121">EF Core supports this, but there are some things to look out for:</span></span>
* <span data-ttu-id="40d56-122">Свойства без задания не сопоставляются с соглашением.</span><span class="sxs-lookup"><span data-stu-id="40d56-122">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="40d56-123">(Таким образом, как правило, для сопоставления свойства, которые не должны сопоставляться, например вычисляемого свойства.)</span><span class="sxs-lookup"><span data-stu-id="40d56-123">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="40d56-124">С помощью автоматически создаваемых значений ключа требуется ключевое свойство, чтения и записи, поскольку значение ключа должно быть задано генератором ключа, при вставке новых сущностей.</span><span class="sxs-lookup"><span data-stu-id="40d56-124">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="40d56-125">Для использования закрытого задания — простой способ избежать следующие действия.</span><span class="sxs-lookup"><span data-stu-id="40d56-125">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="40d56-126">Пример:</span><span class="sxs-lookup"><span data-stu-id="40d56-126">For example:</span></span>
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
<span data-ttu-id="40d56-127">EF Core видит свойства с помощью закрытого setter чтения и записи, что означает, что все свойства сопоставляются как и раньше, и ключ можно по-прежнему создаваться хранилища.</span><span class="sxs-lookup"><span data-stu-id="40d56-127">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="40d56-128">Вместо использования закрытого задания является сделать свойства действительно только для чтения и Добавление более явное сопоставление в OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="40d56-128">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="40d56-129">Аналогично некоторые свойства можно полностью удалены и заменены только поля.</span><span class="sxs-lookup"><span data-stu-id="40d56-129">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="40d56-130">Например рассмотрим этих типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="40d56-130">For example, consider these entity types:</span></span>

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
<span data-ttu-id="40d56-131">И эту настройку в OnModelCreating:</span><span class="sxs-lookup"><span data-stu-id="40d56-131">And this configuration in OnModelCreating:</span></span>
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
<span data-ttu-id="40d56-132">Необходимо отметить следующее:</span><span class="sxs-lookup"><span data-stu-id="40d56-132">Things to note:</span></span>
* <span data-ttu-id="40d56-133">Теперь поле является ключ «свойство».</span><span class="sxs-lookup"><span data-stu-id="40d56-133">The key "property" is now a field.</span></span> <span data-ttu-id="40d56-134">Это не `readonly` таким образом, чтобы сформированные хранилищем ключи можно использовать.</span><span class="sxs-lookup"><span data-stu-id="40d56-134">It is not a `readonly` field so that store-generated keys can be used.</span></span> 
* <span data-ttu-id="40d56-135">Другие свойства являются свойства только для чтения только в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="40d56-135">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="40d56-136">Если значение первичного ключа является только задается EF или прочитать из базы данных, нет необходимости включать его в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="40d56-136">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="40d56-137">Это оставляет ключ «свойство» как простой поле и указывает на то, что его не следует задавать явным образом при создании новых блогах или сообщения.</span><span class="sxs-lookup"><span data-stu-id="40d56-137">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="40d56-138">Этот код вызовет предупреждение "169", указывающее, что поле никогда не используется.</span><span class="sxs-lookup"><span data-stu-id="40d56-138">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="40d56-139">Это сообщение можно проигнорировать, так как на самом деле EF Core, используя extralinguistic образом поле.</span><span class="sxs-lookup"><span data-stu-id="40d56-139">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="40d56-140">Добавление службы</span><span class="sxs-lookup"><span data-stu-id="40d56-140">Injecting services</span></span>

<span data-ttu-id="40d56-141">EF Core также можно ввести «службы» в конструктор типа сущности.</span><span class="sxs-lookup"><span data-stu-id="40d56-141">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="40d56-142">Например ниже могут быть добавлены:</span><span class="sxs-lookup"><span data-stu-id="40d56-142">For example, the following can be injected:</span></span>
* <span data-ttu-id="40d56-143">`DbContext` -текущего экземпляра контекста, также могут быть типизированы как вашего производного типа DbContext</span><span class="sxs-lookup"><span data-stu-id="40d56-143">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="40d56-144">`ILazyLoader` -службы отложенная загрузка — в разделе [отложенная загрузка документации](../querying/related-data.md) Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="40d56-144">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="40d56-145">`Action<object, string>` -Делегат отложенная загрузка — в разделе [отложенная загрузка документации](../querying/related-data.md) Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="40d56-145">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="40d56-146">`IEntityType` -EF Core метаданные, связанные с этого типа сущностей</span><span class="sxs-lookup"><span data-stu-id="40d56-146">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="40d56-147">Начиная с версии 2.1 Core EF могут быть добавлены только службы, известен EF ядром.</span><span class="sxs-lookup"><span data-stu-id="40d56-147">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="40d56-148">Поддержка добавления служб приложений считается в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="40d56-148">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="40d56-149">Например введенный DbContext можно выборочно доступа к базе данных для получения сведений о связанных сущностей без загрузки их все.</span><span class="sxs-lookup"><span data-stu-id="40d56-149">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="40d56-150">В следующем примере это используется для получения количество записей в записи блога без загрузки сообщения:</span><span class="sxs-lookup"><span data-stu-id="40d56-150">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

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
<span data-ttu-id="40d56-151">Несколько замечания об этом.</span><span class="sxs-lookup"><span data-stu-id="40d56-151">A few things to notice about this:</span></span>
* <span data-ttu-id="40d56-152">Конструктор закрыт, так как он вызывается только основных компонентов EF и другой конструктор, открытый для общего использования.</span><span class="sxs-lookup"><span data-stu-id="40d56-152">The constructor is private, since it is only ever called by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="40d56-153">Код, использующий службу подставляемого (т. е. контекст) защитного по нему, `null` для обработки случаев, где EF Core не создает экземпляр.</span><span class="sxs-lookup"><span data-stu-id="40d56-153">The code using the injected service (i.e. the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="40d56-154">Поскольку службы хранится в свойство чтения/записи, оно будет сброшено при присоединении к новому экземпляру контекста сущности.</span><span class="sxs-lookup"><span data-stu-id="40d56-154">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="40d56-155">Вводится DbContext следующим образом часто считается антишаблон, так как оно прикрепляет к типам сущностей непосредственно к EF Core.</span><span class="sxs-lookup"><span data-stu-id="40d56-155">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="40d56-156">Внимательно рассмотрите все параметры, прежде чем с помощью внесения службы следующим образом.</span><span class="sxs-lookup"><span data-stu-id="40d56-156">Carefully consider all options before using service injection like this.</span></span>
