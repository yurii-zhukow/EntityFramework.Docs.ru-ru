---
title: Асинхронное программирование — EF Core
description: Асинхронное выполнение запросов и сохранение данных с помощью Entity Framework Core
author: roji
ms.date: 9/2/2020
ms.assetid: 38f71624-7a68-4c72-a918-3e7b858ef090
uid: core/miscellaneous/async
ms.openlocfilehash: c5dff82fe2442cd5a21c143933cac2c5d4588281
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89620719"
---
# <a name="asynchronous-programming"></a><span data-ttu-id="f6ce5-103">Асинхронное программирование</span><span class="sxs-lookup"><span data-stu-id="f6ce5-103">Asynchronous Programming</span></span>

<span data-ttu-id="f6ce5-104">Асинхронные операции Избегайте блокировки потока во время выполнения запроса в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f6ce5-104">Asynchronous operations avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="f6ce5-105">Асинхронные операции важны для поддержания интерфейса реагирования в многофункциональных клиентских приложениях, а также для увеличения пропускной способности в веб-приложениях, когда они освобождают поток для обслуживания других запросов в веб-приложениях.</span><span class="sxs-lookup"><span data-stu-id="f6ce5-105">Async operations are important for keeping a responsive UI in rich client applications, and can also increase throughput in web applications where they free up the thread to service other requests in web applications.</span></span>

<span data-ttu-id="f6ce5-106">После .NET Standard EF Core предоставляет асинхронные аналоги для всех синхронных методов, выполняющих операции ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="f6ce5-106">Following the .NET standard, EF Core provides asynchronous counterparts to all synchronous methods which perform I/O.</span></span> <span data-ttu-id="f6ce5-107">Они имеют те же эффекты, что и методы синхронизации, и могут использоваться с `async` `await` ключевыми словами C# и.</span><span class="sxs-lookup"><span data-stu-id="f6ce5-107">These have the same effects as the sync methods, and can be used with the C# `async` and `await` keywords.</span></span> <span data-ttu-id="f6ce5-108">Например, вместо использования DbContext. SaveChanges, который будет блокировать поток во время ввода-вывода базы данных, можно использовать DbContext. SaveChangesAsync:</span><span class="sxs-lookup"><span data-stu-id="f6ce5-108">For example, instead of using DbContext.SaveChanges, which will block a thread while database I/O is performed, DbContext.SaveChangesAsync can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Async/Program.cs#SaveChangesAsync)]

