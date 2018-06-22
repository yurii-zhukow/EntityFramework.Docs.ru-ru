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
# <a name="inheritance-relational-database"></a><span data-ttu-id="20e5d-102">Наследование (реляционной базы данных)</span><span class="sxs-lookup"><span data-stu-id="20e5d-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="20e5d-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="20e5d-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="20e5d-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="20e5d-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="20e5d-105">Наследование в модели EF используется для управления, как наследование в классах сущностей представляется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="20e5d-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="20e5d-106">В настоящее время только таблица на иерархию (TPH) шаблон реализуется EF ядра.</span><span class="sxs-lookup"><span data-stu-id="20e5d-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="20e5d-107">Другие распространенные схемы, такие как одна таблица на тип (TPT) и таблица на конкретный тип (TPC) еще не доступны.</span><span class="sxs-lookup"><span data-stu-id="20e5d-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="20e5d-108">Соглашения</span><span class="sxs-lookup"><span data-stu-id="20e5d-108">Conventions</span></span>

<span data-ttu-id="20e5d-109">По соглашению наследования будут сопоставлены с помощью шаблона таблица на иерархию (TPH).</span><span class="sxs-lookup"><span data-stu-id="20e5d-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="20e5d-110">TPH использует одну таблицу для хранения данных для всех типов в иерархии.</span><span class="sxs-lookup"><span data-stu-id="20e5d-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="20e5d-111">Столбец дискриминатора используется для определения подходящего, каждая строка представляет.</span><span class="sxs-lookup"><span data-stu-id="20e5d-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="20e5d-112">EF Core только установить наследование, если два или более наследуемых типов явно включены в модель (см. [наследования](../inheritance.md) Дополнительные сведения).</span><span class="sxs-lookup"><span data-stu-id="20e5d-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="20e5d-113">Ниже приведен пример, показывающий простые сценарии и данные, хранящиеся в таблице реляционной базы данных, используя шаблон TPH.</span><span class="sxs-lookup"><span data-stu-id="20e5d-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="20e5d-114">*Дискриминатора* столбца определяет, какой тип *блог* хранится в каждой строке.</span><span class="sxs-lookup"><span data-stu-id="20e5d-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="20e5d-116">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="20e5d-116">Data Annotations</span></span>

<span data-ttu-id="20e5d-117">Заметки данных нельзя использовать для настройки наследования.</span><span class="sxs-lookup"><span data-stu-id="20e5d-117">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="20e5d-118">Текучий API</span><span class="sxs-lookup"><span data-stu-id="20e5d-118">Fluent API</span></span>

<span data-ttu-id="20e5d-119">Чтобы настроить имя и тип столбца дискриминатора и значения, которые используются для идентификации каждого типа в иерархии можно использовать Fluent API.</span><span class="sxs-lookup"><span data-stu-id="20e5d-119">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

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

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="20e5d-120">Настройка свойства дискриминатора</span><span class="sxs-lookup"><span data-stu-id="20e5d-120">Configuring the discriminator property</span></span>

<span data-ttu-id="20e5d-121">В приведенных выше примерах дискриминатора создается как [затемнять свойство](xref:core/modeling/shadow-properties) на это базовая сущность иерархии.</span><span class="sxs-lookup"><span data-stu-id="20e5d-121">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="20e5d-122">Так как это свойство в модели, его можно настроить так же, как и другие свойства.</span><span class="sxs-lookup"><span data-stu-id="20e5d-122">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="20e5d-123">Например, чтобы задать максимальной длины, если используется значение по умолчанию, по соглашению дискриминатора:</span><span class="sxs-lookup"><span data-stu-id="20e5d-123">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="20e5d-124">Дискриминатора также может быть сопоставлен Фактическое свойство CLR в сущности.</span><span class="sxs-lookup"><span data-stu-id="20e5d-124">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="20e5d-125">Пример:</span><span class="sxs-lookup"><span data-stu-id="20e5d-125">For example:</span></span>
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

<span data-ttu-id="20e5d-126">Объединение эти две вещи существует возможность сопоставления дискриминатора свойством реального и настройте его:</span><span class="sxs-lookup"><span data-stu-id="20e5d-126">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
