---
title: Асинхронные запросы — EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413783"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="783bd-102">Асинхронные запросы</span><span class="sxs-lookup"><span data-stu-id="783bd-102">Asynchronous Queries</span></span>

<span data-ttu-id="783bd-103">Асинхронные запросы позволяют избежать блокирования потока при выполнении запроса в базе данных.</span><span class="sxs-lookup"><span data-stu-id="783bd-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="783bd-104">Асинхронные запросы важны для обеспечения быстрого отклика пользовательского интерфейса в толстых клиентах.</span><span class="sxs-lookup"><span data-stu-id="783bd-104">Async queries are important for keeping a responsive UI in thick-client applications.</span></span> <span data-ttu-id="783bd-105">Они могут также увеличить пропускную способность в веб-приложении, где можно высвободить поток для обработки других запросов.</span><span class="sxs-lookup"><span data-stu-id="783bd-105">They can also increase throughput in web applications where they free up the thread to service other requests in web applications.</span></span> <span data-ttu-id="783bd-106">Дополнительные сведения см. в статье об [асинхронном программировании на C#](/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="783bd-106">For more information, see [Asynchronous Programming in C#](/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="783bd-107">EF Core не поддерживает выполнение нескольких параллельных операций в одном экземпляре контекста.</span><span class="sxs-lookup"><span data-stu-id="783bd-107">EF Core doesn't support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="783bd-108">Следует подождать завершения одной операции, прежде чем запускать следующую.</span><span class="sxs-lookup"><span data-stu-id="783bd-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="783bd-109">Для этого обычно нужно указать ключевое слово `await` в каждой асинхронной операции.</span><span class="sxs-lookup"><span data-stu-id="783bd-109">This is typically done by using the `await` keyword on each async operation.</span></span>

<span data-ttu-id="783bd-110">Entity Framework Core предоставляет набор асинхронных методов расширения, которые аналогичны методам LINQ, выполняющим запросы и возвращающим результаты.</span><span class="sxs-lookup"><span data-stu-id="783bd-110">Entity Framework Core provides a set of async extension methods similar to the LINQ methods, which execute a query and return results.</span></span> <span data-ttu-id="783bd-111">К примерам относятся `ToListAsync()`, `ToArrayAsync()` и `SingleAsync()`.</span><span class="sxs-lookup"><span data-stu-id="783bd-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`.</span></span> <span data-ttu-id="783bd-112">Так как эти методы предназначены только для создания дерева выражения LINQ и не вызывают выполнение запроса в базе данных, асинхронные версии некоторых операторов LINQ, таких как `Where(...)` или `OrderBy(...)`, отсутствуют.</span><span class="sxs-lookup"><span data-stu-id="783bd-112">There are no async versions of some LINQ operators such as `Where(...)` or `OrderBy(...)` because these methods only build up the LINQ expression tree and don't cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="783bd-113">Асинхронные методы расширения EF Core определяются в пространстве имен `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="783bd-113">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="783bd-114">Это пространство имен необходимо импортировать, чтобы методы стали доступными.</span><span class="sxs-lookup"><span data-stu-id="783bd-114">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
