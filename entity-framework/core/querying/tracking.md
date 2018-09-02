---
title: Сравнение запросов с отслеживанием и без отслеживания — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: 985adc795f379199a3bacc985843f32f3168cf64
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993359"
---
# <a name="tracking-vs-no-tracking-queries"></a>Сравнение запросов с отслеживанием и без отслеживания

Поведение отслеживания контролирует, будет ли Entity Framework Core хранить информацию об экземпляре сущности в своем средстве отслеживания изменений. Если сущность отслеживается, любые изменения, обнаруженные в сущности, будут сохраняться в базе данных во время операции `SaveChanges()`. Entity Framework Core также будет адаптировать свойства навигации между сущностями, полученными из запроса отслеживания, и сущностями, которые ранее были загружены в экземпляр DbContext.

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) из репозитория GitHub.

## <a name="tracking-queries"></a>Отслеживания запросов

По умолчанию запросы, возвращающие типы сущностей, отслеживаются. Это означает, что вы можете внести изменения в эти экземпляры сущностей и сохранить эти изменения с помощью операции `SaveChanges()`.

В следующем примере изменение рейтинга блогов будет обнаружено и сохранено в базе данных во время операции `SaveChanges()`.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a>Отключение отслеживания запросов

Никакие запросы отслеживания не полезны, когда результаты используются в сценарии только для чтения. Они быстрее выполняются, потому что нет необходимости настраивать информацию об отслеживании изменений.

Вы можете отключить отслеживание для отдельного запроса:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

Вы также можете изменить поведение отслеживания по умолчанию на уровне экземпляра контекста:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> Запросы отслеживания не выполняют разрешение идентичности в исполняемом запросе. Если результирующий набор содержит одну и ту же сущность несколько раз, тот же экземпляр класса сущности будет возвращен для каждого вхождения в результирующем наборе. Однако для отслеживания сущностей, которые уже были возвращены, используются слабые ссылки. Если предыдущий результат с тем же идентификатором выходит за пределы области, а сборка мусора выполняется, вы можете получить новый экземпляр сущности. Подробнее см. в статье о [принципе работы запросов](overview.md).

## <a name="tracking-and-projections"></a>Отслеживание и проекции

Даже если тип результата запроса не является типом сущности, если результат содержит типы сущностей, они по-прежнему будут отслеживаться по умолчанию. В следующем запросе, который возвращает анонимный тип, будут отслеживаться экземпляры `Blog` в результирующем наборе.

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

Если результирующий набор не содержит типов сущностей, то отслеживание не выполняется. В следующем запросе, который возвращает анонимный тип с некоторыми значениями из сущности (но не экземплярами фактического типа сущности), отслеживание не выполняется.

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
