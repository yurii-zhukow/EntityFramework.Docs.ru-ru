---
title: Включение и исключение типов — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: a5a14f62524754fed179e9a41fac5e29faf185ca
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996154"
---
# <a name="including--excluding-types"></a><span data-ttu-id="fdcc8-102">Включение и исключение типов</span><span class="sxs-lookup"><span data-stu-id="fdcc8-102">Including & Excluding Types</span></span>

<span data-ttu-id="fdcc8-103">В модели означает EF содержит метаданные о, введите и будет предпринята попытка чтения и записи экземпляров в базе данных, включая тип.</span><span class="sxs-lookup"><span data-stu-id="fdcc8-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="fdcc8-104">Соглашения</span><span class="sxs-lookup"><span data-stu-id="fdcc8-104">Conventions</span></span>

<span data-ttu-id="fdcc8-105">По соглашению, типов, которые доступны в `DbSet` свойства в контексте, включены в модель.</span><span class="sxs-lookup"><span data-stu-id="fdcc8-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="fdcc8-106">Кроме того, типы, упомянутые в `OnModelCreating` метод также будут включены.</span><span class="sxs-lookup"><span data-stu-id="fdcc8-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="fdcc8-107">Наконец все типы, которые можно найти, рекурсивно, изучение свойства навигации обнаруженных типов также включены в модели.</span><span class="sxs-lookup"><span data-stu-id="fdcc8-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="fdcc8-108">**Например в следующем примере кода обнаруживаются всех трех типов:**</span><span class="sxs-lookup"><span data-stu-id="fdcc8-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="fdcc8-109">`Blog` так как она предоставляется в `DbSet` свойство контекста</span><span class="sxs-lookup"><span data-stu-id="fdcc8-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="fdcc8-110">`Post` Поскольку обнаружения с помощью `Blog.Posts` свойство навигации</span><span class="sxs-lookup"><span data-stu-id="fdcc8-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="fdcc8-111">`AuditEntry` так как оно встречается в `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="fdcc8-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/IncludedTypes.cs?highlight=3,7,16)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<AuditEntry>();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}

public class AuditEntry
{
    public int AuditEntryId { get; set; }
    public string Username { get; set; }
    public string Action { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="fdcc8-112">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="fdcc8-112">Data Annotations</span></span>

<span data-ttu-id="fdcc8-113">Чтобы исключить тип из модели можно использовать заметки к данным.</span><span class="sxs-lookup"><span data-stu-id="fdcc8-113">You can use Data Annotations to exclude a type from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/IgnoreType.cs?highlight=9)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

[NotMapped]
public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="fdcc8-114">Текучий API</span><span class="sxs-lookup"><span data-stu-id="fdcc8-114">Fluent API</span></span>

<span data-ttu-id="fdcc8-115">Можно использовать Fluent API, чтобы исключить тип из модели.</span><span class="sxs-lookup"><span data-stu-id="fdcc8-115">You can use the Fluent API to exclude a type from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IgnoreType.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Ignore<BlogMetadata>();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```
