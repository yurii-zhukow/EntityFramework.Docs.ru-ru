---
title: Поставщик Azure Cosmos DB — работа с неструктурированными данными — EF Core
description: Работа с Azure Cosmos DB неструктурированными данными с помощью Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/unstructured-data
ms.openlocfilehash: 69f979d46174ff56310b334f28438ac271f45155
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888100"
---
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a>Работа с неструктурированными данными в поставщике EF Core Azure Cosmos DB

EF Core был разработан для упрощения работы с данными, которые следуют за схемой, определенной в модели. Однако одним из достоинств Azure Cosmos DB является гибкость в форме хранимых данных.

## <a name="accessing-the-raw-json"></a>Доступ к необработанному JSON

Доступ к свойствам, которые не отслеживанию, можно получить, EF Core с помощью специального свойства в [теневом состоянии](../../modeling/shadow-properties.md) с именем `"__jObject"`, которое содержит `JObject`, представляющую данные, полученные из хранилища и данные, которые будут сохранены:

[!code-csharp[Unmapped](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=23,24&name=Unmapped)]

``` json
{
    "Id": 1,
    "PartitionKey": "1",
    "TrackingNumber": null,
    "id": "1",
    "Address": {
        "ShipsToCity": "London",
        "ShipsToStreet": "221 B Baker St"
    },
    "_rid": "eLMaAK8TzkIBAAAAAAAAAA==",
    "_self": "dbs/eLMaAA==/colls/eLMaAK8TzkI=/docs/eLMaAK8TzkIBAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683e-0a12bf8d01d5\"",
    "_attachments": "attachments/",
    "BillingAddress": "Clarence House",
    "_ts": 1568164374
}
```

> [!WARNING]
> Свойство `"__jObject"` является частью инфраструктуры EF Core и должно использоваться только в качестве последнего средства, так как в будущих выпусках это может привести к разным поведениям.

> [!NOTE]
> Изменения в сущности будут переопределять значения, хранящиеся в `"__jObject"` во время `SaveChanges`.

## <a name="using-cosmosclient"></a>Использование Космосклиент

Чтобы отвязаться полностью от EF Core получить объект [космосклиент](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) , который входит [в состав пакета SDK для Azure Cosmos DB](/azure/cosmos-db/sql-api-get-started) , из `DbContext`:

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a>Отсутствующие значения свойств

В предыдущем примере мы удалили свойство `"TrackingNumber"` из заказа. Из-за того, как индексирование работает в Cosmos DB, запросы, ссылающиеся на отсутствующее свойство в другом месте, чем в проекции, могут возвращать непредвиденные результаты. Например:

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

Отсортированный запрос фактически не возвращает результатов. Это означает, что необходимо всегда заполнять свойства, сопоставленные EF Core при непосредственной работе с хранилищем.

> [!NOTE]
> Это поведение может измениться в будущих версиях Cosmos. Например, в настоящее время, если политика индексирования определяет составной индекс {ID/? ASC, Траккингнумбер/? ASC)}, затем запрос с предложением ORDER BY c.Id ASC, c. дискриминатор __ASC "возвратит__ элементы, у которых отсутствует свойство `"TrackingNumber"`.
