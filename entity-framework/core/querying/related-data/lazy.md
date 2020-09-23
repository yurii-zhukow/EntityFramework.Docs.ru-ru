---
title: Отложенная загрузка связанных данных — EF Core
description: Отложенная загрузка связанных данных с помощью Entity Framework Core
author: roji
ms.date: 9/8/2020
uid: core/querying/related-data/lazy
ms.openlocfilehash: c42cde469e2be38d53a46cb6c5c252a088978e5c
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078893"
---
# <a name="lazy-loading-of-related-data"></a>Отложенная загрузка связанных данных

## <a name="lazy-loading-with-proxies"></a>Отложенная загрузка с прокси-серверами

Самый простой способ использовать отложенную загрузку — установить пакет [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) и включить его с помощью вызова `UseLazyLoadingProxies`. Пример:

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```

При использовании AddDbContext:

```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```

Затем EF Core включит отложенную загрузку любого свойства навигации, которое может быть переопределено, то есть оно должно быть типа `virtual` и быть в классе, который может быть унаследован. Например, в следующих сущностях свойства навигации `Post.Blog` и `Blog.Posts` будут загружены отложено.

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

## <a name="lazy-loading-without-proxies"></a>Отложенная загрузка без прокси-серверов

Прокси-серверы с отложенной загрузкой работают, внедряя службу `ILazyLoader` в сущность, как описано в статье [Entity Type Constructors](xref:core/modeling/constructors) (Конструкторы типов сущностей). Пример:

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

Этот метод не требует, чтобы наследуемые типы сущностей или свойства навигации были виртуальными, и экземплярам сущностей, созданным с помощью `new`, можно отложено загружаться после прикрепления к контексту. Однако для этого требуется ссылка на службу `ILazyLoader`, которая определена в пакете [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/). Этот пакет содержит минимальный набор типов, поэтому он не оказывает большого влияния. Тем не менее, чтобы полностью избежать зависимости от любых пакетов EF Core в типах сущностей, можно ввести метод `ILazyLoader.Load` в качестве делегата. Пример:

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

В приведенном выше коде используется метод расширения `Load`, чтобы упростить использование делегата:

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
> Параметр конструктора для делегата с отложенной загрузкой должен называться "lazyLoader". Конфигурация для использования отличного от этого имени запланирована в будущей версии.
