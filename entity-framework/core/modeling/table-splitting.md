---
title: Таблицы разделения — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: 4a0bfaf017106a0bfdff084b1c472bdc17459a89
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562594"
---
# <a name="table-splitting"></a><span data-ttu-id="8b79e-102">Разбиение таблиц</span><span class="sxs-lookup"><span data-stu-id="8b79e-102">Table Splitting</span></span>

>[!NOTE]
> <span data-ttu-id="8b79e-103">Этот компонент впервые появился в EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="8b79e-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="8b79e-104">EF Core позволяет сопоставить два или несколько сущностей с отдельной строкой.</span><span class="sxs-lookup"><span data-stu-id="8b79e-104">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="8b79e-105">Это называется _таблицы разделение_ или _общий доступ к таблице_.</span><span class="sxs-lookup"><span data-stu-id="8b79e-105">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="8b79e-106">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="8b79e-106">Configuration</span></span>

<span data-ttu-id="8b79e-107">Чтобы использовать разделение таблицы, типы сущностей должны быть добавлены к одной таблице, имеют первичные ключи, сопоставляются с теми же столбцами и по крайней мере одну связь, настроен между первичного ключа из одного типа сущности в той же таблице.</span><span class="sxs-lookup"><span data-stu-id="8b79e-107">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="8b79e-108">Распространенный сценарий для разбиения таблицы используется только подмножество столбцов в таблице больше производительности или инкапсуляции.</span><span class="sxs-lookup"><span data-stu-id="8b79e-108">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="8b79e-109">В этом примере `Order` представляет собой подмножество `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="8b79e-109">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="8b79e-110">В дополнение к требуемой конфигурации мы вызываем `HasBaseType((string)null)` во избежание сопоставление `DetailedOrder` в той же иерархии, что `Order`.</span><span class="sxs-lookup"><span data-stu-id="8b79e-110">In addition to the required configuration we call `HasBaseType((string)null)` to avoid mapping `DetailedOrder` in the same hierarchy as `Order`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

<span data-ttu-id="8b79e-111">См. в разделе [полный пример проекта](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) дополнительный контекст.</span><span class="sxs-lookup"><span data-stu-id="8b79e-111">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="8b79e-112">Использование</span><span class="sxs-lookup"><span data-stu-id="8b79e-112">Usage</span></span>

<span data-ttu-id="8b79e-113">Сохранение и запрос сущностей с помощью разбиения таблицы осуществляется так же как и для других сущностей, отличается только что требуется отслеживать все сущности, совместное использование строки вставки.</span><span class="sxs-lookup"><span data-stu-id="8b79e-113">Saving and querying entities using table splitting is done in the same way as other entities, the only difference is that all entities sharing a row must be tracked for the insert.</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a><span data-ttu-id="8b79e-114">Маркеры параллелизма</span><span class="sxs-lookup"><span data-stu-id="8b79e-114">Concurrency tokens</span></span>

<span data-ttu-id="8b79e-115">Если любой из типов сущностей, общий доступ к таблице имеется маркер параллелизма затем он должен быть включен в всех других типов сущностей, чтобы избежать значение токена устаревших параллелизма при обновлении только одной сущности сопоставляется с той же таблице.</span><span class="sxs-lookup"><span data-stu-id="8b79e-115">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="8b79e-116">Чтобы избежать предоставление ее в коде-потребителе возможна создайте одну в состоянии тени.</span><span class="sxs-lookup"><span data-stu-id="8b79e-116">To avoid exposing it to the consuming code it's possible the create one in shadow-state.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]