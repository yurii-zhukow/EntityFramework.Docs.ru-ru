---
title: Типы данных, EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
ms.technology: entity-framework-core
uid: core/modeling/relational/data-types
ms.openlocfilehash: fd4668a3f9554eb9d3b1161d5dddce2fcdcac712
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053504"
---
# <a name="data-types"></a>Типы данных

> [!NOTE]  
> В этом разделе конфигурации применяется для реляционных баз данных в целом. Методы расширения, показанный здесь будут доступны после установки поставщика реляционной базы данных (из-за общей *Microsoft.EntityFrameworkCore.Relational* пакета).

Тип данных ссылается на конкретный тип столбца, с которым сопоставлено свойство базы данных.

## <a name="conventions"></a>Соглашения

По соглашению поставщика базы данных выбирает тип данных, на основе типа CLR свойства. Он принимает во внимание другие метаданные, такие как настроенного [максимальную длину](../max-length.md), является ли свойство частью первичного ключа, и т. д.

Например, SQL Server использует `datetime2(7)` для `DateTime` свойства, и `nvarchar(max)` для `string` свойства (или `nvarchar(450)` для `string` свойства, используемые в качестве ключа).

## <a name="data-annotations"></a>Заметки к данным

Заметок к данным можно использовать для указания типа данных для столбца.

Например, следующий код настраивает `Url` как строка не в Юникоде с максимальной длиной `200` и `Rating` как десятичное число с точностью `5` и масштабирование объекта `2`.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>Fluent API

Fluent API также можно указать одинаковые типы данных для столбцов.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
