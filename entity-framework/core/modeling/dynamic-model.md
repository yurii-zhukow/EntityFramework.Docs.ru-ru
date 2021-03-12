---
title: Чередование между несколькими моделями с одинаковым типом DbContext — EF Core
description: Чередование между несколькими моделями с одним и тем же типом DbContext с помощью Entity Framework Core
author: AndriySvyryd
ms.date: 01/03/2020
uid: core/modeling/dynamic-model
ms.openlocfilehash: 0c418000b85c508569b8146af63bff205ae6d222
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103024034"
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

См. [полный пример проекта](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Modeling/DynamicModel) для получения дополнительных контекстов.
