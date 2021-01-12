---
title: Сопоставление пользовательских функций — EF Core
description: Сопоставление пользовательских функций с функциями базы данных
author: maumar
ms.date: 11/23/2020
uid: core/user-defined-function-mapping
ms.openlocfilehash: ba60abdc9c81b34b8f4ed8f501cf2f7e52ba9d7d
ms.sourcegitcommit: 4860d036ea0fb392c28799907bcc924c987d2d7b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97657702"
---
# <a name="user-defined-function-mapping"></a>Сопоставление определяемых пользователем функций

EF Core позволяет использовать в запросах пользовательские функции SQL. Для этого во время настройки модели необходимо сопоставить эти функции с методом CLR. При преобразовании запроса LINQ в SQL пользовательская функция вызывается вместо сопоставленной с ней функции CLR.

## <a name="mapping-a-method-to-a-sql-function"></a>Сопоставление метода с функцией SQL

Чтобы продемонстрировать, как работает сопоставление пользовательских функций, давайте определим следующие сущности:

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#Entities)]

и следующую конфигурацию модели:

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#EntityConfiguration)]

В блоге может быть много записей, и в каждой записи может быть много комментариев.

Далее создадим пользовательскую функцию `CommentedPostCountForBlog`, которая возвращает количество записей хотя бы с одним комментарием в конкретном блоге на основе `Id` блога:

```sql
CREATE FUNCTION dbo.CommentedPostCountForBlog(@id int)
RETURNS int
AS
BEGIN
    RETURN (SELECT COUNT(*)
        FROM [Posts] AS [p]
        WHERE ([p].[BlogId] = @id) AND ((
            SELECT COUNT(*)
            FROM [Comments] AS [c]
            WHERE [p].[PostId] = [c].[PostId]) > 0));
END
```

Чтобы использовать эту функцию в EF Core, определим следующий метод CLR, который сопоставим с пользовательской функцией:

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#BasicFunctionDefinition)]

В данном случае тело метода CLR не имеет значения. Метод будет вызываться на стороне клиента только в том случае, если EF Core не может преобразовать его аргументы. Если аргументы можно преобразовать, EF Core проверяет только сигнатуру метода.

> [!NOTE]
> В этом примере метод определяется в `DbContext`, но его также можно определить как статический метод в других классах.

Теперь это определение функции можно связать с пользовательской функцией в конфигурации модели:

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#BasicFunctionConfiguration)]

По умолчанию EF Core пытается сопоставить функцию CLR с пользовательской функцией с тем же именем. Если имена этих функций разные, с помощью `HasName` можно указать правильное имя пользовательской функции, с которой необходимо выполнить сопоставление.

Теперь выполним следующий запрос:

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Program.cs#BasicQuery)]

Этот запрос создаст следующий код SQL:

```sql
SELECT [b].[BlogId], [b].[Rating], [b].[Url]
FROM [Blogs] AS [b]
WHERE [dbo].[CommentedPostCountForBlog]([b].[BlogId]) > 1
```

## <a name="mapping-a-method-to-a-custom-sql"></a>Сопоставление метода с настраиваемым кодом SQL

В EF Core также разрешены пользовательские функции, которые преобразуются в определенный код SQL. Выражение SQL предоставляется с помощью метода `HasTranslation` при настройке пользовательской функции.

В приведенном ниже примере создается функция, которая вычисляет разницу в процентах между двумя целыми числами.

Метод CLR выглядит следующим образом:

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#HasTranslationFunctionDefinition)]

Определение функции выглядит следующим образом:

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#HasTranslationFunctionConfiguration)]

После определения функции ее можно использовать в запросе. Вместо вызова функции базы данных EF Core будет преобразовывать тело метода непосредственно в код SQL, исходя из дерева выражений SQL, построенного на основе HasTranslation. Следующий запрос LINQ:

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Program.cs#HasTranslationQuery)]

Преобразуется в следующий запрос SQL:

```sql
SELECT 100 * (ABS(CAST([p].[BlogId] AS float) - 3) / ((CAST([p].[BlogId] AS float) + 3) / 2))
FROM [Posts] AS [p]
```

## <a name="mapping-a-queryable-function-to-a-table-valued-function"></a>Сопоставление функции, поддерживающей запросы, с функцией с табличным значением

EF Core также поддерживает сопоставление с функцией с табличным значением с помощью пользовательского метода CLR, возвращающего `IQueryable` типов сущностей, что позволяет EF Core сопоставлять функции с табличным значением (TVF) с параметрами. Этот процесс аналогичен сопоставлению скалярной пользовательской функции с функцией SQL: нам нужна функция с табличным значением в базе данных, функция CLR, используемая в запросах LINQ, и сопоставление между ними.

В качестве примера мы будем использовать функцию с табличным значением, которая возвращает все записи блога, имеющие хотя бы один комментарий, количество отметок "Нравится" которого соответствует заданному пороговому значению:

```sql
CREATE FUNCTION dbo.PostsWithPopularComments(@likeThreshold int)
RETURNS TABLE
AS
RETURN
(
    SELECT [p].[PostId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
    FROM [Posts] AS [p]
    WHERE (
        SELECT COUNT(*)
        FROM [Comments] AS [c]
        WHERE ([p].[PostId] = [c].[PostId]) AND ([c].[Likes] >= @likeThreshold)) > 0
)
```

Сигнатура метода CLR выглядит следующим образом:

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#QueryableFunctionDefinition)]

> [!TIP]
> Вызов `FromExpression` в теле функции CLR позволяет использовать эту функцию вместо обычного класса DbSet.

Ниже приведено сопоставление.

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#QueryableFunctionConfigurationHasDbFunction)]

> [!CAUTION]
> Пока [проблема 23408](https://github.com/dotnet/efcore/issues/23408) не исправлена, сопоставление с `IQueryable` типов сущностей переопределяет сопоставление по умолчанию с таблицей для DbSet. При необходимости, например в случае, когда сущность не имеет ключей, сопоставление с таблицей должно быть задано явно с помощью метода `ToTable`.

> [!NOTE]
> Функция, поддерживающая запросы, должна быть сопоставлена с функцией с табличным значением и не может использовать `HasTranslation`.

После сопоставления функции следующий запрос:

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Program.cs#TableValuedFunctionQuery)]

преобразуется в следующий код:

```sql
SELECT [p].[PostId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [dbo].[PostsWithPopularComments](@likeThreshold) AS [p]
ORDER BY [p].[Rating]
```
