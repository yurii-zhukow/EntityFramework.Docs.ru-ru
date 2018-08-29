---
title: Ограничения внешнего ключа — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: a83f72b5d832e349fb4a5fb3b2de0b82bd79ef2a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993992"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="86dfe-102">Ограничения внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="86dfe-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="86dfe-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="86dfe-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="86dfe-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="86dfe-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="86dfe-105">Ограничение внешнего ключа введен для каждой связи в модели.</span><span class="sxs-lookup"><span data-stu-id="86dfe-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="86dfe-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="86dfe-106">Conventions</span></span>

<span data-ttu-id="86dfe-107">По соглашению с именем ограничения внешнего ключа `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span><span class="sxs-lookup"><span data-stu-id="86dfe-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="86dfe-108">Для составных ключей внешнего `<foreign key property name>` становится подчеркивания запятыми список имен свойств внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="86dfe-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="86dfe-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="86dfe-109">Data Annotations</span></span>

<span data-ttu-id="86dfe-110">Имена ограничение внешнего ключа нельзя настроить с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="86dfe-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="86dfe-111">Текучий API</span><span class="sxs-lookup"><span data-stu-id="86dfe-111">Fluent API</span></span>

<span data-ttu-id="86dfe-112">Fluent API можно использовать для настройки имя ограничения внешнего ключа для связи.</span><span class="sxs-lookup"><span data-stu-id="86dfe-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/RelationshipConstraintName.cs?highlight=12)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .HasForeignKey(p => p.BlogId)
            .HasConstraintName("ForeignKey_Post_Blog");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```
