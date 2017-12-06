---
title: "Переключаясь между несколько моделей с тем же типом DbContext - EF Core"
author: AndriySvyryd
uid: core/modeling/dynamic-model
ms.openlocfilehash: e6828c62c55ae6f48d46a20528268264c3f22b26
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="d4deb-102">Переключаясь между несколько моделей с тем же типом DbContext</span><span class="sxs-lookup"><span data-stu-id="d4deb-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="d4deb-103">Модели, построенной `OnModelCreating` использовать свойство контекста для изменения способа построения модели.</span><span class="sxs-lookup"><span data-stu-id="d4deb-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="d4deb-104">Например он может использоваться для исключения определенное свойство:</span><span class="sxs-lookup"><span data-stu-id="d4deb-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="d4deb-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="d4deb-105">IModelCacheKeyFactory</span></span>
<span data-ttu-id="d4deb-106">Однако при попытке выполнения указанных выше без дополнительных изменений получится ту же модель всякий раз при создании нового контекста для любого значения параметра `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="d4deb-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="d4deb-107">Причиной этого является модель EF используется для повышения производительности путем вызова только механизм кэширования `OnModelCreating` один раз и кэширование модели.</span><span class="sxs-lookup"><span data-stu-id="d4deb-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="d4deb-108">По умолчанию EF предполагает, что для любого контекста заданного типа модели будут совпадать.</span><span class="sxs-lookup"><span data-stu-id="d4deb-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="d4deb-109">Для выполнения этой задачи по умолчанию реализация `IModelCacheKeyFactory` Возвращает ключ, представляющий тип контекста.</span><span class="sxs-lookup"><span data-stu-id="d4deb-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="d4deb-110">Чтобы изменить это потребность в замене `IModelCacheKeyFactory` службы.</span><span class="sxs-lookup"><span data-stu-id="d4deb-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="d4deb-111">Новая реализация должно возвращать объект, который можно сравнивать с другими ключами модели с помощью `Equals` метод, который учитывает все переменные, которые влияют на модели:</span><span class="sxs-lookup"><span data-stu-id="d4deb-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
