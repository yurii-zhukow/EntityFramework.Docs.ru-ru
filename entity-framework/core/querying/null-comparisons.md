---
title: Сравнения со значениями NULL в запросах
description: Сведения о том, как в Entity Framework Core обрабатываются сравнения со значениями NULL в запросах
author: maumar
ms.date: 11/11/2020
uid: core/querying/null-comparisons
ms.openlocfilehash: d1235eb8df7fd22c7a930b3661ec38a99f75e5fa
ms.sourcegitcommit: 032a1767d7a6e42052a005f660b80372c6521e7e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98129563"
---
# <a name="query-null-semantics"></a>Семантика значений NULL в запросах

## <a name="introduction"></a>Вступление

При выполнении сравнений базы данных SQL работают с 3-значной логикой (`true`, `false`, `null`), в отличие от C#, где для этих целей применяется бинарная логика. При преобразовании запросов LINQ в SQL EF Core пытается компенсировать это различие, вводя дополнительные проверки значений NULL для некоторых элементов запроса.
Чтобы продемонстрировать это, определим следующую сущность:

[!code-csharp[Main](../../../samples/core/Querying/NullSemantics/NullSemanticsEntity.cs#Entity)]

и выполним несколько запросов.

[!code-csharp[Main](../../../samples/core/Querying/NullSemantics/Program.cs#BasicExamples)]

В результате первых двух запросов выполняются простые сравнения. В первом запросе оба столбца не допускают значения NULL, в связи с чем проверка значений NULL не требуется. Во втором запросе `NullableInt` может содержать значение `null`, а `Id` не допускает такие значения. При сравнении `null` с отличным от NULL значением получается результат `null`, который будет отфильтрован операцией `WHERE`. Соответственно, не нужны никакие дополнительные условия.

```sql
SELECT [e].[Id], [e].[Int], [e].[NullableInt], [e].[String1], [e].[String2]
FROM [Entities] AS [e]
WHERE [e].[Id] = [e].[Int]

SELECT [e].[Id], [e].[Int], [e].[NullableInt], [e].[String1], [e].[String2]
FROM [Entities] AS [e]
WHERE [e].[Id] = [e].[NullableInt]
```

Третий запрос приводит к выполнению проверки значений NULL. Если `NullableInt` имеет значение `null`, при сравнении `Id <> NullableInt` получается результат `null`, который будет отфильтрован операцией `WHERE`. Тем не менее с точки зрения бинарной логики этот вариант должен возвращаться в составе результата. Поэтому EF Core добавляет соответствующую проверку, чтобы убедиться в этом.

```sql
SELECT [e].[Id], [e].[Int], [e].[NullableInt], [e].[String1], [e].[String2]
FROM [Entities] AS [e]
WHERE ([e].[Id] <> [e].[NullableInt]) OR [e].[NullableInt] IS NULL
```

Запросы 4 и 5 демонстрируют ситуацию, в которой оба столбца допускают значения NULL. Следует отметить, что операция `<>` выдает более сложный (и потенциально более медленный) запрос по сравнению с операцией `==`.

```sql
SELECT [e].[Id], [e].[Int], [e].[NullableInt], [e].[String1], [e].[String2]
FROM [Entities] AS [e]
WHERE ([e].[String1] = [e].[String2]) OR ([e].[String1] IS NULL AND [e].[String2] IS NULL)

SELECT [e].[Id], [e].[Int], [e].[NullableInt], [e].[String1], [e].[String2]
FROM [Entities] AS [e]
WHERE (([e].[String1] <> [e].[String2]) OR ([e].[String1] IS NULL OR [e].[String2] IS NULL)) AND ([e].[String1] IS NOT NULL OR [e].[String2] IS NOT NULL)
```

## <a name="treatment-of-nullable-values-in-functions"></a>Обработка значений, допускающих значения NULL, в функциях

Многие функции в SQL могут возвращать результат `null`, только если некоторые из их аргументов имеют значение `null`. EF Core использует преимущества этого подхода для создания более эффективных запросов.
Пример такой оптимизации показан в приведенном ниже запросе.

[!code-csharp[Main](../../../samples/core/Querying/NullSemantics/Program.cs#Functions)]

Созданный код SQL выглядит следующим образом (нам не нужно рассчитывать функцию `SUBSTRING`, так как она будет иметь значение NULL, только если какой-либо из ее аргументов имеет значение NULL).

```sql
SELECT [e].[Id], [e].[Int], [e].[NullableInt], [e].[String1], [e].[String2]
FROM [Entities] AS [e]
WHERE [e].[String1] IS NULL OR [e].[String2] IS NULL
```

Для определяемых пользователем функций такая оптимизация также возможна. Дополнительные сведения см. на странице [Сопоставление определяемых пользователем функций](xref:core/querying/user-defined-function-mapping#configuring-nullability-of-user-defined-function-based-on-its-arguments).

## <a name="writing-performant-queries"></a>Написание эффективных запросов

- Сравнение столбцов, не допускающих значения NULL, выполняется проще и быстрее, чем сравнение столбцов, допускающих такие значения. Соответственно, во всех возможных случаях рекомендуется помечать столбцы как не допускающие значения NULL.

- Проверка равенства (`==`) выполняется проще и быстрее, чем проверка неравенства (`!=`), так как запросу не требуется различать результаты `null` и `false`. Соответственно, во всех возможных случаях рекомендуется использовать сравнение на равенство. Обратите внимание, что простое отрицание сравнения `==` фактически аналогично сравнению `!=` и не приводит к повышению производительности.

- В некоторых случаях можно упростить сложное сравнение, явно отфильтровав значения `null` из столбца, например если значения `null` отсутствуют или не являются значимыми для результата. Рассмотрим следующий пример.

[!code-csharp[Main](../../../samples/core/Querying/NullSemantics/Program.cs#ManualOptimization)]

Эти запросы приведут к созданию следующего кода SQL.

```sql
SELECT [e].[Id], [e].[Int], [e].[NullableInt], [e].[String1], [e].[String2]
FROM [Entities] AS [e]
WHERE ((([e].[String1] <> [e].[String2]) OR ([e].[String1] IS NULL OR [e].[String2] IS NULL)) AND ([e].[String1] IS NOT NULL OR [e].[String2] IS NOT NULL)) OR ((CAST(LEN([e].[String1]) AS int) = CAST(LEN([e].[String2]) AS int)) OR ([e].[String1] IS NULL AND [e].[String2] IS NULL))

SELECT [e].[Id], [e].[Int], [e].[NullableInt], [e].[String1], [e].[String2]
FROM [Entities] AS [e]
WHERE ([e].[String1] IS NOT NULL AND [e].[String2] IS NOT NULL) AND (([e].[String1] <> [e].[String2]) OR (CAST(LEN([e].[String1]) AS int) = CAST(LEN([e].[String2]) AS int)))
```

Во втором запросе результаты `null` явно отфильтровываются из столбца `String1`. Во время сравнения EF Core может безопасно обрабатывать столбец `String1` как не допускающий значения NULL, что приводит к выполнению более простого запроса.

## <a name="using-relational-null-semantics"></a>Использование реляционной семантики значений NULL

При необходимости можно отключить компенсацию сравнения значений NULL и напрямую использовать реляционную семантику значений NULL. Это можно сделать, вызвав метод `UseRelationalNulls(true)` в построителе параметров в методе `OnConfiguring`.

[!code-csharp[Main](../../../samples/core/Querying/NullSemantics/NullSemanticsContext.cs#UseRelationalNulls)]

> [!WARNING]
> При использовании реляционной семантики значений NULL запросы LINQ будут иметь значение, отличное от C#, и могут возвращать не те результаты, которые ожидались. Используйте этот режим с осторожностью.
