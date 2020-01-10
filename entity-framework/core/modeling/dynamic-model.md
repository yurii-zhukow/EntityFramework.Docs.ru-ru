---
title: Чередование между несколькими моделями с одинаковым типом DbContext — EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 156d5666cbd9352b274ddc70c99704ca62aeb1fd
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781135"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Чередование между несколькими моделями с одинаковым типом DbContext

Модель, встроенная в `OnModelCreating`, может использовать свойство в контексте для изменения способа построения модели. Например, предположим, что нужно настроить сущность по-разному на основе какого-либо свойства:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnModelCreating)]

К сожалению, этот код не будет работать, как есть, поскольку EF строит модель и выполняется `OnModelCreating` только один раз, кэширование результатов по соображениям производительности. Однако можно присоединиться к механизму кэширования модели, чтобы сделать EF осведомленным о свойстве, создающем различные модели.

## <a name="imodelcachekeyfactory"></a>имоделкачекэйфактори

EF использует `IModelCacheKeyFactory` для создания ключей кэша для моделей; по умолчанию EF предполагает, что для любого заданного типа контекста модель будет одинаковой, поэтому реализация по умолчанию этой службы возвращает ключ, который содержит только тип контекста. Чтобы создать различные модели из одного и того же типа контекста, необходимо заменить `IModelCacheKeyFactory`ную службу правильной реализацией. созданный ключ будет сравниваться с другими ключами модели с помощью метода `Equals`, принимая во внимание все переменные, влияющие на модель:

Следующая реализация принимает `IgnoreIntProperty` в учетную запись при создании ключа кэша модели:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicModelCacheKeyFactory.cs?name=DynamicModel)]

Наконец, зарегистрируйте новый `IModelCacheKeyFactory` в `OnConfiguring`е контекста:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnConfiguring)]

См. [полный пример проекта](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) для получения дополнительных контекстов.
