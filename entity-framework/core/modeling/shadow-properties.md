---
title: Свойства тени — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: b7b7b10642564dfa3dbc05755188b5b5c63e0d03
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993806"
---
# <a name="shadow-properties"></a>Свойства тени

Теневые свойства являются свойствами, которые не определены в классе сущностей .NET, но определены для этого типа сущности в модель EF Core. Значение и состояние этих свойств сохраняется исключительно в объекте отслеживания изменений.

Теневые свойства полезны в тех случаях, когда в базе данных, которые не должны быть предоставлены для типов сущностей, сопоставленных данных. Чаще всего они используются для свойств внешнего ключа, связь между двумя сущностями, представленного значение внешнего ключа в базе данных, когда связь осуществляется в типах сущностей, с помощью свойств навигации между типами сущностей.

Значения свойств тени можно получить и изменить с помощью `ChangeTracker` API.

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Теневые свойства можно ссылаться в запросах LINQ через `EF.Property` статический метод.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Соглашения

Теневые свойства могут создаваться по соглашению, если обнаружены связи, но не свойство внешнего ключа не найден в классе зависимой сущности. В этом случае будут вводиться теневые свойства внешнего ключа. Теневые свойства внешнего ключа будет называться `<navigation property name><principal key property name>` (Навигация по зависимой сущности, которое указывает основной сущности, используется для именования). Если имя участника ключевое свойство содержит имя свойства навигации, а затем имя будет просто `<principal key property name>`. Если ни одно свойство навигации на зависимой сущности, вместо него используется имени основного типа.

Например, следующий листинг кода приведет к `BlogId` свойство тени, представленной в `Post` сущности.

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

Теневые свойства не могут создаваться с помощью заметок к данным.

## <a name="fluent-api"></a>Текучий API

Fluent API можно использовать для настройки свойств тени. После вызова перегрузки строка `Property` можно объединять в цепочку любой конфигурации вызовов, что и для других свойств.

Если передано имя `Property` метода совпадает с именем существующего свойства (свойство тени или одно определенное в классе сущности), то код будет настраивать существующие свойства, а не обучаться новое свойство тени.

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
