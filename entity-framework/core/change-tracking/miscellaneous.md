---
title: Дополнительные функции Отслеживание изменений — EF Core
description: Различные функции и сценарии, включающие отслеживание изменений EF Core
author: ajcvickers
ms.date: 12/30/2020
uid: core/change-tracking/miscellaneous
ms.openlocfilehash: 9eb3186f4eef300e4824dc86700497444ece4a2c
ms.sourcegitcommit: 704240349e18b6404e5a809f5b7c9d365b152e2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/16/2021
ms.locfileid: "100543423"
---
# <a name="additional-change-tracking-features"></a>Дополнительные функции Отслеживание изменений

В этом документе рассматриваются различные функции и сценарии, включающие отслеживание изменений.

> [!TIP]
> В этом документе предполагается, что состояния сущностей и основы отслеживания изменений EF Core понятны. Дополнительные сведения по этим темам см. [в разделе Отслеживание изменений в EF Core](xref:core/change-tracking/index) .

> [!TIP]
> Вы можете запустить и отладить весь код, используемый в этой документации, [скачав пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/ChangeTracking/AdditionalChangeTrackingFeatures).

## <a name="add-versus-addasync"></a>`Add` и `AddAsync`

Entity Framework Core (EF Core) предоставляет асинхронные методы всякий раз, когда использование этого метода может привести к взаимодействию с базой данных. Кроме того, существуют синхронные методы, позволяющие избежать издержек при использовании баз данных, не поддерживающих высокий уровень производительности асинхронного доступа.

<xref:Microsoft.EntityFrameworkCore.DbContext.Add%2A?displayProperty=nameWithType> и <xref:Microsoft.EntityFrameworkCore.DbSet%601.Add%2A?displayProperty=nameWithType> обычно не обращаются к базе данных, так как эти методы изначально просто начинают отслеживать сущности. Однако некоторые формы создания значений _могут_ получить доступ к базе данных, чтобы создать значение ключа. Единственным генератором значений, который делает это и поставляется с EF Core, является <xref:Microsoft.EntityFrameworkCore.ValueGeneration.HiLoValueGenerator%601> . Использование этого генератора является редким; Он никогда не настраивается по умолчанию. Это означает, что большинству приложений следует использовать `Add` , а не `AddAsync` .

Другие аналогичные методы, такие как `Update` , `Attach` и, `Remove` не имеют асинхронных перегрузок, так как они никогда не создают новые значения ключей и, следовательно, никогда не нуждаются в доступе к базе данных.

## <a name="addrange-updaterange-attachrange-and-removerange"></a>`AddRange`, `UpdateRange`, `AttachRange` и `RemoveRange`

<xref:Microsoft.EntityFrameworkCore.DbSet%601> и <xref:Microsoft.EntityFrameworkCore.DbContext> предоставляют альтернативные версии `Add` , `Update` , `Attach` и `Remove` , которые принимают несколько экземпляров в одном вызове. Эти методы —,, <xref:Microsoft.EntityFrameworkCore.DbSet%601.AddRange%2A> <xref:Microsoft.EntityFrameworkCore.DbSet%601.UpdateRange%2A> <xref:Microsoft.EntityFrameworkCore.DbSet%601.AttachRange%2A> и <xref:Microsoft.EntityFrameworkCore.DbSet%601.RemoveRange%2A> соответственно.

Эти методы предоставляются для удобства. Использование метода Range имеет те же функциональные возможности, что и несколько вызовов эквивалентного метода, не являющегося диапазоном. Между двумя подходами нет значительной разницы в производительности.

> [!NOTE]
> Это отличается от EF6, где `AddRange` и `Add` оба автоматически вызваны `DetectChanges` , но вызов `Add` несколько раз вызывал DetectChanges несколько раз, а не один раз. Это стало `AddRange` более эффективным в EF6. В EF Core ни один из этих методов не вызывается автоматически `DetectChanges` .

## <a name="dbcontext-versus-dbset-methods"></a>Методы DbContext и DbSet

Многие методы, включая `Add` , `Update` , `Attach` и `Remove` , имеют реализации как в, так <xref:Microsoft.EntityFrameworkCore.DbSet%601> и в <xref:Microsoft.EntityFrameworkCore.DbContext> . Эти методы имеют _точно такое же поведение_ для обычных типов сущностей. Это обусловлено тем, что тип CLR сущности сопоставлен с одним и только одним типом сущности в модели EF Core. Таким образом, тип CLR полностью определяет, где сущность помещается в модели, поэтому DbSet может быть определен неявно.

