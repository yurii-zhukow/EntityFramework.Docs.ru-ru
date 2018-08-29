---
title: Ключи (первичные) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 9e6946100ebabc6ba57cb792b3672219098b1e21
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994025"
---
# <a name="keys-primary"></a><span data-ttu-id="6c27a-102">Ключи (первичные)</span><span class="sxs-lookup"><span data-stu-id="6c27a-102">Keys (primary)</span></span>

<span data-ttu-id="6c27a-103">Ключ служит в качестве основного уникальный идентификатор для каждого экземпляра сущности.</span><span class="sxs-lookup"><span data-stu-id="6c27a-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="6c27a-104">При использовании реляционной базы данных это значение соответствует значению концепцию *первичный ключ*.</span><span class="sxs-lookup"><span data-stu-id="6c27a-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="6c27a-105">Вы также можете настроить уникальный идентификатор, который не является первичным ключом (см. в разделе [альтернативные ключи](alternate-keys.md) Дополнительные сведения).</span><span class="sxs-lookup"><span data-stu-id="6c27a-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

## <a name="conventions"></a><span data-ttu-id="6c27a-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="6c27a-106">Conventions</span></span>

<span data-ttu-id="6c27a-107">По соглашению, свойство с именем `Id` или `<type name>Id` будет настроен в качестве ключа сущности.</span><span class="sxs-lookup"><span data-stu-id="6c27a-107">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="6c27a-108">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="6c27a-108">Data Annotations</span></span>

<span data-ttu-id="6c27a-109">Заметки к данным можно использовать для настройки одно свойство в качестве ключа сущности.</span><span class="sxs-lookup"><span data-stu-id="6c27a-109">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

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

## <a name="fluent-api"></a><span data-ttu-id="6c27a-110">Текучий API</span><span class="sxs-lookup"><span data-stu-id="6c27a-110">Fluent API</span></span>

<span data-ttu-id="6c27a-111">Fluent API можно использовать для настройки одно свойство в качестве ключа сущности.</span><span class="sxs-lookup"><span data-stu-id="6c27a-111">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

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

<span data-ttu-id="6c27a-112">Fluent API также можно настроить несколько свойств в качестве ключа сущности (известный как составной ключ).</span><span class="sxs-lookup"><span data-stu-id="6c27a-112">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="6c27a-113">Составные ключи можно настроить только с помощью Fluent API — соглашения никогда не настроит составной ключ и заметки к данным нельзя использовать для его настройки.</span><span class="sxs-lookup"><span data-stu-id="6c27a-113">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

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
