---
title: Теги запросов — EF Core
description: Использование тегов запросов для выявления конкретных запросов в сообщениях журнала, созданных Entity Framework Core
author: smitpatel
ms.date: 11/14/2018
uid: core/querying/tags
ms.openlocfilehash: f7cd3558682b1c19e03fc6d04957c7112e870734
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92065736"
---
# <a name="query-tags"></a><span data-ttu-id="a2f01-103">Теги запросов</span><span class="sxs-lookup"><span data-stu-id="a2f01-103">Query tags</span></span>

<span data-ttu-id="a2f01-104">Теги запросов помогают сопоставить запросы LINQ в коде со сгенерированными запросами SQL, зарегистрированными в журналах.</span><span class="sxs-lookup"><span data-stu-id="a2f01-104">Query tags help correlate LINQ queries in code with generated SQL queries captured in logs.</span></span>
<span data-ttu-id="a2f01-105">Мы можете пометить запрос LINQ с помощью нового метода `TagWith()`:</span><span class="sxs-lookup"><span data-stu-id="a2f01-105">You annotate a LINQ query using the new `TagWith()` method:</span></span>

> [!TIP]
> <span data-ttu-id="a2f01-106">Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/Tags) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="a2f01-106">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/Tags) on GitHub.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Tags/Program.cs#BasicQueryTag)]

<span data-ttu-id="a2f01-107">Этот запрос LINQ преобразуется в следующую инструкцию SQL:</span><span class="sxs-lookup"><span data-stu-id="a2f01-107">This LINQ query is translated to the following SQL statement:</span></span>

```sql
-- This is my spatial query!

SELECT TOP(@__p_1) [p].[Id], [p].[Location]
FROM [People] AS [p]
ORDER BY [p].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="a2f01-108">Метод `TagWith()` можно вызвать много раз для одного и того же запроса.</span><span class="sxs-lookup"><span data-stu-id="a2f01-108">It's possible to call `TagWith()` many times on the same query.</span></span>
<span data-ttu-id="a2f01-109">Теги запросов являются кумулятивными.</span><span class="sxs-lookup"><span data-stu-id="a2f01-109">Query tags are cumulative.</span></span>
<span data-ttu-id="a2f01-110">Например, при использовании следующих методов</span><span class="sxs-lookup"><span data-stu-id="a2f01-110">For example, given the following methods:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Tags/Program.cs#QueryableMethods)]

<span data-ttu-id="a2f01-111">этот запрос</span><span class="sxs-lookup"><span data-stu-id="a2f01-111">The following query:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Tags/Program.cs#ChainedQueryTags)]

<span data-ttu-id="a2f01-112">преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="a2f01-112">Translates to:</span></span>

```sql
-- GetNearestPeople

-- Limit

SELECT TOP(@__p_1) [p].[Id], [p].[Location]
FROM [People] AS [p]
ORDER BY [p].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="a2f01-113">В качестве тегов запросов можно использовать многострочные литералы.</span><span class="sxs-lookup"><span data-stu-id="a2f01-113">It's also possible to use multi-line strings as query tags.</span></span>
<span data-ttu-id="a2f01-114">Пример:</span><span class="sxs-lookup"><span data-stu-id="a2f01-114">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Tags/Program.cs#MultilineQueryTag)]

<span data-ttu-id="a2f01-115">Преобразуется в следующий запрос SQL:</span><span class="sxs-lookup"><span data-stu-id="a2f01-115">Produces the following SQL:</span></span>

```sql
-- GetNearestPeople

-- Limit

-- This is a multi-line
-- string

SELECT TOP(@__p_1) [p].[Id], [p].[Location]
FROM [People] AS [p]
ORDER BY [p].[Location].STDistance(@__myLocation_0) DESC
```

## <a name="known-limitations"></a><span data-ttu-id="a2f01-116">Известные ограничения</span><span class="sxs-lookup"><span data-stu-id="a2f01-116">Known limitations</span></span>

<span data-ttu-id="a2f01-117">**Теги запросов не поддерживают параметризацию** — EF Core всегда обрабатывает теги в запросе LINQ как строковые литералы, которые включаются в сгенерированный запрос SQL.</span><span class="sxs-lookup"><span data-stu-id="a2f01-117">**Query tags aren't parameterizable:** EF Core always treats query tags in the LINQ query as string literals that are included in the generated SQL.</span></span>
<span data-ttu-id="a2f01-118">Скомпилированные запросы, принимающие теги запросов в качестве параметров, не допускаются.</span><span class="sxs-lookup"><span data-stu-id="a2f01-118">Compiled queries that take query tags as parameters aren't allowed.</span></span>
