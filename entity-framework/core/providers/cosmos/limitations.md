---
title: Поставщик Azure Cosmos DB — ограничения — EF Core
description: Ограничения поставщика Entity Framework Core Azure Cosmos DB по сравнению с другими поставщиками
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 0b8edd9ac5fa3d684498efe222ce11a0356040b8
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89619273"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a><span data-ttu-id="5b576-103">Ограничения поставщика EF Core Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="5b576-103">EF Core Azure Cosmos DB Provider Limitations</span></span>

<span data-ttu-id="5b576-104">У поставщика Cosmos есть ряд ограничений.</span><span class="sxs-lookup"><span data-stu-id="5b576-104">The Cosmos provider has a number of limitations.</span></span> <span data-ttu-id="5b576-105">Многие из этих ограничений являются следствием ограничений в базовом ядре СУБД Cosmos и не относятся к EF.</span><span class="sxs-lookup"><span data-stu-id="5b576-105">Many of these limitations are a result of limitations in the underlying Cosmos database engine and are not specific to EF.</span></span> <span data-ttu-id="5b576-106">Но большинство просто еще [не реализовано](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="5b576-106">But most simply [haven't been implemented yet](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span></span>

## <a name="temporary-limitations"></a><span data-ttu-id="5b576-107">Временные ограничения</span><span class="sxs-lookup"><span data-stu-id="5b576-107">Temporary limitations</span></span>

- <span data-ttu-id="5b576-108">Даже если существует только один тип сущности без наследования, сопоставленный с контейнером, он по-прежнему имеет свойство дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="5b576-108">Even if there is only one entity type without inheritance mapped to a container it still has a discriminator property.</span></span>
- <span data-ttu-id="5b576-109">Типы сущностей с ключами секций неправильно работают в некоторых сценариях</span><span class="sxs-lookup"><span data-stu-id="5b576-109">Entity types with partition keys don't work correctly in some scenarios</span></span>
- <span data-ttu-id="5b576-110">`Include` вызовы не поддерживаются</span><span class="sxs-lookup"><span data-stu-id="5b576-110">`Include` calls are not supported</span></span>
- <span data-ttu-id="5b576-111">`Join` вызовы не поддерживаются</span><span class="sxs-lookup"><span data-stu-id="5b576-111">`Join` calls are not supported</span></span>

## <a name="azure-cosmos-db-sdk-limitations"></a><span data-ttu-id="5b576-112">Ограничения пакета SDK для Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="5b576-112">Azure Cosmos DB SDK limitations</span></span>

- <span data-ttu-id="5b576-113">Предоставляются только асинхронные методы</span><span class="sxs-lookup"><span data-stu-id="5b576-113">Only async methods are provided</span></span>

> [!WARNING]
> <span data-ttu-id="5b576-114">Поскольку отсутствуют версии синхронизации методов низкого уровня, EF Core полагается на, в настоящее время соответствующая функциональность реализуется путем вызова метода `.Wait()` на возвращенном `Task` .</span><span class="sxs-lookup"><span data-stu-id="5b576-114">Since there are no sync versions of the low level methods EF Core relies on, the corresponding functionality is currently implemented by calling `.Wait()` on the returned `Task`.</span></span> <span data-ttu-id="5b576-115">Это означает, что использование таких методов `SaveChanges` , как или, `ToList` а не их асинхронных аналогов, может привести к взаимоблокировке приложения.</span><span class="sxs-lookup"><span data-stu-id="5b576-115">This means that using methods like `SaveChanges`, or `ToList` instead of their async counterparts could lead to a deadlock in your application</span></span>

## <a name="azure-cosmos-db-limitations"></a><span data-ttu-id="5b576-116">Ограничения Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="5b576-116">Azure Cosmos DB limitations</span></span>

<span data-ttu-id="5b576-117">Вы можете увидеть полный обзор [Azure Cosmos DB поддерживаемых функций](/azure/cosmos-db/modeling-data). это наиболее важные отличия по сравнению с реляционной базой данных.</span><span class="sxs-lookup"><span data-stu-id="5b576-117">You can see the full overview of [Azure Cosmos DB supported features](/azure/cosmos-db/modeling-data), these are the most notable differences compared to a relational database:</span></span>

- <span data-ttu-id="5b576-118">Инициированные клиентом транзакции не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="5b576-118">Client-initiated transactions are not supported</span></span>
- <span data-ttu-id="5b576-119">Некоторые запросы между секциями либо не поддерживаются, либо выполняются слишком медленно в зависимости от используемых операторов</span><span class="sxs-lookup"><span data-stu-id="5b576-119">Some cross-partition queries are either not supported or much slower depending on the operators involved</span></span>
