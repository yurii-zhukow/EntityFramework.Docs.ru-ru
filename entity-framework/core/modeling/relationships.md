---
title: Связи - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
ms.technology: entity-framework-core
uid: core/modeling/relationships
ms.openlocfilehash: 1732d32643effb0f12111191ed4ba3abb4182d93
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
ms.locfileid: "26053034"
---
# <a name="relationships"></a>Отношения

Связь определяет, как две сущности связаны друг с другом. В реляционной базе данных это будет представлен ограничение внешнего ключа.

> [!NOTE]  
> Большинство примеров в этой статье используется один ко многим для демонстрации концепции. См. Примеры одному "и" многие ко многим " [другие шаблоны связь](#other-relationship-patterns) в конце статьи.

## <a name="definition-of-terms"></a>Определение терминов

Существует несколько терминов, используемых для описания связей

* **Зависимой сущности:** это сущность, которая содержит внешние ключевые свойства. Иногда называют 'child' связи.

* **Основной сущности:** это сущность, которая содержит первичный/альтернативного ключевые свойства. Иногда называют «parent» связи.

* **Внешний ключ:** свойств в зависимой сущности, которая используется для хранения значения основной ключевого свойства, относящиеся к сущности.

* **Основной ключ:** свойства, которое однозначно определяет основной сущности. Это может быть первичным ключом или дополнительный ключ.

* **Свойство навигации:** свойство, определенное в основной и зависимый сущность, которая содержит ссылки на связанные объекты.

  * **Свойство навигации коллекции:** свойство навигации, которое содержит ссылки на множество связанных сущностей.

  * **Ссылаться на свойство навигации:** свойство навигации, которое содержит ссылку на одну связанную сущность.

  * **Обратное свойство навигации:** при обсуждении свойству навигации определенного, этот термин относится к свойству навигации на другом конце связи.

В следующем коде показано отношение "один ко многим" между `Blog` и`Post`

* `Post`является зависимой сущности

* `Blog`является основной сущности

* `Post.BlogId`внешний ключ

* `Blog.BlogId`основной ключ (в данном случае это первичный ключ, а не дополнительный ключ)

* `Post.Blog`является свойством навигации ссылки

* `Blog.Posts`является свойством навигации коллекции

* `Post.Blog`свойство навигации обратный `Blog.Posts` (и наоборот)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Соглашения

По соглашению связь создается, если свойство навигации, обнаруженных на тип. Если тип, который он указывает не могут быть сопоставлены как скалярный тип поставщиком базы данных, свойство считается свойством навигации.

> [!NOTE]  
> Связи, которые обнаруживаются в соответствии с соглашением всегда будет использовать первичный ключ основной сущности. Чтобы выбрать дополнительный ключ, дополнительная настройка выполняется с помощью плавного API.

### <a name="fully-defined-relationships"></a>Полностью определенные связи

Чаще всего используется шаблон для связи — имеют свойства навигации, определенные на обоих концах связи и свойству внешнего ключа, определенное в классе зависимой сущности.

* Найденные парой свойств навигации между двумя типами, они будут настроены как свойства навигации обратный той же связи.

* Если зависимая сущность содержит свойство с именем `<primary key property name>`, `<navigation property name><primary key property name>`, или `<principal entity name><primary key property name>` , то он будет настроен в качестве внешнего ключа.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> Если существует несколько свойств навигации, определенных между двумя типами (т. е. существует несколько различных пары переходы, которые указывают друг с другом), затем связи не будут созданы по соглашению, и необходимо будет вручную настроить их для идентификации как Сопоставить свойства навигации.

### <a name="no-foreign-key-property"></a>Нет свойство внешнего ключа

Рекомендуется иметь свойство внешнего ключа, определенное в классе зависимой сущности, однако он не требуется. При обнаружении не свойство внешнего ключа с именем будут вводиться тени свойство внешнего ключа `<navigation property name><principal key property name>` (см. [замещения свойств](shadow-properties.md) для получения дополнительной информации).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Одному свойству навигации

Включая только одно свойство навигации (без обратной навигации и не свойство внешнего ключа) достаточно определена по соглашению связь. Вы также можете к одному свойству навигации и свойству внешнего ключа.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Каскадное удаление

По соглашению каскадное удаление будет присвоено *Cascade* для необходимые связи и *ClientSetNull* для связей, необязательно. *CASCADE* означает зависимые сущности также удаляются. *ClientSetNull* означает, что зависимые сущности, которые не были загружены в память останется без изменений и должны быть удалены вручную или обновлен, чтобы она указывала на допустимый основной сущности. Для сущностей, которые загружаются в память ядра EF попытается установить для свойства внешнего ключа значение null.