Исключением из этого правила является использование типов сущностей с общим типом, которые были введены в EF Core 5,0, в первую очередь для сущностей объединения «многие ко многим». При использовании типа сущности общего типа необходимо сначала создать DbSet для используемого типа модели EF Core. Такие методы `Add` , как,, `Update` и, `Attach` `Remove` можно использовать в DbSet без какой-либо неоднозначности, в которой используется тип модели EF Core.

Типы сущностей общего типа используются по умолчанию для сущностей JOIN в связях «многие ко многим». Тип сущности совместно используемого типа также может быть явно настроен для использования в связи «многие ко многим». Например, приведенный ниже код настраивается `Dictionary<string, int>` как тип сущности JOIN.

<!--
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder
                .SharedTypeEntity<Dictionary<string, int>>(
                    "PostTag",
                    b =>
                        {
                            b.IndexerProperty<int>("TagId");
                            b.IndexerProperty<int>("PostId");
                        });

            modelBuilder.Entity<Post>()
                .HasMany(p => p.Tags)
                .WithMany(p => p.Posts)
                .UsingEntity<Dictionary<string, int>>(
                    "PostTag",
                    j => j.HasOne<Tag>().WithMany(),
                    j => j.HasOne<Post>().WithMany());
        }
-->
[!code-csharp[OnModelCreating](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/Samples.cs?name=OnModelCreating)]

[Изменение внешних ключей и переходов](xref:core/change-tracking/relationship-changes) показывает, как связать две сущности путем отслеживания нового экземпляра сущности JOIN. Приведенный ниже код делает это для `Dictionary<string, int>` типа сущности общего типа, используемого для сущности JOIN.

<!--
            using var context = new BlogsContext();

            var post = context.Posts.Single(e => e.Id == 3);
            var tag = context.Tags.Single(e => e.Id == 1);

            var joinEntitySet = context.Set<Dictionary<string, int>>("PostTag");
            var joinEntity = new Dictionary<string, int>
            {
                ["PostId"] = post.Id,
                ["TagId"] = tag.Id
            };
            joinEntitySet.Add(joinEntity);

            Console.WriteLine(context.ChangeTracker.DebugView.LongView);

            context.SaveChanges();
-->
[!code-csharp[DbContext_versus_DbSet_methods_1](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/Samples.cs?name=DbContext_versus_DbSet_methods_1)]

Обратите внимание, что <xref:Microsoft.EntityFrameworkCore.DbContext.Set%60%601(System.String)?displayProperty=nameWithType> используется для создания DbSet для `PostTag` типа сущности. Этот DbSet можно затем использовать для вызова `Add` с новым экземпляром сущности JOIN.

> [!IMPORTANT]
> Тип CLR, используемый для объединения типов сущностей по соглашению, может измениться в будущих выпусках для повышения производительности. Не зависят от какого-либо конкретного типа сущности JOIN, если только она не была явно настроена в приведенном `Dictionary<string, int>` выше коде.

## <a name="property-versus-field-access"></a>Сравнение свойств и доступа к полям

Начиная с EF Core 3,0, доступ к свойствам сущности по умолчанию использует резервное поле свойства. Это эффективно и позволяет избежать запуска побочных эффектов при вызове методов получения и задания свойств. Например, вот как отложенная загрузка позволяет избежать срабатывания бесконечных циклов. Дополнительные сведения о настройке резервных полей в модели см. в разделе [резервные поля](xref:core/modeling/backing-field) .

Иногда может быть целесообразно EF Core создавать побочные эффекты при изменении значений свойств. Например, при привязке данных к сущностям задание свойства может создавать уведомления для У.И. что не происходит при непосредственном задании поля. Это можно сделать, изменив параметр <xref:Microsoft.EntityFrameworkCore.PropertyAccessMode> для:

- Все типы сущностей в модели с использованием <xref:Microsoft.EntityFrameworkCore.ModelBuilder.UsePropertyAccessMode%2A?displayProperty=nameWithType>
- Все свойства и переходы определенного типа сущности с помощью <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder%601.UsePropertyAccessMode%2A?displayProperty=nameWithType>
- Конкретное свойство, использующее <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.PropertyBuilder.UsePropertyAccessMode%2A?displayProperty=nameWithType>
- Конкретная Навигация с помощью <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.NavigationBuilder.UsePropertyAccessMode%2A?displayProperty=nameWithType>

