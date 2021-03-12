---
title: Явная отслеживание сущностей — EF Core
description: Явная отслеживание сущностей с помощью DbContext с использованием добавления, присоединения, обновления и удаления
author: ajcvickers
ms.date: 12/30/2020
uid: core/change-tracking/explicit-tracking
ms.openlocfilehash: a0e51db4a70338e725ed40caa57a7f02245f0707
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103024476"
---
# <a name="explicitly-tracking-entities"></a>Явная отслеживание сущностей

Каждый экземпляр <xref:Microsoft.EntityFrameworkCore.DbContext> отслеживает изменения, внесенные в сущности. Эти отслеживаемые сущности, в свою очередь, записывают изменения в базу данных при вызове <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A>.

Отслеживание изменений Entity Framework Core (EF Core) лучше подходит, если один и тот же <xref:Microsoft.EntityFrameworkCore.DbContext> экземпляр используется для запроса сущностей и их обновления путем вызова <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> . Это происходит потому, что EF Core автоматически отслеживает состояние запрашиваемых сущностей и определяет изменения, внесенные в эти сущности при вызове SaveChanges. Этот подход рассматривается в [Отслеживание изменений EF Core](xref:core/change-tracking/index).

> [!TIP]
> В этом документе предполагается, что состояния сущностей и основы отслеживания изменений EF Core понятны. Дополнительные сведения по этим темам см. [в разделе Отслеживание изменений в EF Core](xref:core/change-tracking/index) .

> [!TIP]
> Вы можете запустить и отладить весь код, используемый в этой документации, [скачав пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/ChangeTracking/ChangeTrackingInEFCore).

> [!TIP]
> Для простоты в этом документе используются и ссылаются на синхронные методы, такие как, <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> а не на их асинхронные эквиваленты, такие как <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A> . Вызов и ожидание асинхронного метода можно заменить, если не указано иное.

## <a name="introduction"></a>Введение

Сущности могут быть явно «присоединены» к <xref:Microsoft.EntityFrameworkCore.DbContext> таким, что затем контекст отслеживает эти сущности. Это в первую очередь полезно в следующих случаях:

1. Создание новых сущностей, которые будут вставлены в базу данных.
2. Повторное подключение отключенных сущностей, которые ранее запрашивались _другим_ экземпляром DbContext.

Первый из них потребуется большинству приложений, а является первичным, обрабатываемым <xref:Microsoft.EntityFrameworkCore.DbContext.Add%2A?displayProperty=nameWithType> методами.

Вторая необходима только приложениям, которые изменяют сущности или их связи, **_пока сущности не отслеживаемы_**. Например, веб-приложение может отправлять сущности веб-клиенту, где пользователь вносит изменения и отправляет сущности обратно. Эти сущности называются "отключенными", так как они были изначально запрошены из DbContext, но были отключены от этого контекста при отправке клиенту.

Теперь веб-приложение должно повторно присоединить эти сущности, чтобы они снова отправляются и указывали внесенные изменения, которые <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> могут внести соответствующие обновления в базу данных. Это в первую очередь обрабатывается <xref:Microsoft.EntityFrameworkCore.DbContext.Attach%2A?displayProperty=nameWithType> <xref:Microsoft.EntityFrameworkCore.DbContext.Update%2A?displayProperty=nameWithType> методами и.

> [!TIP]
> Присоединение сущностей к тому _же экземпляру DbContext_ , из которого они были запрошены, не должен быть обычно нужен. Не осуществляйте регулярный запрос без отслеживания, а затем присоедините возвращенные сущности к тому же контексту. Это будет медленнее, чем при использовании отслеживаемого запроса, а также может привести к таким проблемам, как отсутствие значений теневых свойств, что затрудняет его получение.

### <a name="generated-versus-explicit-key-values"></a>Созданные и явные значения ключа

По умолчанию [Свойства ключа](xref:core/modeling/keys) Integer и GUID настраиваются для использования [автоматически создаваемых значений ключа](xref:core/modeling/generated-properties). Это имеет **существенное преимущество для отслеживания изменений: неопределенное значение ключа указывает, что сущность является "новой"**. По «новому» мы имеем в виду, что он еще не вставлен в базу данных.

В следующих разделах используются две модели. Во-первых, настроено **не** использовать созданные значения ключа:

<!--
    public class Blog
    {
        [DatabaseGenerated(DatabaseGeneratedOption.None)]
        public int Id { get; set; }

        public string Name { get; set; }

        public IList<Post> Posts { get; } = new List<Post>();
    }

    public class Post
    {
        [DatabaseGenerated(DatabaseGeneratedOption.None)]
        public int Id { get; set; }

        public string Title { get; set; }
        public string Content { get; set; }

        public int? BlogId { get; set; }
        public Blog Blog { get; set; }
    }
-->
[!code-csharp[Model](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysSamples.cs?name=Model)]

Несгенерированные (т. е. явно задаются) значения ключей отображаются первыми в каждом примере, так как все является очевидным и легко поддерживайтеся. Затем следует пример, в котором используются созданные значения ключа:

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

Обратите внимание, что ключевые свойства в этой модели не требуют дополнительной настройки, так как использование созданных значений ключа является [значением по умолчанию для простых целочисленных ключей](xref:core/modeling/generated-properties).

## <a name="inserting-new-entities"></a>Вставка новых сущностей

### <a name="explicit-key-values"></a>Явные значения ключа

