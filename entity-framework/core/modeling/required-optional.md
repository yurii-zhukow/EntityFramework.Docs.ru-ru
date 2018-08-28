---
title: Обязательные и необязательные свойства — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: b6716a5b03e1afc2933e317d606ef50f986c22c7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995501"
---
# <a name="required-and-optional-properties"></a>Обязательные и необязательные свойства

Свойство не является обязательным, если он является допустимым, чтобы содержать `null`. Если `null` не является допустимым значением для связанной со свойством, то он считается обязательным свойством.

## <a name="conventions"></a>Соглашения

По соглашению, свойство, тип которого CLR может содержать значение null будет настроен как необязательный (`string`, `int?`, `byte[]`и т. д.). Свойства, типом CLR не может содержать значение null, будут задаваться при необходимости (`int`, `decimal`, `bool`и т. д.).

> [!NOTE]  
> Свойство CLR, тип которого не может содержать значение null, нельзя настроить как необязательные. Свойство будет расцениваться как требуется для Entity Framework.

## <a name="data-annotations"></a>Заметки к данным

Заметки к данным можно использовать для указания, что свойство является обязательным.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Текучий API

Fluent API можно использовать для указания, что свойство является обязательным.

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
