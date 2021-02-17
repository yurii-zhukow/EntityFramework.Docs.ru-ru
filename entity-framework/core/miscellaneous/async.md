---
title: Асинхронное программирование — EF Core
description: Асинхронное выполнение запросов и сохранение данных с помощью Entity Framework Core
author: roji
ms.date: 9/2/2020
uid: core/miscellaneous/async
ms.openlocfilehash: 78c109e9fd73dbb0451f9e29562799e4d22d66c9
ms.sourcegitcommit: 704240349e18b6404e5a809f5b7c9d365b152e2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/16/2021
ms.locfileid: "100543358"
---
# <a name="asynchronous-programming"></a><span data-ttu-id="348d4-103">Асинхронное программирование</span><span class="sxs-lookup"><span data-stu-id="348d4-103">Asynchronous Programming</span></span>

<span data-ttu-id="348d4-104">Асинхронные операции Избегайте блокировки потока во время выполнения запроса в базе данных.</span><span class="sxs-lookup"><span data-stu-id="348d4-104">Asynchronous operations avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="348d4-105">Асинхронные операции важны для поддержания интерфейса реагирования в многофункциональных клиентских приложениях, а также для увеличения пропускной способности в веб-приложениях, когда они освобождают поток для обслуживания других запросов в веб-приложениях.</span><span class="sxs-lookup"><span data-stu-id="348d4-105">Async operations are important for keeping a responsive UI in rich client applications, and can also increase throughput in web applications where they free up the thread to service other requests in web applications.</span></span>

<span data-ttu-id="348d4-106">После .NET Standard EF Core предоставляет асинхронные аналоги для всех синхронных методов, выполняющих операции ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="348d4-106">Following the .NET standard, EF Core provides asynchronous counterparts to all synchronous methods which perform I/O.</span></span> <span data-ttu-id="348d4-107">Они имеют те же эффекты, что и методы синхронизации, и могут использоваться с `async` `await` ключевыми словами C# и.</span><span class="sxs-lookup"><span data-stu-id="348d4-107">These have the same effects as the sync methods, and can be used with the C# `async` and `await` keywords.</span></span> <span data-ttu-id="348d4-108">Например, вместо использования DbContext. SaveChanges, который будет блокировать поток во время ввода-вывода базы данных, можно использовать DbContext. SaveChangesAsync:</span><span class="sxs-lookup"><span data-stu-id="348d4-108">For example, instead of using DbContext.SaveChanges, which will block a thread while database I/O is performed, DbContext.SaveChangesAsync can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Async/Program.cs#SaveChangesAsync)]

