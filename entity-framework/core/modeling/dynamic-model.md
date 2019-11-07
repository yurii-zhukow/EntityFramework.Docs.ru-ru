---
title: Чередование между несколькими моделями с одинаковым типом DbContext — EF Core
author: AndriySvyryd
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 034076b1595894e80b98467354f6c9f139bd7426
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655729"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Чередование между несколькими моделями с одинаковым типом DbContext

Модель, встроенная в `OnModelCreating`, может использовать свойство в контексте для изменения способа построения модели. Например, можно использовать для исключения определенного свойства:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>имоделкачекэйфактори

Однако если вы предпринимали попытку выполнения описанных выше действий без дополнительных изменений, то при создании нового контекста для любого значения `IgnoreIntProperty`будет возникнет одна и та же модель. Это вызвано тем, что механизм кэширования модели EF использует для повышения производительности, вызывая `OnModelCreating` только один раз и выполнив кэширование модели.

По умолчанию EF предполагает, что для любого заданного типа контекста модель будет одинаковой. Для выполнения этой реализации по умолчанию `IModelCacheKeyFactory` Возвращает ключ, который просто содержит тип контекста. Чтобы изменить это, необходимо заменить службу `IModelCacheKeyFactory`. Новая реализация должна возвращать объект, который можно сравнить с другими ключами модели, используя метод `Equals`, который учитывает все переменные, влияющие на модель:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
