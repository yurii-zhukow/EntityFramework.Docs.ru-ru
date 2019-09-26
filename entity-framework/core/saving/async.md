---
title: Асинхронное сохранение — EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 0823b86f0579dd3e42f6bd2aebfb433d3cbe00ab
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197821"
---
# <a name="asynchronous-saving"></a><span data-ttu-id="7aaea-102">Асинхронное сохранение</span><span class="sxs-lookup"><span data-stu-id="7aaea-102">Asynchronous Saving</span></span>

<span data-ttu-id="7aaea-103">Асинхронное сохранение позволяет избежать блокировки потока при записи изменений в базу данных.</span><span class="sxs-lookup"><span data-stu-id="7aaea-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="7aaea-104">Эта функция помогает предотвратить замораживание пользовательского интерфейса многофункционального клиентского приложения.</span><span class="sxs-lookup"><span data-stu-id="7aaea-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="7aaea-105">Асинхронные операции могут также увеличить пропускную способность в веб-приложении, где можно высвободить поток для обработки других запросов во время завершения операции базы данных.</span><span class="sxs-lookup"><span data-stu-id="7aaea-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="7aaea-106">Дополнительные сведения см. в статье об [асинхронном программировании на C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="7aaea-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="7aaea-107">EF Core не поддерживает выполнение нескольких параллельных операций в одном экземпляре контекста.</span><span class="sxs-lookup"><span data-stu-id="7aaea-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="7aaea-108">Следует подождать завершения одной операции, прежде чем запускать следующую.</span><span class="sxs-lookup"><span data-stu-id="7aaea-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="7aaea-109">Для этого обычно нужно указать ключевое слово `await` в каждой асинхронной операции.</span><span class="sxs-lookup"><span data-stu-id="7aaea-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="7aaea-110">Entity Framework Core предоставляет `DbContext.SaveChangesAsync()` в качестве асинхронной альтернативы `DbContext.SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="7aaea-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Async/Sample.cs#Sample)]
