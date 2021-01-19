---
title: Каскадное удаление — EF Core
description: Настройка каскадной реакции на события, активируемой при удалении сущности или разрыве ее связи с основной или родительской сущностью
author: ajcvickers
ms.date: 01/07/2021
uid: core/saving/cascade-delete
ms.openlocfilehash: 7c35de900930cf42da0e534df76124b5fb19ca52
ms.sourcegitcommit: 032a1767d7a6e42052a005f660b80372c6521e7e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98128866"
---
# <a name="cascade-delete"></a>Каскадное удаление

В Entity Framework Core (EF Core) связи представлены с помощью внешних ключей. Сущность с внешним ключом является дочерней или зависимой в связи. Значение внешнего ключа этой сущности должно соответствовать значению первичного ключа (или альтернативному значению ключа) связанной основной или родительской сущности.

Если основная или родительская сущность удалена, значения внешних ключей зависимых и дочерних сущностей больше не соответствуют первичному или альтернативному ключу _какой-либо_ основной или родительской сущности. Это недопустимое состояние, которое в большинстве баз данных приведет к нарушению ссылочного ограничения.

Избежать этого нарушения можно двумя способами.

1. Присвоить внешним ключам значения NULL.
2. Удалить соответствующие зависимые или дочерние сущности.

Первый способ подходит только для необязательных связей, в которых свойство внешнего ключа (и столбец базы данных, с которым оно сопоставлено), должно допускать значения NULL.

Второй вариант допустим для любого типа связи и называется каскадным удалением.

> [!TIP]
> В этом документе описывается каскадное удаление (и удаление потерянных объектов) в контексте обновления базы данных. При этом активно используются концепции, представленные в разделах [Отслеживание изменений в EF Core](xref:core/change-tracking/index) и [Изменение внешних ключей и свойств навигации](xref:core/change-tracking/relationship-changes). Прежде чем приступать к изучению этого документа, обязательно ознакомьтесь с ними.

> [!TIP]  
> Вы можете запустить и отладить весь код, используемый в этой документации, [скачав пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/CascadeDeletes).

## <a name="when-cascading-behaviors-happen"></a>Когда происходит каскадная реакция на события

Каскадное удаление требуется в тех случаях, когда зависимая или дочерняя сущность теряет связь со своей текущей основной или родительской сущностью. Это может произойти в результате удаления основной или родительской сущности, а также в случае разрыва связи между ней и зависимой или дочерней сущностью.

### <a name="deleting-a-principalparent"></a>Удаление основной или родительской сущности

Рассмотрим простую модель, в которой `Blog` является основной или родительской сущностью в связи с дочерней или зависимой сущностью `Post`. В качестве внешнего ключа используется свойство `Post.BlogId`, значение которого должно соответствовать первичному ключу `Post.Id` блога (Blog), которому принадлежит запись (Post).

