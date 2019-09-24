---
title: Значения по умолчанию — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: 0d3613606f21a78e22cfe0ee752ea982a6a17f93
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196987"
---
# <a name="default-values"></a><span data-ttu-id="f3b02-102">Значения по умолчанию</span><span class="sxs-lookup"><span data-stu-id="f3b02-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="f3b02-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="f3b02-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="f3b02-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="f3b02-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="f3b02-105">По умолчанию столбец имеет значение, которое будет вставлено при вставке новой строки, но для столбца не указано значение.</span><span class="sxs-lookup"><span data-stu-id="f3b02-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="f3b02-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="f3b02-106">Conventions</span></span>

<span data-ttu-id="f3b02-107">По соглашению значение по умолчанию не настроено.</span><span class="sxs-lookup"><span data-stu-id="f3b02-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="f3b02-108">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="f3b02-108">Data Annotations</span></span>

<span data-ttu-id="f3b02-109">Нельзя задать значение по умолчанию с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="f3b02-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="f3b02-110">Текучий API</span><span class="sxs-lookup"><span data-stu-id="f3b02-110">Fluent API</span></span>

<span data-ttu-id="f3b02-111">API-интерфейс Fluent можно использовать для указания значения по умолчанию для свойства.</span><span class="sxs-lookup"><span data-stu-id="f3b02-111">You can use the Fluent API to specify the default value for a property.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultValue.cs?highlight=9)] -->
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

<span data-ttu-id="f3b02-112">Можно также указать фрагмент SQL, используемый для вычисления значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f3b02-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultValueSql.cs?highlight=9)] -->
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
