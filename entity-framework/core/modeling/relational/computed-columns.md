---
title: Вычисляемые столбцы - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
ms.technology: entity-framework-core
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 95312504286bd34cc666b5a21273835c4b35d379
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052484"
---
# <a name="computed-columns"></a>Вычисляемые столбцы

> [!NOTE]  
> В этом разделе конфигурации применяется для реляционных баз данных в целом. Методы расширения, показанный здесь будут доступны после установки поставщика реляционной базы данных (из-за общей *Microsoft.EntityFrameworkCore.Relational* пакета).

Вычисляемый столбец является столбцом, значение которого вычисляется в базе данных. Вычисляемый столбец можно использовать другие столбцы в таблице, для вычисления его значения.

## <a name="conventions"></a>Соглашения

По соглашению вычисляемые столбцы не создаются в модели.

## <a name="data-annotations"></a>Заметки к данным

Вычисляемые столбцы могут не настроены заметок к данным.

## <a name="fluent-api"></a>Fluent API

Fluent API можно использовать для указания, что свойство должно сопоставляться с вычисляемым столбцом.

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
