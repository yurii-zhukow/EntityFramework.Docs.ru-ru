---
title: "Собственные типы сущностей — EF Core"
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: a6823377eb626ca92263c31351e1aef61db5a787
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="owned-entity-types"></a><span data-ttu-id="ffb1b-102">Типы сущностей, принадлежащие</span><span class="sxs-lookup"><span data-stu-id="ffb1b-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="ffb1b-103">Этот компонент впервые появился в EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="ffb1b-104">EF ядра позволяет модели типы сущностей, появляющихся только в свойствах навигации других типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="ffb1b-105">Это называется _собственные типы сущностей_.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-105">These are called _owned entity types_.</span></span> <span data-ttu-id="ffb1b-106">Сущность, содержащая тип сущности, принадлежащие является его _владельца_.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="ffb1b-107">Явной настройки</span><span class="sxs-lookup"><span data-stu-id="ffb1b-107">Explicit configuration</span></span>

<span data-ttu-id="ffb1b-108">Объект, к которому типы никогда не включаются ядром EF в модели по соглашению, которым владеет.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="ffb1b-109">Можно использовать `OwnsOne` метод в `OnModelCreating` или указать для типа `OwnedAttrbibute` (новые возможности EF Core 2.1) чтобы настроить тип как тип владельца.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttrbibute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="ffb1b-110">В этом примере StreetAddress — это тип с нет свойства identity.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-110">In this example, StreetAddress is a type with no identity property.</span></span> <span data-ttu-id="ffb1b-111">Он используется как свойство сущности Order для указания адреса доставки конкретного заказа.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span> <span data-ttu-id="ffb1b-112">В `OnModelCreating`, мы используем `OwnsOne` метод, чтобы указать, что свойство ShippingAddress является принадлежащий сущности типа Order.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-112">In `OnModelCreating`, we use the `OwnsOne` method to specify that the ShippingAddress property is an Owned Entity of the Order type.</span></span>

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

<span data-ttu-id="ffb1b-113">Если свойство ShippingAddress является закрытым в типе заказа, можно использовать строковую версию `OwnsOne` метод:</span><span class="sxs-lookup"><span data-stu-id="ffb1b-113">If the ShippingAddress property is private in the Order type, you can use the string version of the `OwnsOne` method:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

<span data-ttu-id="ffb1b-114">В этом примере мы используем `OwnedAttribute` для достижения общей цели:</span><span class="sxs-lookup"><span data-stu-id="ffb1b-114">In this example, we use the `OwnedAttribute` to achieve the same goal:</span></span>

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

## <a name="implicit-keys"></a><span data-ttu-id="ffb1b-115">Неявные ключи</span><span class="sxs-lookup"><span data-stu-id="ffb1b-115">Implicit keys</span></span>

<span data-ttu-id="ffb1b-116">EF Core 2.0 и 2.1 только свойства навигации ссылки можно указать собственные типы.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-116">In EF Core 2.0 and 2.1, only reference navigation properties can point to owned types.</span></span> <span data-ttu-id="ffb1b-117">Коллекции собственных типов не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-117">Collections of owned types are not supported.</span></span> <span data-ttu-id="ffb1b-118">Эти ссылки, которым владеет типы всегда имеют отношение с владельцем, поэтому они не должны собственные значения ключа.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-118">These reference owned types always have a one-to-one relationship with the owner, therefore they don't need their own key values.</span></span> <span data-ttu-id="ffb1b-119">В предыдущем примере тип StreetAddress необходимо определить свойство ключа.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-119">In the previous example, the StreetAddress type does not need to define a key property.</span></span>  

<span data-ttu-id="ffb1b-120">Чтобы понять, как EF ядра отслеживает эти объекты, имеет смысл подумать, что первичный ключ создается как [затемнять свойство](xref:core/modeling/shadow-properties) для типа владельца.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-120">In order to understanding how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="ffb1b-121">Значение ключа экземпляра владельца типа будет таким же, как значение ключа владельца экземпляра.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>      

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="ffb1b-122">Собственные типы с разделения таблицы сопоставления</span><span class="sxs-lookup"><span data-stu-id="ffb1b-122">Mapping owned types with table splitting</span></span>

