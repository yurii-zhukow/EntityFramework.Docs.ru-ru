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
# <a name="ef-core-azure-cosmos-db-provider"></a>Поставщик EF Core для Azure Cosmos DB

>[!NOTE]
> Этот поставщик является новым в EF Core 3.0.

Этот поставщик базы данных позволяет использовать Entity Framework Core с Azure Cosmos DB. Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

Перед чтением этого раздела мы настоятельно рекомендуем ознакомиться с [документацией по Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction).

## <a name="install"></a>Установка

Установите [пакет NuGet Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).

# <a name="net-core-clitabdotnet-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

# <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a>Приступая к работе

> [!TIP]  
> Вы можете скачать используемый в этой статье [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos) из репозитория GitHub.

Как и для других поставщиков, сначала нужно вызвать `UseCosmos`:

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> Для простоты конечная точка и ключ здесь включены в сам код, но в рабочем приложении необходимо обеспечить их [безопасное хранение](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).

В этом примере `Order` представляет собой простую сущность, ссылающуюся на `StreetAddress` [зависимого типа](../../modeling/owned-entities.md).

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

Сохранение данных и запрос на их получение выполняется, как обычно в EF:

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> Путем вызова `EnsureCreated` вы создадите требуемые коллекции и вставите [начальные данные](../../modeling/data-seeding.md), если они есть в модели. Но `EnsureCreated` следует вызывать только во время развертывания, а не в рабочем режиме, так как это может вызвать проблемы с производительностью.

## <a name="cosmos-specific-model-customization"></a>Настройка моделей для Cosmos

По умолчанию все типы сущностей сопоставляются с одним и тем же контейнером с именем на основе производного контекста (в нашем примере `"OrderContext"`). Изменить имя контейнера по умолчанию можно с помощью `HasDefaultContainer`:

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

Сопоставить тип сущности с другим контейнером можно с помощью `ToContainer`:

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

Чтобы определить тип сущности элемента, EF Core добавляет значение дискриминатора, даже если у элемента нет производных типов сущности. Имя и значение дискриминатора [можно изменить](../../modeling/inheritance.md).

## <a name="embedded-entities"></a>Внедренные сущности

В Cosmos зависимые сущности внедряются в тот же элемент, что и владелец. Изменить имя свойства можно с помощью `ToJsonProperty`:

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

В такой конфигурации порядок из приведенного выше примера сохраняется так:

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

Также внедряются коллекции зависимых сущностей. В следующем примере мы будем использовать класс `Distributor` с коллекцией `StreetAddress`:

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

Зависимым сущностям не нужно предоставлять значения ключа явным образом для хранения:

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

Они будут сохранены так:

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

В EF Core всегда должны присутствовать уникальные значения ключа для всех отслеживаемых сущностей. Первичный ключ, создаваемый по умолчанию для коллекций зависимых типов, состоит из свойств внешнего ключа, указывающих на владельца, и свойства `int`, соответствующего индексу в массиве JSON. Чтобы получить запись этих значений, можно использовать API:

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> При необходимости можно изменить первичный ключ по умолчанию для принадлежащих типов сущностей, а затем указать значения ключа явным образом.

## <a name="working-with-disconnected-entities"></a>Работа с отключенными сущностями

Каждый элемент должен иметь значение `id`, уникальное для определенного ключа секции. По умолчанию EF Core создает значение, объединяя дискриминатор и значения первичного ключа с помощью символа | в качестве разделителя. Значения ключа создаются, только если сущность принимает состояние `Added`. Это может вызвать проблему при [присоединении сущностей](../../saving/disconnected-entities.md), если у них нет свойства `id` для типа .NET, чтобы хранить значение.

Чтобы обойти это ограничение, можно создать и задать значение `id` вручную или пометить сущность как добавленную первой, а затем придать ей нужное состояние:

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

В результате мы получим такой файл JSON:

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