<!--
    public class Blog
    {
        public int Id { get; set; }

        public string Name { get; set; }

        public IList<Post> Posts { get; } = new List<Post>();
    }

    public class Post
    {
        public int Id { get; set; }

        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
-->
[!code-csharp[Model](../../../samples/core/CascadeDeletes/IntroRequiredSamples.cs?name=Model)]

По соглашению эта связь настроена как обязательная, поскольку свойство внешнего ключа `Post.BlogId` не допускает значения NULL. Для обязательных связей по умолчанию используется каскадное удаление. Дополнительные сведения о моделировании связей см. в разделе [Связи](xref:core/modeling/relationships).

При удалении блога происходит каскадное удаление всех его записей. Пример:

<!--
            using var context = new BlogsContext();

            var blog = context.Blogs.OrderBy(e => e.Name).Include(e => e.Posts).First();

            context.Remove(blog);
            
            context.SaveChanges();
-->
[!code-csharp[Deleting_principal_parent_1](../../../samples/core/CascadeDeletes/IntroRequiredSamples.cs?name=Deleting_principal_parent_1)]

Метод SaveChanges создает следующий код SQL (на примере SQL Server).

```sql
-- Executed DbCommand (1ms) [Parameters=[@p0='1'], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
DELETE FROM [Posts]
WHERE [Id] = @p0;
SELECT @@ROWCOUNT;

-- Executed DbCommand (0ms) [Parameters=[@p0='2'], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
DELETE FROM [Posts]
WHERE [Id] = @p0;
SELECT @@ROWCOUNT;

-- Executed DbCommand (2ms) [Parameters=[@p1='1'], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
DELETE FROM [Blogs]
WHERE [Id] = @p1;
SELECT @@ROWCOUNT;
```

### <a name="severing-a-relationship"></a>Разрыв связи

Вместо удаления блога можно разорвать связь между ним и каждой его записью. Это можно сделать, установив значение NULL для свойства навигации по ссылке `Post.Blog` для каждой записи:

<!--
            using var context = new BlogsContext();

            var blog = context.Blogs.OrderBy(e => e.Name).Include(e => e.Posts).First();

            foreach (var post in blog.Posts)
            {
                post.Blog = null;
            }
            
            context.SaveChanges();
-->
[!code-csharp[Severing_a_relationship_1](../../../samples/core/CascadeDeletes/IntroRequiredSamples.cs?name=Severing_a_relationship_1)]

Связь также можно разорвать, удалив каждую запись из свойства навигации по коллекции `Blog.Posts`.

<!--
            using var context = new BlogsContext();

            var blog = context.Blogs.OrderBy(e => e.Name).Include(e => e.Posts).First();

            blog.Posts.Clear();
            
            context.SaveChanges();
-->
[!code-csharp[Severing_a_relationship_2](../../../samples/core/CascadeDeletes/IntroRequiredSamples.cs?name=Severing_a_relationship_2)]

В обоих случаях будет достигнут одинаковый результат, то есть записи, которые больше не связаны ни с одним блогом, удаляются, а сам блог остается.

```sql
-- Executed DbCommand (1ms) [Parameters=[@p0='1'], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
DELETE FROM [Posts]
WHERE [Id] = @p0;
SELECT @@ROWCOUNT;

-- Executed DbCommand (0ms) [Parameters=[@p0='2'], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
DELETE FROM [Posts]
WHERE [Id] = @p0;
SELECT @@ROWCOUNT;
```

Удаление сущностей, которые больше не связаны ни с одной основной или родительской сущностью, называется удалением потерянных объектов.

> [!TIP]
> Концепции каскадного удаления и удаления потерянных объектов тесно взаимосвязаны. В обоих случаях при разрыве связи с основной или родительской сущностью ее зависимые и дочерние сущности удаляются. При каскадном удалении это происходит вследствие удаления самой основной или родительской сущности. При удалении потерянных объектов основная или родительская сущность по-прежнему существует, но при этом больше не связана с зависимыми и дочерними сущностями.  

## <a name="where-cascading-behaviors-happen"></a>Где происходит каскадная реакция на события

Каскадная реакция на события может применяться к следующим сущностям.

- Сущности, которые отслеживаются текущим экземпляром <xref:Microsoft.EntityFrameworkCore.DbContext>.
- Сущности в базе данных, которые не были загружены в контекст.

### <a name="cascade-delete-of-tracked-entities"></a>Каскадное удаление отслеживаемых сущностей

EF Core всегда применяет к отслеживаемым сущностям каскадную реакцию на события. Это значит, что, если приложение загружает все соответствующие зависимые или дочерние сущности в DbContext (как показано в приведенных выше примерах), каскадная реакция на события будет корректно применяться, независимо конфигурации базы данных.

> [!TIP]
> Точное время, когда происходит каскадная реакция для отслеживаемых сущностей, можно контролировать с помощью свойств <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.CascadeDeleteTiming?displayProperty=nameWithType> и <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DeleteOrphansTiming?displayProperty=nameWithType>. Дополнительные сведения см. в разделе [Изменение внешних ключей и свойств навигации](xref:core/change-tracking/relationship-changes).

### <a name="cascade-delete-in-the-database"></a>Каскадное удаление в базе данных

Многие системы баз данных также реализуют каскадную реакцию на события, которая активируется при удалении сущности в базе данных. EF Core определяет конфигурацию такой реакции на основе настроек каскадного удаления в модели EF Core при создании базы данных с использованием метода <xref:Microsoft.EntityFrameworkCore.Infrastructure.DatabaseFacade.EnsureCreated%2A> или миграций EF Core. Например, в представленной выше модели при использовании SQL Server для записей создается следующая таблица.

```sql
CREATE TABLE [Posts] (
    [Id] int NOT NULL IDENTITY,
    [Title] nvarchar(max) NULL,
    [Content] nvarchar(max) NULL,
    [BlogId] int NOT NULL,
    CONSTRAINT [PK_Posts] PRIMARY KEY ([Id]),
    CONSTRAINT [FK_Posts_Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [Blogs] ([Id]) ON DELETE CASCADE
);
```

Обратите внимание, что ограничение внешнего ключа, определяющее связь между блогами и записями, настраивается с использованием `ON DELETE CASCADE`.

Если мы знаем, что база данных настроена таким образом, можно удалить блог _без предварительной загрузки записей_. В этом случае база данных автоматически удалит все связанные с ним записи. Пример:

<!--
            using var context = new BlogsContext();

            var blog = context.Blogs.OrderBy(e => e.Name).First();

            context.Remove(blog);
            
            context.SaveChanges();
-->
[!code-csharp[Where_cascading_behaviors_happen_1](../../../samples/core/CascadeDeletes/IntroRequiredSamples.cs?name=Where_cascading_behaviors_happen_1)]

Обратите внимание, что для записей не используется `Include`, поэтому они не загружаются. В этом случае метод SaveChanges удаляет только блог, поскольку это единственная отслеживаемая сущность.

```sql
-- Executed DbCommand (6ms) [Parameters=[@p0='1'], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
DELETE FROM [Blogs]
WHERE [Id] = @p0;
SELECT @@ROWCOUNT;
```

Если ограничение внешнего ключа в базе данных не настроено для каскадного удаления, это приведет к возникновению исключения. Тем не менее в этом случае записи будут удалены базой данных, так как при ее создании была задана настройка `ON DELETE CASCADE`.

> [!NOTE]
> Как правило, базы данных не поддерживают автоматическое удаление потерянных объектов. Это связано с тем, что в базах данных используются только внешние ключи и нет свойств навигации, тогда как в EF Core связи представляются с использованием и внешних ключей, и свойств навигации. Это значит, что в большинстве случаев для разрыва связи обе ее стороны должны быть загружены в DbContext.

> [!NOTE]
> Выполняющаяся в памяти база данных EF Core в настоящее время не поддерживает каскадное удаление в базе данных.

> [!WARNING]
> Не следует настраивать каскадное удаление в базе данных при обратимом удалении сущностей. Это может привести к случайному полному (вместо обратимого) удалению сущностей.

### <a name="database-cascade-limitations"></a>Ограничения для каскадной реакции на события в базе данных

Некоторые базы данных, особенно SQL Server, устанавливают ограничения для каскадных операций, которые приводят к возникновению циклов. Рассмотрим следующую модель.

<!--
    public class Blog
    {
        public int Id { get; set; }
        public string Name { get; set; }

        public IList<Post> Posts { get; } = new List<Post>();
        
        public int OwnerId { get; set; }
        public Person Owner { get; set; }
    }

    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
        
        public int AuthorId { get; set; }
        public Person Author { get; set; }
    }

    public class Person
    {
        public int Id { get; set; }
        public string Name { get; set; }
        
        public IList<Post> Posts { get; } = new List<Post>();

        public Blog OwnedBlog { get; set; }
    }