Режим доступа к свойству и вызовет `Field` `PreferField` EF Core доступа к значению свойства через его резервное поле. Аналогичным образом, и вызовет `Property` `PreferProperty` EF Core доступа к значению свойства через его метод получения и задания.

Если `Field` `Property` используются значения или, а EF Core не может получить доступ к значению через поле или метод считывания/задания свойства соответственно, EF Core выдаст исключение. Это гарантирует, что EF Core всегда использует доступ к полям и свойствам, если вы считаете это.

С другой стороны, `PreferField` `PreferProperty` режимы и будут возвращаться к использованию свойства или резервного поля соответственно, если невозможно использовать предпочтительный доступ. `PreferField` значение по умолчанию — EF Core 3,0, начиная с. Это означает, что EF Core будет использовать поля каждый раз, когда это возможно, но не завершится ошибкой, если вместо этого доступ к свойству должен осуществляться через метод получения или задания.

`FieldDuringConstruction` и `PreferFieldDuringConstruction` настройте EF Core для использования резервных полей _только при создании экземпляров сущности_. Это позволяет выполнять запросы без побочных эффектов getter и Set, в то время как изменения свойств по EF Core приведут к этим побочным эффектам. `PreferFieldDuringConstruction` использовался по умолчанию до EF Core 3,0.

В следующей таблице приведены различные режимы доступа к свойствам.

| пропертякцессмоде              | Предпочтение | Предпочтение созданию сущностей | Резервирование | Резервное Создание сущностей
|:--------------------------------|------------|------------------------------|----------|---------------------------
| `Field`                         | Поле      | Поле                        | Активизирует исключение   | Активизирует исключение
| `Property`                      | Свойство   | Свойство                     | Активизирует исключение   | Активизирует исключение
| `PreferField`                   | Поле      | Поле                        | Свойство | Свойство
| `PreferProperty`                | Свойство   | Свойство                     | Поле    | Поле
| `FieldDuringConstruction`       | Свойство   | Поле                        | Поле    | Активизирует исключение
| `PreferFieldDuringConstruction` | Свойство   | Поле                        | Поле    | Свойство

## <a name="temporary-values"></a>Временные значения

EF Core создает временные значения ключа при отслеживании новых сущностей, которые будут иметь реальные значения ключа, создаваемые базой данных при вызове метода SaveChanges. Общие сведения об использовании этих временных значений см. [в разделе Отслеживание изменений в EF Core](xref:core/change-tracking/index) .

### <a name="accessing-temporary-values"></a>Доступ к временным значениям

Начиная с EF Core 3,0, временные значения хранятся в средстве записи изменений и не задаются непосредственно на экземплярах сущностей. Однако эти _временные значения предоставляются_ при использовании различных механизмов [доступа к отслеживающим сущностям](xref:core/change-tracking/entity-entries). Например, следующий код обращается к временному значению с помощью <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.CurrentValues%2A?displayProperty=nameWithType> :

<!--
        using var context = new BlogsContext();

        var blog = new Blog { Name = ".NET Blog" };

        context.Add(blog);

        Console.WriteLine($"Blog.Id set on entity is {blog.Id}");
        Console.WriteLine($"Blog.Id tracked by EF is {context.Entry(blog).Property(e => e.Id).CurrentValue}");
-->
[!code-csharp[Temporary_values_1](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/Samples.cs?name=Temporary_values_1)]

Выходные данные этого кода:

```output
Blog.Id set on entity is 0
Blog.Id tracked by EF is -2147482643
```

<xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.IsTemporary%2A?displayProperty=nameWithType> может использоваться для проверки временных значений.

### <a name="manipulating-temporary-values"></a>Работа с временными значениями

Иногда бывает полезно явно работать с временными значениями. Например, Коллекция новых сущностей может быть создана на веб-клиенте и затем сериализована обратно на сервер. Значения внешнего ключа — это один из способов настройки связей между этими сущностями. В следующем коде этот подход используется для связывания графа новых сущностей по внешнему ключу, при этом по-прежнему разрешается создавать реальные значения ключа при вызове метода SaveChanges.

