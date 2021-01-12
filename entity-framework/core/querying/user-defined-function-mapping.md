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
# <a name="user-defined-function-mapping"></a><span data-ttu-id="6a3e2-103">Сопоставление определяемых пользователем функций</span><span class="sxs-lookup"><span data-stu-id="6a3e2-103">User-defined function mapping</span></span>

<span data-ttu-id="6a3e2-104">EF Core позволяет использовать в запросах пользовательские функции SQL.</span><span class="sxs-lookup"><span data-stu-id="6a3e2-104">EF Core allows for using user-defined SQL functions in queries.</span></span> <span data-ttu-id="6a3e2-105">Для этого во время настройки модели необходимо сопоставить эти функции с методом CLR.</span><span class="sxs-lookup"><span data-stu-id="6a3e2-105">To do that, the functions need to be mapped to a CLR method during model configuration.</span></span> <span data-ttu-id="6a3e2-106">При преобразовании запроса LINQ в SQL пользовательская функция вызывается вместо сопоставленной с ней функции CLR.</span><span class="sxs-lookup"><span data-stu-id="6a3e2-106">When translating the LINQ query to SQL, the user-defined function is called instead of the CLR function it has been mapped to.</span></span>

## <a name="mapping-a-method-to-a-sql-function"></a><span data-ttu-id="6a3e2-107">Сопоставление метода с функцией SQL</span><span class="sxs-lookup"><span data-stu-id="6a3e2-107">Mapping a method to a SQL function</span></span>

<span data-ttu-id="6a3e2-108">Чтобы продемонстрировать, как работает сопоставление пользовательских функций, давайте определим следующие сущности:</span><span class="sxs-lookup"><span data-stu-id="6a3e2-108">To illustrate how user-defined function mapping work, let's define the following entities:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#Entities)]

<span data-ttu-id="6a3e2-109">и следующую конфигурацию модели:</span><span class="sxs-lookup"><span data-stu-id="6a3e2-109">And the following model configuration:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#EntityConfiguration)]

<span data-ttu-id="6a3e2-110">В блоге может быть много записей, и в каждой записи может быть много комментариев.</span><span class="sxs-lookup"><span data-stu-id="6a3e2-110">Blog can have many posts and each post can have many comments.</span></span>

<span data-ttu-id="6a3e2-111">Далее создадим пользовательскую функцию `CommentedPostCountForBlog`, которая возвращает количество записей хотя бы с одним комментарием в конкретном блоге на основе `Id` блога:</span><span class="sxs-lookup"><span data-stu-id="6a3e2-111">Next, create the user-defined function `CommentedPostCountForBlog`, which returns the count of posts with at least one comment for a given blog, based on the blog `Id`:</span></span>

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

<span data-ttu-id="6a3e2-112">Чтобы использовать эту функцию в EF Core, определим следующий метод CLR, который сопоставим с пользовательской функцией:</span><span class="sxs-lookup"><span data-stu-id="6a3e2-112">To use this function in EF Core, we define the following CLR method, which we map to the user-defined function:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#BasicFunctionDefinition)]

<span data-ttu-id="6a3e2-113">В данном случае тело метода CLR не имеет значения.</span><span class="sxs-lookup"><span data-stu-id="6a3e2-113">The body of the CLR method is not important.</span></span> <span data-ttu-id="6a3e2-114">Метод будет вызываться на стороне клиента только в том случае, если EF Core не может преобразовать его аргументы.</span><span class="sxs-lookup"><span data-stu-id="6a3e2-114">The method will not be invoked client-side, unless EF Core can't translate its arguments.</span></span> <span data-ttu-id="6a3e2-115">Если аргументы можно преобразовать, EF Core проверяет только сигнатуру метода.</span><span class="sxs-lookup"><span data-stu-id="6a3e2-115">If the arguments can be translated, EF Core only cares about the method signature.</span></span>

> [!NOTE]
> <span data-ttu-id="6a3e2-116">В этом примере метод определяется в `DbContext`, но его также можно определить как статический метод в других классах.</span><span class="sxs-lookup"><span data-stu-id="6a3e2-116">In the example, the method is defined on `DbContext`, but it can also be defined as a static method inside other classes.</span></span>