Сущность должна быть отслеживаниь в `Added` состоянии, которое будет вставлено <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> . Сущности обычно помещаются в добавленное состояние путем вызова одного из <xref:Microsoft.EntityFrameworkCore.DbContext.Add%2A?displayProperty=nameWithType> ,,, <xref:Microsoft.EntityFrameworkCore.DbContext.AddRange%2A?displayProperty=nameWithType> <xref:Microsoft.EntityFrameworkCore.DbContext.AddAsync%2A?displayProperty=nameWithType> <xref:Microsoft.EntityFrameworkCore.DbContext.AddRangeAsync%2A?displayProperty=nameWithType> или эквивалентных методов в <xref:Microsoft.EntityFrameworkCore.DbSet%601> .

> [!TIP]
> Все эти методы работают таким же образом в контексте отслеживания изменений. Дополнительные сведения см. в разделе [дополнительные отслеживание изменений функции](xref:core/change-tracking/miscellaneous) .

Например, чтобы начать отслеживание нового блога, выполните следующие действия.

<!--
            context.Add(
                new Blog
                {
                    Id = 1,
                    Name = ".NET Blog",
                });
-->
[!code-csharp[Inserting_new_entities_1](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysSamples.cs?name=Inserting_new_entities_1)]

Проверка [представления отладки отслеживания изменений](xref:core/change-tracking/debug-views) после этого вызова показывает, что контекст отслеживает новую сущность в `Added` состоянии:

```output
Blog {Id: 1} Added
  Id: 1 PK
  Name: '.NET Blog'
  Posts: []
```

Однако методы Add не просто работают с отдельной сущностью. Фактически они начинают отслеживать _весь граф связанных сущностей_, помещая их в `Added` состояние. Например, чтобы вставить новый блог и связанные с ним новые записи:

<!--
            context.Add(
                new Blog
                {
                    Id = 1,
                    Name = ".NET Blog",
                    Posts =
                    {
                        new Post
                        {
                            Id = 1,
                            Title = "Announcing the Release of EF Core 5.0",
                            Content = "Announcing the release of EF Core 5.0, a full featured cross-platform..."
                        },
                        new Post
                        {
                            Id = 2,
                            Title = "Announcing F# 5",
                            Content = "F# 5 is the latest version of F#, the functional programming language..."
                        }
                    }
                });
-->
[!code-csharp[Inserting_new_entities_2](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysSamples.cs?name=Inserting_new_entities_2)]

Теперь контекст отслеживает все эти сущности следующим образом `Added` :

```output
Blog {Id: 1} Added
  Id: 1 PK
  Name: '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}]
Post {Id: 1} Added
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
Post {Id: 2} Added
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: {Id: 1}
```

Обратите внимание, что для `Id` ключевых свойств в приведенных выше примерах были заданы явные значения. Это обусловлено тем, что в модели настроено использование явно заданных значений ключей, а не автоматически создаваемых значений ключа. Если созданные ключи не используются, _перед_ вызовом необходимо явно задать ключевые свойства `Add` . Эти ключевые значения затем вставляются при вызове SaveChanges. Например, при использовании SQLite:

```sql
-- Executed DbCommand (0ms) [Parameters=[@p0='1' (DbType = String), @p1='.NET Blog' (Size = 9)], CommandType='Text', CommandTimeout='30']
INSERT INTO "Blogs" ("Id", "Name")
VALUES (@p0, @p1);

-- Executed DbCommand (0ms) [Parameters=[@p2='1' (DbType = String), @p3='1' (DbType = String), @p4='Announcing the release of EF Core 5.0, a full featured cross-platform...' (Size = 72), @p5='Announcing the Release of EF Core 5.0' (Size = 37)], CommandType='Text', CommandTimeout='30']
INSERT INTO "Posts" ("Id", "BlogId", "Content", "Title")
VALUES (@p2, @p3, @p4, @p5);

-- Executed DbCommand (0ms) [Parameters=[@p0='2' (DbType = String), @p1='1' (DbType = String), @p2='F# 5 is the latest version of F#, the functional programming language...' (Size = 72), @p3='Announcing F# 5' (Size = 15)], CommandType='Text', CommandTimeout='30']
INSERT INTO "Posts" ("Id", "BlogId", "Content", "Title")
VALUES (@p0, @p1, @p2, @p3);
```

Все эти сущности будут отслеживаниться в `Unchanged` состоянии после завершения SaveChanges, так как эти сущности теперь существуют в базе данных:

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}]
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
Post {Id: 2} Unchanged
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: {Id: 1}
```

### <a name="generated-key-values"></a>Созданные значения ключа

Как упоминалось выше, [Свойства ключа](xref:core/modeling/keys) Integer и GUID настраиваются для использования [автоматически создаваемых значений ключа](xref:core/modeling/generated-properties) по умолчанию. Это означает, что приложение _не должно явно задавать значение ключа_. Например, чтобы вставить новый блог и опубликовать все записи с созданными значениями ключа:

<!--
            context.Add(
                new Blog
                {
                    Name = ".NET Blog",
                    Posts =
                    {
                        new Post
                        {
                            Title = "Announcing the Release of EF Core 5.0",
                            Content = "Announcing the release of EF Core 5.0, a full featured cross-platform..."
                        },
                        new Post
                        {
                            Title = "Announcing F# 5",
                            Content = "F# 5 is the latest version of F#, the functional programming language..."
                        }
                    }
                });
-->
[!code-csharp[Inserting_new_entities_3](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/GeneratedKeysSamples.cs?name=Inserting_new_entities_3)]

Как и с явными значениями ключа, контекст теперь отслеживает все эти сущности следующим образом `Added` :

```output
Blog {Id: -2147482644} Added
  Id: -2147482644 PK Temporary
  Name: '.NET Blog'
  Posts: [{Id: -2147482637}, {Id: -2147482636}]
