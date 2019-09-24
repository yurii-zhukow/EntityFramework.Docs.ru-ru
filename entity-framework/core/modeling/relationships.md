---
title: Связи — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 1e9c62bec47263ef452c7ac425a0bb446f9371d8
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197649"
---
# <a name="relationships"></a>Отношения

Связь определяет, как две сущности связаны друг с другом. В реляционной базе данных это представление представляется ограничением внешнего ключа.

> [!NOTE]  
> Большинство примеров в этой статье используют связь «один ко многим» для демонстрации концепций. Примеры связей "один к одному" и "многие ко многим" см. в разделе [другие шаблоны связей](#other-relationship-patterns) в конце статьи.

## <a name="definition-of-terms"></a>Определение терминов

Существует ряд терминов, используемых для описания связей.

* **Зависимая сущность:** Это сущность, содержащая свойства внешнего ключа. Иногда называется "дочерним" отношением.

* **Сущность субъекта:** Это сущность, содержащая свойства первичного или альтернативного ключа. Иногда называется "родителем" связи.

* **Внешний ключ:** Свойства в зависимой сущности, используемые для хранения значений свойства ключа участника, с которым связана сущность.

* **Основной ключ:** Свойства, которые однозначно идентифицируют сущность Principal. Это может быть первичный ключ или альтернативный ключ.

* **Свойство навигации:** Свойство, определенное для основной и (или) зависимой сущности, содержащей ссылки на связанные сущности.

  * **Свойство навигации коллекции:** Свойство навигации, содержащее ссылки на множество связанных сущностей.

  * **Свойство навигации по ссылке:** Свойство навигации, содержащее ссылку на одну связанную сущность.

  * **Обратное свойство навигации:** При обсуждении определенного свойства навигации этот термин относится к свойству навигации на другом конце связи.

В следующем листинге кода показана связь «один ко многим» `Blog` между и`Post`

* `Post`является зависимой сущностью

* `Blog`является основной сущностью

* `Post.BlogId`является внешним ключом

* `Blog.BlogId`является основным ключом (в данном случае это первичный ключ, а не альтернативный ключ);

* `Post.Blog`является свойством навигации по ссылке

* `Blog.Posts`— Это свойство навигации коллекции

* `Post.Blog`Свойство обратной навигации `Blog.Posts` (и наоборот)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Соглашения

По соглашению, связь будет создана при обнаружении свойства навигации для типа. Свойство считается свойством навигации, если тип, на который он указывает, не может быть сопоставлен с текущим поставщиком базы данных как скалярный тип.

> [!NOTE]  
> Связи, обнаруженные соглашением, всегда будут указывать первичный ключ основной сущности. Чтобы выбрать альтернативный ключ, необходимо выполнить дополнительную настройку с помощью API Fluent.

### <a name="fully-defined-relationships"></a>Полностью определенные связи

Наиболее распространенным шаблоном для связей является наличие свойств навигации, определенных на обоих концах связи, и свойства внешнего ключа, определенного в зависимом классе сущности.

* Если между двумя типами обнаружена пара свойств навигации, они будут настроены как обратные свойства навигации одной и той же связи.

* Если зависимая сущность содержит свойство с `<primary key property name>`именем `<navigation property name><primary key property name>`, или `<principal entity name><primary key property name>` , то оно будет настроено как внешний ключ.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> Если существует несколько свойств навигации, определенных между двумя типами (то есть более чем одной отдельной парой переходов, которая указывает друг на друга), связи не создаются по соглашению, и вам потребуется вручную настроить их, чтобы определить, как Связывание свойств навигации.

### <a name="no-foreign-key-property"></a>Нет свойства внешнего ключа

Хотя рекомендуется использовать свойство внешнего ключа, определенное в зависимом классе сущности, оно не является обязательным. Если свойство внешнего ключа не найдено, свойство теневого внешнего ключа будет введено с именем `<navigation property name><principal key property name>` (Дополнительные сведения см. в разделе [Свойства тени](shadow-properties.md) ).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Одно свойство навигации

Включение только одного свойства навигации (без обратной навигации и свойства внешнего ключа) достаточно для того, чтобы иметь связь, определенную по соглашению. Можно также иметь одно свойство навигации и внешнее ключевое свойство.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Каскадное удаление

По соглашению каскадное удаление будет установлено в *CASCADE* для требуемых связей и *клиентсетнулл* для необязательных связей. *CASCADE* означает также удаление зависимых сущностей. *Клиентсетнулл* означает, что зависимые сущности, которые не загружены в память, останутся неизменными и должны быть удалены вручную или обновлены, чтобы они указывали на действительную сущность Principal. Для сущностей, загруженных в память, EF Core попытается установить свойства внешнего ключа в значение null.

Различия между обязательными и дополнительными связями см. в разделе [обязательные и дополнительные связи](#required-and-optional-relationships) .

Дополнительные сведения о различных поведениях при удалении и значения по умолчанию, используемые по соглашению, см. в разделе [каскадное удаление](../saving/cascade-delete.md) .

## <a name="data-annotations"></a>Заметки к данным

Существуют две заметки к данным, которые можно использовать для настройки связей, `[ForeignKey]` и `[InverseProperty]`. Они доступны в `System.ComponentModel.DataAnnotations.Schema` пространстве имен.

### <a name="foreignkey"></a>[Фореигнкэй]

Заметки к данным можно использовать для настройки того, какое свойство должно использоваться в качестве свойства внешнего ключа для данной связи. Обычно это делается, когда свойство внешнего ключа не обнаруживается по соглашению.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> `[ForeignKey]` Заметку можно поместить в любое свойство навигации в связи. Не требуется переходить к свойству навигации в классе зависимой сущности.

### <a name="inverseproperty"></a>[Инверсепроперти]

Заметки к данным можно использовать для настройки способа связывания свойств навигации в зависимых и субъектах сущностей. Обычно это делается при наличии более одной пары свойств навигации между двумя типами сущностей.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a>Текучий API

Чтобы настроить связь в API Fluent, начните с определения свойств навигации, составляющих связь. `HasOne`или `HasMany` определяет свойство навигации для типа сущности, на котором начинается настройка. Затем вы позвоните в `WithOne` цепочку или `WithMany` для поиска обратной навигации. `HasOne`/`WithOne`используются для свойств навигации по ссылке и `HasMany` / `WithMany` используются для свойств навигации по коллекциям.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a>Одно свойство навигации

Если у вас есть только одно свойство навигации, то существуют перегрузки `WithOne` без параметров и. `WithMany` Это указывает на то, что на другом конце связи имеется концептуальная ссылка или коллекция, но в классе сущностей не содержится свойство навигации.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a>Внешний ключ

API-интерфейс Fluent можно использовать для настройки того, какое свойство должно использоваться в качестве свойства внешнего ключа для данной связи.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

В следующем примере кода показано, как настроить составной внешний ключ.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

`HasForeignKey(...)` Чтобы настроить свойство теневого копирования в качестве внешнего ключа, можно использовать строку перегрузки в. Дополнительные сведения см. в разделе [Свойства тени](shadow-properties.md) . Рекомендуется явно добавить свойство Shadow в модель, прежде чем использовать ее как внешний ключ (как показано ниже).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a>Без свойства навигации

Указывать свойство навигации не обязательно. Можно просто предоставить внешний ключ на одной стороне связи.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a>Основной ключ

Если требуется, чтобы внешний ключ ссылался на свойство, отличное от первичного ключа, можно использовать API Fluent, чтобы настроить свойство ключа субъекта для связи. Свойство, настраиваемое в качестве ключа участника, будет автоматически настроено в качестве альтернативного ключа (Дополнительные сведения см. в разделе [альтернативные ключи](alternate-keys.md) ).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?highlight=11)] -->
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

