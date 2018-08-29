---
title: Собственные типы сущностей — EF Core
author: julielerman
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: afbc853feab56d31a8ceba0da05fe6dc508b65bb
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994073"
---
# <a name="owned-entity-types"></a><span data-ttu-id="2df2f-102">Собственные типы сущностей</span><span class="sxs-lookup"><span data-stu-id="2df2f-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="2df2f-103">Этот компонент впервые появился в EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="2df2f-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="2df2f-104">EF Core позволяет модели типы сущностей, которые могут появляться только когда-либо других типов сущностей свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="2df2f-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="2df2f-105">Они называются _собственные типы сущностей_.</span><span class="sxs-lookup"><span data-stu-id="2df2f-105">These are called _owned entity types_.</span></span> <span data-ttu-id="2df2f-106">Сущность, содержащая принадлежащий Тип сущности является его _владельца_.</span><span class="sxs-lookup"><span data-stu-id="2df2f-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="2df2f-107">Явной настройки</span><span class="sxs-lookup"><span data-stu-id="2df2f-107">Explicit configuration</span></span>

<span data-ttu-id="2df2f-108">Объект, к которому типы никогда не включаются по EF Core в модели по соглашению, которым владеет.</span><span class="sxs-lookup"><span data-stu-id="2df2f-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="2df2f-109">Можно использовать `OwnsOne` метод в `OnModelCreating` или указать для типа `OwnedAttribute` (новое в EF Core 2.1) для настройки типа как принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="2df2f-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="2df2f-110">В этом примере StreetAddress — это тип с помощью нет свойство identity.</span><span class="sxs-lookup"><span data-stu-id="2df2f-110">In this example, StreetAddress is a type with no identity property.</span></span> <span data-ttu-id="2df2f-111">Он используется как свойство сущности Order для указания адреса доставки конкретного заказа.</span><span class="sxs-lookup"><span data-stu-id="2df2f-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span> <span data-ttu-id="2df2f-112">В `OnModelCreating`, мы используем `OwnsOne` метод, чтобы указать, что свойство ShippingAddress принадлежит сущности типа Order.</span><span class="sxs-lookup"><span data-stu-id="2df2f-112">In `OnModelCreating`, we use the `OwnsOne` method to specify that the ShippingAddress property is an Owned Entity of the Order type.</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

