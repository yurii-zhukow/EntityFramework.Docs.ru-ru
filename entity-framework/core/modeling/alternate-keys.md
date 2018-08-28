---
title: Альтернативные ключи — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: b26d8bc1630af9e811d9c4e7da850a618bc8042e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996975"
---
# <a name="alternate-keys"></a><span data-ttu-id="8acb0-102">Альтернативные ключи</span><span class="sxs-lookup"><span data-stu-id="8acb0-102">Alternate Keys</span></span>

<span data-ttu-id="8acb0-103">Дополнительный ключ служит в качестве альтернативного уникального идентификатора для каждого экземпляра сущности, помимо первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="8acb0-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="8acb0-104">Альтернативные ключи можно использовать в качестве целевого объекта связи.</span><span class="sxs-lookup"><span data-stu-id="8acb0-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="8acb0-105">При использовании реляционной базы данных сопоставляется концепцию индекс или ограничение уникальности на альтернативный столбца первичного ключа и один или несколько ограничений внешнего ключа, которые ссылаются на столбцы.</span><span class="sxs-lookup"><span data-stu-id="8acb0-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="8acb0-106">Если требуется обеспечить уникальность данных столбца, то требуется уникальный индекс, а не альтернативного ключа, см. в разделе [индексы](indexes.md).</span><span class="sxs-lookup"><span data-stu-id="8acb0-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="8acb0-107">В EF альтернативные ключи обеспечивают больше возможностей, чем уникальные индексы, так как они могут использоваться в качестве целевого объекта внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="8acb0-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="8acb0-108">Альтернативные ключи обычно вводятся автоматически, когда требуется и необходимо настроить их вручную.</span><span class="sxs-lookup"><span data-stu-id="8acb0-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="8acb0-109">См. в разделе [соглашения](#conventions) для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="8acb0-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="8acb0-110">Соглашения</span><span class="sxs-lookup"><span data-stu-id="8acb0-110">Conventions</span></span>

<span data-ttu-id="8acb0-111">По соглашению альтернативного ключа вводится автоматически при идентификации свойством, которое не является первичным ключом, что целевым объектом отношения.</span><span class="sxs-lookup"><span data-stu-id="8acb0-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="8acb0-112">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="8acb0-112">Data Annotations</span></span>

<span data-ttu-id="8acb0-113">Не альтернативные ключи можно настроить с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="8acb0-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="8acb0-114">Текучий API</span><span class="sxs-lookup"><span data-stu-id="8acb0-114">Fluent API</span></span>

<span data-ttu-id="8acb0-115">Fluent API можно использовать для настройки единственное свойство являться и резервным ключом.</span><span class="sxs-lookup"><span data-stu-id="8acb0-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

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

<span data-ttu-id="8acb0-116">Fluent API также можно настроить несколько свойств для являться и резервным ключом (известный как альтернативный составного ключа).</span><span class="sxs-lookup"><span data-stu-id="8acb0-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

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
