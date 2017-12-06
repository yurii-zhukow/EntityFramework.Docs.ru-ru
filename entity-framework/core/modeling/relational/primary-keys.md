---
title: "Первичные ключи - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
ms.technology: entity-framework-core
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: fcb1871149c0f20a2576864028b4171904de1982
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="primary-keys"></a><span data-ttu-id="36803-102">Первичные ключи</span><span class="sxs-lookup"><span data-stu-id="36803-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="36803-103">В этом разделе конфигурации применяется для реляционных баз данных в целом.</span><span class="sxs-lookup"><span data-stu-id="36803-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="36803-104">Методы расширения, показанный здесь будут доступны после установки поставщика реляционной базы данных (из-за общей *Microsoft.EntityFrameworkCore.Relational* пакета).</span><span class="sxs-lookup"><span data-stu-id="36803-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="36803-105">Ограничение первичного ключа введен для ключа каждого типа сущности.</span><span class="sxs-lookup"><span data-stu-id="36803-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="36803-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="36803-106">Conventions</span></span>

<span data-ttu-id="36803-107">По соглашению первичного ключа в базе данных будет называться `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="36803-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="36803-108">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="36803-108">Data Annotations</span></span>

<span data-ttu-id="36803-109">Нет определенных аспектов реляционной базы данных первичного ключа можно настроить с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="36803-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="36803-110">Fluent API</span><span class="sxs-lookup"><span data-stu-id="36803-110">Fluent API</span></span>

<span data-ttu-id="36803-111">Чтобы настроить имя ограничения первичного ключа в базе данных можно использовать Fluent API.</span><span class="sxs-lookup"><span data-stu-id="36803-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/KeyName.cs?highlight=9)] -->
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
