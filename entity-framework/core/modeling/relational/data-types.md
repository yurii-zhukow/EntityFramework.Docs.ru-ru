---
title: Типы данных — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: 9060f66c752be01090ce40be6bf3a32f348ce571
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993525"
---
# <a name="data-types"></a>Типы данных

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

Тип данных относится к конкретному типу базы данных столбца, с которым сопоставлено свойство.

## <a name="conventions"></a>Соглашения

По соглашению поставщик базы данных выбирает тип данных, на основе типа CLR свойства. Он также принимает во внимание другие метаданные, например настроенного [Максимальная длина](../max-length.md), является ли свойство частью первичного ключа и т. д.

Например, SQL Server использует `datetime2(7)` для `DateTime` свойства, и `nvarchar(max)` для `string` свойства (или `nvarchar(450)` для `string` свойствах, которые используются в качестве ключа).

## <a name="data-annotations"></a>Заметки к данным

Заметки к данным можно использовать для указания свой тип данных для столбца.

Например, приведенный ниже код настраивает `Url` как строка не в Юникоде с максимальной длиной `200` и `Rating` как тип decimal с точностью `5` и масштабирование объекта `2`.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>Текучий API

Fluent API также можно указать разные типы данных для столбцов.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
