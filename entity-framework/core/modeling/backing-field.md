---
title: Резервные поля - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: d6faf7c0a07c7b855c1ce402ae29e19c80b9d3b4
ms.sourcegitcommit: 144edccf9b29a7ffad119c235ac9808ec1a46193
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2020
ms.locfileid: "81434179"
---
# <a name="backing-fields"></a>Резервные поля

Резервные поля позволяют EF читать и/или писать в поле, а не в свойство. Это может быть полезно, когда инкапсуляция в классе используется для ограничения использования и/или повышения семантики вокруг доступа к данным по коду приложения, но значение должно быть прочитано и/или записано в базу данных без использования этих ограничений/улучшений.

## <a name="basic-configuration"></a>Базовая конфигурация

По конвенции следующие поля будут обнаружены в качестве резервных полей для данного свойства (перечисленного в порядке приоритета). 

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

В следующем примере `Url` свойство настроено `_url` в качестве резервного поля:

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

Обратите внимание, что резервные поля обнаруживаются только для свойств, включенных в модель. Для получения дополнительной информации о том, какие свойства включены в модель, см [&.](included-properties.md)

Вы также можете настроить резервные поля с помощью аннотации данных или API Fluent, например, если имя поля не соответствует вышеуказанным конвенциям:

### <a name="data-annotations"></a>[Аннотации данных](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/BackingField.cs?name=BackingField&highlight=7)]

### <a name="fluent-api"></a>[Текучий API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs?name=BackingField&highlight=5)]

## <a name="field-and-property-access"></a>Доступ к полю и собственности

По умолчанию EF всегда будет читать и писать в резервное поле - если один из них был правильно настроен - и никогда не будет использовать свойство. Тем не менее, EF также поддерживает другие шаблоны доступа. Например, следующий пример поручает EF писать в резервное поле только при материализации и использовать свойство во всех других случаях:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs?name=BackingFieldAccessMode&highlight=6)]

Ознакомьтесь с [перечней PropertyAccessMode](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) для получения полного набора поддерживаемых опций.

> [!NOTE]
> С EF Core 3.0 режим доступа `PreferFieldDuringConstruction` к `PreferField`свойству по умолчанию изменился с .

## <a name="field-only-properties"></a>Свойства только для полей

Можно также создать концептуальное свойство в модели, которое не имеет соответствующего свойства CLR в классе сущности, но вместо этого использует поле для хранения данных в объекте. Это отличается от [Shadow Properties,](shadow-properties.md)где данные хранятся в трекере изменений, а не в типе CLR сущности. Свойства только по полевым и только используются, когда класс сущности использует методы вместо свойств для получения/установленных значений, или в тех случаях, когда поля не должны подвергаться вообще в модели домена (например, первичные ключи).

Вы можете настроить свойство только для поля, `Property(...)` предоставив имя в API:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

EF попытается найти свойство CLR с заданным именем или поле, если свойство не найдено. Если не найдено ни свойство, ни поле, вместо него будет настроено свойство теней.

Возможно, потребуется обратиться к свойству только для поля из запросов LIN, но такие поля, как правило, являются частными. Этот `EF.Property(...)` метод можно использовать в запросе LIN' для обозначения поля:

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
