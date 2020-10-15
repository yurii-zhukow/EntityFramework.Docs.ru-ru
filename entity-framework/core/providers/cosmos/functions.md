---
title: Сопоставления функций — поставщик Azure Cosmos DB — EF Core
description: Сопоставления функций поставщика Azure Cosmos DB EF Core
author: bricelam
ms.date: 10/08/2020
uid: core/providers/cosmos/functions
ms.openlocfilehash: ffcf82fbe57ea16e6fc62f3c9c80c572b3e1ed91
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92066536"
---
# <a name="function-mappings-of-the-azure-cosmos-db-ef-core-provider"></a><span data-ttu-id="fded9-103">Сопоставления функций поставщика Azure Cosmos DB EF Core</span><span class="sxs-lookup"><span data-stu-id="fded9-103">Function Mappings of the Azure Cosmos DB EF Core Provider</span></span>

<span data-ttu-id="fded9-104">На этой странице показано, какие члены .NET преобразуются в функции SQL при использовании поставщика Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fded9-104">This page shows which .NET members are translated into which SQL functions when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="fded9-105">.NET</span><span class="sxs-lookup"><span data-stu-id="fded9-105">.NET</span></span>                          | <span data-ttu-id="fded9-106">SQL</span><span class="sxs-lookup"><span data-stu-id="fded9-106">SQL</span></span>                              | <span data-ttu-id="fded9-107">Добавлено в</span><span class="sxs-lookup"><span data-stu-id="fded9-107">Added in</span></span>
----------------------------- | -------------------------------- | --------
<span data-ttu-id="fded9-108">набор. Contains (элемент)</span><span class="sxs-lookup"><span data-stu-id="fded9-108">collection.Contains(item)</span></span>     | <span data-ttu-id="fded9-109">@item ОКНЕ @collection</span><span class="sxs-lookup"><span data-stu-id="fded9-109">@item IN @collection</span></span>
<span data-ttu-id="fded9-110">stringValue. Contains (значение)</span><span class="sxs-lookup"><span data-stu-id="fded9-110">stringValue.Contains(value)</span></span>   | <span data-ttu-id="fded9-111">СОДЕРЖИТ ( @stringValue , @value )</span><span class="sxs-lookup"><span data-stu-id="fded9-111">CONTAINS(@stringValue, @value)</span></span>   | <span data-ttu-id="fded9-112">EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="fded9-112">EF Core 5.0</span></span>
<span data-ttu-id="fded9-113">stringValue. EndsWith (значение)</span><span class="sxs-lookup"><span data-stu-id="fded9-113">stringValue.EndsWith(value)</span></span>   | <span data-ttu-id="fded9-114">ENDSWITH ( @stringValue , @value )</span><span class="sxs-lookup"><span data-stu-id="fded9-114">ENDSWITH(@stringValue, @value)</span></span>   | <span data-ttu-id="fded9-115">EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="fded9-115">EF Core 5.0</span></span>
<span data-ttu-id="fded9-116">stringValue. FirstOrDefault ()</span><span class="sxs-lookup"><span data-stu-id="fded9-116">stringValue.FirstOrDefault()</span></span>  | <span data-ttu-id="fded9-117">LEFT ( @stringValue , 1)</span><span class="sxs-lookup"><span data-stu-id="fded9-117">LEFT(@stringValue, 1)</span></span>            | <span data-ttu-id="fded9-118">EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="fded9-118">EF Core 5.0</span></span>
<span data-ttu-id="fded9-119">stringValue. LastOrDefault ()</span><span class="sxs-lookup"><span data-stu-id="fded9-119">stringValue.LastOrDefault()</span></span>   | <span data-ttu-id="fded9-120">RIGHT ( @stringValue , 1)</span><span class="sxs-lookup"><span data-stu-id="fded9-120">RIGHT(@stringValue, 1)</span></span>           | <span data-ttu-id="fded9-121">EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="fded9-121">EF Core 5.0</span></span>
<span data-ttu-id="fded9-122">stringValue. StartsWith (значение)</span><span class="sxs-lookup"><span data-stu-id="fded9-122">stringValue.StartsWith(value)</span></span> | <span data-ttu-id="fded9-123">STARTSWITH ( @stringValue , @value )</span><span class="sxs-lookup"><span data-stu-id="fded9-123">STARTSWITH(@stringValue, @value)</span></span> | <span data-ttu-id="fded9-124">EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="fded9-124">EF Core 5.0</span></span>
