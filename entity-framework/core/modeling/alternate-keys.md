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
# <a name="alternate-keys"></a>Альтернативные ключи

Альтернативный ключ служит в качестве альтернативного уникального идентификатора для каждого экземпляра сущности в дополнение к первичному ключу. Альтернативные ключи можно использовать в качестве целевого объекта связи. При использовании реляционной базы данных эта схема сопоставляется с концепцией уникального индекса или ограничения для альтернативных ключевых столбцов и одного или нескольких ограничений внешнего ключа, которые ссылаются на эти столбцы.

> [!TIP]  
> Если нужно просто обеспечить уникальность столбца, тогда вам нужен уникальный индекс, а не альтернативный ключ, см. раздел [индексы](indexes.md). В EF альтернативные ключи предоставляют больше функций, чем уникальные индексы, так как их можно использовать в качестве целевого объекта внешнего ключа.

Альтернативные ключи обычно вводятся при необходимости, и их не нужно настраивать вручную. Дополнительные сведения см. в разделе [соглашения](#conventions) .

## <a name="conventions"></a>Соглашения

По соглашению альтернативный ключ вводится при определении свойства, которое не является первичным ключом, в качестве целевого объекта связи.

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

## <a name="data-annotations"></a>Заметки к данным

Альтернативные ключи не могут быть настроены с помощью заметок к данным.

## <a name="fluent-api"></a>Текучий API

API-интерфейс Fluent можно использовать для настройки одного свойства в качестве альтернативного ключа.

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

Кроме того, API-интерфейс Fluent можно использовать для настройки нескольких свойств в качестве альтернативного ключа (называемого составным альтернативным ключом).

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
