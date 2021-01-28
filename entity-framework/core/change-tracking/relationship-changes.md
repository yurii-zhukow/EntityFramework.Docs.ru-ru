---
title: Изменение внешних ключей и переходов — EF Core
description: Изменение связей между сущностями путем манипулирования внешними ключами и переходами
author: ajcvickers
ms.date: 12/30/2020
uid: core/change-tracking/relationship-changes
ms.openlocfilehash: b1ebe77ed29291beeef3708b603db026c38bbbec
ms.sourcegitcommit: 7700840119b1639275f3b64836e7abb59103f2e7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2021
ms.locfileid: "98983616"
---
# <a name="changing-foreign-keys-and-navigations"></a>Изменение внешних ключей и переходов

## <a name="overview-of-foreign-keys-and-navigations"></a>Общие сведения о внешних ключах и переходах

Связи в модели Entity Framework Core (EF Core) представлены с помощью внешних ключей (внешние ключи). FK состоит из одного или нескольких свойств в зависимой или дочерней сущности в связи. Эта зависимая или Дочерняя сущность связана с заданной основной или родительской сущностью, если значения свойств внешнего ключа в зависимом или дочернем элементе соответствуют значениям свойств альтернативного или первичного ключа (PK) основного или родительского ключей.

Внешние ключи — это хороший способ хранения связей и управления ими в базе данных, но они не очень удобны при работе с несколькими связанными сущностями в коде приложения. Таким образом, большинство моделей EF Core также являются переходами по слоям в представлении FK. Переходы формы C#/.NET ссылки между экземплярами сущности, отражающими ассоциации, найденные соответствующими значениями внешних ключей и первичными или альтернативными значениями ключа.

Переходы можно использовать на обеих сторонах связи, только на одной стороне, а не на всех, при этом остается только свойство FK. Свойство FK можно скрыть, сделав его [свойством Shadow](xref:core/modeling/shadow-properties). Дополнительные сведения о связях между моделированием см. в разделе [связи](xref:core/modeling/relationships) .

> [!TIP]
> В этом документе предполагается, что состояния сущностей и основы отслеживания изменений EF Core понятны. Дополнительные сведения по этим темам см. [в разделе Отслеживание изменений в EF Core](xref:core/change-tracking/index) .

> [!TIP]
> Вы можете запустить и отладить весь код, используемый в этой документации, [скачав пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/ChangeTracking/ChangingFKsAndNavigations).

### <a name="example-model"></a>Пример модели

Следующая модель содержит четыре типа сущностей со связями между ними. Комментарии в коде указывают, какие свойства являются внешними ключами, первичными ключами и навигациями.

<!--
public class Blog
{
    public int Id { get; set; } // Primary key
    public string Name { get; set; }

    public IList<Post> Posts { get; } = new List<Post>(); // Collection navigation
    public BlogAssets Assets { get; set; } // Reference navigation
}

public class BlogAssets
{
    public int Id { get; set; } // Primary key
    public byte[] Banner { get; set; }

    public int BlogId { get; set; } // Foreign key
    public Blog Blog { get; set; } // Reference navigation
}

public class Post
{
    public int Id { get; set; } // Primary key
    public string Title { get; set; }
    public string Content { get; set; }

    public int? BlogId { get; set; } // Foreign key
    public Blog Blog { get; set; } // Reference navigation

    public IList<Tag> Tags { get; } = new List<Tag>(); // Skip collection navigation
}

public class Tag
{
    public int Id { get; set; } // Primary key
    public string Text { get; set; }

    public IList<Post> Posts { get; } = new List<Post>(); // Skip collection navigation
}
-->
[!code-csharp[Model](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Model)]

В этой модели используются следующие три связи:

- Каждый блог может иметь много записей («один ко многим»):
  - `Blog` является участником или родителем.
  - `Post` — Это зависимый/дочерний элемент. Он содержит свойство FK `Post.BlogId` , значение которого должно совпадать со `Blog.Id` значением PK соответствующего блога.
  - `Post.Blog` — это навигационная ссылка из записи в связанный блог. `Post.Blog` является обратной навигацией для `Blog.Posts` .
  - `Blog.Posts` — Это Навигация коллекции из блога со всеми связанными записями. `Blog.Posts` является обратной навигацией для `Post.Blog` .
- Каждый блог может иметь один ресурс (один к одному):
  - `Blog` является участником или родителем.
  - `BlogAssets` — Это зависимый/дочерний элемент. Он содержит свойство FK `BlogAssets.BlogId` , значение которого должно совпадать со `Blog.Id` значением PK соответствующего блога.
  - `BlogAssets.Blog` — Это ссылка на навигацию из ресурсов в связанный блог. `BlogAssets.Blog` является обратной навигацией для `Blog.Assets` .
  - `Blog.Assets` — это навигационная ссылка из блога на связанные ресурсы. `Blog.Assets` является обратной навигацией для `BlogAssets.Blog` .
- Каждая запись может иметь много тегов, и каждый тег может иметь много записей («многие ко многим»):
  - Связи «многие ко многим» — это более детальный уровень связи между 2 1 и многими. Связи «многие ко многим» рассматриваются далее в этом документе.
  - `Post.Tags` — Это Навигация по коллекции от записи до всех связанных тегов. `Post.Tags` является обратной навигацией для `Tag.Posts` .
  - `Tag.Posts` — Это Навигация в коллекции от тега ко всем связанным записям. `Tag.Posts` является обратной навигацией для `Post.Tags` .

Дополнительные сведения о том, как моделировать и настраивать связи, см. в разделе [связи](xref:core/modeling/relationships) .

## <a name="relationship-fixup"></a>Исправление связи

EF Core поддерживает переходы по значениям внешних ключей и наоборот. То есть, если значение внешнего ключа изменяется таким образом, что теперь оно ссылается на другую сущность или родительский объект, переходы обновляются в соответствии с этим изменением. Аналогично, при изменении навигации значения внешних ключей участвующих сущностей обновляются в соответствии с этим изменением. Это называется "исправлением связи".

### <a name="fixup-by-query"></a>Адресная привязка по запросу

Начальная адресная привязка происходит при запросе сущностей из базы данных. База данных содержит только значения внешнего ключа, поэтому, когда EF Core создает экземпляр сущности из базы данных, он использует значения внешнего ключа для установки ссылок на навигацию и добавления сущностей в области навигации по коллекции по мере необходимости. Например, рассмотрим запрос блогов и связанных с ним сообщений и ресурсов.

<!--
        using var context = new BlogsContext();

        var blogs = context.Blogs
            .Include(e => e.Posts)
            .Include(e => e.Assets)
            .ToList();

        Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Relationship_fixup_1](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Relationship_fixup_1)]

