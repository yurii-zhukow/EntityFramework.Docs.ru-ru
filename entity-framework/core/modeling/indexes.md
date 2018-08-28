---
title: Индексы — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 87fe893243377e3ab83d419ae9bedf813ca50c3f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995484"
---
# <a name="indexes"></a>Индексы

Индексы являются понятием распространенных в многих хранилищах данных. Хотя их реализации в хранилище данных могут отличаться, они используются превратить уточняющие запросы, на основании столбца (или набор столбцов), более эффективным.

## <a name="conventions"></a>Соглашения

По соглашению индекс создается в каждом свойство (или набор свойств), которые используются в качестве внешнего ключа.

## <a name="data-annotations"></a>Заметки к данным

Индексы могут не создаваться с помощью заметок к данным.

## <a name="fluent-api"></a>Текучий API

Fluent API можно использовать для указания индекса по одному свойству. По умолчанию индексы не уникальны.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Index.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

Можно также указать что индекса должны быть уникальными, это означает, что две сущности не может иметь же значения для заданного свойства.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

Можно также указать индекс по нескольким столбцам.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .HasIndex(p => new { p.FirstName, p.LastName });
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

> [!TIP]  
> Есть только один индекс отдельный набор свойств. Если вы используете Fluent API для настройки индекса на набор свойств, которые уже есть индекс, определенный, по соглашению или предыдущей конфигурации, затем подлежащий изменению определения этого индекса. Это полезно, если вы хотите продолжить настройку индекса, который был создан в соответствии с соглашением.
