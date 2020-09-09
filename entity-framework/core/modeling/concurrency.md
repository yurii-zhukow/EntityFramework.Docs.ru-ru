---
title: Маркеры параллелизма — EF Core
description: Настройка маркеров параллелизма для управления оптимистичным параллелизмом в модели Entity Framework Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: 57718a1b0146f437bb4126524889c81ae0dfd895
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89617554"
---
# <a name="concurrency-tokens"></a>Маркеры параллелизма

> [!NOTE]
> На этой странице приведена документация по настройке маркеров параллелизма. См. раздел [Обработка конфликтов параллелизма](xref:core/saving/concurrency) для подробного объяснения того, как работает управление параллелизмом в EF Core и примеры обработки конфликтов параллелизма в приложении.

Свойства, настроенные как маркеры параллелизма, используются для реализации управления оптимистичным параллелизмом.

## <a name="configuration"></a>Конфигурация

### <a name="data-annotations"></a>[Заметки к данным](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Concurrency.cs?name=Concurrency&highlight=5)]

### <a name="fluent-api"></a>[Текучий API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Concurrency.cs?name=Concurrency&highlight=5)]

***

## <a name="timestamprowversion"></a>Timestamp/rowversion

Timestamp/rowversion — это свойство, для которого новое значение автоматически создается базой данных при каждой вставке или обновлении строки. Свойство также обрабатывается как маркер параллелизма, гарантируя, что вы получаете исключение, если обновляемая строка изменилась с момента запроса. Точные сведения зависят от используемого поставщика базы данных. для SQL Server обычно используется свойство *Byte []* , которое будет настроено как столбец типа *ROWVERSION* в базе данных.

Можно настроить свойство как timestamp/rowversion следующим образом:

### <a name="data-annotations"></a>[Заметки к данным](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Timestamp.cs?name=Timestamp&highlight=7)]

### <a name="fluent-api"></a>[Текучий API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Timestamp.cs?name=Timestamp&highlight=9,17)]

***
