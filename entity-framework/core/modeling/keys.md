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
# <a name="keys-primary"></a>Ключи (основной)

Ключ служит в качестве первичного уникальный идентификатор для каждого экземпляра сущности. При использовании реляционной базы данных сопоставляется с понятием *первичный ключ*. Также можно настроить уникальный идентификатор, который не является первичным ключом (см. [альтернативные ключи](alternate-keys.md) для получения дополнительной информации).

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

Данные заметки можно использовать для настройки одного свойства в качестве ключа сущности.

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

## <a name="fluent-api"></a>Fluent API

Fluent API можно использовать для настройки одного свойства в качестве ключа сущности.

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

Можно также использовать Fluent API для настройки нескольких свойств в качестве ключа сущности (известных как составной ключ). Составные ключи можно настроить только с помощью плавного API - соглашения никогда не настроит составной ключ и заметок к данным нельзя использовать для его настройки.

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
