---
title: Новые возможности в EF Core 2.2 — EF Core
description: Изменения и улучшения в Entity Framework Core 2.2
author: ajcvickers
ms.date: 11/14/2018
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: ca71c7479254b25fe932e6abf43fe0fd8f1781b3
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92065697"
---
# <a name="new-features-in-ef-core-22"></a><span data-ttu-id="26a17-103">Новые возможности в EF Core 2.2</span><span class="sxs-lookup"><span data-stu-id="26a17-103">New features in EF Core 2.2</span></span>

## <a name="spatial-data-support"></a><span data-ttu-id="26a17-104">Поддержка пространственных данных</span><span class="sxs-lookup"><span data-stu-id="26a17-104">Spatial data support</span></span>

<span data-ttu-id="26a17-105">Пространственные данные могут использоваться для представления физического расположения и формы объектов.</span><span class="sxs-lookup"><span data-stu-id="26a17-105">Spatial data can be used to represent the physical location and shape of objects.</span></span>
<span data-ttu-id="26a17-106">Многие базы данных имеют собственную поддержку хранения и индексирования пространственных данных, а также запросов к ним.</span><span class="sxs-lookup"><span data-stu-id="26a17-106">Many databases can natively store, index, and query spatial data.</span></span>
<span data-ttu-id="26a17-107">Типичные сценарии использования включают запросы о наличии объектов в пределах указанного расстояния или для проверки того, содержит ли многоугольник указанное расположение.</span><span class="sxs-lookup"><span data-stu-id="26a17-107">Common scenarios include querying for objects within a given distance, and testing if a polygon contains a given location.</span></span>
<span data-ttu-id="26a17-108">Теперь EF Core 2.2 поддерживает работу с пространственными данными в различных базах данных, используя типы из библиотеки [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite).</span><span class="sxs-lookup"><span data-stu-id="26a17-108">EF Core 2.2 now supports working with spatial data from various databases using types from the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS) library.</span></span>