<!--
            var blogs = new List<Blog>
            {
                new Blog { Id = -1, Name = ".NET Blog" },
                new Blog { Id = -2, Name = "Visual Studio Blog" }
            };

            var posts = new List<Post>()
            {
                new Post
                {
                    Id = -1,
                    BlogId = -1,
                    Title = "Announcing the Release of EF Core 5.0",
                    Content = "Announcing the release of EF Core 5.0, a full featured cross-platform..."
                },
                new Post
                {
                    Id = -2,
                    BlogId = -2,
                    Title = "Disassembly improvements for optimized managed debugging",
                    Content = "If you are focused on squeezing out the last bits of performance for your .NET service or..."
                }
            };

            using var context = new BlogsContext();

            foreach (var blog in blogs)
            {
                context.Add(blog).Property(e => e.Id).IsTemporary = true;
            }

            foreach (var post in posts)
            {
                context.Add(post).Property(e => e.Id).IsTemporary = true;
            }

            Console.WriteLine(context.ChangeTracker.DebugView.LongView);

            context.SaveChanges();

            Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Temporary_values_2](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/Samples.cs?name=Temporary_values_2)]

Обратите внимание на указанные ниже моменты.

- Отрицательные числа используются в качестве временных значений ключей. Это не является обязательным, но является общим соглашением для предотвращения конфликта ключей.
- `Post.BlogId`Свойству FK присваивается то же отрицательное значение, что и ПК-PK связанного блога.
- Значения ПЕРВИЧного ключа помечаются как временные, <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.IsTemporary%2A> после того как каждая сущность будет отслеживанием. Это необходимо, так как любое значение ключа, предоставляемое приложением, считается реальным значением ключа.

[Просмотр представления отладки средства записи изменений](xref:core/change-tracking/debug-views) перед вызовом SaveChanges показывает, что значения первичного ключа помечены как временные, а записи связаны с правильными блогами, включая исправление переходов:

```output
Blog {Id: -2} Added
  Id: -2 PK Temporary
  Name: 'Visual Studio Blog'
  Posts: [{Id: -2}]
Blog {Id: -1} Added
  Id: -1 PK Temporary
  Name: '.NET Blog'
  Posts: [{Id: -1}]
Post {Id: -2} Added
  Id: -2 PK Temporary
  BlogId: -2 FK
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: {Id: -2}
  Tags: []
Post {Id: -1} Added
  Id: -1 PK Temporary
  BlogId: -1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: -1}
```

После вызова <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> эти временные значения были заменены реальными значениями, создаваемыми базой данных:

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog'
  Posts: [{Id: 1}]
Blog {Id: 2} Unchanged
  Id: 2 PK
  Name: 'Visual Studio Blog'
  Posts: [{Id: 2}]
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
  Tags: []
Post {Id: 2} Unchanged
  Id: 2 PK
  BlogId: 2 FK
  Content: 'If you are focused on squeezing out the last bits of perform...'
  Title: 'Disassembly improvements for optimized managed debugging'
  Blog: {Id: 2}
  Tags: []
```

## <a name="working-with-default-values"></a>Работа со значениями по умолчанию

EF Core позволяет свойству получить значение по умолчанию из базы данных при <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> вызове метода. Как и для созданных значений ключа, EF Core будет использовать по умолчанию только из базы данных, если значение не было задано явно. Например, рассмотрим следующий тип сущности:

<!--
    public class Token
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public DateTime ValidFrom { get; set; }
    }
-->
[!code-csharp[Token](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=Token)]

`ValidFrom`Свойство настроено для получения значения по умолчанию из базы данных:

<!--
        modelBuilder
            .Entity<Token>()
            .Property(e => e.ValidFrom)
            .HasDefaultValueSql("CURRENT_TIMESTAMP");
-->
[!code-csharp[OnModelCreating_Token](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=OnModelCreating_Token)]

При вставке сущности этого типа EF Core позволяет базе данных формировать значение, если вместо этого не задано явное значение. Например:

<!--
            using var context = new BlogsContext();

            context.AddRange(
                new Token { Name = "A" },
                new Token { Name = "B", ValidFrom = new DateTime(1111, 11, 11, 11, 11, 11)});

            context.SaveChanges();

            Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Working_with_default_values_1](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=Working_with_default_values_1)]

[Просмотр представления отладки "средство записи изменений](xref:core/change-tracking/debug-views) " показывает, что первый маркер был `ValidFrom` создан базой данных, тогда как второй токен использовал явно заданный параметр:

```output
Token {Id: 1} Unchanged
  Id: 1 PK
  Name: 'A'
  ValidFrom: '12/30/2020 6:36:06 PM'
