---
title: Асинхронные запросы — EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: de00e25279e29355a4eb3e55597a8578ceccecb6
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993569"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="f439f-102">Асинхронные запросы</span><span class="sxs-lookup"><span data-stu-id="f439f-102">Asynchronous Queries</span></span>

<span data-ttu-id="f439f-103">Асинхронные запросы позволяют избежать блокирования потока при выполнении запроса в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f439f-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="f439f-104">Они помогают предотвратить замораживание пользовательского интерфейса многофункционального клиентского приложения.</span><span class="sxs-lookup"><span data-stu-id="f439f-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="f439f-105">Асинхронные операции могут также увеличить пропускную способность в веб-приложении, где можно высвободить поток для обработки других запросов во время завершения операции базы данных.</span><span class="sxs-lookup"><span data-stu-id="f439f-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="f439f-106">Дополнительные сведения см. в статье об [асинхронном программировании на C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="f439f-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="f439f-107">EF Core не поддерживает выполнение нескольких параллельных операций в одном экземпляре контекста.</span><span class="sxs-lookup"><span data-stu-id="f439f-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="f439f-108">Следует подождать завершения одной операции, прежде чем запускать следующую.</span><span class="sxs-lookup"><span data-stu-id="f439f-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="f439f-109">Для этого обычно нужно указать ключевое слово `await` в каждой асинхронной операции.</span><span class="sxs-lookup"><span data-stu-id="f439f-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="f439f-110">Entity Framework Core предоставляет набор асинхронных методов расширения, которые можно использовать вместо методов LINQ, вызывающих выполнение запроса и возвращение результатов.</span><span class="sxs-lookup"><span data-stu-id="f439f-110">Entity Framework Core provides a set of asynchronous extension methods that can be used as an alternative to the LINQ methods that cause a query to be executed and results returned.</span></span> <span data-ttu-id="f439f-111">Примеры: `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()` и т. д. Так как эти методы предназначены только для создания дерева выражений LINQ и не вызывают выполнение запроса в базе данных, асинхронные версии операторов LINQ отсутствуют (например, `Where(...)`, `OrderBy(...)` и т. д.).</span><span class="sxs-lookup"><span data-stu-id="f439f-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. There are not async versions of LINQ operators such as `Where(...)`, `OrderBy(...)`, etc. because these methods only build up the LINQ expression tree and do not cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="f439f-112">Асинхронные методы расширения EF Core определяются в пространстве имен `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="f439f-112">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="f439f-113">Это пространство имен необходимо импортировать, чтобы методы стали доступными.</span><span class="sxs-lookup"><span data-stu-id="f439f-113">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