<span data-ttu-id="ffb1b-123">При использовании реляционных баз данных, по соглашению, собственные типы сопоставляются с той же таблицы в качестве владельца.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-123">When using relational databases, by convention owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="ffb1b-124">Это требует разделения таблицы в двух: некоторые столбцы будут использоваться для хранения данных, владельца и некоторых столбцов будет использоваться для хранения данных, принадлежащий сущности.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-124">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="ffb1b-125">Это общие функции, известной как разделение таблицы.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-125">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="ffb1b-126">Собственные типы, хранимые с разделения таблицы можно использовать почти так же, как сложных типов используются в EF6.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-126">Owned types stored with table splitting can be used very similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="ffb1b-127">По соглашению EF Core присвоит имя столбцов базы данных для свойств типа сущности владельца, следуя шаблону _EntityProperty_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-127">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _EntityProperty_OwnedEntityProperty_.</span></span> <span data-ttu-id="ffb1b-128">Поэтому свойства StreetAddress будут отображаться в таблице Orders с именами ShippingAddress_Street и ShippingAddress_City.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-128">Therefore the StreetAddress properties will appear in the Orders table with the names ShippingAddress_Street and ShippingAddress_City.</span></span>

<span data-ttu-id="ffb1b-129">После добавления `HasColumnName` метод Переименование этих столбцов.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-129">You can append the `HasColumnName` method to rename those columns.</span></span> <span data-ttu-id="ffb1b-130">В случае, когда StreetAddress является открытым свойством будет сопоставления</span><span class="sxs-lookup"><span data-stu-id="ffb1b-130">In the case where StreetAddress is a public property, the mappings would be</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="ffb1b-131">Совместное использование .NET однотипные несколько собственных типов</span><span class="sxs-lookup"><span data-stu-id="ffb1b-131">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="ffb1b-132">Тип сущности, принадлежащие можно того же типа .NET другого типа сущности, принадлежащие таким образом тип .NET может оказаться недостаточно для идентификации типа владельца.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-132">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="ffb1b-133">В этих случаях становится свойство, указывающее от владельца владение субъекта _определение навигации_ типа принадлежащий сущности.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-133">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="ffb1b-134">С точки зрения EF Core определение переходов является частью идентификатора типа вместе с типом .NET.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-134">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="ffb1b-135">Например в следующем классе ShippingAddress и BillingAddress будут того же типа .NET, StreetAddress:</span><span class="sxs-lookup"><span data-stu-id="ffb1b-135">For example, in the following class, ShippingAddress and BillingAddress are both of the same .NET type, StreetAddress:</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

<span data-ttu-id="ffb1b-136">Чтобы понять, как основной EF, выделяет отслеживаемых экземпляров этих объектов, может быть полезно представить, что определение навигации стал частью ключа экземпляра со значением ключа владельца и .NET тип владельца типа.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-136">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="ffb1b-137">Вложенные типы, принадлежащие</span><span class="sxs-lookup"><span data-stu-id="ffb1b-137">Nested owned types</span></span>

<span data-ttu-id="ffb1b-138">В этом примере OrderDetails владеет BillingAddress и ShippingAddress, которые оба типа StreetAddress.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-138">In this example OrderDetails owns BillingAddress and ShippingAddress, which are both StreetAddress types.</span></span> <span data-ttu-id="ffb1b-139">Затем OrderDetails принадлежит тип заказа.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-139">Then OrderDetails is owned by the Order type.</span></span>

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

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

<span data-ttu-id="ffb1b-140">Это возможно в цепочке `OwnsOne` метод в fluent сопоставления для настройки этой модели:</span><span class="sxs-lookup"><span data-stu-id="ffb1b-140">It is possible to chain the `OwnsOne` method in a fluent mapping to configure this model:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

<span data-ttu-id="ffb1b-141">Можно добиться того же результата с помощью `OwnedAttribute` OrderDetails и StreetAdress.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-141">It is possible to achieve the same thing using `OwnedAttribute` on both OrderDetails and StreetAdress.</span></span>

<span data-ttu-id="ffb1b-142">Помимо владельца вложенные типы принадлежащие тип может ссылаться на обычной сущности.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-142">In addition to nested owned types, an owned type can reference a regular entity.</span></span> <span data-ttu-id="ffb1b-143">В следующем примере страной является обычной сущности (т. е. без владельца):</span><span class="sxs-lookup"><span data-stu-id="ffb1b-143">In the following example, Country is a regular (i.e. non-owned) entity:</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

