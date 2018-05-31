---
title: Отключенные сущности — EF Core
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: 0b145217d40027c4b8e4746e9c5651652a28c9eb
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152420"
---
# <a name="disconnected-entities"></a><span data-ttu-id="7eaae-102">Отключенные сущности</span><span class="sxs-lookup"><span data-stu-id="7eaae-102">Disconnected entities</span></span>

<span data-ttu-id="7eaae-103">Экземпляр DbContext будет автоматически отслеживать сущности, возращенные из базы данных.</span><span class="sxs-lookup"><span data-stu-id="7eaae-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="7eaae-104">Изменения, внесенные в эти сущности, затем будут обнаружены при вызове SaveChanges, и база данных будет обновлена при необходимости.</span><span class="sxs-lookup"><span data-stu-id="7eaae-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="7eaae-105">Дополнительные сведения см. в статье о [базовом сохранении](basic.md) и [связанных данных](related-data.md).</span><span class="sxs-lookup"><span data-stu-id="7eaae-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="7eaae-106">Тем не менее иногда сущности запрашиваются с помощью одного экземпляра контекста, а затем сохраняются с помощью другого экземпляра.</span><span class="sxs-lookup"><span data-stu-id="7eaae-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="7eaae-107">Это часто происходит в автономных сценариях, например в веб-приложении, в котором сущности запрашиваются, отправляются клиенту, изменяются, возвращаются назад на сервер в запросе, а затем сохраняются.</span><span class="sxs-lookup"><span data-stu-id="7eaae-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="7eaae-108">В этом случае второй экземпляр контекста должен знать, что нужно сделать: добавить новые сущности или обновить имеющиеся.</span><span class="sxs-lookup"><span data-stu-id="7eaae-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

> [!TIP]  
> <span data-ttu-id="7eaae-109">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="7eaae-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) on GitHub.</span></span>

> [!TIP]
> <span data-ttu-id="7eaae-110">EF Core может отслеживать только один экземпляр любой сущности с использованием указанного значения первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="7eaae-110">EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="7eaae-111">Наилучший способ избежать этой проблемы — использовать кратковременный контекст для каждой единицы работы, чтобы контекст был пустым при запуске, имел связанные с ним сущности, сохранял эти сущности, а затем удалялся и отклонялся.</span><span class="sxs-lookup"><span data-stu-id="7eaae-111">The best way to avoid this being an issue is to use a short-lived context for each unit-of-work such that the context starts empty, has entities attached to it, saves those entities, and then the context is disposed and discarded.</span></span>

## <a name="identifying-new-entities"></a><span data-ttu-id="7eaae-112">Определение новых сущностей</span><span class="sxs-lookup"><span data-stu-id="7eaae-112">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="7eaae-113">Определение новых сущностей клиентом</span><span class="sxs-lookup"><span data-stu-id="7eaae-113">Client identifies new entities</span></span>

<span data-ttu-id="7eaae-114">Самый простой способ определения — это когда клиент сообщает серверу, является ли сущность новой или нет.</span><span class="sxs-lookup"><span data-stu-id="7eaae-114">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="7eaae-115">Например, часто запрос для вставки новой сущности отличается от запроса для обновления имеющейся сущности.</span><span class="sxs-lookup"><span data-stu-id="7eaae-115">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="7eaae-116">Далее в этом разделе описаны случаи, когда сущности необходимо определить способами, отличными от вставки или обновления.</span><span class="sxs-lookup"><span data-stu-id="7eaae-116">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="7eaae-117">Использование автоматически созданных ключей</span><span class="sxs-lookup"><span data-stu-id="7eaae-117">With auto-generated keys</span></span>

