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
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="9f599-102">Выполнение в клиенте и на сервере</span><span class="sxs-lookup"><span data-stu-id="9f599-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="9f599-103">Entity Framework Core поддерживает выполнение части запроса в клиенте и передачу второй части в базу данных.</span><span class="sxs-lookup"><span data-stu-id="9f599-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="9f599-104">Поставщик базы данных определяет, какие части запроса могут выполняться в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9f599-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="9f599-105">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="9f599-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="9f599-106">Выполнение в клиенте</span><span class="sxs-lookup"><span data-stu-id="9f599-106">Client evaluation</span></span>

<span data-ttu-id="9f599-107">В следующем примере вспомогательный метод используется для стандартизации URL-адресов блогов, которые возвращаются из базы данных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9f599-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="9f599-108">Так как поставщик SQL Server не знает, как реализован этот метод, его невозможно преобразовать в SQL.</span><span class="sxs-lookup"><span data-stu-id="9f599-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="9f599-109">Все остальные аспекты запроса выполняются в базе данных, но передача возвращаемого `URL` с помощью этого метода выполняется в клиенте.</span><span class="sxs-lookup"><span data-stu-id="9f599-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

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

## <a name="client-evaluation-performance-issues"></a><span data-ttu-id="9f599-110">Проблемы с производительностью при вычислении в клиенте</span><span class="sxs-lookup"><span data-stu-id="9f599-110">Client evaluation performance issues</span></span>

<span data-ttu-id="9f599-111">Хотя выполнение в клиенте может быть очень полезным, в некоторых случаях это может привести к снижению производительности.</span><span class="sxs-lookup"><span data-stu-id="9f599-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="9f599-112">Рассмотрим следующий запрос, где вспомогательный метод теперь используется в фильтре.</span><span class="sxs-lookup"><span data-stu-id="9f599-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="9f599-113">Так как этот запрос невозможно выполнить в базе данных, все данные извлекаются в память, а затем фильтр применяется в клиенте.</span><span class="sxs-lookup"><span data-stu-id="9f599-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="9f599-114">В зависимости от объема данных и степени их фильтрования это может привести к низкой производительности.</span><span class="sxs-lookup"><span data-stu-id="9f599-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

## <a name="client-evaluation-logging"></a><span data-ttu-id="9f599-115">Журналирование вычислений в клиенте</span><span class="sxs-lookup"><span data-stu-id="9f599-115">Client evaluation logging</span></span>

<span data-ttu-id="9f599-116">По умолчанию EF Core будет регистрировать предупреждение при выполнении запроса в клиенте.</span><span class="sxs-lookup"><span data-stu-id="9f599-116">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="9f599-117">Дополнительные сведения о просмотре выходных данных журнала см. в [этой статье](../miscellaneous/logging.md).</span><span class="sxs-lookup"><span data-stu-id="9f599-117">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> 

## <a name="optional-behavior-throw-an-exception-for-client-evaluation"></a><span data-ttu-id="9f599-118">Необязательное поведение: создание исключения при вычислении в клиенте</span><span class="sxs-lookup"><span data-stu-id="9f599-118">Optional behavior: throw an exception for client evaluation</span></span>

<span data-ttu-id="9f599-119">Вы можете изменить поведение при выполнении запроса в клиенте: либо создавать предупреждение, либо ничего не делать.</span><span class="sxs-lookup"><span data-stu-id="9f599-119">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="9f599-120">Это можно указать при настройке параметров для вашего контекста — обычно в `DbContext.OnConfiguring` или в `Startup.cs`, если вы используете ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9f599-120">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
