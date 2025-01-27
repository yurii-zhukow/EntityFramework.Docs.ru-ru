---
title: Резервные поля — EF Core
description: Настройка резервных полей для свойств в модели Entity Framework Core
author: ajcvickers
ms.date: 10/27/2016
uid: core/modeling/backing-field
ms.openlocfilehash: 0df54192da7365451e5213739df8117e66c5cd0f
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94429667"
---
# <a name="backing-fields"></a>Резервные поля

Резервные поля позволяют EF читать и (или) записывать в поле, а не в свойство. Это может быть полезно, если Инкапсуляция в классе используется для ограничения использования и/или улучшения семантики доступа к данным с помощью кода приложения, но значение должно быть считано из базы данных и (или) записано в нее без использования этих ограничений и улучшений.

## <a name="basic-configuration"></a>Базовая конфигурация

В соответствии с соглашением следующие поля будут обнаружены как резервные поля для заданного свойства (в порядке приоритета).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

В следующем примере `Url` свойство настроено для использования в `_url` качестве резервного поля:

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

Обратите внимание, что резервные поля обнаруживаются только для свойств, которые включены в модель. Дополнительные сведения о том, какие свойства включены в модель, см. в разделе [включение & за исключением свойств](xref:core/modeling/entity-properties).

Резервные поля также можно настроить с помощью аннотации данных (доступно в Ефкоре 5,0) или API Fluent, например если имя поля не соответствует приведенным выше соглашениям:

### <a name="data-annotations"></a>[Заметки к данным](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/BackingField.cs?name=BackingField&highlight=7)]

### <a name="fluent-api"></a>[Текучий API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs?name=BackingField&highlight=5)]

***

## <a name="field-and-property-access"></a>Доступ к полям и свойствам

По умолчанию EF всегда будет считывать и записывать данные в резервное поле (предполагая, что одно из них настроено правильно) и никогда не будет использовать свойство. Однако EF также поддерживает другие шаблоны доступа. Например, следующий пример указывает EF на запись в резервное поле только при материализации, а также на использование свойства во всех остальных случаях:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs?name=BackingFieldAccessMode&highlight=6)]

Полный набор поддерживаемых параметров см. в [перечислении пропертякцессмоде](/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) .

> [!NOTE]
> В EF Core 3,0 режим доступа к свойству по умолчанию изменен с `PreferFieldDuringConstruction` на `PreferField` .

## <a name="field-only-properties"></a>Свойства только для полей

Можно также создать концептуальное свойство в модели, которое не имеет соответствующего свойства CLR в классе сущности, а использует поле для хранения данных в сущности. Это отличается от [теневых свойств](xref:core/modeling/shadow-properties), где данные хранятся в средстве записи изменений, а не в типе CLR сущности. Свойства, доступные только для полей, обычно используются, когда класс сущностей использует методы вместо свойств для получения и задания значений или в случаях, когда поля не должны быть представлены вообще в модели предметной области (например, первичные ключи).

Можно настроить свойство только для поля, указав имя в `Property(...)` API:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

EF будет пытаться найти свойство CLR с заданным именем или поле, если свойство не найдено. Если не найдено ни свойство, ни поле, будет настроено свойство теневого копирования.

Может потребоваться ссылка на свойство, доступное только для полей, из запросов LINQ, но такие поля обычно являются частными. `EF.Property(...)`Метод в запросе LINQ можно использовать для ссылки на поле:

```csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
