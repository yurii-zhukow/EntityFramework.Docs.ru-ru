---
title: "Схема по умолчанию - EF Core"
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
---
# <a name="default-schema"></a><span data-ttu-id="db587-102">Схема по умолчанию</span><span class="sxs-lookup"><span data-stu-id="db587-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="db587-103">В этом разделе конфигурации применяется для реляционных баз данных в целом.</span><span class="sxs-lookup"><span data-stu-id="db587-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="db587-104">Методы расширения, показанный здесь будут доступны после установки поставщика реляционной базы данных (из-за общей *Microsoft.EntityFrameworkCore.Relational* пакета).</span><span class="sxs-lookup"><span data-stu-id="db587-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="db587-105">Схема по умолчанию — схему базы данных, объекты будут созданы в, если схема явным образом не настроена для этого объекта.</span><span class="sxs-lookup"><span data-stu-id="db587-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="db587-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="db587-106">Conventions</span></span>

<span data-ttu-id="db587-107">По соглашению поставщика базы данных будет выбирать наиболее подходящий схемы по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="db587-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="db587-108">Например, Microsoft SQL Server будет использовать `dbo` схемы и SQLite не будет использовать схему (поскольку схемы не поддерживаются в SQLite).</span><span class="sxs-lookup"><span data-stu-id="db587-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="db587-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="db587-109">Data Annotations</span></span>

<span data-ttu-id="db587-110">Не удается установить схему по умолчанию, с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="db587-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="db587-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="db587-111">Fluent API</span></span>

<span data-ttu-id="db587-112">Fluent API можно использовать для указания схемы по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="db587-112">You can use the Fluent API to specify a default schema.</span></span>

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