<span data-ttu-id="ffb1b-144">Эта возможность устанавливает типы сущностей принадлежащие отдельно от сложных типов в EF6.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="ffb1b-145">Хранение собственные типы в отдельных таблицах</span><span class="sxs-lookup"><span data-stu-id="ffb1b-145">Storing owned types in separate tables</span></span>

<span data-ttu-id="ffb1b-146">Также в отличие от EF6 сложные типы, принадлежащие типов могут храниться в отдельной таблице от владельца.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-146">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="ffb1b-147">Чтобы переопределить соглашение, которое сопоставляет тип собственности на одну таблицу как владелец, можно просто вызов `ToTable` и укажите другое имя таблицы.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-147">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="ffb1b-148">Следующий пример будет сопоставлен OrderDetails и двумя адресами отдельную таблицу из заказа:</span><span class="sxs-lookup"><span data-stu-id="ffb1b-148">The following example will map OrderDetails and its two addresses to a separate table from Order:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a><span data-ttu-id="ffb1b-149">Собственные типы запросов</span><span class="sxs-lookup"><span data-stu-id="ffb1b-149">Querying owned types</span></span>

<span data-ttu-id="ffb1b-150">При запросе владельца принадлежащие типы включаются по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-150">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="ffb1b-151">Нет необходимости использовать `Include` метод, даже если собственных типов хранятся в отдельной таблице.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-151">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="ffb1b-152">Следующий запрос на основании модели, описанной выше, будет получать заказа, OrderDetails и два StreeAddresses владельца для всех ожидающих заказов из базы данных:</span><span class="sxs-lookup"><span data-stu-id="ffb1b-152">Based on the model described before, the following query will pull Order, OrderDetails and the two owned StreeAddresses for all pending orders from the database:</span></span>

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a><span data-ttu-id="ffb1b-153">Ограничения</span><span class="sxs-lookup"><span data-stu-id="ffb1b-153">Limitations</span></span>

<span data-ttu-id="ffb1b-154">Ниже приведены некоторые ограничения типов владение субъекта.</span><span class="sxs-lookup"><span data-stu-id="ffb1b-154">Here are some limitations of owned entity types.</span></span> <span data-ttu-id="ffb1b-155">Некоторые из этих ограничений лежат в основе работы как собственные типы, но некоторые другие — на момент ограничений, которые необходимо удалить в последующих выпусках:</span><span class="sxs-lookup"><span data-stu-id="ffb1b-155">Some of these limitations are fundamental to how owned types work, but some others are point-in-time restrictions that we would like to remove in future releases:</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="ffb1b-156">Текущий недостатки</span><span class="sxs-lookup"><span data-stu-id="ffb1b-156">Current shortcomings</span></span>
- <span data-ttu-id="ffb1b-157">Наследование из собственных типов не поддерживается</span><span class="sxs-lookup"><span data-stu-id="ffb1b-157">Inheritance of owned types is not supported</span></span>
- <span data-ttu-id="ffb1b-158">Собственные типы нельзя указывать с помощью свойства навигации коллекции</span><span class="sxs-lookup"><span data-stu-id="ffb1b-158">Owned types cannot be pointed at by a collection navigation property</span></span>
- <span data-ttu-id="ffb1b-159">Так как они используют счет разбиения таблиц, по умолчанию принадлежащие типы имеют следующие ограничения, если явно сопоставлены в другую таблицу:</span><span class="sxs-lookup"><span data-stu-id="ffb1b-159">Since they use table splitting by default owned types also have the following restrictions unless explicitly mapped to a different table:</span></span>
   - <span data-ttu-id="ffb1b-160">Они не может принадлежать производный тип</span><span class="sxs-lookup"><span data-stu-id="ffb1b-160">They cannot be owned by a derived type</span></span>
   - <span data-ttu-id="ffb1b-161">Определение свойства навигации нельзя присвоить значение null (т. е. собственные типы в той же таблице всегда обязательны)</span><span class="sxs-lookup"><span data-stu-id="ffb1b-161">The defining navigation property cannot be set to null (i.e. owned types on the same table are always required)</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="ffb1b-162">Ограничения по своей природе</span><span class="sxs-lookup"><span data-stu-id="ffb1b-162">By-design restrictions</span></span>
- <span data-ttu-id="ffb1b-163">Не удается создать `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="ffb1b-163">You cannot create a `DbSet<T>`</span></span>
- <span data-ttu-id="ffb1b-164">Не удается вызвать `Entity<T>()` с типом владельца на `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="ffb1b-164">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>
