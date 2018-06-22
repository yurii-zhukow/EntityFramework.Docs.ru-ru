---
title: Маркеры параллелизма - EF Core
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: f3cf28d5c54e63aa76058e9fe1d9f3de5b37d579
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2018
ms.locfileid: "29745494"
---
# <a name="concurrency-tokens"></a>Маркеры параллелизма

> [!NOTE]
> Этой странице приведены инструкции по настройке маркеры параллелизма. В разделе [обработка конфликтов параллелизма](../saving/concurrency.md) подробное описание того, как работает управление параллелизмом на основных EF и примеры для обработки конфликтов параллелизма в приложении.

Свойства, настроенные как маркеры параллелизма используются для управления оптимистичным параллелизмом.

## <a name="conventions"></a>Соглашения

По соглашению свойства никогда не настроен как маркеры параллелизма.

## <a name="data-annotations"></a>Заметки к данным

Заметки данных можно использовать для настройки свойства в качестве маркера параллелизма.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>Текучий API

Fluent API можно использовать для настройки свойства в качестве маркера параллелизма.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Отметка времени или строки версии

Отметки времени — это свойство, когда новое значение создается в базе данных каждый раз при вставке или обновлении строки. Свойство также рассматриваются как маркер параллелизма. Это гарантирует, что если кто-то еще изменил строка, которую вы пытаетесь обновить, так как запросы для данных, будет вызвано исключение.

Как это можно сделать, зависит от используемого поставщика базы данных. Отметка времени обычно используются для SQL Server на *byte []* свойство, которое будет настроить как *ROWVERSION* столбца в базе данных.

### <a name="conventions"></a>Соглашения

По соглашению свойства никогда не настроен как отметки времени.

### <a name="data-annotations"></a>Заметки к данным

Заметок к данным можно использовать для настройки свойства в качестве метки времени.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>Текучий API

Fluent API можно использовать для настройки свойства в качестве метки времени.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
