---
title: Отключение отслеживания запросов - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: dba4127ade9481b40d4fd3c4323532ddfedf6980
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994244"
---
# <a name="no-tracking-queries"></a>и без отслеживания
Иногда может потребоваться вернуться к сущности на основе запроса, но нет элементов, отслеживаемые по контексту. Это может привести к повышению производительности, при запросе большого количества сущностей в сценариях только для чтения. Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.  

Новый метод расширения AsNoTracking позволяет любому запросу, чтобы выполняться таким образом. Пример:  

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
