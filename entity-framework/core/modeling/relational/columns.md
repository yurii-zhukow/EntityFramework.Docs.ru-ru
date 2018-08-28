---
title: Сопоставление столбцов — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: bca9ca22d211aa58a3bba00f6e4d54b8fe4a0df8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996208"
---
# <a name="column-mapping"></a><span data-ttu-id="d8850-102">Сопоставление столбцов</span><span class="sxs-lookup"><span data-stu-id="d8850-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="d8850-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="d8850-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="d8850-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="d8850-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="d8850-105">Сопоставление столбцов определяет, какие данные столбца следует оттуда и сохранены в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d8850-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="d8850-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="d8850-106">Conventions</span></span>

<span data-ttu-id="d8850-107">По соглашению каждое свойство будет настраиваться для сопоставления со столбцом с тем же именем, как свойство.</span><span class="sxs-lookup"><span data-stu-id="d8850-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d8850-108">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="d8850-108">Data Annotations</span></span>

<span data-ttu-id="d8850-109">Заметки к данным можно использовать для настройки столбца, с которым сопоставлено свойство.</span><span class="sxs-lookup"><span data-stu-id="d8850-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=3)] -->
``` csharp
public class Blog
{
    [Column("blog_id")]
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="d8850-110">Текучий API</span><span class="sxs-lookup"><span data-stu-id="d8850-110">Fluent API</span></span>

<span data-ttu-id="d8850-111">Fluent API можно использовать для настройки столбца, с которым сопоставлено свойство.</span><span class="sxs-lookup"><span data-stu-id="d8850-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.BlogId)
            .HasColumnName("blog_id");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
