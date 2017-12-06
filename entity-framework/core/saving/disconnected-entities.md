---
title: "Отключенные сущности - EF Core"
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: b9d9662ce277e4f7b3d6f997a5117a0592f59fa3
ms.sourcegitcommit: c72d85805db0aa95f980514a18381fdc5e17c786
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/01/2017
---
# <a name="disconnected-entities"></a><span data-ttu-id="109ef-102">Отключенные сущности</span><span class="sxs-lookup"><span data-stu-id="109ef-102">Disconnected entities</span></span>

<span data-ttu-id="109ef-103">Экземпляр DbContext будет автоматически отслеживать сущностей, возвращаемых из базы данных.</span><span class="sxs-lookup"><span data-stu-id="109ef-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="109ef-104">Изменения, внесенные в эти сущности будут обнаружены, затем при вызове SaveChanges и базы данных будут обновляться по мере необходимости.</span><span class="sxs-lookup"><span data-stu-id="109ef-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="109ef-105">В разделе [основные Сохранить](basic.md) и [связанные данные](related-data.md) подробные сведения.</span><span class="sxs-lookup"><span data-stu-id="109ef-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="109ef-106">Тем не менее иногда сущности, в запросах с помощью одного контекста экземпляра, а затем сохраняется с помощью другого экземпляра.</span><span class="sxs-lookup"><span data-stu-id="109ef-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="109ef-107">Это часто происходит в «отключенной» сценарии, такие как веб-приложения, где сущности запроса, отправляются на клиент, изменены, отправляются обратно на сервер в запросе и сохраняются.</span><span class="sxs-lookup"><span data-stu-id="109ef-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="109ef-108">В этом случае контекст второго экземпляра знает, являются ли новые сущности (должен быть установлен) или в существующие (должны быть обновлены).</span><span class="sxs-lookup"><span data-stu-id="109ef-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

> [!TIP]  
> <span data-ttu-id="109ef-109">Можно просмотреть в этой статье [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) на GitHub.</span><span class="sxs-lookup"><span data-stu-id="109ef-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) on GitHub.</span></span>

## <a name="identifying-new-entities"></a><span data-ttu-id="109ef-110">Определение новых сущностей</span><span class="sxs-lookup"><span data-stu-id="109ef-110">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="109ef-111">Клиент определяет новые сущности</span><span class="sxs-lookup"><span data-stu-id="109ef-111">Client identifies new entities</span></span>

<span data-ttu-id="109ef-112">При простом случае работать с клиента сообщает серверу, является ли сущность новой или существующей.</span><span class="sxs-lookup"><span data-stu-id="109ef-112">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="109ef-113">Например часто запрос для вставки новой сущности отличается от запрос на обновление существующей сущности.</span><span class="sxs-lookup"><span data-stu-id="109ef-113">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="109ef-114">Далее в этом разделе описываются варианты где необходимо определить, каким-либо образом вставки или обновления.</span><span class="sxs-lookup"><span data-stu-id="109ef-114">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="109ef-115">С помощью автоматически сформированных ключей</span><span class="sxs-lookup"><span data-stu-id="109ef-115">With auto-generated keys</span></span>

