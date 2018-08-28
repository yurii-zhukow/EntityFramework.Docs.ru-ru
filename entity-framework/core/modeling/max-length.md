---
title: Максимальная длина — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: e54d3671f378b96a49eaf4cb312e72072813fc6d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996195"
---
# <a name="maximum-length"></a>Максимальная длина

Настройка максимальной длиной предоставляет подсказку в хранилище данных о соответствующего типа данных для заданного свойства. Максимальная длина применяется только в типы данных массивов, такие как `string` и `byte[]`.

> [!NOTE]  
> Платформа Entity Framework не выполняет проверку максимальной длиной перед передачей данных к поставщику. Возлагается поставщик или хранилище данных для проверки при необходимости. Например когда исключение, так как тип данных базового столбца для различных версий SQL Server, превышающего максимальную длину приведет к не позволит избыточные данные для сохранения.

## <a name="conventions"></a>Соглашения

По соглашению он остается до поставщик базы данных, чтобы выбрать соответствующий тип данных для свойства. Для свойств, которые имеют длину поставщик базы данных обычно будет выбирать тип данных, который позволяет самая длинная длину данных. Например, Microsoft SQL Server будет использовать `nvarchar(max)` для `string` свойства (или `nvarchar(450)` Если столбец используется в качестве ключа).

## <a name="data-annotations"></a>Заметки к данным

Заметки данных можно использовать для настройки Максимальная длина для свойства. В этом примере для различных версий SQL Server, в результате `nvarchar(500)` используемого типа данных.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Текучий API

Fluent API можно использовать для настройки Максимальная длина для свойства. В этом примере для различных версий SQL Server, в результате `nvarchar(500)` используемого типа данных.

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
