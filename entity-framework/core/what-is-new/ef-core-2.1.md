---
title: Новые возможности в EF Core 2.1 — EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: 16600ccbb1194d584fae15671118d9c046f1f637
ms.sourcegitcommit: 06073f8efde97dd5f540dbfb69f574d8380566fe
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/24/2019
ms.locfileid: "67333862"
---
# <a name="new-features-in-ef-core-21"></a><span data-ttu-id="4b66a-102">Новые возможности в EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="4b66a-102">New features in EF Core 2.1</span></span>

<span data-ttu-id="4b66a-103">Кроме исправления многочисленных ошибок и небольших улучшений функциональности и производительности, EF Core 2.1 содержит ряд новых интересных функций.</span><span class="sxs-lookup"><span data-stu-id="4b66a-103">Besides numerous bug fixes and small functional and performance enhancements, EF Core 2.1 includes some compelling new features:</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="4b66a-104">Отложенная загрузка</span><span class="sxs-lookup"><span data-stu-id="4b66a-104">Lazy loading</span></span>
<span data-ttu-id="4b66a-105">EF Core теперь содержит стандартные блоки, необходимые для создания классов сущностей, которые могут загружать свои свойства навигации по запросу.</span><span class="sxs-lookup"><span data-stu-id="4b66a-105">EF Core now contains the necessary building blocks for anyone to author entity classes that can load their navigation properties on demand.</span></span> <span data-ttu-id="4b66a-106">Мы также разработали новый пакет Microsoft.EntityFrameworkCore.Proxies, который использует эти стандартные блоки для создания прокси-классов отложенной загрузки на основе минимально измененных классов сущностей (например, классов с виртуальными свойствами навигации).</span><span class="sxs-lookup"><span data-stu-id="4b66a-106">We have also created a new package, Microsoft.EntityFrameworkCore.Proxies, that leverages those building blocks to produce lazy loading proxy classes based on minimally modified entity classes (for example, classes with virtual navigation properties).</span></span>

