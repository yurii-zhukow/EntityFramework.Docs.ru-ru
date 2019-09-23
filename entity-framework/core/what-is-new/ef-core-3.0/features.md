---
title: Новые возможности в EF Core 3.0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: 528733d6eec33de2c9538541a6ed5be704b9d433
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005555"
---
# <a name="new-features-included-in-ef-core-30"></a><span data-ttu-id="548a1-102">Новые возможности, добавленные в EF Core 3.0</span><span class="sxs-lookup"><span data-stu-id="548a1-102">New features included in EF Core 3.0</span></span>

<span data-ttu-id="548a1-103">В следующем списке приведены ключевые новые функции, запланированные для реализации в EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="548a1-103">The following list includes the major new features planned for EF Core 3.0.</span></span>

<span data-ttu-id="548a1-104">EF Core 3.0 — основной выпуск, который также содержит многочисленные [критические изменения](xref:core/what-is-new/ef-core-3.0/breaking-changes), касающиеся улучшений API, которые могут негативно повлиять на работу существующих приложений.</span><span class="sxs-lookup"><span data-stu-id="548a1-104">EF Core 3.0 is a major release and also contains numerous [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes), which are API improvements that may have negative impact on existing applications.</span></span>  

## <a name="linq-improvements"></a><span data-ttu-id="548a1-105">Улучшения LINQ</span><span class="sxs-lookup"><span data-stu-id="548a1-105">LINQ improvements</span></span> 

<span data-ttu-id="548a1-106">LINQ позволяет писать запросы к базам данных прямо в коде вашего языка программирования, используя различные типы форматированных данных, функции IntelliSense и проверку типа во время компиляции.</span><span class="sxs-lookup"><span data-stu-id="548a1-106">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to offer IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="548a1-107">Однако LINQ также поддерживает написание неограниченного числа сложных запросов, содержащих произвольные выражения (вызовы методов или операции).</span><span class="sxs-lookup"><span data-stu-id="548a1-107">But LINQ also enables you to write an unlimited number of complicated queries containing arbitrary expressions (method calls or operations).</span></span>
<span data-ttu-id="548a1-108">Обработка всех этих сочетаний всегда представляла непростую задачу для поставщиков LINQ.</span><span class="sxs-lookup"><span data-stu-id="548a1-108">Handling all those combinations has always been a significant challenge for LINQ providers.</span></span>
<span data-ttu-id="548a1-109">В EF Core 3.0 мы переписали свою реализацию LINQ, чтобы сделать возможным преобразование на язык SQL большего количества выражений, создать эффективные запросы в большем числе случаев, предотвратить неэффективные запросы, которые остаются нераспознанными, и упростить постепенное введение новых возможностей запросов и усовершенствований производительности без нарушения работы существующих приложений и поставщиков данных.</span><span class="sxs-lookup"><span data-stu-id="548a1-109">In EF Core 3.0, we've rewritten our LINQ implementation to enable translating more expressions into SQL, to generate efficient queries in more cases, to prevent inefficient queries from going undetected, and to make it easier for us to gradually introduce new query capabilities and performance improvementswithout breaking existing applications and data providers.</span></span>

### <a name="client-evaluation"></a><span data-ttu-id="548a1-110">Выполнение в клиенте</span><span class="sxs-lookup"><span data-stu-id="548a1-110">Client evaluation</span></span>

<span data-ttu-id="548a1-111">Основное изменение структуры в EF Core 3.0 связано с обработкой выражений LINQ, которые не могут быть преобразованы в SQL или параметры:</span><span class="sxs-lookup"><span data-stu-id="548a1-111">The main design change in EF Core 3.0 has to do with how it handles LINQ expressions that it cannot translate to SQL or parameters:</span></span>