. В разделе [связи обязательные и необязательные](#required-and-optional-relationships) раздел разницу между обязательных и необязательных связи.

В разделе [каскадное удаление](../saving/cascade-delete.md) для поведения и значения по умолчанию используется соглашение об удалении Дополнительные сведения о различных.

## <a name="data-annotations"></a>Заметки к данным

Существует два данных заметок, которые можно использовать для настройки связи, `[ForeignKey]` и `[InverseProperty]`.

### <a name="foreignkey"></a>[ForeignKey]

Заметки данных можно использовать для настройки, какое свойство следует использовать как свойство внешнего ключа для данного отношения. Обычно это делается, если свойство внешнего ключа не обнаруживаются по соглашению.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> `[ForeignKey]` Заметки можно поместить в свойство навигации, либо в связи. Он не должен перейти на свойство навигации в классе зависимой сущности.

### <a name="inverseproperty"></a>[InverseProperty]

Заметки данных можно использовать для настройки, как сопоставить свойства навигации для сущностей зависимой и участника. Обычно это делается при наличии более двух свойств навигации между двумя типами сущностей.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a>Fluent API

Чтобы настроить плавного API связи, начинается с определения свойства навигации, которые образуют связь. `HasOne`или `HasMany` определяет свойство навигации типа сущности, в начале конфигурации. Вы затем цепочку вызов `WithOne` или `WithMany` для идентификации обратной навигации. `HasOne`/`WithOne`используется для свойств навигации ссылки и `HasMany` / `WithMany` используются для свойств навигации коллекции.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a>Одному свойству навигации

Если имеется только одно свойство навигации, то существуют перегрузки без параметров `WithOne` и `WithMany`. Это означает, что существует по существу является ссылкой или коллекцией на другом конце канала связи, но отсутствует свойство навигации, включенных в класс сущностей.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a>Внешний ключ

Fluent API можно использовать для настройки того, какое свойство следует использовать как свойство внешнего ключа для данного отношения.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

В следующем коде показано, как настроить составного внешнего ключа.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

Можно использовать перегруженный строка `HasForeignKey(...)` для настройки свойств тени в качестве внешнего ключа (см. [замещения свойств](shadow-properties.md) для получения дополнительной информации). Рекомендуется явно добавить в модель свойство теневого перед его использованием в качестве внешнего ключа (как показано ниже).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a>Основной ключ

Если требуется внешний ключ к ссылке на свойство не является первичным ключом, можно использовать Fluent API для настройки свойство основного ключа для связи. Свойство, которое настроенному в качестве основного ключа будет автоматически будет настроен как дополнительный ключ (см. [альтернативные ключи](alternate-keys.md) для получения дополнительной информации).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/PrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => s.CarLicensePlate)
            .HasPrincipalKey(c => c.LicensePlate);
    }
}

public class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

В следующем коде показано, как настройка составного ключа субъекта.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => new { s.CarState, s.CarLicensePlate })
            .HasPrincipalKey(c => new { c.State, c.LicensePlate });
    }
}

public class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarState { get; set; }
    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

> [!WARNING]  
> Порядок, в котором указываются свойства основного ключа должен совпадать с порядком, в котором они указаны для внешнего ключа.

### <a name="required-and-optional-relationships"></a>Обязательные и необязательные связей

Fluent API позволяет настроить, является ли связь обязательными или необязательными. В конечном счете это контролирует, представляет ли свойство внешнего ключа обязательными или необязательными. Это наиболее полезно при использовании внешнего ключа состояния тени. Если у вас есть свойство внешнего ключа в классе сущностей, то requiredness связи определяется на основе того, является ли свойство внешнего ключа обязательными или необязательными (см. [обязательные и необязательные свойства](required-optional.md) для получения дополнительных сведений сведения о).

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/Required.cs?highlight=11)] -->
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
            .IsRequired();
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
```

### <a name="cascade-delete"></a>Каскадное удаление

Fluent API можно использовать для настройки поведения delete cascade для конкретной связи явным образом.

В разделе [каскадного удаления](../saving/cascade-delete.md) на раздел Сохранение данных подробное описание каждого параметра.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CascadeDelete.cs?highlight=11)] -->
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
            .OnDelete(DeleteBehavior.Cascade);
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

    public int? BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="other-relationship-patterns"></a>Другие модели, связи

### <a name="one-to-one"></a>Один к одному

Один к одному отношениями свойством навигации ссылки на обеих сторонах. Они следуют тем же правилам, как один ко многим отношения, но появилась в свойстве внешнего ключа, чтобы убедиться, что каждому участнику связан только один зависит от уникального индекса.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/Relationships/OneToOne.cs?highlight=6,15,16)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

> [!NOTE]  
> EF выбирает одну из сущности должны находиться зависимого его возможностями для обнаружения свойству внешнего ключа. Если неверный сущности, выбирается в качестве зависимого, чтобы исправить эту ошибку можно использовать Fluent API.

При настройке связи с помощью плавного API, используйте `HasOne` и `WithOne` методы.

При настройке внешнего ключа, необходимо указать тип зависимого объекта - Обратите внимание, предоставленный для универсального параметра `HasForeignKey` в списке ниже. В отношении один ко многим было ясно, что зависит от сущности с ссылки навигации и с коллекции является участником. Но это не так в однозначное - таким образом необходимость явного определения.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/OneToOne.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<BlogImage> BlogImages { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasOne(p => p.BlogImage)
            .WithOne(i => i.Blog)
            .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogForeignKey { get; set; }
    public Blog Blog { get; set; }
}
```

### <a name="many-to-many"></a>«Многие ко многим»

Многие ко многим связи без класса сущности, который представляет таблицу соединения еще не поддерживается. Тем не менее может представлять многие ко многим, включая класс сущностей для таблицы, соединения и две отдельные связи один ко многим сопоставления.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(t => new { t.PostId, t.TagId });

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Post)
            .WithMany(p => p.PostTags)
            .HasForeignKey(pt => pt.PostId);

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Tag)
            .WithMany(t => t.PostTags)
            .HasForeignKey(pt => pt.TagId);
    }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class PostTag
{
    public int PostId { get; set; }
    public Post Post { get; set; }

    public string TagId { get; set; }
    public Tag Tag { get; set; }
}
```