-->
[!code-csharp[Model](../../../samples/core/CascadeDeletes/WithDatabaseCycleSamples.cs?name=Model)]

В этой модели устанавливаются три связи, каждая из которых является обязательной и по соглашению настраивается для каскадного удаления.

- При удалении блога происходит каскадное удаление всех связанных с ним записей.
- При удалении автора записей происходит каскадное удаление созданных им записей.
- При удалении владельца блога происходит каскадное удаление самого блога.

Это вполне разумный, хотя и достаточно строгий подход к управлению блогом. Тем не менее при попытке создать базу данных SQL Server с этими настройками каскадной реакции на событий возникнет следующее исключение.

> Microsoft.Data.SqlClient.SqlException (0x80131904): Введение ограничения внешнего ключа (FOREIGN KEY) FK_Posts_Person_AuthorId для таблицы Posts может привести к появлению циклов или множественных каскадных путей. Укажите ON DELETE NO ACTION или ON UPDATE NO ACTION либо измените другие ограничения внешнего ключа (FOREIGN KEY).

Эту проблему можно решить двумя способами.

1. Изменить одну связь или несколько, отключив их каскадное удаление.
2. Настроить базу данных, отключив одно или несколько из этих действий каскадного удаления, после чего убедиться, что все зависимые сущности загружены, чтобы позволить EF Core реализовывать каскадную реакцию на события.

