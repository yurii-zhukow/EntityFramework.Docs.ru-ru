---
title: Поставщик EF Core для Azure Cosmos DB
description: Документация для поставщика базы данных, которая позволяет использовать Entity Framework Core с API SQL Azure Cosmos DB
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/index
ms.openlocfilehash: 6903aab4911f7478afe3d8987a791ae1c5ccebce
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502218"
---
# <a name="ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="0801d-103">Поставщик EF Core для Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="0801d-103">EF Core Azure Cosmos DB Provider</span></span>

> [!NOTE]
> <span data-ttu-id="0801d-104">Этот поставщик является новым в EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="0801d-104">This provider is new in EF Core 3.0.</span></span>

<span data-ttu-id="0801d-105">Этот поставщик базы данных позволяет использовать Entity Framework Core с Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0801d-105">This database provider allows Entity Framework Core to be used with Azure Cosmos DB.</span></span> <span data-ttu-id="0801d-106">Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="0801d-106">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

<span data-ttu-id="0801d-107">Перед чтением этого раздела мы настоятельно рекомендуем ознакомиться с [документацией по Azure Cosmos DB](/azure/cosmos-db/introduction).</span><span class="sxs-lookup"><span data-stu-id="0801d-107">It is strongly recommended to familiarize yourself with the [Azure Cosmos DB documentation](/azure/cosmos-db/introduction) before reading this section.</span></span>

> [!NOTE]
> <span data-ttu-id="0801d-108">Этот поставщик работает только с API SQL Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="0801d-108">This provider only works with the SQL API of Azure Cosmos DB.</span></span>

## <a name="install"></a><span data-ttu-id="0801d-109">Установка</span><span class="sxs-lookup"><span data-stu-id="0801d-109">Install</span></span>

