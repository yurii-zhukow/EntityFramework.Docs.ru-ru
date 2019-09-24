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
# <a name="keys-primary"></a><span data-ttu-id="e8034-102">Ключи (основной)</span><span class="sxs-lookup"><span data-stu-id="e8034-102">Keys (primary)</span></span>

<span data-ttu-id="e8034-103">Ключ служит в качестве первичного уникального идентификатора для каждого экземпляра сущности.</span><span class="sxs-lookup"><span data-stu-id="e8034-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="e8034-104">При использовании реляционной базы данных эта схема сопоставляется с понятием *первичного ключа*.</span><span class="sxs-lookup"><span data-stu-id="e8034-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="e8034-105">Можно также настроить уникальный идентификатор, который не является первичным ключом (Дополнительные сведения см. в разделе [альтернативные ключи](alternate-keys.md) ).</span><span class="sxs-lookup"><span data-stu-id="e8034-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span> 

<span data-ttu-id="e8034-106">Для настройки или создания первичного ключа можно использовать один из следующих методов.</span><span class="sxs-lookup"><span data-stu-id="e8034-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="e8034-107">Соглашения</span><span class="sxs-lookup"><span data-stu-id="e8034-107">Conventions</span></span>

<span data-ttu-id="e8034-108">По соглашению свойство с именем `Id` или `<type name>Id` будет настроено в качестве ключа сущности.</span><span class="sxs-lookup"><span data-stu-id="e8034-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="e8034-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="e8034-109">Data Annotations</span></span>

<span data-ttu-id="e8034-110">Заметки к данным можно использовать для настройки одного свойства в качестве ключа сущности.</span><span class="sxs-lookup"><span data-stu-id="e8034-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="e8034-111">Текучий API</span><span class="sxs-lookup"><span data-stu-id="e8034-111">Fluent API</span></span>

<span data-ttu-id="e8034-112">API-интерфейс Fluent можно использовать для настройки одного свойства в качестве ключа сущности.</span><span class="sxs-lookup"><span data-stu-id="e8034-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="e8034-113">Можно также использовать API Fluent, чтобы настроить несколько свойств в качестве ключа сущности (называемого составным ключом).</span><span class="sxs-lookup"><span data-stu-id="e8034-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="e8034-114">Составные ключи могут быть настроены только с помощью правил API Fluent, но не будут настраивать составной ключ, и вы не сможете использовать заметки к данным для их настройки.</span><span class="sxs-lookup"><span data-stu-id="e8034-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
