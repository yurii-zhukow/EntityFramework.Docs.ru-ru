---
title: Поставщик EF Core для Azure Cosmos DB
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 28264681-4486-4891-888c-be5e4ade24f1
uid: core/providers/cosmos/index
ms.openlocfilehash: 96686256bb93f5828bb21fed167eb57812806390
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813547"
---
# <a name="ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="e2881-102">Поставщик EF Core для Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="e2881-102">EF Core Azure Cosmos DB Provider</span></span>

>[!NOTE]
> <span data-ttu-id="e2881-103">Этот поставщик является новым в EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="e2881-103">This provider is new in EF Core 3.0.</span></span>

<span data-ttu-id="e2881-104">Этот поставщик базы данных позволяет использовать Entity Framework Core с Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="e2881-104">This database provider allows Entity Framework Core to be used with Azure Cosmos DB.</span></span> <span data-ttu-id="e2881-105">Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="e2881-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

<span data-ttu-id="e2881-106">Перед чтением этого раздела мы настоятельно рекомендуем ознакомиться с [документацией по Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction).</span><span class="sxs-lookup"><span data-stu-id="e2881-106">It is strongly recommended to familiarize yourself with the [Azure Cosmos DB documentation](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) before reading this section.</span></span>

## <a name="install"></a><span data-ttu-id="e2881-107">Установка</span><span class="sxs-lookup"><span data-stu-id="e2881-107">Install</span></span>