Для каждого блога EF Core сначала создаст `Blog` экземпляр. Затем, когда каждая запись загружается из базы данных, `Post.Blog` для нее устанавливается ссылка на соответствующий блог. Аналогичным образом запись добавляется в `Blog.Posts` навигацию по коллекции. То же самое происходит с `BlogAssets` , за исключением того, что в этом случае переходы являются ссылками. `Blog.Assets`Навигация настраивается так, чтобы указывать на экземпляр Assets, а `BlogAsserts.Blog` Навигация установлена так, чтобы указывать на экземпляр блога.

[Просмотр представления отладки "средство записи изменений](xref:core/change-tracking/debug-views) " после того, как этот запрос показывает два блога, каждый с одним активом и двумя отслеживаемыми записями:

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Assets: {Id: 1}
  Posts: [{Id: 1}, {Id: 2}]
Blog {Id: 2} Unchanged
  Id: 2 PK
  Name: 'Visual Studio Blog'
  Assets: {Id: 2}
  Posts: [{Id: 3}, {Id: 4}]
BlogAssets {Id: 1} Unchanged
  Id: 1 PK
  Banner: <null>
  BlogId: 1 FK
  Blog: {Id: 1}
BlogAssets {Id: 2} Unchanged
  Id: 2 PK
  Banner: <null>
  BlogId: 2 FK
  Blog: {Id: 2}
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
  Tags: []
Post {Id: 2} Unchanged
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: {Id: 1}
  Tags: []
Post {Id: 3} Unchanged
  Id: 3 PK
  BlogId: 2 FK
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: {Id: 2}
  Tags: []
Post {Id: 4} Unchanged
  Id: 4 PK
  BlogId: 2 FK
  Content: 'Examine when database queries were executed and measure how ...'
  Title: 'Database Profiling with Visual Studio'
  Blog: {Id: 2}
  Tags: []
```

В представлении Отладка отображаются ключевые значения и переходы. Переходы отображаются с использованием значений первичного ключа связанных сущностей. Например, `Posts: [{Id: 1}, {Id: 2}]` в приведенном выше выводе указывает, что `Blog.Posts` Навигация по коллекциям содержит две связанные записи с первичными ключами 1 и 2 соответственно. Аналогичным образом для каждой записи, связанной с первым блогом, `Blog: {Id: 1}` строка указывает на то, что `Post.Blog` Переход ссылается на блог с первичным ключом 1.

### <a name="fixup-to-locally-tracked-entities"></a>Исправление для локально записанных сущностей

Исправление связи также происходит между сущностями, возвращаемыми запросом отслеживания, и сущностями, которые уже отслеживаются DbContext. Например, можно выполнить три отдельных запроса для блогов, записей и ресурсов:

<!--
        using var context = new BlogsContext();

        var blogs = context.Blogs.ToList();
        Console.WriteLine(context.ChangeTracker.DebugView.LongView);

        var assets = context.Assets.ToList();
        Console.WriteLine(context.ChangeTracker.DebugView.LongView);

        var posts = context.Posts.ToList();
        Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Relationship_fixup_2](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Relationship_fixup_2)]
Еще раз взгляните на представления отладки, после того как будет записан первый запрос только двух блогов:

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Assets: <null>
  Posts: []
Blog {Id: 2} Unchanged
  Id: 2 PK
  Name: 'Visual Studio Blog'
  Assets: <null>
  Posts: []
```

`Blog.Assets`Переходы по ссылке имеют значение null, а `Blog.Posts` навигационные коллекции пусты, так как в данный момент в контексте не отслеживается ни одна из связанных сущностей.

После второго запроса `Blogs.Assets` были исправлены навигационные ссылки, указывающие на новые записанные `BlogAsset` экземпляры. Аналогичным образом, `BlogAssets.Blog` навигационные ссылки указывают на соответствующий уже записанный `Blog` экземпляр.

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Assets: {Id: 1}
  Posts: []
Blog {Id: 2} Unchanged
  Id: 2 PK
  Name: 'Visual Studio Blog'
  Assets: {Id: 2}
  Posts: []
BlogAssets {Id: 1} Unchanged
  Id: 1 PK
  Banner: <null>
  BlogId: 1 FK
  Blog: {Id: 1}
BlogAssets {Id: 2} Unchanged
  Id: 2 PK
  Banner: <null>
  BlogId: 2 FK
  Blog: {Id: 2}
```

Наконец, после третьего запроса `Blog.Posts` навигационные коллекции теперь содержат все связанные записи, а `Post.Blog` ссылки указывают на соответствующий `Blog` экземпляр:

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Assets: {Id: 1}
  Posts: [{Id: 1}, {Id: 2}]
Blog {Id: 2} Unchanged
  Id: 2 PK
  Name: 'Visual Studio Blog'
  Assets: {Id: 2}
  Posts: [{Id: 3}, {Id: 4}]
BlogAssets {Id: 1} Unchanged
  Id: 1 PK
  Banner: <null>
  BlogId: 1 FK
  Blog: {Id: 1}
BlogAssets {Id: 2} Unchanged
  Id: 2 PK
  Banner: <null>
  BlogId: 2 FK
  Blog: {Id: 2}
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
  Tags: []
Post {Id: 2} Unchanged
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: {Id: 1}
  Tags: []
Post {Id: 3} Unchanged
  Id: 3 PK
  BlogId: 2 FK
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: {Id: 2}
  Tags: []
Post {Id: 4} Unchanged
  Id: 4 PK
  BlogId: 2 FK
  Content: 'Examine when database queries were executed and measure how ...'
  Title: 'Database Profiling with Visual Studio'
  Blog: {Id: 2}
  Tags: []
```

Это то же самое, что и в исходном единственном запросе, поскольку EF Core исправлены переходы по мере отслеживания сущностей, даже если они поступают из нескольких разных запросов.

> [!NOTE]
> Адресная привязка никогда не приводит к возврату данных из базы данных. Он соединяет только сущности, которые уже возвращены запросом или уже были зарегистрированы DbContext. Сведения об обработке дубликатов при сериализации сущностей см. [в разделе Разрешение удостоверений в EF Core](xref:core/change-tracking/identity-resolution) .

## <a name="changing-relationships-using-navigations"></a>Изменение связей с помощью переходов

Самый простой способ изменить связь между двумя сущностями — управлять навигацией, оставляя EF Core для правильной привязки обратной навигации и значений FK. Как это можно сделать:

- Добавление или удаление сущности из навигации по коллекции.
- Изменение ссылки на навигацию для указания другой сущности или присвоения ей значения NULL.

### <a name="adding-or-removing-from-collection-navigations"></a>Добавление или удаление из навигации по коллекциям

Например, давайте перейдем одну из записей из блога Visual Studio в блог .NET. Для этого необходимо сначала загрузить блоги и записи, а затем переместить запись из коллекции навигации в одном блоге в коллекцию навигации в другом блоге:

<!--
        using var context = new BlogsContext();

        var dotNetBlog = context.Blogs.Include(e => e.Posts).Single(e => e.Name == ".NET Blog");
        var vsBlog = context.Blogs.Include(e => e.Posts).Single(e => e.Name == "Visual Studio Blog");

        Console.WriteLine(context.ChangeTracker.DebugView.LongView);

        var post = vsBlog.Posts.Single(e => e.Title.StartsWith("Disassembly improvements"));
        vsBlog.Posts.Remove(post);
        dotNetBlog.Posts.Add(post);

        context.ChangeTracker.DetectChanges();
        Console.WriteLine(context.ChangeTracker.DebugView.LongView);

        context.SaveChanges();
-->
[!code-csharp[Changing_relationships_using_navigations_1](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Changing_relationships_using_navigations_1)]

> [!TIP]
> Вызов требуется, <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges?displayProperty=nameWithType> так как доступ к представлению отладки не приводит к [автоматическому обнаружению изменений](xref:core/change-tracking/change-detection).

Это представление отладки печатается после выполнения приведенного выше кода:

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Assets: <null>
  Posts: [{Id: 1}, {Id: 2}, {Id: 3}]
Blog {Id: 2} Unchanged
  Id: 2 PK
  Name: 'Visual Studio Blog'
  Assets: <null>
  Posts: [{Id: 4}]
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
  Tags: []
Post {Id: 2} Unchanged
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: {Id: 1}
  Tags: []
Post {Id: 3} Modified
  Id: 3 PK
  BlogId: 1 FK Modified Originally 2
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: {Id: 1}
  Tags: []
Post {Id: 4} Unchanged
  Id: 4 PK
  BlogId: 2 FK
  Content: 'Examine when database queries were executed and measure how ...'
  Title: 'Database Profiling with Visual Studio'
  Blog: {Id: 2}
  Tags: []
```

`Blog.Posts`Теперь в блоге .NET есть три записи ( `Posts: [{Id: 1}, {Id: 2}, {Id: 3}]` ). Аналогичным образом, `Blog.Posts` Навигация в блоге Visual Studio имеет только одну запись ( `Posts: [{Id: 4}]` ). Это должно быть ожидаемым, так как в коде явным образом изменяются эти коллекции.

Более интересно, даже несмотря на то, что код не был явно изменил `Post.Blog` навигацию, он был исправлен до того, как он указывал на блог Visual Studio ( `Blog: {Id: 1}` ). Кроме того, `Post.BlogId` значение внешнего ключа было Обновлено в соответствии со значением первичного ключа в блоге .NET. Это изменение значения FK в затем сохраняется в базе данных при вызове SaveChanges:

```sql
-- Executed DbCommand (0ms) [Parameters=[@p1='3' (DbType = String), @p0='1' (Nullable = true) (DbType = String)], CommandType='Text', CommandTimeout='30']
UPDATE "Posts" SET "BlogId" = @p0
WHERE "Id" = @p1;
SELECT changes();
```

### <a name="changing-reference-navigations"></a>Изменение переходов по ссылке

В предыдущем примере запись была перемещена из одного блога в другой путем управления навигацией по записям в каждом блоге. То же самое можно сделать, вместо этого измените `Post.Blog` навигацию по ссылке, чтобы она указывала на новый блог. Пример:

<!--
        var post = vsBlog.Posts.Single(e => e.Title.StartsWith("Disassembly improvements"));
        post.Blog = dotNetBlog;
-->
[!code-csharp[Changing_relationships_using_navigations_2](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Changing_relationships_using_navigations_2)]

Представление отладки после этого изменения будет _точно таким же_ , как и в предыдущем примере. Это связано с тем, что EF Core обнаружил изменение ссылочной навигации, а затем предисправил навигацию по коллекции и значение FK для сопоставления.

## <a name="changing-relationships-using-foreign-key-values"></a>Изменение связей с использованием значений внешнего ключа

В предыдущем разделе связи были изменены с помощью переходов, при этом значения внешних ключей обновляются автоматически. Это рекомендуемый способ управления связями в EF Core. Однако можно также управлять значениями FK напрямую. Например, можно переместить запись из одного блога в другой, изменив `Post.BlogId` значение внешнего ключа:

<!--
        var post = vsBlog.Posts.Single(e => e.Title.StartsWith("Disassembly improvements"));
        post.BlogId = dotNetBlog.Id;
-->
[!code-csharp[Changing_relationships_using_foreign_key_values_1](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Changing_relationships_using_foreign_key_values_1)]

Обратите внимание, что это очень похоже на изменение навигации по ссылке, как показано в предыдущем примере.

Представление отладки после этого изменения опять _же,_ как и в случае двух предыдущих примеров. Это связано с тем, что EF Core обнаружил изменение значения FK, а затем исправил ссылки и навигацию коллекции для сопоставления.

> [!TIP]
> Не следует писать код для обработки всех переходов и значений FK при каждом изменении связи. Такой код более сложен и должен обеспечивать единообразное изменение внешних ключей и переходов во всех случаях. Если возможно, просто обработайте одну навигацию или, возможно, и навигацию. При необходимости просто измените значения FK. Избегайте манипулирования как навигацией, так и значениями FK.

## <a name="fixup-for-added-or-deleted-entities"></a>Исправление для добавленных или удаленных сущностей

### <a name="adding-to-a-collection-navigation"></a>Добавление в навигацию по коллекциям

EF Core выполняет следующие действия, когда [обнаруживает](xref:core/change-tracking/change-detection) , что в навигацию коллекции добавлена новая зависимая или Дочерняя сущность:

- Если сущность не будет отслеживанием, она будет отслеживанием. (Сущность обычно находится в `Added` состоянии. Однако если для типа сущности настроено использование созданных ключей и задано значение первичного ключа, то сущность будет отслеживанием в `Unchanged` состоянии.)
- Если сущность связана с другим участником или родителем, то эта связь является серьезной.
- Сущность становится связанной с участником/родителем, которому принадлежит Навигация по коллекции.
- Переходы и значения внешних ключей фиксируются для всех участвующих сущностей.

На основе этого мы видим, что для перемещения записи из одного блога в другой нам не нужно удалять его из старой структуры коллекции, прежде чем добавлять в новый. Поэтому код из приведенного выше примера можно изменить с:

<!--
        var post = vsBlog.Posts.Single(e => e.Title.StartsWith("Disassembly improvements"));
        vsBlog.Posts.Remove(post);
        dotNetBlog.Posts.Add(post);
-->
[!code-csharp[Fixup_for_added_or_deleted_entities_1](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Fixup_for_added_or_deleted_entities_1)]

на:

<!--
        var post = vsBlog.Posts.Single(e => e.Title.StartsWith("Disassembly improvements"));
        dotNetBlog.Posts.Add(post);
-->
[!code-csharp[Fixup_for_added_or_deleted_entities_2](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Fixup_for_added_or_deleted_entities_2)]

EF Core видит, что запись была добавлена в новый блог, и автоматически удаляет ее из коллекции в первом блоге.

### <a name="removing-from-a-collection-navigation"></a>Удаление из структуры навигации коллекции

Удаление зависимой или дочерней сущности из структуры навигации субъекта-родителя или родительского объекта приводит к появлению серьезности отношения к этому участнику или родительскому объекту. Дальнейшие действия зависят от того, является ли связь необязательной или необязательной.

#### <a name="optional-relationships"></a>Необязательные связи

По умолчанию для необязательных связей значение внешнего ключа устанавливается равным null. Это означает, что зависимый/дочерний элемент больше не связан _ни с одним_ участником/родителем. Например, можно загрузить блог и записи, а затем удалить одну из записей из `Blog.Posts` навигации по коллекции:

<!--
        var post = dotNetBlog.Posts.Single(e => e.Title == "Announcing F# 5");
        dotNetBlog.Posts.Remove(post);
-->
[!code-csharp[Fixup_for_added_or_deleted_entities_3](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Fixup_for_added_or_deleted_entities_3)]

[Просмотрев представление Отладка отслеживания изменений](xref:core/change-tracking/debug-views) после этого изменения показывает, что:

- `Post.BlogId`Для FK задано значение null ( `BlogId: <null> FK Modified Originally 1` )
- `Post.Blog`Для навигации по ссылке задано значение null ( `Blog: <null>` )
- Запись была удалена из `Blog.Posts` навигации по коллекции ( `Posts: [{Id: 1}]` )

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Assets: <null>
  Posts: [{Id: 1}]
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
  Tags: []
Post {Id: 2} Modified
  Id: 2 PK
  BlogId: <null> FK Modified Originally 1
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: <null>
  Tags: []
```

Обратите внимание, что запись _не_ помечена как `Deleted` . Он помечается как, `Modified` так что при вызове SaveChanges значение FK в базе данных будет равно null.

#### <a name="required-relationships"></a>Обязательные связи

Установка значения FK в NULL не допускается (и обычно невозможна) для требуемых связей. Таким образом, важность необходимой связи означает, что зависимая или Дочерняя сущность должна быть либо повторно родительской, либо удалена из базы данных при вызове метода SaveChanges, чтобы избежать нарушения ссылочного ограничения. Это называется удалением потерянных объектов и является поведением по умолчанию в EF Core для необходимых связей.

Например, измените связь между блогом и записями, которые должны быть обязательными, а затем выполните тот же код, что и в предыдущем примере:

<!--
        var post = dotNetBlog.Posts.Single(e => e.Title == "Announcing F# 5");
        dotNetBlog.Posts.Remove(post);
-->
[!code-csharp[Fixup_for_added_or_deleted_entities_4](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/RequiredRelationshipsSamples.cs?name=Fixup_for_added_or_deleted_entities_4)]

Просмотрев представление Отладка после этого изменения показывает, что:

- Запись была помечена таким образом `Deleted` , что она будет удалена из базы данных при вызове метода SaveChanges.
- `Post.Blog`Для навигации по ссылке задано значение null ( `Blog: <null>` ).
- Запись была удалена из `Blog.Posts` навигации по коллекции ( `Posts: [{Id: 1}]` ).

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Assets: <null>
  Posts: [{Id: 1}]
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
  Tags: []
Post {Id: 2} Deleted
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: <null>
  Tags: []
```

Обратите внимание, что объект `Post.BlogId` остается неизменным, так как для него не может быть задано значение null.

Вызов метода SaveChanges приводит к удалению потерянной записи:

```sql
-- Executed DbCommand (0ms) [Parameters=[@p0='2' (DbType = String)], CommandType='Text', CommandTimeout='30']
DELETE FROM "Posts"
WHERE "Id" = @p0;
SELECT changes();
```

#### <a name="delete-orphans-timing-and-re-parenting"></a>Удаление потерянных значений времени и повторного создания родительских объектов

По умолчанию пометка потерянных объектов как `Deleted` произойдет, как только будет [обнаружено](xref:core/change-tracking/change-detection)изменение связи. Однако этот процесс можно отложить до фактического вызова SaveChanges. Это может быть полезно, чтобы избежать удаления потерянных сущностей, удаленных из одного участника или родителя, но при этом будет выполнено повторное создание родительского объекта с новым участником или родительским объектом перед вызовом метода SaveChanges. <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DeleteOrphansTiming?displayProperty=nameWithType> используется для задания этого времени. Пример:

<!--
        context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;

        var post = vsBlog.Posts.Single(e => e.Title.StartsWith("Disassembly improvements"));
        vsBlog.Posts.Remove(post);

        context.ChangeTracker.DetectChanges();
        Console.WriteLine(context.ChangeTracker.DebugView.LongView);

        dotNetBlog.Posts.Add(post);

        context.ChangeTracker.DetectChanges();
        Console.WriteLine(context.ChangeTracker.DebugView.LongView);

        context.SaveChanges();
-->
[!code-csharp[Fixup_for_added_or_deleted_entities_5](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/RequiredRelationshipsSamples.cs?name=Fixup_for_added_or_deleted_entities_5)]

После удаления записи из первой коллекции объект не помечается как `Deleted` в предыдущем примере. Вместо этого EF Core отслеживает, что связь является серьезной, несмотря на то, что _это обязательная связь_. (Значение FK считается нулевым, EF Core несмотря на то, что оно не может действительно иметь значение null, поскольку тип не допускает значения NULL. Это называется «концептуальным значением NULL».)

```output
Post {Id: 3} Modified
  Id: 3 PK
  BlogId: <null> FK Modified Originally 2
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: <null>
  Tags: []
```

Вызов метода SaveChanges в это время приведет к удалению потерянной записи. Однако, если, как в приведенном выше примере, POST связан с новым блогом перед вызовом SaveChanges, то он будет исправлена соответствующим образом для нового блога и больше не считается потерянным:

```output
Post {Id: 3} Modified
  Id: 3 PK
  BlogId: 1 FK Modified Originally 2
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: {Id: 1}
  Tags: []
```

Метод SaveChanges, вызываемый в этой точке, обновляет запись в базе данных, а не удаляет ее.

Также можно отключить автоматическое удаление потерянных элементов. Это приведет к исключению, если метод SaveChanges вызывается при отслеживании потерянных объектов. Например, следующий код:

<!--
                var dotNetBlog = context.Blogs.Include(e => e.Posts).Single(e => e.Name == ".NET Blog");

                context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.Never;

                var post = dotNetBlog.Posts.Single(e => e.Title == "Announcing F# 5");
                dotNetBlog.Posts.Remove(post);

                context.SaveChanges(); // Throws
-->
[!code-csharp[Fixup_for_added_or_deleted_entities_6](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/RequiredRelationshipsSamples.cs?name=Fixup_for_added_or_deleted_entities_6)]

Вызовет это исключение:

> System. InvalidOperationException: связь между сущностями "Blog" и "Post" со значением ключа "{Блогид: 1}" было разорвано, но связь либо помечена как обязательная, либо неявно является обязательной, поскольку внешний ключ не допускает значения NULL. Если зависимая или Дочерняя сущность должна быть удалена при серьезном отношении необходимой связи, настройте связь для использования каскадных удалений.

Удаление потерянных и каскадных удалений может быть принудительно применено в любое время путем вызова <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.CascadeChanges?displayProperty=nameWithType> . Если присвоить этому параметру значение времени удаления без сохранения, то все `Never` потерянные фрагменты не будут удаляться, пока не будет явно указана EF Core.

### <a name="changing-a-reference-navigation"></a>Изменение навигации по ссылке

Изменение навигации по ссылке для связи «один ко многим» оказывает тот же результат, что и изменение навигации по коллекции на другом конце связи. Настройка навигации по ссылке для зависимого или дочернего элемента на null эквивалентна удалению сущности из структуры навигации по основному или родительскому элементу коллекции. Все изменения в исправлениях и базах данных происходят, как описано в предыдущем разделе, в том числе при необходимости сделать сущность потерянной.

#### <a name="optional-one-to-one-relationships"></a>Необязательные связи "один к одному"

Для связей «один к одному» изменение навигации по ссылке приводит к тому, что все предыдущие связи будут разорваны. Для необязательных связей это означает, что значение FK для ранее связанного зависимого или дочернего элемента установлено в NULL. Пример:

<!--
        using var context = new BlogsContext();

        var dotNetBlog = context.Blogs.Include(e => e.Assets).Single(e => e.Name == ".NET Blog");
        dotNetBlog.Assets = new BlogAssets();

        context.ChangeTracker.DetectChanges();
        Console.WriteLine(context.ChangeTracker.DebugView.LongView);

        context.SaveChanges();
-->
[!code-csharp[Fixup_for_added_or_deleted_entities_7](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Fixup_for_added_or_deleted_entities_7)]

Представление Отладка перед вызовом SaveChanges показывает, что новые ресурсы заменили существующие ресурсы, которые теперь отмечены как `Modified` имеющие значение ключа FK, равное NULL `BlogAssets.BlogId` :

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Assets: {Id: -2147482629}
  Posts: []
BlogAssets {Id: -2147482629} Added
  Id: -2147482629 PK Temporary
  Banner: <null>
  BlogId: 1 FK
  Blog: {Id: 1}
BlogAssets {Id: 1} Modified
  Id: 1 PK
  Banner: <null>
  BlogId: <null> FK Modified Originally 1
  Blog: <null>
```

Это приводит к обновлению и вставке при вызове SaveChanges:

```sql
-- Executed DbCommand (0ms) [Parameters=[@p1='1' (DbType = String), @p0=NULL], CommandType='Text', CommandTimeout='30']
UPDATE "Assets" SET "BlogId" = @p0
WHERE "Id" = @p1;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p2=NULL, @p3='1' (Nullable = true) (DbType = String)], CommandType='Text', CommandTimeout='30']
INSERT INTO "Assets" ("Banner", "BlogId")
VALUES (@p2, @p3);
SELECT "Id"
FROM "Assets"
WHERE changes() = 1 AND "rowid" = last_insert_rowid();
```

#### <a name="required-one-to-one-relationships"></a>Необходимые связи "один к одному"

Выполнение того же кода, что и в предыдущем примере, но на этот раз с требуемой связью "один к одному" показывается, что ранее связанная связь `BlogAssets` теперь помечена как `Deleted` , так как она становится потерянной после того, как новая система примет `BlogAssets` следующий вид:

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Assets: {Id: -2147482639}
  Posts: []
BlogAssets {Id: -2147482639} Added
  Id: -2147482639 PK Temporary
  Banner: <null>
  BlogId: 1 FK
  Blog: {Id: 1}
BlogAssets {Id: 1} Deleted
  Id: 1 PK
  Banner: <null>
  BlogId: 1 FK
  Blog: <null>
```

После этого вызывается удаление и вставка при вызове SaveChanges:

```sql
-- Executed DbCommand (0ms) [Parameters=[@p0='1' (DbType = String)], CommandType='Text', CommandTimeout='30']
DELETE FROM "Assets"
WHERE "Id" = @p0;
SELECT changes();

-- Executed DbCommand (0ms) [Parameters=[@p1=NULL, @p2='1' (DbType = String)], CommandType='Text', CommandTimeout='30']
INSERT INTO "Assets" ("Banner", "BlogId")
VALUES (@p1, @p2);
SELECT "Id"
FROM "Assets"
WHERE changes() = 1 AND "rowid" = last_insert_rowid();
```

Время пометки потерянных элементов как удаленных можно изменить так же, как показано для навигации по коллекциям и имеет те же эффекты.

### <a name="deleting-an-entity"></a>Удаление сущности

#### <a name="optional-relationships"></a>Необязательные связи

Если сущность помечена как `Deleted` , например, путем вызова <xref:Microsoft.EntityFrameworkCore.DbContext.Remove%2A?displayProperty=nameWithType> , то ссылки на удаленную сущность удаляются из переходов других сущностей. Для необязательных связей значения FK в зависимых сущностях устанавливаются в значение null.

Например, давайте пометив блог Visual Studio как `Deleted` :

<!--
        using var context = new BlogsContext();

        var vsBlog = context.Blogs
            .Include(e => e.Posts)
            .Include(e => e.Assets)
            .Single(e => e.Name == "Visual Studio Blog");

        context.Remove(vsBlog);

        Console.WriteLine(context.ChangeTracker.DebugView.LongView);

        context.SaveChanges();
-->
[!code-csharp[Deleting_an_entity_1](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Deleting_an_entity_1)]

[Просмотр представления отладки средства записи изменений](xref:core/change-tracking/debug-views) перед вызовом SaveChanges показывает:

```output
Blog {Id: 2} Deleted
  Id: 2 PK
  Name: 'Visual Studio Blog'
  Assets: {Id: 2}
  Posts: [{Id: 3}, {Id: 4}]
BlogAssets {Id: 2} Modified
  Id: 2 PK
  Banner: <null>
  BlogId: <null> FK Modified Originally 2
  Blog: <null>
Post {Id: 3} Modified
  Id: 3 PK
  BlogId: <null> FK Modified Originally 2
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: <null>
  Tags: []
Post {Id: 4} Modified
  Id: 4 PK
  BlogId: <null> FK Modified Originally 2
  Content: 'Examine when database queries were executed and measure how ...'
  Title: 'Database Profiling with Visual Studio'
  Blog: <null>
  Tags: []
```

Обратите внимание на указанные ниже моменты.

- Блог помечен как `Deleted`.
- Ресурсы, связанные с удаленным блогом, имеют нулевое значение FK ( `BlogId: <null> FK Modified Originally 2` ) и навигацию по ссылке NULL ( `Blog: <null>` ).
- Каждая запись, связанная с удаленным блогом, содержит значение FK со значением NULL ( `BlogId: <null> FK Modified Originally 2` ) и навигацию по ссылке NULL ( `Blog: <null>` ).

#### <a name="required-relationships"></a>Обязательные связи

Поведение адресной привязки для требуемых связей аналогично поведению для необязательных связей, за исключением того, что зависимые/дочерние сущности помечены как, `Deleted` так как они не могут существовать без принципала или родителя и должны быть удалены из базы данных при вызове метода SaveChanges, чтобы избежать исключения ссылочного ограничения. Это называется каскадным удалением и является поведением по умолчанию в EF Core для необходимых связей. Например, выполнение того же кода, что и в предыдущем примере, но при наличии обязательной связи приводит к представлению в следующем представлении отладки перед вызовом SaveChanges:

```output
Blog {Id: 2} Deleted
  Id: 2 PK
  Name: 'Visual Studio Blog'
  Assets: {Id: 2}
  Posts: [{Id: 3}, {Id: 4}]
BlogAssets {Id: 2} Deleted
  Id: 2 PK
  Banner: <null>
  BlogId: 2 FK
  Blog: {Id: 2}
Post {Id: 3} Deleted
  Id: 3 PK
  BlogId: 2 FK
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: {Id: 2}
  Tags: []
Post {Id: 4} Deleted
  Id: 4 PK
  BlogId: 2 FK
  Content: 'Examine when database queries were executed and measure how ...'
  Title: 'Database Profiling with Visual Studio'
  Blog: {Id: 2}
  Tags: []
```

Как и ожидалось, зависимые элементы/потомки теперь помечены как `Deleted` . Однако обратите внимание, что переходы по удаленным сущностям _не_ изменились. Это может показаться странным, но позволяет избежать полного разделения удаленных графов сущностей путем очистки всех переходов. Это значит, что блог, ресурс и записи по-прежнему формируют граф сущностей даже после удаления. Это значительно упрощает отмену удаления графа сущностей, чем было в EF6, где был разбираться граф.

#### <a name="cascade-delete-timing-and-re-parenting"></a>Каскадное удаление и повторное наследование

По умолчанию каскадное удаление происходит сразу после того, как родительский объект или участник помечается как `Deleted` . Это то же самое, что и при удалении потерянных объектов, как описано выше. Как и при удалении потерянных объектов, этот процесс можно отложить до вызова метода SaveChanges или даже полностью отключить, установив <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.CascadeDeleteTiming?displayProperty=nameWithType> соответствующий параметр. Это полезно так же, как и удаление потерянных объектов, в том числе для повторного наследования потомков или зависимых элементов после удаления субъекта-родителя.

Каскадные удаления, а также удаление потерянных объектов могут быть принудительно запущены в любое время путем вызова <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.CascadeChanges?displayProperty=nameWithType> . Если присвоить этому параметру значение времени каскадного удаления, `Never` оно не будет выполняться каскадным удалением, если только EF Core явно не получит указания.

> [!TIP]
> Концепции каскадного удаления и удаления потерянных объектов тесно взаимосвязаны. В обоих случаях при разрыве связи с основной или родительской сущностью ее зависимые и дочерние сущности удаляются. При каскадном удалении это происходит вследствие удаления самой основной или родительской сущности. При удалении потерянных объектов основная или родительская сущность по-прежнему существует, но при этом больше не связана с зависимыми и дочерними сущностями.

## <a name="many-to-many-relationships"></a>Связи "много ко многим".

Связи «многие ко многим» в EF Core реализуются с помощью сущности JOIN. Каждая сторона связи "многие ко многим" связана с этой сущностью объединения со связью "один ко многим". До EF Core 5,0 эта сущность объединения должна быть явно определена и сопоставлена. Начиная с EF Core 5,0, его можно создать неявно и скрыть. Однако в обоих случаях базовое поведение одинаково. Сначала мы рассмотрим базовое поведение, чтобы понять, как работает отслеживание связей "многие ко многим".

### <a name="how-many-to-many-relationships-work"></a>Как работают связи «многие ко многим»

Рассмотрим эту модель EF Core, которая создает связь "многие ко многим" между записями и тегами, используя явно определенный тип сущности Join:

<!--
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int? BlogId { get; set; }
        public Blog Blog { get; set; }

        public IList<PostTag> PostTags { get; } = new List<PostTag>(); // Collection navigation
    }

    public class Tag
    {
        public int Id { get; set; }
        public string Text { get; set; }

        public IList<PostTag> PostTags { get; } = new List<PostTag>(); // Collection navigation
    }

    public class PostTag
    {
        public int PostId { get; set; } // First part of composite PK; FK to Post
        public int TagId { get; set; } // Second part of composite PK; FK to Tag

        public Post Post { get; set; } // Reference navigation
        public Tag Tag { get; set; } // Reference navigation
    }
    -->
[!code-csharp[Model](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntitySamples.cs?name=Model)]

Обратите внимание, что `PostTag` тип сущности Join содержит два свойства внешнего ключа. В этой модели для того, чтобы запись была связана с тегом, должна существовать сущность Посттаг JOIN, где `PostTag.PostId` значение внешнего ключа соответствует `Post.Id` значению первичного ключа и где `PostTag.TagId` значение внешнего ключа соответствует `Tag.Id` значению первичного ключа. Пример:

<!--
            using var context = new BlogsContext();

            var post = context.Posts.Single(e => e.Id == 3);
            var tag = context.Tags.Single(e => e.Id == 1);

            context.Add(new PostTag { PostId = post.Id, TagId = tag.Id });

            Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Many_to_many_relationships_1](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntitySamples.cs?name=Many_to_many_relationships_1)]

[Просмотр представления отладки "средство записи изменений](xref:core/change-tracking/debug-views) " после выполнения этого кода показывает, что запись и тег связаны с новой `PostTag` сущностью Join:

```output
Post {Id: 3} Unchanged
  Id: 3 PK
  BlogId: 2 FK
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: <null>
  PostTags: [{PostId: 3, TagId: 1}]
PostTag {PostId: 3, TagId: 1} Added
  PostId: 3 PK FK
  TagId: 1 PK FK
  Post: {Id: 3}
  Tag: {Id: 1}
Tag {Id: 1} Unchanged
  Id: 1 PK
  Text: '.NET'
  PostTags: [{PostId: 3, TagId: 1}]
```

Обратите внимание, что структуры навигации по коллекции `Post` и были `Tag` исправлены, как и ссылки на `PostTag` . Этими связями можно управлять с помощью переходов вместо значений FK, как и во всех предыдущих примерах. Например, приведенный выше код можно изменить, чтобы добавить связь, задав ссылки для навигации по ссылке в сущности JOIN.

<!--
            context.Add(new PostTag { Post = post, Tag = tag });
-->
[!code-csharp[Many_to_many_relationships_2](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntitySamples.cs?name=Many_to_many_relationships_2)]

Это приводит к тому же изменению для внешние ключи и переходов, как в предыдущем примере.

### <a name="skip-navigations"></a>Пропустить навигацию

> [!NOTE]
> Переходы по пропуску были введены в EF Core 5,0.

Работа с таблицей соединений вручную может быть утомительной. Начиная с EF Core 5,0, связи "многие ко многим" можно манипулировать напрямую с помощью специальных переходов по коллекции, которые пропускают сущность JOIN. Например, в приведенной выше модели можно добавить два перехода с пропуском. один из записей в теги, а другой — от тега к записям:

<!--
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int? BlogId { get; set; }
        public Blog Blog { get; set; }

        public IList<Tag> Tags { get; } = new List<Tag>(); // Skip collection navigation
        public IList<PostTag> PostTags { get; } = new List<PostTag>(); // Collection navigation
    }

    public class Tag
    {
        public int Id { get; set; }
        public string Text { get; set; }

        public IList<Post> Posts { get; } = new List<Post>(); // Skip collection navigation
        public IList<PostTag> PostTags { get; } = new List<PostTag>(); // Collection navigation
    }

    public class PostTag
    {
        public int PostId { get; set; } // First part of composite PK; FK to Post
        public int TagId { get; set; } // Second part of composite PK; FK to Tag

        public Post Post { get; set; } // Reference navigation
        public Tag Tag { get; set; } // Reference navigation
    }
-->
[!code-csharp[Model](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityAndSkipsSamples.cs?name=Model)]

Для этой связи "многие ко многим" требуется следующая конфигурация, чтобы убедиться, что Навигация по пропуску и обычные переходы используются для одной и той же связи "многие ко многим":

<!--
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Post>()
                .HasMany(p => p.Tags)
                .WithMany(p => p.Posts)
                .UsingEntity<PostTag>(
                    j => j.HasOne(t => t.Tag).WithMany(p => p.PostTags),
                    j => j.HasOne(t => t.Post).WithMany(p => p.PostTags));
        }
-->
[!code-csharp[OnModelCreating](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityAndSkipsSamples.cs?name=OnModelCreating)]

Дополнительные сведения о сопоставлении отношений «многие ко многим» см. в разделе [связи](xref:core/modeling/relationships) .

Пропуски переходов пропускаются и ведут себя как обычные навигационные коллекции. Однако способ их работы с внешними ключами отличается. Давайте связав запись с тегом, но на этот раз используя навигацию Skip:

<!--
            using var context = new BlogsContext();

            var post = context.Posts.Single(e => e.Id == 3);
            var tag = context.Tags.Single(e => e.Id == 1);

            post.Tags.Add(tag);

            context.ChangeTracker.DetectChanges();
            Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Many_to_many_relationships_3](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityAndSkipsSamples.cs?name=Many_to_many_relationships_3)]

Обратите внимание, что этот код не использует сущность JOIN. Вместо этого он просто добавляет сущность в коллекцию навигации точно так же, как если бы это была связь «один ко многим». Результирующее представление отладки, по сути, такое же, как и раньше:

```output
Post {Id: 3} Unchanged
  Id: 3 PK
  BlogId: 2 FK
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: <null>
  PostTags: [{PostId: 3, TagId: 1}]
  Tags: [{Id: 1}]
PostTag {PostId: 3, TagId: 1} Added
  PostId: 3 PK FK
  TagId: 1 PK FK
  Post: {Id: 3}
  Tag: {Id: 1}
Tag {Id: 1} Unchanged
  Id: 1 PK
  Text: '.NET'
  PostTags: [{PostId: 3, TagId: 1}]
  Posts: [{Id: 3}]
```

Обратите внимание, что экземпляр `PostTag` сущности Join был создан автоматически с ЗНАЧЕНИЯМИ FK, для которых заданы значения ключа PK тега и POST, которые теперь связаны. Все обычные переходы по ссылке и коллекции были исправлены в соответствии с этими значениями FK. Кроме того, поскольку эта модель содержит переходы с пропуском, они также были исправлены. В частности, несмотря на то, что мы добавили тег к `Post.Tags` навигации Skip, `Tag.Posts` обратная Навигация по другой стороне этой связи также была исправлена, чтобы содержать связанную запись.

Стоит отметить, что базовые связи "многие ко многим" все еще можно манипулировать напрямую, даже если пропускная Навигация пропускается сверху. Например, тег и POST могут быть связаны, как мы делали до представления пропускаемых переходов:

<!--
            context.Add(new PostTag { Post = post, Tag = tag });
-->
[!code-csharp[Many_to_many_relationships_4](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityAndSkipsSamples.cs?name=Many_to_many_relationships_4)]

Или используйте значения FK:

<!--
            context.Add(new PostTag { PostId = post.Id, TagId = tag.Id });
-->
[!code-csharp[Many_to_many_relationships_5](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityAndSkipsSamples.cs?name=Many_to_many_relationships_5)]

Это по-прежнему приведет к правильному исправлению переходов по пропускам, что приведет к тому же результату представления отладки, что и в предыдущем примере.

### <a name="skip-navigations-only"></a>Пропустить только навигацию

В предыдущем разделе мы добавили пропустить навигацию _в дополнение к_ полному определению двух базовых связей «один ко многим». Это полезно для иллюстрации того, что происходит со значениями FK, но часто не требуется. Вместо этого связь «многие ко многим» может быть определена только с помощью _навигации по пропустить_. Таким способом отношение «многие ко многим» определяется в модели в самом верху этого документа. С помощью этой модели можно снова связать запись и тег, добавив запись в `Tag.Posts` навигацию по пропуску (или, как вариант, добавить тег в `Post.Tags` навигацию Skip):

<!--
            using var context = new BlogsContext();

            var post = context.Posts.Single(e => e.Id == 3);
            var tag = context.Tags.Single(e => e.Id == 1);

            post.Tags.Add(tag);

            context.ChangeTracker.DetectChanges();
            Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Many_to_many_relationships_6](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/OptionalRelationshipsSamples.cs?name=Many_to_many_relationships_6)]

Просмотр представления отладки после внесения этого изменения показывает, что EF Core создал экземпляр `Dictionary<string, object>` для представления сущности JOIN. Эта сущность Join содержит как `PostsId` , так и `TagsId` Свойства внешнего ключа, которые были заданы в соответствии со значениями PK, которые связаны с разделом POST и тегом.

```output
Post {Id: 3} Unchanged
  Id: 3 PK
  BlogId: 2 FK
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: <null>
  Tags: [{Id: 1}]
Tag {Id: 1} Unchanged
  Id: 1 PK
  Text: '.NET'
  Posts: [{Id: 3}]
PostTag (Dictionary<string, object>) {PostsId: 3, TagsId: 1} Added
  PostsId: 3 PK FK
  TagsId: 1 PK FK
```

Дополнительные [](xref:core/modeling/relationships) сведения о неявных сущностях Join и использовании `Dictionary<string, object>` типов сущностей см. в разделе связи.

> [!IMPORTANT]
> Тип CLR, используемый для объединения типов сущностей по соглашению, может измениться в будущих выпусках для повышения производительности. Не зависят от типа объединения, `Dictionary<string, object>` если только он не был явно настроен.

### <a name="join-entities-with-payloads"></a>Соединение сущностей с полезными данными

Пока все примеры использовали тип сущности Join (явный или неявный), который содержит только два свойства внешнего ключа, необходимые для связи «многие ко многим». Ни одно из этих значений FK не должно быть явно задано приложением при управлении связями, поскольку их значения берутся из свойств первичного ключа связанных сущностей. Это позволяет EF Core создавать экземпляры сущности JOIN без отсутствующих данных.

#### <a name="payloads-with-generated-values"></a>Полезные данные с созданными значениями

EF Core поддерживает добавление дополнительных свойств к типу сущности JOIN. Это называется предоставлением сущности для объединения полезных данных. Например, добавим `TaggedOn` свойство в `PostTag` сущность Join:

<!--
    public class PostTag
    {
        public int PostId { get; set; } // Foreign key to Post
        public int TagId { get; set; } // Foreign key to Tag

        public DateTime TaggedOn { get; set; } // Payload
    }
-->
[!code-csharp[Model](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityWithPayloadSamples.cs?name=Model)]

Это свойство полезной нагрузки не будет задано, когда EF Core создает экземпляр сущности JOIN. Наиболее распространенный способ решения этой проблемы — использование свойств полезных данных с автоматически формируемыми значениями. Например, `TaggedOn` свойство может быть настроено для использования создаваемой хранилищем метки времени при вставке каждой новой сущности:

<!--
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Post>()
                .HasMany(p => p.Tags)
                .WithMany(p => p.Posts)
                .UsingEntity<PostTag>(
                    j => j.HasOne<Tag>().WithMany(),
                    j => j.HasOne<Post>().WithMany(),
                    j => j.Property(e => e.TaggedOn).HasDefaultValueSql("CURRENT_TIMESTAMP"));
        }
-->
[!code-csharp[OnModelCreating](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityWithPayloadSamples.cs?name=OnModelCreating)]

Теперь запись может быть помечена таким же образом, как и раньше:

<!--
            using var context = new BlogsContext();

            var post = context.Posts.Single(e => e.Id == 3);
            var tag = context.Tags.Single(e => e.Id == 1);

            post.Tags.Add(tag);

            context.SaveChanges();

            Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Many_to_many_relationships_7](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityWithPayloadSamples.cs?name=Many_to_many_relationships_7)]

При [просмотре представления отладки средства записи изменений](xref:core/change-tracking/debug-views) после вызова SaveChanges показывается, что свойство полезной нагрузки установлено соответствующим образом:

```output
Post {Id: 3} Unchanged
  Id: 3 PK
  BlogId: 2 FK
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: <null>
  Tags: [{Id: 1}]
PostTag {PostId: 3, TagId: 1} Unchanged
  PostId: 3 PK FK
  TagId: 1 PK FK
  TaggedOn: '12/29/2020 8:13:21 PM'
Tag {Id: 1} Unchanged
  Id: 1 PK
  Text: '.NET'
  Posts: [{Id: 3}]
```

#### <a name="explicitly-setting-payload-values"></a>Явное задание значений полезных данных

Далее в предыдущем примере мы добавим свойство полезной нагрузки, которое не использует автоматически созданное значение:

<!--
    public class PostTag
    {
        public int PostId { get; set; } // First part of composite PK; FK to Post
        public int TagId { get; set; } // Second part of composite PK; FK to Tag

        public DateTime TaggedOn { get; set; } // Auto-generated payload property
        public string TaggedBy { get; set; } // Not-generated payload property
    }
-->
[!code-csharp[Model](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityWithStringPayloadSamples.cs?name=Model)]

Теперь запись может быть помечена таким же образом, как и раньше, и сущность JOIN будет по-прежнему создана автоматически. Доступ к этой сущности можно получить с помощью одного из механизмов, описанных в статье [доступ к отслеживающим сущностям](xref:core/change-tracking/entity-entries). Например, приведенный ниже код использует <xref:Microsoft.EntityFrameworkCore.DbSet%601.Find%2A?displayProperty=nameWithType> для доступа к экземпляру сущности Join:

<!--
            using var context = new BlogsContext();

            var post = context.Posts.Single(e => e.Id == 3);
            var tag = context.Tags.Single(e => e.Id == 1);

            post.Tags.Add(tag);

            context.ChangeTracker.DetectChanges();

            var joinEntity = context.Set<PostTag>().Find(post.Id, tag.Id);

            joinEntity.TaggedBy = "ajcvickers";

            context.SaveChanges();

            Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Many_to_many_relationships_8](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityWithStringPayloadSamples.cs?name=Many_to_many_relationships_8)]

После того как сущность JOIN будет найдена, ее можно обменять обычным способом — в этом примере — задать `TaggedBy` свойство полезной нагрузки перед вызовом метода SaveChanges.

> [!NOTE]
> Обратите внимание, что <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges?displayProperty=nameWithType> здесь требуется вызов, чтобы предоставить EF Core возможность обнаружить изменение свойства навигации и создать экземпляр сущности Join перед `Find` использованием. Дополнительные сведения см. в разделе [Обнаружение изменений и уведомления](xref:core/change-tracking/change-detection) .

Кроме того, сущность Join можно создать явным образом, чтобы связать запись с тегом. Пример:

<!--
            using var context = new BlogsContext();

            var post = context.Posts.Single(e => e.Id == 3);
            var tag = context.Tags.Single(e => e.Id == 1);

            context.Add(new PostTag()
            {
                PostId = post.Id,
                TagId = tag.Id,
                TaggedBy = "ajcvickers"
            });

            context.SaveChanges();

            Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Many_to_many_relationships_9](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityWithStringPayloadSamples.cs?name=Many_to_many_relationships_9)]

Наконец, еще один способ задать данные полезных данных — это переопределение <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> или использование <xref:Microsoft.EntityFrameworkCore.DbContext.SavingChanges?displayProperty=nameWithType> события для обработки сущностей перед обновлением базы данных. Пример:

<!--
        public override int SaveChanges()
        {
            foreach (var entityEntry in ChangeTracker.Entries<PostTag>())
            {
                if (entityEntry.State == EntityState.Added)
                {
                    entityEntry.Entity.TaggedBy = "ajcvickers";
                }
            }

            return base.SaveChanges();
        }
-->
[!code-csharp[SaveChanges](../../../samples/core/ChangeTracking/ChangingFKsAndNavigations/ExplicitJoinEntityWithStringPayloadSamples.cs?name=SaveChanges)]
