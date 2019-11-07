---
title: Сопоставление таблиц — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 474c49aca4c65cd5d58b184b1f3c2d30e7abff84
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656106"
---
# <a name="table-mapping"></a><span data-ttu-id="7dc70-102">Сопоставление таблиц</span><span class="sxs-lookup"><span data-stu-id="7dc70-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="7dc70-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="7dc70-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="7dc70-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="7dc70-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="7dc70-105">Сопоставление таблиц определяет, какие табличные данные следует запрашивать и сохранять в базе данных.</span><span class="sxs-lookup"><span data-stu-id="7dc70-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="7dc70-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="7dc70-106">Conventions</span></span>

<span data-ttu-id="7dc70-107">По соглашению каждая сущность будет настроена для сопоставления с таблицей с именем, указанным в свойстве `DbSet<TEntity>`, которое предоставляет сущность в производном контексте.</span><span class="sxs-lookup"><span data-stu-id="7dc70-107">By convention, each entity will be set up to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="7dc70-108">Если `DbSet<TEntity>` не включено для данной сущности, используется имя класса.</span><span class="sxs-lookup"><span data-stu-id="7dc70-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="7dc70-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="7dc70-109">Data Annotations</span></span>

<span data-ttu-id="7dc70-110">Заметки к данным можно использовать для настройки таблицы, сопоставляемой с типом.</span><span class="sxs-lookup"><span data-stu-id="7dc70-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

``` csharp
using System.ComponentModel.DataAnnotations.Schema;

[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="7dc70-111">Можно также указать схему, которой принадлежит таблица.</span><span class="sxs-lookup"><span data-stu-id="7dc70-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="7dc70-112">Текучий API</span><span class="sxs-lookup"><span data-stu-id="7dc70-112">Fluent API</span></span>

<span data-ttu-id="7dc70-113">API-интерфейс Fluent можно использовать для настройки таблицы, сопоставляемой с типом.</span><span class="sxs-lookup"><span data-stu-id="7dc70-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;

class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .ToTable("blogs");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="7dc70-114">Можно также указать схему, которой принадлежит таблица.</span><span class="sxs-lookup"><span data-stu-id="7dc70-114">You can also specify a schema that the table belongs to.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/TableAndSchema.cs?name=Table&highlight=2)]
