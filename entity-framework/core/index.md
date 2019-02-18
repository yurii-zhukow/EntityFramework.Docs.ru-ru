---
title: Общие сведения о EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: 982f69077a68495c48b7a9cce833dd7d4119e252
ms.sourcegitcommit: 735715f10cc8a231c213e4f055d79f0effd86570
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/16/2019
ms.locfileid: "56325318"
---
# <a name="entity-framework-core"></a><span data-ttu-id="e9fec-102">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e9fec-102">Entity Framework Core</span></span>

<span data-ttu-id="e9fec-103">Entity Framework (EF) Core — это простая, кроссплатформенная и расширяемая версия популярной технологии доступа к данным Entity Framework [с открытым исходным кодом](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="e9fec-103">Entity Framework (EF) Core is a lightweight, extensible, [open source](https://github.com/aspnet/EntityFrameworkCore) and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="e9fec-104">EF Core может использоваться как объектно реляционный модуль сопоставления (O/RM), позволяя разработчикам .NET работать с базой данных с помощью объектов .NET и устраняя необходимость в написании большей части кода, требуемого для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="e9fec-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="e9fec-105">EF Core поддерживает множество систем баз данных. Дополнительные сведения см. в разделе [Поставщики баз данных](providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="e9fec-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

## <a name="the-model"></a><span data-ttu-id="e9fec-106">Модель</span><span class="sxs-lookup"><span data-stu-id="e9fec-106">The Model</span></span>

<span data-ttu-id="e9fec-107">В EF Core доступ к данным осуществляется с помощью модели.</span><span class="sxs-lookup"><span data-stu-id="e9fec-107">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="e9fec-108">Модель состоит из классов сущностей и производного контекста, который представляет сеанс взаимодействия с базой данных, позволяя запрашивать и сохранять данные.</span><span class="sxs-lookup"><span data-stu-id="e9fec-108">A model is made up of entity classes and a derived context that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="e9fec-109">Дополнительные сведения см. в разделе [Создание модели](modeling/index.md).</span><span class="sxs-lookup"><span data-stu-id="e9fec-109">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="e9fec-110">Вы можете создать модель из существующей базы данных, вручную составить код модели, соответствующий базе данных, или использовать миграции EF, чтобы создать базу данных из модели (и модифицировать ее по мере изменения модели).</span><span class="sxs-lookup"><span data-stu-id="e9fec-110">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model (and evolve it as your model changes over time).</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace Intro
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }
        public int Rating { get; set; }
        public List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
}
```

## <a name="querying"></a><span data-ttu-id="e9fec-111">Выполнение запросов</span><span class="sxs-lookup"><span data-stu-id="e9fec-111">Querying</span></span>

<span data-ttu-id="e9fec-112">Экземпляры классов сущностей извлекаются из базы данных с помощью LINQ.</span><span class="sxs-lookup"><span data-stu-id="e9fec-112">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="e9fec-113">Дополнительные сведения см. в разделе [Запросы к данным](querying/index.md).</span><span class="sxs-lookup"><span data-stu-id="e9fec-113">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="e9fec-114">Сохранение данных</span><span class="sxs-lookup"><span data-stu-id="e9fec-114">Saving Data</span></span>

<span data-ttu-id="e9fec-115">Для создания, удаления и изменения данных в базе данных используются экземпляры классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="e9fec-115">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="e9fec-116">Дополнительные сведения см. в разделе [Сохранение данных](saving/index.md).</span><span class="sxs-lookup"><span data-stu-id="e9fec-116">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```

## <a name="next-steps"></a><span data-ttu-id="e9fec-117">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="e9fec-117">Next steps</span></span>

<span data-ttu-id="e9fec-118">Вводные руководства см. в разделе [Начало работы с Entity Framework Core](get-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="e9fec-118">For introductory tutorials, see [Getting Started with Entity Framework Core](get-started/index.md).</span></span>