Post {Id: -2147482637} Added
  Id: -2147482637 PK Temporary
  BlogId: -2147482644 FK Temporary
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: -2147482644}
Post {Id: -2147482636} Added
  Id: -2147482636 PK Temporary
  BlogId: -2147482644 FK Temporary
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: {Id: -2147482644}
```

Обратите внимание, что в этом случае для каждой сущности были созданы [временные значения ключей](xref:core/change-tracking/miscellaneous#temporary-values) . Эти значения используются EF Core до вызова SaveChanges, после чего значения реальных ключей считываются из базы данных. Например, при использовании SQLite:

```sql
-- Executed DbCommand (0ms) [Parameters=[@p0='.NET Blog' (Size = 9)], CommandType='Text', CommandTimeout='30']
INSERT INTO "Blogs" ("Name")
VALUES (@p0);
SELECT "Id"
FROM "Blogs"
WHERE changes() = 1 AND "rowid" = last_insert_rowid();

-- Executed DbCommand (0ms) [Parameters=[@p1='1' (DbType = String), @p2='Announcing the release of EF Core 5.0, a full featured cross-platform...' (Size = 72), @p3='Announcing the Release of EF Core 5.0' (Size = 37)], CommandType='Text', CommandTimeout='30']
INSERT INTO "Posts" ("BlogId", "Content", "Title")
VALUES (@p1, @p2, @p3);
SELECT "Id"
FROM "Posts"
WHERE changes() = 1 AND "rowid" = last_insert_rowid();

-- Executed DbCommand (0ms) [Parameters=[@p0='1' (DbType = String), @p1='F# 5 is the latest version of F#, the functional programming language...' (Size = 72), @p2='Announcing F# 5' (Size = 15)], CommandType='Text', CommandTimeout='30']
INSERT INTO "Posts" ("BlogId", "Content", "Title")
VALUES (@p0, @p1, @p2);
SELECT "Id"
FROM "Posts"
WHERE changes() = 1 AND "rowid" = last_insert_rowid();
```

После завершения SaveChanges все сущности были обновлены с использованием их реальных значений ключей и отправляются в `Unchanged` состояние, так как теперь они соответствуют состоянию в базе данных:

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}]
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
Post {Id: 2} Unchanged
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: {Id: 1}
```

Это точно такое же конечное состояние, как и в предыдущем примере, в котором использовались явные значения ключа.