Применение первого подхода в нашем примере позволяет сделать связь между блогом и его владельцем необязательной, предоставив свойство внешнего ключа, допускающее значение NULL.

<!--
            public int? BlogId { get; set; }
-->
[!code-csharp[NullableBlogId](../../../samples/core/CascadeDeletes/OptionalDependentsSamples.cs?name=NullableBlogId)]

Поскольку эта связь не является обязательной, блог может существовать без владельца, то есть каскадное удаление больше не будет настроено по умолчанию. Это значит, что каскадная реакция на события больше не будет приводить к возникновению циклов, благодаря чему база данных может быть создана в SQL Server без ошибок.

Если использовать второй подход, связь между блогом и владельцем можно оставить обязательной и настроить для каскадного удаления, но при этом задать применение этой конфигурации только к отслеживаемым сущностям, а не к базе данных.

<!--
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder
                .Entity<Blog>()
                .HasOne(e => e.Owner)
                .WithOne(e => e.OwnedBlog)
                .OnDelete(DeleteBehavior.ClientCascade);
        }
-->
[!code-csharp[OnModelCreating](../../../samples/core/CascadeDeletes/WithDatabaseCycleSamples.cs?name=OnModelCreating)]

Что произойдет, если загрузить владельца блога и сам блог, а затем удалить владельца?

<!--
            using var context = new BlogsContext();

            var owner = context.People.Single(e => e.Name == "ajcvickers");
            var blog = context.Blogs.Single(e => e.Owner == owner);

            context.Remove(owner);
            
            context.SaveChanges();
-->
[!code-csharp[Database_cascade_limitations_1](../../../samples/core/CascadeDeletes/WithDatabaseCycleSamples.cs?name=Database_cascade_limitations_1)]

EF Core выполнит каскадное удаление владельца и принадлежащего ему блога.

```sql
-- Executed DbCommand (8ms) [Parameters=[@p0='1'], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
DELETE FROM [Blogs]
WHERE [Id] = @p0;
SELECT @@ROWCOUNT;

-- Executed DbCommand (2ms) [Parameters=[@p1='1'], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
DELETE FROM [People]
WHERE [Id] = @p1;
SELECT @@ROWCOUNT;
```

Тем не менее, если блог не был загружен, при удалении его владельца происходит следующее.

<!--
                using var context = new BlogsContext();

                var owner = context.People.Single(e => e.Name == "ajcvickers");

                context.Remove(owner);
            
                context.SaveChanges();
