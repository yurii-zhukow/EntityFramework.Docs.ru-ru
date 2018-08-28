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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="1b359-102">Переключаясь между несколько моделей с тем же типом DbContext</span><span class="sxs-lookup"><span data-stu-id="1b359-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="1b359-103">Модель, построенная `OnModelCreating` использовать свойство контекста для изменения построение модели.</span><span class="sxs-lookup"><span data-stu-id="1b359-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="1b359-104">Например он может использоваться для исключения определенное свойство:</span><span class="sxs-lookup"><span data-stu-id="1b359-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="1b359-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="1b359-105">IModelCacheKeyFactory</span></span>
<span data-ttu-id="1b359-106">Однако если вы попробовал сделать выше без дополнительных изменений получится той же модели каждый раз при создании нового контекста для любого значения `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="1b359-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="1b359-107">Это связано с моделью EF использует для повышения производительности путем вызова только механизм кэширования `OnModelCreating` один раз и кэширования в модели.</span><span class="sxs-lookup"><span data-stu-id="1b359-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="1b359-108">По умолчанию EF считает, что для любого заданного контекста типа модели будут совпадать.</span><span class="sxs-lookup"><span data-stu-id="1b359-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="1b359-109">Для выполнения этой задачи по умолчанию реализация `IModelCacheKeyFactory` Возвращает ключ, который просто содержит тип контекста.</span><span class="sxs-lookup"><span data-stu-id="1b359-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="1b359-110">Изменить этот параметр необходимо заменить `IModelCacheKeyFactory` службы.</span><span class="sxs-lookup"><span data-stu-id="1b359-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="1b359-111">Новая реализация должен возвращать объект, который можно сравнивать для других ключей модели, с помощью `Equals` метод, который учитывает все переменные, которые влияют на модель:</span><span class="sxs-lookup"><span data-stu-id="1b359-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