<span data-ttu-id="0801d-110">Установите [пакет NuGet Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span><span class="sxs-lookup"><span data-stu-id="0801d-110">Install the [Microsoft.EntityFrameworkCore.Cosmos NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span></span>

### <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="0801d-111">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="0801d-111">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

### <a name="visual-studiotabvs"></a>[<span data-ttu-id="0801d-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0801d-112">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a><span data-ttu-id="0801d-113">Начало работы</span><span class="sxs-lookup"><span data-stu-id="0801d-113">Get started</span></span>

> [!TIP]  
> <span data-ttu-id="0801d-114">Вы можете скачать используемый в этой статье [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="0801d-114">You can view this article's [sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span></span>

<span data-ttu-id="0801d-115">Как и для других поставщиков, сначала нужно вызвать [UseCosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos).</span><span class="sxs-lookup"><span data-stu-id="0801d-115">Like for other providers the first step is to call [UseCosmos](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosDbContextOptionsExtensions.UseCosmos):</span></span>

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> <span data-ttu-id="0801d-116">Для простоты конечная точка и ключ здесь включены в сам код, но в рабочем приложении необходимо обеспечить их [безопасное хранение](/aspnet/core/security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="0801d-116">The endpoint and key are hardcoded here for simplicity, but in a production app these should be [stored securily](/aspnet/core/security/app-secrets#secret-manager)</span></span>

<span data-ttu-id="0801d-117">В этом примере `Order` представляет собой простую сущность, ссылающуюся на `StreetAddress` [зависимого типа](../../modeling/owned-entities.md).</span><span class="sxs-lookup"><span data-stu-id="0801d-117">In this example `Order` is a simple entity with a reference to the [owned type](../../modeling/owned-entities.md) `StreetAddress`.</span></span>

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

<span data-ttu-id="0801d-118">Сохранение данных и запрос на их получение выполняется, как обычно в EF:</span><span class="sxs-lookup"><span data-stu-id="0801d-118">Saving and quering data follows the normal EF pattern:</span></span>

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> <span data-ttu-id="0801d-119">Путем вызова [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) вы создадите требуемые контейнеры и вставите [начальные данные](../../modeling/data-seeding.md), если они есть в модели.</span><span class="sxs-lookup"><span data-stu-id="0801d-119">Calling [EnsureCreatedAsync](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreatedAsync) is necessary to create the required containers and insert the [seed data](../../modeling/data-seeding.md) if present in the model.</span></span> <span data-ttu-id="0801d-120">Но `EnsureCreatedAsync` следует вызывать только во время развертывания, а не в рабочем режиме, так как это может вызвать проблемы с производительностью.</span><span class="sxs-lookup"><span data-stu-id="0801d-120">However `EnsureCreatedAsync` should only be called during deployment, not normal operation, as it may cause performance issues.</span></span>

## <a name="cosmos-specific-model-customization"></a><span data-ttu-id="0801d-121">Настройка моделей для Cosmos</span><span class="sxs-lookup"><span data-stu-id="0801d-121">Cosmos-specific model customization</span></span>

<span data-ttu-id="0801d-122">По умолчанию все типы сущностей сопоставляются с одним и тем же контейнером с именем на основе производного контекста (в нашем примере `"OrderContext"`).</span><span class="sxs-lookup"><span data-stu-id="0801d-122">By default all entity types are mapped to the same container, named after the derived context (`"OrderContext"` in this case).</span></span> <span data-ttu-id="0801d-123">Изменить имя контейнера по умолчанию можно с помощью [HasDefaultContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer).</span><span class="sxs-lookup"><span data-stu-id="0801d-123">To change the default container name use [HasDefaultContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosModelBuilderExtensions.HasDefaultContainer):</span></span>

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

<span data-ttu-id="0801d-124">Сопоставить тип сущности с другим контейнером можно с помощью [ToContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer).</span><span class="sxs-lookup"><span data-stu-id="0801d-124">To map an entity type to a different container use [ToContainer](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToContainer):</span></span>

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

<span data-ttu-id="0801d-125">Чтобы определить тип сущности элемента, EF Core добавляет значение дискриминатора, даже если у элемента нет производных типов сущности.</span><span class="sxs-lookup"><span data-stu-id="0801d-125">To identify the entity type that a given item represent EF Core adds a discriminator value even if there are no derived entity types.</span></span> <span data-ttu-id="0801d-126">Имя и значение дискриминатора [можно изменить](../../modeling/inheritance.md).</span><span class="sxs-lookup"><span data-stu-id="0801d-126">The name and value of the discriminator [can be changed](../../modeling/inheritance.md).</span></span>

<span data-ttu-id="0801d-127">Если в том же контейнере не будут храниться другие типы сущностей, дискриминатор можно удалить, вызвав [HasNoDiscriminator](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator).</span><span class="sxs-lookup"><span data-stu-id="0801d-127">If no other entity type will ever be stored in the same container the discriminator can be removed by calling [HasNoDiscriminator](/dotnet/api/Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasNoDiscriminator):</span></span>

[!code-csharp[NoDiscriminator](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=NoDiscriminator)]

### <a name="partition-keys"></a><span data-ttu-id="0801d-128">Ключи секций</span><span class="sxs-lookup"><span data-stu-id="0801d-128">Partition keys</span></span>

<span data-ttu-id="0801d-129">По умолчанию EF Core создаст контейнеры с ключом секции, равным `"__partitionKey"`, не предоставляя для него значения при вставке элементов.</span><span class="sxs-lookup"><span data-stu-id="0801d-129">By default EF Core will create containers with the partition key set to `"__partitionKey"` without supplying any value for it when inserting items.</span></span> <span data-ttu-id="0801d-130">Но чтобы полностью использовать возможности производительности Azure Cosmos, следует использовать [тщательно выбранный ключ секции](/azure/cosmos-db/partition-data).</span><span class="sxs-lookup"><span data-stu-id="0801d-130">But to fully leverage the performance capabilities of Azure Cosmos a [carefully selected partition key](/azure/cosmos-db/partition-data) should be used.</span></span> <span data-ttu-id="0801d-131">Его можно настроить, вызвав [HasPartitionKey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey).</span><span class="sxs-lookup"><span data-stu-id="0801d-131">It can be configured by calling [HasPartitionKey](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.HasPartitionKey):</span></span>

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PartitionKey)]

> [!NOTE]
><span data-ttu-id="0801d-132">Свойство ключа секции может иметь любой тип, если он [преобразован в строку](xref:core/modeling/value-conversions).</span><span class="sxs-lookup"><span data-stu-id="0801d-132">The partition key property can be of any type as long as it is [converted to string](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="0801d-133">После настройки свойство ключа секции всегда должно иметь значение, отличное от NULL.</span><span class="sxs-lookup"><span data-stu-id="0801d-133">Once configured the partition key property should always have a non-null value.</span></span> <span data-ttu-id="0801d-134">При выдаче запроса можно добавить условие, чтобы сделать единую секцию.</span><span class="sxs-lookup"><span data-stu-id="0801d-134">When issuing a query a condition can be added to make it single-partition.</span></span>

[!code-csharp[PartitionKey](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=PartitionKey)]

## <a name="embedded-entities"></a><span data-ttu-id="0801d-135">Внедренные сущности</span><span class="sxs-lookup"><span data-stu-id="0801d-135">Embedded entities</span></span>

<span data-ttu-id="0801d-136">В Cosmos зависимые сущности внедряются в тот же элемент, что и владелец.</span><span class="sxs-lookup"><span data-stu-id="0801d-136">For Cosmos owned entities are embedded in the same item as the owner.</span></span> <span data-ttu-id="0801d-137">Изменить имя свойства можно с помощью [ToJsonProperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty).</span><span class="sxs-lookup"><span data-stu-id="0801d-137">To change a property name use [ToJsonProperty](/dotnet/api/Microsoft.EntityFrameworkCore.CosmosEntityTypeBuilderExtensions.ToJsonProperty):</span></span>

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

<span data-ttu-id="0801d-138">В такой конфигурации порядок из приведенного выше примера сохраняется так:</span><span class="sxs-lookup"><span data-stu-id="0801d-138">With this configuration the order from the example above is stored like this:</span></span>

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
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-692e763901d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163674
}
```

<span data-ttu-id="0801d-139">Также внедряются коллекции зависимых сущностей.</span><span class="sxs-lookup"><span data-stu-id="0801d-139">Collections of owned entities are embedded as well.</span></span> <span data-ttu-id="0801d-140">В следующем примере мы будем использовать класс `Distributor` с коллекцией `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="0801d-140">For the next example we'll use the `Distributor` class with a collection of `StreetAddress`:</span></span>

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

<span data-ttu-id="0801d-141">Зависимым сущностям не нужно предоставлять значения ключа явным образом для хранения:</span><span class="sxs-lookup"><span data-stu-id="0801d-141">The owned entities don't need to provide explicit key values to be stored:</span></span>

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

<span data-ttu-id="0801d-142">Они будут сохранены так:</span><span class="sxs-lookup"><span data-stu-id="0801d-142">They will be persisted in this way:</span></span>

``` json
{
    "Id": 1,
    "Discriminator": "Distributor",
    "id": "Distributor|1",
    "ShippingCenters": [
        {
            "City": "Phoenix",
            "Street": "500 S 48th Street"
        },
        {
            "City": "Anaheim",
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

<span data-ttu-id="0801d-143">В EF Core всегда должны присутствовать уникальные значения ключа для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="0801d-143">Internally EF Core always needs to have unique key values for all tracked entities.</span></span> <span data-ttu-id="0801d-144">Первичный ключ, создаваемый по умолчанию для коллекций зависимых типов, состоит из свойств внешнего ключа, указывающих на владельца, и свойства `int`, соответствующего индексу в массиве JSON.</span><span class="sxs-lookup"><span data-stu-id="0801d-144">The primary key created by default for collections of owned types consists of the foreign key properties pointing to the owner and an `int` property corresponding to the index in the JSON array.</span></span> <span data-ttu-id="0801d-145">Чтобы получить запись этих значений, можно использовать API:</span><span class="sxs-lookup"><span data-stu-id="0801d-145">To retrieve these values entry API could be used:</span></span>

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> <span data-ttu-id="0801d-146">При необходимости можно изменить первичный ключ по умолчанию для принадлежащих типов сущностей, а затем указать значения ключа явным образом.</span><span class="sxs-lookup"><span data-stu-id="0801d-146">When necessary the default primary key for the owned entity types can be changed, but then key values should be provided explicitly.</span></span>

## <a name="working-with-disconnected-entities"></a><span data-ttu-id="0801d-147">Работа с отключенными сущностями</span><span class="sxs-lookup"><span data-stu-id="0801d-147">Working with disconnected entities</span></span>

<span data-ttu-id="0801d-148">Каждый элемент должен иметь значение `id`, уникальное для определенного ключа секции.</span><span class="sxs-lookup"><span data-stu-id="0801d-148">Every item needs to have an `id` value that is unique for the given partition key.</span></span> <span data-ttu-id="0801d-149">По умолчанию EF Core создает значение, объединяя дискриминатор и значения первичного ключа с помощью символа | в качестве разделителя.</span><span class="sxs-lookup"><span data-stu-id="0801d-149">By default EF Core generates the value by concatenating the discriminator and the primary key values, using '|' as a delimiter.</span></span> <span data-ttu-id="0801d-150">Значения ключа создаются, только если сущность принимает состояние `Added`.</span><span class="sxs-lookup"><span data-stu-id="0801d-150">The key values are only generated when an entity enters the `Added` state.</span></span> <span data-ttu-id="0801d-151">Это может вызвать проблему при [присоединении сущностей](../../saving/disconnected-entities.md), если у них нет свойства `id` для типа .NET, чтобы хранить значение.</span><span class="sxs-lookup"><span data-stu-id="0801d-151">This might pose a problem when [attaching entities](../../saving/disconnected-entities.md) if they don't have an `id` property on the .NET type to store the value.</span></span>

<span data-ttu-id="0801d-152">Чтобы обойти это ограничение, можно создать и задать значение `id` вручную или пометить сущность как добавленную первой, а затем придать ей нужное состояние:</span><span class="sxs-lookup"><span data-stu-id="0801d-152">To work around this limitation one could create and set the `id` value manually or mark the entity as added first, then changing it to the desired state:</span></span>

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

<span data-ttu-id="0801d-153">В результате мы получим такой файл JSON:</span><span class="sxs-lookup"><span data-stu-id="0801d-153">This is the resulting JSON:</span></span>

``` json
{
    "Id": 1,
    "Discriminator": "Distributor",
    "id": "Distributor|1",
    "ShippingCenters": [
        {
            "City": "Phoenix",
            "Street": "500 S 48th Street"
        }
    ],
    "_rid": "JBwtAN8oNYEBAAAAAAAAAA==",
    "_self": "dbs/JBwtAA==/colls/JBwtAN8oNYE=/docs/JBwtAN8oNYEBAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-9377-d7a1ae7c01d5\"",
    "_attachments": "attachments/",
    "_ts": 1572917100
}
```
