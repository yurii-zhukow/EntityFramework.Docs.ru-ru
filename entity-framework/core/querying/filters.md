---
title: Глобальные фильтры запросов — EF Core
description: Использование глобальных фильтров запросов для фильтрации результатов в Entity Framework Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: d5793760ea2e61111416284db8d5a8102dd51a41
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89616391"
---
# <a name="global-query-filters"></a><span data-ttu-id="272dd-103">Глобальные фильтры запросов</span><span class="sxs-lookup"><span data-stu-id="272dd-103">Global Query Filters</span></span>

> [!NOTE]
> <span data-ttu-id="272dd-104">Эта возможность появилась в EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="272dd-104">This feature was introduced in EF Core 2.0.</span></span>

<span data-ttu-id="272dd-105">Глобальные фильтры запросов являются предикатами запросов LINQ, которые применяются непосредственно в типах сущностей в модели метаданных (обычно в *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="272dd-105">Global query filters are LINQ query predicates applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="272dd-106">Предикат запроса — это логическое выражение, которое обычно передается в оператор запроса LINQ *Where*.</span><span class="sxs-lookup"><span data-stu-id="272dd-106">A query predicate is a boolean expression typically passed to the LINQ *Where* query operator.</span></span>  <span data-ttu-id="272dd-107">EF Core автоматически применяет такие фильтры ко всем запросам LINQ, включающим эти типы сущностей.</span><span class="sxs-lookup"><span data-stu-id="272dd-107">EF Core applies such filters automatically to any LINQ queries involving those Entity Types.</span></span>  <span data-ttu-id="272dd-108">EF Core также применяет их к типам сущностей, на которые косвенно ссылаются посредством использования Include или свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="272dd-108">EF Core also applies them to Entity Types, referenced indirectly through use of Include or navigation property.</span></span> <span data-ttu-id="272dd-109">Ниже приведены некоторые типичные способы применения этой функции.</span><span class="sxs-lookup"><span data-stu-id="272dd-109">Some common applications of this feature are:</span></span>

* <span data-ttu-id="272dd-110">**Обратимое удаление**. Тип сущности определяет свойство *IsDeleted*.</span><span class="sxs-lookup"><span data-stu-id="272dd-110">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="272dd-111">**Мультитенантность**. Тип сущности определяет свойство *TenantId*.</span><span class="sxs-lookup"><span data-stu-id="272dd-111">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="272dd-112">Пример</span><span class="sxs-lookup"><span data-stu-id="272dd-112">Example</span></span>

<span data-ttu-id="272dd-113">В следующем примере показано, как использовать глобальные фильтры запросов для реализации такого поведения запроса, как мультитенантность и обратимое удаление, в простой модели ведения блогов.</span><span class="sxs-lookup"><span data-stu-id="272dd-113">The following example shows how to use Global Query Filters to implement multi-tenancy and soft-delete query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="272dd-114">Вы можете посмотреть [пример мультитенантности](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) и [примеры использования свойств навигации](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFiltersNavigations) на GitHub.</span><span class="sxs-lookup"><span data-stu-id="272dd-114">You can view a [multi-tenancy sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) and [samples using navigations](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFiltersNavigations) on GitHub.</span></span>

<span data-ttu-id="272dd-115">Сначала определите сущности:</span><span class="sxs-lookup"><span data-stu-id="272dd-115">First, define the entities:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="272dd-116">Запишите объявление поля _tenantId_ в сущности _Blog_.</span><span class="sxs-lookup"><span data-stu-id="272dd-116">Note the declaration of a _tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="272dd-117">Это поле будет использоваться для связывания каждого экземпляра блога с конкретным клиентом.</span><span class="sxs-lookup"><span data-stu-id="272dd-117">This field will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="272dd-118">Также определено свойство _IsDeleted_ в типе сущности _Post_.</span><span class="sxs-lookup"><span data-stu-id="272dd-118">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="272dd-119">Это свойство используется, чтобы проверять, был ли экземпляр _Post_ удален "обратимо".</span><span class="sxs-lookup"><span data-stu-id="272dd-119">This property is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="272dd-120">То есть экземпляр помечен как удаленный без физического удаления базовых данных.</span><span class="sxs-lookup"><span data-stu-id="272dd-120">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="272dd-121">Затем настройте фильтры запросов в _OnModelCreating_, используя API `HasQueryFilter`.</span><span class="sxs-lookup"><span data-stu-id="272dd-121">Next, configure the query filters in _OnModelCreating_ using the `HasQueryFilter` API.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="272dd-122">Выражения предиката, поступающие в вызовы _HasQueryFilter_, будут автоматически применяться ко всем запросам LINQ для этих типов.</span><span class="sxs-lookup"><span data-stu-id="272dd-122">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="272dd-123">Обратите внимание на использование поля уровня экземпляра DbContext. `_tenantId` используется для установки текущего клиента.</span><span class="sxs-lookup"><span data-stu-id="272dd-123">Note the use of a DbContext instance level field: `_tenantId` used to set the current tenant.</span></span> <span data-ttu-id="272dd-124">Фильтры на уровне модели будут использовать значение из правильного экземпляра контекста (то есть экземпляра, выполняющего запрос).</span><span class="sxs-lookup"><span data-stu-id="272dd-124">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

> [!NOTE]
> <span data-ttu-id="272dd-125">Сейчас невозможно определить несколько фильтров запросов для одной и той же сущности. Будет применен только последний из них.</span><span class="sxs-lookup"><span data-stu-id="272dd-125">It is currently not possible to define multiple query filters on the same entity - only the last one will be applied.</span></span> <span data-ttu-id="272dd-126">Однако с помощью логического оператора _AND_ ([`&&` в C#](/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)) можно определить один фильтр с несколькими условиями.</span><span class="sxs-lookup"><span data-stu-id="272dd-126">However, you can define a single filter with multiple conditions using the logical _AND_ operator ([`&&` in C#](/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span></span>

## <a name="use-of-navigations"></a><span data-ttu-id="272dd-127">Использование свойств навигации</span><span class="sxs-lookup"><span data-stu-id="272dd-127">Use of navigations</span></span>

<span data-ttu-id="272dd-128">Свойства навигации можно также использовать при определении глобальных фильтров запросов.</span><span class="sxs-lookup"><span data-stu-id="272dd-128">You can also use navigations in defining global query filters.</span></span> <span data-ttu-id="272dd-129">Использование свойств навигации в фильтре запросов приведет к рекурсивному применению фильтров запросов.</span><span class="sxs-lookup"><span data-stu-id="272dd-129">Using navigations in query filter will cause query filters to be applied recursively.</span></span> <span data-ttu-id="272dd-130">Когда EF Core разворачивает свойства навигации, используемые в фильтрах запросов, он также применяет фильтры запросов, определенные для ссылочных сущностей.</span><span class="sxs-lookup"><span data-stu-id="272dd-130">When EF Core expands navigations used in query filters, it will also apply query filters defined on referenced entities.</span></span>

> [!NOTE]
> <span data-ttu-id="272dd-131">EF Core сейчас не обнаруживает циклы в определениях глобальных фильтров запросов, поэтому при определении циклов следует соблюдать особую осторожность.</span><span class="sxs-lookup"><span data-stu-id="272dd-131">Currently EF Core does not detect cycles in global query filter definitions, so you should be careful when defining them.</span></span> <span data-ttu-id="272dd-132">Их некорректное указание может делать циклы бесконечными при преобразовании запроса.</span><span class="sxs-lookup"><span data-stu-id="272dd-132">If specified incorrectly, cycles could lead to infinite loops during query translation.</span></span>

## <a name="accessing-entity-with-query-filter-using-required-navigation"></a><span data-ttu-id="272dd-133">Доступ к сущности с фильтром запроса с помощью обязательной навигации</span><span class="sxs-lookup"><span data-stu-id="272dd-133">Accessing entity with query filter using required navigation</span></span>

> [!CAUTION]
> <span data-ttu-id="272dd-134">Использование обязательной навигации для доступа к сущности, для которой определен глобальный фильтр запросов, может привести к непредвиденным результатам.</span><span class="sxs-lookup"><span data-stu-id="272dd-134">Using required navigation to access entity which has global query filter defined may lead to unexpected results.</span></span>

<span data-ttu-id="272dd-135">Для обязательной навигации всегда ожидается наличие связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="272dd-135">Required navigation expects the related entity to always be present.</span></span> <span data-ttu-id="272dd-136">Если исключить обязательную связанную сущность с помощью фильтра запросов, родительская сущность также будет отсутствовать в результате.</span><span class="sxs-lookup"><span data-stu-id="272dd-136">If necessary related entity is filtered out by the query filter, the parent entity wouldn't be in result either.</span></span> <span data-ttu-id="272dd-137">Так что результат может содержать меньше элементов, чем вы ожидали.</span><span class="sxs-lookup"><span data-stu-id="272dd-137">So you may get fewer elements than expected in result.</span></span>

<span data-ttu-id="272dd-138">Чтобы продемонстрировать эту проблему, можно использовать указанные выше сущности `Blog` и `Post` и следующий метод _OnModelCreating_:</span><span class="sxs-lookup"><span data-stu-id="272dd-138">To illustrate the problem, we can use the `Blog` and `Post` entities specified above and the following _OnModelCreating_ method:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>().HasMany(b => b.Posts).WithOne(p => p.Blog).IsRequired();
    modelBuilder.Entity<Blog>().HasQueryFilter(b => b.Url.Contains("fish"));
}
```

<span data-ttu-id="272dd-139">Модели можно присвоить начальные значения с помощью следующих данных:</span><span class="sxs-lookup"><span data-stu-id="272dd-139">The model can be seeded with the following data:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFiltersNavigations/Program.cs#SeedData)]

<span data-ttu-id="272dd-140">Проблема возникает при выполнении двух запросов:</span><span class="sxs-lookup"><span data-stu-id="272dd-140">The problem can be observed when executing two queries:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFiltersNavigations/Program.cs#Queries)]

<span data-ttu-id="272dd-141">При указанной выше настройке первый запрос возвращает все шесть сущностей `Post`, однако второй запрос возвращает только три сущности.</span><span class="sxs-lookup"><span data-stu-id="272dd-141">With above setup, the first query returns all 6 `Post`s, however the second query only returns 3.</span></span> <span data-ttu-id="272dd-142">Несоответствие происходит потому, что метод _Include_ во втором запросе загружает связанные сущности `Blog`.</span><span class="sxs-lookup"><span data-stu-id="272dd-142">This mismatch happens because _Include_ method in the second query loads the related `Blog` entities.</span></span> <span data-ttu-id="272dd-143">Поскольку навигация между `Blog` и `Post` является обязательной, при создании запроса EF Core использует `INNER JOIN`:</span><span class="sxs-lookup"><span data-stu-id="272dd-143">Since the navigation between `Blog` and `Post` is required, EF Core uses `INNER JOIN` when constructing the query:</span></span>

```SQL
SELECT [p].[PostId], [p].[BlogId], [p].[Content], [p].[IsDeleted], [p].[Title], [t].[BlogId], [t].[Name], [t].[Url]
FROM [Post] AS [p]
INNER JOIN (
    SELECT [b].[BlogId], [b].[Name], [b].[Url]
    FROM [Blogs] AS [b]
    WHERE CHARINDEX(N'fish', [b].[Url]) > 0
) AS [t] ON [p].[BlogId] = [t].[BlogId]
```

<span data-ttu-id="272dd-144">При использовании `INNER JOIN` фильтр исключает все сущности `Post`, чьи связанные сущности `Blog` были удалены глобальным фильтром запросов.</span><span class="sxs-lookup"><span data-stu-id="272dd-144">Use of the `INNER JOIN` filters out all `Post`s whose related `Blog`s have been removed by a global query filter.</span></span>

<span data-ttu-id="272dd-145">Для решения этой проблемы можно вместо обязательной навигации использовать необязательную.</span><span class="sxs-lookup"><span data-stu-id="272dd-145">It can be addressed by using optional navigation instead of required.</span></span>
<span data-ttu-id="272dd-146">В этом случае первый запрос остается прежним, но второй запрос будет создавать `LEFT JOIN` и возвращать шесть результатов.</span><span class="sxs-lookup"><span data-stu-id="272dd-146">This way the first query stays the same as before, however the second query will now generate `LEFT JOIN` and return 6 results.</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>().HasMany(b => b.Posts).WithOne(p => p.Blog).IsRequired(false);
    modelBuilder.Entity<Blog>().HasQueryFilter(b => b.Url.Contains("fish"));
}
```

<span data-ttu-id="272dd-147">В качестве альтернативного подхода можно указать согласованные фильтры для обеих сущностей `Blog` и `Post`.</span><span class="sxs-lookup"><span data-stu-id="272dd-147">Alternative approach is to specify consistent filters on both `Blog` and `Post` entities.</span></span>
<span data-ttu-id="272dd-148">В этом случае к `Blog` и `Post` будут применяться совпадающие фильтры.</span><span class="sxs-lookup"><span data-stu-id="272dd-148">This way matching filters are applied to both `Blog` and `Post`.</span></span> <span data-ttu-id="272dd-149">Сущности `Post`, которые могут оказаться в непредвиденном состоянии, удаляются, и оба запроса возвращают три результата.</span><span class="sxs-lookup"><span data-stu-id="272dd-149">`Post`s that could end up in unexpected state are removed and both queries return 3 results.</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>().HasMany(b => b.Posts).WithOne(p => p.Blog).IsRequired();
    modelBuilder.Entity<Blog>().HasQueryFilter(b => b.Url.Contains("fish"));
    modelBuilder.Entity<Post>().HasQueryFilter(p => p.Blog.Url.Contains("fish"));
}
```

## <a name="disabling-filters"></a><span data-ttu-id="272dd-150">Отключение фильтров</span><span class="sxs-lookup"><span data-stu-id="272dd-150">Disabling Filters</span></span>

<span data-ttu-id="272dd-151">Фильтры можно отключить для отдельных запросов LINQ, используя оператор `IgnoreQueryFilters()`.</span><span class="sxs-lookup"><span data-stu-id="272dd-151">Filters may be disabled for individual LINQ queries by using the `IgnoreQueryFilters()` operator.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="272dd-152">Ограничения</span><span class="sxs-lookup"><span data-stu-id="272dd-152">Limitations</span></span>

<span data-ttu-id="272dd-153">Глобальные фильтры запросов имеют следующие ограничения:</span><span class="sxs-lookup"><span data-stu-id="272dd-153">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="272dd-154">Фильтры можно определить только для корневого типа сущности в иерархии наследования.</span><span class="sxs-lookup"><span data-stu-id="272dd-154">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
