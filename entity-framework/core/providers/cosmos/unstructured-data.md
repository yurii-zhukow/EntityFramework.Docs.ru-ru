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
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="07e32-102">Работа с неструктурированными данными в поставщике EF Core Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="07e32-102">Working with Unstructured Data in EF Core Azure Cosmos DB Provider</span></span>

<span data-ttu-id="07e32-103">EF Core был разработан для упрощения работы с данными, которые следуют за схемой, определенной в модели.</span><span class="sxs-lookup"><span data-stu-id="07e32-103">EF Core was designed to make it easy to work with data that follows a schema defined in the model.</span></span> <span data-ttu-id="07e32-104">Однако одним из достоинств Azure Cosmos DB является гибкость в форме хранимых данных.</span><span class="sxs-lookup"><span data-stu-id="07e32-104">However one of the strengths of Azure Cosmos DB is the flexibility in the shape of the data stored.</span></span>

## <a name="accessing-the-raw-json"></a><span data-ttu-id="07e32-105">Доступ к необработанному JSON</span><span class="sxs-lookup"><span data-stu-id="07e32-105">Accessing the raw JSON</span></span>

<span data-ttu-id="07e32-106">Доступ к свойствам, которые не отслеживанию, можно получить, EF Core с помощью специального свойства в [теневом состоянии](../../modeling/shadow-properties.md) с именем `"__jObject"` , которое содержит `JObject` данные, полученные из хранилища и данные, которые будут сохранены:</span><span class="sxs-lookup"><span data-stu-id="07e32-106">It is possible to access the properties that are not tracked by EF Core through a special property in [shadow-state](../../modeling/shadow-properties.md) named `"__jObject"` that contains a `JObject` representing the data recieved from the store and data that will be stored:</span></span>

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
> <span data-ttu-id="07e32-107">`"__jObject"` Свойство является частью инфраструктуры EF Core и должно использоваться только в качестве последнего средства, так как в будущих выпусках, скорее всего, будет происходить другое поведение.</span><span class="sxs-lookup"><span data-stu-id="07e32-107">The `"__jObject"` property is part of the EF Core infrastructure and should only be used as a last resort as it is likely to have different behavior in future releases.</span></span>

> [!NOTE]
> <span data-ttu-id="07e32-108">Изменения в сущности будут переопределять значения, хранящиеся `"__jObject"` в `SaveChanges`во время.</span><span class="sxs-lookup"><span data-stu-id="07e32-108">Changes to the entity will override the values stored in `"__jObject"` during `SaveChanges`.</span></span>

## <a name="using-cosmosclient"></a><span data-ttu-id="07e32-109">Использование Космосклиент</span><span class="sxs-lookup"><span data-stu-id="07e32-109">Using CosmosClient</span></span>

<span data-ttu-id="07e32-110">Чтобы полностью отделить от EF Core получить `CosmosClient` объект, который является [частью пакета SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) для Azure Cosmos DB `DbContext`, из:</span><span class="sxs-lookup"><span data-stu-id="07e32-110">To decouple completely from EF Core get the `CosmosClient` object that is [part of the Azure Cosmos DB SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) from `DbContext`:</span></span>

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a><span data-ttu-id="07e32-111">Отсутствующие значения свойств</span><span class="sxs-lookup"><span data-stu-id="07e32-111">Missing property values</span></span>

<span data-ttu-id="07e32-112">В предыдущем примере мы удалили `"TrackingNumber"` свойство из заказа.</span><span class="sxs-lookup"><span data-stu-id="07e32-112">In the previous example we removed the `"TrackingNumber"` property from the order.</span></span> <span data-ttu-id="07e32-113">Из-за того, как индексирование работает в Cosmos DB, запросы, ссылающиеся на отсутствующее свойство в другом месте, чем в проекции, могут возвращать непредвиденные результаты.</span><span class="sxs-lookup"><span data-stu-id="07e32-113">Because of how indexing works in Cosmos DB, queries that reference the missing property somewhere else than in the projection could return unexpected results.</span></span> <span data-ttu-id="07e32-114">Например:</span><span class="sxs-lookup"><span data-stu-id="07e32-114">For example:</span></span>

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

<span data-ttu-id="07e32-115">Отсортированный запрос фактически не возвращает результатов.</span><span class="sxs-lookup"><span data-stu-id="07e32-115">The sorted query actually returns no results.</span></span> <span data-ttu-id="07e32-116">Это означает, что необходимо всегда заполнять свойства, сопоставленные EF Core при непосредственной работе с хранилищем.</span><span class="sxs-lookup"><span data-stu-id="07e32-116">This means that one should take care to always populate properties mapped by EF Core when working with the store directly.</span></span>

> [!NOTE]
> <span data-ttu-id="07e32-117">Это поведение может измениться в будущих версиях Cosmos.</span><span class="sxs-lookup"><span data-stu-id="07e32-117">This behavior might change in future versions of Cosmos.</span></span> <span data-ttu-id="07e32-118">Например, в настоящее время, если политика индексирования определяет составной индекс {ID/?</span><span class="sxs-lookup"><span data-stu-id="07e32-118">For instance, currently if the indexing policy defines the composite index {Id/?</span></span> <span data-ttu-id="07e32-119">ASC, Траккингнумбер/?</span><span class="sxs-lookup"><span data-stu-id="07e32-119">ASC, TrackingNumber/?</span></span> <span data-ttu-id="07e32-120">ASC)}, затем запрос, имеющий "Order By c.ID ASC, c. дискриминатор __ASC", возвратит элементы__ , у которых отсутствует `"TrackingNumber"` свойство.</span><span class="sxs-lookup"><span data-stu-id="07e32-120">ASC)}, then a query the has 'ORDER BY c.Id ASC, c.Discriminator ASC' __would__ return items that are missing the `"TrackingNumber"` property.</span></span>