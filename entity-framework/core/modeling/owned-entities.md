---
title: Собственные типы сущностей — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: b2d72b08de79939904bf4e726c695440c906a8aa
ms.sourcegitcommit: 7bde8e6ad3c4565a4638646ce04bcf5e66f7b5fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/07/2019
ms.locfileid: "54069208"
---
# <a name="owned-entity-types"></a><span data-ttu-id="ebfb4-102">Собственные типы сущностей</span><span class="sxs-lookup"><span data-stu-id="ebfb4-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="ebfb4-103">Этот компонент впервые появился в EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="ebfb4-104">EF Core позволяет модели типы сущностей, которые могут появляться только когда-либо других типов сущностей свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="ebfb4-105">Они называются _собственные типы сущностей_.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-105">These are called _owned entity types_.</span></span> <span data-ttu-id="ebfb4-106">Сущность, содержащая принадлежащий Тип сущности является его _владельца_.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="ebfb4-107">Явной настройки</span><span class="sxs-lookup"><span data-stu-id="ebfb4-107">Explicit configuration</span></span>

<span data-ttu-id="ebfb4-108">Объект, к которому типы никогда не включаются по EF Core в модели по соглашению, которым владеет.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="ebfb4-109">Можно использовать `OwnsOne` метод в `OnModelCreating` или указать для типа `OwnedAttribute` (новое в EF Core 2.1) для настройки типа как принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="ebfb4-110">В этом примере `StreetAddress` — это тип с помощью нет свойство identity.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-110">In this example, `StreetAddress` is a type with no identity property.</span></span> <span data-ttu-id="ebfb4-111">Он используется как свойство сущности Order для указания адреса доставки конкретного заказа.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span>

<span data-ttu-id="ebfb4-112">Мы можем использовать `OwnedAttribute` будет считаться принадлежит сущности при обращении к от другого типа сущности:</span><span class="sxs-lookup"><span data-stu-id="ebfb4-112">We can use the `OwnedAttribute` to treat it as an owned entity when referenced from another entity type:</span></span>

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

<span data-ttu-id="ebfb4-113">Можно также использовать `OwnsOne` метод в `OnModelCreating` позволяет указать, что `ShippingAddress` свойство — это сущность принадлежит `Order` типа сущности и настроить дополнительные аспекты, при необходимости.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-113">It is also possible to use the `OwnsOne` method in `OnModelCreating` to specify that the `ShippingAddress` property is an Owned Entity of the `Order` entity type and to configure additional facets if needed.</span></span>

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

<span data-ttu-id="ebfb4-114">Если `ShippingAddress` свойство является закрытым в `Order` типа, можно использовать строковую версию `OwnsOne` метод:</span><span class="sxs-lookup"><span data-stu-id="ebfb4-114">If the `ShippingAddress` property is private in the `Order` type, you can use the string version of the `OwnsOne` method:</span></span>

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

<span data-ttu-id="ebfb4-115">См. в разделе [полный пример проекта](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) дополнительный контекст.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-115">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) for more context.</span></span> 

## <a name="implicit-keys"></a><span data-ttu-id="ebfb4-116">Неявные ключи</span><span class="sxs-lookup"><span data-stu-id="ebfb4-116">Implicit keys</span></span>

<span data-ttu-id="ebfb4-117">Принадлежащие типы настроены `OwnsOne` или обнаруженные с помощью навигации ссылки на всегда имеют взаимно-однозначной связи с владельцем, поэтому им не нужны свои собственные значения ключа, так как значения внешних ключей являются уникальными.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-117">Owned types configured with `OwnsOne` or discovered through a reference navigation always have a one-to-one relationship with the owner, therefore they don't need their own key values as the foreign key values are unique.</span></span> <span data-ttu-id="ebfb4-118">В предыдущем примере `StreetAddress` типа не определяет ключевое свойство.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-118">In the previous example, the `StreetAddress` type does not need to define a key property.</span></span>  

<span data-ttu-id="ebfb4-119">Чтобы понять, как EF Core отслеживает эти объекты, имеет смысл подумать, что первичный ключ создается как [затемнения свойства](xref:core/modeling/shadow-properties) для принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-119">In order to understand how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="ebfb4-120">Значение ключа экземпляра принадлежащего типа будет таким же, как значение ключа владельца экземпляра.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-120">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>

## <a name="collections-of-owned-types"></a><span data-ttu-id="ebfb4-121">Коллекциями принадлежащих типов</span><span class="sxs-lookup"><span data-stu-id="ebfb4-121">Collections of owned types</span></span>

