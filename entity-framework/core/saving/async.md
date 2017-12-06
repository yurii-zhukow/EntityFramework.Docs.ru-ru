---
title: "Асинхронные экономия - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
ms.technology: entity-framework-core
uid: core/saving/async
ms.openlocfilehash: 640e5f131b0e9ef4e5028d1dcaf80f3e5bbd9d9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-saving"></a><span data-ttu-id="380fb-102">Асинхронного сохранения</span><span class="sxs-lookup"><span data-stu-id="380fb-102">Asynchronous Saving</span></span>

<span data-ttu-id="380fb-103">Асинхронного сохранения позволяет избежать блокировки потока, пока запись изменений базы данных.</span><span class="sxs-lookup"><span data-stu-id="380fb-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="380fb-104">Это может быть полезно предотвратить замораживание пользовательского интерфейса толстая клиентского приложения.</span><span class="sxs-lookup"><span data-stu-id="380fb-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="380fb-105">Асинхронные операции можно также увеличить пропускную способность в веб-приложении, где поток может быть освобожден для обслуживания других запросов завершения операции базы данных.</span><span class="sxs-lookup"><span data-stu-id="380fb-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="380fb-106">Дополнительные сведения см. в разделе [асинхронное программирование в C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="380fb-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="380fb-107">Основные EF не поддерживает несколько параллельных операций, которые выполняются на том же экземпляре контекста.</span><span class="sxs-lookup"><span data-stu-id="380fb-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="380fb-108">Всегда следует подождать завершения перед началом следующей операции операции.</span><span class="sxs-lookup"><span data-stu-id="380fb-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="380fb-109">Обычно это делается с помощью `await` ключевое слово для каждой асинхронной операции.</span><span class="sxs-lookup"><span data-stu-id="380fb-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="380fb-110">Entity Framework Core предоставляет `DbContext.SaveChangesAsync()` асинхронной вместо `DbContext.SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="380fb-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