<span data-ttu-id="6a3e2-117">Теперь это определение функции можно связать с пользовательской функцией в конфигурации модели:</span><span class="sxs-lookup"><span data-stu-id="6a3e2-117">This function definition can now be associated with user-defined function in the model configuration:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#BasicFunctionConfiguration)]

<span data-ttu-id="6a3e2-118">По умолчанию EF Core пытается сопоставить функцию CLR с пользовательской функцией с тем же именем.</span><span class="sxs-lookup"><span data-stu-id="6a3e2-118">By default, EF Core tries to map CLR function to a user-defined function with the same name.</span></span> <span data-ttu-id="6a3e2-119">Если имена этих функций разные, с помощью `HasName` можно указать правильное имя пользовательской функции, с которой необходимо выполнить сопоставление.</span><span class="sxs-lookup"><span data-stu-id="6a3e2-119">If the names differ, we can use `HasName` to provide the correct name for the user-defined function we want to map to.</span></span>

<span data-ttu-id="6a3e2-120">Теперь выполним следующий запрос:</span><span class="sxs-lookup"><span data-stu-id="6a3e2-120">Now, executing the following query:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Program.cs#BasicQuery)]

<span data-ttu-id="6a3e2-121">Этот запрос создаст следующий код SQL:</span><span class="sxs-lookup"><span data-stu-id="6a3e2-121">Will produce this SQL:</span></span>

```sql
SELECT [b].[BlogId], [b].[Rating], [b].[Url]
FROM [Blogs] AS [b]
WHERE [dbo].[CommentedPostCountForBlog]([b].[BlogId]) > 1
```

## <a name="mapping-a-method-to-a-custom-sql"></a><span data-ttu-id="6a3e2-122">Сопоставление метода с настраиваемым кодом SQL</span><span class="sxs-lookup"><span data-stu-id="6a3e2-122">Mapping a method to a custom SQL</span></span>

<span data-ttu-id="6a3e2-123">В EF Core также разрешены пользовательские функции, которые преобразуются в определенный код SQL.</span><span class="sxs-lookup"><span data-stu-id="6a3e2-123">EF Core also allows for user-defined functions that get converted to a specific SQL.</span></span> <span data-ttu-id="6a3e2-124">Выражение SQL предоставляется с помощью метода `HasTranslation` при настройке пользовательской функции.</span><span class="sxs-lookup"><span data-stu-id="6a3e2-124">The SQL expression is provided using `HasTranslation` method during user-defined function configuration.</span></span>

<span data-ttu-id="6a3e2-125">В приведенном ниже примере создается функция, которая вычисляет разницу в процентах между двумя целыми числами.</span><span class="sxs-lookup"><span data-stu-id="6a3e2-125">In the example below, we'll create a function that computes percentage difference between two integers.</span></span>

<span data-ttu-id="6a3e2-126">Метод CLR выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="6a3e2-126">The CLR method is as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#HasTranslationFunctionDefinition)]

<span data-ttu-id="6a3e2-127">Определение функции выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="6a3e2-127">The function definition is as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#HasTranslationFunctionConfiguration)]

<span data-ttu-id="6a3e2-128">После определения функции ее можно использовать в запросе.</span><span class="sxs-lookup"><span data-stu-id="6a3e2-128">Once we define the function, it can be used in the query.</span></span> <span data-ttu-id="6a3e2-129">Вместо вызова функции базы данных EF Core будет преобразовывать тело метода непосредственно в код SQL, исходя из дерева выражений SQL, построенного на основе HasTranslation.</span><span class="sxs-lookup"><span data-stu-id="6a3e2-129">Instead of calling database function, EF Core will translate the method body directly into SQL based on the SQL expression tree constructed from the HasTranslation.</span></span> <span data-ttu-id="6a3e2-130">Следующий запрос LINQ:</span><span class="sxs-lookup"><span data-stu-id="6a3e2-130">The following LINQ query:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Program.cs#HasTranslationQuery)]

<span data-ttu-id="6a3e2-131">Преобразуется в следующий запрос SQL:</span><span class="sxs-lookup"><span data-stu-id="6a3e2-131">Produces the following SQL:</span></span>

```sql
SELECT 100 * (ABS(CAST([p].[BlogId] AS float) - 3) / ((CAST([p].[BlogId] AS float) + 3) / 2))
FROM [Posts] AS [p]
```

