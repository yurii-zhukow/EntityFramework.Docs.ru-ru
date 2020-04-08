---
title: Необработанные SQL-запросы — EF Core
author: smitpatel
ms.date: 10/08/2019
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: a54bb67c0fce9d621382f6372e70fe4cdca48a20
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413717"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="fb0e5-102">Необработанные SQL-запросы</span><span class="sxs-lookup"><span data-stu-id="fb0e5-102">Raw SQL Queries</span></span>

<span data-ttu-id="fb0e5-103">Entity Framework Core позволяет вам переходить на уровень необработанных SQL-запросов при работе с реляционной базой данных.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="fb0e5-104">Необработанные SQL-запросы полезны, если запрос невозможно выразить с помощью LINQ.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-104">Raw SQL queries are useful if the query you want can't be expressed using LINQ.</span></span> <span data-ttu-id="fb0e5-105">Они также полезны, если использование запроса LINQ приводит к отправке неэффективного SQL-запроса.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-105">Raw SQL queries are also used if using a LINQ query is resulting in an inefficient SQL query.</span></span> <span data-ttu-id="fb0e5-106">Необработанные SQL-запросы могут возвращать обычные типы сущностей или [типы сущностей без ключей](xref:core/modeling/keyless-entity-types), которые являются частью модели.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-106">Raw SQL queries can return regular entity types or [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="fb0e5-107">Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-107">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="fb0e5-108">Основные необработанные SQL-запросы</span><span class="sxs-lookup"><span data-stu-id="fb0e5-108">Basic raw SQL queries</span></span>

<span data-ttu-id="fb0e5-109">Вы можете использовать метод расширения `FromSqlRaw`, чтобы начать запрос LINQ на основе необработанного SQL-запроса.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-109">You can use the `FromSqlRaw` extension method to begin a LINQ query based on a raw SQL query.</span></span> <span data-ttu-id="fb0e5-110">Метод `FromSqlRaw` можно использовать только в корневых элементах запроса непосредственно в `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-110">`FromSqlRaw` can only be used on query roots, that is directly on the `DbSet<>`.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

<span data-ttu-id="fb0e5-111">Необработанные SQL-запросы могут использоваться для выполнения хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-111">Raw SQL queries can be used to execute a stored procedure.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a><span data-ttu-id="fb0e5-112">Передача параметров</span><span class="sxs-lookup"><span data-stu-id="fb0e5-112">Passing parameters</span></span>

> [!WARNING]
> <span data-ttu-id="fb0e5-113">**Всегда используйте параметризацию для необработанных SQL-запросов.**</span><span class="sxs-lookup"><span data-stu-id="fb0e5-113">**Always use parameterization for raw SQL queries**</span></span>
>
> <span data-ttu-id="fb0e5-114">При добавлении предоставленных пользователем значений в необработанный SQL-запрос необходимо соблюдать осторожность, чтобы не исключить атаки путем внедрения кода SQL.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-114">When introducing any user-provided values into a raw SQL query, care must be taken to avoid SQL injection attacks.</span></span> <span data-ttu-id="fb0e5-115">Помимо проверки отсутствия в этих значениях недопустимых символов, всегда следует использовать параметризацию, которая отправляет значения отдельно от текста SQL.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-115">In addition to validating that such values don't contain invalid characters, always use parameterization which sends the values separate from the SQL text.</span></span>
>
> <span data-ttu-id="fb0e5-116">В частности, никогда не передавайте объединенную или интерполированную строку (`$""`) с непроверенными предоставленными пользователем значениями непосредственно в `FromSqlRaw` или `ExecuteSqlRaw`.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-116">In particular, never pass a concatenated or interpolated string (`$""`) with non-validated user-provided values into `FromSqlRaw` or `ExecuteSqlRaw`.</span></span> <span data-ttu-id="fb0e5-117">Методы `FromSqlInterpolated` и `ExecuteSqlInterpolated` позволяют использовать синтаксис интерполяции строк таким способом, который обеспечивает защиту от атак путем внедрения кода SQL.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-117">The `FromSqlInterpolated` and `ExecuteSqlInterpolated` methods allow using string interpolation syntax in a way that protects against SQL injection attacks.</span></span>

<span data-ttu-id="fb0e5-118">В следующем примере показана передача одного параметра в хранимую процедуру путем включения заполнителя параметра в строку SQL-запроса и предоставления дополнительного аргумента.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-118">The following example passes a single parameter to a stored procedure by including a parameter placeholder in the SQL query string and providing an additional argument.</span></span> <span data-ttu-id="fb0e5-119">Хотя такой синтаксис может выглядеть как синтаксис `String.Format`, предоставленное значение упаковано в `DbParameter` и созданное имя параметра вставляется вместо указанного заполнителя `{0}`.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-119">While this syntax may look like `String.Format` syntax, the supplied value is wrapped in a `DbParameter` and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

