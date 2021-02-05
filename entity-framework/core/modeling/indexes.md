---
title: Индексы — EF Core
description: Настройка индексов в модели Entity Framework Core
author: roji
ms.date: 12/16/2019
uid: core/modeling/indexes
ms.openlocfilehash: ab81b108c4ff518cf98b7e835da3553c0c41efed
ms.sourcegitcommit: 7700840119b1639275f3b64836e7abb59103f2e7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2021
ms.locfileid: "98128541"
---
# <a name="indexes"></a><span data-ttu-id="50f58-103">Индексы</span><span class="sxs-lookup"><span data-stu-id="50f58-103">Indexes</span></span>

<span data-ttu-id="50f58-104">Индексы являются распространенной концепцией для многих хранилищ данных.</span><span class="sxs-lookup"><span data-stu-id="50f58-104">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="50f58-105">Хотя их реализация в хранилище данных может различаться, они используются для выполнения уточняющих запросов, основанных на столбце (или наборе столбцов), которые более эффективны.</span><span class="sxs-lookup"><span data-stu-id="50f58-105">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span> <span data-ttu-id="50f58-106">Дополнительные сведения о хорошем использовании индекса см. в [разделе "индексы](xref:core/performance/efficient-querying#use-indexes-properly) " в документации по производительности.</span><span class="sxs-lookup"><span data-stu-id="50f58-106">See the [indexes section](xref:core/performance/efficient-querying#use-indexes-properly) in the performance documentation for more information on good index usage.</span></span>

<span data-ttu-id="50f58-107">Индекс для столбца можно указать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="50f58-107">You can specify an index over a column as follows:</span></span>

## <a name="data-annotations"></a>[<span data-ttu-id="50f58-108">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="50f58-108">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Index.cs?name=Index&highlight=1)]

> [!NOTE]
> <span data-ttu-id="50f58-109">Настройка индексов с помощью заметок к данным появилась в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="50f58-109">Configuring indexes via Data Annotations has been introduced in EF Core 5.0.</span></span>

## <a name="fluent-api"></a>[<span data-ttu-id="50f58-110">Текучий API</span><span class="sxs-lookup"><span data-stu-id="50f58-110">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=4)]

***

> [!NOTE]
> <span data-ttu-id="50f58-111">По соглашению индекс создается в каждом свойстве (или наборе свойств), используемом в качестве внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="50f58-111">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>
>
> <span data-ttu-id="50f58-112">EF Core поддерживает только один индекс для каждого отдельного набора свойств.</span><span class="sxs-lookup"><span data-stu-id="50f58-112">EF Core only supports one index per distinct set of properties.</span></span> <span data-ttu-id="50f58-113">Если настроить индекс для набора свойств, для которого уже определен индекс, в соответствии с соглашением или предыдущей конфигурацией, то будет изменено определение этого индекса.</span><span class="sxs-lookup"><span data-stu-id="50f58-113">If you configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="50f58-114">Это полезно, если необходимо дополнительно настроить индекс, созданный по соглашению.</span><span class="sxs-lookup"><span data-stu-id="50f58-114">This is useful if you want to further configure an index that was created by convention.</span></span>

## <a name="composite-index"></a><span data-ttu-id="50f58-115">Составной индекс</span><span class="sxs-lookup"><span data-stu-id="50f58-115">Composite index</span></span>

<span data-ttu-id="50f58-116">Индекс может также охватывать более одного столбца:</span><span class="sxs-lookup"><span data-stu-id="50f58-116">An index can also span more than one column:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="50f58-117">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="50f58-117">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IndexComposite.cs?name=Composite&highlight=1)]

### <a name="fluent-api"></a>[<span data-ttu-id="50f58-118">Текучий API</span><span class="sxs-lookup"><span data-stu-id="50f58-118">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=4)]

***

<span data-ttu-id="50f58-119">Индексы над несколькими столбцами, также известные как *Составные индексы*, ускоряют запросы, которые фильтруются по столбцам индекса, но также запросы, которые фильтруют только по *первым* столбцам, охватываемым индексом.</span><span class="sxs-lookup"><span data-stu-id="50f58-119">Indexes over multiple columns, also known as *composite indexes*, speed up queries which filter on index's columns, but also queries which only filter on the *first* columns covered by the index.</span></span> <span data-ttu-id="50f58-120">Дополнительные сведения см. в документации по [производительности](xref:core/performance/efficient-querying#use-indexes-properly) .</span><span class="sxs-lookup"><span data-stu-id="50f58-120">See the [performance docs](xref:core/performance/efficient-querying#use-indexes-properly) for more information.</span></span>

## <a name="index-uniqueness"></a><span data-ttu-id="50f58-121">Уникальность индекса</span><span class="sxs-lookup"><span data-stu-id="50f58-121">Index uniqueness</span></span>

<span data-ttu-id="50f58-122">По умолчанию индексы не являются уникальными: несколько строк могут иметь одинаковые значения для набора столбцов индекса.</span><span class="sxs-lookup"><span data-stu-id="50f58-122">By default, indexes aren't unique: multiple rows are allowed to have the same value(s) for the index's column set.</span></span> <span data-ttu-id="50f58-123">Уникальный индекс можно сделать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="50f58-123">You can make an index unique as follows:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="50f58-124">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="50f58-124">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IndexUnique.cs?name=IndexUnique&highlight=1)]

### <a name="fluent-api"></a>[<span data-ttu-id="50f58-125">Текучий API</span><span class="sxs-lookup"><span data-stu-id="50f58-125">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=IndexUnique&highlight=5)]

***

