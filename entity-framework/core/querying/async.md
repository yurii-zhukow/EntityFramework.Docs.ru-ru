---
title: "Асинхронные запросы - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
ms.technology: entity-framework-core
uid: core/querying/async
ms.openlocfilehash: 6554f04d0edfe0ca2ee72ebed8b878a1997a9500
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-queries"></a><span data-ttu-id="e1282-102">Асинхронные запросы</span><span class="sxs-lookup"><span data-stu-id="e1282-102">Asynchronous Queries</span></span>

<span data-ttu-id="e1282-103">Асинхронные запросы избежать, блокирующий поток, пока запрос выполняется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="e1282-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="e1282-104">Это может быть полезно предотвратить замораживание пользовательского интерфейса толстая клиентского приложения.</span><span class="sxs-lookup"><span data-stu-id="e1282-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="e1282-105">Асинхронные операции можно также увеличить пропускную способность в веб-приложении, где поток может быть освобожден для обслуживания других запросов завершения операции базы данных.</span><span class="sxs-lookup"><span data-stu-id="e1282-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="e1282-106">Дополнительные сведения см. в разделе [асинхронное программирование в C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="e1282-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="e1282-107">Основные EF не поддерживает несколько параллельных операций, которые выполняются на том же экземпляре контекста.</span><span class="sxs-lookup"><span data-stu-id="e1282-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="e1282-108">Всегда следует подождать завершения перед началом следующей операции операции.</span><span class="sxs-lookup"><span data-stu-id="e1282-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="e1282-109">Обычно это делается с помощью `await` ключевое слово для каждой асинхронной операции.</span><span class="sxs-lookup"><span data-stu-id="e1282-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="e1282-110">Entity Framework Core предоставляет набор методов асинхронного расширения, которые можно использовать в качестве альтернативы методы LINQ, вызывающие выполнение запроса и результатов, возвращенных.</span><span class="sxs-lookup"><span data-stu-id="e1282-110">Entity Framework Core provides a set of asynchronous extension methods that can be used as an alternative to the LINQ methods that cause a query to be executed and results returned.</span></span> <span data-ttu-id="e1282-111">Примеры включают `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`и т. д. Нет асинхронные версии операторы LINQ например `Where(...)`, `OrderBy(...)`и др., так как эти методы построения вверх по дереву выражения LINQ, а не вызывают запрос для выполнения в базе данных.</span><span class="sxs-lookup"><span data-stu-id="e1282-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. There are not async versions of LINQ operators such as `Where(...)`, `OrderBy(...)`, etc. because these methods only build up the LINQ expression tree and do not cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="e1282-112">EF Core асинхронные методы расширения определяются в `Microsoft.EntityFrameworkCore` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="e1282-112">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="e1282-113">Это пространство имен должны импортироваться для методы, которые будут доступны.</span><span class="sxs-lookup"><span data-stu-id="e1282-113">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
