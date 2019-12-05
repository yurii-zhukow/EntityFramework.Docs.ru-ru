---
title: Разбиение таблицы — EF Core
description: Настройка разделения таблиц с помощью Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
uid: core/modeling/table-splitting
ms.openlocfilehash: 0e48c516de43cdc2b54c56f1a96f5e01f9fbbbc4
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824559"
---
# <a name="table-splitting"></a><span data-ttu-id="f3d2b-103">Разделение таблиц</span><span class="sxs-lookup"><span data-stu-id="f3d2b-103">Table Splitting</span></span>

>[!NOTE]
> <span data-ttu-id="f3d2b-104">Эта функция впервые реализована в EF Core 2,0.</span><span class="sxs-lookup"><span data-stu-id="f3d2b-104">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="f3d2b-105">EF Core позволяет сопоставлять две или более сущностей с одной строкой.</span><span class="sxs-lookup"><span data-stu-id="f3d2b-105">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="f3d2b-106">Это называется _разбиением таблицы_ или _совместно используемой таблицей_.</span><span class="sxs-lookup"><span data-stu-id="f3d2b-106">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="f3d2b-107">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="f3d2b-107">Configuration</span></span>

<span data-ttu-id="f3d2b-108">Чтобы использовать разделение таблиц, типы сущностей должны быть сопоставлены с одной и той же таблицей, иметь первичные ключи, сопоставленные с теми же столбцами и по крайней мере одну связь между первичным ключом одного типа сущности и другой в той же таблице.</span><span class="sxs-lookup"><span data-stu-id="f3d2b-108">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="f3d2b-109">Распространенным сценарием разбиения таблицы является использование только подмножества столбцов в таблице для повышения производительности или инкапсуляции.</span><span class="sxs-lookup"><span data-stu-id="f3d2b-109">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="f3d2b-110">В этом примере `Order` представляет подмножество `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="f3d2b-110">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="f3d2b-111">В дополнение к требуемой конфигурации мы вызываем `Property(o => o.Status).HasColumnName("Status")`, чтобы сопоставлять `DetailedOrder.Status` с тем же столбцом, что и `Order.Status`.</span><span class="sxs-lookup"><span data-stu-id="f3d2b-111">In addition to the required configuration we call `Property(o => o.Status).HasColumnName("Status")` to map `DetailedOrder.Status` to the same column as `Order.Status`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> <span data-ttu-id="f3d2b-112">См. [полный пример проекта](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) для получения дополнительных контекстов.</span><span class="sxs-lookup"><span data-stu-id="f3d2b-112">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="f3d2b-113">Метрики</span><span class="sxs-lookup"><span data-stu-id="f3d2b-113">Usage</span></span>

<span data-ttu-id="f3d2b-114">Сохранение и запрос сущностей с помощью разбиения таблицы выполняется так же, как и другие сущности.</span><span class="sxs-lookup"><span data-stu-id="f3d2b-114">Saving and querying entities using table splitting is done in the same way as other entities.</span></span> <span data-ttu-id="f3d2b-115">И начиная с EF Core 3,0, ссылка на зависимую сущность может быть `null`.</span><span class="sxs-lookup"><span data-stu-id="f3d2b-115">And starting with EF Core 3.0 the dependent entity reference can be `null`.</span></span> <span data-ttu-id="f3d2b-116">Если все столбцы, используемые зависимой сущностью, `NULL` являются базой данных, то при запросе не будет создаваться экземпляр для нее.</span><span class="sxs-lookup"><span data-stu-id="f3d2b-116">If all of the columns used by the dependent entity are `NULL` is the database then no instance for it will be created when queried.</span></span> <span data-ttu-id="f3d2b-117">Это также приводит к необязательным значениям свойств и имеет значение `null`, которое может быть непредсказуемым.</span><span class="sxs-lookup"><span data-stu-id="f3d2b-117">This would also happen all of the properties are optional and set to `null`, which might not be expected.</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a><span data-ttu-id="f3d2b-118">Маркеры параллелизма</span><span class="sxs-lookup"><span data-stu-id="f3d2b-118">Concurrency tokens</span></span>

<span data-ttu-id="f3d2b-119">Если любой из типов сущностей, совместно использующий таблицу, имеет маркер параллелизма, он должен быть включен во все остальные типы сущностей, чтобы избежать устаревшего значения маркера параллелизма при обновлении только одной из сущностей, сопоставленных с той же таблицей.</span><span class="sxs-lookup"><span data-stu-id="f3d2b-119">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="f3d2b-120">Чтобы не допустить использования этого кода в коде, его можно создать в теневом состоянии.</span><span class="sxs-lookup"><span data-stu-id="f3d2b-120">To avoid exposing it to the consuming code it's possible the create one in shadow-state.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
