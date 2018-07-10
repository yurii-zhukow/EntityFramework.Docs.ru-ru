---
title: Собственные типы сущностей — EF Core
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: 768429b857b09c1974f4ade31b5bbb6b1c7e15c3
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911878"
---
# <a name="owned-entity-types"></a>Собственные типы сущностей

>[!NOTE]
> Этот компонент впервые появился в EF Core 2.0.

EF Core позволяет модели типы сущностей, которые могут появляться только когда-либо других типов сущностей свойства навигации. Они называются _собственные типы сущностей_. Сущность, содержащая принадлежащий Тип сущности является его _владельца_.

## <a name="explicit-configuration"></a>Явной настройки

Объект, к которому типы никогда не включаются по EF Core в модели по соглашению, которым владеет. Можно использовать `OwnsOne` метод в `OnModelCreating` или указать для типа `OwnedAttribute` (новое в EF Core 2.1) для настройки типа как принадлежащего типа.

В этом примере StreetAddress — это тип с помощью нет свойство identity. Он используется как свойство сущности Order для указания адреса доставки конкретного заказа. В `OnModelCreating`, мы используем `OwnsOne` метод, чтобы указать, что свойство ShippingAddress принадлежит сущности типа Order.

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

Если свойство ShippingAddress является закрытым в типе Order, можно использовать строковую версию `OwnsOne` метод:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

В этом примере мы используем `OwnedAttribute` для достижения одной цели:

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

В EF Core 2.0 и 2.1 принадлежащие типы можно указать только свойства навигации по ссылке. Коллекциями принадлежащих типов не поддерживаются. Эти ссылки, которым владеет типы всегда иметь взаимно-однозначной связи с владельцем, поэтому им не нужны собственные значения ключа. В предыдущем примере тип StreetAddress не определяет ключевое свойство.  

Чтобы понять, как EF Core отслеживает эти объекты, имеет смысл подумать, что первичный ключ создается как [затемнения свойства](xref:core/modeling/shadow-properties) для принадлежащего типа. Значение ключа экземпляра принадлежащего типа будет таким же, как значение ключа владельца экземпляра.      

## <a name="mapping-owned-types-with-table-splitting"></a>Сопоставление, принадлежащие типы с помощью разбиения таблицы

При использовании реляционных баз данных, по соглашению принадлежащие типы сопоставляются с той же таблице, как владелец. Для этого требуется разделение таблицы в двух: некоторые столбцы будут использоваться для хранения данных, владельца, а некоторые столбцы будут использоваться для хранения данных, принадлежащий сущности. Это общие возможности, известной как разделение таблицы.

> [!TIP]
> Принадлежащие типы, хранимые с помощью разбиения таблицы можно использовать почти так же, как сложные типы используются в EF6.

По соглашению EF Core будет имен столбцов базы данных для свойств типа принадлежащих сущностей следующий шаблон _EntityProperty_OwnedEntityProperty_. Таким образом StreetAddress свойства отобразятся в таблице Orders с именами ShippingAddress_Street и ShippingAddress_City.

После добавления `HasColumnName` метод переименовать эти столбцы. В случае, когда StreetAddress является открытым свойством сопоставление будет выполняться

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Совместное использование одного типа .NET среди нескольких принадлежащие типы

Принадлежащий Тип сущности может быть того же типа .NET как другим принадлежащим типом сущности, поэтому тип .NET может оказаться недостаточно для определения принадлежащего типа.

В таких случаях свойство, указывающее от владельца принадлежащих сущностей становится _задания перехода_ типа принадлежащих сущностей. С точки зрения EF Core определяющую навигацию является частью идентификатора типа вместе с типом .NET.   

Например в следующем классе ShippingAddress и BillingAddress имеют тот же тип .NET, StreetAddress:

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

Чтобы понять, как EF Core позволит отличить отслеживаемые экземпляры этих объектов, может быть полезно подумать, что определение навигации стал частью ключа экземпляра со значением ключа владельца и тип .NET, принадлежащего типа.

## <a name="nested-owned-types"></a>Вложенные принадлежащие типы

В этом примере OrderDetails владеет BillingAddress и ShippingAddress, который представляют собой типы StreetAddress. Затем OrderDetails принадлежит тип.

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

Существует возможность цепочки `OwnsOne` метод текучего сопоставления для настройки этой модели:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

Это можно добиться той же самое с использованием `OwnedAttribute` OrderDetails и StreetAdress.

В дополнение к вложенные принадлежащие типы принадлежащий тип может ссылаться на обычной сущности. В следующем примере выбранной страной является обычной сущности (т. е. без владельца):

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

Эта возможность устанавливает собственные типы сущностей помимо сложных типов в EF6.

## <a name="storing-owned-types-in-separate-tables"></a>Хранение принадлежащие типы в отдельных таблицах

Также в отличие от сложных типов EF6, принадлежащие типы могут храниться в отдельной таблице от владельца. Чтобы переопределить соглашение, которое сопоставляется с той же таблице, как владелец принадлежащих типов, можно просто вызвать `ToTable` и укажите другое имя таблицы. Следующий пример будет сопоставлен OrderDetails и двумя адресами отдельную таблицу из заказа:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a>Запрос принадлежащие типы

При запросе владельца принадлежащие типы включаются по умолчанию. Нет необходимости использовать `Include` метод, даже если принадлежащие типы хранятся в отдельной таблице. Исходя из модели, описанной выше, следующий запрос извлечет, заказа, OrderDetails и два собственных StreeAddresses все ожидающие заказы из базы данных:

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a>Ограничения

Некоторые из этих ограничений лежат в основе как принадлежащий рабочих типов сущности, но некоторые другие являются ограничения, что мы можно удалить в последующих выпусках:

### <a name="shortcomings-in-previous-versions"></a>Недостатки в предыдущих версиях
- В EF Core 2.0 принадлежащие переходы на типы сущностей не может объявляться в производные типы сущностей, если собственные типы сущностей, явным образом сопоставляются отдельную таблицу из иерархии владельца. Это ограничение было снято в EF Core 2.1
 
### <a name="current-shortcomings"></a>Нынешних изъянов
- Иерархии наследования, содержащие принадлежащих типов сущностей, не поддерживаются
- Собственные типы сущностей не может быть, на который ссылается свойство навигации коллекции (только одну ссылку переходы в настоящее время поддерживаются)
- Переходы на собственных типов сущностей не может быть null, если они явным образом сопоставлены с отдельной таблицей от владельца 
- Экземпляров принадлежащих типов сущностей, которые не могут совместно использоваться несколько владельцев (это хорошо известных сценарий для объектов значений, которые не может быть реализован с помощью принадлежащих типов сущностей)

### <a name="by-design-restrictions"></a>Ограничения нормальное поведение
- Не удается создать `DbSet<T>`
- Нельзя вызывать `Entity<T>()` с принадлежащего типа в `ModelBuilder`
