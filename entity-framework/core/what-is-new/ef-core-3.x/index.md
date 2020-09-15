---
title: Новые возможности в Entity Framework Core 3.х — EF Core
description: Изменения и улучшения в Entity Framework Core 3.x
author: divega
ms.date: 09/05/2020
uid: core/what-is-new/ef-core-3.x/index
ms.openlocfilehash: d2c887640a9e24cef49fb469ef435d6b08937876
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90072217"
---
# <a name="new-features-in-entity-framework-core-3x"></a><span data-ttu-id="fa139-103">Новые возможности в Entity Framework Core 3.х</span><span class="sxs-lookup"><span data-stu-id="fa139-103">New features in Entity Framework Core 3.x</span></span>

<span data-ttu-id="fa139-104">В следующем списке указаны основные новые возможности EF Core 3.х.</span><span class="sxs-lookup"><span data-stu-id="fa139-104">The following list includes the major new features in EF Core 3.x</span></span>

<span data-ttu-id="fa139-105">EF Core 3.х — это основной выпуск. Он содержит несколько [критических изменений](xref:core/what-is-new/ef-core-3.x/breaking-changes) (улучшения API), которые могут негативно повлиять на работу существующих приложений.</span><span class="sxs-lookup"><span data-stu-id="fa139-105">As a major release, EF Core 3.x also contains several [breaking changes](xref:core/what-is-new/ef-core-3.x/breaking-changes), which are API improvements that may have negative impact on existing applications.</span></span>

## <a name="linq-overhaul"></a><span data-ttu-id="fa139-106">Перепроектирование LINQ</span><span class="sxs-lookup"><span data-stu-id="fa139-106">LINQ overhaul</span></span>

<span data-ttu-id="fa139-107">LINQ позволяет писать запросы к базам данных с помощью любого языка .NET, используя различные типы форматированных данных, функции IntelliSense и проверку типа во время компиляции.</span><span class="sxs-lookup"><span data-stu-id="fa139-107">LINQ enables you to write database queries using your .NET language of choice, taking advantage of rich type information to offer IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="fa139-108">Однако LINQ также поддерживает написание неограниченного числа сложных запросов, содержащих произвольные выражения (вызовы методов или операции).</span><span class="sxs-lookup"><span data-stu-id="fa139-108">But LINQ also enables you to write an unlimited number of complicated queries containing arbitrary expressions (method calls or operations).</span></span>
<span data-ttu-id="fa139-109">Обработка всех этих комбинаций — основная задача поставщиков LINQ.</span><span class="sxs-lookup"><span data-stu-id="fa139-109">How to handle all those combinations is the main challenge for LINQ providers.</span></span>

<span data-ttu-id="fa139-110">В EF Core 3.х мы переопределили поставщик LINQ, чтобы включить преобразование дополнительных шаблонов запросов в SQL. При этом в большинстве случаев создаются более эффективные запросы, а появление неэффективных предотвращается.</span><span class="sxs-lookup"><span data-stu-id="fa139-110">In EF Core 3.x, we rearchitected our LINQ provider to enable translating more query patterns into SQL, generating efficient queries in more cases, and preventing inefficient queries from going undetected.</span></span> <span data-ttu-id="fa139-111">Новый поставщик LINQ открывает больше возможностей для создания запросов и повышения производительности в будущих выпусках. Использование этого поставщика никак не повлияет на работу существующих приложений и поставщиков данных.</span><span class="sxs-lookup"><span data-stu-id="fa139-111">The new LINQ provider is the foundation over which we'll be able to offer new query capabilities and performance improvements in future releases, without breaking existing applications and data providers.</span></span>

### <a name="restricted-client-evaluation"></a><span data-ttu-id="fa139-112">Ограниченная оценка клиента</span><span class="sxs-lookup"><span data-stu-id="fa139-112">Restricted client evaluation</span></span>