Token {Id: 2} Unchanged
  Id: 2 PK
  Name: 'B'
  ValidFrom: '11/11/1111 11:11:11 AM'
```

> [!NOTE]
> Для использования значений по умолчанию базы данных требуется, чтобы в столбце базы данных было настроено ограничение значения по умолчанию. Это выполняется автоматически EF Core миграции при использовании <xref:Microsoft.EntityFrameworkCore.RelationalPropertyBuilderExtensions.HasDefaultValueSql%2A> или <xref:Microsoft.EntityFrameworkCore.RelationalPropertyBuilderExtensions.HasDefaultValue%2A> . Обязательно создайте ограничение по умолчанию для столбца каким бы то ни было другим способом, если не использовать EF Core миграции.

### <a name="using-nullable-properties"></a>Использование свойств, допускающих значение null

EF Core может определить, установлено ли свойство, сравнив значение свойства со значением по умолчанию CLR для этого типа. Это хорошо работает в большинстве случаев, но это означает, что значение CLR по умолчанию не может быть явно вставлено в базу данных. Например, рассмотрим сущность со свойством Integer:

<!--
public class Foo1
{
    public int Id { get; set; }
    public int Count { get; set; }
}
-->
[!code-csharp[Foo1](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=Foo1)]

Где это свойство настроено для использования базы данных по умолчанию, равной-1:

<!--
        modelBuilder
            .Entity<Foo1>()
            .Property(e => e.Count)
            .HasDefaultValue(-1);
-->
[!code-csharp[OnModelCreating_Foo1](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=OnModelCreating_Foo1)]

Целью является то, что по умолчанию используется значение-1, если не задано явное. Однако установка значения 0 (значение по умолчанию CLR для целых чисел) не различается для EF Core не устанавливая никакого значения, это означает, что невозможно вставить 0 для этого свойства. Например:

<!--
        using var context = new BlogsContext();

        var fooA = new Foo1 { Count = 10 };
        var fooB = new Foo1 { Count = 0 };
        var fooC = new Foo1 { };

        context.AddRange(fooA, fooB, fooC);
        context.SaveChanges();

        Debug.Assert(fooA.Count == 10);
        Debug.Assert(fooB.Count == -1); // Not what we want!
        Debug.Assert(fooC.Count == -1);
-->
[!code-csharp[Working_with_default_values_2](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=Working_with_default_values_2)]

Обратите внимание, что экземпляр, `Count` для которого было явно задано значение 0, по-прежнему получает из базы данных значения по умолчанию, а это не то, что мы предполагали. Простой способ решения этой проблемы — сделать `Count` свойство допускающим значение NULL:

<!--
public class Foo2
{
    public int Id { get; set; }
    public int? Count { get; set; }
}
-->
[!code-csharp[Foo2](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=Foo2)]

Это делает значение CLR по умолчанию NULL, а не 0, что означает, что 0 будет вставлен при явном задании:

<!--
        using var context = new BlogsContext();

        var fooA = new Foo2 { Count = 10 };
        var fooB = new Foo2 { Count = 0 };
        var fooC = new Foo2 { };

        context.AddRange(fooA, fooB, fooC);
        context.SaveChanges();

        Debug.Assert(fooA.Count == 10);
        Debug.Assert(fooB.Count == 0);
        Debug.Assert(fooC.Count == -1);
-->
[!code-csharp[Working_with_default_values_3](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=Working_with_default_values_3)]

### <a name="using-nullable-backing-fields"></a>Использование резервных полей, допускающих значения NULL

> [!NOTE]
> Шаблон резервного поля, допускающего значение null, поддерживается EF Core 5,0 и более поздних версий.

Проблема в том, что свойство допускает значение null, которое не может быть концептуально реализовано в модели предметной области. Принудительное применение значения NULL для свойства может привести к нарушению модели.

Начиная с EF Core 5,0, свойство может не допускать значения NULL, а только резервное поле, допускающее значение null. Например:

<!--
public class Foo3
{
    public int Id { get; set; }

    private int? _count;
    public int Count
    {
        get => _count ?? -1;
        set => _count = value;
    }
}
-->
[!code-csharp[Foo3](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=Foo3)]

Это позволяет вставить значение CLR по умолчанию (0), если свойство явно имеет значение 0, а не требуется предоставлять свойство как допускающее значение NULL в модели предметной области. Например:

<!--
            using var context = new BlogsContext();

            var fooA = new Foo3 { Count = 10 };
            var fooB = new Foo3 { Count = 0 };
            var fooC = new Foo3 { };

            context.AddRange(fooA, fooB, fooC);
            context.SaveChanges();

            Debug.Assert(fooA.Count == 10);
            Debug.Assert(fooB.Count == 0);
            Debug.Assert(fooC.Count == -1);
-->
[!code-csharp[Working_with_default_values_4](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=Working_with_default_values_4)]

#### <a name="nullable-backing-fields-for-bool-properties"></a>Резервные поля, допускающие значения NULL для свойств bool

Этот шаблон особенно полезен при использовании свойств bool с создаваемыми по умолчанию хранилищами. Поскольку значение CLR по умолчанию для `bool` равно "false", это означает, что "false" не может быть явно вставлено с использованием обычного шаблона. Например, рассмотрим `User` тип сущности:

<!--
public class User
{
    public int Id { get; set; }
    public string Name { get; set; }

    private bool? _isAuthorized;
    public bool IsAuthorized
    {
        get => _isAuthorized ?? true;
        set => _isAuthorized = value;
    }
}
-->
[!code-csharp[User](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=User)]

Для `IsAuthorized` свойства задано значение по умолчанию "true" для базы данных:

<!--
        modelBuilder
            .Entity<User>()
            .Property(e => e.IsAuthorized)
            .HasDefaultValue(true);
-->
[!code-csharp[OnModelCreating_User](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=OnModelCreating_User)]

`IsAuthorized`Для свойства можно задать значение "true" или "false" явным образом перед вставкой, или же можно оставить параметр неопределенным, если будет использоваться база данных по умолчанию:

<!--
        using var context = new BlogsContext();

        var userA = new User { Name = "Mac" };
        var userB = new User { Name = "Alice", IsAuthorized = true };
        var userC = new User { Name = "Baxter", IsAuthorized = false }; // Always deny Baxter access!

        context.AddRange(userA, userB, userC);

        context.SaveChanges();
-->
[!code-csharp[Working_with_default_values_5](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=Working_with_default_values_5)]

Выходные данные SaveChanges при использовании SQLite показывают, что по умолчанию используется база данных для Mac, а для Алисы и Бакстер заданы явные значения:

```sql
-- Executed DbCommand (0ms) [Parameters=[@p0='Mac' (Size = 3)], CommandType='Text', CommandTimeout='30']
INSERT INTO "User" ("Name")
VALUES (@p0);
SELECT "Id", "IsAuthorized"
FROM "User"
WHERE changes() = 1 AND "rowid" = last_insert_rowid();