> [!TIP]
> Явное значение ключа по-прежнему можно задать даже при использовании созданных значений ключа. После этого EF Core попытается вставить с использованием этого значения ключа. Некоторые конфигурации базы данных, в том числе SQL Server со столбцами идентификаторов, не поддерживают такие вставки и выдадут ([см. Эти документы в качестве обходного решения](xref:core/providers/sql-server/value-generation#inserting-explicit-values-into-identity-columns)).

## <a name="attaching-existing-entities"></a>Присоединение существующих сущностей

### <a name="explicit-key-values"></a>Явные значения ключа

Сущности, возвращаемые из запросов, прописываются в `Unchanged` состоянии. `Unchanged`Состояние означает, что сущность не была изменена с момента запроса. Отсоединенная сущность, которая, возможно, возвращенная веб-клиентом в запросе HTTP, может быть переведена в это состояние с помощью <xref:Microsoft.EntityFrameworkCore.DbContext.Attach%2A?displayProperty=nameWithType> , <xref:Microsoft.EntityFrameworkCore.DbContext.AttachRange%2A?displayProperty=nameWithType> или эквивалентных методов в <xref:Microsoft.EntityFrameworkCore.DbSet%601> . Например, чтобы начать отслеживание существующего блога, выполните следующие действия.

<!--
            context.Attach(
                new Blog
                {
                    Id = 1,
                    Name = ".NET Blog",
                });
-->
[!code-csharp[Attaching_existing_entities_1](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysSamples.cs?name=Attaching_existing_entities_1)]

> [!NOTE]
> В примерах здесь явно создаются сущности с `new` целью простоты. Обычно экземпляры сущностей поступают из другого источника, например, десериализованы из клиента или создаются из данных HTTP POST.

Проверка [представления отладки отслеживания изменений](xref:core/change-tracking/debug-views) после этого вызова показывает, что сущность будет отслеживаться в `Unchanged` состоянии:

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Posts: []
```

Точно так же `Add` , как `Attach` факт, устанавливает весь граф подключенных сущностей в `Unchanged` состояние. Например, чтобы присоединить существующий блог и связать его с существующими записями:

<!--
            context.Attach(
                new Blog
                {
                    Id = 1,
                    Name = ".NET Blog",
                    Posts =
                    {
                        new Post
                        {
                            Id = 1,
                            Title = "Announcing the Release of EF Core 5.0",
                            Content = "Announcing the release of EF Core 5.0, a full featured cross-platform..."
                        },
                        new Post
                        {
                            Id = 2,
                            Title = "Announcing F# 5",
                            Content = "F# 5 is the latest version of F#, the functional programming language..."
                        }
                    }
                });
-->
[!code-csharp[Attaching_existing_entities_2](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysSamples.cs?name=Attaching_existing_entities_2)]

Теперь контекст отслеживает все эти сущности следующим образом `Unchanged` :

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}]
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
Post {Id: 2} Unchanged
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: {Id: 1}
```

Вызов метода SaveChanges на этом этапе не будет действовать. Все сущности помечены как `Unchanged` , поэтому в базе данных нет никаких обновлений.

### <a name="generated-key-values"></a>Созданные значения ключа

Как упоминалось выше, [Свойства ключа](xref:core/modeling/keys) Integer и GUID настраиваются для использования [автоматически создаваемых значений ключа](xref:core/modeling/generated-properties) по умолчанию. Это имеет существенное преимущество при работе с отключенными сущностями: значение неопределенного ключа указывает, что сущность еще не вставлена в базу данных. Это позволяет средству отслеживания изменений автоматически обнаруживать новые сущности и размещать их в `Added` состоянии. Например, можно присоединить этот график блога и записей:

<!--
            context.Attach(
                new Blog
                {
                    Id = 1,
                    Name = ".NET Blog",
                    Posts =
                    {
                        new Post
                        {
                            Id = 1,
                            Title = "Announcing the Release of EF Core 5.0",
                            Content = "Announcing the release of EF Core 5.0, a full featured cross-platform..."
                        },
                        new Post
                        {
                            Id = 2,
                            Title = "Announcing F# 5",
                            Content = "F# 5 is the latest version of F#, the functional programming language..."
                        },
                        new Post
                        {
                            Title = "Announcing .NET 5.0",
                            Content = ".NET 5.0 includes many enhancements, including single file applications, more..."
                        },
                    }
                });
-->
[!code-csharp[Attaching_existing_entities_3](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/GeneratedKeysSamples.cs?name=Attaching_existing_entities_3)]

В блоге имеется значение ключа 1, указывающее, что он уже существует в базе данных. В двух записях также заданы ключевые значения, но третья — нет. EF Core увидите это значение ключа как 0, значение CLR по умолчанию для целого числа. Это приводит к EF Core пометке новой сущности `Added` вместо `Unchanged` :

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}, {Id: -2147482636}]
Post {Id: -2147482636} Added
  Id: -2147482636 PK Temporary
  BlogId: 1 FK
  Content: '.NET 5.0 includes many enhancements, including single file a...'
  Title: 'Announcing .NET 5.0'
  Blog: {Id: 1}
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
Post {Id: 2} Unchanged
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
```

Вызов метода SaveChanges в этом этапе не выполняет никаких действий с `Unchanged` сущностями, но вставляет новую сущность в базу данных. Например, при использовании SQLite:

```sql
-- Executed DbCommand (0ms) [Parameters=[@p0='1' (DbType = String), @p1='.NET 5.0 includes many enhancements, including single file applications, more...' (Size = 80), @p2='Announcing .NET 5.0' (Size = 19)], CommandType='Text', CommandTimeout='30']
INSERT INTO "Posts" ("BlogId", "Content", "Title")
VALUES (@p0, @p1, @p2);
SELECT "Id"
FROM "Posts"
WHERE changes() = 1 AND "rowid" = last_insert_rowid();
```

Важно отметить, что с помощью созданных значений ключа EF Core может **автоматически отличать новый от существующих сущностей в отключенном графе**. В двух словах, при использовании созданных ключей EF Core всегда будет вставлять сущность, если в этой сущности не задано значение ключа.

## <a name="updating-existing-entities"></a>Обновление существующих сущностей

### <a name="explicit-key-values"></a>Явные значения ключа

<xref:Microsoft.EntityFrameworkCore.DbContext.Update%2A?displayProperty=nameWithType>, <xref:Microsoft.EntityFrameworkCore.DbContext.UpdateRange%2A?displayProperty=nameWithType> и эквивалентные методы <xref:Microsoft.EntityFrameworkCore.DbSet%601> ведут себя точно так же `Attach` , как описанные выше методы, за исключением того, что сущности помещаются в, `Modfied` а не в `Unchanged` состояние. Например, чтобы начать отслеживание существующего блога `Modified` :

<!--
            context.Update(
                new Blog
                {
                    Id = 1,
                    Name = ".NET Blog",
                });
-->
[!code-csharp[Updating_existing_entities_1](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysSamples.cs?name=Updating_existing_entities_1)]

Проверка [представления отладки отслеживания изменений](xref:core/change-tracking/debug-views) после этого вызова показывает, что контекст отслеживает эту сущность в `Modified` состоянии:

```output
Blog {Id: 1} Modified
  Id: 1 PK
  Name: '.NET Blog' Modified
  Posts: []
```

Как и в `Add` `Attach` , `Update` фактически помечает _весь граф_ связанных сущностей как `Modified` . Например, чтобы присоединить существующий блог и связать его с существующими записями как `Modified` :

<!--
            context.Update(
                new Blog
                {
                    Id = 1,
                    Name = ".NET Blog",
                    Posts =
                    {
                        new Post
                        {
                            Id = 1,
                            Title = "Announcing the Release of EF Core 5.0",
                            Content = "Announcing the release of EF Core 5.0, a full featured cross-platform..."
                        },
                        new Post
                        {
                            Id = 2,
                            Title = "Announcing F# 5",
                            Content = "F# 5 is the latest version of F#, the functional programming language..."
                        }
                    }
                });
-->
[!code-csharp[Updating_existing_entities_2](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysSamples.cs?name=Updating_existing_entities_2)]

Теперь контекст отслеживает все эти сущности следующим образом `Modified` :

```output
Blog {Id: 1} Modified
  Id: 1 PK
  Name: '.NET Blog' Modified
  Posts: [{Id: 1}, {Id: 2}]
