---
title: Схема по умолчанию — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: ae903ed7200859430aecc55073651236759bc6ce
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197131"
---
# <a name="default-schema"></a><span data-ttu-id="915d0-102">Схема по умолчанию</span><span class="sxs-lookup"><span data-stu-id="915d0-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="915d0-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="915d0-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="915d0-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="915d0-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="915d0-105">Схема по умолчанию — это схема базы данных, в которой будут создаваться объекты, если схема не настроена явно для этого объекта.</span><span class="sxs-lookup"><span data-stu-id="915d0-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="915d0-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="915d0-106">Conventions</span></span>

<span data-ttu-id="915d0-107">По соглашению поставщик базы данных будет выбирать наиболее подходящую схему по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="915d0-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="915d0-108">Например, Microsoft SQL Server будет использовать `dbo` схему, а SQLite не будет использовать схему (поскольку схемы не поддерживаются в SQLite).</span><span class="sxs-lookup"><span data-stu-id="915d0-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="915d0-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="915d0-109">Data Annotations</span></span>

<span data-ttu-id="915d0-110">Невозможно задать схему по умолчанию с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="915d0-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="915d0-111">Текучий API</span><span class="sxs-lookup"><span data-stu-id="915d0-111">Fluent API</span></span>

<span data-ttu-id="915d0-112">Для указания схемы по умолчанию можно использовать API Fluent.</span><span class="sxs-lookup"><span data-stu-id="915d0-112">You can use the Fluent API to specify a default schema.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultSchema.cs?highlight=7)] -->
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
