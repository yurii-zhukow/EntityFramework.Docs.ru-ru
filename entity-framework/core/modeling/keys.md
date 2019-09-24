---
title: Ключи (основной) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 8b32bf6417890a954c933a5973a2c90c609beeca
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197268"
---
# <a name="keys-primary"></a>Ключи (основной)

Ключ служит в качестве первичного уникального идентификатора для каждого экземпляра сущности. При использовании реляционной базы данных эта схема сопоставляется с понятием *первичного ключа*. Можно также настроить уникальный идентификатор, который не является первичным ключом (Дополнительные сведения см. в разделе [альтернативные ключи](alternate-keys.md) ). 

Для настройки или создания первичного ключа можно использовать один из следующих методов.

## <a name="conventions"></a>Соглашения

По соглашению свойство с именем `Id` или `<type name>Id` будет настроено в качестве ключа сущности.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a>Заметки к данным

Заметки к данным можно использовать для настройки одного свойства в качестве ключа сущности.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>Текучий API

API-интерфейс Fluent можно использовать для настройки одного свойства в качестве ключа сущности.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

Можно также использовать API Fluent, чтобы настроить несколько свойств в качестве ключа сущности (называемого составным ключом). Составные ключи могут быть настроены только с помощью правил API Fluent, но не будут настраивать составной ключ, и вы не сможете использовать заметки к данным для их настройки.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