<span data-ttu-id="26a17-109">Поддержка пространственных данных реализована в виде набора пакетов расширений для конкретных поставщиков.</span><span class="sxs-lookup"><span data-stu-id="26a17-109">Spatial data support is implemented as a series of provider-specific extension packages.</span></span>
<span data-ttu-id="26a17-110">Каждый из этих пакетов содержит сопоставления для типов и методов NTS с соответствующими пространственными типами и функциями в базе данных.</span><span class="sxs-lookup"><span data-stu-id="26a17-110">Each of these packages contributes mappings for NTS types and methods, and the corresponding spatial types and functions in the database.</span></span>
<span data-ttu-id="26a17-111">Эти расширения доступны для поставщиков баз данных [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/) и [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (из [проекта Npgsql](https://www.npgsql.org/)).</span><span class="sxs-lookup"><span data-stu-id="26a17-111">Such provider extensions are now available for [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), and [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (from the [Npgsql project](https://www.npgsql.org/)).</span></span>
<span data-ttu-id="26a17-112">Пространственные типы можно использовать непосредственно с [поставщиком EF Core для выполняющейся в памяти базы данных](xref:core/providers/in-memory/index) без дополнительных расширений.</span><span class="sxs-lookup"><span data-stu-id="26a17-112">Spatial types can be used directly with the [EF Core in-memory provider](xref:core/providers/in-memory/index) without additional extensions.</span></span>

<span data-ttu-id="26a17-113">После установки расширения для поставщика можно добавлять к сущностям свойства поддерживаемых типов.</span><span class="sxs-lookup"><span data-stu-id="26a17-113">Once the provider extension is installed, you can add properties of supported types to your entities.</span></span> <span data-ttu-id="26a17-114">Пример:</span><span class="sxs-lookup"><span data-stu-id="26a17-114">For example:</span></span>

```csharp
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

<span data-ttu-id="26a17-115">Затем можно сохранить сущности с пространственными данными:</span><span class="sxs-lookup"><span data-stu-id="26a17-115">You can then persist entities with spatial data:</span></span>

```csharp
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

<span data-ttu-id="26a17-116">Вы можете выполнять запросы к базе денных на основе пространственных данных и операций с ними:</span><span class="sxs-lookup"><span data-stu-id="26a17-116">And you can execute database queries based on spatial data and operations:</span></span>

```csharp
var nearestFriends =
    (from f in context.Friends
    orderby f.Location.Distance(myLocation) descending
    select f).Take(5).ToList();
```

<span data-ttu-id="26a17-117">Дополнительные сведения об этой возможности см. в [документации по пространственному типу](xref:core/modeling/spatial).</span><span class="sxs-lookup"><span data-stu-id="26a17-117">For more information on this feature, see the [spatial types documentation](xref:core/modeling/spatial).</span></span>

## <a name="collections-of-owned-entities"></a><span data-ttu-id="26a17-118">Коллекции сущностей с владельцем</span><span class="sxs-lookup"><span data-stu-id="26a17-118">Collections of owned entities</span></span>

<span data-ttu-id="26a17-119">В EF Core 2.0 добавлена возможность для моделирования владения в связях "один к одному".</span><span class="sxs-lookup"><span data-stu-id="26a17-119">EF Core 2.0 added the ability to model ownership in one-to-one associations.</span></span>
<span data-ttu-id="26a17-120">В EF Core 2.2 эта возможность расширена и позволяет выражать владение в связях "один ко многим".</span><span class="sxs-lookup"><span data-stu-id="26a17-120">EF Core 2.2 extends the ability to express ownership to one-to-many associations.</span></span>
<span data-ttu-id="26a17-121">Владение позволяет установить ограничения на использование сущностей.</span><span class="sxs-lookup"><span data-stu-id="26a17-121">Ownership helps constrain how entities are used.</span></span>

<span data-ttu-id="26a17-122">Например, сущности с владельцем:</span><span class="sxs-lookup"><span data-stu-id="26a17-122">For example, owned entities:</span></span>

- <span data-ttu-id="26a17-123">могут отображаться только в свойствах навигации для других типов сущностей;</span><span class="sxs-lookup"><span data-stu-id="26a17-123">Can only ever appear on navigation properties of other entity types.</span></span>
- <span data-ttu-id="26a17-124">загружаются автоматически и отслеживаются только с помощью DbContext вместе с владельцем.</span><span class="sxs-lookup"><span data-stu-id="26a17-124">Are automatically loaded, and can only be tracked by a DbContext alongside their owner.</span></span>

<span data-ttu-id="26a17-125">В реляционных базах данных коллекции с владельцем сопоставляются с таблицами, отдельными от владельца, как и в случае обычных связей "один ко многим".</span><span class="sxs-lookup"><span data-stu-id="26a17-125">In relational databases, owned collections are mapped to separate tables from the owner, just like regular one-to-many associations.</span></span>
<span data-ttu-id="26a17-126">Но в базах данных, ориентированных на документы, мы планируем реализовать поддержку вложенных сущностей (в коллекциях с владельцем или ссылках) в том же документе, являющемся владельцем.</span><span class="sxs-lookup"><span data-stu-id="26a17-126">But in document-oriented databases, we plan to nest owned entities (in owned collections or references) within the same document as the owner.</span></span>

<span data-ttu-id="26a17-127">Эту возможность можно использовать, вызвав новый API OwnsMany():</span><span class="sxs-lookup"><span data-stu-id="26a17-127">You can use the feature by calling the new OwnsMany() API:</span></span>

```csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

<span data-ttu-id="26a17-128">Дополнительные сведения см. в [обновленной документации по сущностям с владельцем](xref:core/modeling/owned-entities#collections-of-owned-types).</span><span class="sxs-lookup"><span data-stu-id="26a17-128">For more information, see the [updated owned entities documentation](xref:core/modeling/owned-entities#collections-of-owned-types).</span></span>

## <a name="query-tags"></a><span data-ttu-id="26a17-129">Теги запросов</span><span class="sxs-lookup"><span data-stu-id="26a17-129">Query tags</span></span>

<span data-ttu-id="26a17-130">Эта функция упрощает сопоставление запросов LINQ в коде со сгенерированными запросами SQL, зарегистрированными в журналах.</span><span class="sxs-lookup"><span data-stu-id="26a17-130">This feature simplifies the correlation of LINQ queries in code with generated SQL queries captured in logs.</span></span>

<span data-ttu-id="26a17-131">Чтобы воспользоваться преимуществами тегов запросов, можно пометить запрос LINQ с помощью нового метода TagWith().</span><span class="sxs-lookup"><span data-stu-id="26a17-131">To take advantage of query tags, you annotate a LINQ query using the new TagWith() method.</span></span>
<span data-ttu-id="26a17-132">Использование пространственного запроса из предыдущего примера:</span><span class="sxs-lookup"><span data-stu-id="26a17-132">Using the spatial query from a previous example:</span></span>

```csharp
var nearestFriends =
    (from f in context.Friends.TagWith(@"This is my spatial query!")
    orderby f.Location.Distance(myLocation) descending
    select f).Take(5).ToList();
```

<span data-ttu-id="26a17-133">Запрос LINQ будет преобразован в следующий запрос SQL:</span><span class="sxs-lookup"><span data-stu-id="26a17-133">This LINQ query will produce the following SQL output:</span></span>

```sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="26a17-134">Дополнительные сведения см. в документации по [тегам запросов](xref:core/querying/tags).</span><span class="sxs-lookup"><span data-stu-id="26a17-134">For more information, see the [query tags documentation](xref:core/querying/tags).</span></span>
