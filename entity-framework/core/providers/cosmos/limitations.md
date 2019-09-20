---
title: Поставщик Azure Cosmos DB — ограничения — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 9d02a2cd-484e-4687-b8a8-3748ba46dbc9
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 8dcc82a68c89e21ad1902a0bbbce8ebbc3535801
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150767"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a><span data-ttu-id="6d194-102">Ограничения поставщика EF Core Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="6d194-102">EF Core Azure Cosmos DB Provider Limitations</span></span>

<span data-ttu-id="6d194-103">У поставщика Cosmos есть ряд ограничений.</span><span class="sxs-lookup"><span data-stu-id="6d194-103">The Cosmos provider has a number of limitations.</span></span> <span data-ttu-id="6d194-104">Многие из этих ограничений являются следствием ограничений в базовом ядре СУБД Cosmos и не относятся к EF.</span><span class="sxs-lookup"><span data-stu-id="6d194-104">Many of these limitations are a result of limitations in the underlying Cosmos database engine and are not specific to EF.</span></span> <span data-ttu-id="6d194-105">Но большинство просто еще [не реализовано](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="6d194-105">But most simply [haven't been implemented yet](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span></span>

## <a name="temporary-limitations"></a><span data-ttu-id="6d194-106">Временные ограничения</span><span class="sxs-lookup"><span data-stu-id="6d194-106">Temporary limitations</span></span>

- <span data-ttu-id="6d194-107">Даже если существует только один тип сущности без наследования, сопоставленный с контейнером, он по-прежнему имеет свойство дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="6d194-107">Even if there is only one entity type without inheritance mapped to a container it still has a discriminator property.</span></span>
- <span data-ttu-id="6d194-108">Типы сущностей с ключами секций неправильно работают в некоторых сценариях</span><span class="sxs-lookup"><span data-stu-id="6d194-108">Entity types with partition keys don't work correctly in some scenarios</span></span>
- <span data-ttu-id="6d194-109">`Include`вызовы не поддерживаются</span><span class="sxs-lookup"><span data-stu-id="6d194-109">`Include` calls are not supported</span></span>
- <span data-ttu-id="6d194-110">`Join`вызовы не поддерживаются</span><span class="sxs-lookup"><span data-stu-id="6d194-110">`Join` calls are not supported</span></span>

## <a name="azure-cosmos-db-sdk-limitations"></a><span data-ttu-id="6d194-111">Ограничения пакета SDK для Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="6d194-111">Azure Cosmos DB SDK limitations</span></span>

- <span data-ttu-id="6d194-112">Предоставляются только асинхронные методы</span><span class="sxs-lookup"><span data-stu-id="6d194-112">Only async methods are provided</span></span>

> [!WARNING]
> <span data-ttu-id="6d194-113">Поскольку отсутствуют версии синхронизации методов низкого уровня, EF Core полагается на, в настоящее время соответствующая функциональность реализуется путем вызова `.Wait()` метода на возвращенном. `Task`</span><span class="sxs-lookup"><span data-stu-id="6d194-113">Since there are no sync versions of the low level methods EF Core relies on, the corresponding functionality is currently implemented by calling `.Wait()` on the returned `Task`.</span></span> <span data-ttu-id="6d194-114">Это означает, что использование таких `SaveChanges`методов, `ToList` как или, а не их асинхронных аналогов, может привести к взаимоблокировке приложения.</span><span class="sxs-lookup"><span data-stu-id="6d194-114">This means that using methods like `SaveChanges`, or `ToList` instead of their async counterparts could lead to a deadlock in your application</span></span>

## <a name="azure-cosmos-db-limitations"></a><span data-ttu-id="6d194-115">Ограничения Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="6d194-115">Azure Cosmos DB limitations</span></span>

<span data-ttu-id="6d194-116">Вы можете увидеть полный обзор [Azure Cosmos DB поддерживаемых функций](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data). это наиболее важные отличия по сравнению с реляционной базой данных.</span><span class="sxs-lookup"><span data-stu-id="6d194-116">You can see the full overview of [Azure Cosmos DB supported features](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data), these are the most notable differences compared to a relational database:</span></span>

- <span data-ttu-id="6d194-117">Инициированные клиентом транзакции не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="6d194-117">Client-initiated transactions are not supported</span></span>
- <span data-ttu-id="6d194-118">Некоторые запросы между секциями либо не поддерживаются, либо выполняются слишком медленно в зависимости от используемых операторов</span><span class="sxs-lookup"><span data-stu-id="6d194-118">Some cross-partition queries are either not supported or much slower depending on the operators involved</span></span>