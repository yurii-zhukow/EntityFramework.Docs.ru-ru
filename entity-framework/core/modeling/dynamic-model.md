---
title: Чередование между несколькими моделями с одинаковым типом DbContext — EF Core
description: Чередование между несколькими моделями с одним и тем же типом DbContext с помощью Entity Framework Core
author: AndriySvyryd
ms.date: 01/03/2020
uid: core/modeling/dynamic-model
ms.openlocfilehash: 0e0af67eab5262ab2b26edadea470c753b6349a0
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90071528"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="8c238-103">Чередование между несколькими моделями с одинаковым типом DbContext</span><span class="sxs-lookup"><span data-stu-id="8c238-103">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="8c238-104">Модель, встроенная в, `OnModelCreating` может использовать свойство в контексте для изменения способа построения модели.</span><span class="sxs-lookup"><span data-stu-id="8c238-104">The model built in `OnModelCreating` can use a property on the context to change how the model is built.</span></span> <span data-ttu-id="8c238-105">Например, предположим, что нужно настроить сущность по-разному на основе какого-либо свойства:</span><span class="sxs-lookup"><span data-stu-id="8c238-105">For example, suppose you wanted to configure an entity differently based on some property:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnModelCreating)]

<span data-ttu-id="8c238-106">К сожалению, этот код не будет работать как есть, так как EF строит модель и выполняется `OnModelCreating` только один раз, что приводит к кэшированию результатов по соображениям производительности.</span><span class="sxs-lookup"><span data-stu-id="8c238-106">Unfortunately, this code wouldn't work as-is, since EF builds the model and runs `OnModelCreating` only once, caching the result for performance reasons.</span></span> <span data-ttu-id="8c238-107">Однако можно присоединиться к механизму кэширования модели, чтобы сделать EF осведомленным о свойстве, создающем различные модели.</span><span class="sxs-lookup"><span data-stu-id="8c238-107">However, you can hook into the model caching mechanism to make EF aware of the property producing different models.</span></span>

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="8c238-108">имоделкачекэйфактори</span><span class="sxs-lookup"><span data-stu-id="8c238-108">IModelCacheKeyFactory</span></span>

<span data-ttu-id="8c238-109">EF использует `IModelCacheKeyFactory` для создания ключей кэша для моделей; по умолчанию EF предполагает, что для любого заданного типа контекста модель будет одинаковой, поэтому реализация по умолчанию этой службы возвращает ключ, который содержит только тип контекста.</span><span class="sxs-lookup"><span data-stu-id="8c238-109">EF uses the `IModelCacheKeyFactory` to generate cache keys for models; by default, EF assumes that for any given context type the model will be the same, so the default implementation of this service returns a key that just contains the context type.</span></span> <span data-ttu-id="8c238-110">Чтобы создать различные модели из одного и того же типа контекста, необходимо заменить `IModelCacheKeyFactory` службу правильной реализацией. созданный ключ будет сравниваться с другими ключами модели с помощью `Equals` метода, принимая во внимание все переменные, влияющие на модель.</span><span class="sxs-lookup"><span data-stu-id="8c238-110">To produce different models from the same context type, you need to replace the `IModelCacheKeyFactory` service with the correct implementation; the generated key will be compared to other model keys using the `Equals` method, taking into account all the variables that affect the model.</span></span>

<span data-ttu-id="8c238-111">`UseIntProperty`При создании ключа кэша модели в следующей реализации учитывается учетная запись.</span><span class="sxs-lookup"><span data-stu-id="8c238-111">The following implementation takes the `UseIntProperty` into account when producing a model cache key:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicModelCacheKeyFactory.cs?name=DynamicModel)]

<span data-ttu-id="8c238-112">Наконец, зарегистрируйте новый `IModelCacheKeyFactory` в своем контексте `OnConfiguring` :</span><span class="sxs-lookup"><span data-stu-id="8c238-112">Finally, register your new `IModelCacheKeyFactory` in your context's `OnConfiguring`:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnConfiguring)]

<span data-ttu-id="8c238-113">См. [полный пример проекта](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) для получения дополнительных контекстов.</span><span class="sxs-lookup"><span data-stu-id="8c238-113">See the [full sample project](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) for more context.</span></span>
