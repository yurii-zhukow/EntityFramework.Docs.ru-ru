---
title: Сложные операторы запросов — EF Core
description: Подробные сведения о более сложных операторах запросов LINQ при использовании Entity Framework Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 2e187a2a-4072-4198-9040-aaad68e424fd
uid: core/querying/complex-query-operators
ms.openlocfilehash: 61a46361cc4bc1635174aaf2ca3260fa67fc6aaf
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89616439"
---
# <a name="complex-query-operators"></a><span data-ttu-id="6f511-103">Сложные операторы запросов</span><span class="sxs-lookup"><span data-stu-id="6f511-103">Complex Query Operators</span></span>

<span data-ttu-id="6f511-104">В языке LINQ есть множество сложных операторов, которые объединяют несколько источников данных или производят сложную обработку.</span><span class="sxs-lookup"><span data-stu-id="6f511-104">Language Integrated Query (LINQ) contains many complex operators, which combine multiple data sources or does complex processing.</span></span> <span data-ttu-id="6f511-105">Не для всех операторов LINQ есть подходящие преобразования на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="6f511-105">Not all LINQ operators have suitable translations on the server side.</span></span> <span data-ttu-id="6f511-106">Иногда запрос в одной форме преобразуется на сервере, но в другой — не преобразуется, даже если результат совпадает.</span><span class="sxs-lookup"><span data-stu-id="6f511-106">Sometimes, a query in one form translates to the server but if written in a different form doesn't translate even if the result is the same.</span></span> <span data-ttu-id="6f511-107">На этой странице описываются некоторые сложные операторы и их поддерживаемые варианты.</span><span class="sxs-lookup"><span data-stu-id="6f511-107">This page describes some of the complex operators and their supported variations.</span></span> <span data-ttu-id="6f511-108">В будущих выпусках, возможно, будет поддерживаться больше шаблонов и будут добавлены соответствующие преобразования.</span><span class="sxs-lookup"><span data-stu-id="6f511-108">In future releases, we may recognize more patterns and add their corresponding translations.</span></span> <span data-ttu-id="6f511-109">Также важно иметь в виду, что поддержка преобразований зависит от поставщика.</span><span class="sxs-lookup"><span data-stu-id="6f511-109">It's also important to keep in mind that translation support varies between providers.</span></span> <span data-ttu-id="6f511-110">Запрос, который преобразуется в SqlServer, может не работать в базах данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="6f511-110">A particular query, which is translated in SqlServer, may not work for SQLite databases.</span></span>

> [!TIP]
> <span data-ttu-id="6f511-111">Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="6f511-111">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="join"></a><span data-ttu-id="6f511-112">Join</span><span class="sxs-lookup"><span data-stu-id="6f511-112">Join</span></span>

<span data-ttu-id="6f511-113">Оператор Join в LINQ позволяет соединять два источника данных на основе селектора ключа для каждого источника, создавая кортеж значений при совпадении ключей.</span><span class="sxs-lookup"><span data-stu-id="6f511-113">The LINQ Join operator allows you to connect two data sources based on the key selector for each source, generating a tuple of values when the key matches.</span></span> <span data-ttu-id="6f511-114">В реляционных базах данных он естественным образом преобразуется в `INNER JOIN`.</span><span class="sxs-lookup"><span data-stu-id="6f511-114">It naturally translates to `INNER JOIN` on relational databases.</span></span> <span data-ttu-id="6f511-115">Если оператор Join в LINQ имеет внешний и внутренний селекторы ключей, база данных требует одного условия соединения.</span><span class="sxs-lookup"><span data-stu-id="6f511-115">While the LINQ Join has outer and inner key selectors, the database requires a single join condition.</span></span> <span data-ttu-id="6f511-116">Таким образом, EF Core создает условие соединения, сравнивая внешний и внутренний селекторы ключей на равенство.</span><span class="sxs-lookup"><span data-stu-id="6f511-116">So EF Core generates a join condition by comparing the outer key selector to the inner key selector for equality.</span></span> <span data-ttu-id="6f511-117">Кроме того, если селекторы ключей являются анонимными типами, EF Core создает условие соединения для покомпонентного сравнения на равенство.</span><span class="sxs-lookup"><span data-stu-id="6f511-117">Further, if the key selectors are anonymous types, EF Core generates a join condition to compare equality component wise.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a><span data-ttu-id="6f511-118">GroupJoin</span><span class="sxs-lookup"><span data-stu-id="6f511-118">GroupJoin</span></span>

