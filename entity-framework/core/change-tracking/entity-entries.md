---
title: Доступ к отслеживающим сущностям — EF Core
description: Использование Ентитентри, DbContext. записи и DbSet. local для доступа к отслеживающим сущностям
author: ajcvickers
ms.date: 12/30/2020
uid: core/change-tracking/entity-entries
ms.openlocfilehash: 758d21f44dfeb8b1de2702165df0d705edfb91b6
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103024515"
---
# <a name="accessing-tracked-entities"></a>Доступ к отслеживающим сущностям

Существует четыре основных API для доступа к сущностям, которые отправляются <xref:Microsoft.EntityFrameworkCore.DbContext> :

- <xref:System.Data.Entity.DbContext.Entry%2A?displayProperty=nameWithType> Возвращает <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601> экземпляр для заданного экземпляра сущности.
- <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Entries%2A?displayProperty=nameWithType> Возвращает <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601> экземпляры для всех отслеживаний сущностей или для всех отслеживаниющих сущностей заданного типа.
- <xref:Microsoft.EntityFrameworkCore.DbContext.Find%2A?displayProperty=nameWithType>, <xref:Microsoft.EntityFrameworkCore.DbContext.FindAsync%2A?displayProperty=nameWithType> , <xref:Microsoft.EntityFrameworkCore.DbSet%601.Find%2A?displayProperty=nameWithType> и <xref:Microsoft.EntityFrameworkCore.DbSet%601.FindAsync%2A?displayProperty=nameWithType> найдите отдельную сущность по первичному ключу, сначала просматривая отслеживание сущностей, а затем запросите базу данных при необходимости.
- <xref:Microsoft.EntityFrameworkCore.DbSet%601.Local?displayProperty=nameWithType> Возвращает фактические сущности (не Ентитентри экземпляры) для сущностей типа сущности, представленного DbSet.

Каждый из них подробно описан в следующих разделах.

> [!TIP]
> В этом документе предполагается, что состояния сущностей и основы отслеживания изменений EF Core понятны. Дополнительные сведения по этим темам см. [в разделе Отслеживание изменений в EF Core](xref:core/change-tracking/index) .

> [!TIP]
> Вы можете запустить и отладить весь код, используемый в этой документации, [скачав пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/ChangeTracking/AccessingTrackedEntities).

## <a name="using-dbcontextentry-and-entityentry-instances"></a>Использование DbContext. Entry и экземпляров Ентитентри

Для каждой отслеживающая сущность Entity Framework Core (EF Core) отслеживает:

