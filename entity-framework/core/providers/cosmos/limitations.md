---
title: Поставщик Azure Cosmos DB — ограничения — EF Core
description: Ограничения поставщика Entity Framework Core Azure Cosmos DB по сравнению с другими поставщиками
author: AndriySvyryd
ms.date: 11/05/2020
uid: core/providers/cosmos/limitations
ms.openlocfilehash: f7517954b854c8ea1020c039e490b4fb0dea7181
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94430200"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a><span data-ttu-id="31fa9-103">Ограничения поставщика EF Core Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="31fa9-103">EF Core Azure Cosmos DB Provider Limitations</span></span>

<span data-ttu-id="31fa9-104">У поставщика Cosmos есть ряд ограничений.</span><span class="sxs-lookup"><span data-stu-id="31fa9-104">The Cosmos provider has a number of limitations.</span></span> <span data-ttu-id="31fa9-105">Многие из этих ограничений являются следствием ограничений в базовом ядре СУБД Cosmos и не относятся к EF.</span><span class="sxs-lookup"><span data-stu-id="31fa9-105">Many of these limitations are a result of limitations in the underlying Cosmos database engine and are not specific to EF.</span></span> <span data-ttu-id="31fa9-106">Но большинство просто еще [не реализовано](https://github.com/dotnet/efcore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="31fa9-106">But most simply [haven't been implemented yet](https://github.com/dotnet/efcore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span></span>

## <a name="temporary-limitations"></a><span data-ttu-id="31fa9-107">Временные ограничения</span><span class="sxs-lookup"><span data-stu-id="31fa9-107">Temporary limitations</span></span>

- <span data-ttu-id="31fa9-108">`Include` вызовы не поддерживаются</span><span class="sxs-lookup"><span data-stu-id="31fa9-108">`Include` calls are not supported</span></span>
- <span data-ttu-id="31fa9-109">`Join` вызовы не поддерживаются</span><span class="sxs-lookup"><span data-stu-id="31fa9-109">`Join` calls are not supported</span></span>

## <a name="azure-cosmos-db-sdk-limitations"></a><span data-ttu-id="31fa9-110">Ограничения пакета SDK для Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="31fa9-110">Azure Cosmos DB SDK limitations</span></span>

- <span data-ttu-id="31fa9-111">Предоставляются только асинхронные методы</span><span class="sxs-lookup"><span data-stu-id="31fa9-111">Only async methods are provided</span></span>

> [!WARNING]
> <span data-ttu-id="31fa9-112">Поскольку отсутствуют версии синхронизации методов низкого уровня, EF Core полагается на, в настоящее время соответствующая функциональность реализуется путем вызова метода `.Wait()` на возвращенном `Task` .</span><span class="sxs-lookup"><span data-stu-id="31fa9-112">Since there are no sync versions of the low level methods EF Core relies on, the corresponding functionality is currently implemented by calling `.Wait()` on the returned `Task`.</span></span> <span data-ttu-id="31fa9-113">Это означает, что использование таких методов `SaveChanges` , как или, `ToList` а не их асинхронных аналогов, может привести к взаимоблокировке приложения.</span><span class="sxs-lookup"><span data-stu-id="31fa9-113">This means that using methods like `SaveChanges`, or `ToList` instead of their async counterparts could lead to a deadlock in your application</span></span>

## <a name="azure-cosmos-db-limitations"></a><span data-ttu-id="31fa9-114">Ограничения Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="31fa9-114">Azure Cosmos DB limitations</span></span>

<span data-ttu-id="31fa9-115">Вы можете увидеть полный обзор [Azure Cosmos DB поддерживаемых функций](/azure/cosmos-db/modeling-data). это наиболее важные отличия по сравнению с реляционной базой данных.</span><span class="sxs-lookup"><span data-stu-id="31fa9-115">You can see the full overview of [Azure Cosmos DB supported features](/azure/cosmos-db/modeling-data), these are the most notable differences compared to a relational database:</span></span>

- <span data-ttu-id="31fa9-116">Инициированные клиентом транзакции не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="31fa9-116">Client-initiated transactions are not supported</span></span>
- <span data-ttu-id="31fa9-117">Некоторые запросы между секциями выполняются медленнее в зависимости от вовлеченных операторов (например `Skip/Take` , или `OFFSET LIMIT` ).</span><span class="sxs-lookup"><span data-stu-id="31fa9-117">Some cross-partition queries are slower depending on the operators involved (for example `Skip/Take` or `OFFSET LIMIT`)</span></span>
