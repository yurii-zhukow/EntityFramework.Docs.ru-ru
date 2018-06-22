---
title: Альтернативные ключи - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
ms.technology: entity-framework-core
uid: core/modeling/alternate-keys
ms.openlocfilehash: 09f86a8932b71ec8f30ee90a088091a00233c20f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052474"
---
# <a name="alternate-keys"></a><span data-ttu-id="0ca61-102">Альтернативные ключи</span><span class="sxs-lookup"><span data-stu-id="0ca61-102">Alternate Keys</span></span>

<span data-ttu-id="0ca61-103">Дополнительный ключ служит в качестве альтернативного уникального идентификатора для каждого экземпляра сущности, помимо первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="0ca61-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="0ca61-104">Альтернативные ключи можно использовать как целевым объектом связи.</span><span class="sxs-lookup"><span data-stu-id="0ca61-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="0ca61-105">При использовании реляционной базы данных сопоставляется с понятием индекс или ограничение unique для альтернативного ключевые столбцы и один или несколько ограничений внешнего ключа, ссылающихся столбцов.</span><span class="sxs-lookup"><span data-stu-id="0ca61-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="0ca61-106">Если требуется обеспечить уникальность данных столбца, то требуется уникальный индекс, а не дополнительный ключ, см. раздел [индексы](indexes.md).</span><span class="sxs-lookup"><span data-stu-id="0ca61-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="0ca61-107">В EF альтернативные ключи предоставляют больше функциональных возможностей, чем уникальные индексы, так как они могут использоваться в качестве целевого объекта для внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="0ca61-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="0ca61-108">Альтернативные ключи обычно вводятся автоматически, если это требуется, и необходимо вручную настроить их.</span><span class="sxs-lookup"><span data-stu-id="0ca61-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="0ca61-109">В разделе [соглашения](#conventions) для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="0ca61-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="0ca61-110">Соглашения</span><span class="sxs-lookup"><span data-stu-id="0ca61-110">Conventions</span></span>

<span data-ttu-id="0ca61-111">По соглашению дополнительный ключ вводится автоматически при идентификации свойством, не первичный ключ в качестве целевого объекта отношения.</span><span class="sxs-lookup"><span data-stu-id="0ca61-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/AlternateKey.cs?highlight=12)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .HasForeignKey(p => p.BlogUrl)
            .HasPrincipalKey(b => b.Url);
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

    public string BlogUrl { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="0ca61-112">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="0ca61-112">Data Annotations</span></span>

<span data-ttu-id="0ca61-113">Альтернативные ключи не следует задавать с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="0ca61-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="0ca61-114">Fluent API</span><span class="sxs-lookup"><span data-stu-id="0ca61-114">Fluent API</span></span>

<span data-ttu-id="0ca61-115">Fluent API можно использовать один свойство может являться и резервным ключом.</span><span class="sxs-lookup"><span data-stu-id="0ca61-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => c.LicensePlate);
    }
}

class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```

<span data-ttu-id="0ca61-116">Можно также использовать Fluent API для настройки нескольких свойств являться и резервным ключом (известных как составной альтернативное ключ).</span><span class="sxs-lookup"><span data-stu-id="0ca61-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```
