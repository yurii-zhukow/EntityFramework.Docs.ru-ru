---
title: Индексы — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: b6f11401b69bd8e8795f6b22e5392ba16fc9ba2e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197244"
---
# <a name="indexes"></a><span data-ttu-id="67b64-102">Индексы</span><span class="sxs-lookup"><span data-stu-id="67b64-102">Indexes</span></span>

<span data-ttu-id="67b64-103">Индексы являются распространенной концепцией для многих хранилищ данных.</span><span class="sxs-lookup"><span data-stu-id="67b64-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="67b64-104">Хотя их реализация в хранилище данных может различаться, они используются для выполнения уточняющих запросов, основанных на столбце (или наборе столбцов), которые более эффективны.</span><span class="sxs-lookup"><span data-stu-id="67b64-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="67b64-105">Соглашения</span><span class="sxs-lookup"><span data-stu-id="67b64-105">Conventions</span></span>

<span data-ttu-id="67b64-106">По соглашению индекс создается в каждом свойстве (или наборе свойств), используемом в качестве внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="67b64-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="67b64-107">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="67b64-107">Data Annotations</span></span>

<span data-ttu-id="67b64-108">Индексы нельзя создавать с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="67b64-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="67b64-109">Текучий API</span><span class="sxs-lookup"><span data-stu-id="67b64-109">Fluent API</span></span>

<span data-ttu-id="67b64-110">Вы можете использовать API Fluent, чтобы указать индекс для одного свойства.</span><span class="sxs-lookup"><span data-stu-id="67b64-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="67b64-111">По умолчанию индексы не являются уникальными.</span><span class="sxs-lookup"><span data-stu-id="67b64-111">By default, indexes are non-unique.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Index.cs?highlight=7,8)] -->
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

<span data-ttu-id="67b64-112">Можно также указать, что индекс должен быть уникальным, то есть ни одна из двух сущностей не может иметь одинаковые значения для заданных свойств.</span><span class="sxs-lookup"><span data-stu-id="67b64-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="67b64-113">Можно также указать индекс более чем для одного столбца.</span><span class="sxs-lookup"><span data-stu-id="67b64-113">You can also specify an index over more than one column.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexComposite.cs?highlight=7,8)] -->
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
> <span data-ttu-id="67b64-114">Для каждого отдельного набора свойств существует только один индекс.</span><span class="sxs-lookup"><span data-stu-id="67b64-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="67b64-115">Если вы используете API Fluent для настройки индекса для набора свойств, в котором уже определен индекс, либо в соответствии с соглашением, либо с помощью предыдущей конфигурации, вы измените определение этого индекса.</span><span class="sxs-lookup"><span data-stu-id="67b64-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="67b64-116">Это полезно, если необходимо дополнительно настроить индекс, созданный по соглашению.</span><span class="sxs-lookup"><span data-stu-id="67b64-116">This is useful if you want to further configure an index that was created by convention.</span></span>
