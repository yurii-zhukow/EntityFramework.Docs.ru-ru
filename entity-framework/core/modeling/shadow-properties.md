---
title: Свойства тени — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 5fdc4c50c295f73d0fa5eef3518adf4d3eb95599
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197699"
---
# <a name="shadow-properties"></a>Свойства тени

Теневые свойства — это свойства, которые не определены в классе сущности .NET, но определены для этого типа сущности в модели EF Core. Значения и состояния этих свойств хранятся исключительно в средстве отслеживания изменений.

Теневые свойства полезны при наличии в базе данных данных, которые не должны быть представлены в сопоставленных типах сущностей. Они чаще всего используются для свойств внешнего ключа, где связь между двумя сущностями представлена значением внешнего ключа в базе данных, но Управление связью осуществляется на основе типов сущностей с помощью свойств навигации между типами сущностей.

Значения теневых свойств можно получить и изменить с помощью `ChangeTracker` API.

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

На теневые свойства можно ссылаться в запросах LINQ `EF.Property` с помощью статического метода.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Соглашения

Теневые свойства могут создаваться по соглашению при обнаружении связи, но в классе зависимой сущности не найдено свойство внешнего ключа. В этом случае будет введено свойство теневого внешнего ключа. Свойству теневого внешнего ключа будет присвоено имя `<navigation property name><principal key property name>` (Навигация по зависимой сущности, которая указывает на основную сущность, используется для именования). Если имя свойства ключа участника включает имя свойства навигации, то имя будет иметь `<principal key property name>`значение. Если в зависимой сущности нет свойства навигации, в его месте используется имя типа участника.

Например, следующий листинг кода приведет `BlogId` `Post` к тому, что свойство Shadow будет введено в сущность.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/ShadowForeignKey.cs)] -->
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

Теневые свойства не могут быть созданы с помощью заметок к данным.

## <a name="fluent-api"></a>Текучий API

Для настройки теневых свойств можно использовать API-интерфейс Fluent. После вызова перегрузки `Property` строки можно связать любой из вызовов конфигурации для других свойств.

Если имя, передаваемое в `Property` метод, совпадает с именем существующего свойства (теневого или определенного для класса сущностей), то код настроит это существующее свойство, а не создаст новое свойство Shadow.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/ShadowProperty.cs?highlight=7,8)] -->
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
