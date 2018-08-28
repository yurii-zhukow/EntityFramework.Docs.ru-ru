---
title: Схема по умолчанию — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 800551bbadd0a9e8b5eb7070a8ccf6ed2407e3d2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995370"
---
# <a name="default-schema"></a>Схема по умолчанию

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

Схема по умолчанию — схему базы данных, объекты будут созданы в, если схема не настроена явным образом для этого объекта.

## <a name="conventions"></a>Соглашения

По соглашению поставщик базы данных будет выбирать наиболее подходящий схемы по умолчанию. Например, Microsoft SQL Server будет использовать `dbo` схемы и SQLite не будет использовать схему (поскольку схемы не поддерживаются в SQLite).

## <a name="data-annotations"></a>Заметки к данным

Не удается установить схему по умолчанию, с помощью заметок к данным.

## <a name="fluent-api"></a>Текучий API

Fluent API можно использовать для указания схемы по умолчанию.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultSchema.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasDefaultSchema("blogging");
    }
}
```
