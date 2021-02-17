---
title: Сопоставления функций — поставщик Azure Cosmos DB — EF Core
description: Сопоставления функций поставщика Azure Cosmos DB EF Core
author: bricelam
ms.date: 1/26/2021
uid: core/providers/cosmos/functions
ms.openlocfilehash: d4d45ba7db50befb5eb775feed0af44468ee3f74
ms.sourcegitcommit: 704240349e18b6404e5a809f5b7c9d365b152e2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/16/2021
ms.locfileid: "100543592"
---
# <a name="function-mappings-of-the-azure-cosmos-db-ef-core-provider"></a><span data-ttu-id="c579c-103">Сопоставления функций поставщика Azure Cosmos DB EF Core</span><span class="sxs-lookup"><span data-stu-id="c579c-103">Function Mappings of the Azure Cosmos DB EF Core Provider</span></span>

<span data-ttu-id="c579c-104">На этой странице показано, какие члены .NET преобразуются в функции SQL при использовании поставщика Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c579c-104">This page shows which .NET members are translated into which SQL functions when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="c579c-105">.NET</span><span class="sxs-lookup"><span data-stu-id="c579c-105">.NET</span></span>                          | <span data-ttu-id="c579c-106">SQL</span><span class="sxs-lookup"><span data-stu-id="c579c-106">SQL</span></span>                              | <span data-ttu-id="c579c-107">Добавлено в</span><span class="sxs-lookup"><span data-stu-id="c579c-107">Added in</span></span>
----------------------------- | -------------------------------- | --------
<span data-ttu-id="c579c-108">набор. Contains (элемент)</span><span class="sxs-lookup"><span data-stu-id="c579c-108">collection.Contains(item)</span></span>     | <span data-ttu-id="c579c-109">@item ОКНЕ @collection</span><span class="sxs-lookup"><span data-stu-id="c579c-109">@item IN @collection</span></span>
<span data-ttu-id="c579c-110">#. Functions. Random ()</span><span class="sxs-lookup"><span data-stu-id="c579c-110">EF.Functions.Random()</span></span>         | <span data-ttu-id="c579c-111">RAND ()</span><span class="sxs-lookup"><span data-stu-id="c579c-111">RAND()</span></span>                           | <span data-ttu-id="c579c-112">EF Core 6.0</span><span class="sxs-lookup"><span data-stu-id="c579c-112">EF Core 6.0</span></span>
<span data-ttu-id="c579c-113">stringValue. Contains (значение)</span><span class="sxs-lookup"><span data-stu-id="c579c-113">stringValue.Contains(value)</span></span>   | <span data-ttu-id="c579c-114">СОДЕРЖИТ ( @stringValue , @value )</span><span class="sxs-lookup"><span data-stu-id="c579c-114">CONTAINS(@stringValue, @value)</span></span>   | <span data-ttu-id="c579c-115">EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="c579c-115">EF Core 5.0</span></span>
<span data-ttu-id="c579c-116">stringValue. EndsWith (значение)</span><span class="sxs-lookup"><span data-stu-id="c579c-116">stringValue.EndsWith(value)</span></span>   | <span data-ttu-id="c579c-117">ENDSWITH ( @stringValue , @value )</span><span class="sxs-lookup"><span data-stu-id="c579c-117">ENDSWITH(@stringValue, @value)</span></span>   | <span data-ttu-id="c579c-118">EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="c579c-118">EF Core 5.0</span></span>
<span data-ttu-id="c579c-119">stringValue. FirstOrDefault ()</span><span class="sxs-lookup"><span data-stu-id="c579c-119">stringValue.FirstOrDefault()</span></span>  | <span data-ttu-id="c579c-120">LEFT ( @stringValue , 1)</span><span class="sxs-lookup"><span data-stu-id="c579c-120">LEFT(@stringValue, 1)</span></span>            | <span data-ttu-id="c579c-121">EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="c579c-121">EF Core 5.0</span></span>
<span data-ttu-id="c579c-122">stringValue. LastOrDefault ()</span><span class="sxs-lookup"><span data-stu-id="c579c-122">stringValue.LastOrDefault()</span></span>   | <span data-ttu-id="c579c-123">RIGHT ( @stringValue , 1)</span><span class="sxs-lookup"><span data-stu-id="c579c-123">RIGHT(@stringValue, 1)</span></span>           | <span data-ttu-id="c579c-124">EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="c579c-124">EF Core 5.0</span></span>
<span data-ttu-id="c579c-125">stringValue. StartsWith (значение)</span><span class="sxs-lookup"><span data-stu-id="c579c-125">stringValue.StartsWith(value)</span></span> | <span data-ttu-id="c579c-126">STARTSWITH ( @stringValue , @value )</span><span class="sxs-lookup"><span data-stu-id="c579c-126">STARTSWITH(@stringValue, @value)</span></span> | <span data-ttu-id="c579c-127">EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="c579c-127">EF Core 5.0</span></span>
