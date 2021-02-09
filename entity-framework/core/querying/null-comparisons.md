---
title: Сравнения со значениями NULL в запросах
description: Сведения о том, как в Entity Framework Core обрабатываются сравнения со значениями NULL в запросах
author: maumar
ms.date: 11/11/2020
uid: core/querying/null-comparisons
ms.openlocfilehash: fc63d0e0e6aea09e46b1700152312d4b74270219
ms.sourcegitcommit: 7700840119b1639275f3b64836e7abb59103f2e7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2021
ms.locfileid: "98983356"
---
# <a name="query-null-semantics"></a><span data-ttu-id="9aa2a-103">Семантика значений NULL в запросах</span><span class="sxs-lookup"><span data-stu-id="9aa2a-103">Query null semantics</span></span>

## <a name="introduction"></a><span data-ttu-id="9aa2a-104">Вступление</span><span class="sxs-lookup"><span data-stu-id="9aa2a-104">Introduction</span></span>

<span data-ttu-id="9aa2a-105">При выполнении сравнений базы данных SQL работают с 3-значной логикой (`true`, `false`, `null`), в отличие от C#, где для этих целей применяется бинарная логика.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-105">SQL databases operate on 3-valued logic (`true`, `false`, `null`) when performing comparisons, as opposed to the boolean logic of C#.</span></span> <span data-ttu-id="9aa2a-106">При преобразовании запросов LINQ в SQL EF Core пытается компенсировать это различие, вводя дополнительные проверки значений NULL для некоторых элементов запроса.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-106">When translating LINQ queries to SQL, EF Core tries to compensate for the difference by introducing additional null checks for some elements of the query.</span></span>
<span data-ttu-id="9aa2a-107">Чтобы продемонстрировать это, определим следующую сущность:</span><span class="sxs-lookup"><span data-stu-id="9aa2a-107">To illustrate this, let's define the following entity:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/NullSemantics/NullSemanticsEntity.cs#Entity)]

<span data-ttu-id="9aa2a-108">и выполним несколько запросов.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-108">and issue several queries:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/NullSemantics/Program.cs#BasicExamples)]

<span data-ttu-id="9aa2a-109">В результате первых двух запросов выполняются простые сравнения.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-109">The first two queries produce simple comparisons.</span></span> <span data-ttu-id="9aa2a-110">В первом запросе оба столбца не допускают значения NULL, в связи с чем проверка значений NULL не требуется.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-110">In the first query, both columns are non-nullable so null checks are not needed.</span></span> <span data-ttu-id="9aa2a-111">Во втором запросе `NullableInt` может содержать значение `null`, а `Id` не допускает такие значения. При сравнении `null` с отличным от NULL значением получается результат `null`, который будет отфильтрован операцией `WHERE`.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-111">In the second query, `NullableInt` could contain `null`, but `Id` is non-nullable; comparing `null` to non-null yields `null` as a result, which would be filtered out by `WHERE` operation.</span></span> <span data-ttu-id="9aa2a-112">Соответственно, не нужны никакие дополнительные условия.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-112">So no additional terms are needed either.</span></span>

```sql
SELECT [e].[Id], [e].[Int], [e].[NullableInt], [e].[String1], [e].[String2]
FROM [Entities] AS [e]
WHERE [e].[Id] = [e].[Int]

SELECT [e].[Id], [e].[Int], [e].[NullableInt], [e].[String1], [e].[String2]
FROM [Entities] AS [e]
WHERE [e].[Id] = [e].[NullableInt]
```

<span data-ttu-id="9aa2a-113">Третий запрос приводит к выполнению проверки значений NULL.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-113">The third query introduces a null check.</span></span> <span data-ttu-id="9aa2a-114">Если `NullableInt` имеет значение `null`, при сравнении `Id <> NullableInt` получается результат `null`, который будет отфильтрован операцией `WHERE`.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-114">When `NullableInt` is `null` the comparison `Id <> NullableInt` yields `null`, which would be filtered out by `WHERE` operation.</span></span> <span data-ttu-id="9aa2a-115">Тем не менее с точки зрения бинарной логики этот вариант должен возвращаться в составе результата.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-115">However, from the boolean logic perspective this case should be returned as part of the result.</span></span> <span data-ttu-id="9aa2a-116">Поэтому EF Core добавляет соответствующую проверку, чтобы убедиться в этом.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-116">Hence EF Core adds the necessary check to ensure that.</span></span>

