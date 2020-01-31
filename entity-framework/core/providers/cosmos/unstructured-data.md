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
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="cb5fb-103">Работа с неструктурированными данными в поставщике EF Core Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="cb5fb-103">Working with Unstructured Data in EF Core Azure Cosmos DB Provider</span></span>

<span data-ttu-id="cb5fb-104">EF Core был разработан для упрощения работы с данными, которые следуют за схемой, определенной в модели.</span><span class="sxs-lookup"><span data-stu-id="cb5fb-104">EF Core was designed to make it easy to work with data that follows a schema defined in the model.</span></span> <span data-ttu-id="cb5fb-105">Однако одним из достоинств Azure Cosmos DB является гибкость в форме хранимых данных.</span><span class="sxs-lookup"><span data-stu-id="cb5fb-105">However one of the strengths of Azure Cosmos DB is the flexibility in the shape of the data stored.</span></span>

## <a name="accessing-the-raw-json"></a><span data-ttu-id="cb5fb-106">Доступ к необработанному JSON</span><span class="sxs-lookup"><span data-stu-id="cb5fb-106">Accessing the raw JSON</span></span>

<span data-ttu-id="cb5fb-107">Доступ к свойствам, которые не отслеживанию, можно получить, EF Core с помощью специального свойства в [теневом состоянии](../../modeling/shadow-properties.md) с именем `"__jObject"`, которое содержит `JObject`, представляющую данные, полученные из хранилища и данные, которые будут сохранены:</span><span class="sxs-lookup"><span data-stu-id="cb5fb-107">It is possible to access the properties that are not tracked by EF Core through a special property in [shadow-state](../../modeling/shadow-properties.md) named `"__jObject"` that contains a `JObject` representing the data recieved from the store and data that will be stored:</span></span>

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
> <span data-ttu-id="cb5fb-108">Свойство `"__jObject"` является частью инфраструктуры EF Core и должно использоваться только в качестве последнего средства, так как в будущих выпусках это может привести к разным поведениям.</span><span class="sxs-lookup"><span data-stu-id="cb5fb-108">The `"__jObject"` property is part of the EF Core infrastructure and should only be used as a last resort as it is likely to have different behavior in future releases.</span></span>

> [!NOTE]
> <span data-ttu-id="cb5fb-109">Изменения в сущности будут переопределять значения, хранящиеся в `"__jObject"` во время `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="cb5fb-109">Changes to the entity will override the values stored in `"__jObject"` during `SaveChanges`.</span></span>

## <a name="using-cosmosclient"></a><span data-ttu-id="cb5fb-110">Использование Космосклиент</span><span class="sxs-lookup"><span data-stu-id="cb5fb-110">Using CosmosClient</span></span>

<span data-ttu-id="cb5fb-111">Чтобы отвязаться полностью от EF Core получить объект [космосклиент](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) , который входит [в состав пакета SDK для Azure Cosmos DB](/azure/cosmos-db/sql-api-get-started) , из `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="cb5fb-111">To decouple completely from EF Core get the [CosmosClient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) object that is [part of the Azure Cosmos DB SDK](/azure/cosmos-db/sql-api-get-started) from `DbContext`:</span></span>

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a><span data-ttu-id="cb5fb-112">Отсутствующие значения свойств</span><span class="sxs-lookup"><span data-stu-id="cb5fb-112">Missing property values</span></span>

<span data-ttu-id="cb5fb-113">В предыдущем примере мы удалили свойство `"TrackingNumber"` из заказа.</span><span class="sxs-lookup"><span data-stu-id="cb5fb-113">In the previous example we removed the `"TrackingNumber"` property from the order.</span></span> <span data-ttu-id="cb5fb-114">Из-за того, как индексирование работает в Cosmos DB, запросы, ссылающиеся на отсутствующее свойство в другом месте, чем в проекции, могут возвращать непредвиденные результаты.</span><span class="sxs-lookup"><span data-stu-id="cb5fb-114">Because of how indexing works in Cosmos DB, queries that reference the missing property somewhere else than in the projection could return unexpected results.</span></span> <span data-ttu-id="cb5fb-115">Например:</span><span class="sxs-lookup"><span data-stu-id="cb5fb-115">For example:</span></span>

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

<span data-ttu-id="cb5fb-116">Отсортированный запрос фактически не возвращает результатов.</span><span class="sxs-lookup"><span data-stu-id="cb5fb-116">The sorted query actually returns no results.</span></span> <span data-ttu-id="cb5fb-117">Это означает, что необходимо всегда заполнять свойства, сопоставленные EF Core при непосредственной работе с хранилищем.</span><span class="sxs-lookup"><span data-stu-id="cb5fb-117">This means that one should take care to always populate properties mapped by EF Core when working with the store directly.</span></span>

> [!NOTE]
> <span data-ttu-id="cb5fb-118">Это поведение может измениться в будущих версиях Cosmos.</span><span class="sxs-lookup"><span data-stu-id="cb5fb-118">This behavior might change in future versions of Cosmos.</span></span> <span data-ttu-id="cb5fb-119">Например, в настоящее время, если политика индексирования определяет составной индекс {ID/?</span><span class="sxs-lookup"><span data-stu-id="cb5fb-119">For instance, currently if the indexing policy defines the composite index {Id/?</span></span> <span data-ttu-id="cb5fb-120">ASC, Траккингнумбер/?</span><span class="sxs-lookup"><span data-stu-id="cb5fb-120">ASC, TrackingNumber/?</span></span> <span data-ttu-id="cb5fb-121">ASC)}, затем запрос с предложением ORDER BY c.Id ASC, c. дискриминатор __ASC "возвратит__ элементы, у которых отсутствует свойство `"TrackingNumber"`.</span><span class="sxs-lookup"><span data-stu-id="cb5fb-121">ASC)}, then a query that has 'ORDER BY c.Id ASC, c.Discriminator ASC' __would__ return items that are missing the `"TrackingNumber"` property.</span></span>
