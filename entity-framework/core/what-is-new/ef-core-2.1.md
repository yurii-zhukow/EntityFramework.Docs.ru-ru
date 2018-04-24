---
title: Новые возможности в EF Core 2.1 — EF Core
author: divega
ms.author: divega
ms.date: 2/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: db1648095aa4d612af53f4e10a30be36edc40da5
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2018
---
# <a name="new-features-in-ef-core-21"></a><span data-ttu-id="27cbf-102">Новые возможности в EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="27cbf-102">New features in EF Core 2.1</span></span>
> [!NOTE]  
> <span data-ttu-id="27cbf-103">Этот выпуск по-прежнему находится на стадии предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="27cbf-103">This release is still in preview.</span></span>

<span data-ttu-id="27cbf-104">Кроме исправления многочисленных ошибок и небольших улучшений функциональности и производительности, EF Core 2.1 содержит ряд новых интересных функций.</span><span class="sxs-lookup"><span data-stu-id="27cbf-104">Besides numerous bug fixes and small functional and performance enhancements, EF Core 2.1 includes some compelling new features:</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="27cbf-105">«Неспешная» загрузка</span><span class="sxs-lookup"><span data-stu-id="27cbf-105">Lazy loading</span></span>
<span data-ttu-id="27cbf-106">EF Core теперь содержит стандартные блоки, необходимые для создания классов сущностей, которые могут загружать свои свойства навигации по запросу.</span><span class="sxs-lookup"><span data-stu-id="27cbf-106">EF Core now contains the necessary building blocks for anyone to author entity classes that can load their navigation properties on demand.</span></span> <span data-ttu-id="27cbf-107">Мы также разработали новый пакет Microsoft.EntityFrameworkCore.Proxies, который использует эти стандартные блоки для создания прокси-классов отложенной загрузки на основе минимально измененных классов сущностей (например, классов с виртуальными свойствами навигации).</span><span class="sxs-lookup"><span data-stu-id="27cbf-107">We have also created a new package, Microsoft.EntityFrameworkCore.Proxies, that leverages those building blocks to produce lazy loading proxy classes based on minimally modified entity classes (e.g. classes with virtual navigation properties).</span></span>

