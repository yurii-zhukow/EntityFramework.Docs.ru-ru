---
title: Последовательность — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: eb9d9896966af0ad6b778047a1ed6af7358e8eb2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994520"
---
# <a name="sequences"></a><span data-ttu-id="8cb44-102">Последовательности</span><span class="sxs-lookup"><span data-stu-id="8cb44-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="8cb44-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="8cb44-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="8cb44-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="8cb44-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="8cb44-105">Последовательность создает последовательных числовых значений в базе данных.</span><span class="sxs-lookup"><span data-stu-id="8cb44-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="8cb44-106">Последовательности не связаны с определенной таблицей.</span><span class="sxs-lookup"><span data-stu-id="8cb44-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="8cb44-107">Соглашения</span><span class="sxs-lookup"><span data-stu-id="8cb44-107">Conventions</span></span>

<span data-ttu-id="8cb44-108">По соглашению последовательностей не добавляются в модель.</span><span class="sxs-lookup"><span data-stu-id="8cb44-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="8cb44-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="8cb44-109">Data Annotations</span></span>

<span data-ttu-id="8cb44-110">Вы можете настроить не последовательности с использованием заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="8cb44-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="8cb44-111">Текучий API</span><span class="sxs-lookup"><span data-stu-id="8cb44-111">Fluent API</span></span>

<span data-ttu-id="8cb44-112">Fluent API можно использовать для создания последовательности в модели.</span><span class="sxs-lookup"><span data-stu-id="8cb44-112">You can use the Fluent API to create a sequence in the model.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Sequence.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="8cb44-113">Также можно настроить дополнительные аспекты последовательности, например ее схему, начальное значение и приращение.</span><span class="sxs-lookup"><span data-stu-id="8cb44-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceConfigured.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);
    }
}
```

<span data-ttu-id="8cb44-114">Как только вводится последовательности, его можно использовать для формирования значений для свойств в модели.</span><span class="sxs-lookup"><span data-stu-id="8cb44-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="8cb44-115">Например, можно использовать [значения по умолчанию](default-values.md) для вставки следующего значения из последовательности.</span><span class="sxs-lookup"><span data-stu-id="8cb44-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceUsed.cs?highlight=11,12,13)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);

        modelBuilder.Entity<Order>()
            .Property(o => o.OrderNo)
            .HasDefaultValueSql("NEXT VALUE FOR shared.OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```
