---
title: Новые возможности EF Core 5.0
description: Обзор новых возможностей в EF Core 5.0
author: ajcvickers
ms.date: 06/02/2020
uid: core/what-is-new/ef-core-5.0/whatsnew
ms.openlocfilehash: 304ed74fe344b43177525113c70b7be7bb0ac5ed
ms.sourcegitcommit: 31536e52b838a84680d2e93e5bb52fb16df72a97
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2020
ms.locfileid: "86238337"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="54a78-103">Новые возможности EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="54a78-103">What's New in EF Core 5.0</span></span>

<span data-ttu-id="54a78-104">Сейчас EF Core 5.0 находится на стадии разработки.</span><span class="sxs-lookup"><span data-stu-id="54a78-104">EF Core 5.0 is currently in development.</span></span> <span data-ttu-id="54a78-105">На этой странице представлен обзор интересных изменений, появившихся в каждой предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="54a78-105">This page will contain an overview of interesting changes introduced in each preview.</span></span>

<span data-ttu-id="54a78-106">Эта страница не является аналогом статьи о [планировании для EF Core 5.0](xref:core/what-is-new/ef-core-5.0/plan).</span><span class="sxs-lookup"><span data-stu-id="54a78-106">This page does not duplicate the [plan for EF Core 5.0](xref:core/what-is-new/ef-core-5.0/plan).</span></span> <span data-ttu-id="54a78-107">В плане описываются общие темы для EF Core 5.0, в том числе все, что планируется включить перед выпуском окончательной версии.</span><span class="sxs-lookup"><span data-stu-id="54a78-107">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="54a78-108">Мы будем добавлять ссылки на официальную документацию по мере ее публикации.</span><span class="sxs-lookup"><span data-stu-id="54a78-108">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-6"></a><span data-ttu-id="54a78-109">предварительная версия 6</span><span class="sxs-lookup"><span data-stu-id="54a78-109">Preview 6</span></span>

### <a name="split-queries-for-related-collections"></a><span data-ttu-id="54a78-110">Разделение запросов для связанных коллекций</span><span class="sxs-lookup"><span data-stu-id="54a78-110">Split queries for related collections</span></span>

<span data-ttu-id="54a78-111">Начиная с EF Core 3.0, EF Core всегда создает один SQL-запрос для каждого LINQ-запроса.</span><span class="sxs-lookup"><span data-stu-id="54a78-111">Starting with EF Core 3.0, EF Core always generates a single SQL query for each LINQ query.</span></span> <span data-ttu-id="54a78-112">Это обеспечивает согласованность данных, возвращаемых в рамках ограничений используемого режима транзакций.</span><span class="sxs-lookup"><span data-stu-id="54a78-112">This ensures consistency of the data returned within the constraints of the transaction mode in use.</span></span> <span data-ttu-id="54a78-113">Однако если запрос использует `Include` или проекцию для возврата нескольких связанных коллекций, это может привести к снижению производительности.</span><span class="sxs-lookup"><span data-stu-id="54a78-113">However, this can become very slow when the query uses `Include` or a projection to bring back multiple related collections.</span></span>

<span data-ttu-id="54a78-114">Теперь EF Core 5.0 позволяет разделять один LINQ-запрос, включая связанные коллекции, на несколько SQL-запросов.</span><span class="sxs-lookup"><span data-stu-id="54a78-114">EF Core 5.0 now allows a single LINQ query including related collections to be split into multiple SQL queries.</span></span> <span data-ttu-id="54a78-115">Это может значительно повысить производительность, но может также привести к несогласованности возвращаемых результатов в случае изменения данных между двумя запросами.</span><span class="sxs-lookup"><span data-stu-id="54a78-115">This can significantly improve performance, but can result in inconsistency in the results returned if the data changes between the two queries.</span></span> <span data-ttu-id="54a78-116">Для устранения этой проблемы и обеспечения согласованности с разделенными запросами можно использовать сериализуемые транзакции или транзакции с моментальными снимками, но это может привести к дополнительным затратам на производительность и вызвать различия в поведении.</span><span class="sxs-lookup"><span data-stu-id="54a78-116">Serializable or snapshot transactions can be used to mitigate this and achieve consistency with split queries, but that may bring other performance costs and behavioral difference.</span></span>

#### <a name="split-queries-with-include"></a><span data-ttu-id="54a78-117">Разделение запросов с помощью Include</span><span class="sxs-lookup"><span data-stu-id="54a78-117">Split queries with Include</span></span>

<span data-ttu-id="54a78-118">Рассмотрим запрос, который извлекает два уровня связанных коллекций с помощью `Include`:</span><span class="sxs-lookup"><span data-stu-id="54a78-118">For example, consider a query that pulls in two levels of related collections using `Include`:</span></span>

```CSharp
var artists = context.Artists
    .Include(e => e.Albums).ThenInclude(e => e.Tags)
    .ToList();
```

<span data-ttu-id="54a78-119">Если используется поставщик SQLite, EF Core по умолчанию создаст следующий SQL:</span><span class="sxs-lookup"><span data-stu-id="54a78-119">By default, EF Core will generate the following SQL when using the SQLite provider:</span></span>

```sql
SELECT "a"."Id", "a"."Name", "t0"."Id", "t0"."ArtistId", "t0"."Title", "t0"."Id0", "t0"."AlbumId", "t0"."Name"
FROM "Artists" AS "a"
LEFT JOIN (
    SELECT "a0"."Id", "a0"."ArtistId", "a0"."Title", "t"."Id" AS "Id0", "t"."AlbumId", "t"."Name"
    FROM "Album" AS "a0"
    LEFT JOIN "Tag" AS "t" ON "a0"."Id" = "t"."AlbumId"
) AS "t0" ON "a"."Id" = "t0"."ArtistId"
ORDER BY "a"."Id", "t0"."Id", "t0"."Id0"
```

<span data-ttu-id="54a78-120">Для изменения этого поведения можно использовать новый API `AsSplitQuery`.</span><span class="sxs-lookup"><span data-stu-id="54a78-120">The new `AsSplitQuery` API can be used to change this behavior.</span></span> <span data-ttu-id="54a78-121">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-121">For example:</span></span>

```CSharp
var artists = context.Artists
    .AsSplitQuery()
    .Include(e => e.Albums).ThenInclude(e => e.Tags)
    .ToList();
```

<span data-ttu-id="54a78-122">AsSplitQuery доступен для всех поставщиков реляционных баз данных и может использоваться в любом месте запроса так же, как AsNoTracking.</span><span class="sxs-lookup"><span data-stu-id="54a78-122">AsSplitQuery is available for all relational database providers and can be used anywhere in the query, just like AsNoTracking.</span></span> <span data-ttu-id="54a78-123">Теперь EF Core будет создавать следующие три SQL-запроса:</span><span class="sxs-lookup"><span data-stu-id="54a78-123">EF Core will now generate the following three SQL queries:</span></span>

```sql
SELECT "a"."Id", "a"."Name"
FROM "Artists" AS "a"
ORDER BY "a"."Id"

SELECT "a0"."Id", "a0"."ArtistId", "a0"."Title", "a"."Id"
FROM "Artists" AS "a"
INNER JOIN "Album" AS "a0" ON "a"."Id" = "a0"."ArtistId"
ORDER BY "a"."Id", "a0"."Id"

SELECT "t"."Id", "t"."AlbumId", "t"."Name", "a"."Id", "a0"."Id"
FROM "Artists" AS "a"
INNER JOIN "Album" AS "a0" ON "a"."Id" = "a0"."ArtistId"
INNER JOIN "Tag" AS "t" ON "a0"."Id" = "t"."AlbumId"
ORDER BY "a"."Id", "a0"."Id"
```

<span data-ttu-id="54a78-124">Поддерживаются все операции в корне запроса.</span><span class="sxs-lookup"><span data-stu-id="54a78-124">All operations on the query root are supported.</span></span> <span data-ttu-id="54a78-125">В их число входят операции OrderBy/Skip/Take, Join, FirstOrDefault и аналогичные операции выбора одного результата.</span><span class="sxs-lookup"><span data-stu-id="54a78-125">This includes OrderBy/Skip/Take, Join operations, FirstOrDefault and similar single result selecting operations.</span></span>

<span data-ttu-id="54a78-126">Обратите внимание, что включения с фильтрацией с операциями OrderBy/Skip/Take не поддерживаются в предварительной версии 6, но они доступны в ежедневных сборках и будут включены в предварительную версию 7.</span><span class="sxs-lookup"><span data-stu-id="54a78-126">Note that filtered Includes with OrderBy/Skip/Take are not supported in preview 6, but are available in the daily builds and will be included in preview 7.</span></span>

#### <a name="split-queries-with-collection-projections"></a><span data-ttu-id="54a78-127">Разделение запросов с помощью проекций коллекции</span><span class="sxs-lookup"><span data-stu-id="54a78-127">Split queries with collection projections</span></span>

