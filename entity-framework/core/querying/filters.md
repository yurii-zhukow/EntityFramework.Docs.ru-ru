---
title: Глобальные фильтры запросов — EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 233289b0c50e15f4555b342bc654211ce04c24d3
ms.sourcegitcommit: ebfd3382fc583bc90f0da58e63d6e3382b30aa22
ms.contentlocale: ru-RU
ms.lasthandoff: 06/25/2020
ms.locfileid: "85370465"
---
# <a name="global-query-filters"></a><span data-ttu-id="96f9e-102">Глобальные фильтры запросов</span><span class="sxs-lookup"><span data-stu-id="96f9e-102">Global Query Filters</span></span>

> [!NOTE]
> <span data-ttu-id="96f9e-103">Эта возможность появилась в EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="96f9e-103">This feature was introduced in EF Core 2.0.</span></span>

<span data-ttu-id="96f9e-104">Глобальные фильтры запросов являются предикатами запросов LINQ (логическое выражение, которое обычно передается в оператор запроса LINQ *Where*), которые применяются непосредственно в типах сущностей в модели метаданных (обычно в *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="96f9e-104">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="96f9e-105">Такие фильтры автоматически применяются к любым запросам LINQ, связанным с этими типами сущностей, включая типы сущностей, указанные косвенно, например, с помощью оператора Include или ссылок на свойства прямой навигации.</span><span class="sxs-lookup"><span data-stu-id="96f9e-105">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="96f9e-106">Ниже приведены некоторые типичные способы применения этой функции.</span><span class="sxs-lookup"><span data-stu-id="96f9e-106">Some common applications of this feature are:</span></span>

* <span data-ttu-id="96f9e-107">**Обратимое удаление**. Тип сущности определяет свойство *IsDeleted*.</span><span class="sxs-lookup"><span data-stu-id="96f9e-107">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="96f9e-108">**Мультитенантность**. Тип сущности определяет свойство *TenantId*.</span><span class="sxs-lookup"><span data-stu-id="96f9e-108">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="96f9e-109">Пример</span><span class="sxs-lookup"><span data-stu-id="96f9e-109">Example</span></span>

<span data-ttu-id="96f9e-110">В следующем примере показано, как использовать глобальные фильтры запросов для реализации такого поведения запроса, как обратимое удаление и мультитенантность, в простой модели ведения блогов.</span><span class="sxs-lookup"><span data-stu-id="96f9e-110">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="96f9e-111">Вы можете посмотреть [пример мультитенантности](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) и [примеры использования свойств навигации](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFiltersNavigations) на GitHub.</span><span class="sxs-lookup"><span data-stu-id="96f9e-111">You can view a [multi-tenancy sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) and [samples using navigations](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFiltersNavigations) on GitHub.</span></span> 

<span data-ttu-id="96f9e-112">Сначала определите сущности:</span><span class="sxs-lookup"><span data-stu-id="96f9e-112">First, define the entities:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="96f9e-113">Запишите объявление поля _tenantId_ в сущности _Blog_.</span><span class="sxs-lookup"><span data-stu-id="96f9e-113">Note the declaration of a _tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="96f9e-114">Оно будет использоваться для связывания каждого экземпляра блога с конкретным клиентом.</span><span class="sxs-lookup"><span data-stu-id="96f9e-114">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="96f9e-115">Также определено свойство _IsDeleted_ в типе сущности _Post_.</span><span class="sxs-lookup"><span data-stu-id="96f9e-115">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="96f9e-116">Оно используется, чтобы проверять, был ли экземпляр _Post_ удален "обратимо".</span><span class="sxs-lookup"><span data-stu-id="96f9e-116">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="96f9e-117">То есть экземпляр помечен как удаленный без физического удаления базовых данных.</span><span class="sxs-lookup"><span data-stu-id="96f9e-117">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="96f9e-118">Затем настройте фильтры запросов в _OnModelCreating_, используя API `HasQueryFilter`.</span><span class="sxs-lookup"><span data-stu-id="96f9e-118">Next, configure the query filters in _OnModelCreating_ using the `HasQueryFilter` API.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="96f9e-119">Выражения предиката, поступающие в вызовы _HasQueryFilter_, будут автоматически применяться ко всем запросам LINQ для этих типов.</span><span class="sxs-lookup"><span data-stu-id="96f9e-119">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="96f9e-120">Обратите внимание на использование поля уровня экземпляра DbContext. `_tenantId` используется для установки текущего клиента.</span><span class="sxs-lookup"><span data-stu-id="96f9e-120">Note the use of a DbContext instance level field: `_tenantId` used to set the current tenant.</span></span> <span data-ttu-id="96f9e-121">Фильтры на уровне модели будут использовать значение из правильного экземпляра контекста (то есть экземпляра, выполняющего запрос).</span><span class="sxs-lookup"><span data-stu-id="96f9e-121">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

> [!NOTE]
> <span data-ttu-id="96f9e-122">Сейчас невозможно определить несколько фильтров запросов для одной и той же сущности. Будет применен только последний из них.</span><span class="sxs-lookup"><span data-stu-id="96f9e-122">It is currently not possible to define multiple query filters on the same entity - only the last one will be applied.</span></span> <span data-ttu-id="96f9e-123">Однако с помощью логического оператора _AND_ ([`&&` в C#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)) можно определить один фильтр с несколькими условиями.</span><span class="sxs-lookup"><span data-stu-id="96f9e-123">However, you can define a single filter with multiple conditions using the logical _AND_ operator ([`&&` in C#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span></span>

## <a name="use-of-navigations"></a><span data-ttu-id="96f9e-124">Использование свойств навигации</span><span class="sxs-lookup"><span data-stu-id="96f9e-124">Use of navigations</span></span>

<span data-ttu-id="96f9e-125">Свойства навигации можно использовать при определении глобальных фильтров запросов.</span><span class="sxs-lookup"><span data-stu-id="96f9e-125">Navigations can be used when defining global query filters.</span></span> <span data-ttu-id="96f9e-126">Они применяются рекурсивно: когда выполняется преобразование свойств навигации, используемых в фильтрах запросов, при этом также применяются фильтры, определенные для указанных по ссылке сущностей, что может привести к добавлению дополнительных свойств навигации.</span><span class="sxs-lookup"><span data-stu-id="96f9e-126">They are applied recursively - when navigations used in query filters are translated, query filters defined on referenced entities are also applied, potentially adding more navigations.</span></span>

> [!NOTE]
> <span data-ttu-id="96f9e-127">EF Core сейчас не обнаруживает циклы в определениях глобальных фильтров запросов, поэтому при определении циклов следует соблюдать особую осторожность.</span><span class="sxs-lookup"><span data-stu-id="96f9e-127">Currently EF Core does not detect cycles in global query filter definitions, so you should be careful when defining them.</span></span> <span data-ttu-id="96f9e-128">Их некорректное указание может делать циклы бесконечными при преобразовании запроса.</span><span class="sxs-lookup"><span data-stu-id="96f9e-128">If specified incorrectly, this could lead to infinite loops during query translation.</span></span>

## <a name="accessing-entity-with-query-filter-using-required-navigation"></a><span data-ttu-id="96f9e-129">Доступ к сущности с фильтром запроса с помощью обязательной навигации</span><span class="sxs-lookup"><span data-stu-id="96f9e-129">Accessing entity with query filter using required navigation</span></span>

> [!CAUTION]
> <span data-ttu-id="96f9e-130">Использование обязательной навигации для доступа к сущности, для которой определен глобальный фильтр запросов, может привести к непредвиденным результатам.</span><span class="sxs-lookup"><span data-stu-id="96f9e-130">Using required navigation to access entity which has global query filter defined may lead to unexpected results.</span></span> 

<span data-ttu-id="96f9e-131">Для обязательной навигации всегда ожидается наличие связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="96f9e-131">Required navigation expects the related entity to always be present.</span></span> <span data-ttu-id="96f9e-132">Если исключить обязательную связанную сущность с помощью фильтра запросов, родительская сущность может оказаться в непредвиденном состоянии.</span><span class="sxs-lookup"><span data-stu-id="96f9e-132">If required related entity is filtered out by the query filter, the parent entity could end up in unexpected state.</span></span> <span data-ttu-id="96f9e-133">В результате может быть возвращено меньше элементов, чем ожидалось.</span><span class="sxs-lookup"><span data-stu-id="96f9e-133">This may result in returning fewer elements than expected.</span></span> 

<span data-ttu-id="96f9e-134">Чтобы продемонстрировать эту проблему, можно использовать указанные выше сущности `Blog` и `Post` и следующий метод _OnModelCreating_:</span><span class="sxs-lookup"><span data-stu-id="96f9e-134">To illustrate the problem, we can use the `Blog` and `Post` entities specified above and the following _OnModelCreating_ method:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>().HasMany(b => b.Posts).WithOne(p => p.Blog).IsRequired();
    modelBuilder.Entity<Blog>().HasQueryFilter(b => b.Url.Contains("fish"));
}
```

<span data-ttu-id="96f9e-135">Модели можно присвоить начальные значения с помощью следующих данных:</span><span class="sxs-lookup"><span data-stu-id="96f9e-135">The model can be seeded with the following data:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFiltersNavigations/Program.cs#SeedData)]

<span data-ttu-id="96f9e-136">Проблема возникает при выполнении двух запросов:</span><span class="sxs-lookup"><span data-stu-id="96f9e-136">The problem can be observed when executing two queries:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFiltersNavigations/Program.cs#Queries)]

<span data-ttu-id="96f9e-137">При такой настройке первый запрос возвращает все шесть сущностей `Post`, однако второй запрос возвращает только три сущности.</span><span class="sxs-lookup"><span data-stu-id="96f9e-137">With this setup, the first query returns all 6 `Post`s, however the second query only returns 3.</span></span> <span data-ttu-id="96f9e-138">Это происходит потому, что метод _Include_ во втором запросе загружает связанные сущности `Blog`.</span><span class="sxs-lookup"><span data-stu-id="96f9e-138">This happens because _Include_ method in the second query loads the related `Blog` entities.</span></span> <span data-ttu-id="96f9e-139">Поскольку навигация между `Blog` и `Post` является обязательной, при создании запроса EF Core использует `INNER JOIN`:</span><span class="sxs-lookup"><span data-stu-id="96f9e-139">Since the navigation between `Blog` and `Post` is required, EF Core uses `INNER JOIN` when constructing the query:</span></span>

```SQL
SELECT [p].[PostId], [p].[BlogId], [p].[Content], [p].[IsDeleted], [p].[Title], [t].[BlogId], [t].[Name], [t].[Url]
FROM [Post] AS [p]
INNER JOIN (
    SELECT [b].[BlogId], [b].[Name], [b].[Url]
    FROM [Blogs] AS [b]
    WHERE CHARINDEX(N'fish', [b].[Url]) > 0
) AS [t] ON [p].[BlogId] = [t].[BlogId]
```

<span data-ttu-id="96f9e-140">При использовании `INNER JOIN` фильтр исключает все сущности `Post`, чьи связанные сущности `Blog` были удалены глобальным фильтром запросов.</span><span class="sxs-lookup"><span data-stu-id="96f9e-140">Use of the `INNER JOIN` filters out all `Post`s whose related `Blog`s have been removed by a global query filter.</span></span> 

<span data-ttu-id="96f9e-141">Для решения этой проблемы можно вместо обязательной навигации использовать необязательную.</span><span class="sxs-lookup"><span data-stu-id="96f9e-141">It can be addressed by using optional navigation instead of required.</span></span> <span data-ttu-id="96f9e-142">В этом случае первый запрос остается прежним, но второй запрос будет создавать `LEFT JOIN` и возвращать шесть результатов.</span><span class="sxs-lookup"><span data-stu-id="96f9e-142">This way the first query stays the same as before, however the second query will now generate `LEFT JOIN` and return 6 results.</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>().HasMany(b => b.Posts).WithOne(p => p.Blog).IsRequired(false);
    modelBuilder.Entity<Blog>().HasQueryFilter(b => b.Url.Contains("fish"));
}
```

<span data-ttu-id="96f9e-143">В качестве альтернативного подхода можно указать согласованные фильтры для обеих сущностей `Blog` и `Post`.</span><span class="sxs-lookup"><span data-stu-id="96f9e-143">Alternative approach is to specify consistent filters on both `Blog` and `Post` entities.</span></span>
<span data-ttu-id="96f9e-144">В этом случае к `Blog` и `Post` будут применяться совпадающие фильтры.</span><span class="sxs-lookup"><span data-stu-id="96f9e-144">This way matching filters are applied to both `Blog` and `Post`.</span></span> <span data-ttu-id="96f9e-145">Сущности `Post`, которые могут оказаться в непредвиденном состоянии, удаляются, и оба запроса возвращают три результата.</span><span class="sxs-lookup"><span data-stu-id="96f9e-145">`Post`s that could end up in unexpected state are removed and both queries return 3 results.</span></span> 

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>().HasMany(b => b.Posts).WithOne(p => p.Blog).IsRequired();
    modelBuilder.Entity<Blog>().HasQueryFilter(b => b.Url.Contains("fish"));
    modelBuilder.Entity<Post>().HasQueryFilter(p => p.Blog.Url.Contains("fish"));
}
```

## <a name="disabling-filters"></a><span data-ttu-id="96f9e-146">Отключение фильтров</span><span class="sxs-lookup"><span data-stu-id="96f9e-146">Disabling Filters</span></span>

<span data-ttu-id="96f9e-147">Фильтры можно отключить для отдельных запросов LINQ, используя оператор `IgnoreQueryFilters()`.</span><span class="sxs-lookup"><span data-stu-id="96f9e-147">Filters may be disabled for individual LINQ queries by using the `IgnoreQueryFilters()` operator.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="96f9e-148">Ограничения</span><span class="sxs-lookup"><span data-stu-id="96f9e-148">Limitations</span></span>

<span data-ttu-id="96f9e-149">Глобальные фильтры запросов имеют следующие ограничения:</span><span class="sxs-lookup"><span data-stu-id="96f9e-149">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="96f9e-150">Фильтры можно определить только для корневого типа сущности в иерархии наследования.</span><span class="sxs-lookup"><span data-stu-id="96f9e-150">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
