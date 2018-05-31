---
title: Асинхронное сохранение — EF Core
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
ms.technology: entity-framework-core
uid: core/saving/async
ms.openlocfilehash: 640e5f131b0e9ef4e5028d1dcaf80f3e5bbd9d9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052624"
---
# <a name="asynchronous-saving"></a>Асинхронное сохранение

Асинхронное сохранение позволяет избежать блокировки потока при записи изменений в базу данных. Эта функция помогает предотвратить замораживание пользовательского интерфейса многофункционального клиентского приложения. Асинхронные операции могут также увеличить пропускную способность в веб-приложении, где можно высвободить поток для обработки других запросов во время завершения операции базы данных. Дополнительные сведения см. в статье об [асинхронном программировании на C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core не поддерживает выполнение нескольких параллельных операций в одном экземпляре контекста. Следует подождать завершения одной операции, прежде чем запускать следующую. Для этого обычно нужно указать ключевое слово `await` в каждой асинхронной операции.

Entity Framework Core предоставляет `DbContext.SaveChangesAsync()` в качестве асинхронной альтернативы `DbContext.SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
