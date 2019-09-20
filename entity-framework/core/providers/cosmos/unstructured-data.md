---
title: Поставщик Azure Cosmos DB — работа с неструктурированными данными — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: b47d41b6-984f-419a-ab10-2ed3b95e3919
uid: core/providers/cosmos/unstructured-data
ms.openlocfilehash: 86bb0f7915c8a2561e7d5cd5dffc27474218a112
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150773"
---
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a>Работа с неструктурированными данными в поставщике EF Core Azure Cosmos DB

EF Core был разработан для упрощения работы с данными, которые следуют за схемой, определенной в модели. Однако одним из достоинств Azure Cosmos DB является гибкость в форме хранимых данных.

## <a name="accessing-the-raw-json"></a>Доступ к необработанному JSON

Доступ к свойствам, которые не отслеживанию, можно получить, EF Core с помощью специального свойства в [теневом состоянии](../../modeling/shadow-properties.md) с именем `"__jObject"` , которое содержит `JObject` данные, полученные из хранилища и данные, которые будут сохранены:

[!code-csharp[Unmapped](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=21-23&name=Unmapped)]

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
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
> `"__jObject"` Свойство является частью инфраструктуры EF Core и должно использоваться только в качестве последнего средства, так как в будущих выпусках, скорее всего, будет происходить другое поведение.

> [!NOTE]
> Изменения в сущности будут переопределять значения, хранящиеся `"__jObject"` в `SaveChanges`во время.

## <a name="using-cosmosclient"></a>Использование Космосклиент

Чтобы полностью отделить от EF Core получить `CosmosClient` объект, который является [частью пакета SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) для Azure Cosmos DB `DbContext`, из:

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a>Отсутствующие значения свойств

В предыдущем примере мы удалили `"TrackingNumber"` свойство из заказа. Из-за того, как индексирование работает в Cosmos DB, запросы, ссылающиеся на отсутствующее свойство в другом месте, чем в проекции, могут возвращать непредвиденные результаты. Например:

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

Отсортированный запрос фактически не возвращает результатов. Это означает, что необходимо всегда заполнять свойства, сопоставленные EF Core при непосредственной работе с хранилищем.

> [!NOTE]
> Это поведение может измениться в будущих версиях Cosmos. Например, в настоящее время, если политика индексирования определяет составной индекс {ID/? ASC, Траккингнумбер/? ASC)}, затем запрос, имеющий "Order By c.ID ASC, c. дискриминатор __ASC", возвратит элементы__ , у которых отсутствует `"TrackingNumber"` свойство.