Post {Id: 1} Modified
  Id: 1 PK
  BlogId: 1 FK Modified Originally <null>
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...' Modified
  Title: 'Announcing the Release of EF Core 5.0' Modified
  Blog: {Id: 1}
Post {Id: 2} Modified
  Id: 2 PK
  BlogId: 1 FK Modified Originally <null>
  Content: 'F# 5 is the latest version of F#, the functional programming...' Modified
  Title: 'Announcing F# 5' Modified
  Blog: {Id: 1}
```

Вызов метода SaveChanges на этом этапе приведет к тому, что обновления будут отправляться в базу данных для всех этих сущностей. Например, при использовании SQLite:

```sql
-- Executed DbCommand (0ms) [Parameters=[@p1='1' (DbType = String), @p0='.NET Blog' (Size = 9)], CommandType='Text', CommandTimeout='30']
UPDATE "Blogs" SET "Name" = @p0
WHERE "Id" = @p1;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p3='1' (DbType = String), @p0='1' (DbType = String), @p1='Announcing the release of EF Core 5.0, a full featured cross-platform...' (Size = 72), @p2='Announcing the Release of EF Core 5.0' (Size = 37)], CommandType='Text', CommandTimeout='30']
UPDATE "Posts" SET "BlogId" = @p0, "Content" = @p1, "Title" = @p2
WHERE "Id" = @p3;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p3='2' (DbType = String), @p0='1' (DbType = String), @p1='F# 5 is the latest version of F#, the functional programming language...' (Size = 72), @p2='Announcing F# 5' (Size = 15)], CommandType='Text', CommandTimeout='30']
UPDATE "Posts" SET "BlogId" = @p0, "Content" = @p1, "Title" = @p2
WHERE "Id" = @p3;
SELECT changes();
```

### <a name="generated-key-values"></a>Созданные значения ключа

Как и в `Attach` случае, созданные значения ключей имеют одинаковое основное преимущество `Update` : значение неопределенного ключа указывает, что сущность является новой и еще не вставлена в базу данных. Как и в `Attach` случае, DbContext позволяет автоматически обнаруживать новые сущности и размещать их в `Added` состоянии. Например, рассмотрим вызов `Update` с помощью этого графа блога и записей:

<!--
            context.Update(
                new Blog
                {
                    Id = 1,
                    Name = ".NET Blog",
                    Posts =
                    {
                        new Post
                        {
                            Id = 1,
                            Title = "Announcing the Release of EF Core 5.0",
                            Content = "Announcing the release of EF Core 5.0, a full featured cross-platform..."
                        },
                        new Post
                        {
                            Id = 2,
                            Title = "Announcing F# 5",
                            Content = "F# 5 is the latest version of F#, the functional programming language..."
                        }
                    }
                });
-->
[!code-csharp[Updating_existing_entities_3](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/GeneratedKeysSamples.cs?name=Updating_existing_entities_3)]

Как и в `Attach` примере, POST со значением ключа не обнаруживается как New и устанавливается в `Added` состояние. Другие сущности помечаются как `Modified` :

```output
Blog {Id: 1} Modified
  Id: 1 PK
  Name: '.NET Blog' Modified
  Posts: [{Id: 1}, {Id: 2}, {Id: -2147482633}]
Post {Id: -2147482633} Added
  Id: -2147482633 PK Temporary
  BlogId: 1 FK
  Content: '.NET 5.0 includes many enhancements, including single file a...'
  Title: 'Announcing .NET 5.0'
  Blog: {Id: 1}
Post {Id: 1} Modified
  Id: 1 PK
  BlogId: 1 FK Modified Originally <null>
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...' Modified
  Title: 'Announcing the Release of EF Core 5.0' Modified
  Blog: {Id: 1}
Post {Id: 2} Modified
  Id: 2 PK
  BlogId: 1 FK Modified Originally <null>
  Content: 'F# 5 is the latest version of F#, the functional programming...' Modified
  Title: 'Announcing F# 5' Modified
  Blog: {Id: 1}
