---
title: Собственные типы сущностей — EF Core
description: Настройка собственных типов сущностей или статистических выражений при использовании Entity Framework Core
author: AndriySvyryd
ms.date: 11/06/2019
uid: core/modeling/owned-entities
ms.openlocfilehash: a49d9aab735232dfd5a3db456410d527f94f3c18
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92063781"
---
# <a name="owned-entity-types"></a><span data-ttu-id="d4ab6-103">Принадлежащие типы сущностей</span><span class="sxs-lookup"><span data-stu-id="d4ab6-103">Owned Entity Types</span></span>

<span data-ttu-id="d4ab6-104">EF Core позволяет моделировать типы сущностей, которые могут отображаться только в свойствах навигации других типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="d4ab6-105">Они называются _типами собственных сущностей_.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-105">These are called _owned entity types_.</span></span> <span data-ttu-id="d4ab6-106">Сущность, содержащая принадлежащий тип сущности, является ее _владельцем_.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-106">The entity containing an owned entity type is its _owner_.</span></span>

<span data-ttu-id="d4ab6-107">Собственные сущности по сути являются частью владельца и не могут существовать без них, они похожи на [статистические выражения](https://martinfowler.com/bliki/DDD_Aggregate.html).</span><span class="sxs-lookup"><span data-stu-id="d4ab6-107">Owned entities are essentially a part of the owner and cannot exist without it, they are conceptually similar to [aggregates](https://martinfowler.com/bliki/DDD_Aggregate.html).</span></span> <span data-ttu-id="d4ab6-108">Это означает, что принадлежащая сущность является определением на зависимой стороне связи с владельцем.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-108">This means that the owned entity is by definition on the dependent side of the relationship with the owner.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="d4ab6-109">Явная конфигурация</span><span class="sxs-lookup"><span data-stu-id="d4ab6-109">Explicit configuration</span></span>

<span data-ttu-id="d4ab6-110">Собственные типы сущностей никогда не включаются в EF Core в модели по соглашению.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-110">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="d4ab6-111">`OwnsOne` `OnModelCreating` `OwnedAttribute` Для настройки типа в качестве принадлежащего типа можно использовать метод в или добавить аннотацию к типу с помощью метода.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-111">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` to configure the type as an owned type.</span></span>

<span data-ttu-id="d4ab6-112">В этом примере `StreetAddress` — это тип без свойства Identity.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-112">In this example, `StreetAddress` is a type with no identity property.</span></span> <span data-ttu-id="d4ab6-113">Он используется как свойство сущности Order для указания адреса доставки конкретного заказа.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-113">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span>

<span data-ttu-id="d4ab6-114">Мы можем использовать, `OwnedAttribute` чтобы интерпретировать его как принадлежащую сущность при ссылке из другого типа сущности:</span><span class="sxs-lookup"><span data-stu-id="d4ab6-114">We can use the `OwnedAttribute` to treat it as an owned entity when referenced from another entity type:</span></span>

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

<span data-ttu-id="d4ab6-115">Можно также использовать `OwnsOne` метод в, `OnModelCreating` чтобы указать, что `ShippingAddress` свойство является владельцем сущности `Order` типа сущности, и настроить дополнительные аспекты при необходимости.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-115">It is also possible to use the `OwnsOne` method in `OnModelCreating` to specify that the `ShippingAddress` property is an Owned Entity of the `Order` entity type and to configure additional facets if needed.</span></span>

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

<span data-ttu-id="d4ab6-116">Если `ShippingAddress` свойство является частным в `Order` типе, можно использовать строковую версию `OwnsOne` метода:</span><span class="sxs-lookup"><span data-stu-id="d4ab6-116">If the `ShippingAddress` property is private in the `Order` type, you can use the string version of the `OwnsOne` method:</span></span>

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

<span data-ttu-id="d4ab6-117">См. [полный пример проекта](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) для получения дополнительных контекстов.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-117">See the [full sample project](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) for more context.</span></span>

> [!TIP]
> <span data-ttu-id="d4ab6-118">Тип принадлежащей сущности можно пометить как обязательный. Дополнительные сведения см. в разделе [обязательные зависимости "один к одному](xref:core/modeling/relationships#one-to-one) ".</span><span class="sxs-lookup"><span data-stu-id="d4ab6-118">The owned entity type can be marked as required, see [Required one-to-one dependents](xref:core/modeling/relationships#one-to-one) for more information.</span></span>

## <a name="implicit-keys"></a><span data-ttu-id="d4ab6-119">Неявные ключи</span><span class="sxs-lookup"><span data-stu-id="d4ab6-119">Implicit keys</span></span>

<span data-ttu-id="d4ab6-120">Принадлежащие типы, настроенные `OwnsOne` или обнаруженные с помощью навигации по ссылке, всегда имеют связь «один к одному» с владельцем, поэтому им не нужны собственные значения ключей, так как значения внешних ключей уникальны.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-120">Owned types configured with `OwnsOne` or discovered through a reference navigation always have a one-to-one relationship with the owner, therefore they don't need their own key values as the foreign key values are unique.</span></span> <span data-ttu-id="d4ab6-121">В предыдущем примере `StreetAddress` тип не требует определения ключевого свойства.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-121">In the previous example, the `StreetAddress` type does not need to define a key property.</span></span>  

<span data-ttu-id="d4ab6-122">Чтобы понять, как EF Core отслеживает эти объекты, полезно знать, что первичный ключ создается как [свойство теневого копирования](xref:core/modeling/shadow-properties) для принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-122">In order to understand how EF Core tracks these objects, it is useful to know that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="d4ab6-123">Значение ключа экземпляра принадлежащего типа будет таким же, как и значение ключа экземпляра Owner.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-123">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>

## <a name="collections-of-owned-types"></a><span data-ttu-id="d4ab6-124">Коллекции принадлежащих типов</span><span class="sxs-lookup"><span data-stu-id="d4ab6-124">Collections of owned types</span></span>

<span data-ttu-id="d4ab6-125">Чтобы настроить коллекцию принадлежащих типов, используйте `OwnsMany` в `OnModelCreating` .</span><span class="sxs-lookup"><span data-stu-id="d4ab6-125">To configure a collection of owned types use `OwnsMany` in `OnModelCreating`.</span></span>

<span data-ttu-id="d4ab6-126">Принадлежащие типы должны иметь первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-126">Owned types need a primary key.</span></span> <span data-ttu-id="d4ab6-127">Если в типе .NET нет хороших свойств кандидатов, EF Core может попытаться создать его.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-127">If there are no good candidates properties on the .NET type, EF Core can try to create one.</span></span> <span data-ttu-id="d4ab6-128">Однако если собственные типы определяются через коллекцию, достаточно просто создать свойство теневой копии, которое будет действовать как внешний ключ в качестве внешнего ключа владельца и первичный ключ принадлежащего экземпляра, как мы делаем `OwnsOne` : для каждого владельца может существовать несколько экземпляров принадлежащих типов, поэтому ключ владельца недостаточно, чтобы предоставить уникальный идентификатор для каждого принадлежащего экземпляра.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-128">However, when owned types are defined through a collection, it isn't enough to just create a shadow property to act as both the foreign key into the owner and the primary key of the owned instance, as we do for `OwnsOne`: there can be multiple owned type instances for each owner, and hence the key of the owner isn't enough to provide a unique identity for each owned instance.</span></span>

<span data-ttu-id="d4ab6-129">Вот два самых простых решения:</span><span class="sxs-lookup"><span data-stu-id="d4ab6-129">The two most straightforward solutions to this are:</span></span>

- <span data-ttu-id="d4ab6-130">Определение первичного ключа суррогата для нового свойства независимо от внешнего ключа, указывающего на владельца.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-130">Defining a surrogate primary key on a new property independent of the foreign key that points to the owner.</span></span> <span data-ttu-id="d4ab6-131">Содержащиеся значения должны быть уникальными для всех владельцев (например, если у родителя {1} есть дочерний элемент {1} , а родительский элемент {2} не может быть дочерним {1} ), поэтому значение не имеет какого-либо значения.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-131">The contained values would need to be unique across all owners (e.g. if Parent {1} has Child {1}, then Parent {2} cannot have Child {1}), so the value doesn't have any inherent meaning.</span></span> <span data-ttu-id="d4ab6-132">Так как внешний ключ не является частью первичного ключа, его значения можно изменить, поэтому дочерний элемент можно переместить из одного родителя в другой, однако это обычно происходит по семантике агрегата.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-132">Since the foreign key is not part of the primary key its values can be changed, so you could move a child from one parent to another one, however this usually goes against aggregate semantics.</span></span>
- <span data-ttu-id="d4ab6-133">Использование внешнего ключа и дополнительного свойства в качестве составного ключа.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-133">Using the foreign key and an additional property as a composite key.</span></span> <span data-ttu-id="d4ab6-134">Значение дополнительного свойства теперь должно быть уникальным только для данного родителя (поэтому, если родитель {1} имеет дочерний элемент, {1,1} родительский элемент {2} может по-прежнему иметь дочерний элемент {2,1} ).</span><span class="sxs-lookup"><span data-stu-id="d4ab6-134">The additional property value now only needs to be unique for a given parent (so if Parent {1} has Child {1,1} then Parent {2} can still have Child {2,1}).</span></span> <span data-ttu-id="d4ab6-135">Делая внешний ключ частью первичного ключа, связь между владельцем и собственной сущностью становится неизменяемой и в большей мере отражает семантику агрегата.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-135">By making the foreign key part of the primary key the relationship between the owner and the owned entity becomes immutable and reflects aggregate semantics better.</span></span> <span data-ttu-id="d4ab6-136">По умолчанию это EF Core.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-136">This is what EF Core does by default.</span></span>

<span data-ttu-id="d4ab6-137">В этом примере мы будем использовать `Distributor` класс.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-137">In this example we'll use the `Distributor` class.</span></span>

[!code-csharp[Distributor](../../../samples/core/Modeling/OwnedEntities/Distributor.cs?name=Distributor)]

<span data-ttu-id="d4ab6-138">По умолчанию первичным ключом, используемым для принадлежащего типа, на который ссылается `ShippingCenters` свойство навигации, будет, `("DistributorId", "Id")` где `"DistributorId"` — FK, а `"Id"` — уникальное `int` значение.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-138">By default the primary key used for the owned type referenced through the `ShippingCenters` navigation property will be `("DistributorId", "Id")` where `"DistributorId"` is the FK and `"Id"` is a unique `int` value.</span></span>

<span data-ttu-id="d4ab6-139">Для настройки другого вызова первичного ключа `HasKey` .</span><span class="sxs-lookup"><span data-stu-id="d4ab6-139">To configure a different primary key call `HasKey`.</span></span>

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="d4ab6-140">Сопоставление принадлежащих типов с разделением таблицы</span><span class="sxs-lookup"><span data-stu-id="d4ab6-140">Mapping owned types with table splitting</span></span>

<span data-ttu-id="d4ab6-141">При использовании реляционных баз данных ссылочные типы, принадлежащие по умолчанию, сопоставляются с той же таблицей, что и владелец.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-141">When using relational databases, by default reference owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="d4ab6-142">Для этого требуется разделение таблицы двумя столбцами: некоторые столбцы будут использоваться для хранения данных владельца, а некоторые столбцы будут использоваться для хранения данных собственной сущности.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-142">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="d4ab6-143">Это обычная функция, известная как [Разбиение таблицы](xref:core/modeling/table-splitting).</span><span class="sxs-lookup"><span data-stu-id="d4ab6-143">This is a common feature known as [table splitting](xref:core/modeling/table-splitting).</span></span>

<span data-ttu-id="d4ab6-144">По умолчанию EF Core будет наименовать столбцы базы данных для свойств собственного типа сущности после _Navigation_OwnedEntityProperty_шаблона.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-144">By default, EF Core will name the database columns for the properties of the owned entity type following the pattern _Navigation_OwnedEntityProperty_.</span></span> <span data-ttu-id="d4ab6-145">Поэтому `StreetAddress` свойства будут отображаться в таблице "Orders" с именами "ShippingAddress_Street" и "ShippingAddress_City".</span><span class="sxs-lookup"><span data-stu-id="d4ab6-145">Therefore the `StreetAddress` properties will appear in the 'Orders' table with the names 'ShippingAddress_Street' and 'ShippingAddress_City'.</span></span>

<span data-ttu-id="d4ab6-146">`HasColumnName`Для переименования этих столбцов можно использовать метод.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-146">You can use the `HasColumnName` method to rename those columns.</span></span>

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

> [!NOTE]
> <span data-ttu-id="d4ab6-147">Большинство обычных методов настройки типа сущности, таких как [Ignore](/dotnet/api/microsoft.entityframeworkcore.metadata.builders.ownednavigationbuilder.ignore) , можно вызывать одинаковым образом.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-147">Most of the normal entity type configuration methods like [Ignore](/dotnet/api/microsoft.entityframeworkcore.metadata.builders.ownednavigationbuilder.ignore) can be called in the same way.</span></span>

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="d4ab6-148">Совместное использование одного типа .NET между несколькими принадлежащими типами</span><span class="sxs-lookup"><span data-stu-id="d4ab6-148">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="d4ab6-149">Принадлежащий тип сущности может иметь тот же тип .NET, что и другой принадлежащий тип сущности, поэтому тип .NET может быть недостаточно для обнаружения принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-149">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="d4ab6-150">В таких случаях свойство, указывающее из владельца на принадлежащую сущность, превращается в _определяющую навигацию_ для принадлежит к типу сущности.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-150">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="d4ab6-151">С точки зрения EF Core, определяющая Навигация является частью удостоверения типа, рядом с типом .NET.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-151">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>

<span data-ttu-id="d4ab6-152">Например, в следующем классе `ShippingAddress` и `BillingAddress` оба имеют одинаковый тип .NET `StreetAddress` .</span><span class="sxs-lookup"><span data-stu-id="d4ab6-152">For example, in the following class `ShippingAddress` and `BillingAddress` are both of the same .NET type, `StreetAddress`.</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="d4ab6-153">Чтобы понять, как EF Core различает отслеживание экземпляров этих объектов, можно подумать, что определяющая Навигация стала частью ключа экземпляра наряду со значением ключа владельца и типом .NET принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-153">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="d4ab6-154">Вложенные принадлежащие типы</span><span class="sxs-lookup"><span data-stu-id="d4ab6-154">Nested owned types</span></span>

<span data-ttu-id="d4ab6-155">В этом примере `OrderDetails` принадлежат `BillingAddress` и `ShippingAddress` , которые являются `StreetAddress` типами.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-155">In this example `OrderDetails` owns `BillingAddress` and `ShippingAddress`, which are both `StreetAddress` types.</span></span> <span data-ttu-id="d4ab6-156">А `OrderDetails`, в свою очередь, принадлежит типу `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-156">Then `OrderDetails` is owned by the `DetailedOrder` type.</span></span>

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

<span data-ttu-id="d4ab6-157">Каждый переход к принадлежащему типу определяет отдельный тип сущности с полностью независимой конфигурацией.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-157">Each navigation to an owned type defines a separate entity type with completely independent configuration.</span></span>

<span data-ttu-id="d4ab6-158">Помимо вложенных принадлежащих типов, принадлежащие типы могут ссылаться на обычную сущность, которая может быть либо владельцем, либо другой сущностью при условии, что принадлежащая сущность находится на зависимой стороне.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-158">In addition to nested owned types, an owned type can reference a regular entity which can be either the owner or a different entity as long as the owned entity is on the dependent side.</span></span> <span data-ttu-id="d4ab6-159">Эта возможность задает собственные типы сущностей, кроме сложных типов в EF6.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-159">This capability sets owned entity types apart from complex types in EF6.</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

## <a name="configuring-owned-types"></a><span data-ttu-id="d4ab6-160">Настройка принадлежащих типов</span><span class="sxs-lookup"><span data-stu-id="d4ab6-160">Configuring Owned Types</span></span>

<span data-ttu-id="d4ab6-161">Можно связать `OwnsOne` метод в вызове Fluent, чтобы настроить эту модель:</span><span class="sxs-lookup"><span data-stu-id="d4ab6-161">It is possible to chain the `OwnsOne` method in a fluent call to configure this model:</span></span>

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

<span data-ttu-id="d4ab6-162">Обратите внимание на `WithOwner` вызов, используемый для определения свойства навигации, указывающего назад у владельца.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-162">Notice the `WithOwner` call used to define the navigation property pointing back at the owner.</span></span> <span data-ttu-id="d4ab6-163">Чтобы определить навигацию для типа сущности владельца, которая не является частью отношения владения, `WithOwner()` следует вызывать без аргументов.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-163">To define a navigation to the owner entity type that's not part of the ownership relationship `WithOwner()` should be called without any arguments.</span></span>

<span data-ttu-id="d4ab6-164">Этот результат также можно достичь с помощью `OwnedAttribute` как для, так `OrderDetails` и для `StreetAddress` .</span><span class="sxs-lookup"><span data-stu-id="d4ab6-164">It is also possible to achieve this result using `OwnedAttribute` on both `OrderDetails` and `StreetAddress`.</span></span>

<span data-ttu-id="d4ab6-165">Кроме того, обратите внимание на `Navigation` вызов.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-165">In addition, notice the `Navigation` call.</span></span> <span data-ttu-id="d4ab6-166">В Ефкоре 5,0 свойства навигации для собственных типов можно настроить [так же, как для несобственных свойств навигации](xref:core/modeling/relationships#configuring-navigation-properties).</span><span class="sxs-lookup"><span data-stu-id="d4ab6-166">In EFCore 5.0, navigation properties to owned types can be further configured [as for non-owned navigation properties](xref:core/modeling/relationships#configuring-navigation-properties).</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="d4ab6-167">Хранение принадлежащих типов в отдельных таблицах</span><span class="sxs-lookup"><span data-stu-id="d4ab6-167">Storing owned types in separate tables</span></span>

<span data-ttu-id="d4ab6-168">Кроме того, в отличие от сложных типов EF6, принадлежащие типы могут храниться в отдельной таблице от владельца.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-168">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="d4ab6-169">Чтобы переопределить соглашение, которое сопоставляет принадлежащий тип с той же таблицей, что и владелец, можно просто вызвать метод `ToTable` и указать другое имя таблицы.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-169">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="d4ab6-170">В следующем примере сопоставляется `OrderDetails` и его два адреса в отдельную таблицу из `DetailedOrder` :</span><span class="sxs-lookup"><span data-stu-id="d4ab6-170">The following example will map `OrderDetails` and its two addresses to a separate table from `DetailedOrder`:</span></span>

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

<span data-ttu-id="d4ab6-171">Можно также использовать `TableAttribute` для решения этой задачи, но обратите внимание, что это приведет к сбою, если существует несколько переходов к принадлежащему типу, так как в этом случае несколько типов сущностей будут сопоставлены с одной и той же таблицей.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-171">It is also possible to use the `TableAttribute` to accomplish this, but note that this would fail if there are multiple navigations to the owned type since in that case multiple entity types would be mapped to the same table.</span></span>

## <a name="querying-owned-types"></a><span data-ttu-id="d4ab6-172">Запросы к собственным типам</span><span class="sxs-lookup"><span data-stu-id="d4ab6-172">Querying owned types</span></span>

<span data-ttu-id="d4ab6-173">При запросе владельца принадлежащие типы включаются по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-173">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="d4ab6-174">Не обязательно использовать `Include` метод, даже если принадлежащие типы хранятся в отдельной таблице.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-174">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="d4ab6-175">В зависимости от модели, описанной ранее, следующий запрос получает `Order` `OrderDetails` и два, принадлежащие `StreetAddresses` базе данных:</span><span class="sxs-lookup"><span data-stu-id="d4ab6-175">Based on the model described before, the following query will get `Order`, `OrderDetails` and the two owned `StreetAddresses` from the database:</span></span>

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a><span data-ttu-id="d4ab6-176">Ограничения</span><span class="sxs-lookup"><span data-stu-id="d4ab6-176">Limitations</span></span>

<span data-ttu-id="d4ab6-177">Некоторые из этих ограничений основаны на принципах работы собственных типов сущностей, но некоторые другие являются ограничениями, которые можно удалить в следующих выпусках:</span><span class="sxs-lookup"><span data-stu-id="d4ab6-177">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="d4ab6-178">Ограничения по проектированию</span><span class="sxs-lookup"><span data-stu-id="d4ab6-178">By-design restrictions</span></span>

- <span data-ttu-id="d4ab6-179">Нельзя создать `DbSet<T>` для принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-179">You cannot create a `DbSet<T>` for an owned type.</span></span>
- <span data-ttu-id="d4ab6-180">Нельзя вызвать `Entity<T>()` с собственным типом в `ModelBuilder` .</span><span class="sxs-lookup"><span data-stu-id="d4ab6-180">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`.</span></span>
- <span data-ttu-id="d4ab6-181">Экземпляры собственных типов сущностей не могут совместно использоваться несколькими владельцами (это хорошо известный сценарий для объектов значений, которые не могут быть реализованы с помощью собственных типов сущностей).</span><span class="sxs-lookup"><span data-stu-id="d4ab6-181">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types).</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="d4ab6-182">Текущие недостатки</span><span class="sxs-lookup"><span data-stu-id="d4ab6-182">Current shortcomings</span></span>

- <span data-ttu-id="d4ab6-183">Собственные типы сущностей не могут иметь иерархии наследования</span><span class="sxs-lookup"><span data-stu-id="d4ab6-183">Owned entity types cannot have inheritance hierarchies</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="d4ab6-184">Недостатки в предыдущих версиях</span><span class="sxs-lookup"><span data-stu-id="d4ab6-184">Shortcomings in previous versions</span></span>

- <span data-ttu-id="d4ab6-185">В EF Core 2. x навигационные ссылки на типы собственных сущностей не могут иметь значение null, если они явно не сопоставлены с отдельной таблицей от владельца.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-185">In EF Core 2.x reference navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner.</span></span>
- <span data-ttu-id="d4ab6-186">В EF Core 3. x столбцы для собственных типов сущностей, сопоставленные с той же таблицей, что и владелец, всегда помечаются как допускающие значение null.</span><span class="sxs-lookup"><span data-stu-id="d4ab6-186">In EF Core 3.x the columns for owned entity types mapped to the same table as the owner are always marked as nullable.</span></span>
