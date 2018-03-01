---
title: "Каскадное удаление - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: 1ab9d114e27aac0bec972df631a426c8ce87a518
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="cascade-delete"></a><span data-ttu-id="fccec-102">Каскадное удаление</span><span class="sxs-lookup"><span data-stu-id="fccec-102">Cascade Delete</span></span>

<span data-ttu-id="fccec-103">Каскадное удаление обычно используется для описания характеристик, которое допускает удаление строки автоматически активировать к удалению связанных строк в терминологии связанных баз данных.</span><span class="sxs-lookup"><span data-stu-id="fccec-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="fccec-104">Тесно связанных понятие, также охвачен поведения удаления EF Core заключается в автоматическое удаление дочерней сущности, если это отношение к родительскому элементу было разорвано — это i часто называется «удаление потерянных».</span><span class="sxs-lookup"><span data-stu-id="fccec-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this i commonly known as "deleting orphans".</span></span>

<span data-ttu-id="fccec-105">EF Core реализует несколько разных delete поведения и позволяет настраивать поведение удаления отдельных связей.</span><span class="sxs-lookup"><span data-stu-id="fccec-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="fccec-106">EF Core также реализует соглашения, которые автоматически настраивают поведения удаления полезные значения по умолчанию для каждой связи в зависимости от [requiredness связи] (../modeling/relationships.md#required-and-optional-relationships).</span><span class="sxs-lookup"><span data-stu-id="fccec-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship] (../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="fccec-107">Удаление поведения</span><span class="sxs-lookup"><span data-stu-id="fccec-107">Delete behaviors</span></span>
<span data-ttu-id="fccec-108">Удалить поведения определяются в *DeleteBehavior* перечислителя типа и может быть передан *OnDelete* fluent API для управления ли Удаление участника или родительской сущности или прерывание из связь с зависимостью/дочерних сущностей должна иметь побочный эффект для зависимой/дочерних сущностей.</span><span class="sxs-lookup"><span data-stu-id="fccec-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="fccec-109">Существует три действия может занять EF при удалении участника или родительской сущности или разрыва связи в дочернюю.</span><span class="sxs-lookup"><span data-stu-id="fccec-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>
* <span data-ttu-id="fccec-110">Можно удалить дочерний/зависимых</span><span class="sxs-lookup"><span data-stu-id="fccec-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="fccec-111">Значение дочернего элемента значения внешнего ключа можно задать значение null</span><span class="sxs-lookup"><span data-stu-id="fccec-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="fccec-112">Дочерние остается без изменений</span><span class="sxs-lookup"><span data-stu-id="fccec-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="fccec-113">Поведение удаления, настроенный в базовой EF модели является применяются только в том случае, если основной сущности удаляется с помощью основных EF и зависимые сущности загружаются в память (т. е. для записанные зависимые элементы).</span><span class="sxs-lookup"><span data-stu-id="fccec-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (i.e. for tracked dependents).</span></span> <span data-ttu-id="fccec-114">У программы установки в базе данных, чтобы данные, не отслеживается контекстом необходимые действие, применяемое необходимо соответствующим поведением каскадное.</span><span class="sxs-lookup"><span data-stu-id="fccec-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="fccec-115">EF Core используется для создания базы данных, это поведение cascade будет ли программа установки автоматически.</span><span class="sxs-lookup"><span data-stu-id="fccec-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="fccec-116">Для второго действия выше установив значение внешнего ключа значение null является недопустимым, если внешний ключ не допускает значения NULL.</span><span class="sxs-lookup"><span data-stu-id="fccec-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="fccec-117">(Не допускающим внешний ключ соответствует необходимая связь.) В этих случаях EF Core отслеживает, что свойство внешнего ключа был помечен как null до вызова метода SaveChanges, после чего создается исключение из-за изменения не могут быть сохранены в базе данных.</span><span class="sxs-lookup"><span data-stu-id="fccec-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="fccec-118">Это похоже на получение нарушение ограничения из базы данных.</span><span class="sxs-lookup"><span data-stu-id="fccec-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="fccec-119">Существует четыре удалить поведения, перечисленные в приведенной ниже таблице.</span><span class="sxs-lookup"><span data-stu-id="fccec-119">There are four delete behaviors, as listed in the tables below.</span></span> <span data-ttu-id="fccec-120">Для связей, необязательные (допускающие значение NULL внешний ключ) его _—_ можно сохранить значение null, значение внешнего ключа, что приводит к следующему:</span><span class="sxs-lookup"><span data-stu-id="fccec-120">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="fccec-121">Имя поведения</span><span class="sxs-lookup"><span data-stu-id="fccec-121">Behavior Name</span></span>               | <span data-ttu-id="fccec-122">Влияет на зависимые потомки в памяти</span><span class="sxs-lookup"><span data-stu-id="fccec-122">Effect on dependent/child in memory</span></span>    | <span data-ttu-id="fccec-123">Влияет на зависимые потомки в базе данных</span><span class="sxs-lookup"><span data-stu-id="fccec-123">Effect on dependent/child in database</span></span>  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| <span data-ttu-id="fccec-124">**Cascade**</span><span class="sxs-lookup"><span data-stu-id="fccec-124">**Cascade**</span></span>                 | <span data-ttu-id="fccec-125">Сущности будут удалены.</span><span class="sxs-lookup"><span data-stu-id="fccec-125">Entities are deleted</span></span>                   | <span data-ttu-id="fccec-126">Сущности будут удалены.</span><span class="sxs-lookup"><span data-stu-id="fccec-126">Entities are deleted</span></span>                   |
| <span data-ttu-id="fccec-127">**ClientSetNull** (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="fccec-127">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="fccec-128">Свойства внешнего ключа устанавливаются в значение null</span><span class="sxs-lookup"><span data-stu-id="fccec-128">Foreign key properties are set to null</span></span> | <span data-ttu-id="fccec-129">Нет</span><span class="sxs-lookup"><span data-stu-id="fccec-129">None</span></span>                                   |
| <span data-ttu-id="fccec-130">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="fccec-130">**SetNull**</span></span>                 | <span data-ttu-id="fccec-131">Свойства внешнего ключа устанавливаются в значение null</span><span class="sxs-lookup"><span data-stu-id="fccec-131">Foreign key properties are set to null</span></span> | <span data-ttu-id="fccec-132">Свойства внешнего ключа устанавливаются в значение null</span><span class="sxs-lookup"><span data-stu-id="fccec-132">Foreign key properties are set to null</span></span> |
| <span data-ttu-id="fccec-133">**Restrict**</span><span class="sxs-lookup"><span data-stu-id="fccec-133">**Restrict**</span></span>                | <span data-ttu-id="fccec-134">Нет</span><span class="sxs-lookup"><span data-stu-id="fccec-134">None</span></span>                                   | <span data-ttu-id="fccec-135">Нет</span><span class="sxs-lookup"><span data-stu-id="fccec-135">None</span></span>                                   |

<span data-ttu-id="fccec-136">Необходимые связи (допускающий внешний ключ) — _не_ можно сохранить значение null, значение внешнего ключа, что приводит к следующему:</span><span class="sxs-lookup"><span data-stu-id="fccec-136">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="fccec-137">Имя поведения</span><span class="sxs-lookup"><span data-stu-id="fccec-137">Behavior Name</span></span>         | <span data-ttu-id="fccec-138">Влияет на зависимые потомки в памяти</span><span class="sxs-lookup"><span data-stu-id="fccec-138">Effect on dependent/child in memory</span></span> | <span data-ttu-id="fccec-139">Влияет на зависимые потомки в базе данных</span><span class="sxs-lookup"><span data-stu-id="fccec-139">Effect on dependent/child in database</span></span> |
|:----------------------|:------------------------------------|:--------------------------------------|
| <span data-ttu-id="fccec-140">**CASCADE** (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="fccec-140">**Cascade** (Default)</span></span> | <span data-ttu-id="fccec-141">Сущности будут удалены.</span><span class="sxs-lookup"><span data-stu-id="fccec-141">Entities are deleted</span></span>                | <span data-ttu-id="fccec-142">Сущности будут удалены.</span><span class="sxs-lookup"><span data-stu-id="fccec-142">Entities are deleted</span></span>                  |
| <span data-ttu-id="fccec-143">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="fccec-143">**ClientSetNull**</span></span>     | <span data-ttu-id="fccec-144">Вызывает метод SaveChanges</span><span class="sxs-lookup"><span data-stu-id="fccec-144">SaveChanges throws</span></span>                  | <span data-ttu-id="fccec-145">Нет</span><span class="sxs-lookup"><span data-stu-id="fccec-145">None</span></span>                                  |
| <span data-ttu-id="fccec-146">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="fccec-146">**SetNull**</span></span>           | <span data-ttu-id="fccec-147">Вызывает метод SaveChanges</span><span class="sxs-lookup"><span data-stu-id="fccec-147">SaveChanges throws</span></span>                  | <span data-ttu-id="fccec-148">Вызывает метод SaveChanges</span><span class="sxs-lookup"><span data-stu-id="fccec-148">SaveChanges throws</span></span>                    |
| <span data-ttu-id="fccec-149">**Restrict**</span><span class="sxs-lookup"><span data-stu-id="fccec-149">**Restrict**</span></span>          | <span data-ttu-id="fccec-150">Нет</span><span class="sxs-lookup"><span data-stu-id="fccec-150">None</span></span>                                | <span data-ttu-id="fccec-151">Нет</span><span class="sxs-lookup"><span data-stu-id="fccec-151">None</span></span>                                  |

<span data-ttu-id="fccec-152">В таблицах выше *нет* может привести к нарушению ограничения.</span><span class="sxs-lookup"><span data-stu-id="fccec-152">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="fccec-153">Например если основной и дочерней сущностью удаляется, но никакие действия не выполняются, чтобы изменить внешний ключ зависимые потомки, затем базу данных скорее всего вызовет на SaveChanges из-за нарушения ограничения внешнего.</span><span class="sxs-lookup"><span data-stu-id="fccec-153">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="fccec-154">На высоком уровне:</span><span class="sxs-lookup"><span data-stu-id="fccec-154">At a high level:</span></span>
* <span data-ttu-id="fccec-155">Если у вас есть сущностей, которые не могут существовать без родительских и вы EF для обеспечения автоматического удаления дочерних элементов, а затем использовать *Cascade*.</span><span class="sxs-lookup"><span data-stu-id="fccec-155">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="fccec-156">Сущности, которые не могут существовать без родительской обычно сделать использование необходимые связи, для которого *Cascade* значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fccec-156">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="fccec-157">Если сущности, которые не могут иметь родительский элемент, и вы EF, отвечающий за обнуление внешнего ключа для вас, а затем использовать *ClientSetNull*</span><span class="sxs-lookup"><span data-stu-id="fccec-157">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="fccec-158">Сущности, которые могут существовать без родительской обычно сделать использовать необязательный связей, для которого *ClientSetNull* значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fccec-158">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="fccec-159">Если требуется, чтобы базы данных также попытаться распространение значения null во внешних ключах дочерних даже при дочерней сущности не загружается, затем с помощью *SetNull*.</span><span class="sxs-lookup"><span data-stu-id="fccec-159">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="fccec-160">Тем не менее Обратите внимание, что базы данных должен поддерживать это настройки базы данных, как это может привести к другие ограничения, что на практике часто делает этот параметр нецелесообразным.</span><span class="sxs-lookup"><span data-stu-id="fccec-160">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="fccec-161">Именно поэтому *SetNull* не значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fccec-161">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="fccec-162">Если вы не хотите Core EF, когда-либо автоматически удалить сущность или значение null, внешний ключ, автоматически, а затем использовать *ограничение*.</span><span class="sxs-lookup"><span data-stu-id="fccec-162">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="fccec-163">Обратите внимание, что для этого необходимо, код синхронизации дочерних сущностей и их значения внешнего ключа вручную в противном случае ограничение исключения будут создаваться.</span><span class="sxs-lookup"><span data-stu-id="fccec-163">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="fccec-164">В ядре EF, в отличие от EF6 каскадный эффект не осуществляется немедленно, но вместо этого только, когда вызывается метод SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="fccec-164">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="fccec-165">**Изменения в версии 2.0 основные EF:** в предыдущих выпусках *ограничение* вызовет необязательные свойства внешнего ключа в записанные зависимые сущности будет присвоено значение null и был поведение необязательно связей удаления по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fccec-165">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="fccec-166">В версии 2.0 основные EF *ClientSetNull* был введен для представления этого поведения и стала значение по умолчанию для связей, необязательно.</span><span class="sxs-lookup"><span data-stu-id="fccec-166">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="fccec-167">Поведение *ограничение* была скорректирована для никогда не оказывает никакого влияния на зависимые сущности.</span><span class="sxs-lookup"><span data-stu-id="fccec-167">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="fccec-168">Примеры удаления сущности</span><span class="sxs-lookup"><span data-stu-id="fccec-168">Entity deletion examples</span></span>

<span data-ttu-id="fccec-169">В следующем примере кода является частью [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) можно загрузить и запустить.</span><span class="sxs-lookup"><span data-stu-id="fccec-169">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="fccec-170">В образце показано, что происходит для каждого вида поведения удаления для связи с обязательными и необязательными, при удалении родительской сущности.</span><span class="sxs-lookup"><span data-stu-id="fccec-170">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="fccec-171">Давайте рассмотрим каждый вариант, чтобы понять, что произошло.</span><span class="sxs-lookup"><span data-stu-id="fccec-171">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="fccec-172">DeleteBehavior.Cascade с обязательными или необязательными связи</span><span class="sxs-lookup"><span data-stu-id="fccec-172">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="fccec-173">Блог помечается как Deleted</span><span class="sxs-lookup"><span data-stu-id="fccec-173">Blog is marked as Deleted</span></span>
* <span data-ttu-id="fccec-174">В блогах изначально оставить без изменений, поскольку каскадные проводится до SaveChanges</span><span class="sxs-lookup"><span data-stu-id="fccec-174">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="fccec-175">SaveChanges производится пересылка операций удаления для зависимых элементов и дочерних элементов (сообщений) и затем основной или родительский (блог)</span><span class="sxs-lookup"><span data-stu-id="fccec-175">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="fccec-176">После сохранения все сущности отсоединяются, так как они теперь были удалены из базы данных</span><span class="sxs-lookup"><span data-stu-id="fccec-176">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="fccec-177">DeleteBehavior.ClientSetNull или DeleteBehavior.SetNull с обязательную связь</span><span class="sxs-lookup"><span data-stu-id="fccec-177">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="fccec-178">Блог помечается как Deleted</span><span class="sxs-lookup"><span data-stu-id="fccec-178">Blog is marked as Deleted</span></span>
* <span data-ttu-id="fccec-179">В блогах изначально оставить без изменений, поскольку каскадные проводится до SaveChanges</span><span class="sxs-lookup"><span data-stu-id="fccec-179">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="fccec-180">SaveChanges пытается установить post внешнего ключа значение null, но это не удается, так как внешнего ключа не допускает значения NULL</span><span class="sxs-lookup"><span data-stu-id="fccec-180">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="fccec-181">DeleteBehavior.ClientSetNull или DeleteBehavior.SetNull с дополнительную связь</span><span class="sxs-lookup"><span data-stu-id="fccec-181">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="fccec-182">Блог помечается как Deleted</span><span class="sxs-lookup"><span data-stu-id="fccec-182">Blog is marked as Deleted</span></span>
* <span data-ttu-id="fccec-183">В блогах изначально оставить без изменений, поскольку каскадные проводится до SaveChanges</span><span class="sxs-lookup"><span data-stu-id="fccec-183">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="fccec-184">Попыток SaveChanges внешнего ключа оба зависимых элементов и дочерних элементов (сообщений) задает значение null перед удалением основной или родительский (блог)</span><span class="sxs-lookup"><span data-stu-id="fccec-184">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="fccec-185">После сохранения, основной или родительский (блог) удаляется, но по-прежнему отслеживаются зависимые объекты или дочерние элементы (сообщений)</span><span class="sxs-lookup"><span data-stu-id="fccec-185">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="fccec-186">Записанные зависимые элементы и дочерние элементы (сообщений) теперь имеют значение null, значения внешнего ключа и их ссылки на удаленные основной или родительский (блог) был удален.</span><span class="sxs-lookup"><span data-stu-id="fccec-186">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="fccec-187">DeleteBehavior.Restrict с обязательными или необязательными связи</span><span class="sxs-lookup"><span data-stu-id="fccec-187">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="fccec-188">Блог помечается как Deleted</span><span class="sxs-lookup"><span data-stu-id="fccec-188">Blog is marked as Deleted</span></span>
* <span data-ttu-id="fccec-189">В блогах изначально оставить без изменений, поскольку каскадные проводится до SaveChanges</span><span class="sxs-lookup"><span data-stu-id="fccec-189">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="fccec-190">Поскольку *ограничение* сообщает EF, чтобы задать автоматически внешнего ключа null, он остается отличное от null и SaveChanges выдает без сохранения изменений</span><span class="sxs-lookup"><span data-stu-id="fccec-190">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="fccec-191">Обновлены примеры удаления</span><span class="sxs-lookup"><span data-stu-id="fccec-191">Delete orphans examples</span></span>

<span data-ttu-id="fccec-192">В следующем примере кода является частью [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) , можно загрузить выполнения.</span><span class="sxs-lookup"><span data-stu-id="fccec-192">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded an run.</span></span> <span data-ttu-id="fccec-193">В этом примере, сопровождающие для каждого вида поведения удаления для связи с обязательными и необязательными разрыва связи между родительский или основной и его дочерние элементы и зависимости.</span><span class="sxs-lookup"><span data-stu-id="fccec-193">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="fccec-194">В этом примере разрыва связи, удалив зависимые объекты или дочерние элементы (сообщений) из свойства навигации коллекции основной или родительский (блог).</span><span class="sxs-lookup"><span data-stu-id="fccec-194">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="fccec-195">Тем не менее действует, если ссылка на основной или родительский зависимые потомки вместо очищается.</span><span class="sxs-lookup"><span data-stu-id="fccec-195">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="fccec-196">Давайте рассмотрим каждый вариант, чтобы понять, что произошло.</span><span class="sxs-lookup"><span data-stu-id="fccec-196">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="fccec-197">DeleteBehavior.Cascade с обязательными или необязательными связи</span><span class="sxs-lookup"><span data-stu-id="fccec-197">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="fccec-198">В блогах помечаются как измененные, поскольку прерывание связи вызвал FK помечается как значение null</span><span class="sxs-lookup"><span data-stu-id="fccec-198">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="fccec-199">Если внешнего ключа не допускает значения NULL, затем фактическое значение не изменится даже если она помечена как допускающий значения null</span><span class="sxs-lookup"><span data-stu-id="fccec-199">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="fccec-200">SaveChanges производится пересылка операций удаления для зависимых элементов и дочерних элементов (сообщений)</span><span class="sxs-lookup"><span data-stu-id="fccec-200">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="fccec-201">После сохранения отсоединены зависимые объекты или дочерние элементы (сообщений), так как они теперь были удалены из базы данных</span><span class="sxs-lookup"><span data-stu-id="fccec-201">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="fccec-202">DeleteBehavior.ClientSetNull или DeleteBehavior.SetNull с обязательную связь</span><span class="sxs-lookup"><span data-stu-id="fccec-202">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="fccec-203">В блогах помечаются как измененные, поскольку прерывание связи вызвал FK помечается как значение null</span><span class="sxs-lookup"><span data-stu-id="fccec-203">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="fccec-204">Если внешнего ключа не допускает значения NULL, затем фактическое значение не изменится даже если она помечена как допускающий значения null</span><span class="sxs-lookup"><span data-stu-id="fccec-204">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="fccec-205">SaveChanges пытается установить post внешнего ключа значение null, но это не удается, так как внешнего ключа не допускает значения NULL</span><span class="sxs-lookup"><span data-stu-id="fccec-205">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="fccec-206">DeleteBehavior.ClientSetNull или DeleteBehavior.SetNull с дополнительную связь</span><span class="sxs-lookup"><span data-stu-id="fccec-206">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="fccec-207">В блогах помечаются как измененные, поскольку прерывание связи вызвал FK помечается как значение null</span><span class="sxs-lookup"><span data-stu-id="fccec-207">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="fccec-208">Если внешнего ключа не допускает значения NULL, затем фактическое значение не изменится даже если она помечена как допускающий значения null</span><span class="sxs-lookup"><span data-stu-id="fccec-208">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="fccec-209">SaveChanges внешнего ключа оба зависимых элементов и дочерних элементов (сообщений) задает значение null</span><span class="sxs-lookup"><span data-stu-id="fccec-209">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="fccec-210">После сохранения зависимые объекты или дочерние элементы (сообщений) теперь имеют значение null, значения внешнего ключа и их ссылки на удаленные основной или родительский (блог) был удален.</span><span class="sxs-lookup"><span data-stu-id="fccec-210">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="fccec-211">DeleteBehavior.Restrict с обязательными или необязательными связи</span><span class="sxs-lookup"><span data-stu-id="fccec-211">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="fccec-212">В блогах помечаются как измененные, поскольку прерывание связи вызвал FK помечается как значение null</span><span class="sxs-lookup"><span data-stu-id="fccec-212">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="fccec-213">Если внешнего ключа не допускает значения NULL, затем фактическое значение не изменится даже если она помечена как допускающий значения null</span><span class="sxs-lookup"><span data-stu-id="fccec-213">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="fccec-214">Поскольку *ограничение* сообщает EF, чтобы задать автоматически внешнего ключа null, он остается отличное от null и SaveChanges выдает без сохранения изменений</span><span class="sxs-lookup"><span data-stu-id="fccec-214">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="fccec-215">Каскадные отслеживаются сущностям</span><span class="sxs-lookup"><span data-stu-id="fccec-215">Cascading to untracked entities</span></span>

<span data-ttu-id="fccec-216">При вызове *SaveChanges*, каскадное удаление правил, которые будут применяться к любой сущности, которые отслеживаются контекстом.</span><span class="sxs-lookup"><span data-stu-id="fccec-216">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="fccec-217">Ситуация во всех приведенных выше примеров, вот почему SQL был создан для удаления основной или родительский (блог) и все зависимые объекты или дочерние элементы (записей):</span><span class="sxs-lookup"><span data-stu-id="fccec-217">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="fccec-218">Если только участнику загружается — например, когда отправляется запрос для блога без `Include(b => b.Posts)` для включения в блогах — то SaveChanges только создаст SQL для удаления основной или родительский:</span><span class="sxs-lookup"><span data-stu-id="fccec-218">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="fccec-219">Зависимые объекты или дочерние элементы (сообщений) могут быть удалены только в том случае, если соответствующий поведение cascade настройки базы данных.</span><span class="sxs-lookup"><span data-stu-id="fccec-219">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="fccec-220">EF используется для создания базы данных, это поведение cascade будет ли программа установки автоматически.</span><span class="sxs-lookup"><span data-stu-id="fccec-220">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
