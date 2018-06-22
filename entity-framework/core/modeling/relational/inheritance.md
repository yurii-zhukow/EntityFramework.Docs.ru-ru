---
title: Наследование (реляционной базы данных) - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 22eed0002b5903d3cfd18a7e4af0fcd2d46a5c4c
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152369"
---
# <a name="inheritance-relational-database"></a>Наследование (реляционной базы данных)

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

Наследование в модели EF используется для управления, как наследование в классах сущностей представляется в базе данных.

> [!NOTE]  
> В настоящее время только таблица на иерархию (TPH) шаблон реализуется EF ядра. Другие распространенные схемы, такие как одна таблица на тип (TPT) и таблица на конкретный тип (TPC) еще не доступны.

## <a name="conventions"></a>Соглашения

По соглашению наследования будут сопоставлены с помощью шаблона таблица на иерархию (TPH). TPH использует одну таблицу для хранения данных для всех типов в иерархии. Столбец дискриминатора используется для определения подходящего, каждая строка представляет.

EF Core только установить наследование, если два или более наследуемых типов явно включены в модель (см. [наследования](../inheritance.md) Дополнительные сведения).

Ниже приведен пример, показывающий простые сценарии и данные, хранящиеся в таблице реляционной базы данных, используя шаблон TPH. *Дискриминатора* столбца определяет, какой тип *блог* хранится в каждой строке.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/Samples/InheritanceDbSets.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<RssBlog> RssBlogs { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

![изображение](_static/inheritance-tph-data.png)

## <a name="data-annotations"></a>Заметки к данным

Заметки данных нельзя использовать для настройки наследования.

## <a name="fluent-api"></a>Текучий API

Чтобы настроить имя и тип столбца дискриминатора и значения, которые используются для идентификации каждого типа в иерархии можно использовать Fluent API.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("blog_type")
            .HasValue<Blog>("blog_base")
            .HasValue<RssBlog>("blog_rss");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

## <a name="configuring-the-discriminator-property"></a>Настройка свойства дискриминатора

В приведенных выше примерах дискриминатора создается как [затемнять свойство](xref:core/modeling/shadow-properties) на это базовая сущность иерархии. Так как это свойство в модели, его можно настроить так же, как и другие свойства. Например, чтобы задать максимальной длины, если используется значение по умолчанию, по соглашению дискриминатора:

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

Дискриминатора также может быть сопоставлен Фактическое свойство CLR в сущности. Пример:
```C#
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("BlogType");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public string BlogType { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

Объединение эти две вещи существует возможность сопоставления дискриминатора свойством реального и настройте его:
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
