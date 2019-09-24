---
title: Наследование (реляционная база данных) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: a7fb19f9c86d1768967d172c006eb5d894254e0c
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196935"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="1c223-102">Наследование (реляционная база данных)</span><span class="sxs-lookup"><span data-stu-id="1c223-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="1c223-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="1c223-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="1c223-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="1c223-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="1c223-105">Наследование в модели EF используется для управления тем, как наследование в классах сущностей представлено в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1c223-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="1c223-106">В настоящее время в EF Core реализуется только шаблон «одна таблица на иерархию».</span><span class="sxs-lookup"><span data-stu-id="1c223-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="1c223-107">Другие распространенные шаблоны, такие как Table-for Type (TPT) и Table-on-Type (TPC), пока недоступны.</span><span class="sxs-lookup"><span data-stu-id="1c223-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="1c223-108">Соглашения</span><span class="sxs-lookup"><span data-stu-id="1c223-108">Conventions</span></span>

<span data-ttu-id="1c223-109">По соглашению наследование будет сопоставляться с использованием шаблона «одна таблица на иерархию».</span><span class="sxs-lookup"><span data-stu-id="1c223-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="1c223-110">Функция «подтаблица» использует одну таблицу для хранения данных всех типов в иерархии.</span><span class="sxs-lookup"><span data-stu-id="1c223-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="1c223-111">Столбец дискриминатора используется для указания типа, который представляет каждая строка.</span><span class="sxs-lookup"><span data-stu-id="1c223-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="1c223-112">EF Core будет настраивать наследование только в том случае, если в модель явно включены два или более унаследованных типа (см. Дополнительные сведения в разделе [наследование](../inheritance.md) ).</span><span class="sxs-lookup"><span data-stu-id="1c223-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="1c223-113">Ниже приведен пример простого сценария наследования и данных, хранящихся в таблице реляционной базы данных с использованием шаблона «подтаблица».</span><span class="sxs-lookup"><span data-stu-id="1c223-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="1c223-114">Столбец *дискриминатора* определяет, какой тип *блога* хранится в каждой строке.</span><span class="sxs-lookup"><span data-stu-id="1c223-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

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
> <span data-ttu-id="1c223-116">Столбцы базы данных автоматически допускают значение null при необходимости при использовании сопоставления «одна таблица».</span><span class="sxs-lookup"><span data-stu-id="1c223-116">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="1c223-117">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="1c223-117">Data Annotations</span></span>

<span data-ttu-id="1c223-118">Для настройки наследования нельзя использовать заметки к данным.</span><span class="sxs-lookup"><span data-stu-id="1c223-118">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="1c223-119">Текучий API</span><span class="sxs-lookup"><span data-stu-id="1c223-119">Fluent API</span></span>

<span data-ttu-id="1c223-120">API-интерфейс Fluent можно использовать для настройки имени и типа столбца дискриминатора и значений, которые используются для обнаружения каждого типа в иерархии.</span><span class="sxs-lookup"><span data-stu-id="1c223-120">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

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

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="1c223-121">Настройка свойства дискриминатора</span><span class="sxs-lookup"><span data-stu-id="1c223-121">Configuring the discriminator property</span></span>

<span data-ttu-id="1c223-122">В приведенных выше примерах дискриминатор создается как [свойство теневой копии](xref:core/modeling/shadow-properties) базовой сущности иерархии.</span><span class="sxs-lookup"><span data-stu-id="1c223-122">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="1c223-123">Так как это свойство в модели, его можно настроить так же, как и другие свойства.</span><span class="sxs-lookup"><span data-stu-id="1c223-123">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="1c223-124">Например, чтобы задать максимальную длину по умолчанию, используется дискриминатор по соглашению:</span><span class="sxs-lookup"><span data-stu-id="1c223-124">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="1c223-125">Дискриминатор также можно сопоставить с реальным свойством CLR в сущности.</span><span class="sxs-lookup"><span data-stu-id="1c223-125">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="1c223-126">Например:</span><span class="sxs-lookup"><span data-stu-id="1c223-126">For example:</span></span>
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

<span data-ttu-id="1c223-127">Объединяя эти две вещи, можно сопоставлять дискриминатор с реальным свойством и настраивать его:</span><span class="sxs-lookup"><span data-stu-id="1c223-127">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