<span data-ttu-id="54a78-128">`AsSplitQuery` можно также использовать при загрузке коллекций в проекции.</span><span class="sxs-lookup"><span data-stu-id="54a78-128">`AsSplitQuery` can also be used when collections are loaded in projections.</span></span> <span data-ttu-id="54a78-129">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-129">For example:</span></span>

```CSharp
context.Artists
    .AsSplitQuery()
    .Select(e => new
    {
        Artist = e,
        Albums = e.Albums,
    }).ToList();
```

<span data-ttu-id="54a78-130">При использовании поставщика SQLite этот LINQ-запрос создает следующие два SQL-запроса:</span><span class="sxs-lookup"><span data-stu-id="54a78-130">This LINQ query generates the following two SQL queries when using the SQLite provider:</span></span>

```sql
SELECT "a"."Id", "a"."Name"
FROM "Artists" AS "a"
ORDER BY "a"."Id"

SELECT "a0"."Id", "a0"."ArtistId", "a0"."Title", "a"."Id"
FROM "Artists" AS "a"
INNER JOIN "Album" AS "a0" ON "a"."Id" = "a0"."ArtistId"
ORDER BY "a"."Id"
```

<span data-ttu-id="54a78-131">Обратите внимание, что поддерживается только материализация коллекции.</span><span class="sxs-lookup"><span data-stu-id="54a78-131">Note that only materialization of the collection is supported.</span></span> <span data-ttu-id="54a78-132">Любая композиция после `e.Albums` в приведенном выше случае не приведет к разделению запроса.</span><span class="sxs-lookup"><span data-stu-id="54a78-132">Any composition after `e.Albums` in above case won't result in a split query.</span></span> <span data-ttu-id="54a78-133">Улучшения в этой области отслеживаются по проблеме [#21234](https://github.com/dotnet/efcore/issues/21234).</span><span class="sxs-lookup"><span data-stu-id="54a78-133">Improvements in this area are tracked by [#21234](https://github.com/dotnet/efcore/issues/21234).</span></span>

### <a name="indexattribute"></a><span data-ttu-id="54a78-134">IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="54a78-134">IndexAttribute</span></span>

<span data-ttu-id="54a78-135">Новый IndexAttribute можно поместить в тип сущности, чтобы указать индекс для одного столбца.</span><span class="sxs-lookup"><span data-stu-id="54a78-135">The new IndexAttribute can be placed on an entity type to specify an index for a single column.</span></span> <span data-ttu-id="54a78-136">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-136">For example:</span></span>

```CSharp
[Index(nameof(FullName), IsUnique = true)]
public class User
{
    public int Id { get; set; }
    
    [MaxLength(128)]
    public string FullName { get; set; }
}
```

<span data-ttu-id="54a78-137">Для SQL Server функция миграции создаст следующий SQL:</span><span class="sxs-lookup"><span data-stu-id="54a78-137">For SQL Server, Migrations will then generate the following SQL:</span></span>

```sql
CREATE UNIQUE INDEX [IX_Users_FullName]
    ON [Users] ([FullName])
    WHERE [FullName] IS NOT NULL;
```

<span data-ttu-id="54a78-138">IndexAttribute можно также использовать для указания индекса, охватывающего несколько столбцов.</span><span class="sxs-lookup"><span data-stu-id="54a78-138">IndexAttribute can also be used to specify an index spanning multiple columns.</span></span> <span data-ttu-id="54a78-139">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-139">For example:</span></span>

```CSharp
[Index(nameof(FirstName), nameof(LastName), IsUnique = true)]
public class User
{
    public int Id { get; set; }
    
    [MaxLength(64)]
    public string FirstName { get; set; }

    [MaxLength(64)]
    public string LastName { get; set; }
}
```

<span data-ttu-id="54a78-140">В SQL Server будет выведен следующий результат:</span><span class="sxs-lookup"><span data-stu-id="54a78-140">For SQL Server, this results in:</span></span>

```sql
CREATE UNIQUE INDEX [IX_Users_FirstName_LastName]
    ON [Users] ([FirstName], [LastName])
    WHERE [FirstName] IS NOT NULL AND [LastName] IS NOT NULL;
```