<span data-ttu-id="548a1-112">В ранних версиях система EF Core просто определяла, какие части запроса можно преобразовать на язык SQL, и выполняла остальную часть запроса на клиенте.</span><span class="sxs-lookup"><span data-stu-id="548a1-112">In the first few versions, EF Core simply figured out what portions of a query could be translated to SQL, and executed the rest of the query on the client.</span></span>
<span data-ttu-id="548a1-113">Подобное выполнение на стороне клиента в некоторых ситуациях дает хороший результат, однако во многих случаях оно может привести к появлению неэффективных запросов.</span><span class="sxs-lookup"><span data-stu-id="548a1-113">This type of client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries.</span></span>
<span data-ttu-id="548a1-114">Например, если системе EF Core 2.2 не удавалось преобразовать предикат в вызов `Where()`, она выполняла инструкцию SQL без фильтра, считывала все строки из базы данных, а затем фильтровала их в памяти.</span><span class="sxs-lookup"><span data-stu-id="548a1-114">For example, if EF Core 2.2 couldn't translate a predicate in a `Where()` call, it executed a SQL statement without a filter, read all all the rows from the database, and then filtered them in-memory.</span></span>
<span data-ttu-id="548a1-115">Это может быть приемлемым при небольшом количестве строк в базе данных, но может привести к значительным проблемам с производительностью или даже сбою приложения, если база данных содержит большое число строк.</span><span class="sxs-lookup"><span data-stu-id="548a1-115">That may be acceptable if the database contains a small number of rows, but can result in significant performance issues or even application failure if the database contains a large number or rows.</span></span>
<span data-ttu-id="548a1-116">В EF Core 3.0 мы ограничили оценку клиентов, чтобы она осуществлялась только на проекции верхнего уровня (последний вызов `Select()`).</span><span class="sxs-lookup"><span data-stu-id="548a1-116">In EF Core 3.0 we have restricted client evaluation to only happen on the top-level projection (the last call to `Select()`).</span></span>
<span data-ttu-id="548a1-117">Когда система EF Core 3.0 обнаруживает выражения, которые невозможно преобразовать в любом другом месте запроса, она выдает исключение времени выполнения.</span><span class="sxs-lookup"><span data-stu-id="548a1-117">When EF Core 3.0 detects expressions that cannot be translated anywhere else in the query, it throws a runtime exception.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="548a1-118">Поддержка Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="548a1-118">Cosmos DB support</span></span> 

<span data-ttu-id="548a1-119">Поставщик Cosmos DB для EF Core позволяет разработчикам, знакомым с моделью программирования EF, легко использовать Azure Cosmos DB в качестве базы данных приложения.</span><span class="sxs-lookup"><span data-stu-id="548a1-119">The Cosmos DB provider for EF Core enables developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="548a1-120">Мы хотим, чтобы такие преимущества Cosmos DB, как глобальное распределение, постоянная готовность, эластичная масштабируемость и низкая задержка, стали еще доступнее .NET-разработчикам.</span><span class="sxs-lookup"><span data-stu-id="548a1-120">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="548a1-121">Поставщик позволит использовать API-интерфейс SQL в Cosmos DB с большинством функций EF Core, включая автоматическое отслеживание изменений, LINQ и преобразование значений.</span><span class="sxs-lookup"><span data-stu-id="548a1-121">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="548a1-122">Поддержка C# 8.0</span><span class="sxs-lookup"><span data-stu-id="548a1-122">C# 8.0 support</span></span>

<span data-ttu-id="548a1-123">EF Core 3.0 использует преимущества некоторых новых возможностей C# 8.0:</span><span class="sxs-lookup"><span data-stu-id="548a1-123">EF Core 3.0 takes advantage of some of the new features in C# 8.0:</span></span>

### <a name="asynchronous-streams"></a><span data-ttu-id="548a1-124">Асинхронные потоки</span><span class="sxs-lookup"><span data-stu-id="548a1-124">Asynchronous streams</span></span>

<span data-ttu-id="548a1-125">Асинхронные результаты запросов теперь предоставляются с помощью нового стандартного интерфейса `IAsyncEnumerable<T>` и могут использоваться с помощью `await foreach`.</span><span class="sxs-lookup"><span data-stu-id="548a1-125">Asynchronous query results are now exposed using the new standard `IAsyncEnumerable<T>` interface and can be consumed using `await foreach`.</span></span>

``` csharp
var orders = 
  from o in context.Orders
  where o.Status == OrderStatus.Pending
  select o;

await foreach(var o in orders)
{
  Proccess(o);
} 
```

### <a name="nullable-reference-types"></a><span data-ttu-id="548a1-126">Ссылочные типы, допускающие значение null</span><span class="sxs-lookup"><span data-stu-id="548a1-126">Nullable reference types</span></span> 

<span data-ttu-id="548a1-127">Если эта новая функция включена в коде, EF Core может делать заключения о допустимости значений NULL в свойствах ссылочных типов (примитивных типов, таких как строка, или свойств навигации), чтобы принять решение о допустимости значений NULL для столбцов и связей в базе данных.</span><span class="sxs-lookup"><span data-stu-id="548a1-127">When this new feature is enabled in your code, EF Core can reason about the nullability of properties of refrence types (either of primitive types like string or navigation properties) to decide the nullability of columns and relationships in the database.</span></span>

## <a name="interception"></a><span data-ttu-id="548a1-128">Interception</span><span class="sxs-lookup"><span data-stu-id="548a1-128">Interception</span></span>

