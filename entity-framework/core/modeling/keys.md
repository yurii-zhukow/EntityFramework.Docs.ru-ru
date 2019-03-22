---
title: Ключи (первичные) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 6272e323b83ccab2ed060a2ebbde1d1e8e353d66
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319170"
---
# <a name="keys-primary"></a><span data-ttu-id="5b793-102">Ключи (первичные)</span><span class="sxs-lookup"><span data-stu-id="5b793-102">Keys (primary)</span></span>

<span data-ttu-id="5b793-103">Ключ служит в качестве основного уникальный идентификатор для каждого экземпляра сущности.</span><span class="sxs-lookup"><span data-stu-id="5b793-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="5b793-104">При использовании реляционной базы данных это значение соответствует значению концепцию *первичный ключ*.</span><span class="sxs-lookup"><span data-stu-id="5b793-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="5b793-105">Вы также можете настроить уникальный идентификатор, который не является первичным ключом (см. в разделе [альтернативные ключи](alternate-keys.md) Дополнительные сведения).</span><span class="sxs-lookup"><span data-stu-id="5b793-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span> 

<span data-ttu-id="5b793-106">Один из следующих методов можно использовать для установки и создайте первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="5b793-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="5b793-107">Соглашения</span><span class="sxs-lookup"><span data-stu-id="5b793-107">Conventions</span></span>

<span data-ttu-id="5b793-108">По соглашению, свойство с именем `Id` или `<type name>Id` будет настроен в качестве ключа сущности.</span><span class="sxs-lookup"><span data-stu-id="5b793-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="5b793-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="5b793-109">Data Annotations</span></span>

<span data-ttu-id="5b793-110">Заметки к данным можно использовать для настройки одно свойство в качестве ключа сущности.</span><span class="sxs-lookup"><span data-stu-id="5b793-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

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

## <a name="fluent-api"></a><span data-ttu-id="5b793-111">Текучий API</span><span class="sxs-lookup"><span data-stu-id="5b793-111">Fluent API</span></span>

<span data-ttu-id="5b793-112">Fluent API можно использовать для настройки одно свойство в качестве ключа сущности.</span><span class="sxs-lookup"><span data-stu-id="5b793-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

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

<span data-ttu-id="5b793-113">Fluent API также можно настроить несколько свойств в качестве ключа сущности (известный как составной ключ).</span><span class="sxs-lookup"><span data-stu-id="5b793-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="5b793-114">Составные ключи можно настроить только с помощью Fluent API — соглашения никогда не настроит составной ключ и заметки к данным нельзя использовать для его настройки.</span><span class="sxs-lookup"><span data-stu-id="5b793-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

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