>[!NOTE]
> <span data-ttu-id="ebfb4-122">Это новая функция в EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-122">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="ebfb4-123">Чтобы настроить коллекцию принадлежащие типы `OwnsMany` должны использоваться в `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-123">To configure a collection of owned types `OwnsMany` should be used in `OnModelCreating`.</span></span> <span data-ttu-id="ebfb4-124">Тем не менее первичный ключ не будет настроен по соглашению, поэтому оно должно быть указано явным образом.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-124">However the primary key will not be configured by convention, so it needs to be specified explicitly.</span></span> <span data-ttu-id="ebfb4-125">Это часто используется сложные ключи для этих типов сущностей, включение внешний ключ к владельцу и дополнительным уникальным свойством, также может быть в состоянии тени:</span><span class="sxs-lookup"><span data-stu-id="ebfb4-125">It is common to use a complex key for these type of entities incorporating the foreign key to the owner and an additional unique property that can also be in shadow state:</span></span>

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="ebfb4-126">Сопоставление, принадлежащие типы с помощью разбиения таблицы</span><span class="sxs-lookup"><span data-stu-id="ebfb4-126">Mapping owned types with table splitting</span></span>

<span data-ttu-id="ebfb4-127">При использовании реляционных баз данных, по ссылке соглашению принадлежащие типы сопоставляются с той же таблице, как владелец.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-127">When using relational databases, by convention reference owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="ebfb4-128">Для этого требуется разделение таблицы в двух: некоторые столбцы будут использоваться для хранения данных, владельца, а некоторые столбцы будут использоваться для хранения данных, принадлежащий сущности.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-128">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="ebfb4-129">Это общие возможности, известной как разделение таблицы.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-129">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="ebfb4-130">Принадлежащие типы, хранимые с помощью разбиения таблицы можно использовать аналогично сложным типам используются в EF6.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-130">Owned types stored with table splitting can be used similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="ebfb4-131">По соглашению EF Core будет имен столбцов базы данных для свойств типа принадлежащих сущностей следующий шаблон _Navigation_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-131">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _Navigation_OwnedEntityProperty_.</span></span> <span data-ttu-id="ebfb4-132">Таким образом `StreetAddress` свойства будут отображаться в таблице «Заказы» с именами «ShippingAddress_Street» и «ShippingAddress_City».</span><span class="sxs-lookup"><span data-stu-id="ebfb4-132">Therefore the `StreetAddress` properties will appear in the 'Orders' table with the names 'ShippingAddress_Street' and 'ShippingAddress_City'.</span></span>

<span data-ttu-id="ebfb4-133">После добавления `HasColumnName` метод переименовать эти столбцы:</span><span class="sxs-lookup"><span data-stu-id="ebfb4-133">You can append the `HasColumnName` method to rename those columns:</span></span>

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="ebfb4-134">Совместное использование одного типа .NET среди нескольких принадлежащие типы</span><span class="sxs-lookup"><span data-stu-id="ebfb4-134">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="ebfb4-135">Принадлежащий Тип сущности может быть того же типа .NET как другим принадлежащим типом сущности, поэтому тип .NET может оказаться недостаточно для определения принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-135">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="ebfb4-136">В таких случаях свойство, указывающее от владельца принадлежащих сущностей становится _задания перехода_ типа принадлежащих сущностей.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-136">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="ebfb4-137">С точки зрения EF Core определяющую навигацию является частью идентификатора типа вместе с типом .NET.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-137">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="ebfb4-138">Например, в следующем классе `ShippingAddress` и `BillingAddress` имеют тот же тип .NET, `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="ebfb4-138">For example, in the following class `ShippingAddress` and `BillingAddress` are both of the same .NET type, `StreetAddress`:</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="ebfb4-139">Чтобы понять, как EF Core позволит отличить отслеживаемые экземпляры этих объектов, может быть полезно подумать, что определение навигации стал частью ключа экземпляра со значением ключа владельца и тип .NET, принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-139">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="ebfb4-140">Вложенные принадлежащие типы</span><span class="sxs-lookup"><span data-stu-id="ebfb4-140">Nested owned types</span></span>

<span data-ttu-id="ebfb4-141">В этом примере `OrderDetails` владеет `BillingAddress` и `ShippingAddress`, которые принадлежат `StreetAddress` типов.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-141">In this example `OrderDetails` owns `BillingAddress` and `ShippingAddress`, which are both `StreetAddress` types.</span></span> <span data-ttu-id="ebfb4-142">А `OrderDetails`, в свою очередь, принадлежит типу `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-142">Then `OrderDetails` is owned by the `DetailedOrder` type.</span></span>

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

<span data-ttu-id="ebfb4-143">В дополнение к вложенные принадлежащие типы принадлежащий тип может ссылаться на обычной сущности, это может быть владельцем или другую сущность, до тех пор, пока принадлежащих сущностей является зависимой стороны.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-143">In addition to nested owned types, an owned type can reference a regular entity, it can be either the owner or a different entity as long as the owned entity is on the dependent side.</span></span> <span data-ttu-id="ebfb4-144">Эта возможность устанавливает собственные типы сущностей помимо сложных типов в EF6.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="ebfb4-145">Существует возможность цепочки `OwnsOne` метод при вызове fluent для настройки этой модели:</span><span class="sxs-lookup"><span data-stu-id="ebfb4-145">It is possible to chain the `OwnsOne` method in a fluent call to configure this model:</span></span>

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