-- Executed DbCommand (0ms) [Parameters=[@p0='True' (DbType = String), @p1='Alice' (Size = 5)], CommandType='Text', CommandTimeout='30']
INSERT INTO "User" ("IsAuthorized", "Name")
VALUES (@p0, @p1);
SELECT "Id"
FROM "User"
WHERE changes() = 1 AND "rowid" = last_insert_rowid();

-- Executed DbCommand (0ms) [Parameters=[@p0='False' (DbType = String), @p1='Baxter' (Size = 6)], CommandType='Text', CommandTimeout='30']
INSERT INTO "User" ("IsAuthorized", "Name")
VALUES (@p0, @p1);
SELECT "Id"
FROM "User"
WHERE changes() = 1 AND "rowid" = last_insert_rowid();
```

### <a name="schema-defaults-only"></a>Только схемы по умолчанию

Иногда бывает полезно иметь значения по умолчанию в схеме базы данных, созданной EF Core миграции, без EF Core использования этих значений для вставок. Это можно сделать, настроив свойство так, как <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.PropertyBuilder.ValueGeneratedNever%2A?displayProperty=nameWithType> показано ниже.

<!--
        modelBuilder
            .Entity<Bar>()
            .Property(e => e.Count)
            .HasDefaultValue(-1)
            .ValueGeneratedNever();
-->
[!code-csharp[OnModelCreating_Bar](../../../samples/core/ChangeTracking/AdditionalChangeTrackingFeatures/DefaultValueSamples.cs?name=OnModelCreating_Bar)]
