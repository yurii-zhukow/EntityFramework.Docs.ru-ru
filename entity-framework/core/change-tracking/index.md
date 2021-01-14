---
title: Отслеживание изменений — EF Core
description: Общие сведения об отслеживании изменений для EF Core
author: ajcvickers
ms.date: 12/30/2020
uid: core/change-tracking/index
ms.openlocfilehash: 52223e5472b09271d19ac9449a3989b4a0e277f7
ms.sourcegitcommit: 032a1767d7a6e42052a005f660b80372c6521e7e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98129524"
---
# <a name="change-tracking-in-ef-core"></a>Отслеживание изменений в EF Core

Каждый экземпляр <xref:Microsoft.EntityFrameworkCore.DbContext> отслеживает изменения, внесенные в сущности. Эти отслеживаемые сущности, в свою очередь, записывают изменения в базу данных при вызове <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A>.

В этом документе представлены общие сведения об отслеживании изменений Entity Framework Core (EF Core) и о том, как это связано с запросами и обновлениями.

> [!TIP]
> Вы можете запустить и отладить весь код, используемый в этой документации, [скачав пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/ChangeTracking/ChangeTrackingInEFCore).

> [!TIP]
> Для простоты в этой документации используются и описываются синхронные методы, такие как <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A>, а не их асинхронные эквиваленты, такие как <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A>. Вызов и ожидание асинхронного метода можно заменить, если не указано иное.

## <a name="how-to-track-entities"></a>Как отслеживать сущности

Экземпляры сущностей отслеживаются, если они:

- получены из запросов к базе данных;
- явно присоединены к DbContext с помощью `Add`, `Attach`, `Update` или аналогичных методов;
- обнаружены как новые сущности, подключенные к существующим отслеживаемым сущностям.

Экземпляры сущностей больше не отслеживаются, если:

- экземпляр DbContext удален;
- средство отслеживания изменений очищено (EF Core 5.0 и более поздних версий);
- сущности явно окончательно удалены.

DbContext используется для представления кратковременной единицы работы, как описано в статье [Время существования, настройка и инициализация DbContext](xref:core/dbcontext-configuration/index). Это означает, что удаление DbContext — это _обычный способ_ остановки отслеживания сущностей. Иными словами, время существования DbContext должно быть следующим:

1. Создание экземпляра DbContext.
2. Отслеживание определенных сущностей.
3. Внесение определенных изменений в сущности.
4. Вызов метода SaveChanges для обновления базы данных.
5. Удаление экземпляра DbContext.

> [!TIP]
> Вам не нужно очищать средство отслеживания изменений или явно окончательно удалять экземпляры сущностей при использовании этого подхода. Но, если необходимо окончательно удалить сущности, более эффективным будет вызов <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Clear%2A?displayProperty=nameWithType>, чем удаление сущностей по одной.

## <a name="entity-states"></a>Состояния сущностей

Каждая сущность связана с заданным состоянием <xref:Microsoft.EntityFrameworkCore.EntityState>:

- Сущности `Detached` не отслеживаются <xref:Microsoft.EntityFrameworkCore.DbContext>.
- Сущности `Added` являются новыми и еще не вставлены в базу данных. Это означает, что они будут вставлены при вызове <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A>.
- Сущности `Unchanged` _не_ изменены, так как они были запрошены из базы данных. Все сущности, получаемые из запросов, изначально находятся в этом состоянии.
- Сущности `Modified` были изменены с момента запроса из базы данных. Это означает, что они будут обновлены при вызове SaveChanges.
- Сущности `Deleted` существуют в базе данных, но отмечены для удаления при вызове SaveChanges.

EF Core отслеживает изменения на уровне свойств. Например, если изменяется только одно значение свойства, обновление базы данных изменит только это значение. При этом свойства помечаются как измененные, только если сущность находится в состоянии Modified. (С другой стороны, состояние Modified означает, что хотя бы одно значение свойства помечено как измененное.)

В следующей таблице описаны различия между состояниями.

| Состояние сущностей     | Отслеживается DbContext | Существует в базе данных | Измененные свойства | Действие при вызове SaveChanges
|:-----------------|----------------------|--------------------|---------------------|-----------------------
| `Detached`       | Нет                   | -                  | -                   | -
| `Added`          | Да                  | Нет                 | -                   | Вставить
| `Unchanged`      | Да                  | Да                | Нет                  | -
| `Modified`       | Да                  | Да                | Да                 | Update
| `Deleted`        | Да                  | Да                | -                   | Удалить

