---
title: Явная загрузка связанных данных — EF Core
description: Явная загрузка связанных данных с помощью Entity Framework Core
author: roji
ms.date: 9/8/2020
uid: core/querying/related-data/explicit
ms.openlocfilehash: def1c8c2736c64bc6f39af0d344fd1330f8a06f5
ms.sourcegitcommit: c0e6a00b64c2dcd8acdc0fe6d1b47703405cdf09
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2020
ms.locfileid: "91210458"
---
# <a name="explicit-loading-of-related-data"></a><span data-ttu-id="32ea3-103">Явная загрузка связанных данных</span><span class="sxs-lookup"><span data-stu-id="32ea3-103">Explicit Loading of Related Data</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="32ea3-104">Явная загрузка</span><span class="sxs-lookup"><span data-stu-id="32ea3-104">Explicit loading</span></span>

<span data-ttu-id="32ea3-105">Вы можете явно загрузить свойство навигации с помощью API `DbContext.Entry(...)`.</span><span class="sxs-lookup"><span data-stu-id="32ea3-105">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="32ea3-106">Вы также можете явно загрузить свойство навигации, выполнив отдельный запрос, который возвращает связанные сущности.</span><span class="sxs-lookup"><span data-stu-id="32ea3-106">You can also explicitly load a navigation property by executing a separate query that returns the related entities.</span></span> <span data-ttu-id="32ea3-107">Если отслеживание изменений включено, то при материализации сущности запросом EF Core автоматически установит свойства навигации вновь загруженной сущности для ссылки на любые уже загруженные сущности и задаст свойства навигации уже загруженных сущностей, чтобы ссылаться на вновь загруженные сущности.</span><span class="sxs-lookup"><span data-stu-id="32ea3-107">If change tracking is enabled, then when query materializes an entity, EF Core will automatically set the navigation properties of the newly loaded entity to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly loaded entity.</span></span>

## <a name="querying-related-entities"></a><span data-ttu-id="32ea3-108">Запрос связанных сущностей</span><span class="sxs-lookup"><span data-stu-id="32ea3-108">Querying related entities</span></span>

<span data-ttu-id="32ea3-109">Можно также получить запрос LINQ, который представляет содержимое свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="32ea3-109">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="32ea3-110">Это позволяет применять дополнительные операторы к запросу.</span><span class="sxs-lookup"><span data-stu-id="32ea3-110">It allows you to apply additional operators over the query.</span></span> <span data-ttu-id="32ea3-111">Например, применение оператора агрегирования в связанных сущностях без их загрузки в память.</span><span class="sxs-lookup"><span data-stu-id="32ea3-111">For example applying an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="32ea3-112">Можно также фильтровать связанные сущности, которые загружаются в память.</span><span class="sxs-lookup"><span data-stu-id="32ea3-112">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryFiltered)]