<span data-ttu-id="54a78-141">Документация отслеживается по проблеме [#2407](https://github.com/dotnet/EntityFramework.Docs/issues/2407).</span><span class="sxs-lookup"><span data-stu-id="54a78-141">Documentation is tracked by issue [#2407](https://github.com/dotnet/EntityFramework.Docs/issues/2407).</span></span>

### <a name="improved-query-translation-exceptions"></a><span data-ttu-id="54a78-142">Улучшенные исключения преобразования запросов</span><span class="sxs-lookup"><span data-stu-id="54a78-142">Improved query translation exceptions</span></span>

<span data-ttu-id="54a78-143">Мы продолжаем улучшать сообщения об исключениях, создаваемые при сбое преобразования запроса.</span><span class="sxs-lookup"><span data-stu-id="54a78-143">We are continuing to improve the exception messages generated when query translation fails.</span></span> <span data-ttu-id="54a78-144">Например, в этом запросе используется несопоставленное свойство `IsSigned`:</span><span class="sxs-lookup"><span data-stu-id="54a78-144">For example, this query uses the unmapped property `IsSigned`:</span></span>

```CSharp
var artists = context.Artists.Where(e => e.IsSigned).ToList();
```

<span data-ttu-id="54a78-145">EF Core выдаст следующее исключение, указывающее на сбой преобразования из-за несопоставленного `IsSigned`:</span><span class="sxs-lookup"><span data-stu-id="54a78-145">EF Core will throw the following exception indicating that translation failed because `IsSigned` is not mapped:</span></span>

> <span data-ttu-id="54a78-146">Необработанное исключение.</span><span class="sxs-lookup"><span data-stu-id="54a78-146">Unhandled exception.</span></span> <span data-ttu-id="54a78-147">"System.InvalidOperationException: не удалось преобразовать выражение LINQ "DbSet<Artist>() .Where(a => a.IsSigned)".</span><span class="sxs-lookup"><span data-stu-id="54a78-147">System.InvalidOperationException: The LINQ expression 'DbSet<Artist>() .Where(a => a.IsSigned)' could not be translated.</span></span> <span data-ttu-id="54a78-148">Дополнительные сведения: не удалось преобразовать член "IsSigned" для типа сущности "Artist".</span><span class="sxs-lookup"><span data-stu-id="54a78-148">Additional information: Translation of member 'IsSigned' on entity type 'Artist' failed.</span></span> <span data-ttu-id="54a78-149">Возможно, указанный элемент не сопоставлен.</span><span class="sxs-lookup"><span data-stu-id="54a78-149">Possibly the specified member is not mapped.</span></span> <span data-ttu-id="54a78-150">Перепишите запрос в поддерживающей преобразование форме или явно переключитесь на оценку клиента, вставив вызов либо AsEnumerable(), AsAsyncEnumerable(), ToList(), либо ToListAsync().</span><span class="sxs-lookup"><span data-stu-id="54a78-150">Either rewrite the query in a form that can be translated, or switch to client evaluation explicitly by inserting a call to either AsEnumerable(), AsAsyncEnumerable(), ToList(), or ToListAsync().</span></span> <span data-ttu-id="54a78-151">Дополнительные сведения см. в разделе https://go.microsoft.com/fwlink/?linkid=2101038.</span><span class="sxs-lookup"><span data-stu-id="54a78-151">See https://go.microsoft.com/fwlink/?linkid=2101038 for more information.</span></span>

<span data-ttu-id="54a78-152">Аналогично, при попытке преобразовать сравнения строк с семантикой, зависящей от языка и региональных параметров, теперь создаются более понятные сообщения об исключениях.</span><span class="sxs-lookup"><span data-stu-id="54a78-152">Similarly, better exception messages are now generated when attempting to translate string comparisons with culture-dependent semantics.</span></span> <span data-ttu-id="54a78-153">Например, этот запрос пытается использовать `StringComparison.CurrentCulture`:</span><span class="sxs-lookup"><span data-stu-id="54a78-153">For example, this query attempts to use `StringComparison.CurrentCulture`:</span></span>

```CSharp
var artists = context.Artists
    .Where(e => e.Name.Equals("The Unicorns", StringComparison.CurrentCulture))
    .ToList();
```

<span data-ttu-id="54a78-154">Теперь EF Core выдаст следующее исключение:</span><span class="sxs-lookup"><span data-stu-id="54a78-154">EF Core will now throw the following exception:</span></span>

> <span data-ttu-id="54a78-155">Необработанное исключение.</span><span class="sxs-lookup"><span data-stu-id="54a78-155">Unhandled exception.</span></span> <span data-ttu-id="54a78-156">"System.InvalidOperationException: не удается преобразовать выражение LINQ "DbSet<Artist>() .Where(a => a.Name.Equals( value: "The Unicorns", comparisonType: CurrentCulture))".</span><span class="sxs-lookup"><span data-stu-id="54a78-156">System.InvalidOperationException: The LINQ expression 'DbSet<Artist>() .Where(a => a.Name.Equals( value: "The Unicorns", comparisonType: CurrentCulture))' could not be translated.</span></span> <span data-ttu-id="54a78-157">Дополнительные сведения: преобразование метода "string.Equals", принимающего аргумент "StringComparison", не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="54a78-157">Additional information: Translation of 'string.Equals' method which takes 'StringComparison' argument is not supported.</span></span> <span data-ttu-id="54a78-158">Дополнительные сведения см. в разделе https://go.microsoft.com/fwlink/?linkid=2129535.</span><span class="sxs-lookup"><span data-stu-id="54a78-158">See https://go.microsoft.com/fwlink/?linkid=2129535 for more information.</span></span> <span data-ttu-id="54a78-159">Перепишите запрос в поддерживающей преобразование форме или явно переключитесь на оценку клиента, вставив вызов либо AsEnumerable(), AsAsyncEnumerable(), ToList(), либо ToListAsync().</span><span class="sxs-lookup"><span data-stu-id="54a78-159">Either rewrite the query in a form that can be translated, or switch to client evaluation explicitly by inserting a call to either AsEnumerable(), AsAsyncEnumerable(), ToList(), or ToListAsync().</span></span> <span data-ttu-id="54a78-160">Дополнительные сведения см. в разделе https://go.microsoft.com/fwlink/?linkid=2101038.</span><span class="sxs-lookup"><span data-stu-id="54a78-160">See https://go.microsoft.com/fwlink/?linkid=2101038 for more information.</span></span>

### <a name="specify-transaction-id"></a><span data-ttu-id="54a78-161">Указание идентификатора транзакции</span><span class="sxs-lookup"><span data-stu-id="54a78-161">Specify transaction ID</span></span>

<span data-ttu-id="54a78-162">Эту функцию предоставил участник сообщества [@Marusyk](https://github.com/Marusyk).</span><span class="sxs-lookup"><span data-stu-id="54a78-162">This feature was contributed from the community by [@Marusyk](https://github.com/Marusyk).</span></span> <span data-ttu-id="54a78-163">Большое спасибо за публикацию!</span><span class="sxs-lookup"><span data-stu-id="54a78-163">Many thanks for the contribution!</span></span>

<span data-ttu-id="54a78-164">EF Core предоставляет идентификатор транзакции для корреляции транзакций между вызовами.</span><span class="sxs-lookup"><span data-stu-id="54a78-164">EF Core exposes a transaction ID for correlation of transactions across calls.</span></span> <span data-ttu-id="54a78-165">Этот идентификатор обычно задается в EF Core при запуске транзакции.</span><span class="sxs-lookup"><span data-stu-id="54a78-165">This ID typically set by EF Core when a transaction is started.</span></span> <span data-ttu-id="54a78-166">Если транзакцию запускает приложение, эта функция позволяет приложению явно задать идентификатор транзакции для его правильной корреляции везде, где она используется.</span><span class="sxs-lookup"><span data-stu-id="54a78-166">If the application starts the transaction instead, then this feature allows the application to explicitly set the transaction ID so it is correlated correctly everywhere it is used.</span></span> <span data-ttu-id="54a78-167">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-167">For example:</span></span>

```CSharp
using (context.Database.UseTransaction(myTransaction, myId))
{
   ...
}
```

### <a name="ipaddress-mapping"></a><span data-ttu-id="54a78-168">Сопоставление IPAddress</span><span class="sxs-lookup"><span data-stu-id="54a78-168">IPAddress mapping</span></span>

<span data-ttu-id="54a78-169">Эту функцию предоставил участник сообщества [@ralmsdeveloper](https://github.com/ralmsdeveloper).</span><span class="sxs-lookup"><span data-stu-id="54a78-169">This feature was contributed from the community by [@ralmsdeveloper](https://github.com/ralmsdeveloper).</span></span> <span data-ttu-id="54a78-170">Большое спасибо за публикацию!</span><span class="sxs-lookup"><span data-stu-id="54a78-170">Many thanks for the contribution!</span></span>

<span data-ttu-id="54a78-171">Стандартный класс [IPAddress](/dotnet/api/system.net.ipaddress) .NET теперь автоматически сопоставляется со строковым столбцом для баз данных, у которых пока нет собственной поддержки.</span><span class="sxs-lookup"><span data-stu-id="54a78-171">The standard .NET [IPAddress class](/dotnet/api/system.net.ipaddress) is now automatically mapped to a string column for databases that do not already have native support.</span></span> <span data-ttu-id="54a78-172">Например, рассмотрим сопоставление этого типа сущности:</span><span class="sxs-lookup"><span data-stu-id="54a78-172">For example, consider mapping this entity type:</span></span>

```CSharp
public class Host
{
    public int Id { get; set; }
    public IPAddress Address { get; set; }
}
```

<span data-ttu-id="54a78-173">В SQL Server функция миграции создаст следующую таблицу:</span><span class="sxs-lookup"><span data-stu-id="54a78-173">On SQL Server, this will result in Migrations creating the following table:</span></span>

```sql
CREATE TABLE [Host] (
    [Id] int NOT NULL,
    [Address] nvarchar(45) NULL,
    CONSTRAINT [PK_Host] PRIMARY KEY ([Id]));
``` 

<span data-ttu-id="54a78-174">Затем можно добавить сущности обычным образом:</span><span class="sxs-lookup"><span data-stu-id="54a78-174">Entities can then be added in the normal way:</span></span>

```CSharp
context.AddRange(
    new Host { Address = IPAddress.Parse("127.0.0.1")},
    new Host { Address = IPAddress.Parse("0000:0000:0000:0000:0000:0000:0000:0001")});
``` 

<span data-ttu-id="54a78-175">Результирующий SQL вставит нормализованный IPv4- или IPv6-адрес:</span><span class="sxs-lookup"><span data-stu-id="54a78-175">And the resulting SQL will insert the normalized IPv4 or IPv6 address:</span></span>

```sql
Executed DbCommand (14ms) [Parameters=[@p0='1', @p1='127.0.0.1' (Size = 45), @p2='2', @p3='::1' (Size = 45)], CommandType='Text', CommandTimeout='30']
      SET NOCOUNT ON;
      INSERT INTO [Host] ([Id], [Address])
      VALUES (@p0, @p1), (@p2, @p3);
```

### <a name="exclude-onconfiguring-when-scaffolding"></a><span data-ttu-id="54a78-176">Исключение OnConfiguring при формировании шаблонов</span><span class="sxs-lookup"><span data-stu-id="54a78-176">Exclude OnConfiguring when scaffolding</span></span>

<span data-ttu-id="54a78-177">При формировании DbContext из существующей базы данных EF Core по умолчанию создает перегрузку OnConfiguring со строкой подключения для немедленного использования контекста.</span><span class="sxs-lookup"><span data-stu-id="54a78-177">When a DbContext is scaffolded from an existing database, EF Core by default creates an OnConfiguring overload with a connection string so that the context is immediately usable.</span></span> <span data-ttu-id="54a78-178">Однако это бесполезно, если у вас уже есть разделяемый класс с OnConfiguring, или если вы настраиваете контекст другим способом.</span><span class="sxs-lookup"><span data-stu-id="54a78-178">However, this is not useful if you already have a partial class with OnConfiguring, or if you are configuring the context some other way.</span></span>

<span data-ttu-id="54a78-179">Теперь командам формирования шаблонов можно указать пропустить создание OnConfiguring.</span><span class="sxs-lookup"><span data-stu-id="54a78-179">To address this, the scaffolding commands can now be instructed to omit generation of OnConfiguring.</span></span> <span data-ttu-id="54a78-180">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-180">For example:</span></span>

```
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer --no-onconfiguring
```

<span data-ttu-id="54a78-181">Или в консоли диспетчера пакетов:</span><span class="sxs-lookup"><span data-stu-id="54a78-181">Or in the Package Manager Console:</span></span>

```
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer -NoOnConfiguring 
``` 

<span data-ttu-id="54a78-182">Обратите внимание, что рекомендуется использовать [именованную строку подключения и защищенное хранилище, такое как секреты пользователей](/core/managing-schemas/scaffolding?tabs=vs#configuration-and-user-secrets).</span><span class="sxs-lookup"><span data-stu-id="54a78-182">Note that we recommend using [a named connection string and secure storage like User Secrets](/core/managing-schemas/scaffolding?tabs=vs#configuration-and-user-secrets).</span></span>

### <a name="translations-for-firstordefault-on-strings"></a><span data-ttu-id="54a78-183">Преобразования для FirstOrDefault в строках</span><span class="sxs-lookup"><span data-stu-id="54a78-183">Translations for FirstOrDefault on strings</span></span>

<span data-ttu-id="54a78-184">Эту функцию предоставил участник сообщества [@dvoreckyaa](https://github.com/dvoreckyaa).</span><span class="sxs-lookup"><span data-stu-id="54a78-184">This feature was contributed from the community by [@dvoreckyaa](https://github.com/dvoreckyaa).</span></span> <span data-ttu-id="54a78-185">Большое спасибо за публикацию!</span><span class="sxs-lookup"><span data-stu-id="54a78-185">Many thanks for the contribution!</span></span>

<span data-ttu-id="54a78-186">FirstOrDefault и аналогичные операторы для символов в строках теперь можно преобразовывать.</span><span class="sxs-lookup"><span data-stu-id="54a78-186">FirstOrDefault and similar operators for characters in strings are now translated.</span></span> <span data-ttu-id="54a78-187">Например, следующий LINQ-запрос:</span><span class="sxs-lookup"><span data-stu-id="54a78-187">For example, this LINQ query:</span></span>

```CSharp
context.Customers.Where(c => c.ContactName.FirstOrDefault() == 'A').ToList();
```

<span data-ttu-id="54a78-188">будет преобразован в следующий SQL при использовании SQL Server:</span><span class="sxs-lookup"><span data-stu-id="54a78-188">Will be translated to the following SQL when using SQL Server:</span></span>

```sql
SELECT [c].[Id], [c].[ContactName]
FROM [Customer] AS [c]
WHERE SUBSTRING([c].[ContactName], 1, 1) = N'A'
```

### <a name="simplify-case-blocks"></a><span data-ttu-id="54a78-189">Упрощение блоков case</span><span class="sxs-lookup"><span data-stu-id="54a78-189">Simplify case blocks</span></span>

<span data-ttu-id="54a78-190">Теперь EF Core создает улучшенные запросы с блоками case.</span><span class="sxs-lookup"><span data-stu-id="54a78-190">EF Core now generates better queries with CASE blocks.</span></span> <span data-ttu-id="54a78-191">Например, следующий LINQ-запрос:</span><span class="sxs-lookup"><span data-stu-id="54a78-191">For example, this LINQ query:</span></span> 

```CSharp
context.Weapons
    .OrderBy(w => w.Name.CompareTo("Marcus' Lancer") == 0)
    .ThenBy(w => w.Id)
```

<span data-ttu-id="54a78-192">раньше преобразовывался в SQL Server в:</span><span class="sxs-lookup"><span data-stu-id="54a78-192">Was on SQL Server formally translated to:</span></span>

```sql
SELECT [w].[Id], [w].[AmmunitionType], [w].[IsAutomatic], [w].[Name], [w].[OwnerFullName], [w].[SynergyWithId]
FROM [Weapons] AS [w]
ORDER BY CASE
    WHEN (CASE
        WHEN [w].[Name] = N'Marcus'' Lancer' THEN 0
        WHEN [w].[Name] > N'Marcus'' Lancer' THEN 1
        WHEN [w].[Name] < N'Marcus'' Lancer' THEN -1
    END = 0) AND CASE
        WHEN [w].[Name] = N'Marcus'' Lancer' THEN 0
        WHEN [w].[Name] > N'Marcus'' Lancer' THEN 1
        WHEN [w].[Name] < N'Marcus'' Lancer' THEN -1
    END IS NOT NULL THEN CAST(1 AS bit)
    ELSE CAST(0 AS bit)
END, [w].[Id]");
``` 

<span data-ttu-id="54a78-193">Но теперь преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="54a78-193">But is now translated to:</span></span>

```sql
SELECT [w].[Id], [w].[AmmunitionType], [w].[IsAutomatic], [w].[Name], [w].[OwnerFullName], [w].[SynergyWithId]
FROM [Weapons] AS [w]
ORDER BY CASE
    WHEN ([w].[Name] = N'Marcus'' Lancer') AND [w].[Name] IS NOT NULL THEN CAST(1 AS bit)
    ELSE CAST(0 AS bit)
END, [w].[Id]");
``` 

## <a name="preview-5"></a><span data-ttu-id="54a78-194">Предварительная версия 5</span><span class="sxs-lookup"><span data-stu-id="54a78-194">Preview 5</span></span>

### <a name="database-collations"></a><span data-ttu-id="54a78-195">Параметры сортировки баз данных</span><span class="sxs-lookup"><span data-stu-id="54a78-195">Database collations</span></span>

<span data-ttu-id="54a78-196">Модель EF теперь позволяет указывать параметры сортировки по умолчанию для базы данных.</span><span class="sxs-lookup"><span data-stu-id="54a78-196">The default collation for a database can now be specified in the EF model.</span></span> <span data-ttu-id="54a78-197">Это будет передаваться в создаваемые миграции для задания параметров сортировки при создании базы данных.</span><span class="sxs-lookup"><span data-stu-id="54a78-197">This will flow through to generated migrations to set the collation when the database is created.</span></span> <span data-ttu-id="54a78-198">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-198">For example:</span></span>

```CSharp
modelBuilder.UseCollation("German_PhoneBook_CI_AS");
```

<span data-ttu-id="54a78-199">Затем функция миграции формирует следующий код для создания базы данных в SQL Server:</span><span class="sxs-lookup"><span data-stu-id="54a78-199">Migrations then generates the following to create the database on SQL Server:</span></span>

```sql
CREATE DATABASE [Test]
COLLATE German_PhoneBook_CI_AS;
```

<span data-ttu-id="54a78-200">Можно также указать параметры сортировки, которые будут использоваться для конкретных столбцов.</span><span class="sxs-lookup"><span data-stu-id="54a78-200">The collation to use for specific database columns can also be specified.</span></span> <span data-ttu-id="54a78-201">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-201">For example:</span></span>

```CSharp
 modelBuilder
     .Entity<User>()
     .Property(e => e.Name)
     .UseCollation("German_PhoneBook_CI_AS");
```

<span data-ttu-id="54a78-202">Если функция миграции не используется, параметры сортировки реконструируются из базы данных при формировании шаблонов DbContext.</span><span class="sxs-lookup"><span data-stu-id="54a78-202">For those not using migrations, collations are now reverse-engineered from the database when scaffolding a DbContext.</span></span>

<span data-ttu-id="54a78-203">Наконец, `EF.Functions.Collate()` позволяет использовать различные параметры сортировки для нерегламентированных запросов.</span><span class="sxs-lookup"><span data-stu-id="54a78-203">Finally, the `EF.Functions.Collate()` allows for ad-hoc queries using different collations.</span></span> <span data-ttu-id="54a78-204">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-204">For example:</span></span>

```CSharp
context.Users.Single(e => EF.Functions.Collate(e.Name, "French_CI_AS") == "Jean-Michel Jarre");
```

<span data-ttu-id="54a78-205">Будет создан следующий запрос для SQL Server:</span><span class="sxs-lookup"><span data-stu-id="54a78-205">This will generate the following query for SQL Server:</span></span>

```sql
SELECT TOP(2) [u].[Id], [u].[Name]
FROM [Users] AS [u]
WHERE [u].[Name] COLLATE French_CI_AS = N'Jean-Michel Jarre'
```

<span data-ttu-id="54a78-206">Обратите внимание, что нерегламентированные параметры сортировки следует использовать с осторожностью, так как они могут негативно повлиять на производительность базы данных.</span><span class="sxs-lookup"><span data-stu-id="54a78-206">Note that ad-hoc collations should be used with care as they can negatively impact database performance.</span></span>

<span data-ttu-id="54a78-207">Это документируется в проблеме [#2273](https://github.com/dotnet/EntityFramework.Docs/issues/2273).</span><span class="sxs-lookup"><span data-stu-id="54a78-207">Documentation is tracked by issue [#2273](https://github.com/dotnet/EntityFramework.Docs/issues/2273).</span></span>

### <a name="flow-arguments-into-idesigntimedbcontextfactory"></a><span data-ttu-id="54a78-208">Передача аргументов в интерфейс IDesignTimeDbContextFactory</span><span class="sxs-lookup"><span data-stu-id="54a78-208">Flow arguments into IDesignTimeDbContextFactory</span></span>

<span data-ttu-id="54a78-209">Теперь можно передавать аргументы из командной строки в метод `CreateDbContext` интерфейса [IDesignTimeDbContextFactory](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.design.idesigntimedbcontextfactory-1?view=efcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="54a78-209">Arguments are now flowed from the command line into the `CreateDbContext` method of [IDesignTimeDbContextFactory](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.design.idesigntimedbcontextfactory-1?view=efcore-3.1).</span></span> <span data-ttu-id="54a78-210">Например, чтобы указать, что используется сборка для разработки, можно передать в командной строке настраиваемый аргумент, например `dev`:</span><span class="sxs-lookup"><span data-stu-id="54a78-210">For example, to indicate this is a dev build, a custom argument (e.g. `dev`) can be passed on the command line:</span></span>

```
dotnet ef migrations add two --verbose --dev
``` 

<span data-ttu-id="54a78-211">Затем этот аргумент будет передан в фабрику, где его можно использовать для управления созданием и инициализацией контекста.</span><span class="sxs-lookup"><span data-stu-id="54a78-211">This argument will then flow into the factory, where it can be used to control how the context is created and initialized.</span></span> <span data-ttu-id="54a78-212">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-212">For example:</span></span>

```CSharp
public class MyDbContextFactory : IDesignTimeDbContextFactory<SomeDbContext>
{
    public SomeDbContext CreateDbContext(string[] args) 
        => new SomeDbContext(args.Contains("--dev"));
}
```

<span data-ttu-id="54a78-213">Это документируется в проблеме [#2419](https://github.com/dotnet/EntityFramework.Docs/issues/2419).</span><span class="sxs-lookup"><span data-stu-id="54a78-213">Documentation is tracked by issue [#2419](https://github.com/dotnet/EntityFramework.Docs/issues/2419).</span></span>

### <a name="no-tracking-queries-with-identity-resolution"></a><span data-ttu-id="54a78-214">Запросы без отслеживания с разрешением идентификаторов</span><span class="sxs-lookup"><span data-stu-id="54a78-214">No-tracking queries with identity resolution</span></span>

<span data-ttu-id="54a78-215">Теперь можно настроить запросы без отслеживания на выполнение разрешения идентификаторов.</span><span class="sxs-lookup"><span data-stu-id="54a78-215">No-tracking queries can now be configured to perform identity resolution.</span></span> <span data-ttu-id="54a78-216">Например, следующий запрос будет создавать новый экземпляр Blog для каждого объекта Post, даже если у каждого Blog один и тот же первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="54a78-216">For example, the following query will create a new Blog instance for each Post, even if each Blog has the same primary key.</span></span> 

```CSharp
context.Posts.AsNoTracking().Include(e => e.Blog).ToList();
```

<span data-ttu-id="54a78-217">Однако за счет того, что этот запрос обычно выполняется немного медленнее и всегда использует больше памяти, его можно изменить так, чтобы создавался только один экземпляр Blog:</span><span class="sxs-lookup"><span data-stu-id="54a78-217">However, at the expense of usually being slightly slower and always using more memory, this query can be changed to ensure only a single Blog instance is created:</span></span>

```CSharp
context.Posts.AsNoTracking().PerformIdentityResolution().Include(e => e.Blog).ToList();
```

<span data-ttu-id="54a78-218">Обратите внимание, что этот вариант подходит только для запросов без отслеживания, так как все запросы с отслеживанием уже действуют таким образом.</span><span class="sxs-lookup"><span data-stu-id="54a78-218">Note that this is only useful for no-tracking queries since all tracking queries already exhibit this behavior.</span></span> <span data-ttu-id="54a78-219">Кроме того, после проверки API будет изменен синтаксис `PerformIdentityResolution`.</span><span class="sxs-lookup"><span data-stu-id="54a78-219">Also, following API review, the `PerformIdentityResolution` syntax will be changed.</span></span> <span data-ttu-id="54a78-220">См. [#19877](https://github.com/dotnet/efcore/issues/19877#issuecomment-637371073).</span><span class="sxs-lookup"><span data-stu-id="54a78-220">See [#19877](https://github.com/dotnet/efcore/issues/19877#issuecomment-637371073).</span></span>

<span data-ttu-id="54a78-221">Это документируется в проблеме [#1895](https://github.com/dotnet/EntityFramework.Docs/issues/1895).</span><span class="sxs-lookup"><span data-stu-id="54a78-221">Documentation is tracked by issue [#1895](https://github.com/dotnet/EntityFramework.Docs/issues/1895).</span></span>

### <a name="stored-persisted-computed-columns"></a><span data-ttu-id="54a78-222">Сохраняемые вычисляемые столбцы</span><span class="sxs-lookup"><span data-stu-id="54a78-222">Stored (persisted) computed columns</span></span>

<span data-ttu-id="54a78-223">Большинство баз данных позволяют сохранять значения вычисляемых столбцов после вычислений.</span><span class="sxs-lookup"><span data-stu-id="54a78-223">Most databases allow computed column values to be stored after computation.</span></span> <span data-ttu-id="54a78-224">Хотя для этого требуется место на диске, вычисляемый столбец вычисляется только один раз при обновлении, а не при каждом получении его значения.</span><span class="sxs-lookup"><span data-stu-id="54a78-224">While this takes up disk space, the computed column is calculated only once on update, instead of each time its value is retrieved.</span></span> <span data-ttu-id="54a78-225">Это также позволяет индексировать столбец для некоторых баз данных.</span><span class="sxs-lookup"><span data-stu-id="54a78-225">This also allows the column to be indexed for some databases.</span></span>

<span data-ttu-id="54a78-226">EF Core 5.0 поддерживает настройку вычисляемых столбцов как сохраняемых.</span><span class="sxs-lookup"><span data-stu-id="54a78-226">EF Core 5.0 allows computed columns to be configured as stored.</span></span> <span data-ttu-id="54a78-227">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-227">For example:</span></span>
 
```CSharp
modelBuilder
    .Entity<User>()
    .Property(e => e.SomethingComputed)
    .HasComputedColumnSql("my sql", stored: true);
```

### <a name="sqlite-computed-columns"></a><span data-ttu-id="54a78-228">Вычисляемые столбцы SQLite</span><span class="sxs-lookup"><span data-stu-id="54a78-228">SQLite computed columns</span></span>

<span data-ttu-id="54a78-229">EF Core теперь поддерживает вычисляемые столбцы в базах данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="54a78-229">EF Core now supports computed columns in SQLite databases.</span></span>

## <a name="preview-4"></a><span data-ttu-id="54a78-230">Предварительная версия 4</span><span class="sxs-lookup"><span data-stu-id="54a78-230">Preview 4</span></span>

### <a name="configure-database-precisionscale-in-model"></a><span data-ttu-id="54a78-231">Настройка точности и масштаба для базы данных в модели</span><span class="sxs-lookup"><span data-stu-id="54a78-231">Configure database precision/scale in model</span></span>

<span data-ttu-id="54a78-232">Теперь можно указать точность и масштаб для свойства с помощью построителя модели.</span><span class="sxs-lookup"><span data-stu-id="54a78-232">Precision and scale for a property can now be specified using the model builder.</span></span> <span data-ttu-id="54a78-233">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-233">For example:</span></span>

```CSharp
modelBuilder
    .Entity<Blog>()
    .Property(b => b.Numeric)
    .HasPrecision(16, 4);
```

<span data-ttu-id="54a78-234">Точность и масштаб по-прежнему можно задать, используя полный тип базы данных, например "decimal(16,4)".</span><span class="sxs-lookup"><span data-stu-id="54a78-234">Precision and scale can still be set via the full database type, such as "decimal(16,4)".</span></span> 

<span data-ttu-id="54a78-235">Документация отслеживается по проблеме [#527](https://github.com/dotnet/EntityFramework.Docs/issues/527).</span><span class="sxs-lookup"><span data-stu-id="54a78-235">Documentation is tracked by issue [#527](https://github.com/dotnet/EntityFramework.Docs/issues/527).</span></span>

### <a name="specify-sql-server-index-fill-factor"></a><span data-ttu-id="54a78-236">Указание коэффициента заполнения индекса SQL Server</span><span class="sxs-lookup"><span data-stu-id="54a78-236">Specify SQL Server index fill factor</span></span>

<span data-ttu-id="54a78-237">Теперь при создании индекса в SQL Server можно указать коэффициент заполнения.</span><span class="sxs-lookup"><span data-stu-id="54a78-237">The fill factor can now be specified when creating an index on SQL Server.</span></span> <span data-ttu-id="54a78-238">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-238">For example:</span></span>

```CSharp
modelBuilder
    .Entity<Customer>()
    .HasIndex(e => e.Name)
    .HasFillFactor(90);
```

## <a name="preview-3"></a><span data-ttu-id="54a78-239">Предварительная версия 3</span><span class="sxs-lookup"><span data-stu-id="54a78-239">Preview 3</span></span>

### <a name="filtered-include"></a><span data-ttu-id="54a78-240">Включение с фильтрацией</span><span class="sxs-lookup"><span data-stu-id="54a78-240">Filtered Include</span></span>

<span data-ttu-id="54a78-241">Метод Include теперь поддерживает фильтрацию включенных сущностей.</span><span class="sxs-lookup"><span data-stu-id="54a78-241">The Include method now supports filtering of the entities included.</span></span> <span data-ttu-id="54a78-242">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-242">For example:</span></span>

```CSharp
var blogs = context.Blogs
    .Include(e => e.Posts.Where(p => p.Title.Contains("Cheese")))
    .ToList();
```

<span data-ttu-id="54a78-243">Этот запрос возвращает блоги со всеми связанными записями, но только в том случае, если заголовок записи содержит Cheese.</span><span class="sxs-lookup"><span data-stu-id="54a78-243">This query will return blogs together with each associated post, but only when the post title contains "Cheese".</span></span>

<span data-ttu-id="54a78-244">Для сокращения числа включаемых сущностей также можно использовать методы Skip и Take.</span><span class="sxs-lookup"><span data-stu-id="54a78-244">Skip and Take can also be used to reduce the number of included entities.</span></span> <span data-ttu-id="54a78-245">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-245">For example:</span></span>
 
```CSharp
var blogs = context.Blogs
    .Include(e => e.Posts.OrderByDescending(post => post.Title).Take(5)))
    .ToList();
```
<span data-ttu-id="54a78-246">Этот запрос возвратит блоги, включив не более пяти записей для каждого блога.</span><span class="sxs-lookup"><span data-stu-id="54a78-246">This query will return blogs with at most five posts included on each blog.</span></span>

<span data-ttu-id="54a78-247">Подробные сведения см. в [документации по методу Include](xref:core/querying/related-data#filtered-include).</span><span class="sxs-lookup"><span data-stu-id="54a78-247">See the [Include documentation](xref:core/querying/related-data#filtered-include) for full details.</span></span>

### <a name="new-modelbuilder-api-for-navigation-properties"></a><span data-ttu-id="54a78-248">Новый API ModelBuilder для свойств навигации</span><span class="sxs-lookup"><span data-stu-id="54a78-248">New ModelBuilder API for navigation properties</span></span>

<span data-ttu-id="54a78-249">Свойства навигации главным образом настраиваются при [определении связей](xref:core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="54a78-249">Navigation properties are primarily configured when [defining relationships](xref:core/modeling/relationships).</span></span> <span data-ttu-id="54a78-250">Однако новый метод `Navigation` можно использовать в случаях, когда свойства навигации требуют дополнительной настройки.</span><span class="sxs-lookup"><span data-stu-id="54a78-250">However, the new `Navigation` method can be used in the cases where navigation properties need additional configuration.</span></span> <span data-ttu-id="54a78-251">Например, чтобы задать резервное поле для навигации, если не удалось найти поле по соглашению:</span><span class="sxs-lookup"><span data-stu-id="54a78-251">For example, to set a backing field for the navigation when the field would not be found by convention:</span></span>

```CSharp
modelBuilder.Entity<Blog>().Navigation(e => e.Posts).HasField("_myposts");
```

<span data-ttu-id="54a78-252">Обратите внимание, что API `Navigation` не является заменой для конфигурации связи.</span><span class="sxs-lookup"><span data-stu-id="54a78-252">Note that the `Navigation` API does not replace relationship configuration.</span></span> <span data-ttu-id="54a78-253">Вместо этого он допускает дополнительную настройку свойств навигации в уже обнаруженных или определенных связях.</span><span class="sxs-lookup"><span data-stu-id="54a78-253">Instead it allows additional configuration of navigation properties in already discovered or defined relationships.</span></span>

<span data-ttu-id="54a78-254">См. [документацию по настройке свойств навигации](xref:core/modeling/relationships#configuring-navigation-properties).</span><span class="sxs-lookup"><span data-stu-id="54a78-254">See the [Configuring Navigation Properties documentation](xref:core/modeling/relationships#configuring-navigation-properties).</span></span>

### <a name="new-command-line-parameters-for-namespaces-and-connection-strings"></a><span data-ttu-id="54a78-255">Новые параметры командной строки для пространств имен и строк подключения</span><span class="sxs-lookup"><span data-stu-id="54a78-255">New command-line parameters for namespaces and connection strings</span></span> 

<span data-ttu-id="54a78-256">Теперь можно указывать пространства имен в командной строке во время миграции и формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="54a78-256">Migrations and scaffolding now allow namespaces to be specified on the command line.</span></span> <span data-ttu-id="54a78-257">Это можно сделать, например, затем, чтобы реконструировать базу данных, поместив в нее классы контекста и модели в различных пространствах имен:</span><span class="sxs-lookup"><span data-stu-id="54a78-257">For example, to reverse engineer a database putting the context and model classes in different namespaces:</span></span> 

```
dotnet ef dbcontext scaffold "connection string" Microsoft.EntityFrameworkCore.SqlServer --context-namespace "My.Context" --namespace "My.Model"
```

<span data-ttu-id="54a78-258">Подробные сведения см. в документации по [миграции](xref:core/managing-schemas/migrations/index#namespaces) и [реконструированию](xref:core/managing-schemas/scaffolding#directories-and-namespaces).</span><span class="sxs-lookup"><span data-stu-id="54a78-258">See the [Migrations](xref:core/managing-schemas/migrations/index#namespaces) and [Reverse Engineering](xref:core/managing-schemas/scaffolding#directories-and-namespaces) documentation for full details.</span></span>

---
<span data-ttu-id="54a78-259">Кроме того, теперь можно передать строку подключения команде `database-update`.</span><span class="sxs-lookup"><span data-stu-id="54a78-259">Also, a connection string can now be passed to the `database-update` command:</span></span>

```
dotnet ef database update --connection "connection string"
```

<span data-ttu-id="54a78-260">Подробные сведения см. в [документации по средствам](xref:core/miscellaneous/cli/dotnet#dotnet-ef-database-update).</span><span class="sxs-lookup"><span data-stu-id="54a78-260">See the [Tools documentation](xref:core/miscellaneous/cli/dotnet#dotnet-ef-database-update) for full details.</span></span>

<span data-ttu-id="54a78-261">Аналогичные параметры также были добавлены в команды PowerShell, используемые в консоли диспетчера пакетов VS.</span><span class="sxs-lookup"><span data-stu-id="54a78-261">Equivalent parameters have also been added to the PowerShell commands used in the VS Package Manager Console.</span></span>

### <a name="enabledetailederrors-has-returned"></a><span data-ttu-id="54a78-262">Возвращение EnableDetailedErrors</span><span class="sxs-lookup"><span data-stu-id="54a78-262">EnableDetailedErrors has returned</span></span>

<span data-ttu-id="54a78-263">По соображениям производительности EF не выполняет дополнительные проверки на NULL при считывании значений из базы данных.</span><span class="sxs-lookup"><span data-stu-id="54a78-263">For performance reasons, EF doesn't do additional null-checks when reading values from the database.</span></span> <span data-ttu-id="54a78-264">Это может привести к возникновению исключений при обнаружении непредвиденных значений NULL, для которых будет сложно определить причину.</span><span class="sxs-lookup"><span data-stu-id="54a78-264">This can result in exceptions that are hard to root-cause when an unexpected null is encountered.</span></span>

<span data-ttu-id="54a78-265">При использовании `EnableDetailedErrors` будет добавлена дополнительная проверка на NULL для запросов, чтобы облегчить определение причин этих ошибок за счет небольшого снижения производительности.</span><span class="sxs-lookup"><span data-stu-id="54a78-265">Using `EnableDetailedErrors` will add extra null checking to queries such that, for a small performance overhead, these errors are easier to trace back to a root cause.</span></span>  

<span data-ttu-id="54a78-266">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-266">For example:</span></span>
```CSharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .EnableDetailedErrors()
        .EnableSensitiveDataLogging() // Often also useful with EnableDetailedErrors 
        .UseSqlServer(Your.SqlServerConnectionString);
```

<span data-ttu-id="54a78-267">Документация отслеживается по проблеме [#955](https://github.com/dotnet/EntityFramework.Docs/issues/955).</span><span class="sxs-lookup"><span data-stu-id="54a78-267">Documentation is tracked by issue [#955](https://github.com/dotnet/EntityFramework.Docs/issues/955).</span></span>

### <a name="cosmos-partition-keys"></a><span data-ttu-id="54a78-268">Ключи секций Cosmos</span><span class="sxs-lookup"><span data-stu-id="54a78-268">Cosmos partition keys</span></span>

<span data-ttu-id="54a78-269">Теперь можно указать в запросе ключ секции, используемый для данного запроса.</span><span class="sxs-lookup"><span data-stu-id="54a78-269">The partition key to use for a given query can now be specified in the query.</span></span> <span data-ttu-id="54a78-270">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-270">For example:</span></span>

```CSharp
await context.Set<Customer>()
             .WithPartitionKey(myPartitionKey)
             .FirstAsync();
```

<span data-ttu-id="54a78-271">Документация отслеживается по проблеме [#2199](https://github.com/dotnet/EntityFramework.Docs/issues/2199).</span><span class="sxs-lookup"><span data-stu-id="54a78-271">Documentation is tracked by issue [#2199](https://github.com/dotnet/EntityFramework.Docs/issues/2199).</span></span>

### <a name="support-for-the-sql-server-datalength-function"></a><span data-ttu-id="54a78-272">Поддержка функции DATALENGTH SQL Server</span><span class="sxs-lookup"><span data-stu-id="54a78-272">Support for the SQL Server DATALENGTH function</span></span>

<span data-ttu-id="54a78-273">К ней можно обратиться с помощью нового метода `EF.Functions.DataLength`.</span><span class="sxs-lookup"><span data-stu-id="54a78-273">This can be accessed using the new `EF.Functions.DataLength` method.</span></span> <span data-ttu-id="54a78-274">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-274">For example:</span></span>
```CSharp
var count = context.Orders.Count(c => 100 < EF.Functions.DataLength(c.OrderDate));
``` 

## <a name="preview-2"></a><span data-ttu-id="54a78-275">Предварительная версия 2</span><span class="sxs-lookup"><span data-stu-id="54a78-275">Preview 2</span></span>

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a><span data-ttu-id="54a78-276">Использование атрибута C# для указания резервного поля свойств</span><span class="sxs-lookup"><span data-stu-id="54a78-276">Use a C# attribute to specify a property backing field</span></span>

<span data-ttu-id="54a78-277">Теперь атрибут C# можно использовать для указания резервного поля для свойства.</span><span class="sxs-lookup"><span data-stu-id="54a78-277">A C# attribute can now be used to specify the backing field for a property.</span></span> <span data-ttu-id="54a78-278">Этот атрибут позволяет EF Core в обычном порядке выполнять запись и чтение из резервного поля, даже если резервное поле не может быть найдено автоматически.</span><span class="sxs-lookup"><span data-stu-id="54a78-278">This attribute allows EF Core to still write to and read from the backing field as would normally happen, even when the backing field cannot be found automatically.</span></span> <span data-ttu-id="54a78-279">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-279">For example:</span></span>

```CSharp
public class Blog
{
    private string _mainTitle;

    public int Id { get; set; }

    [BackingField(nameof(_mainTitle))]
    public string Title
    {
        get => _mainTitle;
        set => _mainTitle = value;
    }
}
```

<span data-ttu-id="54a78-280">Документация отслеживается по проблеме [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span><span class="sxs-lookup"><span data-stu-id="54a78-280">Documentation is tracked by issue [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span></span>

### <a name="complete-discriminator-mapping"></a><span data-ttu-id="54a78-281">Завершение сопоставления дискриминатора</span><span class="sxs-lookup"><span data-stu-id="54a78-281">Complete discriminator mapping</span></span>

<span data-ttu-id="54a78-282">EF Core использует столбец дискриминатора для [сопоставления TPH в иерархии наследования](/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="54a78-282">EF Core uses a discriminator column for [TPH mapping of an inheritance hierarchy](/ef/core/modeling/inheritance).</span></span> <span data-ttu-id="54a78-283">Некоторые улучшения производительности будут поддерживаться до тех пор, пока EF Core будут известны все возможные значения для дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="54a78-283">Some performance enhancements are possible so long as EF Core knows all possible values for the discriminator.</span></span> <span data-ttu-id="54a78-284">Теперь эти улучшения реализованы в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="54a78-284">EF Core 5.0 now implements these enhancements.</span></span>

<span data-ttu-id="54a78-285">Например, в предыдущих версиях EF Core всегда создавался следующий код SQL для запроса, возвращающего все типы в иерархии:</span><span class="sxs-lookup"><span data-stu-id="54a78-285">For example, previous versions of EF Core would always generate this SQL for a query returning all types in a hierarchy:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
WHERE [a].[Discriminator] IN (N'Animal', N'Cat', N'Dog', N'Human')
```

<span data-ttu-id="54a78-286">Теперь при настройке полного сопоставления дискриминатора EF Core 5.0 будет формировать следующее:</span><span class="sxs-lookup"><span data-stu-id="54a78-286">EF Core 5.0 will now generate the following when a complete discriminator mapping is configured:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
```

<span data-ttu-id="54a78-287">Начиная с предварительной версии 3 это поведение будет доступно по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="54a78-287">It will be the default behavior starting with preview 3.</span></span>

### <a name="performance-improvements-in-microsoftdatasqlite"></a><span data-ttu-id="54a78-288">Улучшения производительности в Microsoft.Data.Sqlite</span><span class="sxs-lookup"><span data-stu-id="54a78-288">Performance improvements in Microsoft.Data.Sqlite</span></span>

<span data-ttu-id="54a78-289">В производительность SQLIte были внесены два улучшения:</span><span class="sxs-lookup"><span data-stu-id="54a78-289">We have made two performance improvements for SQLIte:</span></span>

* <span data-ttu-id="54a78-290">Теперь получение двоичных и строковых данных с помощью GetBytes, GetChars и GetTextReader осуществляется более эффективно за счет использования SqliteBlob и потоков.</span><span class="sxs-lookup"><span data-stu-id="54a78-290">Retrieving binary and string data with GetBytes, GetChars, and GetTextReader is now more efficient by making use of SqliteBlob and streams.</span></span>
* <span data-ttu-id="54a78-291">Инициализация SqliteConnection выполняется в отложенном режиме.</span><span class="sxs-lookup"><span data-stu-id="54a78-291">Initialization of SqliteConnection is now lazy.</span></span>

<span data-ttu-id="54a78-292">Эти улучшения добавлены в поставщик Microsoft.Data.Sqlite ADO.NET и, следовательно, позволяют повысить производительность за пределами EF Core.</span><span class="sxs-lookup"><span data-stu-id="54a78-292">These improvements are in the ADO.NET Microsoft.Data.Sqlite provider and hence also improve performance outside of EF Core.</span></span>

## <a name="preview-1"></a><span data-ttu-id="54a78-293">Предварительная версия 1</span><span class="sxs-lookup"><span data-stu-id="54a78-293">Preview 1</span></span>

### <a name="simple-logging"></a><span data-ttu-id="54a78-294">Простое ведение журнала</span><span class="sxs-lookup"><span data-stu-id="54a78-294">Simple logging</span></span>

<span data-ttu-id="54a78-295">Эта функция добавляет возможность, аналогичную `Database.Log` в EF6.</span><span class="sxs-lookup"><span data-stu-id="54a78-295">This feature adds functionality similar to `Database.Log` in EF6.</span></span> <span data-ttu-id="54a78-296">Это значит, что она позволяет легко получать журналы из EF Core без настройки какой-либо внешней платформы ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="54a78-296">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="54a78-297">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 5 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="54a78-297">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="54a78-298">Дополнительная документация отслеживается по проблеме [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span><span class="sxs-lookup"><span data-stu-id="54a78-298">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="54a78-299">Простой способ получения созданного кода SQL</span><span class="sxs-lookup"><span data-stu-id="54a78-299">Simple way to get generated SQL</span></span>

<span data-ttu-id="54a78-300">В EF Core 5.0 представлен метод расширения `ToQueryString`, который будет возвращать код SQL, создаваемый в EF Core при выполнении запроса LINQ.</span><span class="sxs-lookup"><span data-stu-id="54a78-300">EF Core 5.0 introduces the `ToQueryString` extension method, which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="54a78-301">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 9 января 2020 г](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span><span class="sxs-lookup"><span data-stu-id="54a78-301">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="54a78-302">Дополнительная документация отслеживается по проблеме [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span><span class="sxs-lookup"><span data-stu-id="54a78-302">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a><span data-ttu-id="54a78-303">Использование атрибута C# для указания того, что сущность не имеет ключа</span><span class="sxs-lookup"><span data-stu-id="54a78-303">Use a C# attribute to indicate that an entity has no key</span></span>

<span data-ttu-id="54a78-304">Теперь можно настроить тип сущности, не имеющей ключа, используя новый атрибут `KeylessAttribute`.</span><span class="sxs-lookup"><span data-stu-id="54a78-304">An entity type can now be configured as having no key using the new `KeylessAttribute`.</span></span> <span data-ttu-id="54a78-305">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-305">For example:</span></span>

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

<span data-ttu-id="54a78-306">Документация отслеживается по проблеме [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span><span class="sxs-lookup"><span data-stu-id="54a78-306">Documentation is tracked by issue [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="54a78-307">Возможность изменения подключения или строки подключения в инициализированном DbContext</span><span class="sxs-lookup"><span data-stu-id="54a78-307">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="54a78-308">Теперь создание экземпляра DbContext без подключения или строки подключения стало еще проще.</span><span class="sxs-lookup"><span data-stu-id="54a78-308">It is now easier to create a DbContext instance without any connection or connection string.</span></span> <span data-ttu-id="54a78-309">Кроме того, подключение или строку подключения можно изменить в экземпляре контекста.</span><span class="sxs-lookup"><span data-stu-id="54a78-309">Also, the connection or connection string can now be mutated on the context instance.</span></span> <span data-ttu-id="54a78-310">Эта возможность позволяет одному и тому же экземпляру контекста динамически подключаться к разным базам данных.</span><span class="sxs-lookup"><span data-stu-id="54a78-310">This feature allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="54a78-311">Документация отслеживается по проблеме [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span><span class="sxs-lookup"><span data-stu-id="54a78-311">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="54a78-312">Прокси-серверы отслеживания изменений</span><span class="sxs-lookup"><span data-stu-id="54a78-312">Change-tracking proxies</span></span>

<span data-ttu-id="54a78-313">Теперь EF Core может создавать прокси-серверы среды выполнения, которые автоматически реализуют интерфейсы [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) и [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="54a78-313">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span> <span data-ttu-id="54a78-314">Затем они сообщают об изменениях значений свойств сущности напрямую в EF Core, что позволяет не выполнять проверку на наличие изменений.</span><span class="sxs-lookup"><span data-stu-id="54a78-314">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span> <span data-ttu-id="54a78-315">Однако прокси-серверы имеют собственный набор ограничений, поэтому они предназначены не для всех.</span><span class="sxs-lookup"><span data-stu-id="54a78-315">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="54a78-316">Документация отслеживается по проблеме [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span><span class="sxs-lookup"><span data-stu-id="54a78-316">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="54a78-317">Улучшенные представления отладки</span><span class="sxs-lookup"><span data-stu-id="54a78-317">Enhanced debug views</span></span>

<span data-ttu-id="54a78-318">В представлениях отладки удобно просматривать внутренние компоненты EF Core при отладке.</span><span class="sxs-lookup"><span data-stu-id="54a78-318">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span> <span data-ttu-id="54a78-319">Представление отладки для модели было реализовано некоторое время назад.</span><span class="sxs-lookup"><span data-stu-id="54a78-319">A debug view for the Model was implemented some time ago.</span></span> <span data-ttu-id="54a78-320">В EF Core 5.0 представление модели стало более удобочитаемым. Кроме того, добавлено новое представление отладки для отслеживаемых сущностей в диспетчере состояний.</span><span class="sxs-lookup"><span data-stu-id="54a78-320">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="54a78-321">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 12 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span><span class="sxs-lookup"><span data-stu-id="54a78-321">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="54a78-322">Дополнительная документация отслеживается по проблеме [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span><span class="sxs-lookup"><span data-stu-id="54a78-322">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="54a78-323">Улучшенная обработка семантики значений NULL в базе данных</span><span class="sxs-lookup"><span data-stu-id="54a78-323">Improved handling of database null semantics</span></span>

<span data-ttu-id="54a78-324">Реляционные базы данных обычно обрабатывают NULL как неизвестное значение и, следовательно, как не равное любому другому значению NULL.</span><span class="sxs-lookup"><span data-stu-id="54a78-324">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span> <span data-ttu-id="54a78-325">C# рассматривает NULL как определенное значение, которое аналогично любому другому значению NULL.</span><span class="sxs-lookup"><span data-stu-id="54a78-325">While C# treats null as a defined value, which compares equal to any other null.</span></span> <span data-ttu-id="54a78-326">EF Core по умолчанию преобразует запросы так, чтобы они использовали семантику значений NULL C#.</span><span class="sxs-lookup"><span data-stu-id="54a78-326">EF Core by default translates queries so that they use C# null semantics.</span></span> <span data-ttu-id="54a78-327">В EF Core 5.0 эффективность таких преобразований значительно повышена.</span><span class="sxs-lookup"><span data-stu-id="54a78-327">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="54a78-328">Документация отслеживается по проблеме [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span><span class="sxs-lookup"><span data-stu-id="54a78-328">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="54a78-329">Свойства индексатора</span><span class="sxs-lookup"><span data-stu-id="54a78-329">Indexer properties</span></span>

<span data-ttu-id="54a78-330">EF Core 5.0 поддерживает сопоставление свойств индексатора C#.</span><span class="sxs-lookup"><span data-stu-id="54a78-330">EF Core 5.0 supports mapping of C# indexer properties.</span></span> <span data-ttu-id="54a78-331">Эти свойства позволяют сущностям выступать в качестве контейнеров свойств, в которых столбцы сопоставляются с именованными свойствами в контейнере.</span><span class="sxs-lookup"><span data-stu-id="54a78-331">These properties allow entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="54a78-332">Документация отслеживается по проблеме [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span><span class="sxs-lookup"><span data-stu-id="54a78-332">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="54a78-333">Создание ограничений CHECK для сопоставлений перечислений</span><span class="sxs-lookup"><span data-stu-id="54a78-333">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="54a78-334">Функция миграции в EF Core 5.0 теперь может создавать ограничения CHECK для сопоставлений свойств перечисления.</span><span class="sxs-lookup"><span data-stu-id="54a78-334">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span> <span data-ttu-id="54a78-335">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-335">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="54a78-336">Документация отслеживается по проблеме [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span><span class="sxs-lookup"><span data-stu-id="54a78-336">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="isrelational"></a><span data-ttu-id="54a78-337">Является реляционным</span><span class="sxs-lookup"><span data-stu-id="54a78-337">IsRelational</span></span>

<span data-ttu-id="54a78-338">В дополнение к существующим методам `IsSqlServer`, `IsSqlite` и `IsInMemory` был добавлен новый метод `IsRelational`.</span><span class="sxs-lookup"><span data-stu-id="54a78-338">A new `IsRelational` method has been added in addition to the existing `IsSqlServer`, `IsSqlite`, and `IsInMemory`.</span></span> <span data-ttu-id="54a78-339">Этот метод можно использовать для проверки того, использует ли DbContext какой-либо поставщик реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="54a78-339">This method can be used to test if the DbContext is using any relational database provider.</span></span> <span data-ttu-id="54a78-340">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-340">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

<span data-ttu-id="54a78-341">Документация отслеживается по проблеме [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span><span class="sxs-lookup"><span data-stu-id="54a78-341">Documentation is tracked by issue [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span></span>

### <a name="cosmos-optimistic-concurrency-with-etags"></a><span data-ttu-id="54a78-342">Оптимистический параллелизм Cosmos с тегами ETag</span><span class="sxs-lookup"><span data-stu-id="54a78-342">Cosmos optimistic concurrency with ETags</span></span>

<span data-ttu-id="54a78-343">Поставщик базы данных Azure Cosmos DB теперь поддерживает оптимистичный параллелизм с использованием тегов ETag.</span><span class="sxs-lookup"><span data-stu-id="54a78-343">The Azure Cosmos DB database provider now supports optimistic concurrency using ETags.</span></span> <span data-ttu-id="54a78-344">Используйте построитель моделей в OnModelCreating для настройки ETag:</span><span class="sxs-lookup"><span data-stu-id="54a78-344">Use the model builder in OnModelCreating to configure an ETag:</span></span>

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

<span data-ttu-id="54a78-345">Затем SaveChanges генерирует исключение `DbUpdateConcurrencyException` при конфликте параллелизма, которое [можно обработать](https://docs.microsoft.com/ef/core/saving/concurrency) для реализации повторных попыток и т. д.</span><span class="sxs-lookup"><span data-stu-id="54a78-345">SaveChanges will then throw an `DbUpdateConcurrencyException` on a concurrency conflict, which [can be handled](https://docs.microsoft.com/ef/core/saving/concurrency) to implement retries, etc.</span></span>

<span data-ttu-id="54a78-346">Документация отслеживается по проблеме [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span><span class="sxs-lookup"><span data-stu-id="54a78-346">Documentation is tracked by issue [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="54a78-347">Преобразования запросов для дополнительных конструкций DateTime</span><span class="sxs-lookup"><span data-stu-id="54a78-347">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="54a78-348">Теперь запросы, содержащие новую конструкцию DateTime, поддерживают преобразование.</span><span class="sxs-lookup"><span data-stu-id="54a78-348">Queries containing new DateTime construction are now translated.</span></span>

<span data-ttu-id="54a78-349">Кроме того, теперь сопоставлены следующие функции SQL Server:</span><span class="sxs-lookup"><span data-stu-id="54a78-349">In addition, the following SQL Server functions are now mapped:</span></span>

* <span data-ttu-id="54a78-350">DateDiffWeek</span><span class="sxs-lookup"><span data-stu-id="54a78-350">DateDiffWeek</span></span>
* <span data-ttu-id="54a78-351">DateFromParts</span><span class="sxs-lookup"><span data-stu-id="54a78-351">DateFromParts</span></span>

<span data-ttu-id="54a78-352">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-352">For example:</span></span>

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

<span data-ttu-id="54a78-353">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="54a78-353">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="54a78-354">Преобразования запросов для дополнительных конструкций массивов байтов</span><span class="sxs-lookup"><span data-stu-id="54a78-354">Query translations for more byte array constructs</span></span>

<span data-ttu-id="54a78-355">Запросы, использующие свойства Contains, Length, SequenceEqual и т. д. в массиве byte[], преобразуются в SQL.</span><span class="sxs-lookup"><span data-stu-id="54a78-355">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="54a78-356">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 5 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="54a78-356">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="54a78-357">Дополнительная документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="54a78-357">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="54a78-358">Преобразование запросов, использующих Reverse</span><span class="sxs-lookup"><span data-stu-id="54a78-358">Query translation for Reverse</span></span>

<span data-ttu-id="54a78-359">Теперь можно преобразовывать запросы, использующие `Reverse`.</span><span class="sxs-lookup"><span data-stu-id="54a78-359">Queries using `Reverse` are now translated.</span></span> <span data-ttu-id="54a78-360">Пример:</span><span class="sxs-lookup"><span data-stu-id="54a78-360">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="54a78-361">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="54a78-361">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="54a78-362">Преобразование запросов для побитовых операторов</span><span class="sxs-lookup"><span data-stu-id="54a78-362">Query translation for bitwise operators</span></span>

<span data-ttu-id="54a78-363">Запросы, использующие побитовые операторы, теперь преобразуются в ряде дополнительных случаев, например:</span><span class="sxs-lookup"><span data-stu-id="54a78-363">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="54a78-364">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="54a78-364">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="54a78-365">Преобразование запросов для строк в Cosmos</span><span class="sxs-lookup"><span data-stu-id="54a78-365">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="54a78-366">Запросы, использующие строковые методы Contains, StartsWith и EndsWith, теперь преобразуются при использовании поставщика Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="54a78-366">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="54a78-367">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="54a78-367">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