<span data-ttu-id="109ef-116">Значение автоматически созданного ключа часто может использоваться для определения, должна ли сущность вставлять или обновлять.</span><span class="sxs-lookup"><span data-stu-id="109ef-116">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="109ef-117">Если ключ не задан, (т. е. он по-прежнему имеет значение CLR по умолчанию null, нуль, т. д.), то сущности должен быть новый и требуется вставить.</span><span class="sxs-lookup"><span data-stu-id="109ef-117">If the key has not been set (i.e. it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="109ef-118">С другой стороны Если значение ключа затем должен быть уже сохранен ранее и теперь требуется обновить.</span><span class="sxs-lookup"><span data-stu-id="109ef-118">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="109ef-119">Другими словами Если ключ имеет значение, затем сущность запрос, отправляемый клиенту и теперь вернуться обновляться.</span><span class="sxs-lookup"><span data-stu-id="109ef-119">In other words, if the key has a value, then entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="109ef-120">Это легко проверить для ключа не задано, когда известен тип сущности:</span><span class="sxs-lookup"><span data-stu-id="109ef-120">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="109ef-121">Однако EF также имеет встроенный способ сделать это для любого типа сущности и типа ключа.</span><span class="sxs-lookup"><span data-stu-id="109ef-121">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="109ef-122">Ключи задаются как можно скорее сущностей отслеживаются контекстом, даже если сущность находится в состоянии добавления.</span><span class="sxs-lookup"><span data-stu-id="109ef-122">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="109ef-123">Это помогает при обходе графа сущностей и решить, что делать с каждой, например при использовании TrackGraph API.</span><span class="sxs-lookup"><span data-stu-id="109ef-123">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="109ef-124">Значение ключа можно использовать только как показано здесь _перед_ любой вызов для отслеживания сущности.</span><span class="sxs-lookup"><span data-stu-id="109ef-124">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="109ef-125">С другими ключами</span><span class="sxs-lookup"><span data-stu-id="109ef-125">With other keys</span></span>

<span data-ttu-id="109ef-126">Другой механизм необходим для идентификации новых сущностей, когда значения ключей не могут создаваться автоматически.</span><span class="sxs-lookup"><span data-stu-id="109ef-126">Some other mechanism is needed to identity new entities when key values are not generated automatically.</span></span> <span data-ttu-id="109ef-127">Существует два принципиальных подхода к этому:</span><span class="sxs-lookup"><span data-stu-id="109ef-127">There are two general approaches to this:</span></span>
 * <span data-ttu-id="109ef-128">Запрос для сущности</span><span class="sxs-lookup"><span data-stu-id="109ef-128">Query for the entity</span></span>
 * <span data-ttu-id="109ef-129">Передайте флаг от клиента</span><span class="sxs-lookup"><span data-stu-id="109ef-129">Pass a flag from the client</span></span>

<span data-ttu-id="109ef-130">Для выполнения запроса для сущности, просто используйте метод Find:</span><span class="sxs-lookup"><span data-stu-id="109ef-130">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="109ef-131">Он выходит за рамки настоящего документа показан полный код для передачи флаг от клиента.</span><span class="sxs-lookup"><span data-stu-id="109ef-131">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="109ef-132">В веб-приложения это обычно означает выполнение различных запросов для различных действий или передача какое-либо состояние, в запросе, а затем извлечение его в контроллере.</span><span class="sxs-lookup"><span data-stu-id="109ef-132">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="109ef-133">Сохранение одной сущности</span><span class="sxs-lookup"><span data-stu-id="109ef-133">Saving single entities</span></span>

<span data-ttu-id="109ef-134">Если известно ли необходима инструкция insert или update, а затем добавить или обновить используется соответствующим образом:</span><span class="sxs-lookup"><span data-stu-id="109ef-134">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="109ef-135">Тем не менее если автоматически сгенерированный ключевые значения используются сущности, метод Update можно использовать в обоих случаях:</span><span class="sxs-lookup"><span data-stu-id="109ef-135">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="109ef-136">Метод обновления обычно помечает сущности для обновления, но не вставки.</span><span class="sxs-lookup"><span data-stu-id="109ef-136">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="109ef-137">Тем не менее если сущность имеет автоматического создания ключа, а не значение ключа не задана, то сущность автоматически помечается для вставки.</span><span class="sxs-lookup"><span data-stu-id="109ef-137">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="109ef-138">Это поведение было введено в EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="109ef-138">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="109ef-139">Для более ранних выпусков всегда бывает необходимо явно выбрать добавить или обновить.</span><span class="sxs-lookup"><span data-stu-id="109ef-139">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="109ef-140">Если сущность не использует автоматически сформированные ключи, то приложения необходимо решить, следует ли вставлять или обновлять сущность: пример:</span><span class="sxs-lookup"><span data-stu-id="109ef-140">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="109ef-141">Здесь необходимо:</span><span class="sxs-lookup"><span data-stu-id="109ef-141">The steps here are:</span></span>
* <span data-ttu-id="109ef-142">Если добавить Find возвращает null, а затем базы данных еще не содержит блогов с этим Идентификатором, поэтому мы называем его нужно пометьте для вставки.</span><span class="sxs-lookup"><span data-stu-id="109ef-142">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="109ef-143">Если найти возвращает сущность, затем он существует в базе данных и контекст отслеживает существующей сущности</span><span class="sxs-lookup"><span data-stu-id="109ef-143">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="109ef-144">Затем мы используем SetValues задавать значения для всех свойств в этой сущности теми, которые поступили от клиента.</span><span class="sxs-lookup"><span data-stu-id="109ef-144">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="109ef-145">Вызов SetValues будут помечены как сущность, которая обновляется по мере необходимости.</span><span class="sxs-lookup"><span data-stu-id="109ef-145">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="109ef-146">SetValues только будут отмечены как измененные свойства, которые имеют разные значения в соответствующие Отслеживаемая сущность.</span><span class="sxs-lookup"><span data-stu-id="109ef-146">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="109ef-147">Это означает, что при отправке обновления будут обновлены только те столбцы, которые действительно были изменены.</span><span class="sxs-lookup"><span data-stu-id="109ef-147">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="109ef-148">(И если ничего не изменилось, то обновление не будут отправляться на всех).</span><span class="sxs-lookup"><span data-stu-id="109ef-148">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="109ef-149">Работа с диаграммами</span><span class="sxs-lookup"><span data-stu-id="109ef-149">Working with graphs</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="109ef-150">Все новые или все существующие сущности</span><span class="sxs-lookup"><span data-stu-id="109ef-150">All new/all existing entities</span></span>

<span data-ttu-id="109ef-151">Пример работы с диаграммами вставке или обновлении блога вместе с его коллекцию связанных записей.</span><span class="sxs-lookup"><span data-stu-id="109ef-151">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="109ef-152">Если нужно вставить все сущности в графе или все должны быть обновлены, процесс является таким же, как описано выше для одной сущности.</span><span class="sxs-lookup"><span data-stu-id="109ef-152">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="109ef-153">Например график блогов и сообщений, созданных следующим образом:</span><span class="sxs-lookup"><span data-stu-id="109ef-153">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="109ef-154">можно вставить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="109ef-154">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="109ef-155">Вызов Add отметит блогов и все сообщения для вставки.</span><span class="sxs-lookup"><span data-stu-id="109ef-155">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="109ef-156">Аналогично Если все сущности в виде графа, должны быть обновлены, то обновление может использоваться:</span><span class="sxs-lookup"><span data-stu-id="109ef-156">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="109ef-157">Блог и все сообщения будут помечены обновляться.</span><span class="sxs-lookup"><span data-stu-id="109ef-157">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="109ef-158">Сочетание новых и существующих сущностей</span><span class="sxs-lookup"><span data-stu-id="109ef-158">Mix of new and existing entities</span></span>

<span data-ttu-id="109ef-159">С автоматически формируемым ключам обновления снова используется для операций вставки и обновления, даже если диаграмма содержит набор сущностей, которые требуют вставки и те, которые требуется обновление:</span><span class="sxs-lookup"><span data-stu-id="109ef-159">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="109ef-160">Обновления будут отмечены все сущности в граф, блога или post для вставки, если не имеет набор значение ключа, пока все сущности, помечаются для обновления.</span><span class="sxs-lookup"><span data-stu-id="109ef-160">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="109ef-161">Как и прежде, если не используются автоматически сформированные ключи, запрос и некоторая обработка могут использоваться:</span><span class="sxs-lookup"><span data-stu-id="109ef-161">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="109ef-162">Обработка удалений</span><span class="sxs-lookup"><span data-stu-id="109ef-162">Handling deletes</span></span>

<span data-ttu-id="109ef-163">Удаление может быть непростой задачей, для обработки с момента часто отсутствия сущности означает, он должен быть удален.</span><span class="sxs-lookup"><span data-stu-id="109ef-163">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="109ef-164">Для решения этой проблемы можно использовать «мягкие удалений» таким образом, что объект помечен как удаленный вместо фактического удаления.</span><span class="sxs-lookup"><span data-stu-id="109ef-164">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="109ef-165">Удаляет, а затем становится таким же, как обновления.</span><span class="sxs-lookup"><span data-stu-id="109ef-165">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="109ef-166">Обратимых удалений можно реализовать с помощью [запрос фильтры](xref:core/querying/filters).</span><span class="sxs-lookup"><span data-stu-id="109ef-166">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="109ef-167">Значение true, удаления общий шаблон является использование расширение шаблона запроса для выполнения, возможности по существу graph сумм. Например:</span><span class="sxs-lookup"><span data-stu-id="109ef-167">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff. For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="109ef-168">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="109ef-168">TrackGraph</span></span>

<span data-ttu-id="109ef-169">Внутренне добавить, присоединить и Update использовать graph обхода с изменения, внесенные для каждой сущности, относительно ли он должен быть помечен как Added (вставляемый) Modified (для обновления), без изменений (ничего), или удаленных (для удаления).</span><span class="sxs-lookup"><span data-stu-id="109ef-169">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="109ef-170">Этот механизм выполняется через TrackGraph API.</span><span class="sxs-lookup"><span data-stu-id="109ef-170">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="109ef-171">Например предположим, что, когда клиент отправляет обратно графа сущностей его устанавливает некоторые флаг для каждой сущности, указывающее, как он обрабатывается.</span><span class="sxs-lookup"><span data-stu-id="109ef-171">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="109ef-172">TrackGraph затем может использоваться для обработки этого флага:</span><span class="sxs-lookup"><span data-stu-id="109ef-172">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="109ef-173">Флаги, отображаются только в составе сущности для простоты примера.</span><span class="sxs-lookup"><span data-stu-id="109ef-173">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="109ef-174">Обычно указывается флаги часть DTO или какое-либо состояние, включенных в запрос.</span><span class="sxs-lookup"><span data-stu-id="109ef-174">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