<span data-ttu-id="6f511-119">Оператор GroupJoin в LINQ позволяет соединять два источника данных, так же как оператор JOIN, но создает группу внутренних значений для соответствующих внешних элементов.</span><span class="sxs-lookup"><span data-stu-id="6f511-119">The LINQ GroupJoin operator allows you to connect two data sources similar to Join, but it creates a group of inner values for matching outer elements.</span></span> <span data-ttu-id="6f511-120">Выполнение приведенного ниже запроса дает результат `Blog` & `IEnumerable<Post>`.</span><span class="sxs-lookup"><span data-stu-id="6f511-120">Executing a query like the following example generates a result of `Blog` & `IEnumerable<Post>`.</span></span> <span data-ttu-id="6f511-121">Так как базы данных (особенно реляционные) обычно не позволяют представлять коллекцию объектов на стороне клиента, GroupJoin во многих случаях не преобразуется на сервере.</span><span class="sxs-lookup"><span data-stu-id="6f511-121">Since databases (especially relational databases) don't have a way to represent a collection of client-side objects, GroupJoin doesn't translate to the server in many cases.</span></span> <span data-ttu-id="6f511-122">Для выполнения GroupJoin без специального селектора требуется получить все данные с сервера (первый запрос ниже).</span><span class="sxs-lookup"><span data-stu-id="6f511-122">It requires you to get all of the data from the server to do GroupJoin without a special selector (first query below).</span></span> <span data-ttu-id="6f511-123">Но если селектор ограничивает выбор данных, то получение всех данных с сервера может вызвать проблемы с производительностью (второй запрос ниже).</span><span class="sxs-lookup"><span data-stu-id="6f511-123">But if the selector is limiting data being selected then fetching all of the data from the server may cause performance issues (second query below).</span></span> <span data-ttu-id="6f511-124">Вот почему EF Core не преобразует оператор GroupJoin.</span><span class="sxs-lookup"><span data-stu-id="6f511-124">That's why EF Core doesn't translate GroupJoin.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a><span data-ttu-id="6f511-125">SelectMany</span><span class="sxs-lookup"><span data-stu-id="6f511-125">SelectMany</span></span>

<span data-ttu-id="6f511-126">Оператор SelectMany в LINQ позволяет перечислять селектор коллекции для каждого внешнего элемента и создавать кортежи значений из каждого источника данных.</span><span class="sxs-lookup"><span data-stu-id="6f511-126">The LINQ SelectMany operator allows you to enumerate over a collection selector for each outer element and generate tuples of values from each data source.</span></span> <span data-ttu-id="6f511-127">Таким образом, это соединение, но без каких-либо условий, поэтому каждый внешний элемент соединяется с элементом из источника коллекции.</span><span class="sxs-lookup"><span data-stu-id="6f511-127">In a way, it's a join but without any condition so every outer element is connected with an element from the collection source.</span></span> <span data-ttu-id="6f511-128">В зависимости от того, как селектор коллекции связан с внешним источником данных, оператор SelectMany может преобразовываться в различные запросы на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="6f511-128">Depending on how the collection selector is related to the outer data source, SelectMany can translate into various different queries on the server side.</span></span>

### <a name="collection-selector-doesnt-reference-outer"></a><span data-ttu-id="6f511-129">Селектор коллекции не ссылается на внешний источник</span><span class="sxs-lookup"><span data-stu-id="6f511-129">Collection selector doesn't reference outer</span></span>