- Общее состояние сущности. Это один из `Unchanged` , `Modified` , `Added` или `Deleted` ; Дополнительные сведения см. [в разделе Отслеживание изменений в EF Core](xref:core/change-tracking/index) .
- Связи между отслеживающими сущностями. Например, блог, которому принадлежит запись.
- Текущие значения свойств.
- Исходные значения свойств, когда эти сведения доступны. Исходные значения — это значения свойств, которые существовали при запросе сущности из базы данных.
- Значения свойств, которые были изменены с момента запроса.
- Другие сведения о значениях свойств, например о том, является ли значение [временным](xref:core/change-tracking/miscellaneous#temporary-values).

Передача экземпляра сущности в <xref:System.Data.Entity.DbContext.Entry%2A?displayProperty=nameWithType> результате <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601> предоставления доступа к этим сведениям для данной сущности. Например:

<!--
        using var context = new BlogsContext();

        var blog = context.Blogs.Single(e => e.Id == 1);
        var entityEntry = context.Entry(blog);

-->
[!code-csharp[Using_DbContext_Entry_and_EntityEntry_instances_1](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Using_DbContext_Entry_and_EntityEntry_instances_1)]

В следующих разделах показано, как использовать Ентитентри для доступа к состоянию сущности и управления им, а также состоянием свойств и переходов сущности.

### <a name="working-with-the-entity"></a>Работа с сущностью

Наиболее часто используется <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601> для доступа к текущей <xref:Microsoft.EntityFrameworkCore.EntityState> сущности. Например:

<!--
        var currentState = context.Entry(blog).State;
        if (currentState == EntityState.Unchanged)
        {
            context.Entry(blog).State = EntityState.Modified;
        }
-->
[!code-csharp[Work_with_the_entity_1](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_the_entity_1)]

Метод Entry можно также использовать для сущностей, которые еще не были отслеживанием. Это _не начинает отслеживание сущности_; состояние сущности по-прежнему равно `Detatched` . Однако возвращенный Ентитентри можно затем использовать для изменения состояния сущности, после чего сущность будет относиться к заданному состоянию. Например, следующий код начнет отслеживать экземпляр блога как `Added` :

<!--
        var newBlog = new Blog();
        Debug.Assert(context.Entry(newBlog).State == EntityState.Detached);

        context.Entry(newBlog).State = EntityState.Added;
        Debug.Assert(context.Entry(newBlog).State == EntityState.Added);
-->
[!code-csharp[Work_with_the_entity_2](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_the_entity_2)]

> [!TIP]
> В отличие от EF6, установка состояния отдельной сущности не приведет к отслеживанию всех подключенных сущностей. Это делает установку состояния таким образом, что операция более низкого уровня не вызывает `Add` , `Attach` или `Update` , которая действует на весь граф сущностей.

В следующей таблице приведены способы использования Ентитентри для работы с целой сущностью.

| Ентитентри, элемент                                                                                         | Описание
|:-----------------------------------------------------------------------------------------------------------|----------------------
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.State?displayProperty=nameWithType>         | Возвращает и задает объект <xref:Microsoft.EntityFrameworkCore.EntityState> сущности.
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Entity?displayProperty=nameWithType>        | Возвращает экземпляр сущности.
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Context?displayProperty=nameWithType>       | Объект <xref:Microsoft.EntityFrameworkCore.DbContext> , отслеживающий эту сущность.
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Metadata?displayProperty=nameWithType>      | <xref:Microsoft.EntityFrameworkCore.Metadata.IEntityType> метаданные для типа сущности.
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.IsKeySet?displayProperty=nameWithType>      | Задано ли значение ключа для сущности.
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Reload?displayProperty=nameWithType>        | Перезаписывает значения свойств значениями, считанными из базы данных.
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.DetectChanges?displayProperty=nameWithType> | Принудительное обнаружение изменений только для этой сущности; см. раздел [Обнаружение изменений и уведомления](xref:core/change-tracking/change-detection).

### <a name="working-with-a-single-property"></a>Работа с одним свойством

Несколько перегрузок <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601.Property%2A?displayProperty=nameWithType> предоставляют доступ к сведениям об отдельном свойстве сущности. Например, с помощью строго типизированного API-интерфейса Fluent:

<!--
            PropertyEntry<Blog, string> propertyEntry = context.Entry(blog).Property(e => e.Name);
-->
[!code-csharp[Work_with_a_single_property_1a](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_a_single_property_1a)]

Вместо этого имя свойства можно передать в виде строки. Например:

<!--
            PropertyEntry<Blog, string> propertyEntry = context.Entry(blog).Property<string>("Name");
-->
[!code-csharp[Work_with_a_single_property_1b](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_a_single_property_1b)]

Затем возвращаемый объект <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry%602> можно использовать для доступа к сведениям о свойстве. Например, его можно использовать для получения и задания текущего значения свойства для этой сущности:

<!--
            string currentValue = context.Entry(blog).Property(e => e.Name).CurrentValue;
            context.Entry(blog).Property(e => e.Name).CurrentValue = "1unicorn2";
-->
[!code-csharp[Work_with_a_single_property_1d](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_a_single_property_1d)]

Оба метода свойства, используемые выше, возвращают универсальный экземпляр со строгим типом <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry%602> . Использование этого универсального типа является предпочтительным, так как обеспечивает доступ к значениям свойств без [упаковки типов значений](/dotnet/csharp/programming-guide/types/boxing-and-unboxing). Однако если тип сущности или свойства неизвестен во время компиляции, <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry> вместо этого можно получить неуниверсальное значение.

<!--
            var propertyEntry = context.Entry(blog).Property("Name");
-->
[!code-csharp[Work_with_a_single_property_1c](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_a_single_property_1c)]

Это позволяет получить доступ к сведениям о свойствах для любого свойства, независимо от его типа, за счет использования типов значений упаковки. Например:

<!--
            object blog = context.Blogs.Single(e => e.Id == 1);

            object currentValue = context.Entry(blog).Property("Name").CurrentValue;
            context.Entry(blog).Property("Name").CurrentValue = "1unicorn2";
-->
[!code-csharp[Work_with_a_single_property_1e](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_a_single_property_1e)]

В следующей таблице приведены сведения о свойствах, предоставляемых PropertyEntry.

| PropertyEntry, элемент                               | Описание
|:-------------------------------------------------|----------------------
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry%602.CurrentValue?displayProperty=nameWithType>  | Возвращает и задает текущее значение свойства.
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry%602.OriginalValue?displayProperty=nameWithType> | Возвращает и задает исходное значение свойства, если оно доступно.
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry%602.EntityEntry?displayProperty=nameWithType>   | Обратная ссылка на объект <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601> для сущности.
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.Metadata?displayProperty=nameWithType>          | <xref:Microsoft.EntityFrameworkCore.Metadata.IProperty> метаданные для свойства.
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.IsModified?displayProperty=nameWithType>        | Указывает, помечено ли это свойство как измененное и допускает изменение этого состояния.
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.IsTemporary?displayProperty=nameWithType>       | Указывает, помечено ли это свойство как [временное](xref:core/change-tracking/miscellaneous#temporary-values#temporary-values), и позволяет изменить это состояние.

Примечания.

- Исходное значение свойства — это значение, которое имело свойство при запросе сущности из базы данных. Однако исходные значения недоступны, если сущность была отключена, а затем явно присоединена к другой DbContext, например с `Attach` или `Update` . В этом случае возвращенное исходное значение будет таким же, как текущее значение.
- <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> будет обновлять только свойства, помеченные как измененные. Задайте значение <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.IsModified> true, чтобы принудительно обновить данное значение свойства EF Core или присвоить ему значение false, чтобы запретить EF Core обновления значения свойства.
- [Временные значения](xref:core/change-tracking/miscellaneous) обычно создаются [генераторами значений](xref:core/modeling/generated-properties)EF Core. Установка текущего значения свойства заменит временное значение указанным значением и помечает свойство как невременное. Задайте <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.IsTemporary> для параметра значение true, чтобы принудительно использовать временной даже после его явного задания.

### <a name="working-with-a-single-navigation"></a>Работа с одной навигацией

Несколько перегрузок <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601.Reference%2A?displayProperty=nameWithType> , <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601.Collection%2A?displayProperty=nameWithType> и <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Navigation%2A?displayProperty=nameWithType> предоставляют доступ к сведениям об отдельных переходах.

Доступ к ссылочным переходам к одной связанной сущности осуществляется с помощью <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601.Reference%2A> методов. Навигационные ссылки указывают на сторону «один» связей «один ко многим» и обе стороны связей «один к одному». Например:

<!--
        ReferenceEntry<Post, Blog> referenceEntry1 = context.Entry(post).Reference(e => e.Blog);
        ReferenceEntry<Post, Blog> referenceEntry2 = context.Entry(post).Reference<Blog>("Blog");
        ReferenceEntry referenceEntry3 = context.Entry(post).Reference("Blog");
-->
[!code-csharp[Work_with_a_single_navigation_1](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_a_single_navigation_1)]

Переходы также могут быть коллекциями связанных сущностей при использовании для связей "многие" между "один ко многим" и "многие ко многим". <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601.Collection%2A>Методы используются для доступа к навигации по коллекциям. Например:

<!--
        CollectionEntry<Blog, Post> collectionEntry1 = context.Entry(blog).Collection(e => e.Posts);
        CollectionEntry<Blog, Post> collectionEntry2 = context.Entry(blog).Collection<Post>("Posts");
        CollectionEntry collectionEntry3 = context.Entry(blog).Collection("Posts");
-->
[!code-csharp[Work_with_a_single_navigation_2a](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_a_single_navigation_2a)]

Некоторые операции являются общими для всех переходов. Они доступны для навигации по ссылке и коллекции с помощью <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Navigation%2A?displayProperty=nameWithType> метода. Обратите внимание, что при доступе ко всем переходам доступны только неуниверсальный доступ. Например:

<!--
        NavigationEntry navigationEntry = context.Entry(blog).Navigation("Posts");
-->
[!code-csharp[Work_with_a_single_navigation_2b](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_a_single_navigation_2b)]

В следующей таблице приведены способы использования <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ReferenceEntry%602> , <xref:Microsoft.EntityFrameworkCore.ChangeTracking.CollectionEntry%602> и <xref:Microsoft.EntityFrameworkCore.ChangeTracking.NavigationEntry> :

| Навигатионентри, элемент                                                                                    | Описание
|:----------------------------------------------------------------------------------------------------------|----------------------
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.MemberEntry.CurrentValue?displayProperty=nameWithType> | Возвращает и задает текущее значение навигации. Это вся коллекция для навигации по коллекциям.
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.NavigationEntry.Metadata?displayProperty=nameWithType> | <xref:Microsoft.EntityFrameworkCore.Metadata.INavigationBase> метаданные для навигации.
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.NavigationEntry.IsLoaded?displayProperty=nameWithType> | Возвращает или задает значение, указывающее, была ли связанная сущность или коллекция полностью загружены из базы данных.
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.NavigationEntry.Load?displayProperty=nameWithType>     | Загружает связанную сущность или коллекцию из базы данных; см. раздел [явная загрузка связанных данных](xref:core/querying/related-data/explicit).
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.NavigationEntry.Query?displayProperty=nameWithType>    | Запрос EF Core будет использовать для загрузки этой навигации в виде `IQueryable` , который может составляться дальше; см. [явную загрузку связанных данных](xref:core/querying/related-data/explicit).

### <a name="working-with-all-properties-of-an-entity"></a>Работа со всеми свойствами сущности

<xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Properties?displayProperty=nameWithType> Возвращает объект <xref:System.Collections.Generic.IEnumerable%601> <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry> для каждого свойства сущности. Это можно использовать для выполнения действия для каждого свойства сущности. Например, чтобы задать для любого свойства DateTime значение `DateTime.Now` :

<!--
        foreach (var propertyEntry in context.Entry(blog).Properties)
        {
            if (propertyEntry.Metadata.ClrType == typeof(DateTime))
            {
                propertyEntry.CurrentValue = DateTime.Now;
            }
        }
-->
[!code-csharp[Work_with_all_properties_of_an_entity_1](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_all_properties_of_an_entity_1)]

Кроме того, Ентитентри содержит несколько методов для получения и установки всех значений свойств одновременно. Эти методы используют <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyValues> класс, представляющий коллекцию свойств и их значений. Пропертивалуес можно получить для текущих или исходных значений или для значений, хранимых в настоящее время в базе данных. Например:

<!--
        var currentValues = context.Entry(blog).CurrentValues;
        var originalValues = context.Entry(blog).OriginalValues;
        var databaseValues = context.Entry(blog).GetDatabaseValues();
-->
[!code-csharp[Work_with_all_properties_of_an_entity_2a](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_all_properties_of_an_entity_2a)]

Эти объекты Пропертивалуес не очень полезны. Однако их можно комбинировать для выполнения общих операций, необходимых при управлении сущностями. Это полезно при работе с объектами перемещения данных и при разрешении [конфликтов оптимистичного параллелизма](xref:core/saving/concurrency). В следующих разделах приведены некоторые примеры.

#### <a name="setting-current-or-original-values-from-an-entity-or-dto"></a>Установка текущих или исходных значений из сущности или DTO

Текущие или исходные значения сущности могут быть обновлены путем копирования значений из другого объекта. Например, рассмотрим `BlogDto` объект передачи данных (DTO) с теми же свойствами, что и для типа сущности:

<!--
public class BlogDto
{
    public int Id { get; set; }
    public string Name { get; set; }
}
-->
[!code-csharp[BlogDto](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=BlogDto)]

Это можно использовать для задания текущих значений отслеживающей сущности с помощью <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyValues.SetValues%2A?displayProperty=nameWithType> :

<!--
        var blogDto = new BlogDto { Id = 1, Name = "1unicorn2" };

        context.Entry(blog).CurrentValues.SetValues(blogDto);
-->
[!code-csharp[Work_with_all_properties_of_an_entity_2b](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_all_properties_of_an_entity_2b)]

Этот метод иногда используется при обновлении сущности со значениями, полученными от вызова службы или клиентом в n-уровневых приложении. Обратите внимание, что используемый объект не должен быть того же типа, что и сущность, пока он имеет свойства, имена которых совпадают с именами сущности. В приведенном выше примере экземпляр DTO `BlogDto` используется для установки текущих значений отслеживающей `Blog` сущности.

Обратите внимание, что свойства будут помечаться как измененные, только если значение отличается от текущего значения.

#### <a name="setting-current-or-original-values-from-a-dictionary"></a>Установка текущих или исходных значений из словаря

В предыдущем примере задаются значения из экземпляра сущности или объекта DTO. Такое же поведение доступно, когда значения свойств хранятся в словаре как пары "имя-значение". Например:

<!--
        var blogDictionary = new Dictionary<string, object>
        {
            ["Id"] = 1,
            ["Name"] = "1unicorn2"
        };

        context.Entry(blog).CurrentValues.SetValues(blogDictionary);
-->
[!code-csharp[Work_with_all_properties_of_an_entity_2d](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_all_properties_of_an_entity_2d)]

#### <a name="setting-current-or-original-values-from-the-database"></a>Установка текущих или исходных значений из базы данных

Текущие или исходные значения сущности могут быть обновлены с использованием последних значений из базы данных путем вызова метода <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.GetDatabaseValues> или <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.GetDatabaseValuesAsync%2A> и использования возвращенного объекта для задания текущих или исходных значений или и того, и другого. Например:

<!--
        var databaseValues = context.Entry(blog).GetDatabaseValues();
        context.Entry(blog).CurrentValues.SetValues(databaseValues);
        context.Entry(blog).OriginalValues.SetValues(databaseValues);
-->
[!code-csharp[Work_with_all_properties_of_an_entity_2c](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_all_properties_of_an_entity_2c)]

#### <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a>Создание клонированного объекта, содержащего текущие, исходные значения или базы данных

Объект Пропертивалуес, возвращаемый из Куррентвалуес, Оригиналвалуес или Жетдатабасевалуес, можно использовать для создания клона сущности с помощью <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyValues.ToObject?displayProperty=nameWithType> . Например:

<!--
var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
-->
[!code-csharp[Work_with_all_properties_of_an_entity_2e](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_all_properties_of_an_entity_2e)]

Обратите внимание, что `ToObject` возвращает новый экземпляр, который не следит за DbContext. Возвращаемый объект также не имеет связей, настроенных для других сущностей.

Клонированный объект может быть полезен для устранения проблем, связанных с параллельными обновлениями базы данных, особенно при привязке данных к объектам определенного типа. Дополнительные сведения см. в разделе [оптимистичный параллелизм](xref:core/saving/concurrency) .

### <a name="working-with-all-navigations-of-an-entity"></a>Работа со всеми навигациями сущности

<xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Navigations?displayProperty=nameWithType> Возвращает объект <xref:System.Collections.Generic.IEnumerable%601> <xref:Microsoft.EntityFrameworkCore.ChangeTracking.NavigationEntry> для каждой навигации по сущности. <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.References?displayProperty=nameWithType> и <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Collections?displayProperty=nameWithType> выполняют те же действия, но ограничены ссылками или навигациями по коллекциям соответственно. Это можно использовать для выполнения действий при каждой навигации по сущности. Например, для принудительной загрузки всех связанных сущностей:

<!--
        foreach (var navigationEntry in context.Entry(blog).Navigations)
        {
            navigationEntry.Load();
        }
-->
[!code-csharp[Work_with_all_navigations_of_an_entity_1](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_all_navigations_of_an_entity_1)]

### <a name="working-with-all-members-of-an-entity"></a>Работа со всеми членами сущности

Обычные свойства и свойства навигации имеют разное состояние и поведение. Таким образом, обычно обработка переходов и несамостоятельных переходов выполняется отдельно, как показано в приведенных выше разделах. Однако иногда может оказаться полезным сделать что-то с любым членом сущности, независимо от того, является ли он обычным свойством или навигацией. <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Member%2A?displayProperty=nameWithType> и <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Members?displayProperty=nameWithType> предоставляются для этой цели. Например:

<!--
        foreach (var memberEntry in context.Entry(blog).Members)
        {
            Console.WriteLine(
                $"Member {memberEntry.Metadata.Name} is of type {memberEntry.Metadata.ClrType.ShortDisplayName()} and has value {memberEntry.CurrentValue}");
        }
-->
[!code-csharp[Work_with_all_members_of_an_entity_1](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_all_members_of_an_entity_1)]

При выполнении этого кода в блоге из примера создаются следующие выходные данные:

```output
Member Id is of type int and has value 1
Member Name is of type string and has value .NET Blog
Member Posts is of type IList<Post> and has value System.Collections.Generic.List`1[Post]
```

> [!TIP]
> В [представлении отладки "средство записи изменений](xref:core/change-tracking/debug-views) " отображаются такие сведения. Представление Отладка для всего средства записи изменений создается на основе отдельных объектов <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.DebugView?displayProperty=nameWithType> каждой записи.

## <a name="find-and-findasync"></a>Поиск и FindAsync

<xref:Microsoft.EntityFrameworkCore.DbContext.Find%2A?displayProperty=nameWithType>, <xref:Microsoft.EntityFrameworkCore.DbContext.FindAsync%2A?displayProperty=nameWithType> , <xref:Microsoft.EntityFrameworkCore.DbSet%601.Find%2A?displayProperty=nameWithType> и <xref:Microsoft.EntityFrameworkCore.DbSet%601.FindAsync%2A?displayProperty=nameWithType> предназначены для эффективного поиска одной сущности, если ее первичный ключ известен. Найти сначала проверяет, была ли сущность уже отслеживанием, и, если это так, возвращает сущность немедленно. Запрос к базе данных выполняется только в том случае, если сущность не была записана локально. Например, рассмотрим следующий код, который вызывает метод Find дважды для одной и той же сущности:

<!--
        using var context = new BlogsContext();

        Console.WriteLine("First call to Find...");
        var blog1 = context.Blogs.Find(1);

        Console.WriteLine($"...found blog {blog1.Name}");

        Console.WriteLine();
        Console.WriteLine("Second call to Find...");
        var blog2 = context.Blogs.Find(1);
        Debug.Assert(blog1 == blog2);

        Console.WriteLine("...returned the same instance without executing a query.");
-->
[!code-csharp[Find_and_FindAsync_1](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Find_and_FindAsync_1)]

Выходные данные этого кода (включая ведение журнала EF Core) при использовании SQLite:

```output
First call to Find...
info: 12/29/2020 07:45:53.682 RelationalEventId.CommandExecuted[20101] (Microsoft.EntityFrameworkCore.Database.Command)
      Executed DbCommand (1ms) [Parameters=[@__p_0='1' (DbType = String)], CommandType='Text', CommandTimeout='30']
      SELECT "b"."Id", "b"."Name"
      FROM "Blogs" AS "b"
      WHERE "b"."Id" = @__p_0
      LIMIT 1
...found blog .NET Blog

Second call to Find...
...returned the same instance without executing a query.
```

Обратите внимание, что первый вызов не находит сущность локально, поэтому выполняет запрос к базе данных. И наоборот, второй вызов возвращает один и тот же экземпляр без запроса к базе данных, так как он уже отслеживается.

Функция Find возвращает значение null, если сущность с заданным ключом не отслеживанием локально и не существует в базе данных.

### <a name="composite-keys"></a>Составные ключи

Поиск также можно использовать с составными ключами. Например, рассмотрим `OrderLine` сущность с составным ключом, состоящим из идентификатора заказа и идентификатора продукта:

<!--
public class OrderLine
{
    public int OrderId { get; set; }
    public int ProductId { get; set; }

    //...
}
-->
[!code-csharp[OrderLine](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=OrderLine)]

Составной ключ должен быть настроен в <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A?displayProperty=nameWithType> для определения ключевых частей _и их порядка_. Например:

<!--
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder
            .Entity<OrderLine>()
            .HasKey(e => new { e.OrderId, e.ProductId });
    }
-->
[!code-csharp[OnModelCreating](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=OnModelCreating)]

Обратите внимание, что `OrderId` является первой частью ключа и `ProductId` является второй частью ключа. Этот порядок следует использовать при передаче ключевых значений для поиска. Например:

<!--
        var orderline = context.OrderLines.Find(orderId, productId);
-->
[!code-csharp[Find_and_FindAsync_2](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Find_and_FindAsync_2)]

## <a name="using-changetrackerentries-to-access-all-tracked-entities"></a>Использование ChangeTracker. записи для доступа ко всем отслеживающим сущностям

Пока мы получили доступ только к одному объекту <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry> за раз. <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Entries?displayProperty=nameWithType> Возвращает Ентитентри для каждой сущности, которая в настоящее время отслеживании DbContext. Например:

<!--
        using var context = new BlogsContext();
        var blogs = context.Blogs.Include(e => e.Posts).ToList();

        foreach (var entityEntry in context.ChangeTracker.Entries())
        {
            Console.WriteLine($"Found {entityEntry.Metadata.Name} entity with ID {entityEntry.Property("Id").CurrentValue}");
        }
-->
[!code-csharp[Using_ChangeTracker_Entries_to_access_all_tracked_entities_1a](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Using_ChangeTracker_Entries_to_access_all_tracked_entities_1a)]

Этот код создаст следующие выходные данные:

```output
Found Blog entity with ID 1
Found Post entity with ID 1
Found Post entity with ID 2
```

Обратите внимание, что возвращаются записи для блогов и записей. Вместо этого результаты можно отфильтровать по конкретному типу сущности с помощью <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Entries%60%601?displayProperty=nameWithType> универсальной перегрузки:

<!--
        foreach (var entityEntry in context.ChangeTracker.Entries<Post>())
        {
            Console.WriteLine(
                $"Found {entityEntry.Metadata.Name} entity with ID {entityEntry.Property(e => e.Id).CurrentValue}");
        }
-->
[!code-csharp[Using_ChangeTracker_Entries_to_access_all_tracked_entities_1b](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Using_ChangeTracker_Entries_to_access_all_tracked_entities_1b)]

Выходные данные этого кода показывают, что возвращаются только записи.

```output
Found Post entity with ID 1
Found Post entity with ID 2
```

Кроме того, при использовании универсальной перегрузки возвращаются универсальные <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601> экземпляры. Это позволяет обеспечить доступ к `Id` свойству в этом примере в свободном стиле.

Универсальный тип, используемый для фильтрации, не обязательно должен быть сопоставленным типом сущности. Вместо этого можно использовать Несопоставленный базовый тип или интерфейс. Например, если все типы сущностей в модели реализуют интерфейс, определяющий его свойство ключа:

<!--
public interface IEntityWithKey
{
    int Id { get; set; }
}
-->
[!code-csharp[IEntityWithKey](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=IEntityWithKey)]

Затем этот интерфейс можно использовать для работы с ключом любой отслеживающей сущности строго типизированным способом. Например:

<!--
        foreach (var entityEntry in context.ChangeTracker.Entries<IEntityWithKey>())
        {
            Console.WriteLine(
                $"Found {entityEntry.Metadata.Name} entity with ID {entityEntry.Property(e => e.Id).CurrentValue}");
        }
-->
[!code-csharp[Using_ChangeTracker_Entries_to_access_all_tracked_entities_1c](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Using_ChangeTracker_Entries_to_access_all_tracked_entities_1c)]

## <a name="using-dbsetlocal-to-query-tracked-entities"></a>Использование DbSet. local для отслеживания запросов сущностей

EF Core запросы всегда выполняются в базе данных и возвращают только те сущности, которые были сохранены в базе данных. <xref:Microsoft.EntityFrameworkCore.DbSet%601.Local?displayProperty=nameWithType> предоставляет механизм для запроса DbContext для локальных, отслеживаний сущностей.

Так как `DbSet.Local` используется для запроса отслеживаний сущностей, обычно для загрузки сущностей в DbContext и последующей работы с загруженными сущностями. Это особенно справедливо для привязки данных, но также может быть полезным в других ситуациях. Например, в следующем коде база данных сначала запрашивается для всех блогов и записей. <xref:Microsoft.EntityFrameworkCore.EntityFrameworkQueryableExtensions.Load%2A>Метод расширения используется для выполнения этого запроса с результатами, отслеживаемыми контекстом, без возврата непосредственно в приложение. (Использование `ToList` или аналогичное действие имеет тот же результат, но с издержками на создание возвращенного списка, что здесь не требуется.) Затем в примере используется `DbSet.Local` для доступа к локально отслеживающимся сущностям:

<!--
        using var context = new BlogsContext();

        context.Blogs.Include(e => e.Posts).Load();

        foreach (var blog in context.Blogs.Local)
        {
            Console.WriteLine($"Blog: {blog.Name}");
        }

        foreach (var post in context.Posts.Local)
        {
            Console.WriteLine($"Post: {post.Title}");
        }
-->
[!code-csharp[Using_DbSet_Local_to_query_tracked_entities_1](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Using_DbSet_Local_to_query_tracked_entities_1)]

Обратите внимание, что, в отличие от <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Entries?displayProperty=nameWithType> , `DbSet.Local` возвращает экземпляры сущности напрямую. Конечно, Ентитентри можно всегда получать для возвращаемой сущности путем вызова метода <xref:System.Data.Entity.DbContext.Entry%2A?displayProperty=nameWithType> .

### <a name="the-local-view"></a>Локальное представление

<xref:Microsoft.EntityFrameworkCore.DbSet%601.Local?displayProperty=nameWithType> Возвращает представление локально отслеживаний сущностей, отражающих текущую <xref:Microsoft.EntityFrameworkCore.EntityState> сущность. В частности, это означает, что:

- `Added` сущности включены. Обратите внимание, что это не так для обычных запросов EF Core, так как `Added` сущности еще не существуют в базе данных и поэтому никогда не возвращаются запросом к базе данных.
- `Deleted` сущности исключаются. Обратите внимание, что это еще не так для обычных запросов EF Core, так как `Deleted` сущности по-прежнему существуют в  базе данных и возвращаются запросами к базе данных.

Это означает, что `DbSet.Local` это представление данных, отражающее текущее концептуальное состояние графа сущностей с `Added` включенными сущностями и `Deleted` исключенными сущностями. Это соответствует состоянию базы данных, которое должно быть после вызова SaveChanges.

Обычно это идеальное представление для привязки данных, так как оно представляет пользователю данные по мере их понимания на основе изменений, внесенных приложением.

В следующем примере кода показано, как пометить одну запись как, `Deleted` а затем добавить новую запись, пометив ее как `Added` :

<!--
        using var context = new BlogsContext();

        var posts = context.Posts.Include(e => e.Blog).ToList();

        Console.WriteLine("Local view after loading posts:");

        foreach (var post in context.Posts.Local)
        {
            Console.WriteLine($"  Post: {post.Title}");
        }

        context.Remove(posts[1]);

        context.Add(new Post
        {
            Title = "What’s next for System.Text.Json?",
            Content = ".NET 5.0 was released recently and has come with many...",
            Blog = posts[0].Blog
        });

        Console.WriteLine("Local view after adding and deleting posts:");

        foreach (var post in context.Posts.Local)
        {
            Console.WriteLine($"  Post: {post.Title}");
        }
-->
[!code-csharp[Using_DbSet_Local_to_query_tracked_entities_2](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Using_DbSet_Local_to_query_tracked_entities_2)]

Выходные данные этого кода:

```output
Local view after loading posts:
  Post: Announcing the Release of EF Core 5.0
  Post: Announcing F# 5
  Post: Announcing .NET 5.0
Local view after adding and deleting posts:
  Post: What’s next for System.Text.Json?
  Post: Announcing the Release of EF Core 5.0
  Post: Announcing .NET 5.0
```

Обратите внимание, что удаленная запись удаляется из локального представления, а также включается добавленная запись.

### <a name="using-local-to-add-and-remove-entities"></a>Использование Local для добавления и удаления сущностей

<xref:Microsoft.EntityFrameworkCore.DbSet%601.Local?displayProperty=nameWithType> возвращает экземпляр <xref:Microsoft.EntityFrameworkCore.ChangeTracking.LocalView%601>. Это реализация <xref:System.Collections.Generic.ICollection%601> , которая создает и реагирует на уведомления при добавлении и удалении сущностей из коллекции. (Это та же концепция, что <xref:System.Collections.ObjectModel.ObservableCollection%601> и, но реализованная как проекция поверх существующих EF Core записей отслеживания изменений, а не как независимая коллекция.)

Уведомления локального представления подключаются к DbContext отслеживания изменений, так что локальное представление остается синхронизированным с DbContext. В частности, внесены следующие изменения.

- Добавление новой сущности приводит к `DbSet.Local` тому, что она будет относиться к DbContext, обычно в `Added` состоянии. (Если сущность уже имеет созданное значение ключа, он будет записан как `Unchanged` .)
- Удаление сущности из `DbSet.Local` приводит к тому, что она будет помечена как `Deleted` .
- Сущность, которая будет отслеживанием DbContext, будет автоматически отображаться в `DbSet.Local` коллекции. Например, выполнение запроса для большего числа сущностей автоматически приводит к обновлению локального представления.
- Сущность, помеченная как, `Deleted` будет автоматически удалена из локальной коллекции.

Это означает, что локальное представление можно использовать для управления отслеживающими сущностями, просто путем добавления и удаления из коллекции. Например, позволяет изменить предыдущий пример кода для добавления и удаления записей из локальной коллекции:

<!--
        using var context = new BlogsContext();

        var posts = context.Posts.Include(e => e.Blog).ToList();

        Console.WriteLine("Local view after loading posts:");

        foreach (var post in context.Posts.Local)
        {
            Console.WriteLine($"  Post: {post.Title}");
        }

        context.Posts.Local.Remove(posts[1]);

        context.Posts.Local.Add(new Post
        {
            Title = "What’s next for System.Text.Json?",
            Content = ".NET 5.0 was released recently and has come with many...",
            Blog = posts[0].Blog
        });

        Console.WriteLine("Local view after adding and deleting posts:");

        foreach (var post in context.Posts.Local)
        {
            Console.WriteLine($"  Post: {post.Title}");
        }
-->
[!code-csharp[Using_DbSet_Local_to_query_tracked_entities_3](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Using_DbSet_Local_to_query_tracked_entities_3)]

Выходные данные не изменяются из предыдущего примера, так как изменения, вносимые в локальное представление, синхронизируются с DbContext.

### <a name="using-the-local-view-for-windows-forms-or-wpf-data-binding"></a>Использование локального представления для Windows Forms или привязки данных WPF

<xref:Microsoft.EntityFrameworkCore.DbSet%601.Local?displayProperty=nameWithType> формирует базу для привязки данных к EF Core сущностям. Однако как Windows Forms, так и WPF работают наилучшим образом при использовании с конкретным типом уведомления о предполагаемой коллекции. Локальное представление поддерживает создание следующих типов коллекций:

- <xref:Microsoft.EntityFrameworkCore.ChangeTracking.LocalView%601.ToObservableCollection?displayProperty=nameWithType> Возвращает <xref:System.Collections.ObjectModel.ObservableCollection%601> для привязки данных WPF.
- <xref:Microsoft.EntityFrameworkCore.ChangeTracking.LocalView%601.ToBindingList?displayProperty=nameWithType> Возвращает <xref:System.ComponentModel.BindingList%601> для Windows Forms привязки данных.

Например:

<!--
        ObservableCollection<Post> observableCollection = context.Posts.Local.ToObservableCollection();
        BindingList<Post> bindingList = context.Posts.Local.ToBindingList();
-->
[!code-csharp[Using_DbSet_Local_to_query_tracked_entities_4](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Using_DbSet_Local_to_query_tracked_entities_4)]

Дополнительные сведения о привязке данных WPF с EF Core см. в статье Начало [работы с WPF](xref:core/get-started/wpf) .

> [!TIP]
> Локальное представление для данного экземпляра DbSet создается отложенным при первом доступе, а затем кэшируется. Создание Локалвиев выполняется быстро и не использует значительный объем памяти. Однако он вызывает [DetectChanges](xref:core/change-tracking/change-detection), что может быть достаточно длительным для большого числа сущностей. Коллекции, созданные `ToObservableCollection` и, `ToBindingList` также создаются отложенно, а затем кэшируются. Оба эти метода создают новые коллекции, которые могут работать достаточно долго и использовать большой объем памяти при использовании тысяч сущностей.
