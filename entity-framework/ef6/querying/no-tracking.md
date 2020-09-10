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
# <a name="no-tracking-queries"></a>и без отслеживания
Иногда может потребоваться вернуть сущности из запроса, но не контролировать эти сущности в контексте. Это может привести к повышению производительности при запросе большого количества сущностей в сценариях только для чтения. Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.  

Новый метод расширения Asnotrackin позволяет выполнять любой запрос таким образом. Пример:  

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
