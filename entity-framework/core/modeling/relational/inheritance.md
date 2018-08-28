---
title: Наследование (реляционная база данных) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 019893ec8268ef9e59d581799a13d63610c80616
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996326"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="d29b8-102">Наследование (реляционная база данных)</span><span class="sxs-lookup"><span data-stu-id="d29b8-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="d29b8-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="d29b8-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="d29b8-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="d29b8-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="d29b8-105">Наследование в модели EF используется для управления представление наследования в классы сущностей в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d29b8-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="d29b8-106">В настоящее время только шаблон таблица на иерархию (TPH) реализована в EF Core.</span><span class="sxs-lookup"><span data-stu-id="d29b8-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="d29b8-107">Другие распространенные шаблоны, такие как одна таблица на тип (TPT) и таблица на конкретный тип (TPC) еще не доступны.</span><span class="sxs-lookup"><span data-stu-id="d29b8-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="d29b8-108">Соглашения</span><span class="sxs-lookup"><span data-stu-id="d29b8-108">Conventions</span></span>

<span data-ttu-id="d29b8-109">По соглашению наследования будут сопоставлены с помощью шаблона одна таблица на иерархию (TPH).</span><span class="sxs-lookup"><span data-stu-id="d29b8-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="d29b8-110">TPH использует одну таблицу для хранения данных для всех типов в иерархии.</span><span class="sxs-lookup"><span data-stu-id="d29b8-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="d29b8-111">Столбец дискриминатора используется для определения подходящего, каждая строка представляет.</span><span class="sxs-lookup"><span data-stu-id="d29b8-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="d29b8-112">Если два или более наследуемых типов явно включены в модель EF Core только процедура позволяет настроить наследование (см. в разделе [наследования](../inheritance.md) подробности).</span><span class="sxs-lookup"><span data-stu-id="d29b8-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="d29b8-113">Ниже приведен пример, показывающий простые сценарии и данные, хранящиеся в таблице реляционной базы данных, используя шаблон TPH.</span><span class="sxs-lookup"><span data-stu-id="d29b8-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="d29b8-114">*Дискриминатора* столбца определяет, какой тип *блог* хранится в каждой строке.</span><span class="sxs-lookup"><span data-stu-id="d29b8-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="d29b8-116">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="d29b8-116">Data Annotations</span></span>

<span data-ttu-id="d29b8-117">Заметки к данным нельзя использовать для настройки наследования.</span><span class="sxs-lookup"><span data-stu-id="d29b8-117">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="d29b8-118">Текучий API</span><span class="sxs-lookup"><span data-stu-id="d29b8-118">Fluent API</span></span>

<span data-ttu-id="d29b8-119">Чтобы настроить имя и тип столбца дискриминатора и значений, которые используются для идентификации каждого типа в иерархии можно использовать Fluent API.</span><span class="sxs-lookup"><span data-stu-id="d29b8-119">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

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

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="d29b8-120">Настройка свойства дискриминатора</span><span class="sxs-lookup"><span data-stu-id="d29b8-120">Configuring the discriminator property</span></span>

<span data-ttu-id="d29b8-121">В приведенных выше примерах дискриминатора создается как [затемнения свойства](xref:core/modeling/shadow-properties) на базовая сущность иерархии.</span><span class="sxs-lookup"><span data-stu-id="d29b8-121">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="d29b8-122">Так как это свойство в модели, его можно настроить так же, как и другие свойства.</span><span class="sxs-lookup"><span data-stu-id="d29b8-122">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="d29b8-123">Например, чтобы задать Максимальная длина, когда используется по умолчанию, по соглашению дискриминатора:</span><span class="sxs-lookup"><span data-stu-id="d29b8-123">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="d29b8-124">Дискриминатора может быть сопоставлено Фактическое свойство CLR в сущности.</span><span class="sxs-lookup"><span data-stu-id="d29b8-124">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="d29b8-125">Пример:</span><span class="sxs-lookup"><span data-stu-id="d29b8-125">For example:</span></span>
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

<span data-ttu-id="d29b8-126">Объединяя эти две вещи можно сопоставить настоящее Свойство дискриминатора и настройте его:</span><span class="sxs-lookup"><span data-stu-id="d29b8-126">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
