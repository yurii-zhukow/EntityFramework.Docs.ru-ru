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
# <a name="including--excluding-types"></a>Включение & исключением типов

Включение типа в модель означает, что EF имеет метаданные об этом типе и будет пытаться считывать и записывать экземпляры из базы данных или из нее.

## <a name="conventions"></a>Соглашения

По соглашению типы, предоставляемые в `DbSet` свойствах контекста, включаются в модель. Кроме того, также включены типы, упоминаемые `OnModelCreating` в методе. Наконец, все типы, которые обнаруживаются рекурсивным просмотром свойств навигации обнаруженных типов, также включаются в модель.

**Например, в следующем коде обнаруживаются все три типа:**

* `Blog`так как он предоставляется в `DbSet` свойстве контекста

* `Post`так как обнаруживается через `Blog.Posts` свойство навигации

* `AuditEntry`так как он упоминается в`OnModelCreating`

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

## <a name="data-annotations"></a>Заметки к данным

Заметки к данным можно использовать для исключения типа из модели.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>Текучий API

API-интерфейс Fluent можно использовать для исключения типа из модели.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
