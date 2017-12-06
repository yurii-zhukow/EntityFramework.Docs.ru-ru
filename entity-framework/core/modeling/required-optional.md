---
title: "Обязательные и необязательные свойства - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
ms.technology: entity-framework-core
uid: core/modeling/required-optional
ms.openlocfilehash: 2af1d49e12ef980f81cb9c00556dee471673ccae
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="required-and-optional-properties"></a>Обязательные и необязательные свойства

Свойство является обязательным, если он допустим для того, чтобы содержать `null`. Если `null` не является допустимым значением для связанной со свойством, то он рассматривается как требуемое свойство.

## <a name="conventions"></a>Соглашения

По соглашению, свойство, типом CLR может содержать значение null будет настроен как необязательные (`string`, `int?`, `byte[]`и т. д.). Свойства среды CLR, тип которого не может содержать значение null будет настроен при необходимости (`int`, `decimal`, `bool`и т. д.).

> [!NOTE]  
> Свойство CLR, тип которого не может содержать значение null, нельзя задать как необязательные. Свойство будет всегда считается предусмотренного Entity Framework.

## <a name="data-annotations"></a>Заметки к данным

Заметок к данным можно использовать, чтобы указать, что свойство является обязательным.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Fluent API

Fluent API можно использовать, чтобы указать, что свойство является обязательным.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