-->
[!code-csharp[Database_cascade_limitations_2](../../../samples/core/CascadeDeletes/WithDatabaseCycleSamples.cs?name=Database_cascade_limitations_2)]

После этого возникает исключение из-за нарушения ограничения внешнего ключа в базе данных.

> Microsoft.Data.SqlClient.SqlException: Конфликт инструкции DELETE с ограничением REFERENCE FK_Blogs_People_OwnerId. Конфликт произошел в базе данных Scratch, таблица dbo.Blogs, столбец OwnerId.
Выполнение данной инструкции было прервано.

## <a name="cascading-nulls"></a>Каскадное распространение значений NULL

Для необязательных связей свойства внешнего ключа, допускающие значения NULL, сопоставлены со столбцами базы данных, также допускающими такие значения. Это означает, что внешнему ключу может быть присвоено значение NULL, если текущая основная или родительская сущность удаляется или разрывается ее связь с зависимой или дочерней сущностью.

Еще раз возьмем примеры из раздела [Когда происходит каскадная реакция на события](#when-cascading-behaviors-happen). На этот раз рассмотрим необязательную связь, которая представлена допускающим значение NULL свойством внешнего ключа `Post.BlogId`:

<!--
            public int? BlogId { get; set; }
-->
[!code-csharp[NullableBlogId](../../../samples/core/CascadeDeletes/OptionalDependentsSamples.cs?name=NullableBlogId)]

Этому свойству внешнего ключа будет присваиваться значение NULL для каждой записи при удалении связанного с ней блога. Например, следующий код аналогичен приведенному ранее.

<!--
            using var context = new BlogsContext();

            var blog = context.Blogs.OrderBy(e => e.Name).Include(e => e.Posts).First();

            context.Remove(blog);
            
            context.SaveChanges();
-->
[!code-csharp[Deleting_principal_parent_1b](../../../samples/core/CascadeDeletes/IntroOptionalSamples.cs?name=Deleting_principal_parent_1b)]

Тем не менее теперь при вызове метода SaveChanges будут выполнены следующие обновления базы данных.

```sql
-- Executed DbCommand (2ms) [Parameters=[@p1='1', @p0=NULL (DbType = Int32)], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
UPDATE [Posts] SET [BlogId] = @p0
WHERE [Id] = @p1;
SELECT @@ROWCOUNT;

-- Executed DbCommand (0ms) [Parameters=[@p1='2', @p0=NULL (DbType = Int32)], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
UPDATE [Posts] SET [BlogId] = @p0
WHERE [Id] = @p1;
SELECT @@ROWCOUNT;

-- Executed DbCommand (1ms) [Parameters=[@p2='1'], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
DELETE FROM [Blogs]
WHERE [Id] = @p2;
SELECT @@ROWCOUNT;
```

Аналогичным образом, при разрыве связи в любом из приведенных выше примеров:

<!--
            using var context = new BlogsContext();

            var blog = context.Blogs.OrderBy(e => e.Name).Include(e => e.Posts).First();

            foreach (var post in blog.Posts)
            {
                post.Blog = null;
            }
            
            context.SaveChanges();
-->
[!code-csharp[Severing_a_relationship_1b](../../../samples/core/CascadeDeletes/IntroOptionalSamples.cs?name=Severing_a_relationship_1b)]

Или сделайте так:

<!--
            using var context = new BlogsContext();

            var blog = context.Blogs.OrderBy(e => e.Name).Include(e => e.Posts).First();

            blog.Posts.Clear();
            
            context.SaveChanges();
-->
[!code-csharp[Severing_a_relationship_2b](../../../samples/core/CascadeDeletes/IntroOptionalSamples.cs?name=Severing_a_relationship_2b)]

При вызове метода SaveChanges записи будут обновлены с присвоением внешнему ключу значения NULL.

```sql
-- Executed DbCommand (2ms) [Parameters=[@p1='1', @p0=NULL (DbType = Int32)], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
UPDATE [Posts] SET [BlogId] = @p0
WHERE [Id] = @p1;
SELECT @@ROWCOUNT;

-- Executed DbCommand (0ms) [Parameters=[@p1='2', @p0=NULL (DbType = Int32)], CommandType='Text', CommandTimeout='30']
SET NOCOUNT ON;
UPDATE [Posts] SET [BlogId] = @p0
WHERE [Id] = @p1;
SELECT @@ROWCOUNT;
```

Дополнительные сведения о том, как EF Core управляет внешними ключами и свойствами навигации при изменении их значений, см. в разделе [Изменение внешних ключей и свойств навигации](xref:core/change-tracking/relationship-changes).

> [!NOTE]
> По умолчанию, начиная с первой версии Entity Framework 2008 года, выполнялась адресная привязка таких связей. До появления EF Core у такого подхода не было своего названия и его нельзя было изменить. Сейчас он называется `ClientSetNull` и описывается в следующем разделе.

Базы данных также можно настроить для подобного каскадного распространения значений NULL при удалении основной или родительской сущности в необязательной связи. Тем не менее этот подход применяется гораздо реже операций каскадного удаления в базе данных. Одновременное выполнение каскадного удаления и каскадного распространения значений NULL в базе данных SQL Server почти всегда будет приводить к возникновению циклов связей. Дополнительные сведения о настройке каскадного распространения значений NULL см. в следующем разделе.

## <a name="configuring-cascading-behaviors"></a>Настройка каскадной реакции на события

> [!TIP]
> Прежде чем переходить к этому разделу, ознакомьтесь с приведенными выше. Для понимания параметров конфигурации потребуется знание представленных ранее материалов.

Настройка каскадной реакции на события для отдельных связей осуществляется с помощью метода <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.ReferenceCollectionBuilder.OnDelete%2A> в <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A>. Пример:

<!--
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder
                .Entity<Blog>()
                .HasOne(e => e.Owner)
                .WithOne(e => e.OwnedBlog)
                .OnDelete(DeleteBehavior.ClientCascade);
        }