В следующем примере кода показано, как настроить составной ключ субъекта.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
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
> Порядок указания свойств ключа субъекта должен совпадать с порядком, в котором они указаны для внешнего ключа.

### <a name="required-and-optional-relationships"></a>Обязательные и необязательные связи

С помощью API-интерфейса Fluent можно настроить, является ли связь обязательной или необязательной. В конечном итоге это определяет, является ли свойство внешнего ключа обязательным или необязательным. Это наиболее полезно при использовании внешнего ключа теневого состояния. Если у вас есть свойство внешнего ключа в классе сущностей, обязательность связи определяется на основе того, является ли свойство внешнего ключа обязательным или необязательным (Дополнительные сведения см. в разделе [обязательные и необязательные свойства](required-optional.md) ).

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?highlight=11)] -->
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

Для явной настройки поведения каскадного удаления для данной связи можно использовать API Fluent.

Подробное описание каждого варианта см. в разделе [каскадное удаление](../saving/cascade-delete.md) раздела сохранение данных.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?highlight=11)] -->
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

## <a name="other-relationship-patterns"></a>Другие шаблоны отношений

### <a name="one-to-one"></a>Один к одному

Связь «один к одному» имеет свойство навигации «ссылка» на обеих сторонах. Они следуют тем же соглашениям, что и отношения "один ко многим", но уникальный индекс вводится в свойство внешнего ключа, чтобы гарантировать, что только один зависимый объект связан с каждым участником.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Relationships/OneToOne.cs?highlight=6,15,16)] -->
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
> EF выберет одну из сущностей, которая будет зависимой, исходя из возможности обнаружения свойства внешнего ключа. Если в качестве зависимого объекта выбрана неверная сущность, можно исправить это с помощью API-интерфейса Fluent.

При настройке связи с помощью API-интерфейса Fluent вы используете `HasOne` методы и. `WithOne`

При настройке внешнего ключа необходимо указать тип зависимой сущности — Обратите внимание на универсальный параметр, предоставленный `HasForeignKey` в приведенном ниже списке. В связи «один ко многим» ясно, что сущность с навигацией по ссылке является зависимой, а она является участником коллекции. Но это не так в связи «один к одному», поэтому необходимо явно определить его.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?highlight=11)] -->
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

Связи «многие ко многим» без класса сущности для представления таблицы соединений пока не поддерживаются. Однако можно представить связь «многие ко многим», включив класс сущности для таблицы Join и составляя две отдельные связи «один ко многим».

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(pt => new { pt.PostId, pt.TagId });

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
