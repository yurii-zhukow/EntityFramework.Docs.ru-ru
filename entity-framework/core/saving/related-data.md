---
title: Сохранение связанных данных - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
ms.technology: entity-framework-core
uid: core/saving/related-data
ms.openlocfilehash: b0ed25267c85e82db18d8a89693b6040db7e4b34
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2018
---
# <a name="saving-related-data"></a>Сохранение связанных данных

В дополнение к изолированной сущности, вы также можете использовать связей, определенных в модели.

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) из репозитория GitHub.

## <a name="adding-a-graph-of-new-entities"></a>Добавление графика новых сущностей

При создании нескольких новых связанных сущностей, добавление одного из них в контекст вызовет другими, чтобы добавить слишком.

В следующем примере блогов и три связанных сообщений все вставляются в базу данных. Обнаружены сообщения и добавлен, так как они доступны через `Blog.Posts` свойство навигации.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> Свойство EntityEntry.State используется для задания состояния одной сущности. Например, `context.Entry(blog).State = EntityState.Modified`.

## <a name="adding-a-related-entity"></a>Добавление связанной сущности

При ссылке из свойства навигации сущности, которая уже отслеживается контекстом объекта, сущности будут обнаружены и вставить в базу данных.

В следующем примере `post` вставке сущности, так как оно добавляется в `Posts` свойство `blog` сущности, для которой были извлечены из базы данных.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>Изменение связей

При изменении свойства навигации сущности, произойдут соответствующие изменения в столбец внешнего ключа в базе данных.

В следующем примере `post` обновления сущности должен принадлежать к новому `blog` сущности из-за его `Blog` навигации свойству пункты `blog`. Обратите внимание, что `blog` также быть вставляется в базе данных, так как это новая сущность, на которую ссылается свойство навигации сущности, которая уже отслеживается контекстом (`post`).

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>Удаление связей

Можно удалить связь, задав ссылку навигации `null`, или удаление связанной сущности из навигации коллекции.

Для удаления отношений имеют побочные эффекты на зависимой сущности, согласно cascade настроен в отношении поведение при удалении.

По умолчанию для необходимые связи настраивается поведение delete cascade и дочерние или зависимые сущности будут удалены из базы данных. Каскадное удаление не настроен для связей, необязательный, по умолчанию, но он будет настроен свойство внешнего ключа в null.

В разделе [обязательных и необязательных связи](../modeling/relationships.md#required-and-optional-relationships) Дополнительные сведения о настройке requiredness связей.

В разделе [каскадного удаления](cascade-delete.md) Дополнительные сведения об инструкции каскадное удаление поведения можно использовать, как они могут быть настроены явным образом, и как они выбираются по соглашению.

В следующем примере каскадное удаление настроен на связи между `Blog` и `Post`, поэтому `post` сущность будет удалена из базы данных.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
