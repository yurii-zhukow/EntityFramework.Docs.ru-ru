---
title: Чередование между несколькими моделями с одинаковым типом DbContext — EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 35743b5ba87bb4239d7f758320f9facccc74330f
ms.sourcegitcommit: 92d54fe3702e0c92e198334da22bacb42e9842b1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/10/2020
ms.locfileid: "84664225"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="c23f9-102">Чередование между несколькими моделями с одинаковым типом DbContext</span><span class="sxs-lookup"><span data-stu-id="c23f9-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="c23f9-103">Модель, встроенная в, `OnModelCreating` может использовать свойство в контексте для изменения способа построения модели.</span><span class="sxs-lookup"><span data-stu-id="c23f9-103">The model built in `OnModelCreating` can use a property on the context to change how the model is built.</span></span> <span data-ttu-id="c23f9-104">Например, предположим, что нужно настроить сущность по-разному на основе какого-либо свойства:</span><span class="sxs-lookup"><span data-stu-id="c23f9-104">For example, suppose you wanted to configure an entity differently based on some property:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnModelCreating)]

<span data-ttu-id="c23f9-105">К сожалению, этот код не будет работать как есть, так как EF строит модель и выполняется `OnModelCreating` только один раз, что приводит к кэшированию результатов по соображениям производительности.</span><span class="sxs-lookup"><span data-stu-id="c23f9-105">Unfortunately, this code wouldn't work as-is, since EF builds the model and runs `OnModelCreating` only once, caching the result for performance reasons.</span></span> <span data-ttu-id="c23f9-106">Однако можно присоединиться к механизму кэширования модели, чтобы сделать EF осведомленным о свойстве, создающем различные модели.</span><span class="sxs-lookup"><span data-stu-id="c23f9-106">However, you can hook into the model caching mechanism to make EF aware of the property producing different models.</span></span>

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="c23f9-107">имоделкачекэйфактори</span><span class="sxs-lookup"><span data-stu-id="c23f9-107">IModelCacheKeyFactory</span></span>

<span data-ttu-id="c23f9-108">EF использует `IModelCacheKeyFactory` для создания ключей кэша для моделей; по умолчанию EF предполагает, что для любого заданного типа контекста модель будет одинаковой, поэтому реализация по умолчанию этой службы возвращает ключ, который содержит только тип контекста.</span><span class="sxs-lookup"><span data-stu-id="c23f9-108">EF uses the `IModelCacheKeyFactory` to generate cache keys for models; by default, EF assumes that for any given context type the model will be the same, so the default implementation of this service returns a key that just contains the context type.</span></span> <span data-ttu-id="c23f9-109">Чтобы создать различные модели из одного и того же типа контекста, необходимо заменить `IModelCacheKeyFactory` службу правильной реализацией. созданный ключ будет сравниваться с другими ключами модели с помощью `Equals` метода, принимая во внимание все переменные, влияющие на модель.</span><span class="sxs-lookup"><span data-stu-id="c23f9-109">To produce different models from the same context type, you need to replace the `IModelCacheKeyFactory` service with the correct implementation; the generated key will be compared to other model keys using the `Equals` method, taking into account all the variables that affect the model.</span></span>

<span data-ttu-id="c23f9-110">`UseIntProperty`При создании ключа кэша модели в следующей реализации учитывается учетная запись.</span><span class="sxs-lookup"><span data-stu-id="c23f9-110">The following implementation takes the `UseIntProperty` into account when producing a model cache key:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicModelCacheKeyFactory.cs?name=DynamicModel)]

<span data-ttu-id="c23f9-111">Наконец, зарегистрируйте новый `IModelCacheKeyFactory` в своем контексте `OnConfiguring` :</span><span class="sxs-lookup"><span data-stu-id="c23f9-111">Finally, register your new `IModelCacheKeyFactory` in your context's `OnConfiguring`:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnConfiguring)]

<span data-ttu-id="c23f9-112">См. [полный пример проекта](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) для получения дополнительных контекстов.</span><span class="sxs-lookup"><span data-stu-id="c23f9-112">See the [full sample project](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) for more context.</span></span>
