---
title: Безотложная загрузка связанных данных — EF Core
description: Безотложная загрузка связанных данных с помощью Entity Framework Core
author: roji
ms.date: 9/8/2020
uid: core/querying/related-data/eager
ms.openlocfilehash: bd9c9045c1c2707d69ee4070bea59ad8066789f3
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94430109"
---
# <a name="eager-loading-of-related-data"></a><span data-ttu-id="4ab0e-103">Безотложная загрузка связанных данных</span><span class="sxs-lookup"><span data-stu-id="4ab0e-103">Eager Loading of Related Data</span></span>

## <a name="eager-loading"></a><span data-ttu-id="4ab0e-104">Безотложная загрузка</span><span class="sxs-lookup"><span data-stu-id="4ab0e-104">Eager loading</span></span>

<span data-ttu-id="4ab0e-105">Вы можете использовать метод `Include`, чтобы указать связанные данные, которые будут включены в результаты запроса.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-105">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="4ab0e-106">В следующем примере блоги, возвращенные в результатах, будут иметь свойство `Posts`, заполненное соответствующими записями.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-106">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#SingleInclude)]

> [!TIP]
> <span data-ttu-id="4ab0e-107">EF Core автоматически исправляет свойства навигации для других сущностей, которые были ранее загружены в экземпляр контекста.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-107">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="4ab0e-108">Даже если данные для свойства навигации не включены явно, свойство все равно можно заполнить при условии, что ранее были загружены некоторые или все связанные сущности.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-108">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

<span data-ttu-id="4ab0e-109">Связанные данные из нескольких связей можно включить в один запрос.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-109">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#MultipleIncludes)]

> [!CAUTION]
> <span data-ttu-id="4ab0e-110">Безотложная загрузка навигации по коллекции в одном запросе может вызвать проблемы с производительностью.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-110">Eager loading a collection navigation in a single query may cause performance issues.</span></span> <span data-ttu-id="4ab0e-111">Дополнительные сведения см. в статье [Отдельные и разделенные запросы](xref:core/querying/single-split-queries).</span><span class="sxs-lookup"><span data-stu-id="4ab0e-111">For more information, see [Single vs. split queries](xref:core/querying/single-split-queries).</span></span>

## <a name="including-multiple-levels"></a><span data-ttu-id="4ab0e-112">Включение нескольких уровней</span><span class="sxs-lookup"><span data-stu-id="4ab0e-112">Including multiple levels</span></span>

<span data-ttu-id="4ab0e-113">Вы можете детализировать отношения, чтобы включить несколько уровней связанных данных, используя метод `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-113">You can drill down through relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="4ab0e-114">В следующем примере загружаются все блоги, связанные с ними записи и автор каждой записи.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-114">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#SingleThenInclude)]

<span data-ttu-id="4ab0e-115">Вы можете связать в цепочку несколько вызовов `ThenInclude`, чтобы продолжить включение уровней связанных данных.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-115">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#MultipleThenIncludes)]

<span data-ttu-id="4ab0e-116">Вы можете объединить все эти вызовы, чтобы включить связанные данные из нескольких уровней и нескольких корней в один и тот же запрос.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-116">You can combine all of the calls to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#IncludeTree)]

<span data-ttu-id="4ab0e-117">Может потребоваться включить несколько связанных сущностей для одной включенной сущности.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-117">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="4ab0e-118">Например, при запросе `Blogs` вы включаете `Posts`, а затем хотите включить `Author` и `Tags` из `Posts`.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-118">For example, when querying `Blogs`, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="4ab0e-119">Для включения обеих сущностей вам нужно указать каждый путь включения, начиная с корня.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-119">To include both, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="4ab0e-120">Например, `Blog -> Posts -> Author` и `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-120">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="4ab0e-121">Это не означает, что вы получите избыточные соединения, в большинстве случаев EF будет комбинировать соединения при создании SQL.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-121">It doesn't mean you'll get redundant joins; in most cases, EF will combine the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#MultipleLeafIncludes)]

## <a name="filtered-include"></a><span data-ttu-id="4ab0e-122">Включение с фильтрацией</span><span class="sxs-lookup"><span data-stu-id="4ab0e-122">Filtered include</span></span>

> [!NOTE]
> <span data-ttu-id="4ab0e-123">Эта возможность появилась в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-123">This feature is introduced in EF Core 5.0.</span></span>

<span data-ttu-id="4ab0e-124">При применении метода Include для загрузки связанных данных можно добавить определенные перечислимые операции к включенной навигации по коллекции, что позволяет фильтровать и сортировать результаты.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-124">When applying Include to load related data, you can add certain enumerable operations to the included collection navigation, which allows for filtering and sorting of the results.</span></span>

<span data-ttu-id="4ab0e-125">Поддерживаются операции: `Where`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `Skip` и `Take`.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-125">Supported operations are: `Where`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `Skip`, and `Take`.</span></span>

<span data-ttu-id="4ab0e-126">Такие операции должны применяться к навигации коллекции в лямбда-выражении, переданном в метод Include, как показано в примере ниже:</span><span class="sxs-lookup"><span data-stu-id="4ab0e-126">Such operations should be applied on the collection navigation in the lambda passed to the Include method, as shown in example below:</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#FilteredInclude)]

