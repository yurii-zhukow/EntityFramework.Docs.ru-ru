---
title: События .NET — EF Core
description: События .NET, определяемые EF Core
author: ajcvickers
ms.date: 10/15/2020
uid: core/logging-events-diagnostics/events
ms.openlocfilehash: c26032d19d7bc05d30d4576534c7425da4472072
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103024242"
---
# <a name="net-events-in-ef-core"></a>События .NET в EF Core

> [!TIP]
> [Пример событий можно загрузить](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Miscellaneous/Events) с сайта GitHub.

Entity Framework Core (EF Core) предоставляет [события .NET](/dotnet/standard/events/) для выполнения в качестве обратных вызовов при возникновении определенных моментов в коде EF Core. События проще, чем [перехватчики](xref:core/logging-events-diagnostics/interceptors) , и обеспечивают более гибкую регистрацию. Однако они только синхронизируются и поэтому не могут выполнять неблокирующие асинхронные операции ввода-вывода.

События регистрируются для каждого `DbContext` экземпляра. С помощью [прослушивателя диагностики](xref:core/logging-events-diagnostics/diagnostic-listeners) можно получить ту же информацию, но для всех экземпляров DbContext в процессе.

## <a name="events-raised-by-ef-core"></a>События, вызванные EF Core

EF Core вызывает следующие события:

| Событие | Представленная версия | При возникновении
|:------|--------------------|-------
| <xref:Microsoft.EntityFrameworkCore.DbContext.SavingChanges?displayProperty=nameWithType> | 5,0 | В начале <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> или <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A>
| <xref:Microsoft.EntityFrameworkCore.DbContext.SavedChanges?displayProperty=nameWithType> | 5,0 | В конце успешного <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> или <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A>
| <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesFailed?displayProperty=nameWithType> | 5,0 | В конце сбоя <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> или <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A>
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Tracked?displayProperty=nameWithType> | 2.1 | При отслеживании сущности в контексте
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.StateChanged?displayProperty=nameWithType> | 2.1 | Изменение состояния отслеживающей сущности

### <a name="example-timestamp-state-changes"></a>Пример: изменение состояния метки времени

Каждая сущность, прослеживаниющая DbContext, имеет <xref:Microsoft.EntityFrameworkCore.EntityState> . Например, `Added` состояние указывает, что сущность будет вставлена в базу данных.

В этом примере используются <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Tracked> <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.StateChanged> события и для обнаружения изменения состояния сущности. Затем она помечает сущность на текущее время, указывающее, когда произошло это изменение. В результате отмечаются метки времени, указывающие, когда сущность была вставлена, удалена или последний раз обновлялась.

Типы сущностей в этом примере реализуют интерфейс, который определяет свойства метки времени:

<!--
public interface IHasTimestamps
{
    DateTime? Added { get; set; }
    DateTime? Deleted { get; set; }
    DateTime? Modified { get; set; }
}
-->
[!code-csharp[IHasTimestamps](../../../samples/core/Miscellaneous/Events/Program.cs?name=IHasTimestamps)]

Затем метод в DbContext приложения может установить метки времени для любой сущности, реализующей этот интерфейс:

<!--
    private static void UpdateTimestamps(object sender, EntityEntryEventArgs e)
    {
        if (e.Entry.Entity is IHasTimestamps entityWithTimestamps)
        {
            switch (e.Entry.State)
            {
                case EntityState.Deleted:
                    entityWithTimestamps.Deleted = DateTime.UtcNow;
                    Console.WriteLine($"Stamped for delete: {e.Entry.Entity}");
                    break;
                case EntityState.Modified:
                    entityWithTimestamps.Modified = DateTime.UtcNow;
                    Console.WriteLine($"Stamped for update: {e.Entry.Entity}");
                    break;
                case EntityState.Added:
                    entityWithTimestamps.Added = DateTime.UtcNow;
                    Console.WriteLine($"Stamped for insert: {e.Entry.Entity}");
                    break;
            }
        }
    }
-->
[!code-csharp[UpdateTimestamps](../../../samples/core/Miscellaneous/Events/Program.cs?name=UpdateTimestamps)]

Этот метод имеет соответствующую сигнатуру для использования в качестве обработчика событий для `Tracked` событий и `StateChanged` . Обработчик регистрируется для обоих событий в конструкторе DbContext. Обратите внимание, что события могут быть присоединены к DbContext в любое время; Это не обязательно, если это происходит в конструкторе контекста.

<!--
    public BlogsContext()
    {
        ChangeTracker.StateChanged += UpdateTimestamps;
        ChangeTracker.Tracked += UpdateTimestamps;
    }
-->
[!code-csharp[ContextConstructor](../../../samples/core/Miscellaneous/Events/Program.cs?name=ContextConstructor)]

Оба события необходимы, так как новые сущности инициируют `Tracked` события при первой отслеживании. `StateChanged` события срабатывают только для сущностей, которые изменяют состояние, пока они _уже_ отслеживается.

[Пример](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Miscellaneous/Events) для этого примера содержит простое консольное приложение, которое вносит изменения в базу данных блогов:

<!--
        using (var context = new BlogsContext())
        {
            context.Database.EnsureDeleted();
            context.Database.EnsureCreated();

            context.Add(
                new Blog
                {
                    Id = 1,
                    Name = "EF Blog",
                    Posts =
                    {
                        new Post { Id = 1, Title = "EF Core 3.1!" },
                        new Post { Id = 2, Title = "EF Core 5.0!" }
                    }
                });

            context.SaveChanges();
        }

        using (var context = new BlogsContext())
        {
            var blog = context.Blogs.Include(e => e.Posts).Single();

            blog.Name = "EF Core Blog";
            context.Remove(blog.Posts.First());
            blog.Posts.Add(new Post { Id = 3, Title = "EF Core 6.0!" });

            context.SaveChanges();
        }
-->
[!code-csharp[Demonstration](../../../samples/core/Miscellaneous/Events/Program.cs?name=Demonstration)]

Выходные данные этого кода показывают изменения состояния и применяемые отметки времени:

```output
Stamped for insert: Blog 1 Added on: 10/15/2020 11:01:26 PM
Stamped for insert: Post 1 Added on: 10/15/2020 11:01:26 PM
Stamped for insert: Post 2 Added on: 10/15/2020 11:01:26 PM
Stamped for delete: Post 1 Added on: 10/15/2020 11:01:26 PM Deleted on: 10/15/2020 11:01:26 PM
Stamped for update: Blog 1 Added on: 10/15/2020 11:01:26 PM Modified on: 10/15/2020 11:01:26 PM
Stamped for insert: Post 3 Added on: 10/15/2020 11:01:26 PM
```