<span data-ttu-id="fb0e5-120">Метод `FromSqlInterpolated` похож на `FromSqlRaw`, но позволяет использовать синтаксис интерполяции строк.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-120">`FromSqlInterpolated` is similar to `FromSqlRaw` but allows you to use string interpolation syntax.</span></span> <span data-ttu-id="fb0e5-121">Как и `FromSqlRaw`, метод `FromSqlInterpolated` можно использовать только в корневых элементах запроса.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-121">Just like `FromSqlRaw`, `FromSqlInterpolated` can only be used on query roots.</span></span> <span data-ttu-id="fb0e5-122">Как и в предыдущем примере, значение преобразуется в `DbParameter` и не является уязвимым к внедрению кода SQL.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-122">As with the previous example, the value is converted to a `DbParameter` and isn't vulnerable to SQL injection.</span></span>

> [!NOTE]
> <span data-ttu-id="fb0e5-123">До версии 3.0 `FromSqlRaw` и `FromSqlInterpolated` представляли собой две перегрузки `FromSql`.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-123">Prior to version 3.0, `FromSqlRaw` and `FromSqlInterpolated` were two overloads named `FromSql`.</span></span> <span data-ttu-id="fb0e5-124">Дополнительные сведения см. в разделе [Предыдущие версии](#previous-versions).</span><span class="sxs-lookup"><span data-stu-id="fb0e5-124">For more information, see the [previous versions section](#previous-versions).</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

<span data-ttu-id="fb0e5-125">Вы также можете создать DbParameter и указать его как значение параметра.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-125">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="fb0e5-126">Поскольку используется обычный заполнитель параметра SQL, а не заполнитель строки, можно применить `FromSqlRaw`.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-126">Since a regular SQL parameter placeholder is used, rather than a string placeholder, `FromSqlRaw` can be safely used:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

<span data-ttu-id="fb0e5-127">Метод `FromSqlRaw` позволяет использовать именованные параметры в строке SQL-запроса, что полезно, если у хранимой процедуры есть необязательные параметры.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-127">`FromSqlRaw` allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a><span data-ttu-id="fb0e5-128">Создание с помощью LINQ</span><span class="sxs-lookup"><span data-stu-id="fb0e5-128">Composing with LINQ</span></span>

<span data-ttu-id="fb0e5-129">Метод compose можно использовать поверх исходного необработанного SQL-запроса с помощью операторов LINQ.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-129">You can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="fb0e5-130">EF Core будет обрабатывать его как вложенный запрос и производить составление на его основе в базе данных.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-130">EF Core will treat it as subquery and compose over it in the database.</span></span> <span data-ttu-id="fb0e5-131">В приведенном ниже примере используется необработанный SQL-запрос, который выбирает одну из функций с табличным значением.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-131">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF).</span></span> <span data-ttu-id="fb0e5-132">Затем запрос выполняет составление на ее основе с использованием LINQ для фильтрации и сортировки.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-132">And then composes on it using LINQ to do filtering and sorting.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

<span data-ttu-id="fb0e5-133">Приведенный выше запрос создает следующий код SQL:</span><span class="sxs-lookup"><span data-stu-id="fb0e5-133">Above query generates following SQL:</span></span>

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM (
    SELECT * FROM dbo.SearchBlogs(@p0)
) AS [b]
WHERE [b].[Rating] > 3
ORDER BY [b].[Rating] DESC
```

### <a name="including-related-data"></a><span data-ttu-id="fb0e5-134">Включение связанных данных</span><span class="sxs-lookup"><span data-stu-id="fb0e5-134">Including related data</span></span>

<span data-ttu-id="fb0e5-135">Метод `Include` можно использовать для включения связанных данных, как с любым другим запросом LINQ:</span><span class="sxs-lookup"><span data-stu-id="fb0e5-135">The `Include` method can be used to include related data, just like with any other LINQ query:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedInclude)]

<span data-ttu-id="fb0e5-136">Для составления с помощью LINQ необходимо, чтобы необработанный SQL-запрос поддерживал составление, так как платформа EF Core будет обрабатывать предоставленный код SQL как вложенный запрос.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-136">Composing with LINQ requires your raw SQL query to be composable since EF Core will treat the supplied SQL as a subquery.</span></span> <span data-ttu-id="fb0e5-137">SQL-запросы, поверх которых можно использовать метод compose, содержат ключевое слово `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-137">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span> <span data-ttu-id="fb0e5-138">Кроме того, что переданный SQL-запрос не должен содержать символы и параметры, которые не допускаются во вложенных запросах, такие как:</span><span class="sxs-lookup"><span data-stu-id="fb0e5-138">Further, SQL passed shouldn't contain any characters or options that aren't valid on a subquery, such as:</span></span>

