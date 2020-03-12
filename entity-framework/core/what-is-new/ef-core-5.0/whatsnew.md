---
title: Новые возможности EF Core 5.0
author: ajcvickers
ms.date: 01/29/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: 65d7bd43e8a00c77fd6091a74c677635710d03e3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413849"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="134da-102">Новые возможности EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="134da-102">What's New in EF Core 5.0</span></span>

<span data-ttu-id="134da-103">Сейчас EF Core 5.0 находится на стадии разработки.</span><span class="sxs-lookup"><span data-stu-id="134da-103">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="134da-104">На этой странице представлен обзор интересных изменений, появившихся в каждой предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="134da-104">This page will contain an overview of interesting changes introduced in each preview.</span></span>
<span data-ttu-id="134da-105">Ожидается, что первая предварительная версия EF Core 5.0 выйдет в первом квартале 2020 г.</span><span class="sxs-lookup"><span data-stu-id="134da-105">The first preview of EF Core 5.0 is tentatively expected in in the first quarter of 2020.</span></span>

<span data-ttu-id="134da-106">Эта страница не является аналогом статьи о [планировании для EF Core 5.0](plan.md).</span><span class="sxs-lookup"><span data-stu-id="134da-106">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="134da-107">В плане описываются общие темы для EF Core 5.0, в том числе все, что планируется включить перед выпуском окончательной версии.</span><span class="sxs-lookup"><span data-stu-id="134da-107">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="134da-108">Мы будем добавлять ссылки на официальную документацию по мере ее публикации.</span><span class="sxs-lookup"><span data-stu-id="134da-108">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-1-not-yet-shipped"></a><span data-ttu-id="134da-109">Предварительная версия 1 (еще не выпущена)</span><span class="sxs-lookup"><span data-stu-id="134da-109">Preview 1 (Not yet shipped)</span></span>

### <a name="simple-logging"></a><span data-ttu-id="134da-110">Простое ведение журнала</span><span class="sxs-lookup"><span data-stu-id="134da-110">Simple logging</span></span>

