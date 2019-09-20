---
title: Типы данных — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: d667cbcb821e321faed36d097b531c7c55b81248
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149165"
---
# <a name="data-types"></a>Типы данных

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

Тип данных относится к конкретному типу базы данных столбец, с которым сопоставлено свойство.

## <a name="conventions"></a>Соглашения

По соглашению поставщик базы данных выбирает тип данных на основе типа .NET свойства. Он также учитывает другие метаданные, такие как настроенная [Максимальная длина](../max-length.md), является ли свойство частью первичного ключа и т. д.

Например, SQL Server использует `datetime2(7)` для `DateTime` свойств, а `nvarchar(max)` также для `string` свойств (или `nvarchar(450)` для `string` свойств, которые используются в качестве ключа).

## <a name="data-annotations"></a>Заметки к данным

Можно использовать заметки к данным, чтобы указать точный тип данных для столбца.

Например, `Url` следующий код настраивает в качестве строки не в Юникоде с максимальной `200` длиной и `Rating` `2`в виде десятичного значения с точностью `5` до и масштабом.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>Текучий API

Можно также использовать API Fluent, чтобы указать те же типы данных для столбцов.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
