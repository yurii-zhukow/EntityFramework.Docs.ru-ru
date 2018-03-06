---
title: "Краткий обзор — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: c76b4cd318151b502c549fa0a82800f9987ed94c
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="entity-framework-core-quick-overview"></a>Краткий обзор Entity Framework Core

Entity Framework (EF) — это упрощенная, расширяемая и кроссплатформенная версия популярной технологии для доступа к данным Entity Framework.

EF Core является объектно-реляционным модулем сопоставления (O/RM), который позволяет разработчикам .NET работать с базой данных, используя объекты .NET. Это устраняет необходимость в большей части кода для доступа к данным, который разработчикам обычно приходится писать. EF Core поддерживает множество систем баз данных. Дополнительные сведения см. в разделе [Поставщики баз данных](providers/index.md).

Если вы хотите учиться, создавая код, рекомендуем воспользоваться одним из наших руководств по [началу работы](get-started/index.md) с EF Core.

## <a name="what-is-new-in-ef-core"></a>Новые возможности в EF Core

Если вы знакомы с EF Core и хотите перейти сразу к сведениям о последних выпусках:

- **[Новые возможности в EF Core 2.1 (сейчас находится на этапе предварительной версии)](xref:core/what-is-new/ef-core-2.1)**
- **[Новые возможности в EF Core 2.0 (последняя выпущенная версия)](xref:core/what-is-new/ef-core-2.0)**
- **[Обновление существующих приложений до EF Core 2.0](xref:core/miscellaneous/1x-2x-upgrade)**


## <a name="get-entity-framework-core"></a>Получение Entity Framework Core

[Установите пакет NuGet](https://docs.nuget.org/ndocs/quickstart/use-a-package) для нужного поставщика базы данных. Например, чтобы установить поставщик SQL Server в кроссплатформенной среде разработки с помощью инструмента `dotnet` в командной строке, введите следующее.

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

В Visual Studio можно использовать консоль диспетчера пакетов.

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
Дополнительные сведения о доступных поставщиках см. в разделе [Поставщики баз данных](providers/index.md), а об этапах установки — в разделе [Установка EF Core](get-started/install/index.md).

## <a name="the-model"></a>Модель

В EF Core доступ к данным осуществляется с помощью модели. Модель состоит из классов сущностей и производного контекста, который представляет сеанс взаимодействия с базой данных, позволяя запрашивать и сохранять данные. Дополнительные сведения см. в разделе [Создание модели](modeling/index.md).

Вы можете создать модель из существующей базы данных, вручную составить код модели, соответствующий базе данных, или использовать миграции EF, чтобы создать базу данных из модели (и модифицировать ее по мере изменения модели).

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

## <a name="querying"></a>Выполнение запросов

Экземпляры классов сущностей извлекаются из базы данных с помощью LINQ. Дополнительные сведения см. в разделе [Запросы к данным](querying/index.md).

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a>Сохранение данных

Для создания, удаления и изменения данных в базе данных используются экземпляры классов сущностей. Дополнительные сведения см. в разделе [Сохранение данных](saving/index.md).

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
