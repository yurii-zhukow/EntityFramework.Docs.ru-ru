---
title: Сопоставление таблиц - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: 73957d9c77e6856bfb65e10e6b373c337101f7d9
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052734"
---
# <a name="table-mapping"></a><span data-ttu-id="7a8e9-102">Сопоставление таблиц</span><span class="sxs-lookup"><span data-stu-id="7a8e9-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="7a8e9-103">В этом разделе конфигурации применяется для реляционных баз данных в целом.</span><span class="sxs-lookup"><span data-stu-id="7a8e9-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="7a8e9-104">Методы расширения, показанный здесь будут доступны после установки поставщика реляционной базы данных (из-за общей *Microsoft.EntityFrameworkCore.Relational* пакета).</span><span class="sxs-lookup"><span data-stu-id="7a8e9-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="7a8e9-105">Сопоставление таблицы определяет, какие данные таблицы необходимо запрашивать у и сохранены в базе данных.</span><span class="sxs-lookup"><span data-stu-id="7a8e9-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="7a8e9-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="7a8e9-106">Conventions</span></span>

<span data-ttu-id="7a8e9-107">По соглашению, каждая сущность будет настроен в сопоставляется с таблицей с тем же именем, что `DbSet<TEntity>` свойство, которое предоставляет сущности в контексте производной.</span><span class="sxs-lookup"><span data-stu-id="7a8e9-107">By convention, each entity will be setup to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="7a8e9-108">Если не `DbSet<TEntity>` включено для заданной сущности, используется имя класса.</span><span class="sxs-lookup"><span data-stu-id="7a8e9-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="7a8e9-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="7a8e9-109">Data Annotations</span></span>

<span data-ttu-id="7a8e9-110">Данные заметки можно использовать для настройки в таблице, которая сопоставляется с типом.</span><span class="sxs-lookup"><span data-stu-id="7a8e9-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="7a8e9-111">Можно также указать схему, которой принадлежит таблица.</span><span class="sxs-lookup"><span data-stu-id="7a8e9-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="7a8e9-112">Fluent API</span><span class="sxs-lookup"><span data-stu-id="7a8e9-112">Fluent API</span></span>

<span data-ttu-id="7a8e9-113">Fluent API можно использовать для настройки в таблице, которая сопоставляется с типом.</span><span class="sxs-lookup"><span data-stu-id="7a8e9-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="7a8e9-114">Можно также указать схему, которой принадлежит таблица.</span><span class="sxs-lookup"><span data-stu-id="7a8e9-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