```sql
SELECT [e].[Id], [e].[Int], [e].[NullableInt], [e].[String1], [e].[String2]
FROM [Entities] AS [e]
WHERE ([e].[Id] <> [e].[NullableInt]) OR [e].[NullableInt] IS NULL
```

<span data-ttu-id="9aa2a-117">Запросы 4 и 5 демонстрируют ситуацию, в которой оба столбца допускают значения NULL.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-117">Queries four and five show the pattern when both columns are nullable.</span></span> <span data-ttu-id="9aa2a-118">Следует отметить, что операция `<>` выдает более сложный (и потенциально более медленный) запрос по сравнению с операцией `==`.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-118">It's worth noting that the `<>` operation produces more complicated (and potentially slower) query than the `==` operation.</span></span>

```sql
SELECT [e].[Id], [e].[Int], [e].[NullableInt], [e].[String1], [e].[String2]
FROM [Entities] AS [e]
WHERE ([e].[String1] = [e].[String2]) OR ([e].[String1] IS NULL AND [e].[String2] IS NULL)

SELECT [e].[Id], [e].[Int], [e].[NullableInt], [e].[String1], [e].[String2]
FROM [Entities] AS [e]
WHERE (([e].[String1] <> [e].[String2]) OR ([e].[String1] IS NULL OR [e].[String2] IS NULL)) AND ([e].[String1] IS NOT NULL OR [e].[String2] IS NOT NULL)
```

## <a name="treatment-of-nullable-values-in-functions"></a><span data-ttu-id="9aa2a-119">Обработка значений, допускающих значения NULL, в функциях</span><span class="sxs-lookup"><span data-stu-id="9aa2a-119">Treatment of nullable values in functions</span></span>

<span data-ttu-id="9aa2a-120">Многие функции в SQL могут возвращать результат `null`, только если некоторые из их аргументов имеют значение `null`.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-120">Many functions in SQL can only return a `null` result if some of their arguments are `null`.</span></span> <span data-ttu-id="9aa2a-121">EF Core использует преимущества этого подхода для создания более эффективных запросов.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-121">EF Core takes advantage of this to produce more efficient queries.</span></span>
<span data-ttu-id="9aa2a-122">Пример такой оптимизации показан в приведенном ниже запросе.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-122">The query below illustrates the optimization:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/NullSemantics/Program.cs#Functions)]

