---
title: Асинхронное сохранение — EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 6f482a77300ff2930953686751a579b022bf6f77
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997286"
---
# <a name="asynchronous-saving"></a>Асинхронное сохранение

Асинхронное сохранение позволяет избежать блокировки потока при записи изменений в базу данных. Эта функция помогает предотвратить замораживание пользовательского интерфейса многофункционального клиентского приложения. Асинхронные операции могут также увеличить пропускную способность в веб-приложении, где можно высвободить поток для обработки других запросов во время завершения операции базы данных. Дополнительные сведения см. в статье об [асинхронном программировании на C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core не поддерживает выполнение нескольких параллельных операций в одном экземпляре контекста. Следует подождать завершения одной операции, прежде чем запускать следующую. Для этого обычно нужно указать ключевое слово `await` в каждой асинхронной операции.

Entity Framework Core предоставляет `DbContext.SaveChangesAsync()` в качестве асинхронной альтернативы `DbContext.SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
