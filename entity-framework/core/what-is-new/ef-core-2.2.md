---
title: Новые возможности в EF Core 2.2 — EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 998C04F3-676A-4FCF-8450-CFB0457B4198
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: 5fcf7c6dfb4d8cb7928ef974af6deb52df7c63eb
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181375"
---
# <a name="new-features-in-ef-core-22"></a><span data-ttu-id="aaf33-102">Новые возможности в EF Core 2.2</span><span class="sxs-lookup"><span data-stu-id="aaf33-102">New features in EF Core 2.2</span></span>

## <a name="spatial-data-support"></a><span data-ttu-id="aaf33-103">Поддержка пространственных данных</span><span class="sxs-lookup"><span data-stu-id="aaf33-103">Spatial data support</span></span>

<span data-ttu-id="aaf33-104">Пространственные данные могут использоваться для представления физического расположения и формы объектов.</span><span class="sxs-lookup"><span data-stu-id="aaf33-104">Spatial data can be used to represent the physical location and shape of objects.</span></span>
<span data-ttu-id="aaf33-105">Многие базы данных имеют собственную поддержку хранения и индексирования пространственных данных, а также запросов к ним.</span><span class="sxs-lookup"><span data-stu-id="aaf33-105">Many databases can natively store, index, and query spatial data.</span></span> <span data-ttu-id="aaf33-106">Типичные сценарии использования включают запросы о наличии объектов в пределах указанного расстояния или для проверки того, содержит ли многоугольник указанное расположение.</span><span class="sxs-lookup"><span data-stu-id="aaf33-106">Common scenarios include querying for objects within a given distance, and testing if a polygon contains a given location.</span></span>
<span data-ttu-id="aaf33-107">Теперь EF Core 2.2 поддерживает работу с пространственными данными в различных базах данных, используя типы из библиотеки [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite).</span><span class="sxs-lookup"><span data-stu-id="aaf33-107">EF Core 2.2 now supports working with spatial data from various databases using types from the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS) library.</span></span>

