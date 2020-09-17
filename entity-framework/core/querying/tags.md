---
title: Теги запросов — EF Core
description: Использование тегов запросов для выявления конкретных запросов в сообщениях журнала, созданных Entity Framework Core
author: divega
ms.date: 11/14/2018
uid: core/querying/tags
ms.openlocfilehash: 27f757f4159a36bec324cce56d74b7860e1c3741
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90070995"
---
# <a name="query-tags"></a><span data-ttu-id="b246a-103">Теги запросов</span><span class="sxs-lookup"><span data-stu-id="b246a-103">Query tags</span></span>

> [!NOTE]
> <span data-ttu-id="b246a-104">Это новая функция в EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="b246a-104">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="b246a-105">Эта функция помогает сопоставить запросы LINQ в коде со сгенерированными запросами SQL, зарегистрированными в журналах.</span><span class="sxs-lookup"><span data-stu-id="b246a-105">This feature helps correlate LINQ queries in code with generated SQL queries captured in logs.</span></span>
<span data-ttu-id="b246a-106">Мы можете пометить запрос LINQ с помощью нового метода `TagWith()`:</span><span class="sxs-lookup"><span data-stu-id="b246a-106">You annotate a LINQ query using the new `TagWith()` method:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="b246a-107">Этот запрос LINQ преобразуется в следующую инструкцию SQL:</span><span class="sxs-lookup"><span data-stu-id="b246a-107">This LINQ query is translated to the following SQL statement:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="b246a-108">Метод `TagWith()` можно вызвать много раз для одного и того же запроса.</span><span class="sxs-lookup"><span data-stu-id="b246a-108">It's possible to call `TagWith()` many times on the same query.</span></span>
<span data-ttu-id="b246a-109">Теги запросов являются кумулятивными.</span><span class="sxs-lookup"><span data-stu-id="b246a-109">Query tags are cumulative.</span></span>
<span data-ttu-id="b246a-110">Например, при использовании следующих методов</span><span class="sxs-lookup"><span data-stu-id="b246a-110">For example, given the following methods:</span></span>

``` csharp
IQueryable<Friend> GetNearestFriends(Point myLocation) =>
    from f in context.Friends.TagWith("GetNearestFriends")
    orderby f.Location.Distance(myLocation) descending
    select f;

IQueryable<T> Limit<T>(IQueryable<T> source, int limit) =>
    source.TagWith("Limit").Take(limit);
```

<span data-ttu-id="b246a-111">этот запрос</span><span class="sxs-lookup"><span data-stu-id="b246a-111">The following query:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).ToList();
```

<span data-ttu-id="b246a-112">преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="b246a-112">Translates to:</span></span>

``` sql
-- GetNearestFriends

-- Limit

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="b246a-113">В качестве тегов запросов можно использовать многострочные литералы.</span><span class="sxs-lookup"><span data-stu-id="b246a-113">It's also possible to use multi-line strings as query tags.</span></span>
<span data-ttu-id="b246a-114">Пример:</span><span class="sxs-lookup"><span data-stu-id="b246a-114">For example:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

<span data-ttu-id="b246a-115">Преобразуется в следующий запрос SQL:</span><span class="sxs-lookup"><span data-stu-id="b246a-115">Produces the following SQL:</span></span>

``` sql
-- GetNearestFriends

-- Limit

-- This is a multi-line
-- string

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

## <a name="known-limitations"></a><span data-ttu-id="b246a-116">Известные ограничения</span><span class="sxs-lookup"><span data-stu-id="b246a-116">Known limitations</span></span>

<span data-ttu-id="b246a-117">**Теги запросов не поддерживают параметризацию** — EF Core всегда обрабатывает теги в запросе LINQ как строковые литералы, которые включаются в сгенерированный запрос SQL.</span><span class="sxs-lookup"><span data-stu-id="b246a-117">**Query tags aren't parameterizable:** EF Core always treats query tags in the LINQ query as string literals that are included in the generated SQL.</span></span>
<span data-ttu-id="b246a-118">Скомпилированные запросы, принимающие теги запросов в качестве параметров, не допускаются.</span><span class="sxs-lookup"><span data-stu-id="b246a-118">Compiled queries that take query tags as parameters aren't allowed.</span></span>
