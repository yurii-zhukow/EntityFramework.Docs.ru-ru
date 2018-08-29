---
title: Вычисляемые столбцы — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: b88efdf69e5100e4eff55f3a41925d2d8e7c3178
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993957"
---
# <a name="computed-columns"></a><span data-ttu-id="9ffac-102">Вычисляемые столбцы</span><span class="sxs-lookup"><span data-stu-id="9ffac-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="9ffac-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="9ffac-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="9ffac-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="9ffac-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="9ffac-105">Вычисляемый столбец является столбцом, значение которого вычисляется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9ffac-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="9ffac-106">Вычисляемый столбец могут использоваться другие столбцы в таблице, для которого требуется вычислить его значение.</span><span class="sxs-lookup"><span data-stu-id="9ffac-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="9ffac-107">Соглашения</span><span class="sxs-lookup"><span data-stu-id="9ffac-107">Conventions</span></span>

<span data-ttu-id="9ffac-108">По соглашению вычисляемые столбцы не создаются в модели.</span><span class="sxs-lookup"><span data-stu-id="9ffac-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="9ffac-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="9ffac-109">Data Annotations</span></span>

<span data-ttu-id="9ffac-110">Вычисляемые столбцы можно настроить не с заметками к данным.</span><span class="sxs-lookup"><span data-stu-id="9ffac-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="9ffac-111">Текучий API</span><span class="sxs-lookup"><span data-stu-id="9ffac-111">Fluent API</span></span>

<span data-ttu-id="9ffac-112">Fluent API можно использовать для указания, что свойство должно сопоставляться вычисляемый столбец.</span><span class="sxs-lookup"><span data-stu-id="9ffac-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/ComputedColumn.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .Property(p => p.DisplayName)
            .HasComputedColumnSql("[LastName] + ', ' + [FirstName]");
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string DisplayName { get; set; }
}
```