```

Вызов `SaveChanges` в этой точке приведет к тому, что обновления будут отправляться в базу данных для всех существующих сущностей, а новая сущность будет вставлена. Например, при использовании SQLite:

```sql
-- Executed DbCommand (0ms) [Parameters=[@p1='1' (DbType = String), @p0='.NET Blog' (Size = 9)], CommandType='Text', CommandTimeout='30']
UPDATE "Blogs" SET "Name" = @p0
WHERE "Id" = @p1;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p3='1' (DbType = String), @p0='1' (DbType = String), @p1='Announcing the release of EF Core 5.0, a full featured cross-platform...' (Size = 72), @p2='Announcing the Release of EF Core 5.0' (Size = 37)], CommandType='Text', CommandTimeout='30']
UPDATE "Posts" SET "BlogId" = @p0, "Content" = @p1, "Title" = @p2
WHERE "Id" = @p3;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p3='2' (DbType = String), @p0='1' (DbType = String), @p1='F# 5 is the latest version of F#, the functional programming language...' (Size = 72), @p2='Announcing F# 5' (Size = 15)], CommandType='Text', CommandTimeout='30']
UPDATE "Posts" SET "BlogId" = @p0, "Content" = @p1, "Title" = @p2
WHERE "Id" = @p3;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p0='1' (DbType = String), @p1='.NET 5.0 includes many enhancements, including single file applications, more...' (Size = 80), @p2='Announcing .NET 5.0' (Size = 19)], CommandType='Text', CommandTimeout='30']
INSERT INTO "Posts" ("BlogId", "Content", "Title")
VALUES (@p0, @p1, @p2);
SELECT "Id"
FROM "Posts"
WHERE changes() = 1 AND "rowid" = last_insert_rowid();
```

Это очень простой способ создания обновлений и вставок из отключенного графа. Однако это приводит к отправке обновлений или вставок в базу данных для каждого свойства отслеживаемой сущности, даже если некоторые значения свойств могут быть не изменены. Не страх. для многих приложений с небольшими графами это может быть простой и практичный способ создания обновлений. С другой стороны, другие более сложные шаблоны могут привести к более эффективным обновлениям, как описано в разделе [разрешение идентификации в EF Core](xref:core/change-tracking/identity-resolution).

## <a name="deleting-existing-entities"></a>Удаление существующих сущностей

Чтобы сущность, удаляемая методом SaveChanges, была удалена, она должна быть записана в `Deleted` состояние. Сущности обычно помещаются в `Deleted` состояние путем вызова одного из <xref:Microsoft.EntityFrameworkCore.DbContext.Remove%2A?displayProperty=nameWithType> , <xref:Microsoft.EntityFrameworkCore.DbContext.RemoveRange%2A?displayProperty=nameWithType> или эквивалентных методов в <xref:Microsoft.EntityFrameworkCore.DbSet%601> . Например, чтобы пометить существующий объект Post как `Deleted` :

<!--
            context.Remove(
                new Post
                {
                    Id = 2
                });
-->
[!code-csharp[Deleting_existing_entities_1](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysSamples.cs?name=Deleting_existing_entities_1)]

Проверка [представления отладки отслеживания изменений](xref:core/change-tracking/debug-views) после этого вызова показывает, что контекст отслеживает сущность в `Deleted` состоянии:

```output
Post {Id: 2} Deleted
  Id: 2 PK
  BlogId: <null> FK
  Content: <null>
  Title: <null>
  Blog: <null>
```

Эта сущность будет удалена при вызове SaveChanges. Например, при использовании SQLite:

```sql
-- Executed DbCommand (0ms) [Parameters=[@p0='2' (DbType = String)], CommandType='Text', CommandTimeout='30']
DELETE FROM "Posts"
WHERE "Id" = @p0;
SELECT changes();
```

После завершения SaveChanges Удаленная сущность отсоединяется от DbContext, так как она больше не существует в базе данных. Поэтому представление отладки пусто, так как нет отслеживаемых сущностей.

## <a name="deleting-dependentchild-entities"></a>Удаление зависимых или дочерних сущностей

Удаление зависимых и дочерних сущностей из графа является более простым, чем удаление основных и родительских сущностей. Дополнительные сведения см. в следующем разделе и об [изменении внешних ключей и переходов](xref:core/change-tracking/relationship-changes) .

Вызывать `Remove` для сущности, созданной с помощью, нередко `new` . Кроме того, в отличие от `Add` `Attach` и `Update` , вызов `Remove` для сущности, которая еще не была зарегистрирована в состоянии или, нераспространена `Unchanged` `Modified` . Вместо этого обычно используется отслеживание отдельной сущности или графа связанных сущностей, а затем вызов `Remove` для сущностей, которые необходимо удалить. Этот граф отслеживания сущностей обычно создается одним из следующих:

1. Выполнение запроса для сущностей
2. Использование `Attach` методов или `Update` на графе отключенных сущностей, как описано в предыдущих разделах.

Например, код в предыдущем разделе более вероятно получает сообщение от клиента, а затем делает что-то подобное:

<!--
            context.Attach(post);
            context.Remove(post);
-->
[!code-csharp[Deleting_dependent_child_entities_1](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysSamples.cs?name=Deleting_dependent_child_entities_1)]

Это ведет себя точно так же, как в предыдущем примере, поскольку вызов `Remove` для неотслеживающей сущности приводит к тому, что он сначала присоединяется, а затем помечается как `Deleted` .

В более реалистичных примерах сначала присоединяется граф сущностей, а затем некоторые из этих сущностей помечаются как удаленные. Например:

<!--
            // Attach a blog and associated posts
            context.Attach(blog);

            // Mark one post as Deleted
            context.Remove(blog.Posts[1]);
-->
[!code-csharp[Deleting_dependent_child_entities_2](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysSamples.cs?name=Deleting_dependent_child_entities_2)]

Все сущности помечены как `Unchanged` , за исключением того, в котором `Remove` был вызван:

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}]
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

Эта сущность будет удалена при вызове SaveChanges. Например, при использовании SQLite:

```sql
-- Executed DbCommand (0ms) [Parameters=[@p0='2' (DbType = String)], CommandType='Text', CommandTimeout='30']
DELETE FROM "Posts"
WHERE "Id" = @p0;
SELECT changes();
```

После завершения SaveChanges Удаленная сущность отсоединяется от DbContext, так как она больше не существует в базе данных. Другие сущности остаются в `Unchanged` состоянии:

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Posts: [{Id: 1}]
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
```

