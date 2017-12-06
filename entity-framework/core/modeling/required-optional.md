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
# <a name="required-and-optional-properties"></a><span data-ttu-id="db666-102">Обязательные и необязательные свойства</span><span class="sxs-lookup"><span data-stu-id="db666-102">Required and Optional Properties</span></span>

<span data-ttu-id="db666-103">Свойство является обязательным, если он допустим для того, чтобы содержать `null`.</span><span class="sxs-lookup"><span data-stu-id="db666-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="db666-104">Если `null` не является допустимым значением для связанной со свойством, то он рассматривается как требуемое свойство.</span><span class="sxs-lookup"><span data-stu-id="db666-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="db666-105">Соглашения</span><span class="sxs-lookup"><span data-stu-id="db666-105">Conventions</span></span>

<span data-ttu-id="db666-106">По соглашению, свойство, типом CLR может содержать значение null будет настроен как необязательные (`string`, `int?`, `byte[]`и т. д.).</span><span class="sxs-lookup"><span data-stu-id="db666-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="db666-107">Свойства среды CLR, тип которого не может содержать значение null будет настроен при необходимости (`int`, `decimal`, `bool`и т. д.).</span><span class="sxs-lookup"><span data-stu-id="db666-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="db666-108">Свойство CLR, тип которого не может содержать значение null, нельзя задать как необязательные.</span><span class="sxs-lookup"><span data-stu-id="db666-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="db666-109">Свойство будет всегда считается предусмотренного Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="db666-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="db666-110">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="db666-110">Data Annotations</span></span>

<span data-ttu-id="db666-111">Заметок к данным можно использовать, чтобы указать, что свойство является обязательным.</span><span class="sxs-lookup"><span data-stu-id="db666-111">You can use Data Annotations to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="db666-112">Fluent API</span><span class="sxs-lookup"><span data-stu-id="db666-112">Fluent API</span></span>

<span data-ttu-id="db666-113">Fluent API можно использовать, чтобы указать, что свойство является обязательным.</span><span class="sxs-lookup"><span data-stu-id="db666-113">You can use the Fluent API to indicate that a property is required.</span></span>

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
