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
# <a name="saving-related-data"></a><span data-ttu-id="586a0-102">Сохранение связанных данных</span><span class="sxs-lookup"><span data-stu-id="586a0-102">Saving Related Data</span></span>

<span data-ttu-id="586a0-103">В дополнение к изолированной сущности, вы также можете использовать связей, определенных в модели.</span><span class="sxs-lookup"><span data-stu-id="586a0-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="586a0-104">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="586a0-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="586a0-105">Добавление графика новых сущностей</span><span class="sxs-lookup"><span data-stu-id="586a0-105">Adding a graph of new entities</span></span>

<span data-ttu-id="586a0-106">При создании нескольких новых связанных сущностей, добавление одного из них в контекст вызовет другими, чтобы добавить слишком.</span><span class="sxs-lookup"><span data-stu-id="586a0-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="586a0-107">В следующем примере блогов и три связанных сообщений все вставляются в базу данных.</span><span class="sxs-lookup"><span data-stu-id="586a0-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="586a0-108">Обнаружены сообщения и добавлен, так как они доступны через `Blog.Posts` свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="586a0-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="586a0-109">Свойство EntityEntry.State используется для задания состояния одной сущности.</span><span class="sxs-lookup"><span data-stu-id="586a0-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="586a0-110">Например, `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="586a0-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="586a0-111">Добавление связанной сущности</span><span class="sxs-lookup"><span data-stu-id="586a0-111">Adding a related entity</span></span>

<span data-ttu-id="586a0-112">При ссылке из свойства навигации сущности, которая уже отслеживается контекстом объекта, сущности будут обнаружены и вставить в базу данных.</span><span class="sxs-lookup"><span data-stu-id="586a0-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="586a0-113">В следующем примере `post` вставке сущности, так как оно добавляется в `Posts` свойство `blog` сущности, для которой были извлечены из базы данных.</span><span class="sxs-lookup"><span data-stu-id="586a0-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="586a0-114">Изменение связей</span><span class="sxs-lookup"><span data-stu-id="586a0-114">Changing relationships</span></span>

<span data-ttu-id="586a0-115">При изменении свойства навигации сущности, произойдут соответствующие изменения в столбец внешнего ключа в базе данных.</span><span class="sxs-lookup"><span data-stu-id="586a0-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="586a0-116">В следующем примере `post` обновления сущности должен принадлежать к новому `blog` сущности из-за его `Blog` навигации свойству пункты `blog`.</span><span class="sxs-lookup"><span data-stu-id="586a0-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="586a0-117">Обратите внимание, что `blog` также быть вставляется в базе данных, так как это новая сущность, на которую ссылается свойство навигации сущности, которая уже отслеживается контекстом (`post`).</span><span class="sxs-lookup"><span data-stu-id="586a0-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="586a0-118">Удаление связей</span><span class="sxs-lookup"><span data-stu-id="586a0-118">Removing relationships</span></span>

<span data-ttu-id="586a0-119">Можно удалить связь, задав ссылку навигации `null`, или удаление связанной сущности из навигации коллекции.</span><span class="sxs-lookup"><span data-stu-id="586a0-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="586a0-120">Для удаления отношений имеют побочные эффекты на зависимой сущности, согласно cascade настроен в отношении поведение при удалении.</span><span class="sxs-lookup"><span data-stu-id="586a0-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="586a0-121">По умолчанию для необходимые связи настраивается поведение delete cascade и дочерние или зависимые сущности будут удалены из базы данных.</span><span class="sxs-lookup"><span data-stu-id="586a0-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="586a0-122">Каскадное удаление не настроен для связей, необязательный, по умолчанию, но он будет настроен свойство внешнего ключа в null.</span><span class="sxs-lookup"><span data-stu-id="586a0-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="586a0-123">В разделе [обязательных и необязательных связи](../modeling/relationships.md#required-and-optional-relationships) Дополнительные сведения о настройке requiredness связей.</span><span class="sxs-lookup"><span data-stu-id="586a0-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="586a0-124">В разделе [каскадного удаления](cascade-delete.md) Дополнительные сведения об инструкции каскадное удаление поведения можно использовать, как они могут быть настроены явным образом, и как они выбираются по соглашению.</span><span class="sxs-lookup"><span data-stu-id="586a0-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="586a0-125">В следующем примере каскадное удаление настроен на связи между `Blog` и `Post`, поэтому `post` сущность будет удалена из базы данных.</span><span class="sxs-lookup"><span data-stu-id="586a0-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