<span data-ttu-id="f6ce5-109">Дополнительные сведения см. [в разделе Общие документы по асинхронному программированию C#](/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="f6ce5-109">For more information, see [the general C# asynchronous programming docs](/dotnet/csharp/async).</span></span>

> [!WARNING]
> <span data-ttu-id="f6ce5-110">EF Core не поддерживает выполнение нескольких параллельных операций в одном экземпляре контекста.</span><span class="sxs-lookup"><span data-stu-id="f6ce5-110">EF Core doesn't support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="f6ce5-111">Следует подождать завершения одной операции, прежде чем запускать следующую.</span><span class="sxs-lookup"><span data-stu-id="f6ce5-111">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="f6ce5-112">Для этого обычно нужно указать ключевое слово `await` в каждой асинхронной операции.</span><span class="sxs-lookup"><span data-stu-id="f6ce5-112">This is typically done by using the `await` keyword on each async operation.</span></span>

> [!NOTE]
> <span data-ttu-id="f6ce5-113">EF Core передает токены отмены в используемый базовый поставщик базы данных (например, Microsoft. Data. SqlClient).</span><span class="sxs-lookup"><span data-stu-id="f6ce5-113">EF Core passes cancellation tokens down to the underlying database provider in use (e.g. Microsoft.Data.SqlClient).</span></span> <span data-ttu-id="f6ce5-114">Эти токены могут быть или не учитываться. обратитесь к документации поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="f6ce5-114">These tokens may or may not be honored - consult your database provider's documentation.</span></span>  

## <a name="async-linq-operators"></a><span data-ttu-id="f6ce5-115">Асинхронные операторы LINQ</span><span class="sxs-lookup"><span data-stu-id="f6ce5-115">Async LINQ operators</span></span>

<span data-ttu-id="f6ce5-116">Чтобы обеспечить асинхронное выполнение запросов LINQ, EF Core предоставляет набор асинхронных методов расширения, которые выполняют запрос и возвращают результаты.</span><span class="sxs-lookup"><span data-stu-id="f6ce5-116">In order to support executing LINQ queries asynchronously, EF Core provides a set of async extension methods which execute the query and return results.</span></span> <span data-ttu-id="f6ce5-117">Эти аналоги со стандартными синхронными операторами LINQ включают Толистасинк, Синглеасинк, Асасинценумерабле и т. д.:</span><span class="sxs-lookup"><span data-stu-id="f6ce5-117">These counterparts to the standard, synchronous LINQ operators include ToListAsync, SingleAsync, AsAsyncEnumerable, etc.:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Async/Program.cs#ToListAsync)]

<span data-ttu-id="f6ce5-118">Обратите внимание, что отсутствуют асинхронные версии некоторых операторов LINQ, таких как WHERE или OrderBy, поскольку они создают только дерево выражений LINQ и не вызывают выполнение запроса в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f6ce5-118">Note that there are no async versions of some LINQ operators such as Where or OrderBy, because these only build up the LINQ expression tree and don't cause the query to be executed in the database.</span></span> <span data-ttu-id="f6ce5-119">Только операторы, которые вызывают выполнение запроса, имеют асинхронные аналоги.</span><span class="sxs-lookup"><span data-stu-id="f6ce5-119">Only operators which cause query execution have async counterparts.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f6ce5-120">Асинхронные методы расширения EF Core определяются в пространстве имен `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="f6ce5-120">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="f6ce5-121">Это пространство имен необходимо импортировать, чтобы методы стали доступными.</span><span class="sxs-lookup"><span data-stu-id="f6ce5-121">This namespace must be imported for the methods to be available.</span></span>

## <a name="client-side-async-linq-operators"></a><span data-ttu-id="f6ce5-122">Асинхронные операторы LINQ на стороне клиента</span><span class="sxs-lookup"><span data-stu-id="f6ce5-122">Client-side async LINQ operators</span></span>

<span data-ttu-id="f6ce5-123">Операторы Async LINQ, описанные выше, можно использовать только в запросах EF. их нельзя использовать с запросом LINQ to Objects на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="f6ce5-123">The async LINQ operators discussed above can only be used on EF queries - you cannot use them with client-side LINQ to Objects query.</span></span> <span data-ttu-id="f6ce5-124">Для выполнения асинхронных операций LINQ, выполняемых на стороне клиента вне EF, используйте [пакет System. Interactive. Async.](https://www.nuget.org/packages/System.Interactive.Async) Этот пакет может быть особенно полезен для выполнения операций на клиенте, которые не могут быть преобразованы для ознакомления на сервере.</span><span class="sxs-lookup"><span data-stu-id="f6ce5-124">To perform client-side async LINQ operations outside of EF, use the [System.Interactive.Async package](https://www.nuget.org/packages/System.Interactive.Async); this package can be especially useful for performing operations on the client that cannot be translated for evaluation at the server.</span></span>

<span data-ttu-id="f6ce5-125">К сожалению, обращение к System. Interactive. Async приводит к неоднозначности ошибок компиляции вызова для операторов LINQ, применяемых к Дбсетс EF; Это затрудняет использование в одном проекте как EF, так и System. Interactive. Async.</span><span class="sxs-lookup"><span data-stu-id="f6ce5-125">Unfortunately, referencing System.Interactive.Async causes ambiguous invocation compilation errors on LINQ operators applied to EF's DbSets; this makes it hard to use both EF and System.Interactive.Async in the same project.</span></span> <span data-ttu-id="f6ce5-126">Чтобы обойти эту ошибку, добавьте Аскуерябле в DbSet:</span><span class="sxs-lookup"><span data-stu-id="f6ce5-126">To work around this issue, add AsQueryable to your DbSet:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/AsyncWithSystemInteractive/Program.cs#SystemInteractiveAsync)]