<span data-ttu-id="fa139-113">Самое важное изменение разработки внесено в способ обработки выражений LINQ, которые нельзя преобразовать в параметры или в SQL.</span><span class="sxs-lookup"><span data-stu-id="fa139-113">The most important design change has to do with how we handle LINQ expressions that cannot be converted to parameters or translated to SQL.</span></span>

<span data-ttu-id="fa139-114">В предыдущих версиях система EF Core определяла, какие части запроса можно преобразовать в SQL, и выполняла остальную часть запроса в клиенте.</span><span class="sxs-lookup"><span data-stu-id="fa139-114">In previous versions, EF Core identified what portions of a query could be translated to SQL, and executed the rest of the query on the client.</span></span>
<span data-ttu-id="fa139-115">Подобное выполнение на стороне клиента иногда дает хороший результат. Но во многих случаях оно может привести к появлению неэффективных запросов.</span><span class="sxs-lookup"><span data-stu-id="fa139-115">This type of client-side execution is desirable in some situations, but in many other cases it can result in inefficient queries.</span></span>

<span data-ttu-id="fa139-116">Например, если системе EF Core 2.2 не удавалось преобразовать предикат в вызов `Where()`, она выполняла инструкцию SQL без фильтра, передавала все строки из базы данных, а затем фильтровала их в памяти:</span><span class="sxs-lookup"><span data-stu-id="fa139-116">For example, if EF Core 2.2 couldn't translate a predicate in a `Where()` call, it executed an SQL statement without a filter, transferred all the rows from the database, and then filtered them in-memory:</span></span>

``` csharp
var specialCustomers = context.Customers
    .Where(c => c.Name.StartsWith(n) && IsSpecialCustomer(c));
```

<span data-ttu-id="fa139-117">Это приемлемо при небольшом количестве строк в базе данных, но может привести к значительным проблемам с производительностью или даже сбою приложения, если база данных содержит большое число строк.</span><span class="sxs-lookup"><span data-stu-id="fa139-117">That may be acceptable if the database contains a small number of rows but can result in significant performance issues or even application failure if the database contains a large number of rows.</span></span>

<span data-ttu-id="fa139-118">В EF Core 3.х мы ограничили оценку клиентов, чтобы она осуществлялась только на проекции верхнего уровня (в основном последний вызов `Select()`).</span><span class="sxs-lookup"><span data-stu-id="fa139-118">In EF Core 3.x, we've restricted client evaluation to only happen on the top-level projection (essentially, the last call to `Select()`).</span></span>
<span data-ttu-id="fa139-119">Когда EF Core 3.х обнаруживает выражения, которые нельзя преобразовать в любой другой точке запроса, вызывается исключение среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="fa139-119">When EF Core 3.x detects expressions that can't be translated anywhere else in the query, it throws a runtime exception.</span></span>

<span data-ttu-id="fa139-120">Чтобы оценить условие предиката в клиенте (как в предыдущем примере), разработчикам теперь нужно явно переключить вычисление запроса на LINQ to Objects:</span><span class="sxs-lookup"><span data-stu-id="fa139-120">To evaluate a predicate condition on the client as in the previous example, developers now need to explicitly switch evaluation of the query to LINQ to Objects:</span></span>

``` csharp
var specialCustomers = context.Customers
    .Where(c => c.Name.StartsWith(n))
    .AsEnumerable() // switches to LINQ to Objects
    .Where(c => IsSpecialCustomer(c));
```

