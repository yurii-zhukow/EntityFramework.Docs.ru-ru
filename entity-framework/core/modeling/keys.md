---
title: Ключи (первичные) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 51d163b867085f42f415dbd7afa9e311ab1781a0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929840"
---
# <a name="keys-primary"></a>Ключи (первичные)

Ключ служит в качестве основного уникальный идентификатор для каждого экземпляра сущности. При использовании реляционной базы данных это значение соответствует значению концепцию *первичный ключ*. Вы также можете настроить уникальный идентификатор, который не является первичным ключом (см. в разделе [альтернативные ключи](alternate-keys.md) Дополнительные сведения). 

Один из следующих методов можно использовать для установки и создайте первичный ключ.

## <a name="conventions"></a>Соглашения

По соглашению, свойство с именем `Id` или `<type name>Id` будет настроен в качестве ключа сущности.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a>Заметки к данным

Заметки к данным можно использовать для настройки одно свойство в качестве ключа сущности.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>Текучий API

Fluent API можно использовать для настройки одно свойство в качестве ключа сущности.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=11,12)]

Fluent API также можно настроить несколько свойств в качестве ключа сущности (известный как составной ключ). Составные ключи можно настроить только с помощью Fluent API — соглашения никогда не настроит составной ключ и заметки к данным нельзя использовать для его настройки.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=11,12)]
