---
title: Включая &, исключая типы EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: ca83b1c432bdf4853dba81e12ec4a739bc8218dc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197373"
---
# <a name="including--excluding-types"></a><span data-ttu-id="10758-102">Включение & исключением типов</span><span class="sxs-lookup"><span data-stu-id="10758-102">Including & Excluding Types</span></span>

<span data-ttu-id="10758-103">Включение типа в модель означает, что EF имеет метаданные об этом типе и будет пытаться считывать и записывать экземпляры из базы данных или из нее.</span><span class="sxs-lookup"><span data-stu-id="10758-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="10758-104">Соглашения</span><span class="sxs-lookup"><span data-stu-id="10758-104">Conventions</span></span>

<span data-ttu-id="10758-105">По соглашению типы, предоставляемые в `DbSet` свойствах контекста, включаются в модель.</span><span class="sxs-lookup"><span data-stu-id="10758-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="10758-106">Кроме того, также включены типы, упоминаемые `OnModelCreating` в методе.</span><span class="sxs-lookup"><span data-stu-id="10758-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="10758-107">Наконец, все типы, которые обнаруживаются рекурсивным просмотром свойств навигации обнаруженных типов, также включаются в модель.</span><span class="sxs-lookup"><span data-stu-id="10758-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="10758-108">**Например, в следующем коде обнаруживаются все три типа:**</span><span class="sxs-lookup"><span data-stu-id="10758-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="10758-109">`Blog`так как он предоставляется в `DbSet` свойстве контекста</span><span class="sxs-lookup"><span data-stu-id="10758-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="10758-110">`Post`так как обнаруживается через `Blog.Posts` свойство навигации</span><span class="sxs-lookup"><span data-stu-id="10758-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="10758-111">`AuditEntry`так как он упоминается в`OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="10758-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/IncludedTypes.cs?highlight=3,7,16)] -->
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

## <a name="data-annotations"></a><span data-ttu-id="10758-112">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="10758-112">Data Annotations</span></span>

<span data-ttu-id="10758-113">Заметки к данным можно использовать для исключения типа из модели.</span><span class="sxs-lookup"><span data-stu-id="10758-113">You can use Data Annotations to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a><span data-ttu-id="10758-114">Текучий API</span><span class="sxs-lookup"><span data-stu-id="10758-114">Fluent API</span></span>

<span data-ttu-id="10758-115">API-интерфейс Fluent можно использовать для исключения типа из модели.</span><span class="sxs-lookup"><span data-stu-id="10758-115">You can use the Fluent API to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
