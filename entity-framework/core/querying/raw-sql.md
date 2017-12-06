---
title: "Необработанный SQL-запросы - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: ddf3a841800684688d50dcf9323f4d83c851222f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="raw-sql-queries"></a><span data-ttu-id="148d0-102">Необработанный SQL-запросов</span><span class="sxs-lookup"><span data-stu-id="148d0-102">Raw SQL Queries</span></span>

<span data-ttu-id="148d0-103">Entity Framework Core можно перейти вниз необработанных запросов SQL, при работе с реляционной базой данных.</span><span class="sxs-lookup"><span data-stu-id="148d0-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="148d0-104">Это полезно в том случае, если запрос, который требуется выполнить, не могут быть выражены с помощью LINQ, или с помощью запроса LINQ в результате чего неэффективным SQL, отправляемых в базе данных.</span><span class="sxs-lookup"><span data-stu-id="148d0-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL being sent to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="148d0-105">Можно просмотреть в этой статье [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) на GitHub.</span><span class="sxs-lookup"><span data-stu-id="148d0-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="limitations"></a><span data-ttu-id="148d0-106">Ограничения</span><span class="sxs-lookup"><span data-stu-id="148d0-106">Limitations</span></span>

<span data-ttu-id="148d0-107">Существует ряд ограничений, которые следует учитывать при использовании необработанных запросов SQL.</span><span class="sxs-lookup"><span data-stu-id="148d0-107">There are a couple of limitations to be aware of when using raw SQL queries:</span></span>
* <span data-ttu-id="148d0-108">Запросы SQL могут использоваться только для возврата типов сущностей, которые являются частью модели.</span><span class="sxs-lookup"><span data-stu-id="148d0-108">SQL queries can only be used to return entity types that are part of your model.</span></span> <span data-ttu-id="148d0-109">Отсутствует расширение на нашем невыполненной работы к [enable возвращение специальных типов из необработанных запросов SQL](https://github.com/aspnet/EntityFramework/issues/1862).</span><span class="sxs-lookup"><span data-stu-id="148d0-109">There is an enhancement on our backlog to [enable returning ad-hoc types from raw SQL queries](https://github.com/aspnet/EntityFramework/issues/1862).</span></span>

* <span data-ttu-id="148d0-110">SQL-запрос должен возвращать данные для всех свойств типа сущности.</span><span class="sxs-lookup"><span data-stu-id="148d0-110">The SQL query must return data for all properties of the entity type.</span></span>

* <span data-ttu-id="148d0-111">Имена столбцов в результирующем наборе должны соответствовать имена столбцов, которые сопоставляются свойства.</span><span class="sxs-lookup"><span data-stu-id="148d0-111">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="148d0-112">Обратите внимание, что это отличается от EF6, где сопоставления свойство столбец был пропущен в необработанные запросы SQL и пришлось имена совпадают с именами свойств столбца результирующего набора.</span><span class="sxs-lookup"><span data-stu-id="148d0-112">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="148d0-113">SQL-запроса не может содержать связанные данные.</span><span class="sxs-lookup"><span data-stu-id="148d0-113">The SQL query cannot contain related data.</span></span> <span data-ttu-id="148d0-114">Однако во многих случаях можно создать на основе запроса с помощью `Include` оператор для возврата связанных данных (в разделе [включая связанные данные](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="148d0-114">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="148d0-115">Базовые необработанные запросы SQL</span><span class="sxs-lookup"><span data-stu-id="148d0-115">Basic raw SQL queries</span></span>

<span data-ttu-id="148d0-116">Можно использовать *FromSql* метод расширения, чтобы начать на языке LINQ на основе необработанных запросов SQL.</span><span class="sxs-lookup"><span data-stu-id="148d0-116">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="148d0-117">Необработанные запросы SQL могут использоваться для выполнения хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="148d0-117">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="148d0-118">Передача параметров</span><span class="sxs-lookup"><span data-stu-id="148d0-118">Passing parameters</span></span>

<span data-ttu-id="148d0-119">Как и для любого API, который принимает SQL, важно параметризовать любой пользователь, входных данных для защиты от атак путем внедрения кода SQL.</span><span class="sxs-lookup"><span data-stu-id="148d0-119">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="148d0-120">Можно местозаполнителями параметров в строке запроса SQL, а затем укажите значения параметров, как дополнительные аргументы.</span><span class="sxs-lookup"><span data-stu-id="148d0-120">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="148d0-121">Значения всех параметров, указываемое будут автоматически преобразованы в `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="148d0-121">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="148d0-122">В следующем примере передается один параметр хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="148d0-122">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="148d0-123">Хотя это может выглядеть как `String.Format` синтаксис, предоставленного значения упаковывается в параметр "и" имя создаваемого параметра добавлены where `{0}` заполнитель был указан.</span><span class="sxs-lookup"><span data-stu-id="148d0-123">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="148d0-124">То же запрос, но с использованием интерполяции синтаксис строки, который поддерживается в EF Core 2.0 и более поздних версий:</span><span class="sxs-lookup"><span data-stu-id="148d0-124">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="148d0-125">Можно также создать DbParameter и предоставление его в качестве значения параметра.</span><span class="sxs-lookup"><span data-stu-id="148d0-125">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="148d0-126">Это позволяет использовать именованные параметры в строке запроса SQL</span><span class="sxs-lookup"><span data-stu-id="148d0-126">This allows you to use named parameters in the SQL query string</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="148d0-127">Составление с помощью LINQ</span><span class="sxs-lookup"><span data-stu-id="148d0-127">Composing with LINQ</span></span>

<span data-ttu-id="148d0-128">Если в запросе SQL можно создавать на базы данных, можно составить поверх исходный необработанный SQL-запрос с использованием операторов LINQ.</span><span class="sxs-lookup"><span data-stu-id="148d0-128">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="148d0-129">Запросы SQL, которые могут быть составлены на время с `SELECT` ключевое слово.</span><span class="sxs-lookup"><span data-stu-id="148d0-129">SQL queries that can be composed on being with the `SELECT` keyword.</span></span>

<span data-ttu-id="148d0-130">В следующем примере необработанные SQL-запроса, который выбирает из возвращающего табличное значение функции (TVF) и составляется с помощью LINQ для выполнения фильтрации и сортировки.</span><span class="sxs-lookup"><span data-stu-id="148d0-130">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a><span data-ttu-id="148d0-131">Включая связанные данные</span><span class="sxs-lookup"><span data-stu-id="148d0-131">Including related data</span></span>

<span data-ttu-id="148d0-132">Составление с операторами LINQ можно использовать для включения связанных данных в запросе.</span><span class="sxs-lookup"><span data-stu-id="148d0-132">Composing with LINQ operators can be used to include related data in the query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> <span data-ttu-id="148d0-133">**Всегда используйте параметризации для необработанных запросов SQL:** интерфейсы API, принимающие необработанные SQL строка, таких как `FromSql` и `ExecuteSqlCommand` позволяют легко передавать в качестве параметров значения.</span><span class="sxs-lookup"><span data-stu-id="148d0-133">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="148d0-134">В дополнение к проверке вводимых пользователем данных, для любого значения, используемые в необработанный команда SQL-запроса или всегда используйте параметризации.</span><span class="sxs-lookup"><span data-stu-id="148d0-134">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="148d0-135">Если вы используете объединение строк динамически создавать любой части строки запроса, а затем вы несете ответственность за проверку всех входных данных для защиты от атак путем внедрения кода SQL.</span><span class="sxs-lookup"><span data-stu-id="148d0-135">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
