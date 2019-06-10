---
title: Связи — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 793401362788e865c89ce01b6246b1ba14c36c8a
ms.sourcegitcommit: 8b9568211d37a1c36da9533fa1ac2ef063b0bf8c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2019
ms.locfileid: "66815008"
---
# <a name="relationships"></a>Отношения

Отношение определяет, каким образом две сущности связаны друг с другом. В реляционной базе данных оно представлено ограничение внешнего ключа.

> [!NOTE]  
> Большинство примеров в этой статье для демонстрации понятия использовать отношение один ко многим. Примеры использования одному "и" многие ко многим "см. [другие шаблоны связь](#other-relationship-patterns) в конце статьи.

## <a name="definition-of-terms"></a>Определение терминов

Существует несколько терминов, используемых для описания связей

* **Зависимой сущности:** Это сущность, которая содержит свойства внешнего ключа. Иногда называют «дочерний» связи.

* **Основной сущности:** Это сущность, которая содержит ключевые свойства основного или альтернативное. Иногда называют «parent» связи.

* **Внешний ключ:** Свойства в зависимой сущности, который используется для хранения значений субъекта ключевое свойство, сущность связана с.

* **Основной ключ:** Свойства, которое однозначно определяет основной сущности. Это может быть первичный ключ или резервный ключ.

* **Свойство навигации:** Свойства, определенные в основной и/или зависимой сущности, содержащей ссылки на связанные объекты.

  * **Свойство навигации по коллекции:** Свойство навигации, которое содержит ссылки на множество связанных сущностей.

  * **Свойство навигации ссылки на:** Свойство навигации, которое содержит ссылку на одну связанную сущность.

  * **Свойство обратной навигации:** При обсуждении конкретным свойством навигации, этот термин относится к свойству навигации на другом конце связи.

В следующем примере кода показаны отношение "один ко многим" между `Blog` и `Post`

* `Post` является зависимой сущности

* `Blog` — Это основной сущности

* `Post.BlogId` внешний ключ

* `Blog.BlogId` является главным ключом (в данном случае это первичный ключ, а не альтернативного ключа)

* `Post.Blog` — Это свойство навигации ссылки

* `Blog.Posts` является свойством навигации коллекции

* `Post.Blog` свойство навигации обратный `Blog.Posts` (и наоборот)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Соглашения

По соглашению связь будет создан при наличии свойства навигации, обнаруженных на тип. Свойство считается свойство навигации, если тип, который указывает не могут сопоставляться как скалярный тип текущим поставщиком базы данных.

> [!NOTE]  
> Связи, которые обнаруживаются в соответствии с соглашением, всегда будут обрабатываться первичный ключ основной сущности. Для альтернативного ключа, дополнительная настройка выполняется с помощью Fluent API.

### <a name="fully-defined-relationships"></a>Полностью определенные связи

Самый распространенный шаблон для связи — имеют свойства навигации, определенные на обоих концах связи и свойство внешнего ключа, определенные в классе зависимой сущности.

* При обнаружении пара свойств навигации между двумя типами они будут настроены как свойства навигации обратный той же связи.

* Если зависимая сущность содержит свойство с именем `<primary key property name>`, `<navigation property name><primary key property name>`, или `<principal entity name><primary key property name>` то он будет настроен в качестве внешнего ключа.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> Если есть несколько свойств навигации, определенных между двумя типами (то есть несколько уникальных пара переходов, которые указывают друг с другом), затем связи не будут созданы по соглашению, и необходимо будет вручную настроить их для идентификации как Пара свойств навигации вверх.

### <a name="no-foreign-key-property"></a>Нет свойство внешнего ключа

Рекомендуется иметь свойство внешнего ключа, определенные в классе зависимой сущности, однако он не требуется. При обнаружении не свойство внешнего ключа, свойство внешнего ключа тени познакомитесь с именем `<navigation property name><principal key property name>` (см. в разделе [замещения свойств](shadow-properties.md) Дополнительные сведения).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Одного свойства навигации

Включая только одно свойство навигации (не обратное навигации и не свойство внешнего ключа) достаточно, чтобы иметь связи, определенной по соглашению. Также возможно одного свойства навигации и свойство внешнего ключа.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Каскадное удаление

По соглашению каскадное удаление будет присвоено *Cascade* для необходимые связи и *ClientSetNull* необязательно связей. *CASCADE* означает, что зависимые сущности также удаляются. *ClientSetNull* означает, что зависимые сущности, которые не были загружены в память останется без изменений и должны быть вручную удалены или обновлены с допустимым основной сущности. Для сущностей, которые загружаются в память EF Core попытается установить для свойств внешнего ключа значение null.

