---
title: "Максимальная длина - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
ms.technology: entity-framework-core
uid: core/modeling/max-length
ms.openlocfilehash: 7325c0c3328477473392bf9e7c82f1696bb4f424
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="maximum-length"></a>Максимальная длина

Настройка максимальной длиной предоставляет подсказку в хранилище данных о соответствующего типа данных для заданного свойства. Максимальная длина применяется только к типы данных массивов, такие как `string` и `byte[]`.

> [!NOTE]  
> Платформа Entity Framework не выполняет проверку максимальной длиной перед передачей данных поставщика. Именно поставщик или хранилище данных для проверки, если это необходимо. Например когда для различных версий SQL Server, превышение максимальной длины будет привести к возникновению исключения как тип данных базового столбца не позволит избыточные данные должны храниться.

## <a name="conventions"></a>Соглашения

По соглашению он остается до поставщик базы данных, выберите соответствующий тип данных для свойства. Для свойств, которые имеют длину поставщик базы данных обычно будет выберите тип данных, который позволяет длинного длину данных. Например, Microsoft SQL Server будет использовать `nvarchar(max)` для `string` свойства (или `nvarchar(450)` Если столбец используется в качестве ключа).

## <a name="data-annotations"></a>Заметки к данным

Заметки данных можно использовать для настройки максимальную длину для свойства. В этом примере для различных версий SQL Server, в результате `nvarchar(500)` используемого типа данных.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Fluent API

Fluent API позволяет настроить максимальную длину для свойства. В этом примере для различных версий SQL Server, в результате `nvarchar(500)` используемого типа данных.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .HasMaxLength(500);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