<span data-ttu-id="50f58-126">Попытка вставить более одной сущности с одинаковыми значениями для набора столбцов индекса приведет к возникновению исключения.</span><span class="sxs-lookup"><span data-stu-id="50f58-126">Attempting to insert more than one entity with the same values for the index's column set will cause an exception to be thrown.</span></span>

## <a name="index-name"></a><span data-ttu-id="50f58-127">Имя индекса</span><span class="sxs-lookup"><span data-stu-id="50f58-127">Index name</span></span>

<span data-ttu-id="50f58-128">По соглашению для индексов, созданных в реляционной базе данных, используются имена `IX_<type name>_<property name>` .</span><span class="sxs-lookup"><span data-stu-id="50f58-128">By convention, indexes created in a relational database are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="50f58-129">Для составных индексов `<property name>` преобразуется в список имен свойств с разделителями подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="50f58-129">For composite indexes, `<property name>` becomes an underscore separated list of property names.</span></span>

<span data-ttu-id="50f58-130">Вы можете задать имя индекса, созданного в базе данных:</span><span class="sxs-lookup"><span data-stu-id="50f58-130">You can set the name of the index created in the database:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="50f58-131">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="50f58-131">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IndexName.cs?name=IndexName&highlight=1)]

### <a name="fluent-api"></a>[<span data-ttu-id="50f58-132">Текучий API</span><span class="sxs-lookup"><span data-stu-id="50f58-132">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexName.cs?name=IndexName&highlight=5)]

***

## <a name="index-filter"></a><span data-ttu-id="50f58-133">Фильтр индекса</span><span class="sxs-lookup"><span data-stu-id="50f58-133">Index filter</span></span>

<span data-ttu-id="50f58-134">Некоторые реляционные базы данных позволяют указать отфильтрованный или частичный индекс.</span><span class="sxs-lookup"><span data-stu-id="50f58-134">Some relational databases allow you to specify a filtered or partial index.</span></span> <span data-ttu-id="50f58-135">Это позволяет индексировать только подмножество значений столбца, уменьшая размер индекса и повышая производительность и использование дискового пространства.</span><span class="sxs-lookup"><span data-stu-id="50f58-135">This allows you to index only a subset of a column's values, reducing the index's size and improving both performance and disk space usage.</span></span> <span data-ttu-id="50f58-136">Дополнительные сведения об SQL Server фильтруемых индексах [см. в документации](/sql/relational-databases/indexes/create-filtered-indexes).</span><span class="sxs-lookup"><span data-stu-id="50f58-136">For more information on SQL Server filtered indexes, [see the documentation](/sql/relational-databases/indexes/create-filtered-indexes).</span></span>

<span data-ttu-id="50f58-137">API-интерфейс Fluent можно использовать для указания фильтра по индексу, который указывается в виде выражения SQL:</span><span class="sxs-lookup"><span data-stu-id="50f58-137">You can use the Fluent API to specify a filter on an index, provided as a SQL expression:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexFilter.cs?name=IndexFilter&highlight=5)]

<span data-ttu-id="50f58-138">При использовании поставщика SQL Server EF добавляет `'IS NOT NULL'` Фильтр для всех столбцов, допускающих значение null, которые являются частью уникального индекса.</span><span class="sxs-lookup"><span data-stu-id="50f58-138">When using the SQL Server provider EF adds an `'IS NOT NULL'` filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="50f58-139">Чтобы переопределить это соглашение, можно указать `null` значение.</span><span class="sxs-lookup"><span data-stu-id="50f58-139">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexNoFilter.cs?name=IndexNoFilter&highlight=6)]

## <a name="included-columns"></a><span data-ttu-id="50f58-140">Включенные столбцы</span><span class="sxs-lookup"><span data-stu-id="50f58-140">Included columns</span></span>

<span data-ttu-id="50f58-141">Некоторые реляционные базы данных позволяют настроить набор столбцов, которые будут включены в индекс, но не являются частью ключа.</span><span class="sxs-lookup"><span data-stu-id="50f58-141">Some relational databases allow you to configure a set of columns which get included in the index, but aren't part of its "key".</span></span> <span data-ttu-id="50f58-142">Это может значительно повысить производительность запросов, если все столбцы в запросе включены в индекс как ключевые или неключевые столбцы, так как к самой таблице не требуется доступ.</span><span class="sxs-lookup"><span data-stu-id="50f58-142">This can significantly improve query performance when all columns in the query are included in the index either as key or nonkey columns, as the table itself doesn't need to be accessed.</span></span> <span data-ttu-id="50f58-143">Дополнительные сведения о SQL Server включенных столбцах [см. в документации](/sql/relational-databases/indexes/create-indexes-with-included-columns).</span><span class="sxs-lookup"><span data-stu-id="50f58-143">For more information on SQL Server included columns, [see the documentation](/sql/relational-databases/indexes/create-indexes-with-included-columns).</span></span>

<span data-ttu-id="50f58-144">В следующем примере `Url` столбец является частью ключа индекса, поэтому для любой фильтрации запросов в этом столбце может использоваться индекс.</span><span class="sxs-lookup"><span data-stu-id="50f58-144">In the following example, the `Url` column is part of the index key, so any query filtering on that column can use the index.</span></span> <span data-ttu-id="50f58-145">Кроме того, запросы, обращающиеся только `Title` к `PublishedOn` столбцам и, не требуют доступа к таблице и будут работать более эффективно:</span><span class="sxs-lookup"><span data-stu-id="50f58-145">But in addition, queries accessing only the `Title` and `PublishedOn` columns will not need to access the table and will run more efficiently:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexInclude.cs?name=IndexInclude&highlight=5-9)]
