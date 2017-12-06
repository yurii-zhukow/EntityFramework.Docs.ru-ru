---
title: "Индексы - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
ms.technology: entity-framework-core
uid: core/modeling/indexes
ms.openlocfilehash: f57b545d53613cec6887734bf434958ee8fff4d8
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="indexes"></a>Индексы

Индексы являются понятием общих между несколькими хранилищами данных. Хотя их реализации в хранилище данных могут отличаться, они используются для создания уточняющие запросы, основанный на столбце (или набор столбцов), более эффективным.

## <a name="conventions"></a>Соглашения

По соглашению индекс создается в каждой свойство (или набор свойств), которые используются в качестве внешнего ключа.

## <a name="data-annotations"></a>Заметки к данным

Индексы могут не создаваться с помощью заметок к данным.

## <a name="fluent-api"></a>Fluent API

Fluent API можно использовать для указания индекса на одном свойстве. По умолчанию индексы не уникальны.

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

Можно также указать что индекса должно быть уникальным, это означает, что две сущности не может иметь одинаковые значения для данного свойства.

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
> Имеется только один индекс отдельного набора свойств. Если вы используете Fluent API для настройки индекса на набор свойств, которые уже имеет индекс, определенный, либо соглашение о предыдущей конфигурации затем подлежащий изменению определения этого индекса. Это полезно, если вы хотите дополнительно настроить индекс, который был создан в соответствии с соглашением.
