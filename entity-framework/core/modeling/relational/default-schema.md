---
title: Схема по умолчанию - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
ms.technology: entity-framework-core
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 26106deb2d4e35ecf33e97790a83f9af77991aed
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052754"
---
# <a name="default-schema"></a>Схема по умолчанию

> [!NOTE]  
> В этом разделе конфигурации применяется для реляционных баз данных в целом. Методы расширения, показанный здесь будут доступны после установки поставщика реляционной базы данных (из-за общей *Microsoft.EntityFrameworkCore.Relational* пакета).

Схема по умолчанию — схему базы данных, объекты будут созданы в, если схема явным образом не настроена для этого объекта.

## <a name="conventions"></a>Соглашения

По соглашению поставщика базы данных будет выбирать наиболее подходящий схемы по умолчанию. Например, Microsoft SQL Server будет использовать `dbo` схемы и SQLite не будет использовать схему (поскольку схемы не поддерживаются в SQLite).

## <a name="data-annotations"></a>Заметки к данным

Не удается установить схему по умолчанию, с помощью заметок к данным.

## <a name="fluent-api"></a>Fluent API

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
