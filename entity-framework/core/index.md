---
title: "Краткий обзор — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: 13de9cf98111b8e253e073c591fcec04206b4c4f
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="entity-framework-core-quick-overview"></a><span data-ttu-id="26285-102">Краткий обзор Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="26285-102">Entity Framework Core Quick Overview</span></span>

<span data-ttu-id="26285-103">Entity Framework (EF) — это упрощенная, расширяемая и кроссплатформенная версия популярной технологии для доступа к данным Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="26285-103">Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="26285-104">EF Core является объектно-реляционным модулем сопоставления (O/RM), который позволяет разработчикам .NET работать с базой данных, используя объекты .NET.</span><span class="sxs-lookup"><span data-stu-id="26285-104">EF Core is an object-relational mapper (O/RM) that enables .NET developers to work with a database using .NET objects.</span></span> <span data-ttu-id="26285-105">Это устраняет необходимость в большей части кода для доступа к данным, который разработчикам обычно приходится писать.</span><span class="sxs-lookup"><span data-stu-id="26285-105">It eliminates the need for most of the data-access code that developers usually need to write.</span></span> <span data-ttu-id="26285-106">EF Core поддерживает множество систем баз данных. Дополнительные сведения см. в разделе [Поставщики баз данных](providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="26285-106">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

<span data-ttu-id="26285-107">Если вы хотите учиться, создавая код, рекомендуем воспользоваться одним из наших руководств по [началу работы](get-started/index.md) с EF Core.</span><span class="sxs-lookup"><span data-stu-id="26285-107">If you like to learn by writing code, we'd recommend one of our [Getting Started](get-started/index.md) guides to get you started with EF Core.</span></span>

## <a name="latest-version-ef-core-20"></a><span data-ttu-id="26285-108">Последняя версия: EF Core 2.0</span><span class="sxs-lookup"><span data-stu-id="26285-108">Latest version: EF Core 2.0</span></span>

<span data-ttu-id="26285-109">Если вы знакомы с EF Core и хотите перейти сразу к сведениям о новой версии.</span><span class="sxs-lookup"><span data-stu-id="26285-109">If you are familiar with EF Core and want to jump straight into the details of the new version:</span></span>

- <span data-ttu-id="26285-110">**[Новые возможности в EF Core 2.0](what-is-new/index.md)**</span><span class="sxs-lookup"><span data-stu-id="26285-110">**[New features in EF Core 2.0](what-is-new/index.md)**</span></span>
- <span data-ttu-id="26285-111">**[Обновление существующих приложений до EF Core 2.0](miscellaneous/1x-2x-upgrade.md)**</span><span class="sxs-lookup"><span data-stu-id="26285-111">**[Upgrading existing applications to EF Core 2.0](miscellaneous/1x-2x-upgrade.md)**</span></span>

## <a name="get-entity-framework-core"></a><span data-ttu-id="26285-112">Получение Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="26285-112">Get Entity Framework Core</span></span>

<span data-ttu-id="26285-113">[Установите пакет NuGet](https://docs.nuget.org/ndocs/quickstart/use-a-package) для нужного поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="26285-113">[Install the NuGet package](https://docs.nuget.org/ndocs/quickstart/use-a-package) for the database provider you want to use.</span></span> <span data-ttu-id="26285-114">Пример:</span><span class="sxs-lookup"><span data-stu-id="26285-114">E.g.</span></span> <span data-ttu-id="26285-115">чтобы установить поставщик SQL Server в кроссплатформенной среде разработки с помощью инструмента `dotnet` в командной строке, введите следующее.</span><span class="sxs-lookup"><span data-stu-id="26285-115">to install the SQL Server provider in cross-platform development using `dotnet` tool in the command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="26285-116">В Visual Studio можно использовать консоль диспетчера пакетов.</span><span class="sxs-lookup"><span data-stu-id="26285-116">Or in Visual Studio, using the Package Manager Console:</span></span>

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
<span data-ttu-id="26285-117">Дополнительные сведения о доступных поставщиках см. в разделе [Поставщики баз данных](providers/index.md), а об этапах установки — в разделе [Установка EF Core](get-started/install/index.md).</span><span class="sxs-lookup"><span data-stu-id="26285-117">See [Database Providers](providers/index.md) for information on available providers and [Installing EF Core](get-started/install/index.md) for more detailed installation steps.</span></span>

## <a name="the-model"></a><span data-ttu-id="26285-118">Модель</span><span class="sxs-lookup"><span data-stu-id="26285-118">The Model</span></span>

<span data-ttu-id="26285-119">В EF Core доступ к данным осуществляется с помощью модели.</span><span class="sxs-lookup"><span data-stu-id="26285-119">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="26285-120">Модель состоит из классов сущностей и производного контекста, который представляет сеанс взаимодействия с базой данных, позволяя запрашивать и сохранять данные.</span><span class="sxs-lookup"><span data-stu-id="26285-120">A model is made up of entity classes and a derived context that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="26285-121">Дополнительные сведения см. в разделе [Создание модели](modeling/index.md).</span><span class="sxs-lookup"><span data-stu-id="26285-121">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="26285-122">Вы можете создать модель из существующей базы данных, вручную составить код модели, соответствующий базе данных, или использовать миграции EF, чтобы создать базу данных из модели (и модифицировать ее по мере изменения модели).</span><span class="sxs-lookup"><span data-stu-id="26285-122">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model (and evolve it as your model changes over time).</span></span>

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

## <a name="querying"></a><span data-ttu-id="26285-123">Выполнение запросов</span><span class="sxs-lookup"><span data-stu-id="26285-123">Querying</span></span>

<span data-ttu-id="26285-124">Экземпляры классов сущностей извлекаются из базы данных с помощью LINQ.</span><span class="sxs-lookup"><span data-stu-id="26285-124">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="26285-125">Дополнительные сведения см. в разделе [Запросы к данным](querying/index.md).</span><span class="sxs-lookup"><span data-stu-id="26285-125">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="26285-126">Сохранение данных</span><span class="sxs-lookup"><span data-stu-id="26285-126">Saving Data</span></span>

<span data-ttu-id="26285-127">Для создания, удаления и изменения данных в базе данных используются экземпляры классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="26285-127">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="26285-128">Дополнительные сведения см. в разделе [Сохранение данных](saving/index.md).</span><span class="sxs-lookup"><span data-stu-id="26285-128">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