<span data-ttu-id="27cbf-108">Дополнительные сведения об этом см. в [разделе, посвященном отложенной загрузке](xref:core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="27cbf-108">Read the [section on lazy loading](xref:core/querying/related-data#lazy-loading) for more information about this topic.</span></span>

## <a name="parameters-in-entity-constructors"></a><span data-ttu-id="27cbf-109">Параметры в конструкторах сущностей</span><span class="sxs-lookup"><span data-stu-id="27cbf-109">Parameters in entity constructors</span></span>
<span data-ttu-id="27cbf-110">В качестве одного из стандартных блоков для отложенной загрузки была реализована возможность создавать сущности, конструкторы которых принимают параметры.</span><span class="sxs-lookup"><span data-stu-id="27cbf-110">As one of the required building blocks for lazy loading, we enabled the creation of entities that take parameters in their constructors.</span></span> <span data-ttu-id="27cbf-111">Параметры можно использовать для вставки значений свойств, делегатов отложенной загрузки и служб.</span><span class="sxs-lookup"><span data-stu-id="27cbf-111">You can use parameters to inject property values, lazy loading delegates, and services.</span></span>

<span data-ttu-id="27cbf-112">Дополнительные сведения об этом см. в [разделе, посвященном конструкторам сущностей с параметрами](xref:core/modeling/constructors).</span><span class="sxs-lookup"><span data-stu-id="27cbf-112">Read the [section on entity constructor with parameters](xref:core/modeling/constructors) for more information about this topic.</span></span>

## <a name="value-conversions"></a><span data-ttu-id="27cbf-113">Преобразования значений</span><span class="sxs-lookup"><span data-stu-id="27cbf-113">Value conversions</span></span>
<span data-ttu-id="27cbf-114">В предыдущих версиях EF Core было возможно сопоставление свойств только тех типов, которые поддерживались в собственном коде основного поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="27cbf-114">Until now, EF Core could only map properties of types natively supported by the underlying database provider.</span></span> <span data-ttu-id="27cbf-115">Значения копировались между столбцами и свойствами без какого-либо преобразования.</span><span class="sxs-lookup"><span data-stu-id="27cbf-115">Values were copied back and forth between columns and properties without any transformation.</span></span> <span data-ttu-id="27cbf-116">Начиная с версии EF Core 2.1 возможно преобразование значений, полученных из столбцов, прежде чем они будут применены к свойствам, и наоборот.</span><span class="sxs-lookup"><span data-stu-id="27cbf-116">Starting with EF Core 2.1, value conversions can be applied to transform the values obtained from columns before they are applied to properties, and vice versa.</span></span> <span data-ttu-id="27cbf-117">Мы предлагаем целый ряд преобразований, которые могут при необходимости применяться стандартным способом, а также API явно задаваемой конфигурации, с помощью которого можно регистрировать настраиваемые преобразования между столбцами и свойствами.</span><span class="sxs-lookup"><span data-stu-id="27cbf-117">We have a number of conversions that can be applied by convention as necessary, as well as an explicit configuration API that allows registering custom conversions between columns and properties.</span></span> <span data-ttu-id="27cbf-118">Варианты применения этой возможности:</span><span class="sxs-lookup"><span data-stu-id="27cbf-118">Some of the application of this feature are:</span></span>

- <span data-ttu-id="27cbf-119">Хранение перечислений в строковом виде</span><span class="sxs-lookup"><span data-stu-id="27cbf-119">Storing enums as strings</span></span>
- <span data-ttu-id="27cbf-120">Сопоставление целочисленных значений со знаком с SQL Server</span><span class="sxs-lookup"><span data-stu-id="27cbf-120">Mapping unsigned integers with SQL Server</span></span>
- <span data-ttu-id="27cbf-121">Автоматические шифрование и расшифровка значений свойств</span><span class="sxs-lookup"><span data-stu-id="27cbf-121">Automatic encryption and decryption of property values</span></span>

<span data-ttu-id="27cbf-122">Дополнительные сведения об этом см. в [разделе, посвященном преобразованиям значений](xref:core/modeling/value-conversions).</span><span class="sxs-lookup"><span data-stu-id="27cbf-122">Read the [section on value conversions](xref:core/modeling/value-conversions) for more information about this topic.</span></span>  

## <a name="linq-groupby-translation"></a><span data-ttu-id="27cbf-123">Преобразование оператора LINQ GroupBy</span><span class="sxs-lookup"><span data-stu-id="27cbf-123">LINQ GroupBy translation</span></span>
<span data-ttu-id="27cbf-124">В версиях EF Core, предшествовавших версии 2.1, оператор LINQ GroupBy всегда вычислялся в памяти.</span><span class="sxs-lookup"><span data-stu-id="27cbf-124">Before version 2.1, in EF Core the GroupBy LINQ operator was always be evaluated in memory.</span></span> <span data-ttu-id="27cbf-125">На данный момент в большинстве типовых случаев поддерживается его преобразование в предложение SQL GROUP BY.</span><span class="sxs-lookup"><span data-stu-id="27cbf-125">We now support translating it to the SQL GROUP BY clause in most common cases.</span></span>

<span data-ttu-id="27cbf-126">В этом примере показан запрос с оператором GroupBy, используемый для вычисления различных агрегатных функций:</span><span class="sxs-lookup"><span data-stu-id="27cbf-126">This example shows a query with GroupBy used to compute various aggregate functions:</span></span>

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
          Avg = g.Average(o => Amount)
        });