<span data-ttu-id="348d4-109">Дополнительные сведения см. [в разделе Общие документы по асинхронному программированию C#](/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="348d4-109">For more information, see [the general C# asynchronous programming docs](/dotnet/csharp/async).</span></span>

> [!WARNING]
> <span data-ttu-id="348d4-110">EF Core не поддерживает выполнение нескольких параллельных операций в одном экземпляре контекста.</span><span class="sxs-lookup"><span data-stu-id="348d4-110">EF Core doesn't support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="348d4-111">Следует подождать завершения одной операции, прежде чем запускать следующую.</span><span class="sxs-lookup"><span data-stu-id="348d4-111">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="348d4-112">Для этого обычно нужно указать ключевое слово `await` в каждой асинхронной операции.</span><span class="sxs-lookup"><span data-stu-id="348d4-112">This is typically done by using the `await` keyword on each async operation.</span></span>

> [!WARNING]
> <span data-ttu-id="348d4-113">В асинхронной реализации [Microsoft. Data. SqlClient](https://github.com/dotnet/SqlClient) возникают некоторые известные проблемы (например, [#593](https://github.com/dotnet/SqlClient/issues/593), [#601](https://github.com/dotnet/SqlClient/issues/601)и др.).</span><span class="sxs-lookup"><span data-stu-id="348d4-113">The async implementation of [Microsoft.Data.SqlClient](https://github.com/dotnet/SqlClient) unfortunately has some known issues (e.g. [#593](https://github.com/dotnet/SqlClient/issues/593), [#601](https://github.com/dotnet/SqlClient/issues/601), and others).</span></span>

> [!NOTE]
> <span data-ttu-id="348d4-114">EF Core передает токены отмены в используемый базовый поставщик базы данных (например, Microsoft. Data. SqlClient).</span><span class="sxs-lookup"><span data-stu-id="348d4-114">EF Core passes cancellation tokens down to the underlying database provider in use (e.g. Microsoft.Data.SqlClient).</span></span> <span data-ttu-id="348d4-115">Эти токены могут быть или не учитываться. обратитесь к документации поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="348d4-115">These tokens may or may not be honored - consult your database provider's documentation.</span></span>

## <a name="async-linq-operators"></a><span data-ttu-id="348d4-116">Асинхронные операторы LINQ</span><span class="sxs-lookup"><span data-stu-id="348d4-116">Async LINQ operators</span></span>

<span data-ttu-id="348d4-117">Чтобы обеспечить асинхронное выполнение запросов LINQ, EF Core предоставляет набор асинхронных методов расширения, которые выполняют запрос и возвращают результаты.</span><span class="sxs-lookup"><span data-stu-id="348d4-117">In order to support executing LINQ queries asynchronously, EF Core provides a set of async extension methods which execute the query and return results.</span></span> <span data-ttu-id="348d4-118">Эти аналоги со стандартными синхронными операторами LINQ включают Толистасинк, Синглеасинк, Асасинценумерабле и т. д.:</span><span class="sxs-lookup"><span data-stu-id="348d4-118">These counterparts to the standard, synchronous LINQ operators include ToListAsync, SingleAsync, AsAsyncEnumerable, etc.:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Async/Program.cs#ToListAsync)]

<span data-ttu-id="348d4-119">Обратите внимание, что отсутствуют асинхронные версии некоторых операторов LINQ, таких как WHERE или OrderBy, поскольку они создают только дерево выражений LINQ и не вызывают выполнение запроса в базе данных.</span><span class="sxs-lookup"><span data-stu-id="348d4-119">Note that there are no async versions of some LINQ operators such as Where or OrderBy, because these only build up the LINQ expression tree and don't cause the query to be executed in the database.</span></span> <span data-ttu-id="348d4-120">Только операторы, которые вызывают выполнение запроса, имеют асинхронные аналоги.</span><span class="sxs-lookup"><span data-stu-id="348d4-120">Only operators which cause query execution have async counterparts.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="348d4-121">Асинхронные методы расширения EF Core определяются в пространстве имен `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="348d4-121">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="348d4-122">Это пространство имен необходимо импортировать, чтобы методы стали доступными.</span><span class="sxs-lookup"><span data-stu-id="348d4-122">This namespace must be imported for the methods to be available.</span></span>

## <a name="client-side-async-linq-operators"></a><span data-ttu-id="348d4-123">Асинхронные операторы LINQ на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="348d4-123">Client-side async LINQ operators</span></span>

<span data-ttu-id="348d4-124">Операторы Async LINQ, описанные выше, можно использовать только в запросах EF. их нельзя использовать с запросом LINQ to Objects на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="348d4-124">The async LINQ operators discussed above can only be used on EF queries - you cannot use them with client-side LINQ to Objects query.</span></span> <span data-ttu-id="348d4-125">Для выполнения асинхронных операций LINQ на стороне клиента за пределами EF используется [пакет System. LINQ. Async](https://www.nuget.org/packages/System.Linq.Async); Этот пакет может быть особенно полезен для выполнения операций на клиенте, которые не могут быть преобразованы для ознакомления на сервере.</span><span class="sxs-lookup"><span data-stu-id="348d4-125">To perform client-side async LINQ operations outside of EF, use the [System.Linq.Async package](https://www.nuget.org/packages/System.Linq.Async); this package can be especially useful for performing operations on the client that cannot be translated for evaluation at the server.</span></span>

<span data-ttu-id="348d4-126">К сожалению, обращение к System. Interactive. Async приводит к неоднозначности ошибок компиляции вызова для операторов LINQ, применяемых к Дбсетс EF; Это затрудняет использование в одном проекте как EF, так и System. Interactive. Async.</span><span class="sxs-lookup"><span data-stu-id="348d4-126">Unfortunately, referencing System.Interactive.Async causes ambiguous invocation compilation errors on LINQ operators applied to EF's DbSets; this makes it hard to use both EF and System.Interactive.Async in the same project.</span></span> <span data-ttu-id="348d4-127">Чтобы обойти эту ошибку, добавьте Аскуерябле в DbSet:</span><span class="sxs-lookup"><span data-stu-id="348d4-127">To work around this issue, add AsQueryable to your DbSet:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/AsyncWithSystemInteractive/Program.cs#SystemInteractiveAsync)]