<span data-ttu-id="4ab0e-127">Каждая включаемая навигация позволяет выполнять только один уникальный набор операций фильтра.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-127">Each included navigation allows only one unique set of filter operations.</span></span> <span data-ttu-id="4ab0e-128">В тех случаях, когда для данной навигации по коллекции применяются несколько операций Include (`blog.Posts` в приведенных ниже примерах), операции фильтрации можно указывать только в одном из них:</span><span class="sxs-lookup"><span data-stu-id="4ab0e-128">In cases where multiple Include operations are applied for a given collection navigation (`blog.Posts` in the examples below), filter operations can only be specified on one of them:</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#MultipleLeafIncludesFiltered1)]

<span data-ttu-id="4ab0e-129">Вместо этого можно применить идентичные операции для каждой навигации, которая включается несколько раз:</span><span class="sxs-lookup"><span data-stu-id="4ab0e-129">Instead, identical operations can be applied for each navigation that is included multiple times:</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#MultipleLeafIncludesFiltered2)]

> [!CAUTION]
> <span data-ttu-id="4ab0e-130">При отслеживании запросов результаты включения с фильтрацией могут быть неожиданными из-за [исправления навигации](xref:core/querying/tracking).</span><span class="sxs-lookup"><span data-stu-id="4ab0e-130">In case of tracking queries, results of Filtered Include may be unexpected due to [navigation fixup](xref:core/querying/tracking).</span></span> <span data-ttu-id="4ab0e-131">Все связанные сущности, которые были запрошены ранее и сохранены в средстве отслеживания изменений, будут представлены в результатах запроса включения с фильтрацией, даже если они не соответствуют требованиям фильтра.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-131">All relevant entities that have been queried for previously and have been stored in the Change Tracker will be present in the results of Filtered Include query, even if they don't meet the requirements of the filter.</span></span> <span data-ttu-id="4ab0e-132">При использовании в таких ситуациях включения с фильтрацией рассмотрите возможность работы с запросами `NoTracking` или повторного создания DbContext.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-132">Consider using `NoTracking` queries or re-create the DbContext when using Filtered Include in those situations.</span></span>

<span data-ttu-id="4ab0e-133">Пример.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-133">Example:</span></span>

```csharp
var orders = context.Orders.Where(o => o.Id > 1000).ToList();

// customer entities will have references to all orders where Id > 1000, rather than > 5000
var filtered = context.Customers.Include(c => c.Orders.Where(o => o.Id > 5000)).ToList();
```

> [!NOTE]
> <span data-ttu-id="4ab0e-134">Если используются отслеживаемые запросы, будет загружена та навигация, для которой применено включение с фильтрацией.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-134">In case of tracking queries, the navigation on which filtered include was applied is considered to be loaded.</span></span> <span data-ttu-id="4ab0e-135">Это означает, что EF Core не будет пытаться повторно загрузить значения с помощью [явной загрузки](xref:core/querying/related-data/explicit) или [отложенной загрузки](xref:core/querying/related-data/lazy), даже если некоторые элементы будут отсутствовать.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-135">This means that EF Core will not attempt to re-load it's values using [explicit loading](xref:core/querying/related-data/explicit) or [lazy loading](xref:core/querying/related-data/lazy), even though some elements could still be missing.</span></span>

## <a name="include-on-derived-types"></a><span data-ttu-id="4ab0e-136">Использование метода Include с производными типами</span><span class="sxs-lookup"><span data-stu-id="4ab0e-136">Include on derived types</span></span>

<span data-ttu-id="4ab0e-137">Вы можете включать связанные данные из навигации, определенной только с производным типом, используя `Include` и `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-137">You can include related data from navigation defined only on a derived type using `Include` and `ThenInclude`.</span></span>

<span data-ttu-id="4ab0e-138">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="4ab0e-138">Given the following model:</span></span>

```csharp
public class SchoolContext : DbContext
{
    public DbSet<Person> People { get; set; }
    public DbSet<School> Schools { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<School>().HasMany(s => s.Students).WithOne(s => s.School);
    }
}

public class Person
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public class Student : Person
{
    public School School { get; set; }
}

public class School
{
    public int Id { get; set; }
    public string Name { get; set; }

    public List<Student> Students { get; set; }
}
```

<span data-ttu-id="4ab0e-139">Содержимое навигации `School` всех людей, которые являются учениками, может быть активно загружено с помощью нескольких шаблонов:</span><span class="sxs-lookup"><span data-stu-id="4ab0e-139">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

* <span data-ttu-id="4ab0e-140">С помощью приведения.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-140">using cast</span></span>

  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

* <span data-ttu-id="4ab0e-141">С помощью оператора `as`.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-141">using `as` operator</span></span>

  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

* <span data-ttu-id="4ab0e-142">С помощью перегрузки `Include`, принимающий параметр типа `string`.</span><span class="sxs-lookup"><span data-stu-id="4ab0e-142">using overload of `Include` that takes parameter of type `string`</span></span>

  ```csharp
  context.People.Include("School").ToList()
  ```