<span data-ttu-id="7eaae-118">Значение автоматически созданного ключа часто может использоваться, чтобы определить, что нужно сделать: вставить или обновить сущность.</span><span class="sxs-lookup"><span data-stu-id="7eaae-118">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="7eaae-119">Если ключ не задан (например, он по-прежнему имеет стандартное значение среды выполнения NULL, ноль и т.д.), то сущность должна быть новой и необходимо выполнить операцию вставки.</span><span class="sxs-lookup"><span data-stu-id="7eaae-119">If the key has not been set (i.e. it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="7eaae-120">С другой стороны, если значение ключа задано, то оно должно было быть сохранено ранее и теперь его нужно обновить.</span><span class="sxs-lookup"><span data-stu-id="7eaae-120">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="7eaae-121">Другими словами, если ключ имеет значение, то сущность была запрошена, отправлена клиенту и теперь вернулась для обновления.</span><span class="sxs-lookup"><span data-stu-id="7eaae-121">In other words, if the key has a value, then entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="7eaae-122">Очень просто проверить, задан ли ключ, если известен тип сущности:</span><span class="sxs-lookup"><span data-stu-id="7eaae-122">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="7eaae-123">Однако EF также имеет встроенный способ выполнить это действие для любого типа сущности и ключа:</span><span class="sxs-lookup"><span data-stu-id="7eaae-123">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="7eaae-124">Ключи задаются сразу же после того, как сущности начинают отслеживаться контекстом, даже если сущность находится в добавленном состоянии.</span><span class="sxs-lookup"><span data-stu-id="7eaae-124">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="7eaae-125">Это помогает при обходе графа сущностей. Кроме того, это позволяет решить, что нужно делать с каждым из них, например, при использовании API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="7eaae-125">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="7eaae-126">Значение ключа можно использовать только показанным здесь способом, _прежде чем_ будет выполнен вызов для отслеживания сущности.</span><span class="sxs-lookup"><span data-stu-id="7eaae-126">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="7eaae-127">Использование других ключей</span><span class="sxs-lookup"><span data-stu-id="7eaae-127">With other keys</span></span>

<span data-ttu-id="7eaae-128">Если значения ключей не создаются автоматически, необходимо использовать другой механизм для определения новых сущностей.</span><span class="sxs-lookup"><span data-stu-id="7eaae-128">Some other mechanism is needed to identify new entities when key values are not generated automatically.</span></span> <span data-ttu-id="7eaae-129">Для этого есть два общих подхода:</span><span class="sxs-lookup"><span data-stu-id="7eaae-129">There are two general approaches to this:</span></span>
 * <span data-ttu-id="7eaae-130">запросить сущность;</span><span class="sxs-lookup"><span data-stu-id="7eaae-130">Query for the entity</span></span>
 * <span data-ttu-id="7eaae-131">передать флаг от клиента.</span><span class="sxs-lookup"><span data-stu-id="7eaae-131">Pass a flag from the client</span></span>

<span data-ttu-id="7eaae-132">Запросить сущность можно с помощью метода Find:</span><span class="sxs-lookup"><span data-stu-id="7eaae-132">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="7eaae-133">Мы не может показать в этом документе полный код для передачи флага от клиента.</span><span class="sxs-lookup"><span data-stu-id="7eaae-133">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="7eaae-134">В веб-приложении это часто означает выполнение разных запросов для разных действий или передачу некоторого состояния в запросе, а затем извлечение его в контроллере.</span><span class="sxs-lookup"><span data-stu-id="7eaae-134">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="7eaae-135">Сохранение одной сущности</span><span class="sxs-lookup"><span data-stu-id="7eaae-135">Saving single entities</span></span>

<span data-ttu-id="7eaae-136">Если неизвестно, следует выполнять вставку или обновление, то можно должным образом использовать метод Add или Update:</span><span class="sxs-lookup"><span data-stu-id="7eaae-136">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="7eaae-137">Тем не менее, если в сущности используются автоматически созданные значения ключей, в обоих случаях можно использовать метод Update:</span><span class="sxs-lookup"><span data-stu-id="7eaae-137">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="7eaae-138">Метод Update обычно помечает сущность для обновления, не для вставки.</span><span class="sxs-lookup"><span data-stu-id="7eaae-138">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="7eaae-139">Тем не менее, если сущность имеет автоматически созданный ключ и значение ключа не было задано, то вместо этого сущность автоматически помечается для вставки.</span><span class="sxs-lookup"><span data-stu-id="7eaae-139">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="7eaae-140">Это поведение появилось в EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="7eaae-140">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="7eaae-141">Для более ранних выпусков всегда необходимо явно выбрать метод Add или Update.</span><span class="sxs-lookup"><span data-stu-id="7eaae-141">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="7eaae-142">Если в сущности не используются автоматически созданные ключи, то приложение должно решить, следует ли вставить или обновить сущность. Например:</span><span class="sxs-lookup"><span data-stu-id="7eaae-142">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="7eaae-143">Ниже приведены нужные действия.</span><span class="sxs-lookup"><span data-stu-id="7eaae-143">The steps here are:</span></span>
* <span data-ttu-id="7eaae-144">Если метод Find возвращает значение NULL, то база данных не содержит блогов с этим идентификатором. Поэтому мы вызываем метод Add, чтобы пометить сущность для вставки.</span><span class="sxs-lookup"><span data-stu-id="7eaae-144">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="7eaae-145">Если метод Find возвращает сущность, то она имеется в базе данных и контекст теперь отслеживает имеющуюся сущность.</span><span class="sxs-lookup"><span data-stu-id="7eaae-145">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="7eaae-146">Затем мы используем метод SetValues, чтобы задать для всех свойств этой сущности значения, которые переданы от клиента.</span><span class="sxs-lookup"><span data-stu-id="7eaae-146">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="7eaae-147">При вызове метода SetValues будет помечена сущность, которая будет обновлена при необходимости.</span><span class="sxs-lookup"><span data-stu-id="7eaae-147">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="7eaae-148">SetValues будет помечать сущности только при изменении свойств, значения которых отличаются от значений в отслеживаемой сущности.</span><span class="sxs-lookup"><span data-stu-id="7eaae-148">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="7eaae-149">Это означает, что при отправке операции обновления, будут обновлены только столбцы, которые изменились.</span><span class="sxs-lookup"><span data-stu-id="7eaae-149">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="7eaae-150">(Если ничего не изменилось, операция обновления не будет отправлена.)</span><span class="sxs-lookup"><span data-stu-id="7eaae-150">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="7eaae-151">Работа с графами</span><span class="sxs-lookup"><span data-stu-id="7eaae-151">Working with graphs</span></span>

### <a name="identity-resolution"></a><span data-ttu-id="7eaae-152">Разрешение идентификатора</span><span class="sxs-lookup"><span data-stu-id="7eaae-152">Identity resolution</span></span>

<span data-ttu-id="7eaae-153">Как отмечалось выше, EF Core может отслеживать только один экземпляр любой сущности с использованием указанного значения первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="7eaae-153">As noted above, EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="7eaae-154">При работе с графами граф должен в идеале создаваться таким способом, чтобы этот инвариант поддерживался. Контекст должен использоваться только для одной единицы работы.</span><span class="sxs-lookup"><span data-stu-id="7eaae-154">When working with graphs the graph should ideally be created such that this invariant is maintained, and the context should be used for only one unit-of-work.</span></span> <span data-ttu-id="7eaae-155">Если граф не содержит дубликатов, понадобится обработать его, прежде чем отправить в EF для объединения нескольких экземпляров в один.</span><span class="sxs-lookup"><span data-stu-id="7eaae-155">If the graph does contain duplicates, then it will be necessary to process the graph before sending it to EF to consolidate multiple instances into one.</span></span> <span data-ttu-id="7eaae-156">Это может быть сложной задачей, если экземпляры имеют конфликтующие значения и связи. Поэтому следует как можно быстрее объединить дубликаты в конвейере приложения, чтобы избежать разрешения конфликтов.</span><span class="sxs-lookup"><span data-stu-id="7eaae-156">This may not be trivial where instances have conflicting values and relationships, so consolidating duplicates should be done as soon as possible in your application pipeline to avoid conflict resolution.</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="7eaae-157">Все новые или имеющиеся сущности</span><span class="sxs-lookup"><span data-stu-id="7eaae-157">All new/all existing entities</span></span>

<span data-ttu-id="7eaae-158">Пример работы с графами — обновление или вставка блога вместе с его коллекцией связанных записей.</span><span class="sxs-lookup"><span data-stu-id="7eaae-158">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="7eaae-159">Если необходимо вставить или обновить все сущности в графе, этот процесс аналогичен описанному выше процессу для одной сущности.</span><span class="sxs-lookup"><span data-stu-id="7eaae-159">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="7eaae-160">Например, граф блогов и записей, созданных таким образом</span><span class="sxs-lookup"><span data-stu-id="7eaae-160">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="7eaae-161">можно вставить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="7eaae-161">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="7eaae-162">Вызов метода Add отметит блог и все записи, которые будут вставлены.</span><span class="sxs-lookup"><span data-stu-id="7eaae-162">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="7eaae-163">Аналогично, если все сущности в графе необходимо обновить, можно использовать метод Update:</span><span class="sxs-lookup"><span data-stu-id="7eaae-163">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="7eaae-164">Блог и все записи будут помечены для обновления.</span><span class="sxs-lookup"><span data-stu-id="7eaae-164">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="7eaae-165">Сочетание новых и имеющихся сущностей</span><span class="sxs-lookup"><span data-stu-id="7eaae-165">Mix of new and existing entities</span></span>

<span data-ttu-id="7eaae-166">Если у вас есть автоматически созданные ключи, можно снова использовать метод Update для операций вставки и обновления, даже если граф содержит сочетание сущностей, для которых требуется выполнить вставку и обновление:</span><span class="sxs-lookup"><span data-stu-id="7eaae-166">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="7eaae-167">Метод Update отметит все сущности в графе, блоге или записи для вставки, если для него не задано значения ключа. Все другие записи будут помечены для обновления.</span><span class="sxs-lookup"><span data-stu-id="7eaae-167">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="7eaae-168">Как и ранее, если не используются автоматически созданные ключи, можно выполнить запрос и некоторую обработку:</span><span class="sxs-lookup"><span data-stu-id="7eaae-168">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="7eaae-169">Обработка удалений</span><span class="sxs-lookup"><span data-stu-id="7eaae-169">Handling deletes</span></span>

<span data-ttu-id="7eaae-170">Выполнение удаления может оказаться сложной задачей, так как очень часто отсутствие сущности означает, что ее необходимо удалить.</span><span class="sxs-lookup"><span data-stu-id="7eaae-170">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="7eaae-171">Для решения этой проблемы можно выполнить обратимые удаления. Например, пометить сущность как удаленную, а не удалять ее.</span><span class="sxs-lookup"><span data-stu-id="7eaae-171">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="7eaae-172">Таким образом удаление идентично обновлению.</span><span class="sxs-lookup"><span data-stu-id="7eaae-172">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="7eaae-173">Обратимые удаления можно реализовать с помощью [фильтров запросов](xref:core/querying/filters).</span><span class="sxs-lookup"><span data-stu-id="7eaae-173">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="7eaae-174">Для необратимых удалений обычно используется расширение шаблона запроса, чтобы выполнить действия, которые фактически изменяют граф. Пример:</span><span class="sxs-lookup"><span data-stu-id="7eaae-174">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff. For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="7eaae-175">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="7eaae-175">TrackGraph</span></span>

<span data-ttu-id="7eaae-176">В методах Internally, Add, Attach и Update используется обход графа с определением, выполненным для каждой сущности, независимо от того, следует ли ее пометить как Added (для вставки), Modified (для обновления), Unchanged (не выполнять никаких действий) или Deleted (для удаления).</span><span class="sxs-lookup"><span data-stu-id="7eaae-176">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="7eaae-177">Этот механизм предоставляется через API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="7eaae-177">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="7eaae-178">Например, предположим, что когда клиент отправляет обратно граф сущностей, он устанавливает некоторые флаги на каждой сущности, указывая, как ее следует обрабатывать.</span><span class="sxs-lookup"><span data-stu-id="7eaae-178">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="7eaae-179">TrackGraph затем может использоваться для обработки этого флага:</span><span class="sxs-lookup"><span data-stu-id="7eaae-179">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="7eaae-180">Флаги отображаются только в составе сущности для простоты примера.</span><span class="sxs-lookup"><span data-stu-id="7eaae-180">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="7eaae-181">Обычно флаги входят в состав объекта передачи данных или других состояний, включенных в запросе.</span><span class="sxs-lookup"><span data-stu-id="7eaae-181">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