## <a name="deleting-principalparent-entities"></a>Удаление субъектов или родительских сущностей

Каждое отношение, соединяющее два типа сущностей, имеет основной или родительский элемент, а также зависимый или дочерний элемент. Зависимая или Дочерняя сущность — это свойство внешнего ключа. В связи «один ко многим» основной или родительский элемент находится на стороне «один», а зависимый/дочерний элемент находится на стороне «многие». Дополнительные сведения см. в разделе [связи](xref:core/modeling/relationships) .

В предыдущих примерах мы выполняли удаление записи, которая является зависимой или дочерней сущностью в блоге — публикует связь «один ко многим». Это относительно простая задача, так как удаление зависимой или дочерней сущности не влияет на другие сущности. С другой стороны, удаление основной или родительской сущности также должно повлиять на все зависимые и дочерние сущности. В противном случае значение внешнего ключа будет ссылаться на значение первичного ключа, которое больше не существует. Это недопустимое состояние модели, которое приводит к ошибке ссылочного ограничения в большинстве баз данных.

Это недопустимое состояние модели может быть обработано двумя способами:

1. Установка значения FK в значение null. Это означает, что зависимые и дочерние элементы больше не связаны ни с одним участником/родителем. Это значение по умолчанию для необязательных связей, в которых внешний ключ должен допускать значение null. Установка FK в NULL недопустима для обязательных связей, где внешний ключ обычно не допускает значения NULL.
2. Удаление зависимых объектов и дочерних элементов. Это значение по умолчанию для обязательных связей, которое также допустимо для необязательных связей.

Подробные сведения об отслеживании изменений и связях см. в разделе [изменение внешних ключей и переходов](xref:core/change-tracking/relationship-changes) .

### <a name="optional-relationships"></a>Необязательные связи

`Post.BlogId`Свойство внешнего ключа допускает значение NULL в модели, которую мы использовали. Это означает, что связь является необязательной, поэтому поведением по умолчанию EF Core является установка `BlogId` свойств внешнего ключа со значением NULL при удалении блога. Например:

<!--
            // Attach a blog and associated posts
            context.Attach(blog);

            // Mark one post as Deleted
            context.Remove(blog.Posts[1]);
-->
[!code-csharp[Deleting_principal_parent_entities_1](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysSamples.cs?name=Deleting_principal_parent_entities_1)]

Проверка [представления отладки отслеживания изменений](xref:core/change-tracking/debug-views) после вызова `Remove` показывает, что, как и ожидалось, блог теперь помечен как `Deleted` :

```output
Blog {Id: 1} Deleted
  Id: 1 PK
  Name: '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}]
Post {Id: 1} Modified
  Id: 1 PK
  BlogId: <null> FK Modified Originally 1
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: <null>
Post {Id: 2} Modified
  Id: 2 PK
  BlogId: <null> FK Modified Originally 1
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: <null>
```

Более интересно, что все связанные записи теперь помечены как `Modified` . Это связано с тем, что свойство внешнего ключа в каждой сущности имеет значение null. Вызов метода SaveChanges обновляет значение внешнего ключа для каждой записи в базе данных до значения NULL, а затем удаляет блог:

```sql
-- Executed DbCommand (0ms) [Parameters=[@p1='1' (DbType = String), @p0=NULL], CommandType='Text', CommandTimeout='30']
UPDATE "Posts" SET "BlogId" = @p0
WHERE "Id" = @p1;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p1='2' (DbType = String), @p0=NULL], CommandType='Text', CommandTimeout='30']
UPDATE "Posts" SET "BlogId" = @p0
WHERE "Id" = @p1;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p2='1' (DbType = String)], CommandType='Text', CommandTimeout='30']
DELETE FROM "Blogs"
WHERE "Id" = @p2;
SELECT changes();
```

После завершения SaveChanges Удаленная сущность отсоединяется от DbContext, так как она больше не существует в базе данных. Другие сущности теперь помечены `Unchanged` значениями внешнего ключа NULL, которые соответствуют состоянию базы данных:

```output
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: <null> FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: <null>
Post {Id: 2} Unchanged
  Id: 2 PK
  BlogId: <null> FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: <null>
```

### <a name="required-relationships"></a>Обязательные связи

Если `Post.BlogId` свойство внешнего ключа не допускает значения NULL, то связь между блогами и записями становится "обязательной". В этом случае EF Core будет по умолчанию удалять зависимые и дочерние сущности при удалении основного или родительского объекта. Например, при удалении блога со связанными записями, как в предыдущем примере:

<!--
            // Attach a blog and associated posts
            context.Attach(blog);

            // Mark one post as Deleted
            context.Remove(blog.Posts[1]);
-->
[!code-csharp[Deleting_principal_parent_entities_1](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/ExplicitKeysRequiredSamples.cs?name=Deleting_principal_parent_entities_1)]

Проверка [представления отладки отслеживания изменений](xref:core/change-tracking/debug-views) после вызова `Remove` показывает, что, как и ожидалось, блог снова помечен как `Deleted` :

