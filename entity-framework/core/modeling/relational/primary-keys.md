---
title: Первичные ключи — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: bdb31964d717c64bddc28e7f1ce55b261e285a9b
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196945"
---
# <a name="primary-keys"></a><span data-ttu-id="5c591-102">Первичные ключи</span><span class="sxs-lookup"><span data-stu-id="5c591-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="5c591-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="5c591-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="5c591-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="5c591-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="5c591-105">Ограничение первичного ключа вводится для ключа каждого типа сущности.</span><span class="sxs-lookup"><span data-stu-id="5c591-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="5c591-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="5c591-106">Conventions</span></span>

<span data-ttu-id="5c591-107">По соглашению первичный ключ в базе данных будет называться `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="5c591-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="5c591-108">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="5c591-108">Data Annotations</span></span>

<span data-ttu-id="5c591-109">Никакие аспекты реляционной базы данных, относящиеся к первичному ключу, не могут быть настроены с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="5c591-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="5c591-110">Текучий API</span><span class="sxs-lookup"><span data-stu-id="5c591-110">Fluent API</span></span>

<span data-ttu-id="5c591-111">API-интерфейс Fluent можно использовать для настройки имени ограничения первичного ключа в базе данных.</span><span class="sxs-lookup"><span data-stu-id="5c591-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/KeyName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasKey(b => b.BlogId)
            .HasName("PrimaryKey_BlogId");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
