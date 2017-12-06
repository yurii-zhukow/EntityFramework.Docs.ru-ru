---
title: "Отслеживание vs. Нет отслеживания запросов - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
ms.technology: entity-framework-core
uid: core/querying/tracking
ms.openlocfilehash: 9a22c893f3b1e9991560e25e0252287a2844b39e
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2017
---
# <a name="tracking-vs-no-tracking-queries"></a>Отслеживание vs. Нет отслеживания запросов

Отслеживание поведение элементов управления, независимо от того, имеется ли Entity Framework Core будет хранить сведения об экземпляре сущности в его отслеживания изменений. Если отслеживаются сущности, любые изменения, которые обнаружены в сущности будут сохраняться в базе данных во время `SaveChanges()`. Entity Framework Core будет свойства навигации также исправление вверх между сущностями, полученные в результате запрос отслеживания и сущностей, которые были ранее загружены в экземпляр класса DbContext.

> [!TIP]  
> Можно просмотреть в этой статье [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) на GitHub.

## <a name="tracking-queries"></a>Запросы отслеживания

По умолчанию выполняется трассировка запросов, возвращающих типы сущностей. Это означает, можно внести изменения для этих экземпляров сущности и имеют эти изменения сохраняются `SaveChanges()`.

В следующем примере изменений с оценкой блоги будет определяться и сохраняются в базе данных во время `SaveChanges()`.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a>Нет отслеживания запросов

Отсутствуют запросы отслеживания полезны в тех случаях, когда результаты передаются в сценарий только для чтения. Они выполняются быстрее, поскольку нет необходимости в сведения об отслеживании изменений для установки.

Можно менять отдельного запроса, чтобы быть нет отслеживания:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

Можно также изменить поведение на уровне экземпляра контекста отслеживания по умолчанию:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> Нет отслеживания запросов по-прежнему выполнения разрешения удостоверения в уточнить запрос. Если результирующий набор содержит ту же сущность несколько раз, для каждого вхождения в результирующем наборе возвращается экземпляр этого класса сущностей. Тем не менее слабые ссылки используются для отслеживания объектов, которые уже были возвращены. Если предыдущий результат с тем же удостоверением выходит за пределы области, а сборка мусора выполняется, может получить новый экземпляр сущности. Дополнительные сведения см. в разделе [принцип работы запросов](overview.md).

## <a name="tracking-and-projections"></a>Отслеживание и проекции

Даже если тип результата запроса не тип сущности, если результат содержит типы сущностей будет по-прежнему отслеживаются по умолчанию. В следующем запросе, который возвращает анонимный тип, экземпляры `Blog` в результирующем наборе будут отслеживаться.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=7)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Blog = b,
                Posts = b.Posts.Count()
            });
}
```

Если результирующий набор не содержит все типы сущностей, отслеживание производиться не будет выполнен. В следующем запросе, который возвращает анонимный тип с различными значениями из сущности (но не экземпляров фактический тип сущности) нет отслеживания не выполнено.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Id = b.BlogId,
                Url = b.Url
            });
}
```
