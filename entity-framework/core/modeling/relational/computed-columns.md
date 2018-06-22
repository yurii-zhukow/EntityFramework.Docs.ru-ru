---
title: Вычисляемые столбцы - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
ms.technology: entity-framework-core
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 95312504286bd34cc666b5a21273835c4b35d379
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052484"
---
# <a name="computed-columns"></a><span data-ttu-id="df82a-102">Вычисляемые столбцы</span><span class="sxs-lookup"><span data-stu-id="df82a-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="df82a-103">В этом разделе конфигурации применяется для реляционных баз данных в целом.</span><span class="sxs-lookup"><span data-stu-id="df82a-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="df82a-104">Методы расширения, показанный здесь будут доступны после установки поставщика реляционной базы данных (из-за общей *Microsoft.EntityFrameworkCore.Relational* пакета).</span><span class="sxs-lookup"><span data-stu-id="df82a-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="df82a-105">Вычисляемый столбец является столбцом, значение которого вычисляется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="df82a-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="df82a-106">Вычисляемый столбец можно использовать другие столбцы в таблице, для вычисления его значения.</span><span class="sxs-lookup"><span data-stu-id="df82a-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="df82a-107">Соглашения</span><span class="sxs-lookup"><span data-stu-id="df82a-107">Conventions</span></span>

<span data-ttu-id="df82a-108">По соглашению вычисляемые столбцы не создаются в модели.</span><span class="sxs-lookup"><span data-stu-id="df82a-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="df82a-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="df82a-109">Data Annotations</span></span>

<span data-ttu-id="df82a-110">Вычисляемые столбцы могут не настроены заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="df82a-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="df82a-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="df82a-111">Fluent API</span></span>

<span data-ttu-id="df82a-112">Fluent API можно использовать для указания, что свойство должно сопоставляться с вычисляемым столбцом.</span><span class="sxs-lookup"><span data-stu-id="df82a-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

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
