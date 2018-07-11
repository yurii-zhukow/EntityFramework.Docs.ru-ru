---
title: Сопоставление таблиц — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: ef6080b5d543c2a68a680be2b9effc1c9d531030
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949013"
---
# <a name="table-mapping"></a><span data-ttu-id="72239-102">Сопоставление таблиц</span><span class="sxs-lookup"><span data-stu-id="72239-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="72239-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="72239-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="72239-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="72239-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="72239-105">Сопоставление таблиц определяет, какие данные таблицы следует оттуда и сохранены в базе данных.</span><span class="sxs-lookup"><span data-stu-id="72239-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="72239-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="72239-106">Conventions</span></span>

<span data-ttu-id="72239-107">По соглашению каждая сущность будет настроен для сопоставления с таблицей с тем же именем, что `DbSet<TEntity>` свойство, которое предоставляет сущность в производном контексте.</span><span class="sxs-lookup"><span data-stu-id="72239-107">By convention, each entity will be set up to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="72239-108">Если нет `DbSet<TEntity>` включается для заданной сущности, используется имя класса.</span><span class="sxs-lookup"><span data-stu-id="72239-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="72239-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="72239-109">Data Annotations</span></span>

<span data-ttu-id="72239-110">Заметки к данным можно использовать для настройки в таблице, которая сопоставляется с типом.</span><span class="sxs-lookup"><span data-stu-id="72239-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="72239-111">Можно также указать схему, которой принадлежит таблица.</span><span class="sxs-lookup"><span data-stu-id="72239-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="72239-112">Текучий API</span><span class="sxs-lookup"><span data-stu-id="72239-112">Fluent API</span></span>

<span data-ttu-id="72239-113">Fluent API можно использовать для настройки в таблице, которая сопоставляется с типом.</span><span class="sxs-lookup"><span data-stu-id="72239-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="72239-114">Можно также указать схему, которой принадлежит таблица.</span><span class="sxs-lookup"><span data-stu-id="72239-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
