---
title: Маркеры параллелизма — EF Core
author: rowanmiller
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: 0051d416544a11385f99d36e45843c5b20725af7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994230"
---
# <a name="concurrency-tokens"></a>Маркеры параллелизма

> [!NOTE]
> На этой странице приводятся способы настройки маркеры параллелизма. См. в разделе [обработка конфликтов параллелизма](../saving/concurrency.md) подробное описание того, как работает управление параллелизмом в EF Core и примеры для обработки конфликтов параллелизма в приложении.

Свойства, настроенные как маркеры параллелизма, используемые для реализации управления оптимистичным параллелизмом.

## <a name="conventions"></a>Соглашения

По соглашению свойства никогда не настраиваются как маркеры параллелизма.

## <a name="data-annotations"></a>Заметки к данным

Заметки данных можно использовать для настройки свойства в качестве маркера параллелизма.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>Текучий API

Fluent API можно использовать для настройки свойства в качестве маркера параллелизма.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Метка времени или версии

Метка времени — это свойство, где новое значение формируется по базе данных каждый раз при вставке или обновлении строки. Свойство также рассматриваются как маркер параллелизма. Это гарантирует, что вы получите исключение, если кто-то еще изменил строку, которую вы пытаетесь обновить, так как запросы для данных.

Как это можно сделать, зависит от используемого поставщика базы данных. Для SQL Server, метки времени обычно используется на *byte []* свойство, которое будет настроить как *ROWVERSION* столбца в базе данных.

### <a name="conventions"></a>Соглашения

По соглашению свойства никогда не настраиваются как отметки времени.

### <a name="data-annotations"></a>Заметки к данным

Заметки к данным можно использовать для настройки свойства в качестве метки времени.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>Текучий API

Fluent API можно использовать для настройки свойства в качестве метки времени.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
