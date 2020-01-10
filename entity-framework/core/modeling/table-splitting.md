---
title: Разбиение таблицы — EF Core
description: Настройка разделения таблиц с помощью Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 01/03/2020
uid: core/modeling/table-splitting
ms.openlocfilehash: c38d3ee0efa82f84a1051017ae40c9f3fdd57f1f
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781174"
---
# <a name="table-splitting"></a><span data-ttu-id="b8995-103">Разделение таблиц</span><span class="sxs-lookup"><span data-stu-id="b8995-103">Table Splitting</span></span>

<span data-ttu-id="b8995-104">EF Core позволяет сопоставлять две или более сущностей с одной строкой.</span><span class="sxs-lookup"><span data-stu-id="b8995-104">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="b8995-105">Это называется _разбиением таблицы_ или _совместно используемой таблицей_.</span><span class="sxs-lookup"><span data-stu-id="b8995-105">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="b8995-106">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="b8995-106">Configuration</span></span>

<span data-ttu-id="b8995-107">Чтобы использовать разделение таблиц, типы сущностей должны быть сопоставлены с одной и той же таблицей, иметь первичные ключи, сопоставленные с теми же столбцами и по крайней мере одну связь между первичным ключом одного типа сущности и другой в той же таблице.</span><span class="sxs-lookup"><span data-stu-id="b8995-107">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="b8995-108">Распространенным сценарием разбиения таблицы является использование только подмножества столбцов в таблице для повышения производительности или инкапсуляции.</span><span class="sxs-lookup"><span data-stu-id="b8995-108">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="b8995-109">В этом примере `Order` представляет подмножество `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="b8995-109">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="b8995-110">В дополнение к требуемой конфигурации мы вызываем `Property(o => o.Status).HasColumnName("Status")`, чтобы сопоставлять `DetailedOrder.Status` с тем же столбцом, что и `Order.Status`.</span><span class="sxs-lookup"><span data-stu-id="b8995-110">In addition to the required configuration we call `Property(o => o.Status).HasColumnName("Status")` to map `DetailedOrder.Status` to the same column as `Order.Status`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting)]

> [!TIP]
> <span data-ttu-id="b8995-111">См. [полный пример проекта](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) для получения дополнительных контекстов.</span><span class="sxs-lookup"><span data-stu-id="b8995-111">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="b8995-112">Метрики</span><span class="sxs-lookup"><span data-stu-id="b8995-112">Usage</span></span>

<span data-ttu-id="b8995-113">Сохранение и запрос сущностей с помощью разбиения таблицы выполняется так же, как и для других сущностей:</span><span class="sxs-lookup"><span data-stu-id="b8995-113">Saving and querying entities using table splitting is done in the same way as other entities:</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="optional-dependent-entity"></a><span data-ttu-id="b8995-114">Необязательная зависимая сущность</span><span class="sxs-lookup"><span data-stu-id="b8995-114">Optional dependent entity</span></span>

> [!NOTE]
> <span data-ttu-id="b8995-115">Эта функция появилась в EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="b8995-115">This feature was introduced in EF Core 3.0.</span></span>

<span data-ttu-id="b8995-116">Если все столбцы, используемые зависимой сущностью, `NULL` в базе данных, то при запросе не будет создаваться экземпляр для нее.</span><span class="sxs-lookup"><span data-stu-id="b8995-116">If all of the columns used by a dependent entity are `NULL` in the database, then no instance for it will be created when queried.</span></span> <span data-ttu-id="b8995-117">Это позволяет моделировать необязательную зависимую сущность, где свойство связи в участнике будет иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="b8995-117">This allows modeling an optional dependent entity, where the relationship property on the principal would be null.</span></span> <span data-ttu-id="b8995-118">Обратите внимание, что все свойства зависимых объектов также являются необязательными и имеют значение `null`, что может быть не ожидаемым.</span><span class="sxs-lookup"><span data-stu-id="b8995-118">Note that This would also happen all of the dependent's properties are optional and set to `null`, which might not be expected.</span></span>

## <a name="concurrency-tokens"></a><span data-ttu-id="b8995-119">Маркеры параллелизма</span><span class="sxs-lookup"><span data-stu-id="b8995-119">Concurrency tokens</span></span>

<span data-ttu-id="b8995-120">Если любой из типов сущностей, совместно использующий таблицу, имеет маркер параллелизма, он также должен включаться во все другие типы сущностей.</span><span class="sxs-lookup"><span data-stu-id="b8995-120">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types as well.</span></span> <span data-ttu-id="b8995-121">Это необходимо для того, чтобы избежать устаревшего значения маркера параллелизма при обновлении только одной из сущностей, сопоставленных с одной и той же таблицей.</span><span class="sxs-lookup"><span data-stu-id="b8995-121">This is necessary in order to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="b8995-122">Чтобы предотвратить предоставление маркера параллелизма для кода, можно создать его как [свойство теневого копирования](xref:core/modeling/shadow-properties):</span><span class="sxs-lookup"><span data-stu-id="b8995-122">To avoid exposing the concurrency token to the consuming code, it's possible the create one as a [shadow property](xref:core/modeling/shadow-properties):</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
