---
title: Включение и исключение типов — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: f533b24312af37634ce4957e43c39ce776bf0bf0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929801"
---
# <a name="including--excluding-types"></a>Включение и исключение типов

В модели означает EF содержит метаданные о, введите и будет предпринята попытка чтения и записи экземпляров в базе данных, включая тип.

## <a name="conventions"></a>Соглашения

По соглашению, типов, которые доступны в `DbSet` свойства в контексте, включены в модель. Кроме того, типы, упомянутые в `OnModelCreating` метод также будут включены. Наконец все типы, которые можно найти, рекурсивно, изучение свойства навигации обнаруженных типов также включены в модели.

**Например в следующем примере кода обнаруживаются всех трех типов:**

* `Blog` так как она предоставляется в `DbSet` свойство контекста

* `Post` Поскольку обнаружения с помощью `Blog.Posts` свойство навигации

* `AuditEntry` так как оно встречается в `OnModelCreating`

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/IncludedTypes.cs?highlight=3,7,16)] -->
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

Чтобы исключить тип из модели можно использовать заметки к данным.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>Текучий API

Можно использовать Fluent API, чтобы исключить тип из модели.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreType.cs?highlight=12)]
