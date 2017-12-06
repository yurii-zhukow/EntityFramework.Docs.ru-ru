---
title: "Включая & за исключением типов - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
ms.technology: entity-framework-core
uid: core/modeling/included-types
ms.openlocfilehash: a8d7293a144968d2506bdcc76e55a1a0b1e3fd4b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="including--excluding-types"></a><span data-ttu-id="16bd7-102">Включая & за исключением типов</span><span class="sxs-lookup"><span data-stu-id="16bd7-102">Including & Excluding Types</span></span>

<span data-ttu-id="16bd7-103">В модели означает EF содержит метаданные о этот тип и будет предпринята попытка чтения и записи экземпляров из/в базе данных, включая тип.</span><span class="sxs-lookup"><span data-stu-id="16bd7-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="16bd7-104">Соглашения</span><span class="sxs-lookup"><span data-stu-id="16bd7-104">Conventions</span></span>

<span data-ttu-id="16bd7-105">По соглашению, типы, предоставляемые в `DbSet` свойства на контекст, включены в модель.</span><span class="sxs-lookup"><span data-stu-id="16bd7-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="16bd7-106">Кроме того, типы, упомянутые в `OnModelCreating` метод также будут включены.</span><span class="sxs-lookup"><span data-stu-id="16bd7-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="16bd7-107">Наконец все типы, которые обнаруживаются путем изучения свойства навигации обнаруженных типов рекурсивно также включаются в модель.</span><span class="sxs-lookup"><span data-stu-id="16bd7-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="16bd7-108">**Например в следующем примере кода обнаруживаются все три типа:**</span><span class="sxs-lookup"><span data-stu-id="16bd7-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="16bd7-109">`Blog`так как она предоставляется в `DbSet` свойства контекста</span><span class="sxs-lookup"><span data-stu-id="16bd7-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="16bd7-110">`Post`Поскольку обнаружения с помощью `Blog.Posts` свойство навигации</span><span class="sxs-lookup"><span data-stu-id="16bd7-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="16bd7-111">`AuditEntry`так как оно встречается в`OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="16bd7-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="16bd7-112">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="16bd7-112">Data Annotations</span></span>

<span data-ttu-id="16bd7-113">Чтобы исключить тип из модели можно использовать заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="16bd7-113">You can use Data Annotations to exclude a type from the model.</span></span>

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

## <a name="fluent-api"></a><span data-ttu-id="16bd7-114">Fluent API</span><span class="sxs-lookup"><span data-stu-id="16bd7-114">Fluent API</span></span>

<span data-ttu-id="16bd7-115">Чтобы исключить тип из модели можно использовать Fluent API.</span><span class="sxs-lookup"><span data-stu-id="16bd7-115">You can use the Fluent API to exclude a type from the model.</span></span>

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
