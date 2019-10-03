---
title: Необработанные SQL-запросы — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: d8f52edfdf4bd7776ab8d81185c867cbfd7bcf44
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813596"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="39760-102">Необработанные SQL-запросы</span><span class="sxs-lookup"><span data-stu-id="39760-102">Raw SQL Queries</span></span>

<span data-ttu-id="39760-103">Entity Framework Core позволяет вам переходить на уровень необработанных SQL-запросов при работе с реляционной базой данных.</span><span class="sxs-lookup"><span data-stu-id="39760-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="39760-104">Это может быть полезно, если запрос, который вы хотите выполнить, не может быть выражен с помощью LINQ или если использование запроса LINQ приводит к отправке неэффективного SQL-запроса.</span><span class="sxs-lookup"><span data-stu-id="39760-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in an inefficient SQL query.</span></span> <span data-ttu-id="39760-105">Необработанные SQL-запросы могут возвращать обычные типы сущностей или [типы сущностей без ключей](xref:core/modeling/keyless-entity-types), которые являются частью модели.</span><span class="sxs-lookup"><span data-stu-id="39760-105">Raw SQL queries can return regular entity types or [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="39760-106">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="39760-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="39760-107">Основные необработанные SQL-запросы</span><span class="sxs-lookup"><span data-stu-id="39760-107">Basic raw SQL queries</span></span>

<span data-ttu-id="39760-108">Вы можете использовать метод расширения `FromSqlRaw`, чтобы начать запрос LINQ на основе необработанного SQL-запроса.</span><span class="sxs-lookup"><span data-stu-id="39760-108">You can use the `FromSqlRaw` extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="39760-109">Необработанные SQL-запросы могут использоваться для выполнения хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="39760-109">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="39760-110">Передача параметров</span><span class="sxs-lookup"><span data-stu-id="39760-110">Passing parameters</span></span>

> [!WARNING]
> <span data-ttu-id="39760-111">**Всегда используйте параметризацию для необработанных SQL-запросов.**</span><span class="sxs-lookup"><span data-stu-id="39760-111">**Always use parameterization for raw SQL queries**</span></span>
>
> <span data-ttu-id="39760-112">При добавлении предоставленных пользователем значений в необработанный SQL-запрос необходимо соблюдать осторожность, чтобы не исключить атаки путем внедрения кода SQL.</span><span class="sxs-lookup"><span data-stu-id="39760-112">When introducing any user-provided values into a raw SQL query, care must be taken to avoid SQL injection attacks.</span></span> <span data-ttu-id="39760-113">Помимо проверки отсутствия в этих значениях недопустимых символов, всегда следует использовать параметризацию, которая отправляет значения отдельно от текста SQL.</span><span class="sxs-lookup"><span data-stu-id="39760-113">In addition to validating that such values don't contain invalid characters, always use parameterization which sends the values separate from the SQL text.</span></span>
>
> <span data-ttu-id="39760-114">В частности, никогда не передавайте объединенную или интерполированную строку (`$""`) с непроверенными предоставленными пользователем значениями в `FromSqlRaw` или `ExecuteSqlRaw`.</span><span class="sxs-lookup"><span data-stu-id="39760-114">In particular, never pass a concatenated or interpolated string (`$""`) with unvalidated user-provided values into `FromSqlRaw` or `ExecuteSqlRaw`.</span></span> <span data-ttu-id="39760-115">Методы `FromSqlInterpolated` и `ExecuteSqlInterpolated` позволяют использовать синтаксис интерполяции строк таким способом, который обеспечивает защиту от атак путем внедрения кода SQL.</span><span class="sxs-lookup"><span data-stu-id="39760-115">The `FromSqlInterpolated` and `ExecuteSqlInterpolated` methods allow using string interpolation syntax in a way that protects against SQL injection attacks.</span></span>

<span data-ttu-id="39760-116">В следующем примере показана передача одного параметра в хранимую процедуру путем включения заполнителя параметра в строку SQL-запроса и предоставления дополнительного аргумента.</span><span class="sxs-lookup"><span data-stu-id="39760-116">The following example passes a single parameter to a stored procedure by including a parameter placeholder in the SQL query string and providing an additional argument.</span></span> <span data-ttu-id="39760-117">Хотя это может выглядеть как синтаксис `String.Format`, предоставленное значение упаковано в `DbParameter` и созданное имя параметра вставляется вместо указанного заполнителя `{0}`.</span><span class="sxs-lookup"><span data-stu-id="39760-117">While this may look like `String.Format` syntax, the supplied value is wrapped in a `DbParameter` and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="39760-118">В качестве альтернативы `FromSqlRaw` можно использовать `FromSqlInterpolated` для безопасного применения интерполяции строки.</span><span class="sxs-lookup"><span data-stu-id="39760-118">As an alternative to `FromSqlRaw`, you can use `FromSqlInterpolated` which allows the safe use of string interpolation.</span></span> <span data-ttu-id="39760-119">Как и в предыдущем примере, значение преобразуется в `DbParameter` и, следовательно, не является уязвимым к внедрению кода SQL.</span><span class="sxs-lookup"><span data-stu-id="39760-119">As with the previous example, the value is converted to a `DbParameter` and is therefore not vulnerable to SQL injection:</span></span>

> [!NOTE]
> <span data-ttu-id="39760-120">До версии 3.0 `FromSqlRaw` и `FromSqlInterpolated` представляли собой две перегрузки `FromSql`.</span><span class="sxs-lookup"><span data-stu-id="39760-120">Prior to version 3.0, `FromSqlRaw` and `FromSqlInterpolated` were two overloads named `FromSql`.</span></span> <span data-ttu-id="39760-121">Дополнительные сведения см. в [разделе о предыдущих версиях](#previous-versions).</span><span class="sxs-lookup"><span data-stu-id="39760-121">See the [previous versions section](#previous-versions) for more details.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlInterpolated($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="39760-122">Вы также можете создать DbParameter и указать его как значение параметра.</span><span class="sxs-lookup"><span data-stu-id="39760-122">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="39760-123">Поскольку используется обычный заполнитель параметра SQL, а не заполнитель строки, можно применить `FromSqlRaw`.</span><span class="sxs-lookup"><span data-stu-id="39760-123">Since a regular SQL parameter placeholder is used, rather than a string placeholder, `FromSqlRaw` can be safely used:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

<span data-ttu-id="39760-124">Благодаря этому можно использовать именованные параметры в строке SQL-запроса, что полезно, если у хранимой процедуры есть необязательные параметры.</span><span class="sxs-lookup"><span data-stu-id="39760-124">This allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="39760-125">Создание с помощью LINQ</span><span class="sxs-lookup"><span data-stu-id="39760-125">Composing with LINQ</span></span>

<span data-ttu-id="39760-126">Если SQL-запрос может быть составлен в базе данных, вы можете использовать метод compose поверх исходного необработанного SQL-запроса с помощью операторов LINQ.</span><span class="sxs-lookup"><span data-stu-id="39760-126">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="39760-127">SQL-запросы, поверх которых можно использовать метод compose, содержат ключевое слово `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="39760-127">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span>

<span data-ttu-id="39760-128">В следующем примере используется необработанный SQL-запрос, который выбирает из функции с табличным значением (TVF), а затем выполняет компоновку на ее основе с использованием LINQ для выполнения фильтрации и сортировки.</span><span class="sxs-lookup"><span data-stu-id="39760-128">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

<span data-ttu-id="39760-129">Будет создан следующий SQL-запрос:</span><span class="sxs-lookup"><span data-stu-id="39760-129">This will produce the following SQL query:</span></span>

``` sql
SELECT [b].[Id], [b].[Name], [b].[Rating]
        FROM (
            SELECT * FROM dbo.SearchBlogs(@p0)
        ) AS b
        WHERE b."Rating" > 3
        ORDER BY b."Rating" DESC
```

## <a name="change-tracking"></a><span data-ttu-id="39760-130">Отслеживание изменений</span><span class="sxs-lookup"><span data-stu-id="39760-130">Change Tracking</span></span>

<span data-ttu-id="39760-131">Запросы, использующие методы `FromSql`, следуют тем же правилам отслеживания изменений, что и любой другой запрос LINQ в EF Core.</span><span class="sxs-lookup"><span data-stu-id="39760-131">Queries that use the `FromSql` methods follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="39760-132">Например, если запрос проецирует типы сущностей, результаты будут отслеживаться по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="39760-132">For example, if the query projects entity types, the results will be tracked by default.</span></span>

<span data-ttu-id="39760-133">В следующем примере используется необработанный SQL-запрос, который выбирает одну из функций с табличным значением, а затем отключает отслеживание изменений с помощью вызова `AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="39760-133">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to `AsNoTracking`:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a><span data-ttu-id="39760-134">Включение связанных данных</span><span class="sxs-lookup"><span data-stu-id="39760-134">Including related data</span></span>

<span data-ttu-id="39760-135">Метод `Include` можно использовать для включения связанных данных, как с любым другим запросом LINQ:</span><span class="sxs-lookup"><span data-stu-id="39760-135">The `Include` method can be used to include related data, just like with any other LINQ query:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

<span data-ttu-id="39760-136">Обратите внимание, что для этого требуется необработанный SQL-запрос с поддержкой составления. В противном случае он не будет работать с вызовами хранимых процедур.</span><span class="sxs-lookup"><span data-stu-id="39760-136">Note that this requires your raw SQL query to be composable; it will notably not work with stored procedure calls.</span></span> <span data-ttu-id="39760-137">Примечания о составлении см. в разделе [Ограничения](#limitations).</span><span class="sxs-lookup"><span data-stu-id="39760-137">See notes on composability under [Limitations](#limitations)).</span></span>

## <a name="limitations"></a><span data-ttu-id="39760-138">Ограничения</span><span class="sxs-lookup"><span data-stu-id="39760-138">Limitations</span></span>

<span data-ttu-id="39760-139">Есть несколько ограничений, которые следует учитывать при использовании необработанных SQL-запросов:</span><span class="sxs-lookup"><span data-stu-id="39760-139">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="39760-140">SQL-запрос должен возвращать данные для всех свойств типа сущности.</span><span class="sxs-lookup"><span data-stu-id="39760-140">The SQL query must return data for all properties of the entity type.</span></span>

* <span data-ttu-id="39760-141">Имена столбцов в результирующем наборе должны совпадать с именами столбцов, с которыми сопоставляются свойства.</span><span class="sxs-lookup"><span data-stu-id="39760-141">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="39760-142">Обратите внимание, что это отличается от EF6, где сопоставление свойств или столбцов игнорировалось для необработанных SQL-запросов, а имена столбцов в результирующем наборе должны были соответствовать именам свойств.</span><span class="sxs-lookup"><span data-stu-id="39760-142">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="39760-143">SQL-запрос не может содержать связанные данные.</span><span class="sxs-lookup"><span data-stu-id="39760-143">The SQL query cannot contain related data.</span></span> <span data-ttu-id="39760-144">Однако во многих случаях вы можете использовать метод compose поверх запроса с помощью оператора `Include` для возврата связанных данных (см. раздел [Включение связанных данных](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="39760-144">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="39760-145">Инструкции `SELECT`, переданные этому методу, как правило, должны быть составными: если EF Core вычисляет дополнительные операторы запросов на сервере (например, для преобразования операторов LINQ, применяемых после методов `FromSql`), передаваемый SQL-запрос будет рассматриваться как вложенный запрос.</span><span class="sxs-lookup"><span data-stu-id="39760-145">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql` methods), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="39760-146">Это означает, что переданный SQL-запрос не должен содержать символы и параметры, которые не допускаются во вложенных запросах, такие как:</span><span class="sxs-lookup"><span data-stu-id="39760-146">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="39760-147">конечная точка с запятой;</span><span class="sxs-lookup"><span data-stu-id="39760-147">A trailing semicolon</span></span>
  * <span data-ttu-id="39760-148">на сервере SQL Server конечное указание на уровне запроса (например, `OPTION (HASH JOIN)`);</span><span class="sxs-lookup"><span data-stu-id="39760-148">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="39760-149">на сервере SQL Server предложение `ORDER BY`, которое не сопровождается `OFFSET 0` или `TOP 100 PERCENT` в предложении `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="39760-149">On SQL Server, an `ORDER BY` clause that is not accompanied of `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="39760-150">Обратите внимание, что SQL Server не допускает составления вызовов хранимых процедур, поэтому любая попытка применить дополнительные операторы запроса к такому вызову приведет к формированию недопустимого SQL.</span><span class="sxs-lookup"><span data-stu-id="39760-150">Note that SQL Server does not allow composing over stored procedure calls, so any attempt to apply additional query operators to such a call will result in invalid SQL.</span></span> <span data-ttu-id="39760-151">Операторы запроса могут быть добавлены после `AsEnumerable()` для выполнения в клиенте.</span><span class="sxs-lookup"><span data-stu-id="39760-151">Query operators may be introduced after `AsEnumerable()` for client evaluation.</span></span>

## <a name="previous-versions"></a><span data-ttu-id="39760-152">Предыдущие версии</span><span class="sxs-lookup"><span data-stu-id="39760-152">Previous versions</span></span>

<span data-ttu-id="39760-153">В EF Core версии 2.2 и более ранних версиях существовали две перегрузки `FromSql`, действие которых было аналогично поведению новых `FromSqlRaw` и `FromSqlInterpolated`.</span><span class="sxs-lookup"><span data-stu-id="39760-153">EF Core version 2.2 and earlier had two overloads named `FromSql` which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span></span> <span data-ttu-id="39760-154">Это приводило к случайному вызову метода необработанной строки в тех случаях, когда требовалось вызвать метод интерполированной строки, и наоборот.</span><span class="sxs-lookup"><span data-stu-id="39760-154">This made it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span> <span data-ttu-id="39760-155">Это может привести к тому, что запросы не параметризуются, хотя они должны параметризоваться.</span><span class="sxs-lookup"><span data-stu-id="39760-155">This could result in queries not being parameterized when they should have been.</span></span>
