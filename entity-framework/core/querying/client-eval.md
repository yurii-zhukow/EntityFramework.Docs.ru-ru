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
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="10c89-102">Vs клиента. Оценка Server</span><span class="sxs-lookup"><span data-stu-id="10c89-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="10c89-103">Entity Framework Core поддерживает части запроса, оцениваемого на клиенте и части, отправленных в базе данных.</span><span class="sxs-lookup"><span data-stu-id="10c89-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="10c89-104">Именно поставщик базы данных, чтобы определить, какие части запроса будет вычисляться в базе данных.</span><span class="sxs-lookup"><span data-stu-id="10c89-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="10c89-105">Можно просмотреть в этой статье [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) на GitHub.</span><span class="sxs-lookup"><span data-stu-id="10c89-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="10c89-106">Оценки клиента</span><span class="sxs-lookup"><span data-stu-id="10c89-106">Client evaluation</span></span>

<span data-ttu-id="10c89-107">В следующем примере вспомогательный метод используется для стандартизации URL-адреса для веб-журналы, которые возвращаются из базы данных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="10c89-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="10c89-108">Так как поставщик SQL Server не понять, как реализовать этот метод, не удается преобразовать ее в SQL.</span><span class="sxs-lookup"><span data-stu-id="10c89-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="10c89-109">Все другие аспекты запроса вычисляются в базе данных, но передача возвращаемого `URL` посредством этого метода выполняется на клиенте.</span><span class="sxs-lookup"><span data-stu-id="10c89-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

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

## <a name="disabling-client-evaluation"></a><span data-ttu-id="10c89-110">Отключение оценки клиента</span><span class="sxs-lookup"><span data-stu-id="10c89-110">Disabling client evaluation</span></span>

<span data-ttu-id="10c89-111">Во время оценки клиента могут быть очень полезными, в некоторых случаях это может привести к снижению производительности.</span><span class="sxs-lookup"><span data-stu-id="10c89-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="10c89-112">Рассмотрим следующий запрос, где вспомогательный метод сейчас используется в фильтре.</span><span class="sxs-lookup"><span data-stu-id="10c89-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="10c89-113">Так как не удается выполнить в базе данных, все данные помещаются в памяти, а затем фильтр применяется на клиентском компьютере.</span><span class="sxs-lookup"><span data-stu-id="10c89-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="10c89-114">В зависимости от объема данных, а также объем данных, фильтруются это может привести к снижению производительности.</span><span class="sxs-lookup"><span data-stu-id="10c89-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

<span data-ttu-id="10c89-115">По умолчанию EF Core журнал предупреждения при выполнении оценки клиента.</span><span class="sxs-lookup"><span data-stu-id="10c89-115">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="10c89-116">В разделе [входа](../miscellaneous/logging.md) Дополнительные сведения о просмотре выходных данных ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="10c89-116">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="10c89-117">Можно изменить поведение при возникновении оценки клиента выдать или не выполняют никаких действий.</span><span class="sxs-lookup"><span data-stu-id="10c89-117">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="10c89-118">Это делается при настройке параметров для текущего контекста — обычно в `DbContext.OnConfiguring`, или в `Startup.cs` при использовании ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10c89-118">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