<span data-ttu-id="4b66a-107">Дополнительные сведения об этом см. в [разделе, посвященном отложенной загрузке](xref:core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="4b66a-107">Read the [section on lazy loading](xref:core/querying/related-data#lazy-loading) for more information about this topic.</span></span>

## <a name="parameters-in-entity-constructors"></a><span data-ttu-id="4b66a-108">Параметры в конструкторах сущностей</span><span class="sxs-lookup"><span data-stu-id="4b66a-108">Parameters in entity constructors</span></span>
<span data-ttu-id="4b66a-109">В качестве одного из стандартных блоков для отложенной загрузки была реализована возможность создавать сущности, конструкторы которых принимают параметры.</span><span class="sxs-lookup"><span data-stu-id="4b66a-109">As one of the required building blocks for lazy loading, we enabled the creation of entities that take parameters in their constructors.</span></span> <span data-ttu-id="4b66a-110">Параметры можно использовать для вставки значений свойств, делегатов отложенной загрузки и служб.</span><span class="sxs-lookup"><span data-stu-id="4b66a-110">You can use parameters to inject property values, lazy loading delegates, and services.</span></span>

<span data-ttu-id="4b66a-111">Дополнительные сведения об этом см. в [разделе, посвященном конструкторам сущностей с параметрами](xref:core/modeling/constructors).</span><span class="sxs-lookup"><span data-stu-id="4b66a-111">Read the [section on entity constructor with parameters](xref:core/modeling/constructors) for more information about this topic.</span></span>

## <a name="value-conversions"></a><span data-ttu-id="4b66a-112">Преобразования значений</span><span class="sxs-lookup"><span data-stu-id="4b66a-112">Value conversions</span></span>
<span data-ttu-id="4b66a-113">В предыдущих версиях EF Core было возможно сопоставление свойств только тех типов, которые поддерживались в собственном коде основного поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="4b66a-113">Until now, EF Core could only map properties of types natively supported by the underlying database provider.</span></span> <span data-ttu-id="4b66a-114">Значения копировались между столбцами и свойствами без какого-либо преобразования.</span><span class="sxs-lookup"><span data-stu-id="4b66a-114">Values were copied back and forth between columns and properties without any transformation.</span></span> <span data-ttu-id="4b66a-115">Начиная с версии EF Core 2.1 возможно преобразование значений, полученных из столбцов, прежде чем они будут применены к свойствам, и наоборот.</span><span class="sxs-lookup"><span data-stu-id="4b66a-115">Starting with EF Core 2.1, value conversions can be applied to transform the values obtained from columns before they are applied to properties, and vice versa.</span></span> <span data-ttu-id="4b66a-116">Мы предлагаем целый ряд преобразований, которые могут при необходимости применяться стандартным способом, а также API явно задаваемой конфигурации, с помощью которого можно регистрировать настраиваемые преобразования между столбцами и свойствами.</span><span class="sxs-lookup"><span data-stu-id="4b66a-116">We have a number of conversions that can be applied by convention as necessary, as well as an explicit configuration API that allows registering custom conversions between columns and properties.</span></span> <span data-ttu-id="4b66a-117">Варианты применения этой возможности:</span><span class="sxs-lookup"><span data-stu-id="4b66a-117">Some of the application of this feature are:</span></span>

- <span data-ttu-id="4b66a-118">Хранение перечислений в строковом виде</span><span class="sxs-lookup"><span data-stu-id="4b66a-118">Storing enums as strings</span></span>
- <span data-ttu-id="4b66a-119">Сопоставление целочисленных значений со знаком с SQL Server</span><span class="sxs-lookup"><span data-stu-id="4b66a-119">Mapping unsigned integers with SQL Server</span></span>
- <span data-ttu-id="4b66a-120">Автоматические шифрование и расшифровка значений свойств</span><span class="sxs-lookup"><span data-stu-id="4b66a-120">Automatic encryption and decryption of property values</span></span>

<span data-ttu-id="4b66a-121">Дополнительные сведения об этом см. в [разделе, посвященном преобразованиям значений](xref:core/modeling/value-conversions).</span><span class="sxs-lookup"><span data-stu-id="4b66a-121">Read the [section on value conversions](xref:core/modeling/value-conversions) for more information about this topic.</span></span>  

## <a name="linq-groupby-translation"></a><span data-ttu-id="4b66a-122">Преобразование оператора LINQ GroupBy</span><span class="sxs-lookup"><span data-stu-id="4b66a-122">LINQ GroupBy translation</span></span>
<span data-ttu-id="4b66a-123">В версиях EF Core, предшествовавших версии 2.1, оператор LINQ GroupBy всегда вычислялся в памяти.</span><span class="sxs-lookup"><span data-stu-id="4b66a-123">Before version 2.1, in EF Core the GroupBy LINQ operator would always be evaluated in memory.</span></span> <span data-ttu-id="4b66a-124">На данный момент в большинстве типовых случаев поддерживается его преобразование в предложение SQL GROUP BY.</span><span class="sxs-lookup"><span data-stu-id="4b66a-124">We now support translating it to the SQL GROUP BY clause in most common cases.</span></span>

<span data-ttu-id="4b66a-125">В этом примере показан запрос с оператором GroupBy, используемый для вычисления различных агрегатных функций:</span><span class="sxs-lookup"><span data-stu-id="4b66a-125">This example shows a query with GroupBy used to compute various aggregate functions:</span></span>

``` csharp
var query = context.Orders
    .GroupBy(o => new { o.CustomerId, o.EmployeeId })
    .Select(g => new
        {
          g.Key.CustomerId,
          g.Key.EmployeeId,
          Sum = g.Sum(o => o.Amount),
          Min = g.Min(o => o.Amount),
          Max = g.Max(o => o.Amount),
          Avg = g.Average(o => o.Amount)
        });
```

<span data-ttu-id="4b66a-126">Соответствующее преобразование в SQL будет иметь следующий вид:</span><span class="sxs-lookup"><span data-stu-id="4b66a-126">The corresponding SQL translation looks like this:</span></span>

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a><span data-ttu-id="4b66a-127">Присвоение начальных значений данных</span><span class="sxs-lookup"><span data-stu-id="4b66a-127">Data Seeding</span></span>
<span data-ttu-id="4b66a-128">В новом выпуске будет возможно предоставление начальных значений для заполнения базы данных.</span><span class="sxs-lookup"><span data-stu-id="4b66a-128">With the new release it will be possible to provide initial data to populate a database.</span></span> <span data-ttu-id="4b66a-129">В отличие от версии EF6, начальные значения данных связываются с типом сущности в рамках конфигурации модели.</span><span class="sxs-lookup"><span data-stu-id="4b66a-129">Unlike in EF6, seeding data is associated to an entity type as part of the model configuration.</span></span> <span data-ttu-id="4b66a-130">В результате система миграции EF Core может автоматически вычислить, какие операции вставки, обновления или удалений необходимо применить при обновлении базы данных с использованием новой версии модели.</span><span class="sxs-lookup"><span data-stu-id="4b66a-130">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="4b66a-131">Например, чтобы настроить начальные значения данных для отправки в `OnModelCreating`, можно использовать следующее:</span><span class="sxs-lookup"><span data-stu-id="4b66a-131">As an example, you can use this to configure seed data for a Post in `OnModelCreating`:</span></span>

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

<span data-ttu-id="4b66a-132">Дополнительные сведения об этом см. в [разделе, посвященном присвоению начальных значений данных](xref:core/modeling/data-seeding).</span><span class="sxs-lookup"><span data-stu-id="4b66a-132">Read the [section on data seeding](xref:core/modeling/data-seeding) for more information about this topic.</span></span>  

## <a name="query-types"></a><span data-ttu-id="4b66a-133">Типы запросов</span><span class="sxs-lookup"><span data-stu-id="4b66a-133">Query types</span></span>
<span data-ttu-id="4b66a-134">Модель EF Core теперь может включать типы запросов.</span><span class="sxs-lookup"><span data-stu-id="4b66a-134">An EF Core model can now include query types.</span></span> <span data-ttu-id="4b66a-135">В отличие от типов сущностей типы запросов не имеют определенных ключей и не могут вставляться, удаляться или обновляться (то есть доступны только для чтения), однако при этом они могут напрямую возвращаться запросами.</span><span class="sxs-lookup"><span data-stu-id="4b66a-135">Unlike entity types, query types do not have keys defined on them and cannot be inserted, deleted or updated (that is, they are read-only), but they can be returned directly by queries.</span></span> <span data-ttu-id="4b66a-136">Ниже приведены некоторые сценарии использования типов запросов:</span><span class="sxs-lookup"><span data-stu-id="4b66a-136">Some of the usage scenarios for query types are:</span></span>

- <span data-ttu-id="4b66a-137">Сопоставление с представлениями без первичных ключей</span><span class="sxs-lookup"><span data-stu-id="4b66a-137">Mapping to views without primary keys</span></span>
- <span data-ttu-id="4b66a-138">Сопоставление с таблицами без первичных ключей</span><span class="sxs-lookup"><span data-stu-id="4b66a-138">Mapping to tables without primary keys</span></span>
- <span data-ttu-id="4b66a-139">Сопоставление с запросами, определенными в модели</span><span class="sxs-lookup"><span data-stu-id="4b66a-139">Mapping to queries defined in the model</span></span>
- <span data-ttu-id="4b66a-140">Использование в качестве типа возвращаемого значения для запросов `FromSql()`</span><span class="sxs-lookup"><span data-stu-id="4b66a-140">Serving as the return type for `FromSql()` queries</span></span>

<span data-ttu-id="4b66a-141">Дополнительные сведения об этом см. в [разделе, посвященном типам запросов](xref:core/modeling/query-types).</span><span class="sxs-lookup"><span data-stu-id="4b66a-141">Read the [section on query types](xref:core/modeling/query-types) for more information about this topic.</span></span>

## <a name="include-for-derived-types"></a><span data-ttu-id="4b66a-142">Использование метода Include с производными типами</span><span class="sxs-lookup"><span data-stu-id="4b66a-142">Include for derived types</span></span>
<span data-ttu-id="4b66a-143">В новой версии при написании выражений для метода `Include` можно будет указывать только свойства навигации, определенные для производных типов.</span><span class="sxs-lookup"><span data-stu-id="4b66a-143">It will be now possible to specify navigation properties only defined on derived types when writing expressions for the `Include` method.</span></span> <span data-ttu-id="4b66a-144">Для строго типизированной версии метода `Include` будет поддерживаться либо явное приведение, либо оператор `as`.</span><span class="sxs-lookup"><span data-stu-id="4b66a-144">For the strongly typed version of `Include`, we support using either an explicit cast or the `as` operator.</span></span> <span data-ttu-id="4b66a-145">Кроме того, теперь поддерживается использование ссылок на имена свойств навигации, определенных для производных типов в строковой версии метода `Include`:</span><span class="sxs-lookup"><span data-stu-id="4b66a-145">We also now support referencing the names of navigation property defined on derived types in the string version of `Include`:</span></span>

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

<span data-ttu-id="4b66a-146">Дополнительные сведения об этом см. в [разделе, посвященном использованию метода Include с производными типами](xref:core/querying/related-data#include-on-derived-types).</span><span class="sxs-lookup"><span data-stu-id="4b66a-146">Read the [section on Include with derived types](xref:core/querying/related-data#include-on-derived-types) for more information about this topic.</span></span>

## <a name="systemtransactions-support"></a><span data-ttu-id="4b66a-147">Поддержка System.Transactions</span><span class="sxs-lookup"><span data-stu-id="4b66a-147">System.Transactions support</span></span>
<span data-ttu-id="4b66a-148">Мы реализовали возможность работы с функциями System.Transactions, такими как TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="4b66a-148">We have added the ability to work with System.Transactions features such as TransactionScope.</span></span> <span data-ttu-id="4b66a-149">Это будет возможно на платформах .NET Framework и .NET Core при использовании поставщиков баз данных, обеспечивающих соответствующую поддержку.</span><span class="sxs-lookup"><span data-stu-id="4b66a-149">This will work on both .NET Framework and .NET Core when using database providers that support it.</span></span>

<span data-ttu-id="4b66a-150">Дополнительные сведения об этом см. в [разделе, посвященном работе с System.Transactions](xref:core/saving/transactions#using-systemtransactions).</span><span class="sxs-lookup"><span data-stu-id="4b66a-150">Read the [section on System.Transactions](xref:core/saving/transactions#using-systemtransactions) for more information about this topic.</span></span>

## <a name="better-column-ordering-in-initial-migration"></a><span data-ttu-id="4b66a-151">Оптимизация порядка столбцов при первоначальной миграции</span><span class="sxs-lookup"><span data-stu-id="4b66a-151">Better column ordering in initial migration</span></span>
<span data-ttu-id="4b66a-152">На основе отзывов клиентов мы обновили функции миграции таким образом, чтобы изначально столбцы таблиц создавались в том порядке, в котором свойства объявлены в классе.</span><span class="sxs-lookup"><span data-stu-id="4b66a-152">Based on customer feedback, we have updated migrations to initially generate columns for tables in the same order as properties are declared in classes.</span></span> <span data-ttu-id="4b66a-153">Обратите внимание, что EF Core не позволяет изменять порядок при добавлении новых элементов после создания первоначальной таблицы.</span><span class="sxs-lookup"><span data-stu-id="4b66a-153">Note that EF Core cannot change order when new members are added after the initial table creation.</span></span>

## <a name="optimization-of-correlated-subqueries"></a><span data-ttu-id="4b66a-154">Оптимизация коррелированных вложенных запросов</span><span class="sxs-lookup"><span data-stu-id="4b66a-154">Optimization of correlated subqueries</span></span>
<span data-ttu-id="4b66a-155">Мы оптимизировали систему преобразования запросов таким образом, чтобы исключить выполнение SQL-запросов "N+1" в большинстве типовых сценариев, в которых использование свойства навигации в проекции приводит к объединению данных из корневого запроса с данными коррелированного вложенного запроса.</span><span class="sxs-lookup"><span data-stu-id="4b66a-155">We have improved our query translation to avoid executing "N + 1" SQL queries in many common scenarios in which the usage of a navigation property in the projection leads to joining data from the root query with data from a correlated subquery.</span></span> <span data-ttu-id="4b66a-156">Для оптимизации применяется буферизация результатов из вложенного запроса. Чтобы явно согласиться на использование нового поведения, необходимо соответствующим образом изменить запрос.</span><span class="sxs-lookup"><span data-stu-id="4b66a-156">The optimization requires buffering the results from the subquery, and we require that you modify the query to opt-in the new behavior.</span></span>

<span data-ttu-id="4b66a-157">Например, следующий запрос обычно преобразуется в один запрос к Customers и N (где N соответствует возвращаемому числу клиентов) отдельных запросов к Orders:</span><span class="sxs-lookup"><span data-stu-id="4b66a-157">As an example, the following query normally gets translated into one query for Customers, plus N (where "N" is the number of customers returned) separate queries for Orders:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

<span data-ttu-id="4b66a-158">Включив в нужном месте `ToList()`, вы указываете на необходимость буферизации для Orders, что позволяет обеспечить оптимизацию:</span><span class="sxs-lookup"><span data-stu-id="4b66a-158">By including `ToList()` in the right place, you indicate that buffering is appropriate for the Orders, which enable the optimization:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

<span data-ttu-id="4b66a-159">Обратите внимание, что этот запрос будет преобразован только в два SQL-запроса: один к Customers и один к Orders.</span><span class="sxs-lookup"><span data-stu-id="4b66a-159">Note that this query will be translated to only two SQL queries: One for Customers and the next one for Orders.</span></span>

## <a name="owned-attribute"></a><span data-ttu-id="4b66a-160">Атрибут [Owned]</span><span class="sxs-lookup"><span data-stu-id="4b66a-160">[Owned] attribute</span></span>

<span data-ttu-id="4b66a-161">В новой версии можно настраивать [собственные типы сущностей](xref:core/modeling/owned-entities), просто аннотируя тип с использованием `[Owned]` с последующей проверкой того, что сущность владельца добавлена в модель:</span><span class="sxs-lookup"><span data-stu-id="4b66a-161">It is now possible to configure [owned entity types](xref:core/modeling/owned-entities) by simply annotating the type with `[Owned]` and then making sure the owner entity is added to the model:</span></span>

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="command-line-tool-dotnet-ef-included-in-net-core-sdk"></a><span data-ttu-id="4b66a-162">Программа командной строки dotnet-ef, входящая в пакет SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="4b66a-162">Command-line tool dotnet-ef included in .NET Core SDK</span></span>

<span data-ttu-id="4b66a-163">Теперь команды _dotnet-ef_ входят в пакет SDK для .NET Core, поэтому необязательно использовать DotNetCliToolReference в проекте, чтобы выполнять перенос или формировать DbContext из существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="4b66a-163">The _dotnet-ef_ commands are now part of the .NET Core SDK, therefore it will no longer be necessary to use DotNetCliToolReference in the project to be able to use migrations or to scaffold a DbContext from an existing database.</span></span>

<span data-ttu-id="4b66a-164">Дополнительные сведения о включении программ командной строки для разных версий пакета SDK для .NET Core и EF Core см. в разделе об [установке средств](xref:core/miscellaneous/cli/dotnet#installing-the-tools).</span><span class="sxs-lookup"><span data-stu-id="4b66a-164">See the section on [installing the tools](xref:core/miscellaneous/cli/dotnet#installing-the-tools) for more details on how to enable command line tools for different versions of the .NET Core SDK and EF Core.</span></span>

## <a name="microsoftentityframeworkcoreabstractions-package"></a><span data-ttu-id="4b66a-165">Пакет Microsoft.EntityFrameworkCore.Abstractions</span><span class="sxs-lookup"><span data-stu-id="4b66a-165">Microsoft.EntityFrameworkCore.Abstractions package</span></span>
<span data-ttu-id="4b66a-166">Новый пакет содержит атрибуты и интерфейсы, которые можно использовать в проектах для улучшения функций EF Core без необходимости создавать зависимости от Core EF.</span><span class="sxs-lookup"><span data-stu-id="4b66a-166">The new package contains attributes and interfaces that you can use in your projects to light up EF Core features without taking a dependency on EF Core as a whole.</span></span> <span data-ttu-id="4b66a-167">Например, здесь находится атрибут [Owned] и интерфейс ILazyLoader.</span><span class="sxs-lookup"><span data-stu-id="4b66a-167">For example, the [Owned] attribute and the ILazyLoader interface are located here.</span></span>

## <a name="state-change-events"></a><span data-ttu-id="4b66a-168">События изменения состояния</span><span class="sxs-lookup"><span data-stu-id="4b66a-168">State change events</span></span>

<span data-ttu-id="4b66a-169">Новые события `Tracked` и `StateChanged` в `ChangeTracker` можно использовать для записи логики, которая реагирует на ввод сущностей DbContext или изменение их состояния.</span><span class="sxs-lookup"><span data-stu-id="4b66a-169">New `Tracked` And `StateChanged` events on `ChangeTracker` can be used to write logic that reacts to entities entering the DbContext or changing their state.</span></span>

## <a name="raw-sql-parameter-analyzer"></a><span data-ttu-id="4b66a-170">Анализатор необработанных параметров SQL</span><span class="sxs-lookup"><span data-stu-id="4b66a-170">Raw SQL parameter analyzer</span></span>

<span data-ttu-id="4b66a-171">Новый анализатор кода входит в состав EF Core для обнаружения потенциально небезопасных случаев использования необработанных API SQL, например `FromSql` или `ExecuteSqlCommand`.</span><span class="sxs-lookup"><span data-stu-id="4b66a-171">A new code analyzer is included with EF Core that detects potentially unsafe usages of our raw-SQL APIs, like `FromSql` or `ExecuteSqlCommand`.</span></span> <span data-ttu-id="4b66a-172">Например, в следующем запросе будет отображено предупреждение, так как для _minAge_ параметры отсутствуют:</span><span class="sxs-lookup"><span data-stu-id="4b66a-172">For example, for the following query, you will see a warning because _minAge_ is not parameterized:</span></span>

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a><span data-ttu-id="4b66a-173">Совместимость с поставщиками баз данных</span><span class="sxs-lookup"><span data-stu-id="4b66a-173">Database provider compatibility</span></span>

<span data-ttu-id="4b66a-174">Рекомендуется использовать EF Core 2.1 с поставщиками, которые были обновлены и по меньшей мере протестированы для работы с EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="4b66a-174">It is recommended that you use EF Core 2.1 with providers that have been updated or at least tested to work with EF Core 2.1.</span></span>

> [!TIP]
> <span data-ttu-id="4b66a-175">Если вы столкнулись с неожиданными проблемами с совместимостью или другими ошибками в новых возможностях, сообщите нам об этом с помощью [средства отслеживания проблем](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span><span class="sxs-lookup"><span data-stu-id="4b66a-175">If you find any unexpected incompatibility or any issue in the new features, or if you have feedback on them, please report it using [our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span></span>
