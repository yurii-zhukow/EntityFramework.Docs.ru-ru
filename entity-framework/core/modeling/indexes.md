---
title: Индексы — EF Core
author: roji
ms.date: 12/16/2019
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 9565b499ababace3595153e7159e017d2df1cc5a
ms.sourcegitcommit: 949faaba02e07e44359e77d7935f540af5c32093
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2020
ms.locfileid: "87526749"
---
# <a name="indexes"></a><span data-ttu-id="22242-102">Индексы</span><span class="sxs-lookup"><span data-stu-id="22242-102">Indexes</span></span>

<span data-ttu-id="22242-103">Индексы являются распространенной концепцией для многих хранилищ данных.</span><span class="sxs-lookup"><span data-stu-id="22242-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="22242-104">Хотя их реализация в хранилище данных может различаться, они используются для выполнения уточняющих запросов, основанных на столбце (или наборе столбцов), которые более эффективны.</span><span class="sxs-lookup"><span data-stu-id="22242-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

<span data-ttu-id="22242-105">Индексы нельзя создавать с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="22242-105">Indexes cannot be created using data annotations.</span></span> <span data-ttu-id="22242-106">API-интерфейс Fluent можно использовать для указания индекса в одном столбце следующим образом:</span><span class="sxs-lookup"><span data-stu-id="22242-106">You can use the Fluent API to specify an index on a single column as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=4)]

<span data-ttu-id="22242-107">Кроме того, можно указать индекс более чем для одного столбца:</span><span class="sxs-lookup"><span data-stu-id="22242-107">You can also specify an index over more than one column:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=4)]

> [!NOTE]
> <span data-ttu-id="22242-108">По соглашению индекс создается в каждом свойстве (или наборе свойств), используемом в качестве внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="22242-108">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>
>
> <span data-ttu-id="22242-109">EF Core поддерживает только один индекс для каждого отдельного набора свойств.</span><span class="sxs-lookup"><span data-stu-id="22242-109">EF Core only supports one index per distinct set of properties.</span></span> <span data-ttu-id="22242-110">Если вы используете API Fluent для настройки индекса для набора свойств, в котором уже определен индекс, либо в соответствии с соглашением, либо с помощью предыдущей конфигурации, вы измените определение этого индекса.</span><span class="sxs-lookup"><span data-stu-id="22242-110">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="22242-111">Это полезно, если необходимо дополнительно настроить индекс, созданный по соглашению.</span><span class="sxs-lookup"><span data-stu-id="22242-111">This is useful if you want to further configure an index that was created by convention.</span></span>

## <a name="index-uniqueness"></a><span data-ttu-id="22242-112">Уникальность индекса</span><span class="sxs-lookup"><span data-stu-id="22242-112">Index uniqueness</span></span>

<span data-ttu-id="22242-113">По умолчанию индексы не являются уникальными: несколько строк могут иметь одинаковые значения для набора столбцов индекса.</span><span class="sxs-lookup"><span data-stu-id="22242-113">By default, indexes aren't unique: multiple rows are allowed to have the same value(s) for the index's column set.</span></span> <span data-ttu-id="22242-114">Уникальный индекс можно сделать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="22242-114">You can make an index unique as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=IndexUnique&highlight=5)]

<span data-ttu-id="22242-115">Попытка вставить более одной сущности с одинаковыми значениями для набора столбцов индекса приведет к возникновению исключения.</span><span class="sxs-lookup"><span data-stu-id="22242-115">Attempting to insert more than one entity with the same values for the index's column set will cause an exception to be thrown.</span></span>

## <a name="index-name"></a><span data-ttu-id="22242-116">Имя индекса</span><span class="sxs-lookup"><span data-stu-id="22242-116">Index name</span></span>

<span data-ttu-id="22242-117">По соглашению для индексов, созданных в реляционной базе данных, используются имена `IX_<type name>_<property name>` .</span><span class="sxs-lookup"><span data-stu-id="22242-117">By convention, indexes created in a relational database are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="22242-118">Для составных индексов `<property name>` преобразуется в список имен свойств с разделителями подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="22242-118">For composite indexes, `<property name>` becomes an underscore separated list of property names.</span></span>

