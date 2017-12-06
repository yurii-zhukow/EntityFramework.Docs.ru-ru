---
title: "Vs клиента. Оценка Server - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
ms.technology: entity-framework-core
uid: core/querying/client-eval
ms.openlocfilehash: e1852b780041e9e92fb4d25129175346e3a601a3
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="client-vs-server-evaluation"></a>Vs клиента. Оценка Server

Entity Framework Core поддерживает части запроса, оцениваемого на клиенте и части, отправленных в базе данных. Именно поставщик базы данных, чтобы определить, какие части запроса будет вычисляться в базе данных.

> [!TIP]  
> Можно просмотреть в этой статье [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) на GitHub.

## <a name="client-evaluation"></a>Оценки клиента

В следующем примере вспомогательный метод используется для стандартизации URL-адреса для веб-журналы, которые возвращаются из базы данных SQL Server. Так как поставщик SQL Server не понять, как реализовать этот метод, не удается преобразовать ее в SQL. Все другие аспекты запроса вычисляются в базе данных, но передача возвращаемого `URL` посредством этого метода выполняется на клиенте.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs?highlight=6)] -->
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

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
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

## <a name="disabling-client-evaluation"></a>Отключение оценки клиента

Во время оценки клиента могут быть очень полезными, в некоторых случаях это может привести к снижению производительности. Рассмотрим следующий запрос, где вспомогательный метод сейчас используется в фильтре. Так как не удается выполнить в базе данных, все данные помещаются в памяти, а затем фильтр применяется на клиентском компьютере. В зависимости от объема данных, а также объем данных, фильтруются это может привести к снижению производительности.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

По умолчанию EF Core журнал предупреждения при выполнении оценки клиента. В разделе [входа](../miscellaneous/logging.md) Дополнительные сведения о просмотре выходных данных ведения журнала. Можно изменить поведение при возникновении оценки клиента выдать или не выполняют никаких действий. Это делается при настройке параметров для текущего контекста — обычно в `DbContext.OnConfiguring`, или в `Startup.cs` при использовании ASP.NET Core.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
