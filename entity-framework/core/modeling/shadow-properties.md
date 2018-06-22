---
title: Свойства тени — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
ms.technology: entity-framework-core
uid: core/modeling/shadow-properties
ms.openlocfilehash: 8c7f70789ddc6ebd24f9bfae931069834db593c2
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053554"
---
# <a name="shadow-properties"></a>Свойства тени

Замещения свойств являются свойствами, которые не определены в классе сущностей .NET, но определены для данного типа сущности в EF базовой модели. Значение и состояние этих свойств поддерживается исключительно в объекте отслеживания изменений.

Замещения свойств полезны в тех случаях, когда в базе данных, которые не должны быть предоставлены для типов сущностей, сопоставленных данных. Чаще всего используются для свойства внешнего ключа, связь между двумя сущностями, представленного значение внешнего ключа в базе данных, когда связь осуществляется для типов сущностей, с помощью свойств навигации между типами сущностей.

Значения свойств можно получить и изменить с помощью `ChangeTracker` API.

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Замещения свойств можно ссылаться в запросах LINQ через `EF.Property` статический метод.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Соглашения

Замещения свойств могут создаваться по соглашению, если обнаруженные связи, но не свойство внешнего ключа не найден в классе зависимой сущности. В этом случае будут вводиться тени свойство внешнего ключа. Свойство внешнего ключа тень будет называться `<navigation property name><principal key property name>` (Навигация по зависимой сущности, которое указывает основной сущности, используется для именования). Если имя свойства основного ключа содержит имя свойства навигации, а затем имя будет просто `<principal key property name>`. Если зависимая сущность не свойство навигации, вместо него используется имя типа субъекта.

Например, следующий листинг кода приведет к `BlogId` проникновения в свойство теневого `Post` сущности.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/ShadowForeignKey.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
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
```

## <a name="data-annotations"></a>Заметки к данным

Замещения свойств не могут создаваться с помощью заметок к данным.

## <a name="fluent-api"></a>Fluent API

Fluent API можно использовать для настройки свойств тени. После вызова перегруженный строка `Property` можно соединить в цепочку любой конфигурации вызовов, что и для других свойств.

Если имя, указанное в `Property` метода совпадает с именем существующего свойства (свойство теневого или один определенный для класса сущностей), а затем код будет настроить существующие свойства, а не обучаться новое свойство тени.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/ShadowProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property<DateTime>("LastUpdated");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
