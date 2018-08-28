---
title: Первичные ключи — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: 916f3adbcd08cb1037c7fbf68e99630feb321a61
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998072"
---
# <a name="primary-keys"></a><span data-ttu-id="1955f-102">Первичные ключи</span><span class="sxs-lookup"><span data-stu-id="1955f-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="1955f-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="1955f-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="1955f-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="1955f-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="1955f-105">Ограничение первичного ключа введен для ключа каждого типа сущности.</span><span class="sxs-lookup"><span data-stu-id="1955f-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="1955f-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="1955f-106">Conventions</span></span>

<span data-ttu-id="1955f-107">По соглашению, первичный ключ в базе данных будет называться `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="1955f-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="1955f-108">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="1955f-108">Data Annotations</span></span>

<span data-ttu-id="1955f-109">Нет конкретных аспектов реляционной базы данных первичного ключа можно настроить с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="1955f-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="1955f-110">Текучий API</span><span class="sxs-lookup"><span data-stu-id="1955f-110">Fluent API</span></span>

<span data-ttu-id="1955f-111">Fluent API можно использовать для настройки имени ограничения первичного ключа в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1955f-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

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
