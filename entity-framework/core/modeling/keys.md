---
title: Ключи (основной) - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
ms.technology: entity-framework-core
uid: core/modeling/keys
ms.openlocfilehash: f3bf3c7f2a28e065b350fe000a5164406cd5ca08
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052574"
---
# <a name="keys-primary"></a><span data-ttu-id="a6456-102">Ключи (основной)</span><span class="sxs-lookup"><span data-stu-id="a6456-102">Keys (primary)</span></span>

<span data-ttu-id="a6456-103">Ключ служит в качестве первичного уникальный идентификатор для каждого экземпляра сущности.</span><span class="sxs-lookup"><span data-stu-id="a6456-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="a6456-104">При использовании реляционной базы данных сопоставляется с понятием *первичный ключ*.</span><span class="sxs-lookup"><span data-stu-id="a6456-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="a6456-105">Также можно настроить уникальный идентификатор, который не является первичным ключом (см. [альтернативные ключи](alternate-keys.md) для получения дополнительной информации).</span><span class="sxs-lookup"><span data-stu-id="a6456-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

## <a name="conventions"></a><span data-ttu-id="a6456-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="a6456-106">Conventions</span></span>

<span data-ttu-id="a6456-107">По соглашению, свойство с именем `Id` или `<type name>Id` будет настроен в качестве ключа сущности.</span><span class="sxs-lookup"><span data-stu-id="a6456-107">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="a6456-108">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="a6456-108">Data Annotations</span></span>

<span data-ttu-id="a6456-109">Данные заметки можно использовать для настройки одного свойства в качестве ключа сущности.</span><span class="sxs-lookup"><span data-stu-id="a6456-109">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=3,4)] -->
``` csharp
class Car
{
    [Key]
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="a6456-110">Fluent API</span><span class="sxs-lookup"><span data-stu-id="a6456-110">Fluent API</span></span>

<span data-ttu-id="a6456-111">Fluent API можно использовать для настройки одного свойства в качестве ключа сущности.</span><span class="sxs-lookup"><span data-stu-id="a6456-111">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasKey(c => c.LicensePlate);
    }
}

class Car
{
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<span data-ttu-id="a6456-112">Можно также использовать Fluent API для настройки нескольких свойств в качестве ключа сущности (известных как составной ключ).</span><span class="sxs-lookup"><span data-stu-id="a6456-112">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="a6456-113">Составные ключи можно настроить только с помощью плавного API - соглашения никогда не настроит составной ключ и заметок к данным нельзя использовать для его настройки.</span><span class="sxs-lookup"><span data-stu-id="a6456-113">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public string State { get; set; }
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```
