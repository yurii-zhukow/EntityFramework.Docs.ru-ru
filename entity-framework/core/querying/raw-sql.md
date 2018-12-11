---
title: Необработанные SQL-запросы — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 0ad9731840c5f72064f2f66932b9867a0144f437
ms.sourcegitcommit: 2da6f9b05e1ce3a46491e5cc68f17758bdeb6b02
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2018
ms.locfileid: "53006873"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="db0de-102">Необработанные SQL-запросы</span><span class="sxs-lookup"><span data-stu-id="db0de-102">Raw SQL Queries</span></span>

<span data-ttu-id="db0de-103">Entity Framework Core позволяет вам переходить на уровень необработанных SQL-запросов при работе с реляционной базой данных.</span><span class="sxs-lookup"><span data-stu-id="db0de-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="db0de-104">Это может быть полезно, если запрос, который вы хотите выполнить, не может быть выражен с помощью LINQ или если использование запроса LINQ приводит к отправке неэффективных SQL-запросов.</span><span class="sxs-lookup"><span data-stu-id="db0de-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL queries.</span></span> <span data-ttu-id="db0de-105">Необработанные SQL-запросы могут возвращать типы сущностей или, начиная с EF Core 2.1, [типы запросов](xref:core/modeling/query-types), которые являются частью модели.</span><span class="sxs-lookup"><span data-stu-id="db0de-105">Raw SQL queries can return entity types or, starting with EF Core 2.1, [query types](xref:core/modeling/query-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="db0de-106">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="db0de-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="limitations"></a><span data-ttu-id="db0de-107">Ограничения</span><span class="sxs-lookup"><span data-stu-id="db0de-107">Limitations</span></span>

<span data-ttu-id="db0de-108">Есть несколько ограничений, которые следует учитывать при использовании необработанных SQL-запросов:</span><span class="sxs-lookup"><span data-stu-id="db0de-108">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="db0de-109">SQL-запрос должен возвращать данные для всех свойств сущности или типа запроса.</span><span class="sxs-lookup"><span data-stu-id="db0de-109">The SQL query must return data for all properties of the entity or query type.</span></span>

* <span data-ttu-id="db0de-110">Имена столбцов в результирующем наборе должны совпадать с именами столбцов, с которыми сопоставляются свойства.</span><span class="sxs-lookup"><span data-stu-id="db0de-110">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="db0de-111">Обратите внимание, что это отличается от EF6, где сопоставление свойств или столбцов игнорировалось для необработанных SQL-запросов, а имена столбцов в результирующем наборе должны были соответствовать именам свойств.</span><span class="sxs-lookup"><span data-stu-id="db0de-111">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="db0de-112">SQL-запрос не может содержать связанные данные.</span><span class="sxs-lookup"><span data-stu-id="db0de-112">The SQL query cannot contain related data.</span></span> <span data-ttu-id="db0de-113">Однако во многих случаях вы можете использовать метод compose поверх запроса с помощью оператора `Include` для возврата связанных данных (см. раздел [Включение связанных данных](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="db0de-113">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="db0de-114">Операторы `SELECT`, переданные этому методу, обычно должны быть составными: если EF Core должен вычислять дополнительные операторы запросов на сервере (например, для преобразования операторов LINQ, применяемых после `FromSql`), передаваемый SQL-запрос будет рассматриваться как вложенный запрос.</span><span class="sxs-lookup"><span data-stu-id="db0de-114">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql`), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="db0de-115">Это означает, что переданный SQL-запрос не должен содержать символы и параметры, которые не допускаются во вложенных запросах, такие как:</span><span class="sxs-lookup"><span data-stu-id="db0de-115">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="db0de-116">конечная точка с запятой;</span><span class="sxs-lookup"><span data-stu-id="db0de-116">a trailing semicolon</span></span>
  * <span data-ttu-id="db0de-117">на сервере SQL Server конечное указание на уровне запроса (например, `OPTION (HASH JOIN)`);</span><span class="sxs-lookup"><span data-stu-id="db0de-117">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="db0de-118">на сервере SQL Server предложение `ORDER BY`, которое не сопровождается `TOP 100 PERCENT` в предложении `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="db0de-118">On SQL Server, an `ORDER BY` clause that is not accompanied of `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="db0de-119">Операторы SQL, отличные от `SELECT`, автоматически распознаются как несоставные.</span><span class="sxs-lookup"><span data-stu-id="db0de-119">SQL statements other than `SELECT` are recognized automatically as non-composable.</span></span> <span data-ttu-id="db0de-120">Как следствие, полные результаты хранимых процедур всегда возвращаются клиенту, а любые операторы LINQ, применяемые после `FromSql`, вычисляются в памяти.</span><span class="sxs-lookup"><span data-stu-id="db0de-120">As a consequence, the full results of stored procedures are always returned to the client and any LINQ operators applied after `FromSql` are evaluated in-memory.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="db0de-121">Основные необработанные SQL-запросы</span><span class="sxs-lookup"><span data-stu-id="db0de-121">Basic raw SQL queries</span></span>

<span data-ttu-id="db0de-122">Вы можете использовать метод расширения *FromSql*, чтобы начать запрос LINQ на основе необработанного SQL-запроса.</span><span class="sxs-lookup"><span data-stu-id="db0de-122">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="db0de-123">Необработанные SQL-запросы могут использоваться для выполнения хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="db0de-123">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="db0de-124">Передача параметров</span><span class="sxs-lookup"><span data-stu-id="db0de-124">Passing parameters</span></span>

<span data-ttu-id="db0de-125">Как и для любого API, который принимает SQL, важно параметризовать любые входные данные пользователя для защиты от атак путем внедрения кода SQL.</span><span class="sxs-lookup"><span data-stu-id="db0de-125">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="db0de-126">Вы можете включить заполнители параметров в строку SQL-запроса, а затем указать значения параметров в качестве дополнительных аргументов.</span><span class="sxs-lookup"><span data-stu-id="db0de-126">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="db0de-127">Значения всех указанных параметров будут автоматически преобразованы в `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="db0de-127">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="db0de-128">В следующем примере в хранимую процедуру передается один параметр.</span><span class="sxs-lookup"><span data-stu-id="db0de-128">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="db0de-129">Хотя это может выглядеть как синтаксис `String.Format`, предоставленное значение упаковано в параметр и созданное имя параметра вставляется вместо указанного заполнителя `{0}`.</span><span class="sxs-lookup"><span data-stu-id="db0de-129">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="db0de-130">Это тот же запрос, но с использованием синтаксиса интерполяции строк, который поддерживается в EF Core версии 2.0 и выше:</span><span class="sxs-lookup"><span data-stu-id="db0de-130">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="db0de-131">Вы также можете создать DbParameter и указать его как значение параметра.</span><span class="sxs-lookup"><span data-stu-id="db0de-131">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="db0de-132">Это позволяет использовать именованные параметры в строке SQL-запроса.</span><span class="sxs-lookup"><span data-stu-id="db0de-132">This allows you to use named parameters in the SQL query string</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="db0de-133">Создание с помощью LINQ</span><span class="sxs-lookup"><span data-stu-id="db0de-133">Composing with LINQ</span></span>

<span data-ttu-id="db0de-134">Если SQL-запрос может быть составлен в базе данных, вы можете использовать метод compose поверх исходного необработанного SQL-запроса с помощью операторов LINQ.</span><span class="sxs-lookup"><span data-stu-id="db0de-134">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="db0de-135">SQL-запросы, поверх которых можно использовать метод compose, содержат ключевое слово `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="db0de-135">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span>

<span data-ttu-id="db0de-136">В следующем примере используется необработанный SQL-запрос, который выбирает из функции с табличным значением (TVF), а затем выполняет компоновку на ее основе с использованием LINQ для выполнения фильтрации и сортировки.</span><span class="sxs-lookup"><span data-stu-id="db0de-136">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a><span data-ttu-id="db0de-137">Включение связанных данных</span><span class="sxs-lookup"><span data-stu-id="db0de-137">Including related data</span></span>

<span data-ttu-id="db0de-138">Компоновка с операторами LINQ может использоваться для включения связанных данных в запрос.</span><span class="sxs-lookup"><span data-stu-id="db0de-138">Composing with LINQ operators can be used to include related data in the query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> <span data-ttu-id="db0de-139">**Всегда используйте параметризацию для необработанных SQL-запросов.** API, которые принимают необработанную строку SQL, такую ​​как `FromSql` и `ExecuteSqlCommand`, позволяют легко передавать значения в качестве параметров.</span><span class="sxs-lookup"><span data-stu-id="db0de-139">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="db0de-140">В дополнение к проверке вводимых пользователем данных всегда используйте параметры для любых значений, используемых в необработанном SQL-запросе или команде.</span><span class="sxs-lookup"><span data-stu-id="db0de-140">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="db0de-141">Если вы используете объединение строк для динамической сборки любой части строки запроса, вам необходимо проверить все входные данные для защиты от атак путем внедрения кода SQL.</span><span class="sxs-lookup"><span data-stu-id="db0de-141">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
