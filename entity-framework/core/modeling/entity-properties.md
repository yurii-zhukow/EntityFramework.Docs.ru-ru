---
title: Свойства сущности — EF Core
description: Настройка и сопоставьте свойства сущности с помощью Entity Framework Core
author: roji
ms.date: 12/10/2019
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/entity-properties
ms.openlocfilehash: b67603fbffd1f1c8506bc21f8972c851eb8eef29
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502429"
---
# <a name="entity-properties"></a>Свойства объекта

Каждый тип сущности в модели имеет набор свойств, которые EF Core будут считывать и записывать данные из базы данных. Если используется реляционная база данных, то свойства сущности сопоставляются со столбцами таблицы.

## <a name="included-and-excluded-properties"></a>Включенные и исключенные свойства

По соглашению все открытые свойства с методом считывания и методом задания будут включаться в модель.

Конкретные свойства можно исключить следующим образом.

### <a name="data-annotationstabdata-annotations"></a>[Заметки к данным](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?name=IgnoreProperty&highlight=6)]

### <a name="fluent-apitabfluent-api"></a>[API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?name=IgnoreProperty&highlight=3,4)]

***

## <a name="column-names"></a>Имена столбцов

По соглашению при использовании реляционной базы данных свойства сущности сопоставляются со столбцами таблицы, имеющими то же имя, что и свойство.

Если вы предпочитаете настроить столбцы с разными именами, можно сделать следующее:

### <a name="data-annotationstabdata-annotations"></a>[Заметки к данным](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnName.cs?Name=ColumnName&highlight=3)]

### <a name="fluent-apitabfluent-api"></a>[API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnName.cs?Name=ColumnName&highlight=3-5)]

***

## <a name="column-data-types"></a>Типы данных столбцов

При использовании реляционной базы данных поставщик базы данных выбирает тип данных, основанный на типе .NET свойства. Он также учитывает другие метаданные, такие как настроенная [Максимальная длина](#maximum-length), является ли свойство частью первичного ключа и т. д.

Например, SQL Server сопоставляет `DateTime` свойства `datetime2(7)` столбцам, а `string` свойства `nvarchar(max)` столбцам (или `nvarchar(450)` для свойств, которые используются в качестве ключа).

Можно также настроить столбцы, указав для столбца точный тип данных. Например, следующий код настраивает `Url` как строку, не поддерживающую Юникод, с максимальной длиной `200` и `Rating` как десятичную с точностью `5` и масштабом `2`:

### <a name="data-annotationstabdata-annotations"></a>[Заметки к данным](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnDataType.cs?name=ColumnDataType&highlight=4,6)]

### <a name="fluent-apitabfluent-api"></a>[API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnDataType.cs?name=ColumnDataType&highlight=5-6)]

***

### <a name="maximum-length"></a>Максимальная длина

Настройка максимальной длины предоставляет указание поставщику базы данных о соответствующем типе данных столбца для выбора конкретного свойства. Максимальная длина применяется только к типам данных массива, таким как `string` и `byte[]`.

> [!NOTE]
> Entity Framework не выполняет никаких проверок максимальной длины перед передачей данных поставщику. Поставщик или хранилище данных можно проверить, если это уместно. Например, если целевой объект SQL Server, то превышение максимальной длины приведет к возникновению исключения, так как тип данных базового столбца не позволит сохранить излишние данные.

В следующем примере Настройка максимальной длины 500 приведет к созданию в SQL Server столбца типа `nvarchar(500)`.

#### <a name="data-annotationstabdata-annotations"></a>[Заметки к данным](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?name=MaxLength&highlight=4)]

#### <a name="fluent-apitabfluent-api"></a>[API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?name=MaxLength&highlight=3-5)]

***

## <a name="required-and-optional-properties"></a>Обязательные и необязательные свойства

Свойство считается необязательным, если для него допустимо значение `null`. Если `null` не является допустимым значением свойства, свойство считается обязательным. При сопоставлении со схемой реляционной базы данных обязательные свойства создаются как столбцы, не допускающие значения NULL, а дополнительные свойства создаются как столбцы, допускающие значение null.

### <a name="conventions"></a>Обозначения

По соглашению свойство, тип .NET которого может содержать значение null, будет настроено как необязательный, тогда как свойства, типы .NET которых не могут содержать значение null, будут настроены как обязательные. Например, все свойства с типами значений .NET (`int`, `decimal`, `bool`и т. д.) настраиваются как обязательные, а все свойства с типами значений .NET, допускающими значение null (`int?`, `decimal?`, `bool?`и т. д.), настраиваются как необязательные.

C#в 8 появилась новая функция, называемая [ссылочными типами, допускающими значение NULL](/dotnet/csharp/tutorials/nullable-reference-types), которая позволяет создавать заметки для ссылочных типов, указывающих, является ли он допустимым для того, чтобы они содержали значение null. Эта функция отключена по умолчанию и, если она включена, изменяет поведение EF Core следующим образом:

* Если ссылочные типы, допускающие значение null, отключены (по умолчанию), все свойства с ссылочными типами .NET настраиваются как необязательные по соглашению (например, `string`).
* Если ссылочные типы, допускающие значение null, включены, свойства будут C# настроены на основе допустимости значений NULL для их типа .net: `string?` будет настроена как необязательная, тогда как `string` будет настроена в соответствии с требованиями.

В следующем примере показан тип сущности с обязательными и необязательными свойствами с отключенной ссылкой, допускающей значение null (по умолчанию) и включенной.

#### <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[Без ссылочных типов, допускающих значения NULL (по умолчанию)](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

#### <a name="with-nullable-reference-typestabwith-nrt"></a>[С ссылочными типами, допускающими значение null](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

Рекомендуется использовать ссылочные типы, допускающие значение null, так как она C# передает возможность принимать нулевое значение, выраженную в коде, в модель EF Core и в базу данных, а также устраняет использование заметок к данным в интерфейсе API или аннотациях данных для выражения одной концепции дважды

> [!NOTE]
> Соблюдайте осторожность при включении ссылочных типов, допускающих значение null, в существующем проекте: свойства ссылочного типа, которые ранее были настроены как необязательные, теперь будут настроены как обязательные, если только они не имеют явно заметку null. При управлении схемой реляционной базы данных это может привести к созданию миграции, которые изменяют допустимость значений NULL в столбце базы данных.

Дополнительные сведения о ссылочных типах, допускающих значения NULL, и об их использовании с EF Core [см. на специальной странице документации по этой функции](xref:core/miscellaneous/nullable-reference-types).

### <a name="explicit-configuration"></a>Явная конфигурация

Свойство, которое было бы необязательным по соглашению, может быть настроено следующим образом:

#### <a name="data-annotationstabdata-annotations"></a>[Заметки к данным](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?name=Required&highlight=4)]

#### <a name="fluent-apitabfluent-api"></a>[API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?name=Required&highlight=3-5)]

***
