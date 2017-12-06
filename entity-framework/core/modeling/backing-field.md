---
title: "Резервного поля — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
ms.technology: entity-framework-core
uid: core/modeling/backing-field
ms.openlocfilehash: e95417b3368d09a64f9ec02ae40c38dc770c2e6b
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2017
---
# <a name="backing-fields"></a>Резервные поля

> [!NOTE]  
> Этот компонент впервые появился в EF Core 1.1.

Резервные поля Разрешить EF чтения или записи в поле, а не свойство. Это может быть полезно, когда инкапсуляция в классе используется ограничение использования и/или улучшения семантики вокруг доступ к данным с помощью кода приложения, но значение следует чтения или записи в базу данных без использования этих ограничений или улучшения.

## <a name="conventions"></a>Соглашения

По соглашению следующие поля будут обнаружены как резервного поля для заданного свойства (перечислены в порядке приоритета). Поля обнаруживаются только для свойств, которые включены в модель. Включены свойства в модели Дополнительные сведения см. в разделе [включая & исключение свойств](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

При настройке резервное поле EF записи непосредственно к этому полю при материализации экземпляров сущностей из базы данных (вместо использования метода задания свойства). Если EF должен прочитать или записать значение в другое время, он будет по возможности используйте свойство. Например если EF должен обновить значение свойства, воспользуется метод задания свойства, если он был определен. Если свойство доступно только для чтения, затем используется значение поля.

## <a name="data-annotations"></a>Заметки к данным

Резервного поля нельзя настроить с помощью заметок к данным.

## <a name="fluent-api"></a>Fluent API

Fluent API можно использовать для настройки резервное поле для свойства.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Управление, если используется поле

Можно настроить при EF использует поле или свойство. В разделе [PropertyAccessMode перечисления](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) поддерживаемые параметры.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Поля без свойства

Можно также создать концептуального свойства в модели, не имеет соответствующее свойство CLR в класс сущностей, но использует поле для хранения данных в сущности. Это поведение отличается от [замещения свойств](shadow-properties.md), где данные хранятся в объекте отслеживания изменений. Это обычно будет использоваться, если класс сущностей использует методы для получения или задания значения.

Можно предоставить EF имя поля в `Property(...)` API. Если нет свойства с заданным именем, EF будет выглядеть для поля.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

Можно также предоставить имя, отличное от имени поля свойства. Это имя используется при создании модели, прежде всего будет использоваться для имени столбца, сопоставленного в базе данных.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

Если нет свойства в класс сущностей, можно использовать `EF.Property(...)` метод в запросе LINQ, ссылка на свойство, по существу является частью модели.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
