---
title: "Сопоставление столбцов - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
ms.technology: entity-framework-core
uid: core/modeling/relational/columns
ms.openlocfilehash: 697b966dbac892e332fe65feaa4dd11f00dd8298
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="column-mapping"></a><span data-ttu-id="4ad1f-102">Сопоставление столбцов</span><span class="sxs-lookup"><span data-stu-id="4ad1f-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="4ad1f-103">В этом разделе конфигурации применяется для реляционных баз данных в целом.</span><span class="sxs-lookup"><span data-stu-id="4ad1f-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="4ad1f-104">Методы расширения, показанный здесь будут доступны после установки поставщика реляционной базы данных (из-за общей *Microsoft.EntityFrameworkCore.Relational* пакета).</span><span class="sxs-lookup"><span data-stu-id="4ad1f-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="4ad1f-105">Сопоставление столбцов определяет, какие данные столбца должны запрашивать у и сохранены в базе данных.</span><span class="sxs-lookup"><span data-stu-id="4ad1f-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="4ad1f-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="4ad1f-106">Conventions</span></span>

<span data-ttu-id="4ad1f-107">По соглашению каждое свойство будет настроен для сопоставления со столбцом с тем же именем, что и свойство.</span><span class="sxs-lookup"><span data-stu-id="4ad1f-107">By convention, each property will be setup to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="4ad1f-108">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="4ad1f-108">Data Annotations</span></span>

<span data-ttu-id="4ad1f-109">Данные заметки можно использовать для настройки столбцов, с которым сопоставлено свойство.</span><span class="sxs-lookup"><span data-stu-id="4ad1f-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=3)] -->
``` csharp
public class Blog
{
    [Column("blog_id")]
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="4ad1f-110">Fluent API</span><span class="sxs-lookup"><span data-stu-id="4ad1f-110">Fluent API</span></span>

<span data-ttu-id="4ad1f-111">Fluent API можно использовать для настройки столбцов, с которым сопоставлено свойство.</span><span class="sxs-lookup"><span data-stu-id="4ad1f-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

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