<span data-ttu-id="6f511-130">Если селектор коллекции не ссылается на элементы во внешнем источнике, результатом является декартово произведение обоих источников данных.</span><span class="sxs-lookup"><span data-stu-id="6f511-130">When the collection selector isn't referencing anything from the outer source, the result is a cartesian product of both data sources.</span></span> <span data-ttu-id="6f511-131">В реляционных базах данных такой оператор преобразуется в `CROSS JOIN`.</span><span class="sxs-lookup"><span data-stu-id="6f511-131">It translates to `CROSS JOIN` in relational databases.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a><span data-ttu-id="6f511-132">Селектор коллекции ссылается на внешний источник в предложении WHERE</span><span class="sxs-lookup"><span data-stu-id="6f511-132">Collection selector references outer in a where clause</span></span>

<span data-ttu-id="6f511-133">Если в селекторе коллекции есть предложение WHERE, которое ссылается на внешний элемент, то EF Core преобразует его в соединение базы данных и использует предикат в качестве условия соединения.</span><span class="sxs-lookup"><span data-stu-id="6f511-133">When the collection selector has a where clause, which references the outer element, then EF Core translates it to a database join and uses the predicate as the join condition.</span></span> <span data-ttu-id="6f511-134">Обычно такое бывает при использовании свойства навигации по коллекции для внешнего элемента в качестве селектора коллекции.</span><span class="sxs-lookup"><span data-stu-id="6f511-134">Normally this case arises when using collection navigation on the outer element as the collection selector.</span></span> <span data-ttu-id="6f511-135">Если коллекция для внешнего элемента пуста, то для него результаты отсутствуют.</span><span class="sxs-lookup"><span data-stu-id="6f511-135">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="6f511-136">Но если к селектору коллекции применяется `DefaultIfEmpty`, внешний элемент соединяется со значением внутреннего элемента по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6f511-136">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="6f511-137">В связи с этим различием запросы такого типа преобразуются в `INNER JOIN` при отсутствии `DefaultIfEmpty` и в `LEFT JOIN`, если `DefaultIfEmpty` применяется.</span><span class="sxs-lookup"><span data-stu-id="6f511-137">Because of this distinction, this kind of queries translates to `INNER JOIN` in the absence of `DefaultIfEmpty` and `LEFT JOIN` when `DefaultIfEmpty` is applied.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a><span data-ttu-id="6f511-138">Селектор коллекции ссылается на внешний источник не в предложении WHERE</span><span class="sxs-lookup"><span data-stu-id="6f511-138">Collection selector references outer in a non-where case</span></span>