-->
[!code-csharp[OnModelCreating](../../../samples/core/CascadeDeletes/WithDatabaseCycleSamples.cs?name=OnModelCreating)]

Дополнительные сведения о настройке связей между типами сущностей см. в разделе [Связи](xref:core/modeling/relationships).

Метод `OnDelete` принимает значение из перечисления <xref:Microsoft.EntityFrameworkCore.DeleteBehavior>, с использованием которого связаны определенные сложности. В этом перечислении определяется поведение EF Core для отслеживаемых сущностей и задается конфигурация каскадного удаления в базе данных в случаях, когда для создания схемы используется EF.

### <a name="impact-on-database-schema"></a>Влияние на схему базы данных

В следующей таблице показан результат применения каждого значения метода `OnDelete` для ограничения внешнего ключа, создаваемого с помощью миграций EF Core или метода <xref:Microsoft.EntityFrameworkCore.Infrastructure.DatabaseFacade.EnsureCreated%2A>.

| Значение перечисления DeleteBehavior        | Влияние на схему базы данных
|:----------------------|--------------------------
| Cascade               | ON DELETE CASCADE
| Ограничение              | ON DELETE NO ACTION
| NoAction              | <database default>
| SetNull               | ON DELETE SET NULL
| ClientSetNull         | ON DELETE NO ACTION
| ClientCascade         | ON DELETE NO ACTION
| ClientNoAction        | <database default>

