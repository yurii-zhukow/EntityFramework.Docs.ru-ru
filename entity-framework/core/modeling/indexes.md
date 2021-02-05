---
title: Индексы — EF Core
description: Настройка индексов в модели Entity Framework Core
author: roji
ms.date: 12/16/2019
uid: core/modeling/indexes
ms.openlocfilehash: ab81b108c4ff518cf98b7e835da3553c0c41efed
ms.sourcegitcommit: 7700840119b1639275f3b64836e7abb59103f2e7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2021
ms.locfileid: "98128541"
---
# <a name="indexes"></a>Индексы

Индексы являются распространенной концепцией для многих хранилищ данных. Хотя их реализация в хранилище данных может различаться, они используются для выполнения уточняющих запросов, основанных на столбце (или наборе столбцов), которые более эффективны. Дополнительные сведения о хорошем использовании индекса см. в [разделе "индексы](xref:core/performance/efficient-querying#use-indexes-properly) " в документации по производительности.

Индекс для столбца можно указать следующим образом:

## <a name="data-annotations"></a>[Заметки к данным](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Index.cs?name=Index&highlight=1)]

> [!NOTE]
> Настройка индексов с помощью заметок к данным появилась в EF Core 5,0.

## <a name="fluent-api"></a>[Текучий API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=4)]

***

> [!NOTE]
> По соглашению индекс создается в каждом свойстве (или наборе свойств), используемом в качестве внешнего ключа.
>
> EF Core поддерживает только один индекс для каждого отдельного набора свойств. Если настроить индекс для набора свойств, для которого уже определен индекс, в соответствии с соглашением или предыдущей конфигурацией, то будет изменено определение этого индекса. Это полезно, если необходимо дополнительно настроить индекс, созданный по соглашению.

## <a name="composite-index"></a>Составной индекс

Индекс может также охватывать более одного столбца:

### <a name="data-annotations"></a>[Заметки к данным](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IndexComposite.cs?name=Composite&highlight=1)]

### <a name="fluent-api"></a>[Текучий API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=4)]

***

Индексы над несколькими столбцами, также известные как *Составные индексы*, ускоряют запросы, которые фильтруются по столбцам индекса, но также запросы, которые фильтруют только по *первым* столбцам, охватываемым индексом. Дополнительные сведения см. в документации по [производительности](xref:core/performance/efficient-querying#use-indexes-properly) .

## <a name="index-uniqueness"></a>Уникальность индекса

По умолчанию индексы не являются уникальными: несколько строк могут иметь одинаковые значения для набора столбцов индекса. Уникальный индекс можно сделать следующим образом:

### <a name="data-annotations"></a>[Заметки к данным](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IndexUnique.cs?name=IndexUnique&highlight=1)]

### <a name="fluent-api"></a>[Текучий API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=IndexUnique&highlight=5)]

***

Попытка вставить более одной сущности с одинаковыми значениями для набора столбцов индекса приведет к возникновению исключения.

## <a name="index-name"></a>Имя индекса

По соглашению для индексов, созданных в реляционной базе данных, используются имена `IX_<type name>_<property name>` . Для составных индексов `<property name>` преобразуется в список имен свойств с разделителями подчеркивания.

Вы можете задать имя индекса, созданного в базе данных:

### <a name="data-annotations"></a>[Заметки к данным](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IndexName.cs?name=IndexName&highlight=1)]

### <a name="fluent-api"></a>[Текучий API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexName.cs?name=IndexName&highlight=5)]

***

## <a name="index-filter"></a>Фильтр индекса

Некоторые реляционные базы данных позволяют указать отфильтрованный или частичный индекс. Это позволяет индексировать только подмножество значений столбца, уменьшая размер индекса и повышая производительность и использование дискового пространства. Дополнительные сведения об SQL Server фильтруемых индексах [см. в документации](/sql/relational-databases/indexes/create-filtered-indexes).

API-интерфейс Fluent можно использовать для указания фильтра по индексу, который указывается в виде выражения SQL:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexFilter.cs?name=IndexFilter&highlight=5)]

При использовании поставщика SQL Server EF добавляет `'IS NOT NULL'` Фильтр для всех столбцов, допускающих значение null, которые являются частью уникального индекса. Чтобы переопределить это соглашение, можно указать `null` значение.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexNoFilter.cs?name=IndexNoFilter&highlight=6)]

## <a name="included-columns"></a>Включенные столбцы

Некоторые реляционные базы данных позволяют настроить набор столбцов, которые будут включены в индекс, но не являются частью ключа. Это может значительно повысить производительность запросов, если все столбцы в запросе включены в индекс как ключевые или неключевые столбцы, так как к самой таблице не требуется доступ. Дополнительные сведения о SQL Server включенных столбцах [см. в документации](/sql/relational-databases/indexes/create-indexes-with-included-columns).

В следующем примере `Url` столбец является частью ключа индекса, поэтому для любой фильтрации запросов в этом столбце может использоваться индекс. Кроме того, запросы, обращающиеся только `Title` к `PublishedOn` столбцам и, не требуют доступа к таблице и будут работать более эффективно:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexInclude.cs?name=IndexInclude&highlight=5-9)]
