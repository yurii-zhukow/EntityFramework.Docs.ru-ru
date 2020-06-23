---
title: Глобальные фильтры запросов — EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: f6c59bcbab31edcbed22079a1320c060ce08c6f7
ms.sourcegitcommit: 92d54fe3702e0c92e198334da22bacb42e9842b1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/10/2020
ms.locfileid: "84664134"
---
# <a name="global-query-filters"></a>Глобальные фильтры запросов

> [!NOTE]
> Эта возможность появилась в EF Core 2.0.

Глобальные фильтры запросов являются предикатами запросов LINQ (логическое выражение, которое обычно передается в оператор запроса LINQ *Where*), которые применяются непосредственно в типах сущностей в модели метаданных (обычно в *OnModelCreating*). Такие фильтры автоматически применяются к любым запросам LINQ, связанным с этими типами сущностей, включая типы сущностей, указанные косвенно, например, с помощью оператора Include или ссылок на свойства прямой навигации. Ниже приведены некоторые типичные способы применения этой функции.

* **Обратимое удаление**. Тип сущности определяет свойство *IsDeleted*.
* **Мультитенантность**. Тип сущности определяет свойство *TenantId*.

## <a name="example"></a>Пример

В следующем примере показано, как использовать глобальные фильтры запросов для реализации такого поведения запроса, как обратимое удаление и мультитенантность, в простой модели ведения блогов.

> [!TIP]
> Вы можете посмотреть [пример мультитенантности](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) и [примеры использования свойств навигации](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFiltersNavigations) на GitHub. 

Сначала определите сущности:

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

Запишите объявление поля _tenantId_ в сущности _Blog_. Оно будет использоваться для связывания каждого экземпляра блога с конкретным клиентом. Также определено свойство _IsDeleted_ в типе сущности _Post_. Оно используется, чтобы проверять, был ли экземпляр _Post_ удален "обратимо". То есть экземпляр помечен как удаленный без физического удаления базовых данных.

Затем настройте фильтры запросов в _OnModelCreating_, используя API `HasQueryFilter`.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

Выражения предиката, поступающие в вызовы _HasQueryFilter_, будут автоматически применяться ко всем запросам LINQ для этих типов.

> [!TIP]
> Обратите внимание на использование поля уровня экземпляра DbContext. `_tenantId` используется для установки текущего клиента. Фильтры на уровне модели будут использовать значение из правильного экземпляра контекста (то есть экземпляра, выполняющего запрос).

> [!NOTE]
> Сейчас невозможно определить несколько фильтров запросов для одной и той же сущности. Будет применен только последний из них. Однако с помощью логического оператора _AND_ ([`&&` в C#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)) можно определить один фильтр с несколькими условиями.

## <a name="use-of-navigations"></a>Использование свойств навигации

Свойства навигации можно использовать при определении глобальных фильтров запросов. Они применяются рекурсивно: когда выполняется преобразование свойств навигации, используемых в фильтрах запросов, при этом также применяются фильтры, определенные для указанных по ссылке сущностей, что может привести к добавлению дополнительных свойств навигации.

> [!NOTE]
> EF Core сейчас не обнаруживает циклы в определениях глобальных фильтров запросов, поэтому при определении циклов следует соблюдать особую осторожность. Их некорректное указание может делать циклы бесконечными при преобразовании запроса.

## <a name="accessing-entity-with-query-filter-using-reqiured-navigation"></a>Доступ к сущности с фильтром запроса с помощью обязательной навигации

> [!CAUTION]
> Использование обязательной навигации для доступа к сущности, для которой определен глобальный фильтр запросов, может привести к непредвиденным результатам. 

Для обязательной навигации всегда ожидается наличие связанной сущности. Если исключить обязательную связанную сущность с помощью фильтра запросов, родительская сущность может оказаться в непредвиденном состоянии. В результате может быть возвращено меньше элементов, чем ожидалось. 

Чтобы продемонстрировать эту проблему, можно использовать указанные выше сущности `Blog` и `Post` и следующий метод _OnModelCreating_:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>().HasMany(b => b.Posts).WithOne(p => p.Blog).IsRequired();
    modelBuilder.Entity<Blog>().HasQueryFilter(b => b.Url.Contains("fish"));
}
```

Модели можно присвоить начальные значения с помощью следующих данных:

[!code-csharp[Main](../../../samples/core/QueryFiltersNavigations/Program.cs#SeedData)]

Проблема возникает при выполнении двух запросов:

[!code-csharp[Main](../../../samples/core/QueryFiltersNavigations/Program.cs#Queries)]

При такой настройке первый запрос возвращает все шесть сущностей `Post`, однако второй запрос возвращает только три сущности. Это происходит потому, что метод _Include_ во втором запросе загружает связанные сущности `Blog`. Поскольку навигация между `Blog` и `Post` является обязательной, при создании запроса EF Core использует `INNER JOIN`:

```SQL
SELECT [p].[PostId], [p].[BlogId], [p].[Content], [p].[IsDeleted], [p].[Title], [t].[BlogId], [t].[Name], [t].[Url]
FROM [Post] AS [p]
INNER JOIN (
    SELECT [b].[BlogId], [b].[Name], [b].[Url]
    FROM [Blogs] AS [b]
    WHERE CHARINDEX(N'fish', [b].[Url]) > 0
) AS [t] ON [p].[BlogId] = [t].[BlogId]
```

При использовании `INNER JOIN` фильтр исключает все сущности `Post`, чьи связанные сущности `Blog` были удалены глобальным фильтром запросов. 

Для решения этой проблемы можно вместо обязательной навигации использовать необязательную. В этом случае первый запрос остается прежним, но второй запрос будет создавать `LEFT JOIN` и возвращать шесть результатов.

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>().HasMany(b => b.Posts).WithOne(p => p.Blog).IsRequired(false);
    modelBuilder.Entity<Blog>().HasQueryFilter(b => b.Url.Contains("fish"));
}
```

В качестве альтернативного подхода можно указать согласованные фильтры для обеих сущностей `Blog` и `Post`.
В этом случае к `Blog` и `Post` будут применяться совпадающие фильтры. Сущности `Post`, которые могут оказаться в непредвиденном состоянии, удаляются, и оба запроса возвращают три результата. 

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>().HasMany(b => b.Posts).WithOne(p => p.Blog).IsRequired();
    modelBuilder.Entity<Blog>().HasQueryFilter(b => b.Url.Contains("fish"));
    modelBuilder.Entity<Post>().HasQueryFilter(p => p.Blog.Url.Contains("fish"));
}
```

## <a name="disabling-filters"></a>Отключение фильтров

Фильтры можно отключить для отдельных запросов LINQ, используя оператор `IgnoreQueryFilters()`.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Ограничения

Глобальные фильтры запросов имеют следующие ограничения:

* Фильтры можно определить только для корневого типа сущности в иерархии наследования.
