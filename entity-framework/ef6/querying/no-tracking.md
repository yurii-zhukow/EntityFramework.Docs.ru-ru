---
title: Запросы без отслеживания — EF6
description: Запросы без отслеживания в Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
uid: ef6/querying/no-tracking
ms.openlocfilehash: eb9e29c219e0cdf1e379cf8bb925f4226b1434a9
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89620290"
---
# <a name="no-tracking-queries"></a><span data-ttu-id="feb49-103">и без отслеживания</span><span class="sxs-lookup"><span data-stu-id="feb49-103">No-Tracking Queries</span></span>
<span data-ttu-id="feb49-104">Иногда может потребоваться вернуть сущности из запроса, но не контролировать эти сущности в контексте.</span><span class="sxs-lookup"><span data-stu-id="feb49-104">Sometimes you may want to get entities back from a query but not have those entities be tracked by the context.</span></span> <span data-ttu-id="feb49-105">Это может привести к повышению производительности при запросе большого количества сущностей в сценариях только для чтения.</span><span class="sxs-lookup"><span data-stu-id="feb49-105">This may result in better performance when querying for large numbers of entities in read-only scenarios.</span></span> <span data-ttu-id="feb49-106">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="feb49-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="feb49-107">Новый метод расширения Asnotrackin позволяет выполнять любой запрос таким образом.</span><span class="sxs-lookup"><span data-stu-id="feb49-107">A new extension method AsNoTracking allows any query to be run in this way.</span></span> <span data-ttu-id="feb49-108">Пример:</span><span class="sxs-lookup"><span data-stu-id="feb49-108">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs without tracking them
    var blogs1 = context.Blogs.AsNoTracking();

    // Query for some blogs without tracking them
    var blogs2 = context.Blogs
                        .Where(b => b.Name.Contains(".NET"))
                        .AsNoTracking()
                        .ToList();
}
```  
