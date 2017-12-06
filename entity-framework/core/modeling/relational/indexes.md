---
title: "Индексы - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: 683b580bb155e0416f13c5d63e3280078fbcee21
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="indexes"></a><span data-ttu-id="0dec1-102">Индексы</span><span class="sxs-lookup"><span data-stu-id="0dec1-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="0dec1-103">В этом разделе конфигурации применяется для реляционных баз данных в целом.</span><span class="sxs-lookup"><span data-stu-id="0dec1-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="0dec1-104">Методы расширения, показанный здесь будут доступны после установки поставщика реляционной базы данных (из-за общей *Microsoft.EntityFrameworkCore.Relational* пакета).</span><span class="sxs-lookup"><span data-stu-id="0dec1-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="0dec1-105">Индекс в реляционной базе данных сопоставляет совпадает с концепцией индекса в ядро платформы Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0dec1-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="0dec1-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="0dec1-106">Conventions</span></span>

<span data-ttu-id="0dec1-107">По соглашению с именем индексы `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="0dec1-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="0dec1-108">Составные индексы `<property name>` становится подчеркивания запятыми список имен свойств.</span><span class="sxs-lookup"><span data-stu-id="0dec1-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="0dec1-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="0dec1-109">Data Annotations</span></span>

<span data-ttu-id="0dec1-110">Индексы не следует задавать с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="0dec1-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="0dec1-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="0dec1-111">Fluent API</span></span>

<span data-ttu-id="0dec1-112">Чтобы настроить имя индекса, можно использовать Fluent API.</span><span class="sxs-lookup"><span data-stu-id="0dec1-112">You can use the Fluent API to configure the name of an index.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/IndexName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .HasName("Index_Url");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
