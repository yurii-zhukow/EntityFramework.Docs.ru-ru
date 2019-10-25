---
title: Наследование (реляционная база данных) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: c660107619470a726fe13ad8eee2850749e6dcd9
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812091"
---
# <a name="inheritance-relational-database"></a>Наследование (реляционная база данных)

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

Наследование в модели EF используется для управления тем, как наследование в классах сущностей представлено в базе данных.

> [!NOTE]  
> В настоящее время в EF Core реализуется только шаблон «одна таблица на иерархию». Другие распространенные шаблоны, такие как Table-for Type (TPT) и Table-on-Type (TPC), пока недоступны.

## <a name="conventions"></a>Соглашения

По соглашению наследование будет сопоставляться с использованием шаблона «одна таблица на иерархию». Функция «подтаблица» использует одну таблицу для хранения данных всех типов в иерархии. Столбец дискриминатора используется для указания типа, который представляет каждая строка.

EF Core будет настраивать наследование только в том случае, если в модель явно включены два или более унаследованных типа (см. Дополнительные сведения в разделе [наследование](../inheritance.md) ).

Ниже приведен пример простого сценария наследования и данных, хранящихся в таблице реляционной базы данных с использованием шаблона «подтаблица». Столбец *дискриминатора* определяет, какой тип *блога* хранится в каждой строке.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/InheritanceDbSets.cs)] -->
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

>[!NOTE]
> Столбцы базы данных автоматически допускают значение null при необходимости при использовании сопоставления «одна таблица».

## <a name="data-annotations"></a>Заметки к данным

Для настройки наследования нельзя использовать заметки к данным.

## <a name="fluent-api"></a>Текучий API

API-интерфейс Fluent можно использовать для настройки имени и типа столбца дискриминатора и значений, которые используются для обнаружения каждого типа в иерархии.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
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

В приведенных выше примерах дискриминатор создается как [свойство теневой копии](xref:core/modeling/shadow-properties) базовой сущности иерархии. Так как это свойство в модели, его можно настроить так же, как и другие свойства. Например, чтобы задать максимальную длину по умолчанию, используется дискриминатор по соглашению:

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

Дискриминатор также можно сопоставить с реальным свойством CLR в сущности. Пример:

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

Объединяя эти две вещи, можно сопоставлять дискриминатор с реальным свойством и настраивать его:

```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