<span data-ttu-id="aaf33-108">Поддержка пространственных данных реализована в виде набора пакетов расширений для конкретных поставщиков.</span><span class="sxs-lookup"><span data-stu-id="aaf33-108">Spatial data support is implemented as a series of provider-specific extension packages.</span></span>
<span data-ttu-id="aaf33-109">Каждый из этих пакетов содержит сопоставления для типов и методов NTS с соответствующими пространственными типами и функциями в базе данных.</span><span class="sxs-lookup"><span data-stu-id="aaf33-109">Each of these packages contributes mappings for NTS types and methods, and the corresponding spatial types and functions in the database.</span></span>
<span data-ttu-id="aaf33-110">Эти расширения доступны для поставщиков баз данных [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/) и [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (из [проекта Npgsql](https://www.npgsql.org/)).</span><span class="sxs-lookup"><span data-stu-id="aaf33-110">Such provider extensions are now available for [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), and [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (from the [Npgsql project](https://www.npgsql.org/)).</span></span>
<span data-ttu-id="aaf33-111">Пространственные типы можно использовать непосредственно с [поставщиком EF Core для выполняющейся в памяти базы данных](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/) без дополнительных расширений.</span><span class="sxs-lookup"><span data-stu-id="aaf33-111">Spatial types can be used directly with the [EF Core in-memory provider](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/) without additional extensions.</span></span>

<span data-ttu-id="aaf33-112">После установки расширения для поставщика можно добавлять к сущностям свойства поддерживаемых типов.</span><span class="sxs-lookup"><span data-stu-id="aaf33-112">Once the provider extension is installed, you can add properties of supported types to your entities.</span></span> <span data-ttu-id="aaf33-113">Например:</span><span class="sxs-lookup"><span data-stu-id="aaf33-113">For example:</span></span>

``` csharp
using NetTopologySuite.Geometries;

namespace MyApp
{
  public class Friend
  {
    [Key]
    public string Name { get; set; }
  
    [Required]
    public Point Location { get; set; }
  }
}
``` 

<span data-ttu-id="aaf33-114">Затем можно сохранить сущности с пространственными данными:</span><span class="sxs-lookup"><span data-stu-id="aaf33-114">You can then persist entities with spatial data:</span></span>

``` csharp
using (var context = new MyDbContext())
{
    context.Add(
        new Friend
        {
            Name = "Bill",
            Location = new Point(-122.34877, 47.6233355) {SRID = 4326 }
        });
    context.SaveChanges();
}
```
<span data-ttu-id="aaf33-115">Вы можете выполнять запросы к базе денных на основе пространственных данных и операций с ними:</span><span class="sxs-lookup"><span data-stu-id="aaf33-115">And you can execute database queries based on spatial data and operations:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="aaf33-116">Дополнительные сведения об этой возможности см. в [документации по пространственному типу](xref:core/modeling/spatial).</span><span class="sxs-lookup"><span data-stu-id="aaf33-116">For more information on this feature, see the [spatial types documentation](xref:core/modeling/spatial).</span></span> 

## <a name="collections-of-owned-entities"></a><span data-ttu-id="aaf33-117">Коллекции сущностей с владельцем</span><span class="sxs-lookup"><span data-stu-id="aaf33-117">Collections of owned entities</span></span>

<span data-ttu-id="aaf33-118">В EF Core 2.0 добавлена возможность для моделирования владения в связях "один к одному".</span><span class="sxs-lookup"><span data-stu-id="aaf33-118">EF Core 2.0 added the ability to model ownership in one-to-one associations.</span></span>
<span data-ttu-id="aaf33-119">В EF Core 2.2 эта возможность расширена и позволяет выражать владение в связях "один ко многим".</span><span class="sxs-lookup"><span data-stu-id="aaf33-119">EF Core 2.2 extends the ability to express ownership to one-to-many associations.</span></span>
<span data-ttu-id="aaf33-120">Владение позволяет установить ограничения на использование сущностей.</span><span class="sxs-lookup"><span data-stu-id="aaf33-120">Ownership helps constrain how entities are used.</span></span>

<span data-ttu-id="aaf33-121">Например, сущности с владельцем:</span><span class="sxs-lookup"><span data-stu-id="aaf33-121">For example, owned entities:</span></span>
- <span data-ttu-id="aaf33-122">могут отображаться только в свойствах навигации для других типов сущностей;</span><span class="sxs-lookup"><span data-stu-id="aaf33-122">Can only ever appear on navigation properties of other entity types.</span></span> 
- <span data-ttu-id="aaf33-123">загружаются автоматически и отслеживаются только с помощью DbContext вместе с владельцем.</span><span class="sxs-lookup"><span data-stu-id="aaf33-123">Are automatically loaded, and can only be tracked by a DbContext alongside their owner.</span></span>

<span data-ttu-id="aaf33-124">В реляционных базах данных коллекции с владельцем сопоставляются с таблицами, отдельными от владельца, как и в случае обычных связей "один ко многим".</span><span class="sxs-lookup"><span data-stu-id="aaf33-124">In relational databases, owned collections are mapped to separate tables from the owner, just like regular one-to-many associations.</span></span>
<span data-ttu-id="aaf33-125">Но в базах данных, ориентированных на документы, мы планируем реализовать поддержку вложенных сущностей (в коллекциях с владельцем или ссылках) в том же документе, являющемся владельцем.</span><span class="sxs-lookup"><span data-stu-id="aaf33-125">But in document-oriented databases, we plan to nest owned entities (in owned collections or references) within the same document as the owner.</span></span>

<span data-ttu-id="aaf33-126">Эту возможность можно использовать, вызвав новый API OwnsMany():</span><span class="sxs-lookup"><span data-stu-id="aaf33-126">You can use the feature by calling the new OwnsMany() API:</span></span>

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

<span data-ttu-id="aaf33-127">Дополнительные сведения см. в [обновленной документации по сущностям с владельцем](xref:core/modeling/owned-entities#collections-of-owned-types).</span><span class="sxs-lookup"><span data-stu-id="aaf33-127">For more information, see the [updated owned entities documentation](xref:core/modeling/owned-entities#collections-of-owned-types).</span></span>

## <a name="query-tags"></a><span data-ttu-id="aaf33-128">Теги запросов</span><span class="sxs-lookup"><span data-stu-id="aaf33-128">Query tags</span></span>

<span data-ttu-id="aaf33-129">Эта функция упрощает сопоставление запросов LINQ в коде со сгенерированными запросами SQL, зарегистрированными в журналах.</span><span class="sxs-lookup"><span data-stu-id="aaf33-129">This feature simplifies the correlation of LINQ queries in code with generated SQL queries captured in logs.</span></span>

<span data-ttu-id="aaf33-130">Чтобы воспользоваться преимуществами тегов запросов, можно пометить запрос LINQ с помощью нового метода TagWith().</span><span class="sxs-lookup"><span data-stu-id="aaf33-130">To take advantage of query tags, you annotate a LINQ query using the new TagWith() method.</span></span>
<span data-ttu-id="aaf33-131">Использование пространственного запроса из предыдущего примера:</span><span class="sxs-lookup"><span data-stu-id="aaf33-131">Using the spatial query from a previous example:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="aaf33-132">Запрос LINQ будет преобразован в следующий запрос SQL:</span><span class="sxs-lookup"><span data-stu-id="aaf33-132">This LINQ query will produce the following SQL output:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="aaf33-133">Дополнительные сведения см. в документации по [тегам запросов](xref:core/querying/tags).</span><span class="sxs-lookup"><span data-stu-id="aaf33-133">For more information, see the [query tags documentation](xref:core/querying/tags).</span></span> 