## <a name="mapping-a-queryable-function-to-a-table-valued-function"></a><span data-ttu-id="6a3e2-132">Сопоставление функции, поддерживающей запросы, с функцией с табличным значением</span><span class="sxs-lookup"><span data-stu-id="6a3e2-132">Mapping a queryable function to a table-valued function</span></span>

<span data-ttu-id="6a3e2-133">EF Core также поддерживает сопоставление с функцией с табличным значением с помощью пользовательского метода CLR, возвращающего `IQueryable` типов сущностей, что позволяет EF Core сопоставлять функции с табличным значением (TVF) с параметрами.</span><span class="sxs-lookup"><span data-stu-id="6a3e2-133">EF Core also supports mapping to a table-valued function using a user-defined CLR method returning an `IQueryable` of entity types, allowing EF Core to map TVFs with parameters.</span></span> <span data-ttu-id="6a3e2-134">Этот процесс аналогичен сопоставлению скалярной пользовательской функции с функцией SQL: нам нужна функция с табличным значением в базе данных, функция CLR, используемая в запросах LINQ, и сопоставление между ними.</span><span class="sxs-lookup"><span data-stu-id="6a3e2-134">The process is similar to mapping a scalar user-defined function to a SQL function: we need a TVF in the database, a CLR function that is used in the LINQ queries, and a mapping between the two.</span></span>

<span data-ttu-id="6a3e2-135">В качестве примера мы будем использовать функцию с табличным значением, которая возвращает все записи блога, имеющие хотя бы один комментарий, количество отметок "Нравится" которого соответствует заданному пороговому значению:</span><span class="sxs-lookup"><span data-stu-id="6a3e2-135">As an example, we'll use a table-valued function that returns all posts having at least one comment that meets a given "Like" threshold:</span></span>

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

<span data-ttu-id="6a3e2-136">Сигнатура метода CLR выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="6a3e2-136">The CLR method signature is as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#QueryableFunctionDefinition)]

> [!TIP]
> <span data-ttu-id="6a3e2-137">Вызов `FromExpression` в теле функции CLR позволяет использовать эту функцию вместо обычного класса DbSet.</span><span class="sxs-lookup"><span data-stu-id="6a3e2-137">The `FromExpression` call in the CLR function body allows for the function to be used instead of a regular DbSet.</span></span>

<span data-ttu-id="6a3e2-138">Ниже приведено сопоставление.</span><span class="sxs-lookup"><span data-stu-id="6a3e2-138">And below is the mapping:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Model.cs#QueryableFunctionConfigurationHasDbFunction)]

> [!CAUTION]
> <span data-ttu-id="6a3e2-139">Пока [проблема 23408](https://github.com/dotnet/efcore/issues/23408) не исправлена, сопоставление с `IQueryable` типов сущностей переопределяет сопоставление по умолчанию с таблицей для DbSet.</span><span class="sxs-lookup"><span data-stu-id="6a3e2-139">Until [issue 23408](https://github.com/dotnet/efcore/issues/23408) is fixed, mapping to an `IQueryable` of entity types overrides the default mapping to a table for the DbSet.</span></span> <span data-ttu-id="6a3e2-140">При необходимости, например в случае, когда сущность не имеет ключей, сопоставление с таблицей должно быть задано явно с помощью метода `ToTable`.</span><span class="sxs-lookup"><span data-stu-id="6a3e2-140">If necessary - for example when the entity is not keyless - mapping to the table must be specified explicitly using `ToTable` method.</span></span>

> [!NOTE]
> <span data-ttu-id="6a3e2-141">Функция, поддерживающая запросы, должна быть сопоставлена с функцией с табличным значением и не может использовать `HasTranslation`.</span><span class="sxs-lookup"><span data-stu-id="6a3e2-141">Queryable function must be mapped to a table-valued function and can't use of `HasTranslation`.</span></span>

<span data-ttu-id="6a3e2-142">После сопоставления функции следующий запрос:</span><span class="sxs-lookup"><span data-stu-id="6a3e2-142">When the function is mapped, the following query:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/UserDefinedFunctionMapping/Program.cs#TableValuedFunctionQuery)]

<span data-ttu-id="6a3e2-143">преобразуется в следующий код:</span><span class="sxs-lookup"><span data-stu-id="6a3e2-143">Produces:</span></span>

```sql
SELECT [p].[PostId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [dbo].[PostsWithPopularComments](@likeThreshold) AS [p]
ORDER BY [p].[Rating]
```
