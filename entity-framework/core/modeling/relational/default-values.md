---
title: "Значения по умолчанию - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
ms.technology: entity-framework-core
uid: core/modeling/relational/default-values
ms.openlocfilehash: 73b916b6d9f9c984c8ea010f2319eafa7d031a58
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="default-values"></a><span data-ttu-id="22e85-102">Значения по умолчанию</span><span class="sxs-lookup"><span data-stu-id="22e85-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="22e85-103">В этом разделе конфигурации применяется для реляционных баз данных в целом.</span><span class="sxs-lookup"><span data-stu-id="22e85-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="22e85-104">Методы расширения, показанный здесь будут доступны после установки поставщика реляционной базы данных (из-за общей *Microsoft.EntityFrameworkCore.Relational* пакета).</span><span class="sxs-lookup"><span data-stu-id="22e85-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="22e85-105">Значение столбца по умолчанию является значение, которое будет вставлена, если новая строка вставляется, но не указано значение для столбца.</span><span class="sxs-lookup"><span data-stu-id="22e85-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="22e85-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="22e85-106">Conventions</span></span>

<span data-ttu-id="22e85-107">По соглашению значение по умолчанию не настроен.</span><span class="sxs-lookup"><span data-stu-id="22e85-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="22e85-108">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="22e85-108">Data Annotations</span></span>

<span data-ttu-id="22e85-109">Не удается назначить значение по умолчанию, с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="22e85-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="22e85-110">Fluent API</span><span class="sxs-lookup"><span data-stu-id="22e85-110">Fluent API</span></span>

<span data-ttu-id="22e85-111">Fluent API можно использовать для указания значения по умолчанию для свойства.</span><span class="sxs-lookup"><span data-stu-id="22e85-111">You can use the Fluent API to specify the default value for a property.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValue.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Rating)
            .HasDefaultValue(3);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public int Rating { get; set; }
}
```

<span data-ttu-id="22e85-112">Можно также указать фрагмент SQL, который используется для вычисления значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="22e85-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValueSql.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Created)
            .HasDefaultValueSql("getdate()");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public DateTime Created { get; set; }
}
```
