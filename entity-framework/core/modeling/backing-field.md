---
title: Резервные поля — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 79221b6f7968675ff10f80d5df181b674b6a20c9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994097"
---
# <a name="backing-fields"></a>Резервные поля

> [!NOTE]  
> Этот компонент впервые появился в EF Core 1.1.

Резервные поля Разрешить EF на чтение или запись в поле, а не свойство. Это может быть полезно, когда инкапсуляция в классе используется для ограничения использования и улучшения семантики вокруг доступ к данным в коде приложения, но значение следует чтения или записи в базу данных без использования этих ограничений / Улучшенные возможности.

## <a name="conventions"></a>Соглашения

По соглашению будут обнаружены следующие поля как резервные поля для заданного свойства (перечисленные в порядке приоритетности). Поля обнаруживаются только для свойств, которые включаются в модели. Дополнительные сведения, на котором включены свойства в модели, см. в разделе [Включение и исключение свойств](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

При настройке резервное поле EF записи непосредственно к этому полю при материализации экземпляров сущности из базы данных (а не с помощью метода задания свойства). Если EF должен считывать или записывать значения в других случаях, она будет по возможности используйте свойство. Например если EF необходимо обновить значение для свойства, он если он был определен будет использовать метод задания свойства. Если свойство доступно только для чтения, он запишет к полю.

## <a name="data-annotations"></a>Заметки к данным

Резервные поля нельзя настроить с помощью заметок к данным.

## <a name="fluent-api"></a>Текучий API

Fluent API можно использовать для настройки резервное поле для свойства.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Управление, если используется поле

Вы можете настроить, когда EF использует поле или свойство. См. в разделе [PropertyAccessMode перечисления](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) поддерживаемые параметры.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Поля типа без свойства

Также можно создать свойство концептуальной модели, не имеет соответствующего свойства среды CLR в классе сущности, но вместо этого использует поле для хранения данных в сущности. Это отличается от [замещения свойств](shadow-properties.md), где данные хранятся в объекте отслеживания изменений. Это обычно будет использоваться, если класс сущностей использует методы для получения и задания значений.

Можно предоставить EF имя поля в `Property(...)` API. Если отсутствует свойство с заданным именем, EF будет выглядеть для поля.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

Можно также даю свойству имя, отличное от имени поля. Это имя используется при создании модели, прежде всего он будет использоваться для имени столбца, который сопоставляется в базе данных.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

Если отсутствует свойство в классе сущности, можно использовать `EF.Property(...)` метода в запросе LINQ для ссылки на свойство, которое по существу является частью модели.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