См. в разделе [обязательных и необязательных связи](#required-and-optional-relationships) раздел для разницы между обязательных и необязательных связи.

См. в разделе [каскадное удаление](../saving/cascade-delete.md) для поведения и значения по умолчанию, используемый в соответствии с соглашением об удалении Дополнительные сведения о различных.

## <a name="data-annotations"></a>Заметки к данным

Существуют два заметок к данным, которые могут использоваться для настройки связей, `[ForeignKey]` и `[InverseProperty]`. Они доступны в `System.ComponentModel.DataAnnotations.Schema` пространства имен.

### <a name="foreignkey"></a>[ForeignKey]

Заметки данных можно использовать для настройки, какое свойство следует использовать как свойство внешнего ключа для конкретной связи. Обычно это делается свойства внешнего ключа не обнаружен по соглашению.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> `[ForeignKey]` Заметки можно поместить в свойство навигации, либо в связи. Он не должен перейти в свойстве навигации в классе зависимой сущности.

### <a name="inverseproperty"></a>[InverseProperty]

Заметки данных можно использовать для настройки, как сопоставить свойства навигации в зависимой и основной сущности. Обычно это делается при наличии более чем одна пара свойств навигации между двумя типами сущностей.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a>Текучий API

Чтобы настроить отношение в Fluent API, начните с определение свойства навигации, составляющих связи. `HasOne` или `HasMany` определяет свойство навигации типа сущности, в начале конфигурации. Затем вы цепочку вызов `WithOne` или `WithMany` для идентификации обратной навигации. `HasOne`/`WithOne` используются для свойства навигации по ссылке и `HasMany` / `WithMany` используются для свойства навигации по коллекции.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a>Одного свойства навигации

Если имеется только одно свойство навигации, то существуют перегрузки без параметров `WithOne` и `WithMany`. Это означает, что имеется по сути, является ссылкой или коллекцией на другом конце связи, но нет свойства навигации, включенные в классе сущности.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a>Внешний ключ

Fluent API можно использовать для настройки того, какое свойство следует использовать как свойство внешнего ключа для конкретной связи.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?highlight=17)]

В следующем примере кода показаны способы настройки составного внешнего ключа.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?highlight=20)]

Можно использовать перегрузку строка `HasForeignKey(...)` для настройки теневого свойство как внешний ключ (см. в разделе [замещения свойств](shadow-properties.md) Дополнительные сведения). Мы рекомендуем явным образом добавить в модель теневые свойства перед его использованием в качестве внешнего ключа (как показано ниже).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a>Без свойства навигации

Не нужно обязательно предоставить свойство навигации. Можно просто предоставить внешний ключ на одной стороне связи.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a>Ключ субъекта-службы

Если требуется, чтобы внешний ключ для ссылки на свойство, отличный от первичного ключа, можно использовать Fluent API для настройки свойство основного ключа для связи. Свойство, настроенному в качестве основного ключа будет автоматически иметь установки в качестве альтернативного ключа (см. в разделе [альтернативные ключи](alternate-keys.md) Дополнительные сведения).

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

В следующем коде демонстрируется настройка составного ключа субъекта.

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
> Порядок, в котором указываются свойства основного ключа должен соответствовать порядку, в котором они указаны для внешнего ключа.

### <a name="required-and-optional-relationships"></a>Обязательные и необязательные связей

Fluent API позволяет настроить, является ли связь обязательными или необязательными. В конечном счете определяет, является ли свойство внешнего ключа обязательными или необязательными. Это может пригодиться при использовании внешнего ключа состояние тени. Если у вас есть свойство внешнего ключа в классе сущностей, а затем requiredness связи определяется в зависимости от свойства внешнего ключа обязательными или нет (см. в разделе [обязательные и необязательные свойства](required-optional.md) для получения дополнительных сведений сведения о).

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

Fluent API можно использовать для настройки поведения cascade delete для конкретной связи явно.

См. в разделе [каскадное удаление](../saving/cascade-delete.md) на разделе Сохранение данных подробное описание каждого параметра.

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

## <a name="other-relationship-patterns"></a>Другие шаблоны связи

### <a name="one-to-one"></a>Один к одному

Связи один к одному имеют свойства навигации ссылки на обеих сторонах. Они следуют тем же правилам как отношения один ко многим, но появилось в свойстве внешнего ключа, чтобы убедиться, что каждому участнику связан только один зависит от уникального индекса.

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
> EF выберет одной сущности к зависимым его возможностями для обнаружения свойство внешнего ключа. Если выбрать неправильный сущности в качестве зависимого, Fluent API можно использовать для устранения этой проблемы.

Настраивая связь с Fluent API, используйте `HasOne` и `WithOne` методы.

При настройке внешнего ключа, необходимо указать тип зависимой сущности — Обратите внимание, что универсальный параметр, переданный `HasForeignKey` в списке ниже. В отношении один ко многим становится ясно, что зависит от сущности с навигации ссылки на и с коллекции является участником. Однако это не является таким образом, в взаимно-однозначной связи — таким образом необходимость явно определить его.

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

### <a name="many-to-many"></a>Многие ко многим

Связи многие ко многим без класса сущности для представления таблицы соединения еще не поддерживаются. Тем не менее вы можете представить связь многие ко многим, включив класс сущности таблицы соединения и две отдельные связи один ко многим сопоставления.

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
