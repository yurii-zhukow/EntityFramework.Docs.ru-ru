---
title: Выполнение в клиенте и на сервере — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: cb207d9e1b1004a4084dd6fc66712183b5bdd5dc
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921703"
---
# <a name="client-vs-server-evaluation"></a>Выполнение в клиенте и на сервере

Entity Framework Core поддерживает выполнение части запроса в клиенте и передачу второй части в базу данных. Поставщик базы данных определяет, какие части запроса могут выполняться в базе данных.

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) из репозитория GitHub.

## <a name="client-evaluation"></a>Выполнение в клиенте

В следующем примере вспомогательный метод используется для стандартизации URL-адресов блогов, которые возвращаются из базы данных SQL Server. Так как поставщик SQL Server не знает, как реализован этот метод, его невозможно преобразовать в SQL. Все остальные аспекты запроса выполняются в базе данных, но передача возвращаемого `URL` с помощью этого метода выполняется в клиенте.

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs?highlight=6)] -->
``` csharp
var blogs = context.Blogs
    .OrderByDescending(blog => blog.Rating)
    .Select(blog => new
    {
        Id = blog.BlogId,
        Url = StandardizeUrl(blog.Url)
    })
    .ToList();
```

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
``` csharp
public static string StandardizeUrl(string url)
{
    url = url.ToLower();

    if (!url.StartsWith("http://"))
    {
        url = string.Concat("http://", url);
    }

    return url;
}
```

## <a name="client-evaluation-performance-issues"></a>Проблемы с производительностью при вычислении в клиенте

Хотя выполнение в клиенте может быть очень полезным, в некоторых случаях это может привести к снижению производительности. Рассмотрим следующий запрос, где вспомогательный метод теперь используется в фильтре. Так как этот запрос невозможно выполнить в базе данных, все данные извлекаются в память, а затем фильтр применяется в клиенте. В зависимости от объема данных и степени их фильтрования это может привести к низкой производительности.

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

## <a name="client-evaluation-logging"></a>Журналирование вычислений в клиенте

По умолчанию EF Core будет регистрировать предупреждение при выполнении запроса в клиенте. Дополнительные сведения о просмотре выходных данных журнала см. в [этой статье](../miscellaneous/logging.md). 

## <a name="optional-behavior-throw-an-exception-for-client-evaluation"></a>Необязательное поведение: создание исключения при вычислении в клиенте

Вы можете изменить поведение при выполнении запроса в клиенте: либо создавать предупреждение, либо ничего не делать. Это можно указать при настройке параметров для вашего контекста — обычно в `DbContext.OnConfiguring` или в `Startup.cs`, если вы используете ASP.NET Core.

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