<span data-ttu-id="9aa2a-123">Созданный код SQL выглядит следующим образом (нам не нужно рассчитывать функцию `SUBSTRING`, так как она будет иметь значение NULL, только если какой-либо из ее аргументов имеет значение NULL).</span><span class="sxs-lookup"><span data-stu-id="9aa2a-123">The generated SQL is as follows (we don't need to evaluate the `SUBSTRING` function since it will be only null when either of the arguments to it is null.):</span></span>

```sql
SELECT [e].[Id], [e].[Int], [e].[NullableInt], [e].[String1], [e].[String2]
FROM [Entities] AS [e]
WHERE [e].[String1] IS NULL OR [e].[String2] IS NULL
```

<span data-ttu-id="9aa2a-124">Для определяемых пользователем функций такая оптимизация также возможна.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-124">The optimization can also be used for user-defined functions.</span></span> <span data-ttu-id="9aa2a-125">Дополнительные сведения см. на странице [Сопоставление определяемых пользователем функций](xref:core/querying/user-defined-function-mapping#configuring-nullability-of-user-defined-function-based-on-its-arguments).</span><span class="sxs-lookup"><span data-stu-id="9aa2a-125">See [user defined function mapping](xref:core/querying/user-defined-function-mapping#configuring-nullability-of-user-defined-function-based-on-its-arguments) page for more details.</span></span>

## <a name="writing-performant-queries"></a><span data-ttu-id="9aa2a-126">Написание эффективных запросов</span><span class="sxs-lookup"><span data-stu-id="9aa2a-126">Writing performant queries</span></span>

- <span data-ttu-id="9aa2a-127">Сравнение столбцов, не допускающих значения NULL, выполняется проще и быстрее, чем сравнение столбцов, допускающих такие значения.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-127">Comparing non-nullable columns is simpler and faster than comparing nullable columns.</span></span> <span data-ttu-id="9aa2a-128">Соответственно, во всех возможных случаях рекомендуется помечать столбцы как не допускающие значения NULL.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-128">Consider marking columns as non-nullable whenever possible.</span></span>

- <span data-ttu-id="9aa2a-129">Проверка равенства (`==`) выполняется проще и быстрее, чем проверка неравенства (`!=`), так как запросу не требуется различать результаты `null` и `false`.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-129">Checking for equality (`==`) is simpler and faster than checking for non-equality (`!=`), because query doesn't need to distinguish between `null` and `false` result.</span></span> <span data-ttu-id="9aa2a-130">Во всех возможных случаях рекомендуется использовать сравнение на равенство.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-130">Use equality comparison whenever possible.</span></span> <span data-ttu-id="9aa2a-131">Обратите внимание, что простое отрицание сравнения `==` фактически аналогично сравнению `!=` и не приводит к повышению производительности.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-131">However, simply negating `==` comparison is effectively the same as `!=`, so it doesn't result in performance improvement.</span></span>

- <span data-ttu-id="9aa2a-132">В некоторых случаях можно упростить сложное сравнение, явно отфильтровав значения `null` из столбца, например если значения `null` отсутствуют или не являются значимыми для результата.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-132">In some cases, it is possible to simplify a complex comparison by filtering out `null` values from a column explicitly - for example when no `null` values are present or these values are not relevant in the result.</span></span> <span data-ttu-id="9aa2a-133">Рассмотрим следующий пример.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-133">Consider the following example:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/NullSemantics/Program.cs#ManualOptimization)]

<span data-ttu-id="9aa2a-134">Эти запросы приведут к созданию следующего кода SQL.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-134">These queries produce the following SQL:</span></span>

```sql
SELECT [e].[Id], [e].[Int], [e].[NullableInt], [e].[String1], [e].[String2]
FROM [Entities] AS [e]
WHERE ((([e].[String1] <> [e].[String2]) OR ([e].[String1] IS NULL OR [e].[String2] IS NULL)) AND ([e].[String1] IS NOT NULL OR [e].[String2] IS NOT NULL)) OR ((CAST(LEN([e].[String1]) AS int) = CAST(LEN([e].[String2]) AS int)) OR ([e].[String1] IS NULL AND [e].[String2] IS NULL))

SELECT [e].[Id], [e].[Int], [e].[NullableInt], [e].[String1], [e].[String2]
FROM [Entities] AS [e]
WHERE ([e].[String1] IS NOT NULL AND [e].[String2] IS NOT NULL) AND (([e].[String1] <> [e].[String2]) OR (CAST(LEN([e].[String1]) AS int) = CAST(LEN([e].[String2]) AS int)))
```

<span data-ttu-id="9aa2a-135">Во втором запросе результаты `null` явно отфильтровываются из столбца `String1`.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-135">In the second query, `null` results are filtered out from `String1` column explicitly.</span></span> <span data-ttu-id="9aa2a-136">Во время сравнения EF Core может безопасно обрабатывать столбец `String1` как не допускающий значения NULL, что приводит к выполнению более простого запроса.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-136">EF Core can safely treat the `String1` column as non-nullable during comparison, resulting in a simpler query.</span></span>

## <a name="using-relational-null-semantics"></a><span data-ttu-id="9aa2a-137">Использование реляционной семантики значений NULL</span><span class="sxs-lookup"><span data-stu-id="9aa2a-137">Using relational null semantics</span></span>

<span data-ttu-id="9aa2a-138">При необходимости можно отключить компенсацию сравнения значений NULL и напрямую использовать реляционную семантику значений NULL.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-138">It's possible to disable the null comparison compensation and use relational null semantics directly.</span></span> <span data-ttu-id="9aa2a-139">Это можно сделать, вызвав метод `UseRelationalNulls(true)` в построителе параметров в методе `OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-139">This can be done by calling `UseRelationalNulls(true)` method on the options builder inside `OnConfiguring` method:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/NullSemantics/NullSemanticsContext.cs#UseRelationalNulls)]

> [!WARNING]
> <span data-ttu-id="9aa2a-140">При использовании реляционной семантики значений NULL запросы LINQ будут иметь значение, отличное от C#, и могут возвращать не те результаты, которые ожидались.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-140">When using relational null semantics, your LINQ queries no longer have the same meaning as they do in C#, and may yield different results than expected.</span></span> <span data-ttu-id="9aa2a-141">Используйте этот режим с осторожностью.</span><span class="sxs-lookup"><span data-stu-id="9aa2a-141">Exercise caution when using this mode.</span></span>
