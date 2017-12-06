---
title: "Наследование (реляционной базы данных) - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: a7f697dfe2b93c7b93a2dd14945732db4f37628c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="71cca-102">Наследование (реляционной базы данных)</span><span class="sxs-lookup"><span data-stu-id="71cca-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="71cca-103">В этом разделе конфигурации применяется для реляционных баз данных в целом.</span><span class="sxs-lookup"><span data-stu-id="71cca-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="71cca-104">Методы расширения, показанный здесь будут доступны после установки поставщика реляционной базы данных (из-за общей *Microsoft.EntityFrameworkCore.Relational* пакета).</span><span class="sxs-lookup"><span data-stu-id="71cca-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="71cca-105">Наследование в модели EF используется для управления, как наследование в классах сущностей представляется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="71cca-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="71cca-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="71cca-106">Conventions</span></span>

<span data-ttu-id="71cca-107">По соглашению наследования будут сопоставлены с помощью шаблона таблица на иерархию (TPH).</span><span class="sxs-lookup"><span data-stu-id="71cca-107">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="71cca-108">TPH использует одну таблицу для хранения данных для всех типов в иерархии.</span><span class="sxs-lookup"><span data-stu-id="71cca-108">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="71cca-109">Столбец дискриминатора используется для определения подходящего, каждая строка представляет.</span><span class="sxs-lookup"><span data-stu-id="71cca-109">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="71cca-110">EF только установить наследование, если два или более наследуемых типов явно включены в модель (см. [наследования](../inheritance.md) Дополнительные сведения).</span><span class="sxs-lookup"><span data-stu-id="71cca-110">EF will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="71cca-111">Ниже приведен пример, показывающий простые сценарии и данные, хранящиеся в таблице реляционной базы данных, используя шаблон TPH.</span><span class="sxs-lookup"><span data-stu-id="71cca-111">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="71cca-112">*Дискриминатора* столбца определяет, какой тип *блог* хранится в каждой строке.</span><span class="sxs-lookup"><span data-stu-id="71cca-112">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="71cca-114">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="71cca-114">Data Annotations</span></span>

<span data-ttu-id="71cca-115">Заметки данных нельзя использовать для настройки наследования.</span><span class="sxs-lookup"><span data-stu-id="71cca-115">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="71cca-116">Fluent API</span><span class="sxs-lookup"><span data-stu-id="71cca-116">Fluent API</span></span>

<span data-ttu-id="71cca-117">Чтобы настроить имя и тип столбца дискриминатора и значения, которые используются для идентификации каждого типа в иерархии можно использовать Fluent API.</span><span class="sxs-lookup"><span data-stu-id="71cca-117">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

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
