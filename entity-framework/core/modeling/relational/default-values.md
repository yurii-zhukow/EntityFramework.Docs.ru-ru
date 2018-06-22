---
title: Значения по умолчанию - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
ms.technology: entity-framework-core
uid: core/modeling/relational/default-values
ms.openlocfilehash: 73b916b6d9f9c984c8ea010f2319eafa7d031a58
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052764"
---
# <a name="default-values"></a>Значения по умолчанию

> [!NOTE]  
> В этом разделе конфигурации применяется для реляционных баз данных в целом. Методы расширения, показанный здесь будут доступны после установки поставщика реляционной базы данных (из-за общей *Microsoft.EntityFrameworkCore.Relational* пакета).

Значение столбца по умолчанию является значение, которое будет вставлена, если новая строка вставляется, но не указано значение для столбца.

## <a name="conventions"></a>Соглашения

По соглашению значение по умолчанию не настроен.

## <a name="data-annotations"></a>Заметки к данным

Не удается назначить значение по умолчанию, с помощью заметок к данным.

## <a name="fluent-api"></a>Fluent API

Fluent API можно использовать для указания значения по умолчанию для свойства.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValue.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Rating)
            .HasDefaultValue(3);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public int Rating { get; set; }
}
```

Можно также указать фрагмент SQL, который используется для вычисления значения по умолчанию.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValueSql.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Created)
            .HasDefaultValueSql("getdate()");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public DateTime Created { get; set; }
}
```