// OnModelCreating
modelBuilder.Entity<Order>().OwnsOne(p => p.ShippingAddress);
```

<span data-ttu-id="2df2f-113">Если свойство ShippingAddress является закрытым в типе Order, можно использовать строковую версию `OwnsOne` метод:</span><span class="sxs-lookup"><span data-stu-id="2df2f-113">If the ShippingAddress property is private in the Order type, you can use the string version of the `OwnsOne` method:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

<span data-ttu-id="2df2f-114">В этом примере мы используем `OwnedAttribute` для достижения одной цели:</span><span class="sxs-lookup"><span data-stu-id="2df2f-114">In this example, we use the `OwnedAttribute` to achieve the same goal:</span></span>

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="implicit-keys"></a><span data-ttu-id="2df2f-115">Неявные ключи</span><span class="sxs-lookup"><span data-stu-id="2df2f-115">Implicit keys</span></span>

<span data-ttu-id="2df2f-116">В EF Core 2.0 и 2.1 принадлежащие типы можно указать только свойства навигации по ссылке.</span><span class="sxs-lookup"><span data-stu-id="2df2f-116">In EF Core 2.0 and 2.1, only reference navigation properties can point to owned types.</span></span> <span data-ttu-id="2df2f-117">Коллекциями принадлежащих типов не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="2df2f-117">Collections of owned types are not supported.</span></span> <span data-ttu-id="2df2f-118">Эти ссылки, которым владеет типы всегда иметь взаимно-однозначной связи с владельцем, поэтому им не нужны собственные значения ключа.</span><span class="sxs-lookup"><span data-stu-id="2df2f-118">These reference owned types always have a one-to-one relationship with the owner, therefore they don't need their own key values.</span></span> <span data-ttu-id="2df2f-119">В предыдущем примере тип StreetAddress не определяет ключевое свойство.</span><span class="sxs-lookup"><span data-stu-id="2df2f-119">In the previous example, the StreetAddress type does not need to define a key property.</span></span>  

<span data-ttu-id="2df2f-120">Чтобы понять, как EF Core отслеживает эти объекты, имеет смысл подумать, что первичный ключ создается как [затемнения свойства](xref:core/modeling/shadow-properties) для принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="2df2f-120">In order to understand how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="2df2f-121">Значение ключа экземпляра принадлежащего типа будет таким же, как значение ключа владельца экземпляра.</span><span class="sxs-lookup"><span data-stu-id="2df2f-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>      

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="2df2f-122">Сопоставление, принадлежащие типы с помощью разбиения таблицы</span><span class="sxs-lookup"><span data-stu-id="2df2f-122">Mapping owned types with table splitting</span></span>

<span data-ttu-id="2df2f-123">При использовании реляционных баз данных, по соглашению принадлежащие типы сопоставляются с той же таблице, как владелец.</span><span class="sxs-lookup"><span data-stu-id="2df2f-123">When using relational databases, by convention owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="2df2f-124">Для этого требуется разделение таблицы в двух: некоторые столбцы будут использоваться для хранения данных, владельца, а некоторые столбцы будут использоваться для хранения данных, принадлежащий сущности.</span><span class="sxs-lookup"><span data-stu-id="2df2f-124">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="2df2f-125">Это общие возможности, известной как разделение таблицы.</span><span class="sxs-lookup"><span data-stu-id="2df2f-125">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="2df2f-126">Принадлежащие типы, хранимые с помощью разбиения таблицы можно использовать почти так же, как сложные типы используются в EF6.</span><span class="sxs-lookup"><span data-stu-id="2df2f-126">Owned types stored with table splitting can be used very similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="2df2f-127">По соглашению EF Core будет имен столбцов базы данных для свойств типа принадлежащих сущностей следующий шаблон _EntityProperty_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="2df2f-127">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _EntityProperty_OwnedEntityProperty_.</span></span> <span data-ttu-id="2df2f-128">Таким образом StreetAddress свойства отобразятся в таблице Orders с именами ShippingAddress_Street и ShippingAddress_City.</span><span class="sxs-lookup"><span data-stu-id="2df2f-128">Therefore the StreetAddress properties will appear in the Orders table with the names ShippingAddress_Street and ShippingAddress_City.</span></span>

<span data-ttu-id="2df2f-129">После добавления `HasColumnName` метод переименовать эти столбцы.</span><span class="sxs-lookup"><span data-stu-id="2df2f-129">You can append the `HasColumnName` method to rename those columns.</span></span> <span data-ttu-id="2df2f-130">В случае, когда StreetAddress является открытым свойством сопоставление будет выполняться</span><span class="sxs-lookup"><span data-stu-id="2df2f-130">In the case where StreetAddress is a public property, the mappings would be</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="2df2f-131">Совместное использование одного типа .NET среди нескольких принадлежащие типы</span><span class="sxs-lookup"><span data-stu-id="2df2f-131">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="2df2f-132">Принадлежащий Тип сущности может быть того же типа .NET как другим принадлежащим типом сущности, поэтому тип .NET может оказаться недостаточно для определения принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="2df2f-132">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="2df2f-133">В таких случаях свойство, указывающее от владельца принадлежащих сущностей становится _задания перехода_ типа принадлежащих сущностей.</span><span class="sxs-lookup"><span data-stu-id="2df2f-133">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="2df2f-134">С точки зрения EF Core определяющую навигацию является частью идентификатора типа вместе с типом .NET.</span><span class="sxs-lookup"><span data-stu-id="2df2f-134">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="2df2f-135">Например в следующем классе ShippingAddress и BillingAddress имеют тот же тип .NET, StreetAddress:</span><span class="sxs-lookup"><span data-stu-id="2df2f-135">For example, in the following class, ShippingAddress and BillingAddress are both of the same .NET type, StreetAddress:</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

<span data-ttu-id="2df2f-136">Чтобы понять, как EF Core позволит отличить отслеживаемые экземпляры этих объектов, может быть полезно подумать, что определение навигации стал частью ключа экземпляра со значением ключа владельца и тип .NET, принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="2df2f-136">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="2df2f-137">Вложенные принадлежащие типы</span><span class="sxs-lookup"><span data-stu-id="2df2f-137">Nested owned types</span></span>

<span data-ttu-id="2df2f-138">В этом примере OrderDetails владеет BillingAddress и ShippingAddress, который представляют собой типы StreetAddress.</span><span class="sxs-lookup"><span data-stu-id="2df2f-138">In this example OrderDetails owns BillingAddress and ShippingAddress, which are both StreetAddress types.</span></span> <span data-ttu-id="2df2f-139">Затем OrderDetails принадлежит тип.</span><span class="sxs-lookup"><span data-stu-id="2df2f-139">Then OrderDetails is owned by the Order type.</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
    public OrderStatus Status { get; set; }
}

public class OrderDetails
{
    public StreetAddress BillingAddress { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

public enum OrderStatus
{
    Pending,
    Shipped
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

<span data-ttu-id="2df2f-140">Существует возможность цепочки `OwnsOne` метод текучего сопоставления для настройки этой модели:</span><span class="sxs-lookup"><span data-stu-id="2df2f-140">It is possible to chain the `OwnsOne` method in a fluent mapping to configure this model:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

<span data-ttu-id="2df2f-141">Это можно добиться той же самое с использованием `OwnedAttribute` OrderDetails и StreetAdress.</span><span class="sxs-lookup"><span data-stu-id="2df2f-141">It is possible to achieve the same thing using `OwnedAttribute` on both OrderDetails and StreetAdress.</span></span>

<span data-ttu-id="2df2f-142">В дополнение к вложенные принадлежащие типы принадлежащий тип может ссылаться на обычной сущности.</span><span class="sxs-lookup"><span data-stu-id="2df2f-142">In addition to nested owned types, an owned type can reference a regular entity.</span></span> <span data-ttu-id="2df2f-143">В следующем примере выбранной страной является обычной сущности, не принадлежащими:</span><span class="sxs-lookup"><span data-stu-id="2df2f-143">In the following example, Country is a regular non-owned entity:</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

<span data-ttu-id="2df2f-144">Эта возможность устанавливает собственные типы сущностей помимо сложных типов в EF6.</span><span class="sxs-lookup"><span data-stu-id="2df2f-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="2df2f-145">Хранение принадлежащие типы в отдельных таблицах</span><span class="sxs-lookup"><span data-stu-id="2df2f-145">Storing owned types in separate tables</span></span>

<span data-ttu-id="2df2f-146">Также в отличие от сложных типов EF6, принадлежащие типы могут храниться в отдельной таблице от владельца.</span><span class="sxs-lookup"><span data-stu-id="2df2f-146">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="2df2f-147">Чтобы переопределить соглашение, которое сопоставляется с той же таблице, как владелец принадлежащих типов, можно просто вызвать `ToTable` и укажите другое имя таблицы.</span><span class="sxs-lookup"><span data-stu-id="2df2f-147">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="2df2f-148">Следующий пример будет сопоставлен OrderDetails и двумя адресами отдельную таблицу из заказа:</span><span class="sxs-lookup"><span data-stu-id="2df2f-148">The following example will map OrderDetails and its two addresses to a separate table from Order:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
        od.ToTable("OrderDetails");
    });
```

