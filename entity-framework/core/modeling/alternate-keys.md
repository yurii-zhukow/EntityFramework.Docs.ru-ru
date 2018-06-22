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
# <a name="alternate-keys"></a>Альтернативные ключи

Дополнительный ключ служит в качестве альтернативного уникального идентификатора для каждого экземпляра сущности, помимо первичного ключа. Альтернативные ключи можно использовать как целевым объектом связи. При использовании реляционной базы данных сопоставляется с понятием индекс или ограничение unique для альтернативного ключевые столбцы и один или несколько ограничений внешнего ключа, ссылающихся столбцов.

> [!TIP]  
> Если требуется обеспечить уникальность данных столбца, то требуется уникальный индекс, а не дополнительный ключ, см. раздел [индексы](indexes.md). В EF альтернативные ключи предоставляют больше функциональных возможностей, чем уникальные индексы, так как они могут использоваться в качестве целевого объекта для внешнего ключа.

Альтернативные ключи обычно вводятся автоматически, если это требуется, и необходимо вручную настроить их. В разделе [соглашения](#conventions) для получения дополнительных сведений.

## <a name="conventions"></a>Соглашения

По соглашению дополнительный ключ вводится автоматически при идентификации свойством, не первичный ключ в качестве целевого объекта отношения.

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

## <a name="data-annotations"></a>Заметки к данным

Альтернативные ключи не следует задавать с помощью заметок к данным.

## <a name="fluent-api"></a>Fluent API

Fluent API можно использовать один свойство может являться и резервным ключом.

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

Можно также использовать Fluent API для настройки нескольких свойств являться и резервным ключом (известных как составной альтернативное ключ).

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
