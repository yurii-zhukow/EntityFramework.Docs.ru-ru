---
title: "Переключаясь между несколько моделей с тем же типом DbContext - EF Core"
author: AndriySvyryd
ms.author: divega
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/dynamic-model
ms.openlocfilehash: 57136802001124ebf9ae7682e33f8dc7826fc2b0
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Переключаясь между несколько моделей с тем же типом DbContext

Модели, построенной `OnModelCreating` использовать свойство контекста для изменения способа построения модели. Например он может использоваться для исключения определенное свойство:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
Однако при попытке выполнения указанных выше без дополнительных изменений получится ту же модель всякий раз при создании нового контекста для любого значения параметра `IgnoreIntProperty`. Причиной этого является модель EF используется для повышения производительности путем вызова только механизм кэширования `OnModelCreating` один раз и кэширование модели.

По умолчанию EF предполагает, что для любого контекста заданного типа модели будут совпадать. Для выполнения этой задачи по умолчанию реализация `IModelCacheKeyFactory` Возвращает ключ, представляющий тип контекста. Чтобы изменить это потребность в замене `IModelCacheKeyFactory` службы. Новая реализация должно возвращать объект, который можно сравнивать с другими ключами модели с помощью `Equals` метод, который учитывает все переменные, которые влияют на модели:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