<span data-ttu-id="fa139-121">Дополнительные сведения о том, как это может повлиять на существующие приложения, см. в [документации по критическим изменениям](xref:core/what-is-new/ef-core-3.x/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client).</span><span class="sxs-lookup"><span data-stu-id="fa139-121">See the [breaking changes documentation](xref:core/what-is-new/ef-core-3.x/breaking-changes#linq-queries-are-no-longer-evaluated-on-the-client) for more details about how this can affect existing applications.</span></span>

### <a name="single-sql-statement-per-linq-query"></a><span data-ttu-id="fa139-122">Одна инструкция SQL для каждого запроса LINQ</span><span class="sxs-lookup"><span data-stu-id="fa139-122">Single SQL statement per LINQ query</span></span>

<span data-ttu-id="fa139-123">В версии 3.х существенно изменен еще один аспект разработки. Теперь для каждого запроса LINQ всегда создается отдельная инструкция SQL.</span><span class="sxs-lookup"><span data-stu-id="fa139-123">Another aspect of the design that changed significantly in 3.x is that we now always generate a single SQL statement per LINQ query.</span></span> <span data-ttu-id="fa139-124">В предыдущих версиях иногда создавалось несколько инструкций SQL. Например, для преобразования вызовов `Include()` к свойствам навигации коллекции и запросов на основе определенных шаблонов с вложенными запросами.</span><span class="sxs-lookup"><span data-stu-id="fa139-124">In previous versions, we used to generate multiple SQL statements in certain cases, translated `Include()` calls on collection navigation properties and translated queries that followed certain patterns with subqueries.</span></span> <span data-ttu-id="fa139-125">В некоторых случаях это было удобно, а для `Include()` даже помогало предотвратить отправку избыточных данных по каналу. Но реализовать такой сценарий было сложно. Это приводило к неэффективному поведению (запросы N+1).</span><span class="sxs-lookup"><span data-stu-id="fa139-125">Although this was in some cases convenient, and for `Include()` it even helped avoid sending redundant data over the wire, the implementation was complex, and it resulted in some extremely inefficient behaviors (N+1 queries).</span></span> <span data-ttu-id="fa139-126">Иногда после нескольких запросов возвращались несогласованные данные.</span><span class="sxs-lookup"><span data-stu-id="fa139-126">There were situations in which the data returned across multiple queries was potentially inconsistent.</span></span>

<span data-ttu-id="fa139-127">Если EF Core 3.х не может преобразовать запрос LINQ в одну инструкцию SQL, вызывается исключение среды выполнения, как и при оценке клиента.</span><span class="sxs-lookup"><span data-stu-id="fa139-127">Similarly to client evaluation, if EF Core 3.x can't translate a LINQ query into a single SQL statement, it throws a runtime exception.</span></span> <span data-ttu-id="fa139-128">Но мы реализовали в EF Core возможность преобразовать много распространенных шаблонов, которые использовались для создания нескольких запросов, в один общий запрос с помощью инструкций JOIN.</span><span class="sxs-lookup"><span data-stu-id="fa139-128">But we made EF Core capable of translating many of the common patterns that used to generate multiple queries to a single query with JOINs.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="fa139-129">Поддержка Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="fa139-129">Cosmos DB support</span></span>

<span data-ttu-id="fa139-130">Поставщик Cosmos DB для EF Core позволяет разработчикам, знакомым с моделью программирования EF, легко использовать Azure Cosmos DB в качестве базы данных приложения.</span><span class="sxs-lookup"><span data-stu-id="fa139-130">The Cosmos DB provider for EF Core enables developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span> <span data-ttu-id="fa139-131">Мы хотим, чтобы такие преимущества Cosmos DB, как глобальное распределение, постоянная готовность, эластичная масштабируемость и низкая задержка, стали еще доступнее .NET-разработчикам.</span><span class="sxs-lookup"><span data-stu-id="fa139-131">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span> <span data-ttu-id="fa139-132">Поставщик позволяет использовать API-интерфейс SQL в Cosmos DB с большинством функций EF Core, включая автоматическое отслеживание изменений, LINQ и преобразование значений.</span><span class="sxs-lookup"><span data-stu-id="fa139-132">The provider enables most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>

<span data-ttu-id="fa139-133">Дополнительные сведения см. в [документации по поставщику Cosmos DB](xref:core/providers/cosmos/index).</span><span class="sxs-lookup"><span data-stu-id="fa139-133">See the [Cosmos DB provider documentation](xref:core/providers/cosmos/index) for more details.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="fa139-134">Поддержка C# 8.0</span><span class="sxs-lookup"><span data-stu-id="fa139-134">C# 8.0 support</span></span>

<span data-ttu-id="fa139-135">В EF Core 3.х используются преимущества некоторых [новых возможностей в C# 8.0](/dotnet/csharp/whats-new/csharp-8):</span><span class="sxs-lookup"><span data-stu-id="fa139-135">EF Core 3.x takes advantage of a couple of the [new features in C# 8.0](/dotnet/csharp/whats-new/csharp-8):</span></span>

### <a name="asynchronous-streams"></a><span data-ttu-id="fa139-136">Асинхронные потоки</span><span class="sxs-lookup"><span data-stu-id="fa139-136">Asynchronous streams</span></span>

<span data-ttu-id="fa139-137">Асинхронные результаты запросов теперь предоставляются с помощью нового стандартного интерфейса `IAsyncEnumerable<T>` и могут использоваться с помощью `await foreach`.</span><span class="sxs-lookup"><span data-stu-id="fa139-137">Asynchronous query results are now exposed using the new standard `IAsyncEnumerable<T>` interface and can be consumed using `await foreach`.</span></span>

``` csharp
var orders =
    from o in context.Orders
    where o.Status == OrderStatus.Pending
    select o;

await foreach(var o in orders.AsAsyncEnumerable())
{
    Process(o);
}
```

<span data-ttu-id="fa139-138">Дополнительные сведения см. в [документации по асинхронным потокам в C#](/dotnet/csharp/whats-new/csharp-8#asynchronous-streams).</span><span class="sxs-lookup"><span data-stu-id="fa139-138">See the [asynchronous streams in the C# documentation](/dotnet/csharp/whats-new/csharp-8#asynchronous-streams) for more details.</span></span>

### <a name="nullable-reference-types"></a><span data-ttu-id="fa139-139">Ссылочные типы, допускающие значение null</span><span class="sxs-lookup"><span data-stu-id="fa139-139">Nullable reference types</span></span>

<span data-ttu-id="fa139-140">Если эта новая функция включена в код, EF Core проверяет допустимость значений NULL в свойствах ссылочного типа и применяет ее к соответствующим столбцам и связям в базе данных. Свойства ссылочных типов, не допускающих значения NULL, обрабатываются так, как если бы в них использовался атрибут заметки к данным `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="fa139-140">When this new feature is enabled in your code, EF Core examines the nullability of reference type properties and applies it to corresponding columns and relationships in the database: properties of non-nullable references types are treated as if they had the `[Required]` data annotation attribute.</span></span>

<span data-ttu-id="fa139-141">Например, в указанном ниже классе свойства типа `string?` будут настроены как необязательные, а типа `string` — как обязательные:</span><span class="sxs-lookup"><span data-stu-id="fa139-141">For example, in the following class, properties marked as of type `string?` will be configured as optional, whereas `string` will be configured as required:</span></span>

``` csharp
public class Customer
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string? MiddleName { get; set; }
}
```

<span data-ttu-id="fa139-142">Дополнительные сведения см. в статье о [работе со ссылочными типами, допускающими значения NULL](xref:core/miscellaneous/nullable-reference-types), в документации по EF Core.</span><span class="sxs-lookup"><span data-stu-id="fa139-142">See [Working with nullable reference types](xref:core/miscellaneous/nullable-reference-types) in the EF Core documentation for more details.</span></span>

## <a name="interception-of-database-operations"></a><span data-ttu-id="fa139-143">Перехват операций базы данных</span><span class="sxs-lookup"><span data-stu-id="fa139-143">Interception of database operations</span></span>

<span data-ttu-id="fa139-144">Новый API перехвата в EF Core 3.х позволяет автоматически вызывать пользовательскую логику, когда в рамках нормальной работы EF Core выполняются низкоуровневые операции с базами данных.</span><span class="sxs-lookup"><span data-stu-id="fa139-144">The new interception API in EF Core 3.x allows providing custom logic to be invoked automatically whenever low-level database operations occur as part of the normal operation of EF Core.</span></span> <span data-ttu-id="fa139-145">Например, при открытии соединений, фиксации транзакций или выполнении команд.</span><span class="sxs-lookup"><span data-stu-id="fa139-145">For example, when opening connections, committing transactions, or executing commands.</span></span>

<span data-ttu-id="fa139-146">Как и функции перехвата, использовавшиеся в EF 6, перехватчики позволяют перехватывать операции до или после того, как они происходят.</span><span class="sxs-lookup"><span data-stu-id="fa139-146">Similarly to the interception features that existed in EF 6, interceptors allow you to intercept operations before or after they happen.</span></span> <span data-ttu-id="fa139-147">Если перехватить их до выполнения, можно обойти его и предоставить альтернативные результаты на основе логики перехвата.</span><span class="sxs-lookup"><span data-stu-id="fa139-147">When you intercept them before they happen, you are allowed to by-pass execution and supply alternate results from the interception logic.</span></span>

<span data-ttu-id="fa139-148">Например, для управления текстом команды можно создать `DbCommandInterceptor`:</span><span class="sxs-lookup"><span data-stu-id="fa139-148">For example, to manipulate command text, you can create a `DbCommandInterceptor`:</span></span>

``` csharp
public class HintCommandInterceptor : DbCommandInterceptor
{
    public override InterceptionResult<DbDataReader> ReaderExecuting(
        DbCommand command,
        CommandEventData eventData,
        InterceptionResult<DbDataReader> result)
    {
        // Manipulate the command text, etc. here...
        command.CommandText += " OPTION (OPTIMIZE FOR UNKNOWN)";
        return result;
    }
}
```

<span data-ttu-id="fa139-149">Зарегистрировать перехватчик можно с помощью  `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="fa139-149">And register it with your `DbContext`:</span></span>

``` csharp
services.AddDbContext(b => b
    .UseSqlServer(connectionString)
    .AddInterceptors(new HintCommandInterceptor()));
```

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="fa139-150">Реконструирование представлений базы данных</span><span class="sxs-lookup"><span data-stu-id="fa139-150">Reverse engineering of database views</span></span>

<span data-ttu-id="fa139-151">Типы запросов, представляющие данные, которые можно считать из базы данных, но нельзя обновить, переименованы в [типы сущностей без ключей](xref:core/modeling/keyless-entity-types).</span><span class="sxs-lookup"><span data-stu-id="fa139-151">Query types, which represent data that can be read from the database but not updated, have been renamed to [keyless entity types](xref:core/modeling/keyless-entity-types).</span></span>
<span data-ttu-id="fa139-152">Так как в большинстве случаев они отлично подходят для сопоставления представлений базы данных, EF Core теперь автоматически создает типы сущностей без ключей при реконструировании представлений баз данных.</span><span class="sxs-lookup"><span data-stu-id="fa139-152">As they are an excellent fit for mapping database views in most scenarios, EF Core now automatically creates keyless entity types when reverse engineering database views.</span></span>

<span data-ttu-id="fa139-153">Например, с помощью [программы командной строки dotnet для EF](xref:core/miscellaneous/cli/dotnet) можно ввести:</span><span class="sxs-lookup"><span data-stu-id="fa139-153">For example, using the [dotnet ef command-line tool](xref:core/miscellaneous/cli/dotnet) you can type:</span></span>

```dotnetcli
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="fa139-154">Теперь средство автоматически формирует типы шаблонов для представлений и таблиц без ключей:</span><span class="sxs-lookup"><span data-stu-id="fa139-154">And the tool will now automatically scaffold types for views and tables without keys:</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Names>(entity =>
    {
        entity.HasNoKey();
        entity.ToView("Names");
    });

    modelBuilder.Entity<Things>(entity =>
    {
        entity.HasNoKey();
    });
}
```

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="fa139-155">Зависимые сущности, имеющие общую с субъектом таблицу, теперь являются необязательными</span><span class="sxs-lookup"><span data-stu-id="fa139-155">Dependent entities sharing the table with the principal are now optional</span></span>

<span data-ttu-id="fa139-156">Начиная с EF Core 3.х, если `OrderDetails` принадлежит `Order` или явно сопоставляется с той же таблицей, можно добавлять `Order` без `OrderDetails`, и все свойства `OrderDetails`, за исключением первичного ключа, будут сопоставляться со столбцами, допускающими значения NULL.</span><span class="sxs-lookup"><span data-stu-id="fa139-156">Starting with EF Core 3.x, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="fa139-157">При отправке запроса EF Core задаст `OrderDetails` значение `null`, если какому-либо его обязательному свойству не задано значение или если отсутствуют необходимые свойства, помимо первичного ключа, и все свойства имеют значение `null`.</span><span class="sxs-lookup"><span data-stu-id="fa139-157">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

## <a name="ef-63-on-net-core"></a><span data-ttu-id="fa139-158">EF 6.3 на платформе .NET Core</span><span class="sxs-lookup"><span data-stu-id="fa139-158">EF 6.3 on .NET Core</span></span>

<span data-ttu-id="fa139-159">Это не совсем функция EF Core 3.х, но мы думаем, что такая возможность важна для многих наших клиентов.</span><span class="sxs-lookup"><span data-stu-id="fa139-159">This isn't really an EF Core 3.x feature, but we think it is important to many of our current customers.</span></span>

<span data-ttu-id="fa139-160">Мы понимаем, что многие существующие приложения используют предыдущие версии EF и перенос этих приложений на EF Core исключительно для поддержки функций .NET Core может оказаться очень трудоемким.</span><span class="sxs-lookup"><span data-stu-id="fa139-160">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can require a significant effort.</span></span>
<span data-ttu-id="fa139-161">По этой причине мы решили перенести новую версию EF 6 в .NET Core 3.х.</span><span class="sxs-lookup"><span data-stu-id="fa139-161">For that reason, we decided to port the newest version of EF 6 to run on .NET Core 3.x.</span></span>

<span data-ttu-id="fa139-162">Дополнительные сведения см. в статье о [новых возможностях EF 6](xref:ef6/what-is-new/index).</span><span class="sxs-lookup"><span data-stu-id="fa139-162">For more details, see [what's new in EF 6](xref:ef6/what-is-new/index).</span></span>

## <a name="postponed-features"></a><span data-ttu-id="fa139-163">Отложенные функции</span><span class="sxs-lookup"><span data-stu-id="fa139-163">Postponed features</span></span>

<span data-ttu-id="fa139-164">Некоторые функции, первоначально запланированные в EF Core 3.х, будут реализованы в будущих выпусках:</span><span class="sxs-lookup"><span data-stu-id="fa139-164">Some features originally planned for EF Core 3.x were postponed to future releases:</span></span>

- <span data-ttu-id="fa139-165">Возможность пропускать части модели при миграции (вопрос [№ 2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725)).</span><span class="sxs-lookup"><span data-stu-id="fa139-165">Ability to ignore parts of a model in migrations, tracked as [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span></span>
- <span data-ttu-id="fa139-166">Сущности контейнера свойств, которым посвящены два отдельных вопроса: [№ 9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) об общих сущностях и [№ 13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) о поддержке сопоставления индексированных свойств.</span><span class="sxs-lookup"><span data-stu-id="fa139-166">Property bag entities, tracked as two separate issues: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) about shared-type entities and [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) about indexed property mapping support.</span></span>