> [!NOTE]
> Эта таблица определена неоднозначно и в ближайшем выпуске будет пересмотрена. См. [описание проблемы 21252 на сайте GitHub](https://github.com/dotnet/efcore/issues/21252).

Действия `ON DELETE NO ACTION` и `ON DELETE RESTRICT` в реляционных базах данных, как правило, идентичны или очень близки. Несмотря на название `NO ACTION`, оба эти параметра приводят к принудительному применению ссылочных ограничений. Единственное различие между ними заключается в том, _когда_ база данных выполняет проверку ограничений.  Сведения о конкретных различиях между параметрами `ON DELETE NO ACTION` и `ON DELETE RESTRICT` в вашей системе базы данных следует искать в документации по ней.

Каскадная реакция на события в базе данных реализуется только при использовании значений `Cascade` и `SetNull`. При любых других значениях каскадное распространение изменений в базе данных не осуществляется.

### <a name="impact-on-savechanges-behavior"></a>Влияние на поведение метода SaveChanges

В следующих разделах приводятся таблицы, в которых описывается, что происходит с зависимыми или дочерними сущностями при удалении основной или родительской сущности, а также при разрыве ее связи с зависимой или дочерней сущностью. Каждая таблица описывает одну из следующих ситуаций.

- Необязательные (внешний ключ, допускающий значения NULL) и обязательные (не допускающий значения NULL внешний ключ) связи
- Зависимые или дочерние сущности загружаются в DbContext и отслеживаются в нем (либо существуют только в базе данных).

#### <a name="required-relationship-with-dependentschildren-loaded"></a>Обязательная связь с загруженными зависимыми или дочерними сущностями

| Значение перечисления DeleteBehavior    | Поведение при удалении основной или родительской сущности             | Поведение при разрыве связи с основной или родительской сущностью
|:------------------|------------------------------------------|----------------------------------------
| Cascade           | EF Core удаляет зависимые сущности            | EF Core удаляет зависимые сущности
| Ограничение          | `InvalidOperationException`              | `InvalidOperationException`
| NoAction          | `InvalidOperationException`              | `InvalidOperationException`
| SetNull           | При создании базы данных возникает исключение `SqlException`      | При создании базы данных возникает исключение `SqlException`
| ClientSetNull     | `InvalidOperationException`              | `InvalidOperationException`
| ClientCascade     | EF Core удаляет зависимые сущности            | EF Core удаляет зависимые сущности
| ClientNoAction    | `DbUpdateException`                      | `InvalidOperationException`

Примечания.

- По умолчанию для таких обязательных связей задается значение `Cascade`.
- Применение любого другого значения для каскадного удаления обязательных связей приведет к возникновению исключения при вызове метода SaveChanges.
  - Как правило, в этом случае возникает исключение EF Core `InvalidOperationException`, так как в загруженных дочерних или зависимых сущностях обнаруживается недопустимое состояние.
  - Если используется значение `ClientNoAction`, EF Core не проверяет зависимые сущности адресной привязки перед их отправкой в базу данных, поэтому в этом случае база данных создает исключение, которое затем упаковывается в исключение `DbUpdateException` методом SaveChanges.
  - Значение `SetNull` при создании базы данных отклоняется, поскольку столбец внешнего ключа не допускает значения NULL.
- Поскольку зависимые и дочерние сущности загружаются, они всегда будут удаляться с помощью EF Core и никогда не удаляются базой данных.

#### <a name="required-relationship-with-dependentschildren-not-loaded"></a>Обязательная связь, для которой не загружены зависимые или дочерние сущности

| Значение перечисления DeleteBehavior    | Поведение при удалении основной или родительской сущности             | Поведение при разрыве связи с основной или родительской сущностью
|:------------------|------------------------------------------|----------------------------------------
| Cascade           | База данных удаляет зависимые сущности           | Н/Д
| Ограничение          | `DbUpdateException`                      | Н/Д
| NoAction          | `DbUpdateException`                      | Н/Д
| SetNull           | При создании базы данных возникает исключение `SqlException`      | Н/Д
| ClientSetNull     | `DbUpdateException`                      | Н/Д
| ClientCascade     | `DbUpdateException`                      | Н/Д
| ClientNoAction    | `DbUpdateException`                      | Н/Д

Примечания.

- Разрыв связи в этом случае невозможен, поскольку зависимые или дочерние сущности не загружаются.
- По умолчанию для таких обязательных связей задается значение `Cascade`.
- Применение любого другого значения для каскадного удаления обязательных связей приведет к возникновению исключения при вызове метода SaveChanges.
  - Как правило, это исключение`DbUpdateException`, поскольку зависимые или дочерние сущности не загружаются, поэтому недопустимое состояние может быть обнаружено только базой данных. После этого метод SaveChanges упаковывает исключение базы данных в `DbUpdateException`.
  - Значение `SetNull` при создании базы данных отклоняется, поскольку столбец внешнего ключа не допускает значения NULL.

#### <a name="optional-relationship-with-dependentschildren-loaded"></a>Необязательная связь с загруженными зависимыми или дочерними сущностями

| Значение перечисления DeleteBehavior    | Поведение при удалении основной или родительской сущности             | Поведение при разрыве связи с основной или родительской сущностью
|:------------------|------------------------------------------|----------------------------------------
| Cascade           | EF Core удаляет зависимые сущности            | EF Core удаляет зависимые сущности
| Ограничение          | EF Core присваивает значения NULL внешним ключам зависимых сущностей     | EF Core присваивает значения NULL внешним ключам зависимых сущностей
| NoAction          | EF Core присваивает значения NULL внешним ключам зависимых сущностей     | EF Core присваивает значения NULL внешним ключам зависимых сущностей
| SetNull           | EF Core присваивает значения NULL внешним ключам зависимых сущностей     | EF Core присваивает значения NULL внешним ключам зависимых сущностей
| ClientSetNull     | EF Core присваивает значения NULL внешним ключам зависимых сущностей     | EF Core присваивает значения NULL внешним ключам зависимых сущностей
| ClientCascade     | EF Core удаляет зависимые сущности            | EF Core удаляет зависимые сущности
| ClientNoAction    | `DbUpdateException`                      | EF Core присваивает значения NULL внешним ключам зависимых сущностей

Примечания.

- По умолчанию для таких необязательных связей задается значение `ClientSetNull`.
- Зависимые и дочерние сущности никогда не удаляются, если не задано значение `Cascade` или `ClientCascade`.
- При любых других значениях EF Core присваивает значения NULL внешним ключам зависимых сущностей.
  - Исключением является значение `ClientNoAction`, при котором EF Core не затрагивает внешние ключи зависимых или дочерних сущностей при удалении основной или родительской сущности. Таким образом, база данных создает исключение, которое упаковывается в `DbUpdateException` методом SaveChanges.

#### <a name="optional-relationship-with-dependentschildren-not-loaded"></a>Необязательная связь, для которой не загружены зависимые или дочерние сущности

| Значение перечисления DeleteBehavior    | Поведение при удалении основной или родительской сущности             | Поведение при разрыве связи с основной или родительской сущностью
|:------------------|------------------------------------------|----------------------------------------
| Cascade           | База данных удаляет зависимые сущности           | Н/Д
| Ограничение          | `DbUpdateException`                      | Н/Д
| NoAction          | `DbUpdateException`                      | Н/Д
| SetNull           | База данных присваивает значения NULL внешним ключам зависимых сущностей    | Н/Д
| ClientSetNull     | `DbUpdateException`                      | Н/Д
| ClientCascade     | `DbUpdateException`                      | Н/Д
| ClientNoAction    | `DbUpdateException`                      | Н/Д

Примечания.

- Разрыв связи в этом случае невозможен, поскольку зависимые или дочерние сущности не загружаются.
- По умолчанию для таких необязательных связей задается значение `ClientSetNull`.
- Чтобы избежать возникновения исключения базы данных, зависимые и дочерние сущности должны загружаться, если только база данных не настроена для каскадного удаления или распространения значений NULL.