> [!NOTE]
> В этом тексте для ясности используются термины, связанные с реляционной базой данных. Базы данных NoSQL обычно поддерживают аналогичные операции, но, возможно, с другими именами. Дополнительные сведения см. в документации поставщика базы данных.

## <a name="tracking-from-queries"></a>Отслеживание из запросов

Отслеживание изменений EF Core наиболее эффективно, когда один и тот же экземпляр <xref:Microsoft.EntityFrameworkCore.DbContext> используется для запрашивания сущностей и их обновления путем вызова <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A>. Это происходит потому, что EF Core автоматически отслеживает состояние запрашиваемых сущностей и определяет изменения, внесенные в эти сущности при вызове SaveChanges.

Этот подход имеет несколько преимуществ по сравнению с [явным отслеживанием экземпляров сущностей](xref:core/change-tracking/explicit-tracking):

- Он является простым. Состояниями сущностей редко требуется управлять явным образом — EF Core следит за изменениями состояния.
- Обновления ограничиваются только теми значениями, которые действительно изменились.
- Значения [теневых свойств](xref:core/modeling/shadow-properties) сохраняются и используются по мере необходимости. Это особенно важно, если внешние ключи хранятся в теневом состоянии.
- Исходные значения свойств сохраняются автоматически и используются для эффективного обновления.

## <a name="simple-query-and-update"></a>Простой запрос и обновление

Например, рассмотрим простую модель с участием блогов и публикаций:

<!--
public class Blog
{
    public int Id { get; set; }

    public string Name { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }

    public int? BlogId { get; set; }
    public Blog Blog { get; set; }
}
-->
[!code-csharp[Model](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/GeneratedKeysSamples.cs?name=Model)]

Эту модель можно использовать для запрашивания блогов и публикаций с последующим внесением определенных обновлений в базу данных:

<!--
            using var context = new BlogsContext();

            var blog = context.Blogs.Include(e => e.Posts).First(e => e.Name == ".NET Blog");

            blog.Name = ".NET Blog (Updated!)";

            foreach (var post in blog.Posts.Where(e => !e.Title.Contains("5.0")))
            {
                post.Title = post.Title.Replace("5", "5.0");
            }

            context.SaveChanges();
-->
[!code-csharp[Simple_query_and_update_1](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/GeneratedKeysSamples.cs?name=Simple_query_and_update_1)]

Вызов SaveChanges приводит к следующим обновлениям базы данных с использованием SQLite в качестве примера базы данных:

```sql
-- Executed DbCommand (0ms) [Parameters=[@p1='1' (DbType = String), @p0='.NET Blog (Updated!)' (Size = 20)], CommandType='Text', CommandTimeout='30']
UPDATE "Blogs" SET "Name" = @p0
WHERE "Id" = @p1;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p1='2' (DbType = String), @p0='Announcing F# 5.0' (Size = 17)], CommandType='Text', CommandTimeout='30']
UPDATE "Posts" SET "Title" = @p0
WHERE "Id" = @p1;
SELECT changes();
```

[Представление отладки средства отслеживания изменений](xref:core/change-tracking/debug-views) — это удобный способ визуализации отслеживаемых сущностей и их состояния. Например, перед вызовом SaveChanges вставьте следующий код в приведенный выше пример:

<!--
                context.ChangeTracker.DetectChanges();
                Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Simple_query_and_update_2](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/GeneratedKeysSamples.cs?name=Simple_query_and_update_2)]

Будут получены следующие выходные данные:

```output
Blog {Id: 1} Modified
  Id: 1 PK
  Name: '.NET Blog (Updated!)' Modified Originally '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}, {Id: 3}]
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
Post {Id: 2} Modified
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5.0' Modified Originally 'Announcing F# 5'
  Blog: {Id: 1}
```

Учитывайте следующее:

- Свойство `Blog.Name` помечено как измененное (`Name: '.NET Blog (Updated!)' Modified Originally '.NET Blog'`), что приводит к тому, что состояние блога изменяется на `Modified`.
- Свойство `Post.Title` публикации 2 помечено как измененное (`Title: 'Announcing F# 5.0' Modified Originally 'Announcing F# 5'`), что приводит к тому, что состояние публикации изменяется на `Modified`.
- Другие значения свойств публикации 2 не изменились и поэтому не помечаются как измененные. Именно поэтому эти значения не включаются в обновление базы данных.
- Другая публикация вообще не была изменена. Именно поэтому она по-прежнему находится в состоянии `Unchanged` и не включается в обновление базы данных.