<span data-ttu-id="134da-111">Эта функция добавляет возможность, аналогичную `Database.Log` в EF6.</span><span class="sxs-lookup"><span data-stu-id="134da-111">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="134da-112">Это значит, что она позволяет легко получать журналы из EF Core без настройки какой-либо внешней платформы ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="134da-112">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="134da-113">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 5 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="134da-113">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="134da-114">Дополнительная документация отслеживается по проблеме [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span><span class="sxs-lookup"><span data-stu-id="134da-114">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="134da-115">Простой способ получения созданного кода SQL</span><span class="sxs-lookup"><span data-stu-id="134da-115">Simple way to get generated SQL</span></span>

<span data-ttu-id="134da-116">В EF Core 5.0 представлен метод расширения `ToQueryString`, который будет возвращать код SQL, создаваемый в EF Core при выполнении запроса LINQ.</span><span class="sxs-lookup"><span data-stu-id="134da-116">EF Core 5.0 introduces the `ToQueryString` extension method which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="134da-117">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 9 января 2020 г](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span><span class="sxs-lookup"><span data-stu-id="134da-117">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="134da-118">Дополнительная документация отслеживается по проблеме [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span><span class="sxs-lookup"><span data-stu-id="134da-118">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="134da-119">Улучшенные представления отладки</span><span class="sxs-lookup"><span data-stu-id="134da-119">Enhanced debug views</span></span>

<span data-ttu-id="134da-120">В представлениях отладки удобно просматривать внутренние компоненты EF Core при отладке.</span><span class="sxs-lookup"><span data-stu-id="134da-120">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="134da-121">Представление отладки для модели было реализовано некоторое время назад.</span><span class="sxs-lookup"><span data-stu-id="134da-121">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="134da-122">В EF Core 5.0 представление модели стало более удобочитаемым. Кроме того, добавлено новое представление отладки для отслеживаемых сущностей в диспетчере состояний.</span><span class="sxs-lookup"><span data-stu-id="134da-122">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="134da-123">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 12 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span><span class="sxs-lookup"><span data-stu-id="134da-123">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="134da-124">Дополнительная документация отслеживается по проблеме [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span><span class="sxs-lookup"><span data-stu-id="134da-124">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="134da-125">Возможность изменения подключения или строки подключения в инициализированном DbContext</span><span class="sxs-lookup"><span data-stu-id="134da-125">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="134da-126">Теперь создание экземпляра DbContext без подключения или строки подключения стало еще проще.</span><span class="sxs-lookup"><span data-stu-id="134da-126">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="134da-127">Кроме того, подключение или строку подключения можно изменить в экземпляре контекста.</span><span class="sxs-lookup"><span data-stu-id="134da-127">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="134da-128">Это позволяет одному и тому же экземпляру контекста динамически подключаться к разным базам данных.</span><span class="sxs-lookup"><span data-stu-id="134da-128">This allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="134da-129">Документация отслеживается по проблеме [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span><span class="sxs-lookup"><span data-stu-id="134da-129">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="134da-130">Прокси-серверы отслеживания изменений</span><span class="sxs-lookup"><span data-stu-id="134da-130">Change-tracking proxies</span></span>

<span data-ttu-id="134da-131">Теперь EF Core может создавать прокси-серверы среды выполнения, которые автоматически реализуют интерфейсы [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) и [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="134da-131">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="134da-132">Затем они сообщают об изменениях значений свойств сущности напрямую в EF Core, что позволяет не выполнять проверку на наличие изменений.</span><span class="sxs-lookup"><span data-stu-id="134da-132">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="134da-133">Однако прокси-серверы имеют собственный набор ограничений, поэтому они предназначены не для всех.</span><span class="sxs-lookup"><span data-stu-id="134da-133">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="134da-134">Документация отслеживается по проблеме [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span><span class="sxs-lookup"><span data-stu-id="134da-134">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="134da-135">Улучшенная обработка семантики значений NULL в базе данных</span><span class="sxs-lookup"><span data-stu-id="134da-135">Improved handling of database null semantics</span></span>

<span data-ttu-id="134da-136">Реляционные базы данных обычно обрабатывают NULL как неизвестное значение и, следовательно, как не равное любому другому значению NULL.</span><span class="sxs-lookup"><span data-stu-id="134da-136">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="134da-137">С другой стороны, C# рассматривает NULL как определенное значение, которое аналогично любому другому значению NULL.</span><span class="sxs-lookup"><span data-stu-id="134da-137">C#, on the other hand, treats null as a defined value which compares equal to any other null.</span></span>
<span data-ttu-id="134da-138">EF Core по умолчанию преобразует запросы так, чтобы они использовали семантику значений NULL C#.</span><span class="sxs-lookup"><span data-stu-id="134da-138">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="134da-139">В EF Core 5.0 эффективность таких преобразований значительно повышена.</span><span class="sxs-lookup"><span data-stu-id="134da-139">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="134da-140">Документация отслеживается по проблеме [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span><span class="sxs-lookup"><span data-stu-id="134da-140">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="134da-141">Свойства индексатора</span><span class="sxs-lookup"><span data-stu-id="134da-141">Indexer properties</span></span>

<span data-ttu-id="134da-142">EF Core 5.0 поддерживает сопоставление свойств индексатора C#.</span><span class="sxs-lookup"><span data-stu-id="134da-142">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="134da-143">Это позволяет сущностям выступать в качестве контейнеров свойств, в которых столбцы сопоставляются с именованными свойствами в контейнере.</span><span class="sxs-lookup"><span data-stu-id="134da-143">This allows entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="134da-144">Документация отслеживается по проблеме [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span><span class="sxs-lookup"><span data-stu-id="134da-144">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="134da-145">Создание ограничений CHECK для сопоставлений перечислений</span><span class="sxs-lookup"><span data-stu-id="134da-145">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="134da-146">Функция миграции в EF Core 5.0 теперь может создавать ограничения CHECK для сопоставлений свойств перечисления.</span><span class="sxs-lookup"><span data-stu-id="134da-146">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="134da-147">Пример:</span><span class="sxs-lookup"><span data-stu-id="134da-147">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="134da-148">Документация отслеживается по проблеме [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span><span class="sxs-lookup"><span data-stu-id="134da-148">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="134da-149">Преобразования запросов для дополнительных конструкций DateTime</span><span class="sxs-lookup"><span data-stu-id="134da-149">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="134da-150">Теперь запросы, содержащие новую конструкцию DateTime, поддерживают преобразование.</span><span class="sxs-lookup"><span data-stu-id="134da-150">Queries containing new DataTime construction are now translated.</span></span>
<span data-ttu-id="134da-151">Кроме того, функция DateDiffWeek SQL Server является сопоставляемой.</span><span class="sxs-lookup"><span data-stu-id="134da-151">Also, the SQL Server function DateDiffWeek is now mapped.</span></span>

<span data-ttu-id="134da-152">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="134da-152">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="134da-153">Преобразования запросов для дополнительных конструкций массивов байтов</span><span class="sxs-lookup"><span data-stu-id="134da-153">Query translations for more byte array constructs</span></span>

<span data-ttu-id="134da-154">Запросы, использующие свойства Contains, Length, SequenceEqual и т. д. в массиве byte[], преобразуются в SQL.</span><span class="sxs-lookup"><span data-stu-id="134da-154">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="134da-155">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 5 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="134da-155">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="134da-156">Дополнительная документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="134da-156">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="134da-157">Преобразование запросов, использующих Reverse</span><span class="sxs-lookup"><span data-stu-id="134da-157">Query translation for Reverse</span></span>

<span data-ttu-id="134da-158">Теперь можно преобразовывать запросы, использующие `Reverse`.</span><span class="sxs-lookup"><span data-stu-id="134da-158">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="134da-159">Пример:</span><span class="sxs-lookup"><span data-stu-id="134da-159">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="134da-160">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="134da-160">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="134da-161">Преобразование запросов для побитовых операторов</span><span class="sxs-lookup"><span data-stu-id="134da-161">Query translation for bitwise operators</span></span>

<span data-ttu-id="134da-162">Запросы, использующие побитовые операторы, теперь преобразуются в ряде дополнительных случаев, например:</span><span class="sxs-lookup"><span data-stu-id="134da-162">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="134da-163">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="134da-163">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="134da-164">Преобразование запросов для строк в Cosmos</span><span class="sxs-lookup"><span data-stu-id="134da-164">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="134da-165">Запросы, использующие строковые методы Contains, StartsWith и EndsWith, теперь преобразуются при использовании поставщика Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="134da-165">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="134da-166">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="134da-166">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
