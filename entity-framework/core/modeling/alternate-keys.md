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
# <a name="alternate-keys"></a>Альтернативные ключи

Дополнительный ключ служит в качестве альтернативного уникального идентификатора для каждого экземпляра сущности, помимо первичного ключа. Альтернативные ключи можно использовать в качестве целевого объекта связи. При использовании реляционной базы данных сопоставляется концепцию индекс или ограничение уникальности на альтернативный столбца первичного ключа и один или несколько ограничений внешнего ключа, которые ссылаются на столбцы.

> [!TIP]  
> Если требуется обеспечить уникальность данных столбца, то требуется уникальный индекс, а не альтернативного ключа, см. в разделе [индексы](indexes.md). В EF альтернативные ключи обеспечивают больше возможностей, чем уникальные индексы, так как они могут использоваться в качестве целевого объекта внешнего ключа.

Альтернативные ключи обычно вводятся автоматически, когда требуется и необходимо настроить их вручную. См. в разделе [соглашения](#conventions) для получения дополнительных сведений.

## <a name="conventions"></a>Соглашения

По соглашению альтернативного ключа вводится автоматически при идентификации свойством, которое не является первичным ключом, что целевым объектом отношения.

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

Не альтернативные ключи можно настроить с помощью заметок к данным.

## <a name="fluent-api"></a>Текучий API

Fluent API можно использовать для настройки единственное свойство являться и резервным ключом.

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

Fluent API также можно настроить несколько свойств для являться и резервным ключом (известный как альтернативный составного ключа).

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
