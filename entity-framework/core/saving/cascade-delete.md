---
title: Каскадное удаление — EF Core
description: Настройка поведения удаления для связанных сущностей при удалении основной сущности
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
uid: core/saving/cascade-delete
ms.openlocfilehash: cf17e6586b89376b2d7fcc0f9cbfc8e1c4f6ba58
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89617405"
---
# <a name="cascade-delete"></a><span data-ttu-id="642d9-103">Каскадное удаление</span><span class="sxs-lookup"><span data-stu-id="642d9-103">Cascade Delete</span></span>

<span data-ttu-id="642d9-104">Каскадное удаление обычно используется в терминологии базы данных, чтобы описать характеристики, которые позволяют удалить строку, чтобы автоматически инициировать удаление связанных строк.</span><span class="sxs-lookup"><span data-stu-id="642d9-104">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="642d9-105">Тесно связанная концепция, которая также охватывается поведением удаления EF Core, — это автоматическое удаление дочерней сущности, когда ее связь с родительской сущностью разорвана. Часто это называется "удалением потерянных объектов".</span><span class="sxs-lookup"><span data-stu-id="642d9-105">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this is commonly known as "deleting orphans".</span></span>

<span data-ttu-id="642d9-106">EF Core реализует несколько поведений удаления и предоставляет конфигурацию поведений удаления отдельных связей.</span><span class="sxs-lookup"><span data-stu-id="642d9-106">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="642d9-107">Эта платформа также реализует соглашения, с помощью которых можно автоматически настроить полезные стандартные поведения удаления для каждой связи на основе [необходимости связей](xref:core/modeling/relationships#required-and-optional-relationships).</span><span class="sxs-lookup"><span data-stu-id="642d9-107">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship](xref:core/modeling/relationships#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="642d9-108">Поведения удаления</span><span class="sxs-lookup"><span data-stu-id="642d9-108">Delete behaviors</span></span>

<span data-ttu-id="642d9-109">Поведения удаления определены в типе перечислителя *DeleteBehavior*. Их можно передать в текучий API *OnDelete*, чтобы определить, будет ли удаление основной или родительской сущности или разрыв связей с зависимыми или дочерними сущностями иметь побочный эффект в зависимых или дочерних сущностях.</span><span class="sxs-lookup"><span data-stu-id="642d9-109">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="642d9-110">Существует три действия, которые EF может предпринять при удалении основной или родительской сущности, а также при разрыве связи с дочерней сущностью:</span><span class="sxs-lookup"><span data-stu-id="642d9-110">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>

* <span data-ttu-id="642d9-111">дочерняя или зависимая сущность может быть удалена;</span><span class="sxs-lookup"><span data-stu-id="642d9-111">The child/dependent can be deleted</span></span>
* <span data-ttu-id="642d9-112">значения дочернего внешнего ключа могут быть NULL;</span><span class="sxs-lookup"><span data-stu-id="642d9-112">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="642d9-113">дочерняя сущность остается неизменной.</span><span class="sxs-lookup"><span data-stu-id="642d9-113">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="642d9-114">Поведение удаления, настроенное в модели EF, применяется, только когда основная сущность удалена с помощью EF Core, а зависимые сущности загружены в память (т. е. для отслеживаемых зависимостей).</span><span class="sxs-lookup"><span data-stu-id="642d9-114">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (that is, for tracked dependents).</span></span> <span data-ttu-id="642d9-115">В базе данных необходимо настроить соответствующее каскадное поведение, чтобы убедиться, что к данным, не отслеживаемым контекстом, применены необходимые действия.</span><span class="sxs-lookup"><span data-stu-id="642d9-115">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="642d9-116">Если вы создаете базу данных с помощью EF Core, каскадное поведение будет установлено автоматически.</span><span class="sxs-lookup"><span data-stu-id="642d9-116">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="642d9-117">Для второго действия, указанного выше, устанавливать для внешнего ключа значение NULL недопустимо, если он не допускает этого значения.</span><span class="sxs-lookup"><span data-stu-id="642d9-117">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="642d9-118">(Внешний ключ, не допускающий значения NULL, соответствует обязательной связи.) В этих случаях EF Core отслеживает, чтобы свойство внешнего ключа было отмечено как NULL, до вызова метода SaveChanges. Иначе возникнет исключение, так как изменение нельзя сохранить в базе данных.</span><span class="sxs-lookup"><span data-stu-id="642d9-118">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="642d9-119">Это похоже на получение нарушения ограничения из базы данных.</span><span class="sxs-lookup"><span data-stu-id="642d9-119">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="642d9-120">Есть четыре поведения удаления, как показано в таблицах ниже.</span><span class="sxs-lookup"><span data-stu-id="642d9-120">There are four delete behaviors, as listed in the tables below.</span></span>

### <a name="optional-relationships"></a><span data-ttu-id="642d9-121">Необязательные связи</span><span class="sxs-lookup"><span data-stu-id="642d9-121">Optional relationships</span></span>

<span data-ttu-id="642d9-122">Для необязательных связей (с внешним ключом, допускающим значение NULL) _можно_ сохранить значение NULL внешнего ключа. Это имеет такие последствия:</span><span class="sxs-lookup"><span data-stu-id="642d9-122">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="642d9-123">Имя поведения</span><span class="sxs-lookup"><span data-stu-id="642d9-123">Behavior Name</span></span>               | <span data-ttu-id="642d9-124">Влияние на зависимую или дочернюю сущность в памяти</span><span class="sxs-lookup"><span data-stu-id="642d9-124">Effect on dependent/child in memory</span></span>    | <span data-ttu-id="642d9-125">Влияние на зависимую или дочернюю сущность в базе данных</span><span class="sxs-lookup"><span data-stu-id="642d9-125">Effect on dependent/child in database</span></span>  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| <span data-ttu-id="642d9-126">**Cascade**</span><span class="sxs-lookup"><span data-stu-id="642d9-126">**Cascade**</span></span>                 | <span data-ttu-id="642d9-127">Сущности удаляются</span><span class="sxs-lookup"><span data-stu-id="642d9-127">Entities are deleted</span></span>                   | <span data-ttu-id="642d9-128">Сущности удаляются</span><span class="sxs-lookup"><span data-stu-id="642d9-128">Entities are deleted</span></span>                   |
| <span data-ttu-id="642d9-129">**ClientSetNull** (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="642d9-129">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="642d9-130">Свойства внешнего ключа имеют значения NULL</span><span class="sxs-lookup"><span data-stu-id="642d9-130">Foreign key properties are set to null</span></span> | <span data-ttu-id="642d9-131">Нет</span><span class="sxs-lookup"><span data-stu-id="642d9-131">None</span></span>                                   |
| <span data-ttu-id="642d9-132">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="642d9-132">**SetNull**</span></span>                 | <span data-ttu-id="642d9-133">Свойства внешнего ключа имеют значения NULL</span><span class="sxs-lookup"><span data-stu-id="642d9-133">Foreign key properties are set to null</span></span> | <span data-ttu-id="642d9-134">Свойства внешнего ключа имеют значения NULL</span><span class="sxs-lookup"><span data-stu-id="642d9-134">Foreign key properties are set to null</span></span> |
| <span data-ttu-id="642d9-135">**Restrict**</span><span class="sxs-lookup"><span data-stu-id="642d9-135">**Restrict**</span></span>                | <span data-ttu-id="642d9-136">Отсутствуют</span><span class="sxs-lookup"><span data-stu-id="642d9-136">None</span></span>                                   | <span data-ttu-id="642d9-137">None</span><span class="sxs-lookup"><span data-stu-id="642d9-137">None</span></span>                                   |

### <a name="required-relationships"></a><span data-ttu-id="642d9-138">Обязательные связи</span><span class="sxs-lookup"><span data-stu-id="642d9-138">Required relationships</span></span>

<span data-ttu-id="642d9-139">Для обязательных связей (внешний ключ, не допускающий значения NULL) _невозможно_ сохранить значение NULL внешнего ключа. Это имеет такие последствия:</span><span class="sxs-lookup"><span data-stu-id="642d9-139">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="642d9-140">Имя поведения</span><span class="sxs-lookup"><span data-stu-id="642d9-140">Behavior Name</span></span>         | <span data-ttu-id="642d9-141">Влияние на зависимую или дочернюю сущность в памяти</span><span class="sxs-lookup"><span data-stu-id="642d9-141">Effect on dependent/child in memory</span></span> | <span data-ttu-id="642d9-142">Влияние на зависимую или дочернюю сущность в базе данных</span><span class="sxs-lookup"><span data-stu-id="642d9-142">Effect on dependent/child in database</span></span> |
|:----------------------|:------------------------------------|:--------------------------------------|
| <span data-ttu-id="642d9-143">**Cascade** (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="642d9-143">**Cascade** (Default)</span></span> | <span data-ttu-id="642d9-144">Сущности удаляются</span><span class="sxs-lookup"><span data-stu-id="642d9-144">Entities are deleted</span></span>                | <span data-ttu-id="642d9-145">Сущности удаляются</span><span class="sxs-lookup"><span data-stu-id="642d9-145">Entities are deleted</span></span>                  |
| <span data-ttu-id="642d9-146">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="642d9-146">**ClientSetNull**</span></span>     | <span data-ttu-id="642d9-147">Строки SaveChanges</span><span class="sxs-lookup"><span data-stu-id="642d9-147">SaveChanges throws</span></span>                  | <span data-ttu-id="642d9-148">Нет</span><span class="sxs-lookup"><span data-stu-id="642d9-148">None</span></span>                                  |
| <span data-ttu-id="642d9-149">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="642d9-149">**SetNull**</span></span>           | <span data-ttu-id="642d9-150">Строки SaveChanges</span><span class="sxs-lookup"><span data-stu-id="642d9-150">SaveChanges throws</span></span>                  | <span data-ttu-id="642d9-151">Строки SaveChanges</span><span class="sxs-lookup"><span data-stu-id="642d9-151">SaveChanges throws</span></span>                    |
| <span data-ttu-id="642d9-152">**Restrict**</span><span class="sxs-lookup"><span data-stu-id="642d9-152">**Restrict**</span></span>          | <span data-ttu-id="642d9-153">Отсутствуют</span><span class="sxs-lookup"><span data-stu-id="642d9-153">None</span></span>                                | <span data-ttu-id="642d9-154">None</span><span class="sxs-lookup"><span data-stu-id="642d9-154">None</span></span>                                  |

<span data-ttu-id="642d9-155">Указанное в таблице выше значение *Нет* может привести к нарушению ограничения.</span><span class="sxs-lookup"><span data-stu-id="642d9-155">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="642d9-156">Например, если основная или дочерняя сущность удалена, но никаких действий, чтобы изменить внешний ключ зависимой или дочерней сущности не выполняется, то база данных, вероятно, вызовет метод SaveChanges из-за нарушения ограничения внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="642d9-156">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="642d9-157">На высоком уровне:</span><span class="sxs-lookup"><span data-stu-id="642d9-157">At a high level:</span></span>

* <span data-ttu-id="642d9-158">Если у вас есть сущности, которые не могут существовать без родительской сущности, и вы хотите, чтобы EF автоматически удалил дочернюю сущность, воспользуйтесь поведением *Cascade*.</span><span class="sxs-lookup"><span data-stu-id="642d9-158">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="642d9-159">Сущности, которые не могут существовать без родительского элемента, обычно используют обязательные связи, в которых по умолчанию используется поведение *Cascade*.</span><span class="sxs-lookup"><span data-stu-id="642d9-159">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="642d9-160">Если у вас есть сущности, у которых может быть или отсутствовать родительский элемент, и вы хотите, чтобы EF автоматически обнулил внешний ключ, используйте поведение *ClientSetNull*.</span><span class="sxs-lookup"><span data-stu-id="642d9-160">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="642d9-161">Сущности, которые могут существовать без родительского элемента, обычно используют необязательные связи, в которых по умолчанию используется поведение *ClientSetNull*.</span><span class="sxs-lookup"><span data-stu-id="642d9-161">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="642d9-162">Если требуется, чтобы база данных также пыталась распространять значения NULL для дочерних внешних ключей, даже когда дочерняя сущность не загружена, используйте поведение *SetNull*.</span><span class="sxs-lookup"><span data-stu-id="642d9-162">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="642d9-163">Тем не менее, обратите внимание, что база данных должна поддерживать эту возможность. Настройка базы данных таким образом может привести к другим ограничениям, которые на практике часто делают этот вариант непрактичным.</span><span class="sxs-lookup"><span data-stu-id="642d9-163">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="642d9-164">Поэтому *SetNull* не используется по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="642d9-164">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="642d9-165">Если вы хотите, чтобы платформа EF Core никогда автоматически не удаляла сущность или не обнуляла внешний ключ, используйте поведение *Restrict*.</span><span class="sxs-lookup"><span data-stu-id="642d9-165">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="642d9-166">Обратите внимание, что для этого необходимо вручную синхронизировать в коде дочерние сущности и их значения внешнего ключа, в противном случае возникнут исключения ограничения.</span><span class="sxs-lookup"><span data-stu-id="642d9-166">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="642d9-167">В EF Core, в отличие от EF6, каскадные эффекты не происходят немедленно, а только при вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="642d9-167">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="642d9-168">**Изменения в EF Core 2.0.** В предыдущих выпусках поведение *Restrict* присваивало для необязательных свойств внешнего ключа в отслеживаемых зависимых сущностях значение NULL. Это было стандартное поведение удаления для необязательных связей.</span><span class="sxs-lookup"><span data-stu-id="642d9-168">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="642d9-169">В EF Core 2.0 предоставлено *ClientSetNull*, которое имеет аналогичное поведение и является стандартным для необязательных связей.</span><span class="sxs-lookup"><span data-stu-id="642d9-169">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="642d9-170">Поведение *Restrict* было изменено, чтобы никогда не иметь побочных эффектов в зависимых сущностях.</span><span class="sxs-lookup"><span data-stu-id="642d9-170">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="642d9-171">Примеры удаления сущности</span><span class="sxs-lookup"><span data-stu-id="642d9-171">Entity deletion examples</span></span>

<span data-ttu-id="642d9-172">Приведенный ниже код является частью [примера](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/), который можно скачать и выполнить.</span><span class="sxs-lookup"><span data-stu-id="642d9-172">The code below is part of a [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="642d9-173">В примере показано, что происходит с каждым поведением удаления для обязательных и необязательных связей при удалении родительской сущности.</span><span class="sxs-lookup"><span data-stu-id="642d9-173">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="642d9-174">Рассмотрим каждый вариант, чтобы понять, что происходит.</span><span class="sxs-lookup"><span data-stu-id="642d9-174">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="642d9-175">DeleteBehavior.Cascade с обязательной или необязательной связью</span><span class="sxs-lookup"><span data-stu-id="642d9-175">DeleteBehavior.Cascade with required or optional relationship</span></span>

```console
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '2' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="642d9-176">Блог помечен как удаленный.</span><span class="sxs-lookup"><span data-stu-id="642d9-176">Blog is marked as Deleted</span></span>
* <span data-ttu-id="642d9-177">Записи изначально остаются без изменений, так как каскадные удаления не выполняются до вызова метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="642d9-177">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="642d9-178">SaveChanges отправляет операции удаления в зависимые или дочерние сущности (записи), а затем в основные или родительские (блог).</span><span class="sxs-lookup"><span data-stu-id="642d9-178">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="642d9-179">После сохранения все сущности отсоединяются, так как они были удалены из базы данных.</span><span class="sxs-lookup"><span data-stu-id="642d9-179">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="642d9-180">DeleteBehavior.ClientSetNull или DeleteBehavior.SetNull с обязательной связью</span><span class="sxs-lookup"><span data-stu-id="642d9-180">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="642d9-181">Блог помечен как удаленный.</span><span class="sxs-lookup"><span data-stu-id="642d9-181">Blog is marked as Deleted</span></span>
* <span data-ttu-id="642d9-182">Записи изначально остаются без изменений, так как каскадные удаления не выполняются до вызова метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="642d9-182">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="642d9-183">SaveChanges пытается установить для внешнего ключа записи значение NULL. Однако эта попытка завершается неудачей, так как внешний ключ не допускает этого значения.</span><span class="sxs-lookup"><span data-stu-id="642d9-183">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="642d9-184">DeleteBehavior.ClientSetNull или DeleteBehavior.SetNull с необязательной связью</span><span class="sxs-lookup"><span data-stu-id="642d9-184">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '2' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="642d9-185">Блог помечен как удаленный.</span><span class="sxs-lookup"><span data-stu-id="642d9-185">Blog is marked as Deleted</span></span>
* <span data-ttu-id="642d9-186">Записи изначально остаются без изменений, так как каскадные удаления не выполняются до вызова метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="642d9-186">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="642d9-187">SaveChanges пытается установить для внешнего ключа зависимых или дочерних сущностей (записей) значение NULL, прежде чем удалить основные или родительские (блог).</span><span class="sxs-lookup"><span data-stu-id="642d9-187">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="642d9-188">После сохранения основная или родительская сущность (блог) удаляется, но зависимые или дочерние сущности (записи) по-прежнему отслеживаются.</span><span class="sxs-lookup"><span data-stu-id="642d9-188">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="642d9-189">Отслеживаемые зависимые и дочерние сущности (записи) теперь имеют NULL в качестве значений внешних ключей, а их связь с удаленными основными и родительскими сущностями (блог) удалена.</span><span class="sxs-lookup"><span data-stu-id="642d9-189">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="642d9-190">DeleteBehavior.Restrict с обязательной или необязательной связью</span><span class="sxs-lookup"><span data-stu-id="642d9-190">DeleteBehavior.Restrict with required or optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="642d9-191">Блог помечен как удаленный.</span><span class="sxs-lookup"><span data-stu-id="642d9-191">Blog is marked as Deleted</span></span>
* <span data-ttu-id="642d9-192">Записи изначально остаются без изменений, так как каскадные удаления не выполняются до вызова метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="642d9-192">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="642d9-193">Так как *Restrict* указывает EF не устанавливать автоматически для внешнего ключа значение NULL, он не имеет этого значения и метод SaveChanges вызывается без сохранения.</span><span class="sxs-lookup"><span data-stu-id="642d9-193">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="642d9-194">Примеры удаления потерянных объектов</span><span class="sxs-lookup"><span data-stu-id="642d9-194">Delete orphans examples</span></span>

<span data-ttu-id="642d9-195">Приведенный ниже код является частью [примера](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/), который можно скачать и выполнить.</span><span class="sxs-lookup"><span data-stu-id="642d9-195">The code below is part of a [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="642d9-196">В примере показано, что происходит с каждым поведением удаления для обязательных и необязательных связей при разрыве связи между родительскими или основными сущностями и их дочерними или зависимыми сущностями.</span><span class="sxs-lookup"><span data-stu-id="642d9-196">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="642d9-197">В этом примере связь разрывается путем удаления зависимых или дочерних сущностей (записей) из свойства навигации коллекции в основных или родительских сущностях (блог).</span><span class="sxs-lookup"><span data-stu-id="642d9-197">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="642d9-198">Тем не менее поведение остается прежним, если ссылка из зависимой или дочерней сущности к основной или родительской сущности обнулена.</span><span class="sxs-lookup"><span data-stu-id="642d9-198">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="642d9-199">Рассмотрим каждый вариант, чтобы понять, что происходит.</span><span class="sxs-lookup"><span data-stu-id="642d9-199">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="642d9-200">DeleteBehavior.Cascade с обязательной или необязательной связью</span><span class="sxs-lookup"><span data-stu-id="642d9-200">DeleteBehavior.Cascade with required or optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '2' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '2' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="642d9-201">Записи помечены как измененные, так как из-за разрыва связи внешний ключ помечен как NULL.</span><span class="sxs-lookup"><span data-stu-id="642d9-201">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="642d9-202">Если внешний ключ не допускает значения NULL, то фактическое значение не будет изменено, даже если оно помечено как NULL.</span><span class="sxs-lookup"><span data-stu-id="642d9-202">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="642d9-203">SaveChanges отправляет операции удаления в зависимые или дочерние сущности (записи).</span><span class="sxs-lookup"><span data-stu-id="642d9-203">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="642d9-204">После сохранения зависимые или дочерние сущности (записи) отсоединяются, так как они были удалены из базы данных.</span><span class="sxs-lookup"><span data-stu-id="642d9-204">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="642d9-205">DeleteBehavior.ClientSetNull или DeleteBehavior.SetNull с обязательной связью</span><span class="sxs-lookup"><span data-stu-id="642d9-205">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '2' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="642d9-206">Записи помечены как измененные, так как из-за разрыва связи внешний ключ помечен как NULL.</span><span class="sxs-lookup"><span data-stu-id="642d9-206">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="642d9-207">Если внешний ключ не допускает значения NULL, то фактическое значение не будет изменено, даже если оно помечено как NULL.</span><span class="sxs-lookup"><span data-stu-id="642d9-207">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="642d9-208">SaveChanges пытается установить для внешнего ключа записи значение NULL. Однако эта попытка завершается неудачей, так как внешний ключ не допускает этого значения.</span><span class="sxs-lookup"><span data-stu-id="642d9-208">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="642d9-209">DeleteBehavior.ClientSetNull или DeleteBehavior.SetNull с необязательной связью</span><span class="sxs-lookup"><span data-stu-id="642d9-209">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '2' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '2' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="642d9-210">Записи помечены как измененные, так как из-за разрыва связи внешний ключ помечен как NULL.</span><span class="sxs-lookup"><span data-stu-id="642d9-210">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="642d9-211">Если внешний ключ не допускает значения NULL, то фактическое значение не будет изменено, даже если оно помечено как NULL.</span><span class="sxs-lookup"><span data-stu-id="642d9-211">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="642d9-212">SaveChanges устанавливает для внешнего ключа зависимых или дочерних сущностей (записей) значение NULL.</span><span class="sxs-lookup"><span data-stu-id="642d9-212">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="642d9-213">После сохранения зависимые или дочерние сущности (записи) теперь имеют NULL в качестве значений внешних ключей, а их связь с удаленными основными или родительскими сущностями (блог) удалена.</span><span class="sxs-lookup"><span data-stu-id="642d9-213">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="642d9-214">DeleteBehavior.Restrict с обязательной или необязательной связью</span><span class="sxs-lookup"><span data-stu-id="642d9-214">DeleteBehavior.Restrict with required or optional relationship</span></span>

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '2' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="642d9-215">Записи помечены как измененные, так как из-за разрыва связи внешний ключ помечен как NULL.</span><span class="sxs-lookup"><span data-stu-id="642d9-215">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="642d9-216">Если внешний ключ не допускает значения NULL, то фактическое значение не будет изменено, даже если оно помечено как NULL.</span><span class="sxs-lookup"><span data-stu-id="642d9-216">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="642d9-217">Так как *Restrict* указывает EF не устанавливать автоматически для внешнего ключа значение NULL, он не имеет этого значения и метод SaveChanges вызывается без сохранения.</span><span class="sxs-lookup"><span data-stu-id="642d9-217">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="642d9-218">Использование каскадных функций в неотслеживаемых сущностях</span><span class="sxs-lookup"><span data-stu-id="642d9-218">Cascading to untracked entities</span></span>

<span data-ttu-id="642d9-219">При вызове метода *SaveChanges* правила каскадного удаления будут применяться ко всем сущностям, которые отслеживаются контекстом.</span><span class="sxs-lookup"><span data-stu-id="642d9-219">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="642d9-220">Такая ситуация приведена в примерах выше. Вот почему был создан код SQL, чтобы удалить основные или родительские сущности (блог) и все зависимые или дочерние сущности (записи).</span><span class="sxs-lookup"><span data-stu-id="642d9-220">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="642d9-221">Если загружается только субъект, например, когда запрос, выполненный для блога без `Include(b => b.Posts)`, также содержит записи, то SaveChanges создаст код SQL, чтобы удалить основную или родительскую сущность.</span><span class="sxs-lookup"><span data-stu-id="642d9-221">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="642d9-222">Зависимые или дочерние сущности (записи) будут удалены, только если в базе данных настроено соответствующее каскадное поведение.</span><span class="sxs-lookup"><span data-stu-id="642d9-222">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="642d9-223">Если вы создаете базу данных с помощью EF, каскадное поведение будет установлено автоматически.</span><span class="sxs-lookup"><span data-stu-id="642d9-223">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
