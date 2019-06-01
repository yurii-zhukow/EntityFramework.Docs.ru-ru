---
title: Наследование (реляционная база данных) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 2d0a2abc554f5f115479f886ca3f9f4f01b80b5b
ms.sourcegitcommit: ea1cdec0b982b922a59b9d9301d3ed2b94baca0f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452280"
---
# <a name="inheritance-relational-database"></a>Наследование (реляционная база данных)

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

Наследование в модели EF используется для управления представление наследования в классы сущностей в базе данных.

> [!NOTE]  
> В настоящее время только шаблон таблица на иерархию (TPH) реализована в EF Core. Другие распространенные шаблоны, такие как одна таблица на тип (TPT) и таблица на конкретный тип (TPC) еще не доступны.

## <a name="conventions"></a>Соглашения

По соглашению наследования будут сопоставлены с помощью шаблона одна таблица на иерархию (TPH). TPH использует одну таблицу для хранения данных для всех типов в иерархии. Столбец дискриминатора используется для определения подходящего, каждая строка представляет.

Если два или более наследуемых типов явно включены в модель EF Core только процедура позволяет настроить наследование (см. в разделе [наследования](../inheritance.md) подробности).

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

![image](_static/inheritance-tph-data.png)

>[!NOTE]
> Столбцы базы данных, автоматически становятся допускает значения NULL, при необходимости, при использовании TPH сопоставления.

## <a name="data-annotations"></a>Заметки к данным

Заметки к данным нельзя использовать для настройки наследования.

## <a name="fluent-api"></a>Текучий API

Чтобы настроить имя и тип столбца дискриминатора и значений, которые используются для идентификации каждого типа в иерархии можно использовать Fluent API.

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

В приведенных выше примерах дискриминатора создается как [затемнения свойства](xref:core/modeling/shadow-properties) на базовая сущность иерархии. Так как это свойство в модели, его можно настроить так же, как и другие свойства. Например, чтобы задать Максимальная длина, когда используется по умолчанию, по соглашению дискриминатора:

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

Дискриминатора может быть сопоставлено Фактическое свойство CLR в сущности. Пример:
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

Объединяя эти две вещи можно сопоставить настоящее Свойство дискриминатора и настройте его:
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
