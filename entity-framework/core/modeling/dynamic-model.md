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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="60a74-102">Чередование между несколькими моделями с одинаковым типом DbContext</span><span class="sxs-lookup"><span data-stu-id="60a74-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="60a74-103">Модель, встроенная в `OnModelCreating`, может использовать свойство в контексте для изменения способа построения модели.</span><span class="sxs-lookup"><span data-stu-id="60a74-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="60a74-104">Например, можно использовать для исключения определенного свойства:</span><span class="sxs-lookup"><span data-stu-id="60a74-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="60a74-105">имоделкачекэйфактори</span><span class="sxs-lookup"><span data-stu-id="60a74-105">IModelCacheKeyFactory</span></span>

<span data-ttu-id="60a74-106">Однако если вы предпринимали попытку выполнения описанных выше действий без дополнительных изменений, то при создании нового контекста для любого значения `IgnoreIntProperty`будет возникнет одна и та же модель.</span><span class="sxs-lookup"><span data-stu-id="60a74-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="60a74-107">Это вызвано тем, что механизм кэширования модели EF использует для повышения производительности, вызывая `OnModelCreating` только один раз и выполнив кэширование модели.</span><span class="sxs-lookup"><span data-stu-id="60a74-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="60a74-108">По умолчанию EF предполагает, что для любого заданного типа контекста модель будет одинаковой.</span><span class="sxs-lookup"><span data-stu-id="60a74-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="60a74-109">Для выполнения этой реализации по умолчанию `IModelCacheKeyFactory` Возвращает ключ, который просто содержит тип контекста.</span><span class="sxs-lookup"><span data-stu-id="60a74-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="60a74-110">Чтобы изменить это, необходимо заменить службу `IModelCacheKeyFactory`.</span><span class="sxs-lookup"><span data-stu-id="60a74-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="60a74-111">Новая реализация должна возвращать объект, который можно сравнить с другими ключами модели, используя метод `Equals`, который учитывает все переменные, влияющие на модель:</span><span class="sxs-lookup"><span data-stu-id="60a74-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