<span data-ttu-id="ebfb4-146">Это также можно добиться той же самое с использованием `OwnedAttribute` на обоих `OrderDetails` и `StreetAdress`.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-146">It is also possible to achieve the same thing using `OwnedAttribute` on both `OrderDetails` and `StreetAdress`.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="ebfb4-147">Хранение принадлежащие типы в отдельных таблицах</span><span class="sxs-lookup"><span data-stu-id="ebfb4-147">Storing owned types in separate tables</span></span>

<span data-ttu-id="ebfb4-148">Также в отличие от сложных типов EF6, принадлежащие типы могут храниться в отдельной таблице от владельца.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-148">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="ebfb4-149">Чтобы переопределить соглашение, которое сопоставляется с той же таблице, как владелец принадлежащих типов, можно просто вызвать `ToTable` и укажите другое имя таблицы.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-149">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="ebfb4-150">Следующий пример сопоставит `OrderDetails` и двумя адресами отдельной таблицы из `DetailedOrder`:</span><span class="sxs-lookup"><span data-stu-id="ebfb4-150">The following example will map `OrderDetails` and its two addresses to a separate table from `DetailedOrder`:</span></span>

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a><span data-ttu-id="ebfb4-151">Запрос принадлежащие типы</span><span class="sxs-lookup"><span data-stu-id="ebfb4-151">Querying owned types</span></span>

<span data-ttu-id="ebfb4-152">При запросе владельца принадлежащие типы включаются по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-152">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="ebfb4-153">Нет необходимости использовать `Include` метод, даже если принадлежащие типы хранятся в отдельной таблице.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-153">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="ebfb4-154">Исходя из модели, описанной выше, следующий запрос получает `Order`, `OrderDetails` и два принадлежит `StreetAddresses` из базы данных:</span><span class="sxs-lookup"><span data-stu-id="ebfb4-154">Based on the model described before, the following query will get `Order`, `OrderDetails` and the two owned `StreetAddresses` from the database:</span></span>

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a><span data-ttu-id="ebfb4-155">Ограничения</span><span class="sxs-lookup"><span data-stu-id="ebfb4-155">Limitations</span></span>

<span data-ttu-id="ebfb4-156">Некоторые из этих ограничений лежат в основе как принадлежащий рабочих типов сущности, но некоторые другие являются ограничения, что мы можно удалить в последующих выпусках:</span><span class="sxs-lookup"><span data-stu-id="ebfb4-156">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="ebfb4-157">Ограничения нормальное поведение</span><span class="sxs-lookup"><span data-stu-id="ebfb4-157">By-design restrictions</span></span>
- <span data-ttu-id="ebfb4-158">Не удается создать `DbSet<T>` для принадлежащего типа</span><span class="sxs-lookup"><span data-stu-id="ebfb4-158">You cannot create a `DbSet<T>` for an owned type</span></span>
- <span data-ttu-id="ebfb4-159">Нельзя вызывать `Entity<T>()` с принадлежащего типа в `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="ebfb4-159">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="ebfb4-160">Нынешних изъянов</span><span class="sxs-lookup"><span data-stu-id="ebfb4-160">Current shortcomings</span></span>
- <span data-ttu-id="ebfb4-161">Иерархии наследования, содержащие принадлежащих типов сущностей, не поддерживаются</span><span class="sxs-lookup"><span data-stu-id="ebfb4-161">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="ebfb4-162">Справочник по переходы на принадлежащие типы сущностей не может быть null, если они явным образом сопоставлены с отдельной таблицей от владельца</span><span class="sxs-lookup"><span data-stu-id="ebfb4-162">Reference navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="ebfb4-163">Экземпляров принадлежащих типов сущностей, которые не могут совместно использоваться несколько владельцев (это хорошо известных сценарий для объектов значений, которые не может быть реализован с помощью принадлежащих типов сущностей)</span><span class="sxs-lookup"><span data-stu-id="ebfb4-163">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="ebfb4-164">Недостатки в предыдущих версиях</span><span class="sxs-lookup"><span data-stu-id="ebfb4-164">Shortcomings in previous versions</span></span>
- <span data-ttu-id="ebfb4-165">В EF Core 2.0 принадлежащие переходы на типы сущностей не может объявляться в производные типы сущностей, если собственные типы сущностей, явным образом сопоставляются отдельную таблицу из иерархии владельца.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-165">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="ebfb4-166">Это ограничение было снято в EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="ebfb4-166">This limitation has been removed in EF Core 2.1</span></span>
- <span data-ttu-id="ebfb4-167">В EF Core 2.0 и 2.1 только одну ссылку поддерживались переходы на принадлежащие типы.</span><span class="sxs-lookup"><span data-stu-id="ebfb4-167">In EF Core 2.0 and 2.1 only reference navigations to owned types were supported.</span></span> <span data-ttu-id="ebfb4-168">Это ограничение было снято в EF Core 2.2</span><span class="sxs-lookup"><span data-stu-id="ebfb4-168">This limitation has been removed in EF Core 2.2</span></span>