## <a name="querying-owned-types"></a><span data-ttu-id="2df2f-149">Запрос принадлежащие типы</span><span class="sxs-lookup"><span data-stu-id="2df2f-149">Querying owned types</span></span>

<span data-ttu-id="2df2f-150">При запросе владельца принадлежащие типы включаются по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2df2f-150">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="2df2f-151">Нет необходимости использовать `Include` метод, даже если принадлежащие типы хранятся в отдельной таблице.</span><span class="sxs-lookup"><span data-stu-id="2df2f-151">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="2df2f-152">Исходя из модели, описанной выше, следующий запрос извлечет, заказа, OrderDetails и два собственных StreetAddresses все ожидающие заказы из базы данных:</span><span class="sxs-lookup"><span data-stu-id="2df2f-152">Based on the model described before, the following query will pull Order, OrderDetails and the two owned StreetAddresses for all pending orders from the database:</span></span>

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a><span data-ttu-id="2df2f-153">Ограничения</span><span class="sxs-lookup"><span data-stu-id="2df2f-153">Limitations</span></span>

<span data-ttu-id="2df2f-154">Некоторые из этих ограничений лежат в основе как принадлежащий рабочих типов сущности, но некоторые другие являются ограничения, что мы можно удалить в последующих выпусках:</span><span class="sxs-lookup"><span data-stu-id="2df2f-154">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="2df2f-155">Недостатки в предыдущих версиях</span><span class="sxs-lookup"><span data-stu-id="2df2f-155">Shortcomings in previous versions</span></span>
- <span data-ttu-id="2df2f-156">В EF Core 2.0 принадлежащие переходы на типы сущностей не может объявляться в производные типы сущностей, если собственные типы сущностей, явным образом сопоставляются отдельную таблицу из иерархии владельца.</span><span class="sxs-lookup"><span data-stu-id="2df2f-156">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="2df2f-157">Это ограничение было снято в EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="2df2f-157">This limitation has been removed in EF Core 2.1</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="2df2f-158">Нынешних изъянов</span><span class="sxs-lookup"><span data-stu-id="2df2f-158">Current shortcomings</span></span>
- <span data-ttu-id="2df2f-159">Иерархии наследования, содержащие принадлежащих типов сущностей, не поддерживаются</span><span class="sxs-lookup"><span data-stu-id="2df2f-159">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="2df2f-160">Собственные типы сущностей не может быть, на который ссылается свойство навигации коллекции (только одну ссылку переходы в настоящее время поддерживаются)</span><span class="sxs-lookup"><span data-stu-id="2df2f-160">Owned entity types cannot be pointed at by a collection navigation property (only reference navigations are currently supported)</span></span>
- <span data-ttu-id="2df2f-161">Переходы на собственных типов сущностей не может быть null, если они явным образом сопоставлены с отдельной таблицей от владельца</span><span class="sxs-lookup"><span data-stu-id="2df2f-161">Navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="2df2f-162">Экземпляров принадлежащих типов сущностей, которые не могут совместно использоваться несколько владельцев (это хорошо известных сценарий для объектов значений, которые не может быть реализован с помощью принадлежащих типов сущностей)</span><span class="sxs-lookup"><span data-stu-id="2df2f-162">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="2df2f-163">Ограничения нормальное поведение</span><span class="sxs-lookup"><span data-stu-id="2df2f-163">By-design restrictions</span></span>
- <span data-ttu-id="2df2f-164">Не удается создать `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="2df2f-164">You cannot create a `DbSet<T>`</span></span>
- <span data-ttu-id="2df2f-165">Нельзя вызывать `Entity<T>()` с принадлежащего типа в `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="2df2f-165">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>
