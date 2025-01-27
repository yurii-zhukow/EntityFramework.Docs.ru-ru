---
title: Свойства теневой копии и индексатора — EF Core
description: Настройка свойств тени и индексатора в модели Entity Framework Core
author: AndriySvyryd
ms.date: 10/09/2020
uid: core/modeling/shadow-properties
ms.openlocfilehash: d419de2da2a9fc29e675dde76e824217347d2e9c
ms.sourcegitcommit: 788a56c2248523967b846bcca0e98c2ed7ef0d6b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2020
ms.locfileid: "95003436"
---
# <a name="shadow-and-indexer-properties"></a>Свойства теневой копии и индексатора

Теневые свойства — это свойства, не определенные в классе сущности .NET, но определенные для этого типа сущности в модели EF Core. Значения и состояния этих свойств хранятся исключительно в средстве отслеживания изменений. Теневые свойства полезны при наличии в базе данных данных, которые не должны быть представлены в сопоставленных типах сущностей.

Свойства индексатора — это свойства типа сущности, которые поддерживаются [индексатором](/dotnet/csharp/programming-guide/indexers/) в классе сущности .NET. Доступ к ним можно получить с помощью индексатора в экземплярах класса .NET. Он также позволяет добавлять дополнительные свойства в тип сущности, не изменяя класс CLR.

## <a name="foreign-key-shadow-properties"></a>Свойства тени внешнего ключа

Свойства теневой копии чаще всего используются для свойств внешнего ключа, где связь между двумя сущностями представляется значением внешнего ключа в базе данных, но Управление связью осуществляется на основе типов сущностей с помощью свойств навигации между типами сущностей. По соглашению EF будет представлять теневое свойство при обнаружении связи, но свойство внешнего ключа не найдено в зависимом классе сущности.

Свойству будет присвоено имя `<navigation property name><principal key property name>` (Навигация по зависимой сущности, которая указывает на основную сущность, используется для именования). Если имя свойства ключа участника включает имя свойства навигации, то имя будет иметь значение `<principal key property name>` . Если в зависимой сущности нет свойства навигации, в его месте используется имя типа участника.

Например, приведенный ниже листинг кода приведет к тому, что в `BlogId` сущность будет введено свойство Shadow `Post` :

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions&highlight=21-23)]

## <a name="configuring-shadow-properties"></a>Настройка теневых свойств

Для настройки теневых свойств можно использовать API-интерфейс Fluent. После вызова перегрузки строки `Property` можно связать любой из вызовов конфигурации для других свойств. В следующем примере, поскольку не `Blog` имеет свойства CLR с именем `LastUpdated` , создается свойство Shadow:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]

Если имя, передаваемое в `Property` метод, совпадает с именем существующего свойства (теневого или определенного для класса сущностей), то код настроит это существующее свойство, а не создаст новое свойство Shadow.

## <a name="accessing-shadow-properties"></a>Доступ к теневым свойствам

Значения теневых свойств можно получить и изменить с помощью `ChangeTracker` API:

```csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

На теневые свойства можно ссылаться в запросах LINQ с помощью `EF.Property` статического метода:

```csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

Доступ к теневым свойствам после неотслеживаемого запроса невозможен, так как отслеживание изменений не отслеживает возвращаемые сущности.

## <a name="configuring-indexer-properties"></a>Настройка свойств индексатора

Для настройки свойств индексатора можно использовать API-интерфейс Fluent. После вызова метода `IndexerProperty` можно связать любой из вызовов конфигурации для других свойств. В следующем примере `Blog` определен индексатор, который будет использоваться для создания свойства индексатора.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexerProperty.cs?name=ShadowProperty&highlight=3)]

Если имя, передаваемое в `IndexerProperty` метод, совпадает с именем существующего свойства индексатора, то код настроит это существующее свойство. Если тип сущности имеет свойство, которое поддерживается свойством класса сущностей, возникает исключение, так как свойства индексатора должны быть доступны только через индексатор.

## <a name="property-bag-entity-types"></a>Типы сущностей контейнера свойств

> [!NOTE]
> Поддержка типов сущностей контейнера свойств была представлена в EF Core 5,0.

Типы сущностей, которые содержат только свойства индексатора, называются типами сущностей контейнера свойств. Эти типы сущностей не имеют теневых свойств, а EF создаст свойства индексатора. Сейчас `Dictionary<string, object>` поддерживается только в качестве типа сущности контейнера свойств. Его необходимо настроить в качестве общего типа сущности с уникальным именем, а соответствующее `DbSet` свойство должно быть реализовано с помощью `Set` вызова.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SharedType.cs?name=SharedType&highlight=3,7)]
