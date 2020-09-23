---
title: Явная загрузка связанных данных — EF Core
description: Явная загрузка связанных данных с помощью Entity Framework Core
author: roji
ms.date: 9/8/2020
uid: core/querying/related-data/explicit
ms.openlocfilehash: 4cfc11237b498f5357476ee4ad96fdc279cd3fee
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078897"
---
# <a name="explicit-loading-of-related-data"></a>Явная загрузка связанных данных

## <a name="explicit-loading"></a>Явная загрузка

Вы можете явно загрузить свойство навигации с помощью API `DbContext.Entry(...)`.

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Sample.cs#Eager)]

Вы также можете явно загрузить свойство навигации, выполнив отдельный запрос, который возвращает связанные сущности. Если отслеживание изменений включено, то при материализации сущности запросом EF Core автоматически установит свойства навигации вновь загруженной сущности для ссылки на любые уже загруженные сущности и задаст свойства навигации уже загруженных сущностей, чтобы ссылаться на вновь загруженные сущности.

## <a name="querying-related-entities"></a>Запрос связанных сущностей

Можно также получить запрос LINQ, который представляет содержимое свойства навигации.

Это позволяет применять дополнительные операторы к запросу. Например, применение оператора агрегирования в связанных сущностях без их загрузки в память.

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Можно также фильтровать связанные сущности, которые загружаются в память.

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryFiltered)]
