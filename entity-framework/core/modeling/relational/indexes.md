---
title: Индексы — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 7dcf27dedbde45302a462a4c41a811b9868e40bb
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197012"
---
# <a name="indexes"></a>Индексы

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

Индекс в реляционной базе данных сопоставляется с тем же понятием, что и индекс в ядре Entity Framework.

## <a name="conventions"></a>Соглашения

По соглашению индексы `IX_<type name>_<property name>`называются. Для составных `<property name>` индексов преобразуется в список имен свойств с разделителями подчеркивания.

## <a name="data-annotations"></a>Заметки к данным

Индексы не могут быть настроены с помощью заметок к данным.

## <a name="fluent-api"></a>Текучий API

Для настройки имени индекса можно использовать API-интерфейс Fluent.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

Можно также указать фильтр.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

При использовании поставщика SQL Server EF добавляет фильтр "не равно NULL" для всех столбцов, допускающих значение null, которые являются частью уникального индекса. Чтобы переопределить это соглашение, можно указать `null` значение.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a>Включить столбцы в индексы SQL Server

Можно настроить [индексы с помощью включаемых столбцов](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) , чтобы значительно повысить производительность запросов, если все столбцы в запросе включены в индекс в качестве ключевых или неключевых столбцов.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ForSqlServerHasIndex.cs?name=Model)]