<span data-ttu-id="6f511-139">Если селектор коллекции ссылается на внешний элемент, который не входит в предложение WHERE (как в случае выше), он не преобразуется в соединение базы данных.</span><span class="sxs-lookup"><span data-stu-id="6f511-139">When the collection selector references the outer element, which isn't in a where clause (as the case above), it doesn't translate to a database join.</span></span> <span data-ttu-id="6f511-140">Поэтому необходимо оценить селектор коллекции для каждого внешнего элемента.</span><span class="sxs-lookup"><span data-stu-id="6f511-140">That's why we need to evaluate the collection selector for each outer element.</span></span> <span data-ttu-id="6f511-141">Во многих реляционных базах данных такой оператор преобразуется в операции `APPLY`.</span><span class="sxs-lookup"><span data-stu-id="6f511-141">It translates to `APPLY` operations in many relational databases.</span></span> <span data-ttu-id="6f511-142">Если коллекция для внешнего элемента пуста, то для него результаты отсутствуют.</span><span class="sxs-lookup"><span data-stu-id="6f511-142">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="6f511-143">Но если к селектору коллекции применяется `DefaultIfEmpty`, внешний элемент соединяется со значением внутреннего элемента по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6f511-143">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="6f511-144">В связи с этим различием запросы такого типа преобразуются в `CROSS APPLY` при отсутствии `DefaultIfEmpty` и в `OUTER APPLY`, если `DefaultIfEmpty` применяется.</span><span class="sxs-lookup"><span data-stu-id="6f511-144">Because of this distinction, this kind of queries translates to `CROSS APPLY` in the absence of `DefaultIfEmpty` and `OUTER APPLY` when `DefaultIfEmpty` is applied.</span></span> <span data-ttu-id="6f511-145">Некоторые базы данных, такие как SQLite, не поддерживают операторы `APPLY`, поэтому запросы такого типа могут не преобразовываться.</span><span class="sxs-lookup"><span data-stu-id="6f511-145">Certain databases like SQLite don't support `APPLY` operators so this kind of query may not be translated.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToApply)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
CROSS APPLY [Posts] AS [p]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
OUTER APPLY [Posts] AS [p]
```

## <a name="groupby"></a><span data-ttu-id="6f511-146">GroupBy</span><span class="sxs-lookup"><span data-stu-id="6f511-146">GroupBy</span></span>

<span data-ttu-id="6f511-147">Операторы GroupBy в LINQ создают результат типа `IGrouping<TKey, TElement>`, где `TKey` и `TElement` могут быть произвольного типа.</span><span class="sxs-lookup"><span data-stu-id="6f511-147">LINQ GroupBy operators create a result of type `IGrouping<TKey, TElement>` where `TKey` and `TElement` could be any arbitrary type.</span></span> <span data-ttu-id="6f511-148">Кроме того, `IGrouping` реализует интерфейс `IEnumerable<TElement>`. Это означает, что после группирования можно производить композицию с помощью любого оператора LINQ.</span><span class="sxs-lookup"><span data-stu-id="6f511-148">Furthermore, `IGrouping` implements `IEnumerable<TElement>`, which means you can compose over it using any LINQ operator after the grouping.</span></span> <span data-ttu-id="6f511-149">Так как ни одна структура базы данных не может представлять `IGrouping`, в большинстве случаев операторы GroupBy не преобразуются.</span><span class="sxs-lookup"><span data-stu-id="6f511-149">Since no database structure can represent an `IGrouping`, GroupBy operators have no translation in most cases.</span></span> <span data-ttu-id="6f511-150">Если к каждой группе применяется статистический оператор, возвращающий скалярное значение, в реляционных базах данных его можно преобразовать в оператор SQL `GROUP BY`.</span><span class="sxs-lookup"><span data-stu-id="6f511-150">When an aggregate operator is applied to each group, which returns a scalar, it can be translated to SQL `GROUP BY` in relational databases.</span></span> <span data-ttu-id="6f511-151">Применение оператора SQL `GROUP BY` также ограничено.</span><span class="sxs-lookup"><span data-stu-id="6f511-151">The SQL `GROUP BY` is restrictive too.</span></span> <span data-ttu-id="6f511-152">Он требует выполнять группирование только по скалярным значениям.</span><span class="sxs-lookup"><span data-stu-id="6f511-152">It requires you to group only by scalar values.</span></span> <span data-ttu-id="6f511-153">Проекция может содержать только ключевые столбцы группирования или любое статистическое выражение, примененное к столбцу.</span><span class="sxs-lookup"><span data-stu-id="6f511-153">The projection can only contain grouping key columns or any aggregate applied over a column.</span></span> <span data-ttu-id="6f511-154">EF Core распознает этот шаблон и преобразует его на стороне сервера, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="6f511-154">EF Core identifies this pattern and translates it to the server, as in the following example:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

<span data-ttu-id="6f511-155">EF Core также преобразует запросы, в которых статистический оператор, выполняющий действия над группированием, включен в оператор LINQ Where или OrderBy (либо другое упорядочение).</span><span class="sxs-lookup"><span data-stu-id="6f511-155">EF Core also translates queries where an aggregate operator on the grouping appears in a Where or OrderBy (or other ordering) LINQ operator.</span></span> <span data-ttu-id="6f511-156">Для предложения WHERE в SQL используется предложение `HAVING`.</span><span class="sxs-lookup"><span data-stu-id="6f511-156">It uses `HAVING` clause in SQL for the where clause.</span></span> <span data-ttu-id="6f511-157">Часть запроса перед применением оператора GroupBy может быть любым сложным запросом, который может быть преобразован на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="6f511-157">The part of the query before applying the GroupBy operator can be any complex query as long as it can be translated to server.</span></span> <span data-ttu-id="6f511-158">Кроме того, после применения статистических операторов к запросу группирования для удаления группирований из результирующего источника этот запрос можно использовать для композиции, как и любой другой запрос.</span><span class="sxs-lookup"><span data-stu-id="6f511-158">Furthermore, once you apply aggregate operators on a grouping query to remove groupings from the resulting source, you can compose on top of it like any other query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

<span data-ttu-id="6f511-159">EF Core поддерживает следующие статистические операторы:</span><span class="sxs-lookup"><span data-stu-id="6f511-159">The aggregate operators EF Core supports are as follows</span></span>

- <span data-ttu-id="6f511-160">Средний</span><span class="sxs-lookup"><span data-stu-id="6f511-160">Average</span></span>
- <span data-ttu-id="6f511-161">Счетчик</span><span class="sxs-lookup"><span data-stu-id="6f511-161">Count</span></span>
- <span data-ttu-id="6f511-162">LongCount</span><span class="sxs-lookup"><span data-stu-id="6f511-162">LongCount</span></span>
- <span data-ttu-id="6f511-163">Макс.</span><span class="sxs-lookup"><span data-stu-id="6f511-163">Max</span></span>
- <span data-ttu-id="6f511-164">Min</span><span class="sxs-lookup"><span data-stu-id="6f511-164">Min</span></span>
- <span data-ttu-id="6f511-165">SUM</span><span class="sxs-lookup"><span data-stu-id="6f511-165">Sum</span></span>

## <a name="left-join"></a><span data-ttu-id="6f511-166">Левое соединение</span><span class="sxs-lookup"><span data-stu-id="6f511-166">Left Join</span></span>

<span data-ttu-id="6f511-167">Хотя в LINQ нет оператора левого соединения, в запросах к реляционным базам данных левое соединение используется часто.</span><span class="sxs-lookup"><span data-stu-id="6f511-167">While Left Join isn't a LINQ operator, relational databases have the concept of a Left Join which is frequently used in queries.</span></span> <span data-ttu-id="6f511-168">Определенный шаблон в запросах LINQ дает тот же результат, что и оператор `LEFT JOIN` на сервере.</span><span class="sxs-lookup"><span data-stu-id="6f511-168">A particular pattern in LINQ queries gives the same result as a `LEFT JOIN` on the server.</span></span> <span data-ttu-id="6f511-169">EF Core распознает такой шаблон и создает эквивалентный оператор `LEFT JOIN` на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="6f511-169">EF Core identifies such patterns and generates the equivalent `LEFT JOIN` on the server side.</span></span> <span data-ttu-id="6f511-170">Шаблон предполагает создание соединения GroupJoin между двумя источниками данных и последующее преобразование группирования в плоскую структуру с помощью оператора SelectMany с DefaultIfEmpty, применяемого к источнику группирования, для сопоставления со значением NULL, когда во внутреннем источнике нет соответствующего элемента.</span><span class="sxs-lookup"><span data-stu-id="6f511-170">The pattern involves creating a GroupJoin between both the data sources and then flattening out the grouping by using the SelectMany operator with DefaultIfEmpty on the grouping source to match null when the inner doesn't have a related element.</span></span> <span data-ttu-id="6f511-171">В приведенном ниже примере показано, как выглядит этот шаблон и какой результат он дает.</span><span class="sxs-lookup"><span data-stu-id="6f511-171">The following example shows what that pattern looks like and what it generates.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

<span data-ttu-id="6f511-172">Приведенный выше шаблон создает сложную структуру в дереве выражения.</span><span class="sxs-lookup"><span data-stu-id="6f511-172">The above pattern creates a complex structure in the expression tree.</span></span> <span data-ttu-id="6f511-173">По этой причине EF Core требует преобразовать результаты группирования с помощью оператора GroupJoin в плоскую структуру сразу после этого оператора.</span><span class="sxs-lookup"><span data-stu-id="6f511-173">Because of that, EF Core requires you to flatten out the grouping results of the GroupJoin operator in a step immediately following the operator.</span></span> <span data-ttu-id="6f511-174">Если конструкция GroupJoin-DefaultIfEmpty-SelectMany используется в рамках другого шаблона, она может не распознаваться как левое соединение.</span><span class="sxs-lookup"><span data-stu-id="6f511-174">Even if the GroupJoin-DefaultIfEmpty-SelectMany is used but in a different pattern, we may not identify it as a Left Join.</span></span>