<span data-ttu-id="22242-119">Для задания имени индекса, созданного в базе данных, можно использовать API-интерфейс Fluent.</span><span class="sxs-lookup"><span data-stu-id="22242-119">You can use the Fluent API to set the name of the index created in the database:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexName.cs?name=IndexName&highlight=5)]

## <a name="index-filter"></a><span data-ttu-id="22242-120">Фильтр индекса</span><span class="sxs-lookup"><span data-stu-id="22242-120">Index filter</span></span>

<span data-ttu-id="22242-121">Некоторые реляционные базы данных позволяют указать отфильтрованный или частичный индекс.</span><span class="sxs-lookup"><span data-stu-id="22242-121">Some relational databases allow you to specify a filtered or partial index.</span></span> <span data-ttu-id="22242-122">Это позволяет индексировать только подмножество значений столбца, уменьшая размер индекса и повышая производительность и использование дискового пространства.</span><span class="sxs-lookup"><span data-stu-id="22242-122">This allows you to index only a subset of a column's values, reducing the index's size and improving both performance and disk space usage.</span></span> <span data-ttu-id="22242-123">Дополнительные сведения об SQL Server фильтруемых индексах [см. в документации](/sql/relational-databases/indexes/create-filtered-indexes).</span><span class="sxs-lookup"><span data-stu-id="22242-123">For more information on SQL Server filtered indexes, [see the documentation](/sql/relational-databases/indexes/create-filtered-indexes).</span></span>

<span data-ttu-id="22242-124">API-интерфейс Fluent можно использовать для указания фильтра по индексу, который указывается в виде выражения SQL:</span><span class="sxs-lookup"><span data-stu-id="22242-124">You can use the Fluent API to specify a filter on an index, provided as a SQL expression:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexFilter.cs?name=IndexFilter&highlight=5)]

<span data-ttu-id="22242-125">При использовании поставщика SQL Server EF добавляет `'IS NOT NULL'` Фильтр для всех столбцов, допускающих значение null, которые являются частью уникального индекса.</span><span class="sxs-lookup"><span data-stu-id="22242-125">When using the SQL Server provider EF adds an `'IS NOT NULL'` filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="22242-126">Чтобы переопределить это соглашение, можно указать `null` значение.</span><span class="sxs-lookup"><span data-stu-id="22242-126">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexNoFilter.cs?name=IndexNoFilter&highlight=6)]

## <a name="included-columns"></a><span data-ttu-id="22242-127">Включенные столбцы</span><span class="sxs-lookup"><span data-stu-id="22242-127">Included columns</span></span>

<span data-ttu-id="22242-128">Некоторые реляционные базы данных позволяют настроить набор столбцов, которые будут включены в индекс, но не являются частью ключа.</span><span class="sxs-lookup"><span data-stu-id="22242-128">Some relational databases allow you to configure a set of columns which get included in the index, but aren't part of its "key".</span></span> <span data-ttu-id="22242-129">Это может значительно повысить производительность запросов, если все столбцы в запросе включены в индекс как ключевые или неключевые столбцы, так как к самой таблице не требуется доступ.</span><span class="sxs-lookup"><span data-stu-id="22242-129">This can significantly improve query performance when all columns in the query are included in the index either as key or nonkey columns, as the table itself doesn't need to be accessed.</span></span> <span data-ttu-id="22242-130">Дополнительные сведения о SQL Server включенных столбцах [см. в документации](/sql/relational-databases/indexes/create-indexes-with-included-columns).</span><span class="sxs-lookup"><span data-stu-id="22242-130">For more information on SQL Server included columns, [see the documentation](/sql/relational-databases/indexes/create-indexes-with-included-columns).</span></span>

<span data-ttu-id="22242-131">В следующем примере `Url` столбец является частью ключа индекса, поэтому для любой фильтрации запросов в этом столбце может использоваться индекс.</span><span class="sxs-lookup"><span data-stu-id="22242-131">In the following example, the `Url` column is part of the index key, so any query filtering on that column can use the index.</span></span> <span data-ttu-id="22242-132">Кроме того, запросы, обращающиеся только `Title` к `PublishedOn` столбцам и, не требуют доступа к таблице и будут работать более эффективно:</span><span class="sxs-lookup"><span data-stu-id="22242-132">But in addition, queries accessing only the `Title` and `PublishedOn` columns will not need to access the table and will run more efficiently:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexInclude.cs?name=IndexInclude&highlight=5-9)]
