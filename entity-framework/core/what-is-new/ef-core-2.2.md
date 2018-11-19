---
title: Новые возможности в EF Core 2.2 — EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 998C04F3-676A-4FCF-8450-CFB0457B4198
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: 79b4efc3aee23e19a9ea1deb6373b9984b77f886
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688772"
---
# <a name="new-features-in-ef-core-22"></a>Новые возможности в EF Core 2.2

## <a name="spatial-data-support"></a>Поддержка пространственных данных

Пространственные данные могут использоваться для представления физического расположения и формы объектов.
Многие базы данных имеют собственную поддержку хранения и индексирования пространственных данных, а также запросов к ним. Типичные сценарии использования включают запросы о наличии объектов в пределах указанного расстояния или для проверки того, содержит ли многоугольник указанное расположение.
Теперь EF Core 2.2 поддерживает работу с пространственными данными в различных базах данных, используя типы из библиотеки [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite).

Поддержка пространственных данных реализована в виде набора пакетов расширений для конкретных поставщиков.
Каждый из этих пакетов содержит сопоставления для типов и методов NTS с соответствующими пространственными типами и функциями в базе данных.
Эти расширения доступны для поставщиков баз данных [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/) и [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (из [проекта Npgsql](http://www.npgsql.org/)).
Пространственные типы можно использовать непосредственно с [поставщиком EF Core для выполняющейся в памяти базы данных](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/) без дополнительных расширений.

После установки расширения для поставщика можно добавлять к сущностям свойства поддерживаемых типов. Пример:

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

Затем можно сохранить сущности с пространственными данными:

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
Вы можете выполнять запросы к базе денных на основе пространственных данных и операций с ними:

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Дополнительные сведения об этой возможности см. в [документации по пространственному типу](xref:core/modeling/spatial). 

## <a name="collections-of-owned-entities"></a>Коллекции сущностей с владельцем

В EF Core 2.0 добавлена возможность для моделирования владения в связях "один к одному".
В EF Core 2.2 эта возможность расширена и позволяет выражать владение в связях "один ко многим".
Владение позволяет установить ограничения на использование сущностей.

Например, сущности с владельцем:
- могут отображаться только в свойствах навигации для других типов сущностей; 
- загружаются автоматически и отслеживаются только с помощью DbContext вместе с владельцем.

В реляционных базах данных коллекции с владельцем сопоставляются с таблицами, отдельными от владельца, как и в случае обычных связей "один ко многим".
Но в базах данных, ориентированных на документы, мы планируем реализовать поддержку вложенных сущностей (в коллекциях с владельцем или ссылках) в том же документе, являющемся владельцем.

Эту возможность можно использовать, вызвав новый API OwnsMany():

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

Дополнительные сведения см. в [обновленной документации по сущностям с владельцем](xref:core/modeling/owned-entities#collections-of-owned-types).

## <a name="query-tags"></a>Теги запросов

Эта функция упрощает сопоставление запросов LINQ в коде со сгенерированными запросами SQL, зарегистрированными в журналах.

Чтобы воспользоваться преимуществами тегов запросов, можно пометить запрос LINQ с помощью нового метода TagWith().
Использование пространственного запроса из предыдущего примера:

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

Запрос LINQ будет преобразован в следующий запрос SQL:

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

Дополнительные сведения см. в документации по [тегам запросов](xref:core/querying/tags). 