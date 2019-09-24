---
title: Вычисленные столбцы — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: da106c94698a202744d7cd465aa84d0d72802833
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197230"
---
# <a name="computed-columns"></a><span data-ttu-id="f680c-102">Вычисленные столбцы</span><span class="sxs-lookup"><span data-stu-id="f680c-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="f680c-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="f680c-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="f680c-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="f680c-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="f680c-105">Вычисляемый столбец — это столбец, значение которого вычисляется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f680c-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="f680c-106">Вычисляемый столбец может использовать другие столбцы таблицы для вычисления ее значения.</span><span class="sxs-lookup"><span data-stu-id="f680c-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="f680c-107">Соглашения</span><span class="sxs-lookup"><span data-stu-id="f680c-107">Conventions</span></span>

<span data-ttu-id="f680c-108">По соглашению, вычисленные столбцы не создаются в модели.</span><span class="sxs-lookup"><span data-stu-id="f680c-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="f680c-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="f680c-109">Data Annotations</span></span>

<span data-ttu-id="f680c-110">Вычисленные столбцы не могут быть настроены с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="f680c-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="f680c-111">Текучий API</span><span class="sxs-lookup"><span data-stu-id="f680c-111">Fluent API</span></span>

<span data-ttu-id="f680c-112">API-интерфейс Fluent можно использовать для указания того, что свойство должно сопоставляться с вычисляемым столбцом.</span><span class="sxs-lookup"><span data-stu-id="f680c-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/ComputedColumn.cs?highlight=9)] -->
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
