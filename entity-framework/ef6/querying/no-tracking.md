---
title: Запросы без отслеживания — EF6
description: Запросы без отслеживания в Entity Framework 6
author: divega
ms.date: 10/23/2016
uid: ef6/querying/no-tracking
ms.openlocfilehash: ea4f05eb7a9b95fba55f70f249876bc9c5630f18
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90073903"
---
# <a name="no-tracking-queries"></a><span data-ttu-id="60983-103">и без отслеживания</span><span class="sxs-lookup"><span data-stu-id="60983-103">No-Tracking Queries</span></span>
<span data-ttu-id="60983-104">Иногда может потребоваться вернуть сущности из запроса, но не контролировать эти сущности в контексте.</span><span class="sxs-lookup"><span data-stu-id="60983-104">Sometimes you may want to get entities back from a query but not have those entities be tracked by the context.</span></span> <span data-ttu-id="60983-105">Это может привести к повышению производительности при запросе большого количества сущностей в сценариях только для чтения.</span><span class="sxs-lookup"><span data-stu-id="60983-105">This may result in better performance when querying for large numbers of entities in read-only scenarios.</span></span> <span data-ttu-id="60983-106">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="60983-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="60983-107">Новый метод расширения Asnotrackin позволяет выполнять любой запрос таким образом.</span><span class="sxs-lookup"><span data-stu-id="60983-107">A new extension method AsNoTracking allows any query to be run in this way.</span></span> <span data-ttu-id="60983-108">Пример:</span><span class="sxs-lookup"><span data-stu-id="60983-108">For example:</span></span>  

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