## <a name="query-then-insert-update-and-delete"></a>Запрос, вставка, обновление и удаление

Такие обновления, как в предыдущем примере, можно сочетать с операциями вставки и удаления в одной единице работы. Пример:

<!--
            using var context = new BlogsContext();

            var blog = context.Blogs.Include(e => e.Posts).First(e => e.Name == ".NET Blog");

            // Modify property values
            blog.Name = ".NET Blog (Updated!)";

            // Insert a new Post
            blog.Posts.Add(new Post
            {
                Title = "What’s next for System.Text.Json?",
                Content = ".NET 5.0 was released recently and has come with many..."
            });

            // Mark an existing Post as Deleted
            var postToDelete = blog.Posts.Single(e => e.Title == "Announcing F# 5");
            context.Remove(postToDelete);

            context.ChangeTracker.DetectChanges();
            Console.WriteLine(context.ChangeTracker.DebugView.LongView);

            context.SaveChanges();
-->
[!code-csharp[Query_then_insert_update_and_delete_1](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/GeneratedKeysSamples.cs?name=Query_then_insert_update_and_delete_1)]

В этом примере:

- Блог и связанные с ним публикации запрашиваются из базы данных и отслеживаются.
- Свойство `Blog.Name` изменяется.
- В коллекцию существующих записей блога добавляется новая публикация.
- Существующая публикация помечается для удаления путем вызова <xref:Microsoft.EntityFrameworkCore.DbContext.Remove%2A?displayProperty=nameWithType>.

Еще раз взгляните на [представление отладки средства отслеживания изменений](xref:core/change-tracking/debug-views) перед вызовом команды SaveChanges. Вы увидите, как EF Core отслеживает эти изменения:

```output
Blog {Id: 1} Modified
  Id: 1 PK
  Name: '.NET Blog (Updated!)' Modified Originally '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}, {Id: 3}, {Id: -2147482638}]
Post {Id: -2147482638} Added
  Id: -2147482638 PK Temporary
  BlogId: 1 FK
  Content: '.NET 5.0 was released recently and has come with many...'
  Title: 'What's next for System.Text.Json?'
  Blog: {Id: 1}
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
Post {Id: 2} Deleted
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: {Id: 1}
```

Обратите внимание на указанные ниже моменты.

- Блог помечен как `Modified`. Это вызовет обновление базы данных.
- Публикация 2 помечена как `Deleted`. Это вызовет удаление базы данных.
- Новая публикация с временным идентификатором связана с блогом 1 и помечена как `Added`. Это вызовет вставку в базу данных.

Это приводит к выполнению следующих команд базы данных (при использовании SQLite) при вызове SaveChanges:

```sql
-- Executed DbCommand (0ms) [Parameters=[@p1='1' (DbType = String), @p0='.NET Blog (Updated!)' (Size = 20)], CommandType='Text', CommandTimeout='30']
UPDATE "Blogs" SET "Name" = @p0
WHERE "Id" = @p1;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p0='2' (DbType = String)], CommandType='Text', CommandTimeout='30']
DELETE FROM "Posts"
WHERE "Id" = @p0;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p0='1' (DbType = String), @p1='.NET 5.0 was released recently and has come with many...' (Size = 56), @p2='What's next for System.Text.Json?' (Size = 33)], CommandType='Text', CommandTimeout='30']
INSERT INTO "Posts" ("BlogId", "Content", "Title")
VALUES (@p0, @p1, @p2);
SELECT "Id"
FROM "Posts"
WHERE changes() = 1 AND "rowid" = last_insert_rowid();
```

Дополнительные сведения о вставке и удалении сущностей см. в статье [Явное отслеживание сущностей](xref:core/change-tracking/explicit-tracking). Дополнительные сведения о том, как EF Core автоматически обнаруживает подобные изменения, см. в статье [Обнаружение изменений и уведомления](xref:core/change-tracking/change-detection).

> [!TIP]
> Вызовите <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.HasChanges?displayProperty=nameWithType>, чтобы определить, были ли внесены какие-либо изменения, которые приведут к обновлению базы данных с помощью SaveChanges. Если HasChanges возвращает значение false, SaveChanges не будет выполняться.
