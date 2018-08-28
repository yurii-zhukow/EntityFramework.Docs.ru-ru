---
title: Переключаясь между несколько моделей с тем же типом DbContext — EF Core
author: AndriySvyryd
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 1d87efb668c12a2862583fba16a6c444b3cda9de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994990"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Переключаясь между несколько моделей с тем же типом DbContext

Модель, построенная `OnModelCreating` использовать свойство контекста для изменения построение модели. Например он может использоваться для исключения определенное свойство:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
Однако если вы попробовал сделать выше без дополнительных изменений получится той же модели каждый раз при создании нового контекста для любого значения `IgnoreIntProperty`. Это связано с моделью EF использует для повышения производительности путем вызова только механизм кэширования `OnModelCreating` один раз и кэширования в модели.

По умолчанию EF считает, что для любого заданного контекста типа модели будут совпадать. Для выполнения этой задачи по умолчанию реализация `IModelCacheKeyFactory` Возвращает ключ, который просто содержит тип контекста. Изменить этот параметр необходимо заменить `IModelCacheKeyFactory` службы. Новая реализация должен возвращать объект, который можно сравнивать для других ключей модели, с помощью `Equals` метод, который учитывает все переменные, которые влияют на модель:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
