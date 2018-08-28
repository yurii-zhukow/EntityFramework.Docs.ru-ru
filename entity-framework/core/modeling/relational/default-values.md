---
title: Значения по умолчанию — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: 341f243ddddc345bb4236e5c34f814694b71e32a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996256"
---
# <a name="default-values"></a><span data-ttu-id="3bfe4-102">Значения по умолчанию</span><span class="sxs-lookup"><span data-stu-id="3bfe4-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="3bfe4-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="3bfe4-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="3bfe4-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="3bfe4-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="3bfe4-105">Значение по умолчанию столбца является значением, который будет вставлен при вставке новой строки, но значение не указано для столбца.</span><span class="sxs-lookup"><span data-stu-id="3bfe4-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="3bfe4-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="3bfe4-106">Conventions</span></span>

<span data-ttu-id="3bfe4-107">По соглашению значение по умолчанию не настроен.</span><span class="sxs-lookup"><span data-stu-id="3bfe4-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3bfe4-108">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="3bfe4-108">Data Annotations</span></span>

<span data-ttu-id="3bfe4-109">Значение по умолчанию, с помощью заметок к данным нельзя будет сделать.</span><span class="sxs-lookup"><span data-stu-id="3bfe4-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="3bfe4-110">Текучий API</span><span class="sxs-lookup"><span data-stu-id="3bfe4-110">Fluent API</span></span>

<span data-ttu-id="3bfe4-111">Fluent API можно использовать для указания значения по умолчанию для свойства.</span><span class="sxs-lookup"><span data-stu-id="3bfe4-111">You can use the Fluent API to specify the default value for a property.</span></span>

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

<span data-ttu-id="3bfe4-112">Можно также указать фрагмент SQL, который используется для вычисления значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3bfe4-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

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
