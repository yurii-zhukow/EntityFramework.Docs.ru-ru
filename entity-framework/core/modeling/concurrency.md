---
title: Маркеры параллелизма — EF Core
author: rowanmiller
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: db768c1de99000be91d33764ccd3c3924237f8bb
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197455"
---
# <a name="concurrency-tokens"></a>Маркеры параллелизма

> [!NOTE]
> На этой странице приведена документация по настройке маркеров параллелизма. См. раздел [Обработка конфликтов параллелизма](../saving/concurrency.md) для подробного объяснения того, как работает управление параллелизмом в EF Core и примеры обработки конфликтов параллелизма в приложении.

Свойства, настроенные как маркеры параллелизма, используются для реализации управления оптимистичным параллелизмом.

## <a name="conventions"></a>Соглашения

По соглашению свойства никогда не настраиваются в качестве маркеров параллелизма.

## <a name="data-annotations"></a>Заметки к данным

Заметки к данным можно использовать для настройки свойства как маркера параллелизма.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>Текучий API

API-интерфейс Fluent можно использовать для настройки свойства как маркера параллелизма.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Метка времени/версия строки

Отметка времени — это свойство, в котором база данных создает новое значение при каждой вставке или обновлении строки. Свойство также обрабатывается как маркер параллелизма. Это гарантирует, что вы получите исключение, если кто-то другой изменил строку, которую вы пытаетесь обновить, с момента запроса данных.

Как это достигается при использовании поставщика базы данных. Для SQL Server метка времени обычно используется для свойства *Byte []* , которое будет настроено как столбец *ROWVERSION* в базе данных.

### <a name="conventions"></a>Соглашения

По соглашению свойства никогда не настраиваются как метки времени.

### <a name="data-annotations"></a>Заметки к данным

Для настройки свойства в качестве метки времени можно использовать заметки к данным.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>Текучий API

Для настройки свойства в качестве метки времени можно использовать API-интерфейс Fluent.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Timestamp.cs#ConfigureTimestampFluent)]
