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
# <a name="table-mapping"></a>Сопоставление таблиц

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

Сопоставление таблиц определяет, какие табличные данные следует запрашивать и сохранять в базе данных.

## <a name="conventions"></a>Соглашения

По соглашению каждая сущность будет настроена для сопоставления с таблицей с именем, указанным в свойстве `DbSet<TEntity>`, которое предоставляет сущность в производном контексте. Если `DbSet<TEntity>` не включено для данной сущности, используется имя класса.

## <a name="data-annotations"></a>Заметки к данным

Заметки к данным можно использовать для настройки таблицы, сопоставляемой с типом.

``` csharp
using System.ComponentModel.DataAnnotations.Schema;

[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

Можно также указать схему, которой принадлежит таблица.

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Текучий API

API-интерфейс Fluent можно использовать для настройки таблицы, сопоставляемой с типом.

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

Можно также указать схему, которой принадлежит таблица.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/TableAndSchema.cs?name=Table&highlight=2)]