<span data-ttu-id="548a1-129">Новый API перехвата в EF Core 3.0 позволяет программно наблюдать и изменять результат низкоуровневых операций с базами данных, выполняемых в рамках нормальной работы EF Core, например открытия соединений, инициации транзакций и выполнения команд.</span><span class="sxs-lookup"><span data-stu-id="548a1-129">The new interception API in EF Core 3.0 allows programatically observing and modifying the outcome of low-level database operations that occur as part of the normal operation of EF Core, such as opening connections, initating transactions, and executing commands.</span></span> 

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="548a1-130">Реконструирование представлений базы данных</span><span class="sxs-lookup"><span data-stu-id="548a1-130">Reverse engineering of database views</span></span>

<span data-ttu-id="548a1-131">Типы сущностей без ключей (ранее называемые [типами запросов](xref:core/modeling/query-types)) представляют данные, которые можно прочитать базе данных, но невозможно изменить.</span><span class="sxs-lookup"><span data-stu-id="548a1-131">Entity types without keys (previously known as [query types](xref:core/modeling/query-types)) represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="548a1-132">Благодаря этой особенности они отлично подходят для сопоставления представлений базы данных в большинстве сценариев, поэтому мы автоматизировали создание типов сущностей без ключей при реконструировании представлений базы данных.</span><span class="sxs-lookup"><span data-stu-id="548a1-132">This characteristic makes them an excellent fit for mapping database views in most scenarios, so we automated the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="548a1-133">Зависимые сущности, имеющие общую с субъектом таблицу, теперь являются необязательными</span><span class="sxs-lookup"><span data-stu-id="548a1-133">Dependent entities sharing the table with the principal are now optional</span></span>

<span data-ttu-id="548a1-134">Начиная с EF Core 3.0, если класс `OrderDetails` принадлежит классу `Order` или явно сопоставляется с той же таблицей, можно добавлять класс `Order` без класса `OrderDetails`, и все свойства `OrderDetails`, за исключением первичного ключа, будут сопоставляться со столбцами, допускающими значения NULL.</span><span class="sxs-lookup"><span data-stu-id="548a1-134">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="548a1-135">При отправке запроса EF Core задаст `OrderDetails` значение `null`, если какому-либо его обязательному свойству не задано значение или если отсутствуют необходимые свойства, помимо первичного ключа, и все свойства имеют значение `null`.</span><span class="sxs-lookup"><span data-stu-id="548a1-135">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

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

## <a name="ef-63-on-net-core"></a><span data-ttu-id="548a1-136">EF 6.3 на платформе .NET Core</span><span class="sxs-lookup"><span data-stu-id="548a1-136">EF 6.3 on .NET Core</span></span>

<span data-ttu-id="548a1-137">Мы понимаем, что многие существующие приложения используют предыдущие версии EF и перенос этих приложений на EF Core исключительно для поддержки функций .NET Core может оказаться очень трудоемким.</span><span class="sxs-lookup"><span data-stu-id="548a1-137">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="548a1-138">По этой причине мы адаптировали новую версию EF 6 к работе на платформе .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="548a1-138">For that reason, we have enabled the newewst version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="548a1-139">Однако имеются некоторые ограничения, например:</span><span class="sxs-lookup"><span data-stu-id="548a1-139">There are some limitations, for example:</span></span>
- <span data-ttu-id="548a1-140">Новые поставщики должны работать на .NET Core.</span><span class="sxs-lookup"><span data-stu-id="548a1-140">New providers are required to work on .NET Core</span></span>
- <span data-ttu-id="548a1-141">Поддержка пространственных индексов в SQL Server не будет реализована.</span><span class="sxs-lookup"><span data-stu-id="548a1-141">Spatial support with SQL Server won't be enabled</span></span>

## <a name="postponed-features"></a><span data-ttu-id="548a1-142">Отложенные функции</span><span class="sxs-lookup"><span data-stu-id="548a1-142">Postponed features</span></span>

<span data-ttu-id="548a1-143">Некоторые функции, первоначально запланированные для EF Core 3.0, были отложены до будущих выпусков:</span><span class="sxs-lookup"><span data-stu-id="548a1-143">Some features originally planned for EF Core 3.0 were postponed to future releases:</span></span> 

- <span data-ttu-id="548a1-144">Возможность пропускать части модели при миграции (вопрос [№ 2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725)).</span><span class="sxs-lookup"><span data-stu-id="548a1-144">Ability to ingore parts of a model in migrations, tracked by [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span></span>
- <span data-ttu-id="548a1-145">Сущности контейнера свойств, которым посвящены два отдельных вопроса: [№ 9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) об общих сущностях и [№ 13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) о поддержке сопоставления индексированных свойств.</span><span class="sxs-lookup"><span data-stu-id="548a1-145">Property bag entities, tracked by two separate issues: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) about shared-type entities and [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) about indexed property mapping support.</span></span>
