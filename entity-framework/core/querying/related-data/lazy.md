---
title: Отложенная загрузка связанных данных — EF Core
description: Отложенная загрузка связанных данных с помощью Entity Framework Core
author: roji
ms.date: 9/8/2020
uid: core/querying/related-data/lazy
ms.openlocfilehash: 55622b9c5a8f70ef4e7246d6eb14678036948f18
ms.sourcegitcommit: 4860d036ea0fb392c28799907bcc924c987d2d7b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97635462"
---
# <a name="lazy-loading-of-related-data"></a><span data-ttu-id="8f213-103">Отложенная загрузка связанных данных</span><span class="sxs-lookup"><span data-stu-id="8f213-103">Lazy Loading of Related Data</span></span>

## <a name="lazy-loading-with-proxies"></a><span data-ttu-id="8f213-104">Отложенная загрузка с прокси-серверами</span><span class="sxs-lookup"><span data-stu-id="8f213-104">Lazy loading with proxies</span></span>

<span data-ttu-id="8f213-105">Самый простой способ использовать отложенную загрузку — установить пакет [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) и включить его с помощью вызова `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="8f213-105">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="8f213-106">Пример:</span><span class="sxs-lookup"><span data-stu-id="8f213-106">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```

<span data-ttu-id="8f213-107">При использовании AddDbContext:</span><span class="sxs-lookup"><span data-stu-id="8f213-107">Or when using AddDbContext:</span></span>

```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```

<span data-ttu-id="8f213-108">Затем EF Core включит отложенную загрузку любого свойства навигации, которое может быть переопределено, то есть оно должно быть типа `virtual` и быть в классе, который может быть унаследован.</span><span class="sxs-lookup"><span data-stu-id="8f213-108">EF Core will then enable lazy loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="8f213-109">Например, в следующих сущностях свойства навигации `Post.Blog` и `Blog.Posts` будут загружены отложено.</span><span class="sxs-lookup"><span data-stu-id="8f213-109">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>

```csharp
public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public virtual Blog Blog { get; set; }
}
```

> [!WARNING]
> <span data-ttu-id="8f213-110">Отложенная загрузка может приводить к излишним циклическим обращениям к базе данных (так называемая проблема N+1). Соблюдайте осторожность, чтобы избежать этого.</span><span class="sxs-lookup"><span data-stu-id="8f213-110">Lazy loading can cause unneeded extra database roundtrips to occur (the so-called N+1 problem), and care should be taken to avoid this.</span></span> <span data-ttu-id="8f213-111">Дополнительные сведения см. в разделе, посвященном [производительности](xref:core/performance/efficient-querying#beware-of-lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="8f213-111">See the [performance section](xref:core/performance/efficient-querying#beware-of-lazy-loading) for more details.</span></span>

## <a name="lazy-loading-without-proxies"></a><span data-ttu-id="8f213-112">Отложенная загрузка без прокси-серверов</span><span class="sxs-lookup"><span data-stu-id="8f213-112">Lazy loading without proxies</span></span>

<span data-ttu-id="8f213-113">Прокси-серверы с отложенной загрузкой работают, внедряя службу `ILazyLoader` в сущность, как описано в статье [Entity Type Constructors](xref:core/modeling/constructors) (Конструкторы типов сущностей).</span><span class="sxs-lookup"><span data-stu-id="8f213-113">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](xref:core/modeling/constructors).</span></span> <span data-ttu-id="8f213-114">Пример:</span><span class="sxs-lookup"><span data-stu-id="8f213-114">For example:</span></span>

```csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```

<span data-ttu-id="8f213-115">Этот метод не требует, чтобы наследуемые типы сущностей или свойства навигации были виртуальными, и экземплярам сущностей, созданным с помощью `new`, можно отложено загружаться после прикрепления к контексту.</span><span class="sxs-lookup"><span data-stu-id="8f213-115">This method doesn't require entity types to be inherited from or navigation properties to be virtual, and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="8f213-116">Однако для этого требуется ссылка на службу `ILazyLoader`, которая определена в пакете [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/).</span><span class="sxs-lookup"><span data-stu-id="8f213-116">However, it requires a reference to the `ILazyLoader` service, which is defined in the [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) package.</span></span> <span data-ttu-id="8f213-117">Этот пакет содержит минимальный набор типов, поэтому он не оказывает большого влияния.</span><span class="sxs-lookup"><span data-stu-id="8f213-117">This package contains a minimal set of types so that there is little impact in depending on it.</span></span> <span data-ttu-id="8f213-118">Тем не менее, чтобы полностью избежать зависимости от любых пакетов EF Core в типах сущностей, можно ввести метод `ILazyLoader.Load` в качестве делегата.</span><span class="sxs-lookup"><span data-stu-id="8f213-118">However, to completely avoid depending on any EF Core packages in the entity types, it's possible to inject the `ILazyLoader.Load` method as a delegate.</span></span> <span data-ttu-id="8f213-119">Пример:</span><span class="sxs-lookup"><span data-stu-id="8f213-119">For example:</span></span>

```csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```

<span data-ttu-id="8f213-120">В приведенном выше коде используется метод расширения `Load`, чтобы упростить использование делегата:</span><span class="sxs-lookup"><span data-stu-id="8f213-120">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>

```csharp
public static class PocoLoadingExtensions
{
    public static TRelated Load<TRelated>(
        this Action<object, string> loader,
        object entity,
        ref TRelated navigationField,
        [CallerMemberName] string navigationName = null)
        where TRelated : class
    {
        loader?.Invoke(entity, navigationName);

        return navigationField;
    }
}
```

> [!NOTE]
> <span data-ttu-id="8f213-121">Параметр конструктора для делегата с отложенной загрузкой должен называться "lazyLoader".</span><span class="sxs-lookup"><span data-stu-id="8f213-121">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="8f213-122">Конфигурация для использования отличного от этого имени запланирована в будущей версии.</span><span class="sxs-lookup"><span data-stu-id="8f213-122">Configuration to use a different name than this is planned for a future release.</span></span>
