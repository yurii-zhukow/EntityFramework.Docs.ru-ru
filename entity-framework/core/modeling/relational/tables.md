---
title: Сопоставление таблиц — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 62dce317b901bc862b3c7d20ed1d176805bb24dd
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196968"
---
# <a name="table-mapping"></a><span data-ttu-id="294d2-102">Сопоставление таблиц</span><span class="sxs-lookup"><span data-stu-id="294d2-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="294d2-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="294d2-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="294d2-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="294d2-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="294d2-105">Сопоставление таблиц определяет, какие табличные данные следует запрашивать и сохранять в базе данных.</span><span class="sxs-lookup"><span data-stu-id="294d2-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="294d2-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="294d2-106">Conventions</span></span>

<span data-ttu-id="294d2-107">По соглашению каждая сущность будет настроена для сопоставления с таблицей с именем, указанным в свойстве `DbSet<TEntity>`, которое предоставляет сущность в производном контексте.</span><span class="sxs-lookup"><span data-stu-id="294d2-107">By convention, each entity will be set up to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="294d2-108">Если для `DbSet<TEntity>` данной сущности не включено значение, используется имя класса.</span><span class="sxs-lookup"><span data-stu-id="294d2-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="294d2-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="294d2-109">Data Annotations</span></span>

<span data-ttu-id="294d2-110">Заметки к данным можно использовать для настройки таблицы, сопоставляемой с типом.</span><span class="sxs-lookup"><span data-stu-id="294d2-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

``` csharp
using System.ComponentModel.DataAnnotations.Schema;
```
``` csharp
[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="294d2-111">Можно также указать схему, которой принадлежит таблица.</span><span class="sxs-lookup"><span data-stu-id="294d2-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="294d2-112">Текучий API</span><span class="sxs-lookup"><span data-stu-id="294d2-112">Fluent API</span></span>

<span data-ttu-id="294d2-113">API-интерфейс Fluent можно использовать для настройки таблицы, сопоставляемой с типом.</span><span class="sxs-lookup"><span data-stu-id="294d2-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
```
``` csharp
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

<span data-ttu-id="294d2-114">Можно также указать схему, которой принадлежит таблица.</span><span class="sxs-lookup"><span data-stu-id="294d2-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
