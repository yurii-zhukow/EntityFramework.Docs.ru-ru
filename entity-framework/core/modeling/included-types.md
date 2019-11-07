---
title: Включая &, исключая типы EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: 1249e71c02e58afe7fe06b3fdcf523dfa0c9b17c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655739"
---
# <a name="including--excluding-types"></a>Включение и исключение типов

Включение типа в модель означает, что EF имеет метаданные об этом типе и будет пытаться считывать и записывать экземпляры из базы данных или из нее.

## <a name="conventions"></a>Соглашения

По соглашению типы, предоставляемые в свойствах `DbSet` в контексте, включаются в модель. Кроме того, также включены типы, упоминаемые в методе `OnModelCreating`. Наконец, все типы, которые обнаруживаются рекурсивным просмотром свойств навигации обнаруженных типов, также включаются в модель.

**Например, в следующем коде обнаруживаются все три типа:**

* `Blog`, так как он предоставляется в свойстве `DbSet` в контексте

* `Post`, так как он обнаруживается с помощью свойства навигации `Blog.Posts`

* `AuditEntry`, так как он упоминается в `OnModelCreating`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/IncludedTypes.cs?name=IncludedTypes&highlight=3,7,16)]

## <a name="data-annotations"></a>Заметки к данным

Заметки к данным можно использовать для исключения типа из модели.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>Текучий API

API-интерфейс Fluent можно использовать для исключения типа из модели.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
