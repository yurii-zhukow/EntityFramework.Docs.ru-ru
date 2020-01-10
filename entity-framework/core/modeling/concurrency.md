---
title: Маркеры параллелизма — EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: 8a5f3aa09c2a83d5be0998a11ef2ee8100437514
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781148"
---
# <a name="concurrency-tokens"></a>Маркеры параллелизма

> [!NOTE]
> На этой странице приведена документация по настройке маркеров параллелизма. См. раздел [Обработка конфликтов параллелизма](../saving/concurrency.md) для подробного объяснения того, как работает управление параллелизмом в EF Core и примеры обработки конфликтов параллелизма в приложении.

Свойства, настроенные как маркеры параллелизма, используются для реализации управления оптимистичным параллелизмом.

## <a name="configuration"></a>Конфигурация

### <a name="data-annotationstabdata-annotations"></a>[Заметки к данным](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Concurrency.cs?name=Concurrency&highlight=5)]

### <a name="fluent-apitabfluent-api"></a>[API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Concurrency.cs?name=Concurrency&highlight=5)]

***

## <a name="timestamprowversion"></a>Timestamp/rowversion

Timestamp/rowversion — это свойство, для которого новое значение автоматически создается базой данных при каждой вставке или обновлении строки. Свойство также обрабатывается как маркер параллелизма, гарантируя, что вы получаете исключение, если обновляемая строка изменилась с момента запроса. Точные сведения зависят от используемого поставщика базы данных. для SQL Server обычно используется свойство *Byte []* , которое будет настроено как столбец типа *ROWVERSION* в базе данных.

Можно настроить свойство как timestamp/rowversion следующим образом:

### <a name="data-annotationstabdata-annotations"></a>[Заметки к данным](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Timestamp.cs?name=Timestamp&highlight=7)]

### <a name="fluent-apitabfluent-api"></a>[API Fluent](#tab/fluent-api)

[! код-CSharp [Main] (.. /.. /.. /Самплес/коре/моделинг/флуентапи/тиместамп.КС? Name = timestamp & выделение = 9, 17]

***
