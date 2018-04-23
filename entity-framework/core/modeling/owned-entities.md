---
title: Собственные типы сущностей — EF Core
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: f2f05499a3e3494f420d916df2db19667a6f1e29
ms.sourcegitcommit: 26f33758c47399ae933f22fec8e1d19fa7d2c0b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/19/2018
---
# <a name="owned-entity-types"></a>Типы сущностей, принадлежащие

>[!NOTE]
> Этот компонент впервые появился в EF Core 2.0.

EF ядра позволяет модели типы сущностей, появляющихся только в свойствах навигации других типов сущностей. Это называется _собственные типы сущностей_. Сущность, содержащая тип сущности, принадлежащие является его _владельца_.

## <a name="explicit-configuration"></a>Явной настройки

Объект, к которому типы никогда не включаются ядром EF в модели по соглашению, которым владеет. Можно использовать `OwnsOne` метод в `OnModelCreating` или указать для типа `OwnedAttribute` (новые возможности EF Core 2.1) чтобы настроить тип как тип владельца.

В этом примере StreetAddress — это тип с нет свойства identity. Он используется как свойство сущности Order для указания адреса доставки конкретного заказа. В `OnModelCreating`, мы используем `OwnsOne` метод, чтобы указать, что свойство ShippingAddress является принадлежащий сущности типа Order.

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

Если свойство ShippingAddress является закрытым в типе заказа, можно использовать строковую версию `OwnsOne` метод:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

В этом примере мы используем `OwnedAttribute` для достижения общей цели:

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

## <a name="implicit-keys"></a>Неявные ключи

EF Core 2.0 и 2.1 только свойства навигации ссылки можно указать собственные типы. Коллекции собственных типов не поддерживаются. Эти ссылки, которым владеет типы всегда имеют отношение с владельцем, поэтому они не должны собственные значения ключа. В предыдущем примере тип StreetAddress необходимо определить свойство ключа.  

Чтобы понять, как EF ядра отслеживает эти объекты, имеет смысл подумать, что первичный ключ создается как [затемнять свойство](xref:core/modeling/shadow-properties) для типа владельца. Значение ключа экземпляра владельца типа будет таким же, как значение ключа владельца экземпляра.      

## <a name="mapping-owned-types-with-table-splitting"></a>Собственные типы с разделения таблицы сопоставления

При использовании реляционных баз данных, по соглашению, собственные типы сопоставляются с той же таблицы в качестве владельца. Это требует разделения таблицы в двух: некоторые столбцы будут использоваться для хранения данных, владельца и некоторых столбцов будет использоваться для хранения данных, принадлежащий сущности. Это общие функции, известной как разделение таблицы.

> [!TIP]
> Собственные типы, хранимые с разделения таблицы можно использовать почти так же, как сложных типов используются в EF6.

По соглашению EF Core присвоит имя столбцов базы данных для свойств типа сущности владельца, следуя шаблону _EntityProperty_OwnedEntityProperty_. Поэтому свойства StreetAddress будут отображаться в таблице Orders с именами ShippingAddress_Street и ShippingAddress_City.

После добавления `HasColumnName` метод Переименование этих столбцов. В случае, когда StreetAddress является открытым свойством будет сопоставления

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Совместное использование .NET однотипные несколько собственных типов

Тип сущности, принадлежащие можно того же типа .NET другого типа сущности, принадлежащие таким образом тип .NET может оказаться недостаточно для идентификации типа владельца.

В этих случаях становится свойство, указывающее от владельца владение субъекта _определение навигации_ типа принадлежащий сущности. С точки зрения EF Core определение переходов является частью идентификатора типа вместе с типом .NET.   

Например в следующем классе ShippingAddress и BillingAddress будут того же типа .NET, StreetAddress:

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

Чтобы понять, как основной EF, выделяет отслеживаемых экземпляров этих объектов, может быть полезно представить, что определение навигации стал частью ключа экземпляра со значением ключа владельца и .NET тип владельца типа.

## <a name="nested-owned-types"></a>Вложенные типы, принадлежащие

В этом примере OrderDetails владеет BillingAddress и ShippingAddress, которые оба типа StreetAddress. Затем OrderDetails принадлежит тип заказа.

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

Это возможно в цепочке `OwnsOne` метод в fluent сопоставления для настройки этой модели:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

Можно добиться того же результата с помощью `OwnedAttribute` OrderDetails и StreetAdress.

Помимо владельца вложенные типы принадлежащие тип может ссылаться на обычной сущности. В следующем примере страной является обычной сущности (т. е. без владельца):

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

Эта возможность устанавливает типы сущностей принадлежащие отдельно от сложных типов в EF6.

## <a name="storing-owned-types-in-separate-tables"></a>Хранение собственные типы в отдельных таблицах

Также в отличие от EF6 сложные типы, принадлежащие типов могут храниться в отдельной таблице от владельца. Чтобы переопределить соглашение, которое сопоставляет тип собственности на одну таблицу как владелец, можно просто вызов `ToTable` и укажите другое имя таблицы. Следующий пример будет сопоставлен OrderDetails и двумя адресами отдельную таблицу из заказа:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a>Собственные типы запросов

При запросе владельца принадлежащие типы включаются по умолчанию. Нет необходимости использовать `Include` метод, даже если собственных типов хранятся в отдельной таблице. Следующий запрос на основании модели, описанной выше, будет получать заказа, OrderDetails и два StreeAddresses владельца для всех ожидающих заказов из базы данных:

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a>Ограничения

Ниже приведены некоторые ограничения типов владение субъекта. Некоторые из этих ограничений лежат в основе работы как собственные типы, но некоторые другие — на момент ограничений, которые необходимо удалить в последующих выпусках:

### <a name="current-shortcomings"></a>Текущий недостатки
- Наследование из собственных типов не поддерживается
- Собственные типы нельзя указывать с помощью свойства навигации коллекции
- Так как они используют счет разбиения таблиц, по умолчанию принадлежащие типы имеют следующие ограничения, если явно сопоставлены в другую таблицу:
   - Они не может принадлежать производный тип
   - Определение свойства навигации нельзя присвоить значение null (т. е. собственные типы в той же таблице всегда обязательны)

### <a name="by-design-restrictions"></a>Ограничения по своей природе
- Не удается создать `DbSet<T>`
- Не удается вызвать `Entity<T>()` с типом владельца на `ModelBuilder`
