---
title: Глобальные фильтры запросов — EF Core
description: Использование глобальных фильтров запросов для фильтрации результатов в Entity Framework Core
author: maumar
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 6436f9f8e2e09d44ef9528fd2022720d40095fe0
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94430135"
---
# <a name="global-query-filters"></a><span data-ttu-id="766f9-103">Глобальные фильтры запросов</span><span class="sxs-lookup"><span data-stu-id="766f9-103">Global Query Filters</span></span>

<span data-ttu-id="766f9-104">Глобальные фильтры запросов являются предикатами запросов LINQ, которые применяются непосредственно в типах сущностей в модели метаданных (обычно в `OnModelCreating`).</span><span class="sxs-lookup"><span data-stu-id="766f9-104">Global query filters are LINQ query predicates applied to Entity Types in the metadata model (usually in `OnModelCreating`).</span></span> <span data-ttu-id="766f9-105">Предикат запроса — это логическое выражение, которое обычно передается в оператор запроса LINQ `Where`.</span><span class="sxs-lookup"><span data-stu-id="766f9-105">A query predicate is a boolean expression typically passed to the LINQ `Where` query operator.</span></span>  <span data-ttu-id="766f9-106">EF Core автоматически применяет такие фильтры ко всем запросам LINQ, включающим эти типы сущностей.</span><span class="sxs-lookup"><span data-stu-id="766f9-106">EF Core applies such filters automatically to any LINQ queries involving those Entity Types.</span></span>  <span data-ttu-id="766f9-107">EF Core также применяет их к типам сущностей, на которые косвенно ссылаются посредством использования Include или свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="766f9-107">EF Core also applies them to Entity Types, referenced indirectly through use of Include or navigation property.</span></span> <span data-ttu-id="766f9-108">Ниже приведены некоторые типичные способы применения этой функции.</span><span class="sxs-lookup"><span data-stu-id="766f9-108">Some common applications of this feature are:</span></span>

* <span data-ttu-id="766f9-109">**Обратимое удаление**. Тип сущности определяет свойство `IsDeleted`.</span><span class="sxs-lookup"><span data-stu-id="766f9-109">**Soft delete** - An Entity Type defines an `IsDeleted` property.</span></span>
* <span data-ttu-id="766f9-110">**Мультитенантность**. Тип сущности определяет свойство `TenantId`.</span><span class="sxs-lookup"><span data-stu-id="766f9-110">**Multi-tenancy** - An Entity Type defines a `TenantId` property.</span></span>

## <a name="example"></a><span data-ttu-id="766f9-111">Пример</span><span class="sxs-lookup"><span data-stu-id="766f9-111">Example</span></span>

<span data-ttu-id="766f9-112">В следующем примере показано, как использовать глобальные фильтры запросов для реализации такого поведения запроса, как мультитенантность и обратимое удаление, в простой модели ведения блогов.</span><span class="sxs-lookup"><span data-stu-id="766f9-112">The following example shows how to use Global Query Filters to implement multi-tenancy and soft-delete query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="766f9-113">Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/QueryFilters) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="766f9-113">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="766f9-114">Сначала определите сущности:</span><span class="sxs-lookup"><span data-stu-id="766f9-114">First, define the entities:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/QueryFilters/Entities.cs#Entities)]

<span data-ttu-id="766f9-115">Запишите объявление поля `_tenantId` в сущности `Blog`.</span><span class="sxs-lookup"><span data-stu-id="766f9-115">Note the declaration of a `_tenantId` field on the `Blog` entity.</span></span> <span data-ttu-id="766f9-116">Это поле будет использоваться для связывания каждого экземпляра блога с конкретным клиентом.</span><span class="sxs-lookup"><span data-stu-id="766f9-116">This field will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="766f9-117">Также определено свойство `IsDeleted` в типе сущности `Post`.</span><span class="sxs-lookup"><span data-stu-id="766f9-117">Also defined is an `IsDeleted` property on the `Post` entity type.</span></span> <span data-ttu-id="766f9-118">Это свойство используется, чтобы проверять, был ли экземпляр записи удален обратимо.</span><span class="sxs-lookup"><span data-stu-id="766f9-118">This property is used to keep track of whether a post instance has been "soft-deleted".</span></span> <span data-ttu-id="766f9-119">То есть экземпляр помечен как удаленный без физического удаления базовых данных.</span><span class="sxs-lookup"><span data-stu-id="766f9-119">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="766f9-120">Затем настройте фильтры запросов в `OnModelCreating`, используя API `HasQueryFilter`.</span><span class="sxs-lookup"><span data-stu-id="766f9-120">Next, configure the query filters in `OnModelCreating` using the `HasQueryFilter` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/QueryFilters/BloggingContext.cs#FilterConfiguration)]

