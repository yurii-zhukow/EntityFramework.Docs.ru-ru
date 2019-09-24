---
title: Индексы — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: b6f11401b69bd8e8795f6b22e5392ba16fc9ba2e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197244"
---
# <a name="indexes"></a>Индексы

Индексы являются распространенной концепцией для многих хранилищ данных. Хотя их реализация в хранилище данных может различаться, они используются для выполнения уточняющих запросов, основанных на столбце (или наборе столбцов), которые более эффективны.

## <a name="conventions"></a>Соглашения

По соглашению индекс создается в каждом свойстве (или наборе свойств), используемом в качестве внешнего ключа.

## <a name="data-annotations"></a>Заметки к данным

Индексы нельзя создавать с помощью заметок к данным.

## <a name="fluent-api"></a>Текучий API

Вы можете использовать API Fluent, чтобы указать индекс для одного свойства. По умолчанию индексы не являются уникальными.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Index.cs?highlight=7,8)] -->
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

Можно также указать, что индекс должен быть уникальным, то есть ни одна из двух сущностей не может иметь одинаковые значения для заданных свойств.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

Можно также указать индекс более чем для одного столбца.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexComposite.cs?highlight=7,8)] -->
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
> Для каждого отдельного набора свойств существует только один индекс. Если вы используете API Fluent для настройки индекса для набора свойств, в котором уже определен индекс, либо в соответствии с соглашением, либо с помощью предыдущей конфигурации, вы измените определение этого индекса. Это полезно, если необходимо дополнительно настроить индекс, созданный по соглашению.
