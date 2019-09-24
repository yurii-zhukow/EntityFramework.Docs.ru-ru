---
title: Ограничения внешнего ключа-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: d7ed4466f4df9ec01267b048ba1bbcc6e8bbdad5
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197068"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="cd52b-102">Ограничения внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="cd52b-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="cd52b-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="cd52b-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="cd52b-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="cd52b-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="cd52b-105">Ограничение внешнего ключа вводится для каждой связи в модели.</span><span class="sxs-lookup"><span data-stu-id="cd52b-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="cd52b-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="cd52b-106">Conventions</span></span>

<span data-ttu-id="cd52b-107">По соглашению ограничения внешнего ключа именуются `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span><span class="sxs-lookup"><span data-stu-id="cd52b-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="cd52b-108">Для составных внешних `<foreign key property name>` ключей преобразуется в список имен свойств внешнего ключа с разделителями подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="cd52b-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="cd52b-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="cd52b-109">Data Annotations</span></span>

<span data-ttu-id="cd52b-110">Имена ограничений внешнего ключа нельзя настроить с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="cd52b-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="cd52b-111">Текучий API</span><span class="sxs-lookup"><span data-stu-id="cd52b-111">Fluent API</span></span>

<span data-ttu-id="cd52b-112">С помощью API-интерфейса Fluent можно настроить имя ограничения внешнего ключа для связи.</span><span class="sxs-lookup"><span data-stu-id="cd52b-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?highlight=12)] -->
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