- <span data-ttu-id="fb0e5-139">конечная точка с запятой;</span><span class="sxs-lookup"><span data-stu-id="fb0e5-139">A trailing semicolon</span></span>
- <span data-ttu-id="fb0e5-140">на сервере SQL Server конечное указание на уровне запроса (например, `OPTION (HASH JOIN)`);</span><span class="sxs-lookup"><span data-stu-id="fb0e5-140">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
- <span data-ttu-id="fb0e5-141">на сервере SQL Server предложение `ORDER BY`, которое не используется с `OFFSET 0` или `TOP 100 PERCENT` в предложении `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-141">On SQL Server, an `ORDER BY` clause that isn't used with `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

<span data-ttu-id="fb0e5-142">SQL Server не допускает составления вызовов хранимых процедур, поэтому любая попытка применить дополнительные операторы запроса к такому вызову приведет к формированию недопустимого SQL.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-142">SQL Server doesn't allow composing over stored procedure calls, so any attempt to apply additional query operators to such a call will result in invalid SQL.</span></span> <span data-ttu-id="fb0e5-143">Используйте метод `AsEnumerable` или `AsAsyncEnumerable` сразу после метода `FromSqlRaw` или `FromSqlInterpolated`, чтобы платформа EF Core не пыталась выполнить составление для хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-143">Use `AsEnumerable` or `AsAsyncEnumerable` method right after `FromSqlRaw` or `FromSqlInterpolated` methods to make sure that EF Core doesn't try to compose over a stored procedure.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="fb0e5-144">Отслеживание изменений</span><span class="sxs-lookup"><span data-stu-id="fb0e5-144">Change Tracking</span></span>

<span data-ttu-id="fb0e5-145">Запросы, использующие методы `FromSqlRaw` или `FromSqlInterpolated`, подчиняются тем же правилам отслеживания изменений, что и любой другой запрос LINQ в EF Core.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-145">Queries that use the `FromSqlRaw` or `FromSqlInterpolated` methods follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="fb0e5-146">Например, если запрос проецирует типы сущностей, результаты будут отслеживаться по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-146">For example, if the query projects entity types, the results will be tracked by default.</span></span>

<span data-ttu-id="fb0e5-147">В следующем примере используется необработанный SQL-запрос, который выбирает одну из функций с табличным значением, а затем отключает отслеживание изменений с помощью вызова `AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="fb0e5-147">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to `AsNoTracking`:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a><span data-ttu-id="fb0e5-148">Ограничения</span><span class="sxs-lookup"><span data-stu-id="fb0e5-148">Limitations</span></span>

<span data-ttu-id="fb0e5-149">Есть несколько ограничений, которые следует учитывать при использовании необработанных SQL-запросов:</span><span class="sxs-lookup"><span data-stu-id="fb0e5-149">There are a few limitations to be aware of when using raw SQL queries:</span></span>

- <span data-ttu-id="fb0e5-150">SQL-запрос должен возвращать данные для всех свойств типа сущности.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-150">The SQL query must return data for all properties of the entity type.</span></span>
- <span data-ttu-id="fb0e5-151">Имена столбцов в результирующем наборе должны совпадать с именами столбцов, с которыми сопоставляются свойства.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-151">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="fb0e5-152">Обратите внимание, что это поведение отличается от EF6.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-152">Note this behavior is different from EF6.</span></span> <span data-ttu-id="fb0e5-153">В EF6 сопоставление свойства со столбцом игнорировалось для необработанных SQL-запросов, а имена столбцов в результирующем наборе должны были соответствовать именам свойств.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-153">EF6 ignored property to column mapping for raw SQL queries and result set column names had to match the property names.</span></span>
- <span data-ttu-id="fb0e5-154">SQL-запрос не может содержать связанные данные.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-154">The SQL query can't contain related data.</span></span> <span data-ttu-id="fb0e5-155">Однако во многих случаях вы можете использовать метод compose поверх запроса с помощью оператора `Include` для возврата связанных данных (см. раздел [Включение связанных данных](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="fb0e5-155">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

## <a name="previous-versions"></a><span data-ttu-id="fb0e5-156">Предыдущие версии</span><span class="sxs-lookup"><span data-stu-id="fb0e5-156">Previous versions</span></span>

<span data-ttu-id="fb0e5-157">В EF Core версии 2.2 и более ранних версиях существовали две перегрузки метода `FromSql`, действие которых было аналогично поведению новых `FromSqlRaw` и `FromSqlInterpolated`.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-157">EF Core version 2.2 and earlier had two overloads of method named `FromSql`, which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span></span> <span data-ttu-id="fb0e5-158">Из-за этого можно было легко случайно вызвать метод необработанной строки в тех случаях, когда требовалось вызвать метод интерполированной строки, и наоборот.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-158">It was easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span> <span data-ttu-id="fb0e5-159">Непреднамеренный вызов неправильной перегрузки мог привести к тому, что запросы не параметризовались, хотя должны были.</span><span class="sxs-lookup"><span data-stu-id="fb0e5-159">Calling wrong overload accidentally could result in queries not being parameterized when they should have been.</span></span>
