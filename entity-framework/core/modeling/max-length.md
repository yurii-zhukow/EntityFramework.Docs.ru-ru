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
# <a name="maximum-length"></a><span data-ttu-id="bd743-102">Максимальная длина</span><span class="sxs-lookup"><span data-stu-id="bd743-102">Maximum Length</span></span>

<span data-ttu-id="bd743-103">Настройка максимальной длиной предоставляет подсказку в хранилище данных о соответствующего типа данных для заданного свойства.</span><span class="sxs-lookup"><span data-stu-id="bd743-103">Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property.</span></span> <span data-ttu-id="bd743-104">Максимальная длина применяется только к типы данных массивов, такие как `string` и `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="bd743-104">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]  
> <span data-ttu-id="bd743-105">Платформа Entity Framework не выполняет проверку максимальной длиной перед передачей данных поставщика.</span><span class="sxs-lookup"><span data-stu-id="bd743-105">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="bd743-106">Именно поставщик или хранилище данных для проверки, если это необходимо.</span><span class="sxs-lookup"><span data-stu-id="bd743-106">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="bd743-107">Например когда для различных версий SQL Server, превышение максимальной длины будет привести к возникновению исключения как тип данных базового столбца не позволит избыточные данные должны храниться.</span><span class="sxs-lookup"><span data-stu-id="bd743-107">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

## <a name="conventions"></a><span data-ttu-id="bd743-108">Соглашения</span><span class="sxs-lookup"><span data-stu-id="bd743-108">Conventions</span></span>

<span data-ttu-id="bd743-109">По соглашению он остается до поставщик базы данных, выберите соответствующий тип данных для свойства.</span><span class="sxs-lookup"><span data-stu-id="bd743-109">By convention, it is left up to the database provider to choose an appropriate data type for properties.</span></span> <span data-ttu-id="bd743-110">Для свойств, которые имеют длину поставщик базы данных обычно будет выберите тип данных, который позволяет длинного длину данных.</span><span class="sxs-lookup"><span data-stu-id="bd743-110">For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data.</span></span> <span data-ttu-id="bd743-111">Например, Microsoft SQL Server будет использовать `nvarchar(max)` для `string` свойства (или `nvarchar(450)` Если столбец используется в качестве ключа).</span><span class="sxs-lookup"><span data-stu-id="bd743-111">For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="bd743-112">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="bd743-112">Data Annotations</span></span>

<span data-ttu-id="bd743-113">Заметки данных можно использовать для настройки максимальную длину для свойства.</span><span class="sxs-lookup"><span data-stu-id="bd743-113">You can use the Data Annotations to configure a maximum length for a property.</span></span> <span data-ttu-id="bd743-114">В этом примере для различных версий SQL Server, в результате `nvarchar(500)` используемого типа данных.</span><span class="sxs-lookup"><span data-stu-id="bd743-114">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="bd743-115">Fluent API</span><span class="sxs-lookup"><span data-stu-id="bd743-115">Fluent API</span></span>

<span data-ttu-id="bd743-116">Fluent API позволяет настроить максимальную длину для свойства.</span><span class="sxs-lookup"><span data-stu-id="bd743-116">You can use the Fluent API to configure a maximum length for a property.</span></span> <span data-ttu-id="bd743-117">В этом примере для различных версий SQL Server, в результате `nvarchar(500)` используемого типа данных.</span><span class="sxs-lookup"><span data-stu-id="bd743-117">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

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