```output
Blog {Id: 1} Deleted
  Id: 1 PK
  Name: '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}]
Post {Id: 1} Deleted
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

Более интересно в этом случае, так как все связанные записи также помечены как `Deleted` . Вызов метода SaveChanges приводит к удалению блога и всех связанных с ним записей из базы данных:

```sql
-- Executed DbCommand (0ms) [Parameters=[@p0='1' (DbType = String)], CommandType='Text', CommandTimeout='30']
DELETE FROM "Posts"
WHERE "Id" = @p0;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p0='2' (DbType = String)], CommandType='Text', CommandTimeout='30']
DELETE FROM "Posts"
WHERE "Id" = @p0;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p1='1' (DbType = String)], CommandType='Text', CommandTimeout='30']
DELETE FROM "Blogs"
WHERE "Id" = @p1;
```

После завершения SaveChanges все удаленные сущности отсоединяются от DbContext, так как они больше не существуют в базе данных. Поэтому выходные данные представления отладка не заполнены.

> [!NOTE]
> В этом документе только поверхность работы с связями в EF Core. Дополнительные сведения об обновлении и удалении зависимых и дочерних сущностей при вызове SaveChanges см. в разделе [связи](xref:core/modeling/relationships) для получения дополнительных сведений о связях моделирования и [изменении внешних ключей и переходов](xref:core/change-tracking/relationship-changes) .

## <a name="custom-tracking-with-trackgraph"></a>Настраиваемое отслеживание с помощью Траккграф

<xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.TrackGraph%2A?displayProperty=nameWithType> работает так же `Add` , как `Attach` и, `Update` за исключением того, что он создает обратный вызов для каждого экземпляра сущности перед его отслеживанием. Это позволяет использовать пользовательскую логику при определении способа трассировки отдельных сущностей в графе.

Например, рассмотрим правило, которое EF Core использует при отслеживании сущностей с созданными значениями ключа: Если значение ключа равно нулю, то сущность является новой и должна быть вставлена. Добавим это правило, чтобы сказать, что значение ключа отрицательное, сущность следует удалить. Это позволяет нам изменить значения первичного ключа в сущностях отключенного графа, чтобы пометить удаленные сущности:

<!--
            blog.Posts.Add(
                new Post
                {
                    Title = "Announcing .NET 5.0",
                    Content = ".NET 5.0 includes many enhancements, including single file applications, more..."
                }
            );

            var toDelete = blog.Posts.Single(e => e.Title == "Announcing F# 5");
            toDelete.Id = -toDelete.Id;
-->
[!code-csharp[Custom_tracking_with_TrackGraph_1a](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/GeneratedKeysSamples.cs?name=Custom_tracking_with_TrackGraph_1a)]

После этого отключенный граф можно будет отслеживанию с помощью Траккграф:

<!--
        public static void UpdateBlog(Blog blog)
        {
            using var context = new BlogsContext();

            context.ChangeTracker.TrackGraph(
                blog, node =>
                    {
                        var propertyEntry = node.Entry.Property("Id");
                        var keyValue = (int)propertyEntry.CurrentValue;

                        if (keyValue == 0)
                        {
                            node.Entry.State = EntityState.Added;
                        }
                        else if (keyValue < 0)
                        {
                            propertyEntry.CurrentValue = -keyValue;
                            node.Entry.State = EntityState.Deleted;
                        }
                        else
                        {
                            node.Entry.State = EntityState.Modified;
                        }

                        Console.WriteLine($"Tracking {node.Entry.Metadata.DisplayName()} with key value {keyValue} as {node.Entry.State}");

                    });

            context.SaveChanges();
        }
-->
[!code-csharp[Custom_tracking_with_TrackGraph_1b](../../../samples/core/ChangeTracking/ChangeTrackingInEFCore/GeneratedKeysSamples.cs?name=Custom_tracking_with_TrackGraph_1b)]

Для каждой сущности в графе приведенный выше код проверяет значение первичного ключа _перед отслеживанием сущности_. Для нестандартных (нулевых) значений ключа код выполняет действия, выполняемые EF Core. То есть если ключ не задан, сущность помечается как `Added` . Если ключ задан и значение не отрицательное, то сущность помечается как `Modified` . Однако при обнаружении отрицательного значения ключа восстанавливается его вещественное неотрицательное значение, а сущность — как `Deleted` .

Результат выполнения этого кода:

```output
Tracking Blog with key value 1 as Modified
Tracking Post with key value 1 as Modified
Tracking Post with key value -2 as Deleted
Tracking Post with key value 0 as Added
```

> [!NOTE]
> Для простоты в этом коде предполагается, что каждая сущность имеет целочисленное свойство первичного ключа с именем `Id` . Это можно кодифицированные в абстрактный базовый класс или интерфейс. Кроме того, свойство или свойства первичного ключа можно получить из <xref:Microsoft.EntityFrameworkCore.Metadata.IEntityType> метаданных таким образом, чтобы этот код работал с любым типом сущности.

Траккграф имеет две перегрузки. В простой перегрузке, используемой выше, EF Core определяет, когда следует прерывать обход графа. В частности, он останавливает посещение новых связанных сущностей из заданной сущности, когда эта сущность уже отслеживается или если обратный вызов не начинает отслеживание сущности.

Расширенная перегрузка, <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.TrackGraph%60%601(System.Object,%60%600,System.Func{Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntryGraphNode{%60%600},System.Boolean})?displayProperty=nameWithType> , имеет обратный вызов, возвращающий логическое значение. Если обратный вызов возвращает значение false, обход графа останавливается, в противном случае — продолжение. Необходимо соблюдать осторожность, чтобы избежать бесконечных циклов при использовании этой перегрузки.

Расширенная перегрузка также позволяет передавать состояние в Траккграф, а это состояние затем передается каждому обратному вызову.
