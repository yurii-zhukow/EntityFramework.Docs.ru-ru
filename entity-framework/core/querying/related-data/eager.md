---
title: Безотложная загрузка связанных данных — EF Core
description: Безотложная загрузка связанных данных с помощью Entity Framework Core
author: roji
ms.date: 9/8/2020
uid: core/querying/related-data/eager
ms.openlocfilehash: 66956fcd85bb21a08c69fa93b93c12382bbfc8eb
ms.sourcegitcommit: 788a56c2248523967b846bcca0e98c2ed7ef0d6b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2020
ms.locfileid: "95003579"
---
# <a name="eager-loading-of-related-data"></a>Безотложная загрузка связанных данных

## <a name="eager-loading"></a>Безотложная загрузка

Вы можете использовать метод `Include`, чтобы указать связанные данные, которые будут включены в результаты запроса. В следующем примере блоги, возвращенные в результатах, будут иметь свойство `Posts`, заполненное соответствующими записями.

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#SingleInclude)]

> [!TIP]
> EF Core автоматически исправляет свойства навигации для других сущностей, которые были ранее загружены в экземпляр контекста. Даже если данные для свойства навигации не включены явно, свойство все равно можно заполнить при условии, что ранее были загружены некоторые или все связанные сущности.

Связанные данные из нескольких связей можно включить в один запрос.

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#MultipleIncludes)]

> [!CAUTION]
> Безотложная загрузка навигации по коллекции в одном запросе может вызвать проблемы с производительностью. Дополнительные сведения см. в статье [Отдельные и разделенные запросы](xref:core/querying/single-split-queries).

## <a name="including-multiple-levels"></a>Включение нескольких уровней

Вы можете детализировать отношения, чтобы включить несколько уровней связанных данных, используя метод `ThenInclude`. В следующем примере загружаются все блоги, связанные с ними записи и автор каждой записи.

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#SingleThenInclude)]

Вы можете связать в цепочку несколько вызовов `ThenInclude`, чтобы продолжить включение уровней связанных данных.

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#MultipleThenIncludes)]

Вы можете объединить все эти вызовы, чтобы включить связанные данные из нескольких уровней и нескольких корней в один и тот же запрос.

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#IncludeTree)]

Может потребоваться включить несколько связанных сущностей для одной включенной сущности. Например, при запросе `Blogs` вы включаете `Posts`, а затем хотите включить `Author` и `Tags` из `Posts`. Для включения обеих сущностей вам нужно указать каждый путь включения, начиная с корня. Например, `Blog -> Posts -> Author` и `Blog -> Posts -> Tags`. Это не означает, что вы получите избыточные соединения, в большинстве случаев EF будет комбинировать соединения при создании SQL.

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#MultipleLeafIncludes)]

> [!TIP]
> Можно также загрузить несколько объектов навигации с помощью одного метода `Include`. Это возможно для "цепочек" навигации, где все объекты являются ссылками или объединяются в одну коллекцию.

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#IncludeMultipleNavigationsWithSingleInclude)]

## <a name="filtered-include"></a>Включение с фильтрацией

> [!NOTE]
> Эта возможность появилась в EF Core 5.0.

При применении метода Include для загрузки связанных данных можно добавить определенные перечислимые операции к включенной навигации по коллекции, что позволяет фильтровать и сортировать результаты.

Поддерживаются операции: `Where`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `Skip` и `Take`.

Такие операции должны применяться к навигации коллекции в лямбда-выражении, переданном в метод Include, как показано в примере ниже:

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#FilteredInclude)]

Каждая включаемая навигация позволяет выполнять только один уникальный набор операций фильтра. В тех случаях, когда для данной навигации по коллекции применяются несколько операций Include (`blog.Posts` в приведенных ниже примерах), операции фильтрации можно указывать только в одном из них:

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#MultipleLeafIncludesFiltered1)]

Вместо этого можно применить идентичные операции для каждой навигации, которая включается несколько раз:

[!code-csharp[Main](../../../../samples/core/Querying/RelatedData/Program.cs#MultipleLeafIncludesFiltered2)]

> [!CAUTION]
> При отслеживании запросов результаты включения с фильтрацией могут быть неожиданными из-за [исправления навигации](xref:core/querying/tracking). Все связанные сущности, которые были запрошены ранее и сохранены в средстве отслеживания изменений, будут представлены в результатах запроса включения с фильтрацией, даже если они не соответствуют требованиям фильтра. При использовании в таких ситуациях включения с фильтрацией рассмотрите возможность работы с запросами `NoTracking` или повторного создания DbContext.

Пример.

```csharp
var orders = context.Orders.Where(o => o.Id > 1000).ToList();

// customer entities will have references to all orders where Id > 1000, rather than > 5000
var filtered = context.Customers.Include(c => c.Orders.Where(o => o.Id > 5000)).ToList();
```

> [!NOTE]
> Если используются отслеживаемые запросы, будет загружена та навигация, для которой применено включение с фильтрацией. Это означает, что EF Core не будет пытаться повторно загрузить значения с помощью [явной загрузки](xref:core/querying/related-data/explicit) или [отложенной загрузки](xref:core/querying/related-data/lazy), даже если некоторые элементы будут отсутствовать.

## <a name="include-on-derived-types"></a>Использование метода Include с производными типами

Вы можете включать связанные данные из навигации, определенной только с производным типом, используя `Include` и `ThenInclude`.

Рассмотрим следующую модель:

```csharp
public class SchoolContext : DbContext
{
    public DbSet<Person> People { get; set; }
    public DbSet<School> Schools { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<School>().HasMany(s => s.Students).WithOne(s => s.School);
    }
}

public class Person
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public class Student : Person
{
    public School School { get; set; }
}

public class School
{
    public int Id { get; set; }
    public string Name { get; set; }

    public List<Student> Students { get; set; }
}
```

Содержимое навигации `School` всех людей, которые являются учениками, может быть активно загружено с помощью нескольких шаблонов:

* С помощью приведения.

  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

* С помощью оператора `as`.

  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

* С помощью перегрузки `Include`, принимающий параметр типа `string`.

  ```csharp
  context.People.Include("School").ToList()
  ```
