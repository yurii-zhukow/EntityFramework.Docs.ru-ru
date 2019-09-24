---
title: Альтернативные ключи — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: 87df5d174a1db12fb3ab763ac76c3b863a83087e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197475"
---
# <a name="alternate-keys"></a><span data-ttu-id="99797-102">Альтернативные ключи</span><span class="sxs-lookup"><span data-stu-id="99797-102">Alternate Keys</span></span>

<span data-ttu-id="99797-103">Альтернативный ключ служит в качестве альтернативного уникального идентификатора для каждого экземпляра сущности в дополнение к первичному ключу.</span><span class="sxs-lookup"><span data-stu-id="99797-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="99797-104">Альтернативные ключи можно использовать в качестве целевого объекта связи.</span><span class="sxs-lookup"><span data-stu-id="99797-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="99797-105">При использовании реляционной базы данных эта схема сопоставляется с концепцией уникального индекса или ограничения для альтернативных ключевых столбцов и одного или нескольких ограничений внешнего ключа, которые ссылаются на эти столбцы.</span><span class="sxs-lookup"><span data-stu-id="99797-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="99797-106">Если нужно просто обеспечить уникальность столбца, тогда вам нужен уникальный индекс, а не альтернативный ключ, см. раздел [индексы](indexes.md).</span><span class="sxs-lookup"><span data-stu-id="99797-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="99797-107">В EF альтернативные ключи предоставляют больше функций, чем уникальные индексы, так как их можно использовать в качестве целевого объекта внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="99797-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="99797-108">Альтернативные ключи обычно вводятся при необходимости, и их не нужно настраивать вручную.</span><span class="sxs-lookup"><span data-stu-id="99797-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="99797-109">Дополнительные сведения см. в разделе [соглашения](#conventions) .</span><span class="sxs-lookup"><span data-stu-id="99797-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="99797-110">Соглашения</span><span class="sxs-lookup"><span data-stu-id="99797-110">Conventions</span></span>

<span data-ttu-id="99797-111">По соглашению альтернативный ключ вводится при определении свойства, которое не является первичным ключом, в качестве целевого объекта связи.</span><span class="sxs-lookup"><span data-stu-id="99797-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/AlternateKey.cs?highlight=12)] -->
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

## <a name="data-annotations"></a><span data-ttu-id="99797-112">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="99797-112">Data Annotations</span></span>

<span data-ttu-id="99797-113">Альтернативные ключи не могут быть настроены с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="99797-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="99797-114">Текучий API</span><span class="sxs-lookup"><span data-stu-id="99797-114">Fluent API</span></span>

<span data-ttu-id="99797-115">API-интерфейс Fluent можно использовать для настройки одного свойства в качестве альтернативного ключа.</span><span class="sxs-lookup"><span data-stu-id="99797-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?highlight=7,8)] -->
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

<span data-ttu-id="99797-116">Кроме того, API-интерфейс Fluent можно использовать для настройки нескольких свойств в качестве альтернативного ключа (называемого составным альтернативным ключом).</span><span class="sxs-lookup"><span data-stu-id="99797-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?highlight=7,8)] -->
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
