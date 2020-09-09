---
title: Чередование между несколькими моделями с одинаковым типом DbContext — EF Core
description: Чередование между несколькими моделями с одним и тем же типом DbContext с помощью Entity Framework Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 436cb46e002438f391c654b64efdfacf494d580d
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89617518"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Чередование между несколькими моделями с одинаковым типом DbContext

Модель, встроенная в, `OnModelCreating` может использовать свойство в контексте для изменения способа построения модели. Например, предположим, что нужно настроить сущность по-разному на основе какого-либо свойства:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnModelCreating)]

К сожалению, этот код не будет работать как есть, так как EF строит модель и выполняется `OnModelCreating` только один раз, что приводит к кэшированию результатов по соображениям производительности. Однако можно присоединиться к механизму кэширования модели, чтобы сделать EF осведомленным о свойстве, создающем различные модели.

## <a name="imodelcachekeyfactory"></a>имоделкачекэйфактори

EF использует `IModelCacheKeyFactory` для создания ключей кэша для моделей; по умолчанию EF предполагает, что для любого заданного типа контекста модель будет одинаковой, поэтому реализация по умолчанию этой службы возвращает ключ, который содержит только тип контекста. Чтобы создать различные модели из одного и того же типа контекста, необходимо заменить `IModelCacheKeyFactory` службу правильной реализацией. созданный ключ будет сравниваться с другими ключами модели с помощью `Equals` метода, принимая во внимание все переменные, влияющие на модель.

`UseIntProperty`При создании ключа кэша модели в следующей реализации учитывается учетная запись.

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicModelCacheKeyFactory.cs?name=DynamicModel)]

Наконец, зарегистрируйте новый `IModelCacheKeyFactory` в своем контексте `OnConfiguring` :

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnConfiguring)]

См. [полный пример проекта](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) для получения дополнительных контекстов.