<span data-ttu-id="e2881-108">Установите [пакет NuGet Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span><span class="sxs-lookup"><span data-stu-id="e2881-108">Install the [Microsoft.EntityFrameworkCore.Cosmos NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span></span>

# <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="e2881-109">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="e2881-109">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

# <a name="visual-studiotabvs"></a>[<span data-ttu-id="e2881-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e2881-110">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a><span data-ttu-id="e2881-111">Приступая к работе</span><span class="sxs-lookup"><span data-stu-id="e2881-111">Get Started</span></span>

> [!TIP]  
> <span data-ttu-id="e2881-112">Вы можете скачать используемый в этой статье [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="e2881-112">You can view this article's [sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span></span>

<span data-ttu-id="e2881-113">Как и для других поставщиков, сначала нужно вызвать `UseCosmos`:</span><span class="sxs-lookup"><span data-stu-id="e2881-113">Like for other providers the first step is to call `UseCosmos`:</span></span>

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> <span data-ttu-id="e2881-114">Для простоты конечная точка и ключ здесь включены в сам код, но в рабочем приложении необходимо обеспечить их [безопасное хранение](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="e2881-114">The endpoint and key are hardcoded here for simplicity, but in a production app these should be [stored securily](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)</span></span>

<span data-ttu-id="e2881-115">В этом примере `Order` представляет собой простую сущность, ссылающуюся на `StreetAddress` [зависимого типа](../../modeling/owned-entities.md).</span><span class="sxs-lookup"><span data-stu-id="e2881-115">In this example `Order` is a simple entity with a reference to the [owned type](../../modeling/owned-entities.md) `StreetAddress`.</span></span>

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

<span data-ttu-id="e2881-116">Сохранение данных и запрос на их получение выполняется, как обычно в EF:</span><span class="sxs-lookup"><span data-stu-id="e2881-116">Saving and quering data follows the normal EF pattern:</span></span>

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> <span data-ttu-id="e2881-117">Путем вызова `EnsureCreated` вы создадите требуемые коллекции и вставите [начальные данные](../../modeling/data-seeding.md), если они есть в модели.</span><span class="sxs-lookup"><span data-stu-id="e2881-117">Calling `EnsureCreated` is necessary to create the required collections and insert the [seed data](../../modeling/data-seeding.md) if present in the model.</span></span> <span data-ttu-id="e2881-118">Но `EnsureCreated` следует вызывать только во время развертывания, а не в рабочем режиме, так как это может вызвать проблемы с производительностью.</span><span class="sxs-lookup"><span data-stu-id="e2881-118">However `EnsureCreated` should only be called during deployment, not normal operation, as it may cause performance issues.</span></span>

## <a name="cosmos-specific-model-customization"></a><span data-ttu-id="e2881-119">Настройка моделей для Cosmos</span><span class="sxs-lookup"><span data-stu-id="e2881-119">Cosmos-specific Model Customization</span></span>

<span data-ttu-id="e2881-120">По умолчанию все типы сущностей сопоставляются с одним и тем же контейнером с именем на основе производного контекста (в нашем примере `"OrderContext"`).</span><span class="sxs-lookup"><span data-stu-id="e2881-120">By default all entity types are mapped to the same container, named after the derived context (`"OrderContext"` in this case).</span></span> <span data-ttu-id="e2881-121">Изменить имя контейнера по умолчанию можно с помощью `HasDefaultContainer`:</span><span class="sxs-lookup"><span data-stu-id="e2881-121">To change the default container name use `HasDefaultContainer`:</span></span>

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

<span data-ttu-id="e2881-122">Сопоставить тип сущности с другим контейнером можно с помощью `ToContainer`:</span><span class="sxs-lookup"><span data-stu-id="e2881-122">To map an entity type to a different container use `ToContainer`:</span></span>

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

<span data-ttu-id="e2881-123">Чтобы определить тип сущности элемента, EF Core добавляет значение дискриминатора, даже если у элемента нет производных типов сущности.</span><span class="sxs-lookup"><span data-stu-id="e2881-123">To identify the entity type that a given item represent EF Core adds a discriminator value even if there are no derived entity types.</span></span> <span data-ttu-id="e2881-124">Имя и значение дискриминатора [можно изменить](../../modeling/inheritance.md).</span><span class="sxs-lookup"><span data-stu-id="e2881-124">The name and value of the discriminator [can be changed](../../modeling/inheritance.md).</span></span>

## <a name="embedded-entities"></a><span data-ttu-id="e2881-125">Внедренные сущности</span><span class="sxs-lookup"><span data-stu-id="e2881-125">Embedded Entities</span></span>

<span data-ttu-id="e2881-126">В Cosmos зависимые сущности внедряются в тот же элемент, что и владелец.</span><span class="sxs-lookup"><span data-stu-id="e2881-126">For Cosmos owned entities are embedded in the same item as the owner.</span></span> <span data-ttu-id="e2881-127">Изменить имя свойства можно с помощью `ToJsonProperty`:</span><span class="sxs-lookup"><span data-stu-id="e2881-127">To change a property name use `ToJsonProperty`:</span></span>

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

<span data-ttu-id="e2881-128">В такой конфигурации порядок из приведенного выше примера сохраняется так:</span><span class="sxs-lookup"><span data-stu-id="e2881-128">With this configuration the order from the example above is stored like this:</span></span>

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
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-692e763901d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163674
}
```

<span data-ttu-id="e2881-129">Также внедряются коллекции зависимых сущностей.</span><span class="sxs-lookup"><span data-stu-id="e2881-129">Collections of owned entities are embedded as well.</span></span> <span data-ttu-id="e2881-130">В следующем примере мы будем использовать класс `Distributor` с коллекцией `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="e2881-130">For the next example we'll use the `Distributor` class with a collection of `StreetAddress`:</span></span>

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

<span data-ttu-id="e2881-131">Зависимым сущностям не нужно предоставлять значения ключа явным образом для хранения:</span><span class="sxs-lookup"><span data-stu-id="e2881-131">The owned entities don't need to provide explicit key values to be stored:</span></span>

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

<span data-ttu-id="e2881-132">Они будут сохранены так:</span><span class="sxs-lookup"><span data-stu-id="e2881-132">They will be persisted in this way:</span></span>

``` json
{
    "Id": 1,
    "Discriminator": "Distributor",
    "id": "Distributor|1",
    "ShippingCenters": [
        {
            "City": "Phoenix",
            "Discriminator": "StreetAddress",
            "Street": "500 S 48th Street"
        },
        {
            "City": "Anaheim",
            "Discriminator": "StreetAddress",
            "Street": "5650 Dolly Ave"
        }
    ],
    "_rid": "6QEKANzISj0BAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKANzISj0=/docs/6QEKANzISj0BAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-7b2b439701d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163705
}
```

<span data-ttu-id="e2881-133">В EF Core всегда должны присутствовать уникальные значения ключа для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="e2881-133">Internally EF Core always needs to have unique key values for all tracked entities.</span></span> <span data-ttu-id="e2881-134">Первичный ключ, создаваемый по умолчанию для коллекций зависимых типов, состоит из свойств внешнего ключа, указывающих на владельца, и свойства `int`, соответствующего индексу в массиве JSON.</span><span class="sxs-lookup"><span data-stu-id="e2881-134">The primary key created by default for collections of owned types consists of the foreign key properties pointing to the owner and an `int` property corresponding to the index in the JSON array.</span></span> <span data-ttu-id="e2881-135">Чтобы получить запись этих значений, можно использовать API:</span><span class="sxs-lookup"><span data-stu-id="e2881-135">To retrieve these values entry API could be used:</span></span>

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> <span data-ttu-id="e2881-136">При необходимости можно изменить первичный ключ по умолчанию для принадлежащих типов сущностей, а затем указать значения ключа явным образом.</span><span class="sxs-lookup"><span data-stu-id="e2881-136">When necessary the default primary key for the owned entity types can be changed, but then key values should be provided explicitly.</span></span>

## <a name="working-with-disconnected-entities"></a><span data-ttu-id="e2881-137">Работа с отключенными сущностями</span><span class="sxs-lookup"><span data-stu-id="e2881-137">Working with Disconnected Entities</span></span>

<span data-ttu-id="e2881-138">Каждый элемент должен иметь значение `id`, уникальное для определенного ключа секции.</span><span class="sxs-lookup"><span data-stu-id="e2881-138">Every item needs to have an `id` value that is unique for the given partition key.</span></span> <span data-ttu-id="e2881-139">По умолчанию EF Core создает значение, объединяя дискриминатор и значения первичного ключа с помощью символа | в качестве разделителя.</span><span class="sxs-lookup"><span data-stu-id="e2881-139">By default EF Core generates the value by concatenating the discriminator and the primary key values, using '|' as a delimiter.</span></span> <span data-ttu-id="e2881-140">Значения ключа создаются, только если сущность принимает состояние `Added`.</span><span class="sxs-lookup"><span data-stu-id="e2881-140">The key values are only generated when an entity enters the `Added` state.</span></span> <span data-ttu-id="e2881-141">Это может вызвать проблему при [присоединении сущностей](../../saving/disconnected-entities.md), если у них нет свойства `id` для типа .NET, чтобы хранить значение.</span><span class="sxs-lookup"><span data-stu-id="e2881-141">This might pose a problem when [attaching entities](../../saving/disconnected-entities.md) if they don't have an `id` property on the .NET type to store the value.</span></span>

<span data-ttu-id="e2881-142">Чтобы обойти это ограничение, можно создать и задать значение `id` вручную или пометить сущность как добавленную первой, а затем придать ей нужное состояние:</span><span class="sxs-lookup"><span data-stu-id="e2881-142">To work around this limitation one could create and set the `id` value manually or mark the entity as added first, then changing it to the desired state:</span></span>

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

<span data-ttu-id="e2881-143">В результате мы получим такой файл JSON:</span><span class="sxs-lookup"><span data-stu-id="e2881-143">This is the resulting JSON:</span></span>

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
        "ShipsToStreet": "3 Abbey Road"
    },
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-8f7ac48f01d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163739
}
```