<span data-ttu-id="766f9-121">Выражения предиката, поступающие в вызовы `HasQueryFilter`, будут автоматически применяться ко всем запросам LINQ для этих типов.</span><span class="sxs-lookup"><span data-stu-id="766f9-121">The predicate expressions passed to the `HasQueryFilter` calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="766f9-122">Обратите внимание на использование поля уровня экземпляра DbContext. `_tenantId` используется для установки текущего клиента.</span><span class="sxs-lookup"><span data-stu-id="766f9-122">Note the use of a DbContext instance level field: `_tenantId` used to set the current tenant.</span></span> <span data-ttu-id="766f9-123">Фильтры на уровне модели будут использовать значение из правильного экземпляра контекста (то есть экземпляра, выполняющего запрос).</span><span class="sxs-lookup"><span data-stu-id="766f9-123">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

> [!NOTE]
> <span data-ttu-id="766f9-124">Сейчас невозможно определить несколько фильтров запросов для одной и той же сущности. Будет применен только последний из них.</span><span class="sxs-lookup"><span data-stu-id="766f9-124">It is currently not possible to define multiple query filters on the same entity - only the last one will be applied.</span></span> <span data-ttu-id="766f9-125">Однако с помощью логического оператора `AND` ([`&&` в C#](/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)) можно определить один фильтр с несколькими условиями.</span><span class="sxs-lookup"><span data-stu-id="766f9-125">However, you can define a single filter with multiple conditions using the logical `AND` operator ([`&&` in C#](/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span></span>

## <a name="use-of-navigations"></a><span data-ttu-id="766f9-126">Использование свойств навигации</span><span class="sxs-lookup"><span data-stu-id="766f9-126">Use of navigations</span></span>

<span data-ttu-id="766f9-127">Свойства навигации можно также использовать при определении глобальных фильтров запросов.</span><span class="sxs-lookup"><span data-stu-id="766f9-127">You can also use navigations in defining global query filters.</span></span> <span data-ttu-id="766f9-128">Использование свойств навигации в фильтре запросов приведет к рекурсивному применению фильтров запросов.</span><span class="sxs-lookup"><span data-stu-id="766f9-128">Using navigations in query filter will cause query filters to be applied recursively.</span></span> <span data-ttu-id="766f9-129">Когда EF Core разворачивает свойства навигации, используемые в фильтрах запросов, он также применяет фильтры запросов, определенные для ссылочных сущностей.</span><span class="sxs-lookup"><span data-stu-id="766f9-129">When EF Core expands navigations used in query filters, it will also apply query filters defined on referenced entities.</span></span>

<span data-ttu-id="766f9-130">Чтобы понять это на примере, настройте фильтры запросов в `OnModelCreating` следующим образом: [!code-csharp[Main](../../../samples/core/Querying/QueryFilters/FilteredBloggingContextRequired.cs#NavigationInFilter)]</span><span class="sxs-lookup"><span data-stu-id="766f9-130">To illustrate this configure query filters in `OnModelCreating` in the following way: [!code-csharp[Main](../../../samples/core/Querying/QueryFilters/FilteredBloggingContextRequired.cs#NavigationInFilter)]</span></span>

<span data-ttu-id="766f9-131">Затем запросите все сущности `Blog`: [!code-csharp[Main](../../../samples/core/Querying/QueryFilters/FilteredBloggingContextRequired.cs#QueriesNavigation)]</span><span class="sxs-lookup"><span data-stu-id="766f9-131">Next, query for all `Blog` entities: [!code-csharp[Main](../../../samples/core/Querying/QueryFilters/FilteredBloggingContextRequired.cs#QueriesNavigation)]</span></span>

<span data-ttu-id="766f9-132">Этот запрос возвращает следующий SQL-код, который применяет фильтры, определенные для сущностей `Blog` и `Post`.</span><span class="sxs-lookup"><span data-stu-id="766f9-132">This query produces the following SQL, which applies query filters defined for both `Blog` and `Post` entities:</span></span>

```sql
SELECT [b].[BlogId], [b].[Name], [b].[Url]
FROM [Blogs] AS [b]
WHERE (
    SELECT COUNT(*)
    FROM [Posts] AS [p]
    WHERE ([p].[Title] LIKE N'%fish%') AND ([b].[BlogId] = [p].[BlogId])) > 0
```

> [!NOTE]
> <span data-ttu-id="766f9-133">EF Core сейчас не обнаруживает циклы в определениях глобальных фильтров запросов, поэтому при определении циклов следует соблюдать особую осторожность.</span><span class="sxs-lookup"><span data-stu-id="766f9-133">Currently EF Core does not detect cycles in global query filter definitions, so you should be careful when defining them.</span></span> <span data-ttu-id="766f9-134">Их некорректное указание может делать циклы бесконечными при преобразовании запроса.</span><span class="sxs-lookup"><span data-stu-id="766f9-134">If specified incorrectly, cycles could lead to infinite loops during query translation.</span></span>

## <a name="accessing-entity-with-query-filter-using-required-navigation"></a><span data-ttu-id="766f9-135">Доступ к сущности с фильтром запроса с помощью обязательной навигации</span><span class="sxs-lookup"><span data-stu-id="766f9-135">Accessing entity with query filter using required navigation</span></span>

> [!CAUTION]
> <span data-ttu-id="766f9-136">Использование обязательной навигации для доступа к сущности, для которой определен глобальный фильтр запросов, может привести к непредвиденным результатам.</span><span class="sxs-lookup"><span data-stu-id="766f9-136">Using required navigation to access entity which has global query filter defined may lead to unexpected results.</span></span>

<span data-ttu-id="766f9-137">Для обязательной навигации всегда ожидается наличие связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="766f9-137">Required navigation expects the related entity to always be present.</span></span> <span data-ttu-id="766f9-138">Если исключить обязательную связанную сущность с помощью фильтра запросов, родительская сущность также будет отсутствовать в результате.</span><span class="sxs-lookup"><span data-stu-id="766f9-138">If necessary related entity is filtered out by the query filter, the parent entity wouldn't be in result either.</span></span> <span data-ttu-id="766f9-139">Так что результат может содержать меньше элементов, чем вы ожидали.</span><span class="sxs-lookup"><span data-stu-id="766f9-139">So you may get fewer elements than expected in result.</span></span>

<span data-ttu-id="766f9-140">Чтобы продемонстрировать эту проблему, можно использовать указанные выше сущности `Blog` и `Post` и следующий метод `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="766f9-140">To illustrate the problem, we can use the `Blog` and `Post` entities specified above and the following `OnModelCreating` method:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/QueryFilters/FilteredBloggingContextRequired.cs#IncorrectFilter)]

<span data-ttu-id="766f9-141">Модели можно присвоить начальные значения с помощью следующих данных:</span><span class="sxs-lookup"><span data-stu-id="766f9-141">The model can be seeded with the following data:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/QueryFilters/Program.cs#SeedData)]

<span data-ttu-id="766f9-142">Проблема возникает при выполнении двух запросов:</span><span class="sxs-lookup"><span data-stu-id="766f9-142">The problem can be observed when executing two queries:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/QueryFilters/Program.cs#Queries)]

<span data-ttu-id="766f9-143">При указанной выше настройке первый запрос возвращает все шесть сущностей `Post`, однако второй запрос возвращает только три сущности.</span><span class="sxs-lookup"><span data-stu-id="766f9-143">With above setup, the first query returns all 6 `Post`s, however the second query only returns 3.</span></span> <span data-ttu-id="766f9-144">Несоответствие происходит потому, что метод `Include` во втором запросе загружает связанные сущности `Blog`.</span><span class="sxs-lookup"><span data-stu-id="766f9-144">This mismatch happens because `Include` method in the second query loads the related `Blog` entities.</span></span> <span data-ttu-id="766f9-145">Поскольку навигация между `Blog` и `Post` является обязательной, при создании запроса EF Core использует `INNER JOIN`:</span><span class="sxs-lookup"><span data-stu-id="766f9-145">Since the navigation between `Blog` and `Post` is required, EF Core uses `INNER JOIN` when constructing the query:</span></span>

```sql
SELECT [p].[PostId], [p].[BlogId], [p].[Content], [p].[IsDeleted], [p].[Title], [t].[BlogId], [t].[Name], [t].[Url]
FROM [Posts] AS [p]
INNER JOIN (
    SELECT [b].[BlogId], [b].[Name], [b].[Url]
    FROM [Blogs] AS [b]
    WHERE [b].[Url] LIKE N'%fish%'
) AS [t] ON [p].[BlogId] = [t].[BlogId]
```

<span data-ttu-id="766f9-146">При использовании `INNER JOIN` фильтр исключает все сущности `Post`, чьи связанные сущности `Blog` были удалены глобальным фильтром запросов.</span><span class="sxs-lookup"><span data-stu-id="766f9-146">Use of the `INNER JOIN` filters out all `Post`s whose related `Blog`s have been removed by a global query filter.</span></span>

<span data-ttu-id="766f9-147">Для решения этой проблемы можно вместо обязательной навигации использовать необязательную.</span><span class="sxs-lookup"><span data-stu-id="766f9-147">It can be addressed by using optional navigation instead of required.</span></span>
<span data-ttu-id="766f9-148">В этом случае первый запрос остается прежним, но второй запрос будет создавать `LEFT JOIN` и возвращать шесть результатов.</span><span class="sxs-lookup"><span data-stu-id="766f9-148">This way the first query stays the same as before, however the second query will now generate `LEFT JOIN` and return 6 results.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/QueryFilters/FilteredBloggingContextRequired.cs#OptionalNavigation)]

<span data-ttu-id="766f9-149">В качестве альтернативного подхода можно указать согласованные фильтры для обеих сущностей `Blog` и `Post`.</span><span class="sxs-lookup"><span data-stu-id="766f9-149">Alternative approach is to specify consistent filters on both `Blog` and `Post` entities.</span></span>
<span data-ttu-id="766f9-150">В этом случае к `Blog` и `Post` будут применяться совпадающие фильтры.</span><span class="sxs-lookup"><span data-stu-id="766f9-150">This way matching filters are applied to both `Blog` and `Post`.</span></span> <span data-ttu-id="766f9-151">Сущности `Post`, которые могут оказаться в непредвиденном состоянии, удаляются, и оба запроса возвращают три результата.</span><span class="sxs-lookup"><span data-stu-id="766f9-151">`Post`s that could end up in unexpected state are removed and both queries return 3 results.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/QueryFilters/FilteredBloggingContextRequired.cs#MatchingFilters)]

## <a name="disabling-filters"></a><span data-ttu-id="766f9-152">Отключение фильтров</span><span class="sxs-lookup"><span data-stu-id="766f9-152">Disabling Filters</span></span>

<span data-ttu-id="766f9-153">Фильтры можно отключить для отдельных запросов LINQ, используя оператор <xref:Microsoft.EntityFrameworkCore.EntityFrameworkQueryableExtensions.IgnoreQueryFilters%2A>.</span><span class="sxs-lookup"><span data-stu-id="766f9-153">Filters may be disabled for individual LINQ queries by using the <xref:Microsoft.EntityFrameworkCore.EntityFrameworkQueryableExtensions.IgnoreQueryFilters%2A> operator.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="766f9-154">Ограничения</span><span class="sxs-lookup"><span data-stu-id="766f9-154">Limitations</span></span>

<span data-ttu-id="766f9-155">Глобальные фильтры запросов имеют следующие ограничения:</span><span class="sxs-lookup"><span data-stu-id="766f9-155">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="766f9-156">Фильтры можно определить только для корневого типа сущности в иерархии наследования.</span><span class="sxs-lookup"><span data-stu-id="766f9-156">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
