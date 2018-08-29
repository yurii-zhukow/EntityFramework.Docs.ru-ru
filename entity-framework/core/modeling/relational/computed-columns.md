---
title: Вычисляемые столбцы — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: b88efdf69e5100e4eff55f3a41925d2d8e7c3178
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993957"
---
# <a name="computed-columns"></a>Вычисляемые столбцы

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

Вычисляемый столбец является столбцом, значение которого вычисляется в базе данных. Вычисляемый столбец могут использоваться другие столбцы в таблице, для которого требуется вычислить его значение.

## <a name="conventions"></a>Соглашения

По соглашению вычисляемые столбцы не создаются в модели.

## <a name="data-annotations"></a>Заметки к данным

Вычисляемые столбцы можно настроить не с заметками к данным.

## <a name="fluent-api"></a>Текучий API

Fluent API можно использовать для указания, что свойство должно сопоставляться вычисляемый столбец.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/ComputedColumn.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .Property(p => p.DisplayName)
            .HasComputedColumnSql("[LastName] + ', ' + [FirstName]");
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string DisplayName { get; set; }
}
```
