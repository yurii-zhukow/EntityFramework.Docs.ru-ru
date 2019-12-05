---
title: Индексы (реляционная база данных) — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/modeling/relational/indexes
ms.openlocfilehash: e14615275f85ee9b6b32d080905465d33963feca
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824573"
---
# <a name="indexes-relational-database"></a>Индексы (реляционная база данных)

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

Индекс в реляционной базе данных сопоставляется с тем же понятием, что и индекс в ядре Entity Framework.

## <a name="conventions"></a>Обозначения

По соглашению индексы называются `IX_<type name>_<property name>`. Для составных индексов `<property name>` преобразуется в список имен свойств с разделителями подчеркивания.

## <a name="data-annotations"></a>Заметки к данным

Индексы не могут быть настроены с помощью заметок к данным.

## <a name="fluent-api"></a>Текучий API

Для настройки имени индекса можно использовать API-интерфейс Fluent.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

Можно также указать фильтр.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

При использовании поставщика SQL Server EF добавляет фильтр `'IS NOT NULL'` для всех столбцов, допускающих значение null, которые являются частью уникального индекса. Чтобы переопределить это соглашение, можно указать значение `null`.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a>Включить столбцы в индексы SQL Server

Можно настроить [индексы с помощью включаемых столбцов](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) , чтобы значительно повысить производительность запросов, если все столбцы в запросе включены в индекс в качестве ключевых или неключевых столбцов.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexInclude.cs?name=Model)]