```

<span data-ttu-id="27cbf-127">Соответствующее преобразование в SQL будет иметь следующий вид:</span><span class="sxs-lookup"><span data-stu-id="27cbf-127">The corresponding SQL translation looks like this:</span></span>

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a><span data-ttu-id="27cbf-128">Присвоение начальных значений данных</span><span class="sxs-lookup"><span data-stu-id="27cbf-128">Data Seeding</span></span>
<span data-ttu-id="27cbf-129">В новом выпуске будет возможно предоставление начальных значений для заполнения базы данных.</span><span class="sxs-lookup"><span data-stu-id="27cbf-129">With the new release it will be possible to provide initial data to populate a database.</span></span> <span data-ttu-id="27cbf-130">В отличие от версии EF6, начальные значения данных связываются с типом сущности в рамках конфигурации модели.</span><span class="sxs-lookup"><span data-stu-id="27cbf-130">Unlike in EF6, seeding data is associated to an entity type as part of the model configuration.</span></span> <span data-ttu-id="27cbf-131">В результате система миграции EF Core может автоматически вычислить, какие операции вставки, обновления или удалений необходимо применить при обновлении базы данных с использованием новой версии модели.</span><span class="sxs-lookup"><span data-stu-id="27cbf-131">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="27cbf-132">Например, чтобы настроить начальные значения данных для отправки в `OnModelCreating`, можно использовать следующее:</span><span class="sxs-lookup"><span data-stu-id="27cbf-132">As an example, you can use this to configure seed data for a Post in `OnModelCreating`:</span></span>

``` csharp
modelBuilder.Entity<Post>().HasData(new Post{ Id = 1, Text = "Hello World!" });
```

<span data-ttu-id="27cbf-133">Дополнительные сведения об этом см. в [разделе, посвященном присвоению начальных значений данных](xref:core/modeling/data-seeding).</span><span class="sxs-lookup"><span data-stu-id="27cbf-133">Read the [section on data seeding](xref:core/modeling/data-seeding) for more information about this topic.</span></span>  

## <a name="query-types"></a><span data-ttu-id="27cbf-134">Типы запросов</span><span class="sxs-lookup"><span data-stu-id="27cbf-134">Query types</span></span>
<span data-ttu-id="27cbf-135">Модель EF Core теперь может включать типы запросов.</span><span class="sxs-lookup"><span data-stu-id="27cbf-135">An EF Core model can now include query types.</span></span> <span data-ttu-id="27cbf-136">В отличие от типов сущностей, типы запросов не имеют определенных ключей и не могут вставляться, удаляться или обновляться (то есть доступны только для чтения), однако при этом они могут напрямую возвращаться запросами.</span><span class="sxs-lookup"><span data-stu-id="27cbf-136">Unlike entity types, query types do not have keys defined on them and cannot be inserted, deleted or updated (i.e. they are read-only), but they can be returned directly by queries.</span></span> <span data-ttu-id="27cbf-137">Ниже приведены некоторые сценарии использования типов запросов:</span><span class="sxs-lookup"><span data-stu-id="27cbf-137">Some of the usage scenarios for query types are:</span></span>

- <span data-ttu-id="27cbf-138">Сопоставление с представлениями без первичных ключей</span><span class="sxs-lookup"><span data-stu-id="27cbf-138">Mapping to views without primary keys</span></span>
- <span data-ttu-id="27cbf-139">Сопоставление с таблицами без первичных ключей</span><span class="sxs-lookup"><span data-stu-id="27cbf-139">Mapping to tables without primary keys</span></span>
- <span data-ttu-id="27cbf-140">Сопоставление с запросами, определенными в модели</span><span class="sxs-lookup"><span data-stu-id="27cbf-140">Mapping to queries defined in the model</span></span>
- <span data-ttu-id="27cbf-141">Использование в качестве типа возвращаемого значения для запросов `FromSql()`</span><span class="sxs-lookup"><span data-stu-id="27cbf-141">Serving as the return type for `FromSql()` queries</span></span>

<span data-ttu-id="27cbf-142">Дополнительные сведения об этом см. в [разделе, посвященном типам запросов](xref:core/modeling/query-types).</span><span class="sxs-lookup"><span data-stu-id="27cbf-142">Read the [section on query types](xref:core/modeling/query-types) for more information about this topic.</span></span>

## <a name="include-for-derived-types"></a><span data-ttu-id="27cbf-143">Использование метода Include с производными типами</span><span class="sxs-lookup"><span data-stu-id="27cbf-143">Include for derived types</span></span>
<span data-ttu-id="27cbf-144">В новой версии при написании выражений для метода `Include` можно будет указывать только свойства навигации, определенные для производных типов.</span><span class="sxs-lookup"><span data-stu-id="27cbf-144">It will be now possible to specify navigation properties only defined on derived types when writing expressions for the `Include` method.</span></span> <span data-ttu-id="27cbf-145">Для строго типизированной версии метода `Include` будет поддерживаться либо явное приведение, либо оператор `as`.</span><span class="sxs-lookup"><span data-stu-id="27cbf-145">For the strongly typed version of `Include`, we support using either an explicit cast or the `as` operator.</span></span> <span data-ttu-id="27cbf-146">Кроме того, теперь поддерживается использование ссылок на имена свойств навигации, определенных для производных типов в строковой версии метода `Include`:</span><span class="sxs-lookup"><span data-stu-id="27cbf-146">We also now support referencing the names of navigation property defined on derived types in the string version of `Include`:</span></span>

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

<span data-ttu-id="27cbf-147">Дополнительные сведения об этом см. в [разделе, посвященном использованию метода Include с производными типами](xref:core/querying/related-data#include-on-derived-types).</span><span class="sxs-lookup"><span data-stu-id="27cbf-147">Read the [section on Include with derived types](xref:core/querying/related-data#include-on-derived-types) for more information about this topic.</span></span>

## <a name="systemtransactions-support"></a><span data-ttu-id="27cbf-148">Поддержка System.Transactions</span><span class="sxs-lookup"><span data-stu-id="27cbf-148">System.Transactions support</span></span>
<span data-ttu-id="27cbf-149">Мы реализовали возможность работы с функциями System.Transactions, такими как TransactionScope.</span><span class="sxs-lookup"><span data-stu-id="27cbf-149">We have added the ability to work with System.Transactions features such as TransactionScope.</span></span> <span data-ttu-id="27cbf-150">Это будет возможно на платформах .NET Framework и .NET Core при использовании поставщиков баз данных, обеспечивающих соответствующую поддержку.</span><span class="sxs-lookup"><span data-stu-id="27cbf-150">This will work on both .NET Framework and .NET Core when using database providers that support it.</span></span>

<span data-ttu-id="27cbf-151">Дополнительные сведения об этом см. в [разделе, посвященном работе с System.Transactions](xref:core/saving/transactions#using-systemtransactions).</span><span class="sxs-lookup"><span data-stu-id="27cbf-151">Read the [section on System.Transactions](xref:core/saving/transactions#using-systemtransactions) for more information about this topic.</span></span>

## <a name="better-column-ordering-in-initial-migration"></a><span data-ttu-id="27cbf-152">Оптимизация порядка столбцов при первоначальной миграции</span><span class="sxs-lookup"><span data-stu-id="27cbf-152">Better column ordering in initial migration</span></span>
<span data-ttu-id="27cbf-153">На основе отзывов клиентов мы обновили функции миграции таким образом, чтобы изначально столбцы таблиц создавались в том порядке, в котором свойства объявлены в классе.</span><span class="sxs-lookup"><span data-stu-id="27cbf-153">Based on customer feedback, we have updated migrations to initially generate columns for tables in the same order as properties are declared in classes.</span></span> <span data-ttu-id="27cbf-154">Обратите внимание, что EF Core не позволяет изменять порядок при добавлении новых элементов после создания первоначальной таблицы.</span><span class="sxs-lookup"><span data-stu-id="27cbf-154">Note that EF Core cannot change order when new members are added after the initial table creation.</span></span>

## <a name="optimization-of-correlated-subqueries"></a><span data-ttu-id="27cbf-155">Оптимизация коррелированных вложенных запросов</span><span class="sxs-lookup"><span data-stu-id="27cbf-155">Optimization of correlated subqueries</span></span>
<span data-ttu-id="27cbf-156">Мы оптимизировали систему преобразования запросов таким образом, чтобы исключить выполнение SQL-запросов "N+1" в большинстве типовых сценариев, в которых использование свойства навигации в проекции приводит к объединению данных из корневого запроса с данными коррелированного вложенного запроса.</span><span class="sxs-lookup"><span data-stu-id="27cbf-156">We have improved our query translation to avoid executing "N + 1" SQL queries in many common scenarios in which the usage of a navigation property in the projection leads to joining data from the root query with data from a correlated subquery.</span></span> <span data-ttu-id="27cbf-157">Для оптимизации применяется буферизация результатов вложенного запроса. Чтобы использовать преимущества нового поведения, необходимо соответствующим образом изменить запрос.</span><span class="sxs-lookup"><span data-stu-id="27cbf-157">The optimization requires buffering the results form the subquery, and we require that you modify the query to opt-in the new behavior.</span></span>

<span data-ttu-id="27cbf-158">Например, следующий запрос обычно преобразуется в один запрос к Customers и N (где N соответствует возвращаемому числу клиентов) отдельных запросов к Orders:</span><span class="sxs-lookup"><span data-stu-id="27cbf-158">As an example, the following query normally gets translated into one query for Customers, plus N (where "N" is the number of customers returned) separate queries for Orders:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

<span data-ttu-id="27cbf-159">Включив в нужном месте `ToList()`, вы указываете на необходимость буферизации для Orders, что позволяет обеспечить оптимизацию:</span><span class="sxs-lookup"><span data-stu-id="27cbf-159">By including `ToList()` in the right place, you indicate that buffering is appropriate for the Orders, which enable the optimization:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

<span data-ttu-id="27cbf-160">Обратите внимание, что этот запрос будет преобразован всего в два SQL-запроса: один к Customers и еще один к Orders.</span><span class="sxs-lookup"><span data-stu-id="27cbf-160">Note that this query will be translated to only two SQL queries: One for Customers and the next one for Orders.</span></span>

## <a name="ownedattribute"></a><span data-ttu-id="27cbf-161">OwnedAttribute</span><span class="sxs-lookup"><span data-stu-id="27cbf-161">OwnedAttribute</span></span>

<span data-ttu-id="27cbf-162">В новой версии можно настраивать [собственные типы сущностей](xref:core/modeling/owned-entities), просто аннотируя тип с использованием `[Owned]` с последующей проверкой того, что сущность владельца добавлена в модель:</span><span class="sxs-lookup"><span data-stu-id="27cbf-162">It is now possible to configure [owned entity types](xref:core/modeling/owned-entities) by simply annotating the type with `[Owned]` and then making sure the owner entity is added to the model:</span></span>

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

## <a name="new-dotnet-ef-global-tool"></a><span data-ttu-id="27cbf-163">Новое глобальное средство dotnet-ef</span><span class="sxs-lookup"><span data-stu-id="27cbf-163">New dotnet-ef global tool</span></span>

<span data-ttu-id="27cbf-164">Команды _dotnet-ef_ преобразованы в глобальное средство .NET CLI, поэтому теперь не обязательно использовать DotNetCliToolReference в проекте, чтобы выполнять перенос или формировать DbContext из существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="27cbf-164">The _dotnet-ef_ commands have been converted to a .NET CLI global tool, so it will no longer be necessary to use DotNetCliToolReference in the project to be able to use migrations or to scaffold a DbContext from an existing database.</span></span>

## <a name="microsoftentityframeworkcoreabstractions-package"></a><span data-ttu-id="27cbf-165">Пакет Microsoft.EntityFrameworkCore.Abstractions</span><span class="sxs-lookup"><span data-stu-id="27cbf-165">Microsoft.EntityFrameworkCore.Abstractions package</span></span>
<span data-ttu-id="27cbf-166">Новый пакет содержит атрибуты и интерфейсы, которые можно использовать в проектах для улучшения функций EF Core без необходимости создавать зависимости от Core EF.</span><span class="sxs-lookup"><span data-stu-id="27cbf-166">The new package contains attributes and interfaces that you can use in your projects to light up EF Core features without taking a dependency on EF Core as a whole.</span></span> <span data-ttu-id="27cbf-167">Например,</span><span class="sxs-lookup"><span data-stu-id="27cbf-167">E.g.</span></span> <span data-ttu-id="27cbf-168">атрибут [Owned], представленный в предварительной версии 1, был перемещен сюда.</span><span class="sxs-lookup"><span data-stu-id="27cbf-168">the [Owned] attribute introduced in Preview 1 was moved here.</span></span>

## <a name="state-change-events"></a><span data-ttu-id="27cbf-169">События изменения состояния</span><span class="sxs-lookup"><span data-stu-id="27cbf-169">State change events</span></span>

<span data-ttu-id="27cbf-170">Новые события `Tracked` и `StateChanged` в `ChangeTracker` можно использовать для записи логики, которая реагирует на ввод сущностей DbContext или изменение их состояния.</span><span class="sxs-lookup"><span data-stu-id="27cbf-170">New `Tracked` And `StateChanged` events on `ChangeTracker` can be used to write logic that reacts to entities entering the DbContext or changing their state.</span></span>

## <a name="raw-sql-parameter-analyzer"></a><span data-ttu-id="27cbf-171">Анализатор необработанных параметров SQL</span><span class="sxs-lookup"><span data-stu-id="27cbf-171">Raw SQL parameter analyzer</span></span>

<span data-ttu-id="27cbf-172">Новый анализатор кода входит в состав EF Core для обнаружения потенциально небезопасных случаев использования необработанных API SQL, например `FromSql` или `ExecuteSqlCommand`.</span><span class="sxs-lookup"><span data-stu-id="27cbf-172">A new code analyzer is included with EF Core that detects potentially unsafe usages of our raw-SQL APIs, like `FromSql` or `ExecuteSqlCommand`.</span></span> <span data-ttu-id="27cbf-173">Например,</span><span class="sxs-lookup"><span data-stu-id="27cbf-173">E.g.</span></span> <span data-ttu-id="27cbf-174">в следующем запросе, будет отображено предупреждение, так как для _minAge_ параметры отсутствуют:</span><span class="sxs-lookup"><span data-stu-id="27cbf-174">for the following query, you will see a warning because _minAge_ is not parameterized:</span></span>

``` csharp
var sql = $"SELECT * FROM People WHERE Age > {minAge}";
var query = context.People.FromSql(sql);
```

## <a name="database-provider-compatibility"></a><span data-ttu-id="27cbf-175">Совместимость с поставщиками баз данных</span><span class="sxs-lookup"><span data-stu-id="27cbf-175">Database provider compatibility</span></span>

<span data-ttu-id="27cbf-176">Версия EF Core 2.1 обеспечивает совместимость с поставщиками баз данных, созданных для EF Core 2.0 или требующих минимальных изменений.</span><span class="sxs-lookup"><span data-stu-id="27cbf-176">EF Core 2.1 was designed to be compatible with database providers created for EF Core 2.0, or at least require minimal changes.</span></span> <span data-ttu-id="27cbf-177">Некоторые описываемые выше возможности (например, преобразование значений) требуют обновления поставщика, однако другие (например, отложенная загрузка) работают и с существующими поставщиками.</span><span class="sxs-lookup"><span data-stu-id="27cbf-177">While some of the features described above (e.g. value conversions) require an updated provider, others (e.g. lazy loading) will light up with existing providers.</span></span>

> [!TIP]
> <span data-ttu-id="27cbf-178">Если вы столкнулись с неожиданными проблемами с совместимостью или другими ошибками в новых возможностях, сообщите нам об этом с помощью [средства отслеживания проблем](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span><span class="sxs-lookup"><span data-stu-id="27cbf-178">If you find any unexpected incompatibility or any issue in the new features, or if you have feedback on them, please report it using [our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span></span>
