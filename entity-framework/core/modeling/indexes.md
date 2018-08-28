---
title: Индексы — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 87fe893243377e3ab83d419ae9bedf813ca50c3f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995484"
---
# <a name="indexes"></a><span data-ttu-id="694bc-102">Индексы</span><span class="sxs-lookup"><span data-stu-id="694bc-102">Indexes</span></span>

<span data-ttu-id="694bc-103">Индексы являются понятием распространенных в многих хранилищах данных.</span><span class="sxs-lookup"><span data-stu-id="694bc-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="694bc-104">Хотя их реализации в хранилище данных могут отличаться, они используются превратить уточняющие запросы, на основании столбца (или набор столбцов), более эффективным.</span><span class="sxs-lookup"><span data-stu-id="694bc-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="694bc-105">Соглашения</span><span class="sxs-lookup"><span data-stu-id="694bc-105">Conventions</span></span>

<span data-ttu-id="694bc-106">По соглашению индекс создается в каждом свойство (или набор свойств), которые используются в качестве внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="694bc-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="694bc-107">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="694bc-107">Data Annotations</span></span>

<span data-ttu-id="694bc-108">Индексы могут не создаваться с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="694bc-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="694bc-109">Текучий API</span><span class="sxs-lookup"><span data-stu-id="694bc-109">Fluent API</span></span>

<span data-ttu-id="694bc-110">Fluent API можно использовать для указания индекса по одному свойству.</span><span class="sxs-lookup"><span data-stu-id="694bc-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="694bc-111">По умолчанию индексы не уникальны.</span><span class="sxs-lookup"><span data-stu-id="694bc-111">By default, indexes are non-unique.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Index.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="694bc-112">Можно также указать что индекса должны быть уникальными, это означает, что две сущности не может иметь же значения для заданного свойства.</span><span class="sxs-lookup"><span data-stu-id="694bc-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="694bc-113">Можно также указать индекс по нескольким столбцам.</span><span class="sxs-lookup"><span data-stu-id="694bc-113">You can also specify an index over more than one column.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .HasIndex(p => new { p.FirstName, p.LastName });
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

> [!TIP]  
> <span data-ttu-id="694bc-114">Есть только один индекс отдельный набор свойств.</span><span class="sxs-lookup"><span data-stu-id="694bc-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="694bc-115">Если вы используете Fluent API для настройки индекса на набор свойств, которые уже есть индекс, определенный, по соглашению или предыдущей конфигурации, затем подлежащий изменению определения этого индекса.</span><span class="sxs-lookup"><span data-stu-id="694bc-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="694bc-116">Это полезно, если вы хотите продолжить настройку индекса, который был создан в соответствии с соглашением.</span><span class="sxs-lookup"><span data-stu-id="694bc-116">This is useful if you want to further configure an index that was created by convention.</span></span>
