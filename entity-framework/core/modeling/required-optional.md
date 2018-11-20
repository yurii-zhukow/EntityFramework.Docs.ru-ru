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
# <a name="required-and-optional-properties"></a><span data-ttu-id="65b8c-102">Обязательные и необязательные свойства</span><span class="sxs-lookup"><span data-stu-id="65b8c-102">Required and Optional Properties</span></span>

<span data-ttu-id="65b8c-103">Свойство считается необязательным, если для него допустимо значение `null`.</span><span class="sxs-lookup"><span data-stu-id="65b8c-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="65b8c-104">Если `null` не является допустимым значением свойства, свойство считается обязательным.</span><span class="sxs-lookup"><span data-stu-id="65b8c-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="65b8c-105">Соглашения</span><span class="sxs-lookup"><span data-stu-id="65b8c-105">Conventions</span></span>

<span data-ttu-id="65b8c-106">По соглашению, свойство, для которого CLR-тип может иметь значение null, будет настроено как необязательное (`string`, `int?`, `byte[]`и т. д.).</span><span class="sxs-lookup"><span data-stu-id="65b8c-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="65b8c-107">Свойства, для которых CLR-тип не может иметь значение null, будут настроены как обязательные (`int`, `decimal`, `bool`и т. д.).</span><span class="sxs-lookup"><span data-stu-id="65b8c-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="65b8c-108">Свойство, для которого CLR-тип не может иметь значение null, не может быть настроено как необязательное.</span><span class="sxs-lookup"><span data-stu-id="65b8c-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="65b8c-109">Такое свойство всегда будет расцениваться Entity Framework как обязательное.</span><span class="sxs-lookup"><span data-stu-id="65b8c-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="65b8c-110">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="65b8c-110">Data Annotations</span></span>

<span data-ttu-id="65b8c-111">Заметки к данным можно использовать для указания, что свойство является обязательным.</span><span class="sxs-lookup"><span data-stu-id="65b8c-111">You can use Data Annotations to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="65b8c-112">Fluent API</span><span class="sxs-lookup"><span data-stu-id="65b8c-112">Fluent API</span></span>

<span data-ttu-id="65b8c-113">Fluent API можно использовать для указания того, что свойство является обязательным.</span><span class="sxs-lookup"><span data-stu-id="65b8c-113">You can use the Fluent API to indicate that a property is required.</span></span>

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
