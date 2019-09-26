---
title: Сохранение связанных данных — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
uid: core/saving/related-data
ms.openlocfilehash: 45c7b8e4bfa4ce7967ad76ef4a7d4818b0d3aebf
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197889"
---
# <a name="saving-related-data"></a><span data-ttu-id="a3452-102">Сохранение связанных данных</span><span class="sxs-lookup"><span data-stu-id="a3452-102">Saving Related Data</span></span>

<span data-ttu-id="a3452-103">В дополнение к изолированным сущностям вы также можете использовать связи, определенные в вашей модели.</span><span class="sxs-lookup"><span data-stu-id="a3452-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="a3452-104">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="a3452-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="a3452-105">Добавление графа новых сущностей</span><span class="sxs-lookup"><span data-stu-id="a3452-105">Adding a graph of new entities</span></span>

<span data-ttu-id="a3452-106">Если вы создадите несколько новых связанных сущностей, добавление одной из них в контекст приведет к добавлению других.</span><span class="sxs-lookup"><span data-stu-id="a3452-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="a3452-107">В следующем примере блог и три связанные записи вставляются в базу данных.</span><span class="sxs-lookup"><span data-stu-id="a3452-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="a3452-108">Записи обнаруживаются и добавляются, потому что они доступны через свойство навигации `Blog.Posts`.</span><span class="sxs-lookup"><span data-stu-id="a3452-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="a3452-109">Используйте свойство EntityEntry.State, чтобы установить состояние только одной сущности.</span><span class="sxs-lookup"><span data-stu-id="a3452-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="a3452-110">Например, `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="a3452-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="a3452-111">Добавление связанной сущности</span><span class="sxs-lookup"><span data-stu-id="a3452-111">Adding a related entity</span></span>

<span data-ttu-id="a3452-112">Если вы ссылаетесь на новую сущность из свойства навигации сущности, которая уже отслеживается контекстом, сущность будет обнаружена и вставлена в базу данных.</span><span class="sxs-lookup"><span data-stu-id="a3452-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="a3452-113">В следующем примере вставляется сущность `post`, так как она добавлена в свойство `Posts` сущности `blog`, которая была получена из базы данных.</span><span class="sxs-lookup"><span data-stu-id="a3452-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="a3452-114">Изменение связей</span><span class="sxs-lookup"><span data-stu-id="a3452-114">Changing relationships</span></span>

<span data-ttu-id="a3452-115">Если вы измените свойство навигации для сущности, соответствующие изменения будут внесены в столбец внешнего ключа в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a3452-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="a3452-116">В следующем примере сущность `post` обновляется таким образом, чтобы принадлежать к новой сущности `blog`, потому что ее свойство навигации `Blog` указывает на `blog`.</span><span class="sxs-lookup"><span data-stu-id="a3452-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="a3452-117">Обратите внимание, что `blog` также будет вставлена в базу данных, так как это новая сущность, на которую ссылается свойство навигации сущности, которая уже отслеживается контекстом (`post`).</span><span class="sxs-lookup"><span data-stu-id="a3452-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="a3452-118">Удаление связей</span><span class="sxs-lookup"><span data-stu-id="a3452-118">Removing relationships</span></span>

<span data-ttu-id="a3452-119">Вы можете удалить связи, установив для свойства навигации по ссылке значение `null` или удалив связанную сущность из навигации коллекции.</span><span class="sxs-lookup"><span data-stu-id="a3452-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="a3452-120">Удаление связи может иметь побочные эффекты для зависимой сущности в соответствии с поведением каскадного удаления, настроенным в связи.</span><span class="sxs-lookup"><span data-stu-id="a3452-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="a3452-121">По умолчанию для обязательных связей настроено поведение каскадного удаления, и дочерняя или зависимая сущность будет удалена из базы данных.</span><span class="sxs-lookup"><span data-stu-id="a3452-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="a3452-122">Для необязательных связей каскадное удаление по умолчанию не настроено, но для свойства внешнего ключа будет установлено значение null.</span><span class="sxs-lookup"><span data-stu-id="a3452-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="a3452-123">Дополнительные сведения о настройке обязательных и необязательных связей см. в [этом разделе](../modeling/relationships.md#required-and-optional-relationships).</span><span class="sxs-lookup"><span data-stu-id="a3452-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="a3452-124">Дополнительные сведения о том, как работают поведения каскадного удаления, как они могут быть настроены явно и как они выбираются по соглашению, см. в [этой статье](cascade-delete.md).</span><span class="sxs-lookup"><span data-stu-id="a3452-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="a3452-125">В следующем примере каскадное удаление настраивается в связи между `Blog` ​​и `Post`, поэтому сущность `post` удаляется из базы данных.</span><span class="sxs-lookup"><span data-stu-id="a3452-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#RemovingRelationships)]
