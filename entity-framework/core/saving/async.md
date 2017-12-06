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
# <a name="asynchronous-saving"></a>Асинхронного сохранения

Асинхронного сохранения позволяет избежать блокировки потока, пока запись изменений базы данных. Это может быть полезно предотвратить замораживание пользовательского интерфейса толстая клиентского приложения. Асинхронные операции можно также увеличить пропускную способность в веб-приложении, где поток может быть освобожден для обслуживания других запросов завершения операции базы данных. Дополнительные сведения см. в разделе [асинхронное программирование в C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> Основные EF не поддерживает несколько параллельных операций, которые выполняются на том же экземпляре контекста. Всегда следует подождать завершения перед началом следующей операции операции. Обычно это делается с помощью `await` ключевое слово для каждой асинхронной операции.

Entity Framework Core предоставляет `DbContext.SaveChangesAsync()` асинхронной вместо `DbContext.SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
