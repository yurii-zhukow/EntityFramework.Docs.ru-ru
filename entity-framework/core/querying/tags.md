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
# <a name="query-tags"></a>Теги запросов

Теги запросов помогают сопоставить запросы LINQ в коде со сгенерированными запросами SQL, зарегистрированными в журналах.
Мы можете пометить запрос LINQ с помощью нового метода `TagWith()`:

> [!TIP]
> Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/Tags) из репозитория GitHub.

[!code-csharp[Main](../../../samples/core/Querying/Tags/Program.cs#BasicQueryTag)]

Этот запрос LINQ преобразуется в следующую инструкцию SQL:

```sql
-- This is my spatial query!

SELECT TOP(@__p_1) [p].[Id], [p].[Location]
FROM [People] AS [p]
ORDER BY [p].[Location].STDistance(@__myLocation_0) DESC
```

Метод `TagWith()` можно вызвать много раз для одного и того же запроса.
Теги запросов являются кумулятивными.
Например, при использовании следующих методов

[!code-csharp[Main](../../../samples/core/Querying/Tags/Program.cs#QueryableMethods)]

этот запрос

[!code-csharp[Main](../../../samples/core/Querying/Tags/Program.cs#ChainedQueryTags)]

преобразуется в:

```sql
-- GetNearestPeople

-- Limit

SELECT TOP(@__p_1) [p].[Id], [p].[Location]
FROM [People] AS [p]
ORDER BY [p].[Location].STDistance(@__myLocation_0) DESC
```

В качестве тегов запросов можно использовать многострочные литералы.
Пример:

[!code-csharp[Main](../../../samples/core/Querying/Tags/Program.cs#MultilineQueryTag)]

Преобразуется в следующий запрос SQL:

```sql
-- GetNearestPeople

-- Limit

-- This is a multi-line
-- string

SELECT TOP(@__p_1) [p].[Id], [p].[Location]
FROM [People] AS [p]
ORDER BY [p].[Location].STDistance(@__myLocation_0) DESC
```

## <a name="known-limitations"></a>Известные ограничения

**Теги запросов не поддерживают параметризацию** — EF Core всегда обрабатывает теги в запросе LINQ как строковые литералы, которые включаются в сгенерированный запрос SQL.
Скомпилированные запросы, принимающие теги запросов в качестве параметров, не допускаются.
