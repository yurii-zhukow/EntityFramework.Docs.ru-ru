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
# <a name="accessing-tracked-entities"></a><span data-ttu-id="ca23d-103">Доступ к отслеживающим сущностям</span><span class="sxs-lookup"><span data-stu-id="ca23d-103">Accessing Tracked Entities</span></span>

<span data-ttu-id="ca23d-104">Существует четыре основных API для доступа к сущностям, которые отправляются <xref:Microsoft.EntityFrameworkCore.DbContext> :</span><span class="sxs-lookup"><span data-stu-id="ca23d-104">There are four main APIs for accessing entities tracked by a <xref:Microsoft.EntityFrameworkCore.DbContext>:</span></span>

- <span data-ttu-id="ca23d-105"><xref:System.Data.Entity.DbContext.Entry%2A?displayProperty=nameWithType> Возвращает <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601> экземпляр для заданного экземпляра сущности.</span><span class="sxs-lookup"><span data-stu-id="ca23d-105"><xref:System.Data.Entity.DbContext.Entry%2A?displayProperty=nameWithType> returns an <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601> instance for a given entity instance.</span></span>
- <span data-ttu-id="ca23d-106"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Entries%2A?displayProperty=nameWithType> Возвращает <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601> экземпляры для всех отслеживаний сущностей или для всех отслеживаниющих сущностей заданного типа.</span><span class="sxs-lookup"><span data-stu-id="ca23d-106"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Entries%2A?displayProperty=nameWithType> returns <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601> instances for all tracked entities, or for all tracked entities of a given type.</span></span>
- <span data-ttu-id="ca23d-107"><xref:Microsoft.EntityFrameworkCore.DbContext.Find%2A?displayProperty=nameWithType>, <xref:Microsoft.EntityFrameworkCore.DbContext.FindAsync%2A?displayProperty=nameWithType> , <xref:Microsoft.EntityFrameworkCore.DbSet%601.Find%2A?displayProperty=nameWithType> и <xref:Microsoft.EntityFrameworkCore.DbSet%601.FindAsync%2A?displayProperty=nameWithType> найдите отдельную сущность по первичному ключу, сначала просматривая отслеживание сущностей, а затем запросите базу данных при необходимости.</span><span class="sxs-lookup"><span data-stu-id="ca23d-107"><xref:Microsoft.EntityFrameworkCore.DbContext.Find%2A?displayProperty=nameWithType>, <xref:Microsoft.EntityFrameworkCore.DbContext.FindAsync%2A?displayProperty=nameWithType>, <xref:Microsoft.EntityFrameworkCore.DbSet%601.Find%2A?displayProperty=nameWithType>, and <xref:Microsoft.EntityFrameworkCore.DbSet%601.FindAsync%2A?displayProperty=nameWithType> find a single entity by primary key, first looking in tracked entities, and then querying the database if needed.</span></span>
- <span data-ttu-id="ca23d-108"><xref:Microsoft.EntityFrameworkCore.DbSet%601.Local?displayProperty=nameWithType> Возвращает фактические сущности (не Ентитентри экземпляры) для сущностей типа сущности, представленного DbSet.</span><span class="sxs-lookup"><span data-stu-id="ca23d-108"><xref:Microsoft.EntityFrameworkCore.DbSet%601.Local?displayProperty=nameWithType> returns actual entities (not EntityEntry instances) for entities of the entity type represented by the DbSet.</span></span>

<span data-ttu-id="ca23d-109">Каждый из них подробно описан в следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="ca23d-109">Each of these is described in more detail in the sections below.</span></span>

> [!TIP]
> <span data-ttu-id="ca23d-110">В этом документе предполагается, что состояния сущностей и основы отслеживания изменений EF Core понятны.</span><span class="sxs-lookup"><span data-stu-id="ca23d-110">This document assumes that entity states and the basics of EF Core change tracking are understood.</span></span> <span data-ttu-id="ca23d-111">Дополнительные сведения по этим темам см. [в разделе Отслеживание изменений в EF Core](xref:core/change-tracking/index) .</span><span class="sxs-lookup"><span data-stu-id="ca23d-111">See [Change Tracking in EF Core](xref:core/change-tracking/index) for more information on these topics.</span></span>

> [!TIP]
> <span data-ttu-id="ca23d-112">Вы можете запустить и отладить весь код, используемый в этой документации, [скачав пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/ChangeTracking/AccessingTrackedEntities).</span><span class="sxs-lookup"><span data-stu-id="ca23d-112">You can run and debug into all the code in this document by [downloading the sample code from GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/ChangeTracking/AccessingTrackedEntities).</span></span>

## <a name="using-dbcontextentry-and-entityentry-instances"></a><span data-ttu-id="ca23d-113">Использование DbContext. Entry и экземпляров Ентитентри</span><span class="sxs-lookup"><span data-stu-id="ca23d-113">Using DbContext.Entry and EntityEntry instances</span></span>

<span data-ttu-id="ca23d-114">Для каждой отслеживающая сущность Entity Framework Core (EF Core) отслеживает:</span><span class="sxs-lookup"><span data-stu-id="ca23d-114">For each tracked entity, Entity Framework Core (EF Core) keeps track of:</span></span>

- <span data-ttu-id="ca23d-115">Общее состояние сущности.</span><span class="sxs-lookup"><span data-stu-id="ca23d-115">The overall state of the entity.</span></span> <span data-ttu-id="ca23d-116">Это один из `Unchanged` , `Modified` , `Added` или `Deleted` ; Дополнительные сведения см. [в разделе Отслеживание изменений в EF Core](xref:core/change-tracking/index) .</span><span class="sxs-lookup"><span data-stu-id="ca23d-116">This is one of `Unchanged`, `Modified`, `Added`, or `Deleted`; see [Change Tracking in EF Core](xref:core/change-tracking/index) for more information.</span></span>
- <span data-ttu-id="ca23d-117">Связи между отслеживающими сущностями.</span><span class="sxs-lookup"><span data-stu-id="ca23d-117">The relationships between tracked entities.</span></span> <span data-ttu-id="ca23d-118">Например, блог, которому принадлежит запись.</span><span class="sxs-lookup"><span data-stu-id="ca23d-118">For example, the blog to which a post belongs.</span></span>
- <span data-ttu-id="ca23d-119">Текущие значения свойств.</span><span class="sxs-lookup"><span data-stu-id="ca23d-119">The "current values" of properties.</span></span>
- <span data-ttu-id="ca23d-120">Исходные значения свойств, когда эти сведения доступны.</span><span class="sxs-lookup"><span data-stu-id="ca23d-120">The "original values" of properties, when this information is available.</span></span> <span data-ttu-id="ca23d-121">Исходные значения — это значения свойств, которые существовали при запросе сущности из базы данных.</span><span class="sxs-lookup"><span data-stu-id="ca23d-121">Original values are the property values that existed when entity was queried from the database.</span></span>
- <span data-ttu-id="ca23d-122">Значения свойств, которые были изменены с момента запроса.</span><span class="sxs-lookup"><span data-stu-id="ca23d-122">Which property values have been modified since they were queried.</span></span>
- <span data-ttu-id="ca23d-123">Другие сведения о значениях свойств, например о том, является ли значение [временным](xref:core/change-tracking/miscellaneous#temporary-values).</span><span class="sxs-lookup"><span data-stu-id="ca23d-123">Other information about property values, such as whether or not the value is [temporary](xref:core/change-tracking/miscellaneous#temporary-values).</span></span>

<span data-ttu-id="ca23d-124">Передача экземпляра сущности в <xref:System.Data.Entity.DbContext.Entry%2A?displayProperty=nameWithType> результате <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601> предоставления доступа к этим сведениям для данной сущности.</span><span class="sxs-lookup"><span data-stu-id="ca23d-124">Passing an entity instance to <xref:System.Data.Entity.DbContext.Entry%2A?displayProperty=nameWithType> results in an <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601> providing access to this information for the given entity.</span></span> <span data-ttu-id="ca23d-125">Например:</span><span class="sxs-lookup"><span data-stu-id="ca23d-125">For example:</span></span>

<!--
        using var context = new BlogsContext();

        var blog = context.Blogs.Single(e => e.Id == 1);
        var entityEntry = context.Entry(blog);

-->
[!code-csharp[Using_DbContext_Entry_and_EntityEntry_instances_1](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Using_DbContext_Entry_and_EntityEntry_instances_1)]

<span data-ttu-id="ca23d-126">В следующих разделах показано, как использовать Ентитентри для доступа к состоянию сущности и управления им, а также состоянием свойств и переходов сущности.</span><span class="sxs-lookup"><span data-stu-id="ca23d-126">The following sections show how to use an EntityEntry to access and manipulate entity state, as well as the state of the entity's properties and navigations.</span></span>

### <a name="working-with-the-entity"></a><span data-ttu-id="ca23d-127">Работа с сущностью</span><span class="sxs-lookup"><span data-stu-id="ca23d-127">Working with the entity</span></span>

<span data-ttu-id="ca23d-128">Наиболее часто используется <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601> для доступа к текущей <xref:Microsoft.EntityFrameworkCore.EntityState> сущности.</span><span class="sxs-lookup"><span data-stu-id="ca23d-128">The most common use of <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601> is to access the current <xref:Microsoft.EntityFrameworkCore.EntityState> of an entity.</span></span> <span data-ttu-id="ca23d-129">Например:</span><span class="sxs-lookup"><span data-stu-id="ca23d-129">For example:</span></span>

<!--
        var currentState = context.Entry(blog).State;
        if (currentState == EntityState.Unchanged)
        {
            context.Entry(blog).State = EntityState.Modified;
        }
-->
[!code-csharp[Work_with_the_entity_1](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_the_entity_1)]

<span data-ttu-id="ca23d-130">Метод Entry можно также использовать для сущностей, которые еще не были отслеживанием.</span><span class="sxs-lookup"><span data-stu-id="ca23d-130">The Entry method can also be used on entities that are not yet tracked.</span></span> <span data-ttu-id="ca23d-131">Это _не начинает отслеживание сущности_; состояние сущности по-прежнему равно `Detatched` .</span><span class="sxs-lookup"><span data-stu-id="ca23d-131">This _does not start tracking the entity_; the state of the entity is still `Detatched`.</span></span> <span data-ttu-id="ca23d-132">Однако возвращенный Ентитентри можно затем использовать для изменения состояния сущности, после чего сущность будет относиться к заданному состоянию.</span><span class="sxs-lookup"><span data-stu-id="ca23d-132">However, the returned EntityEntry can then be used to change the entity state, at which point the entity will become tracked in the given state.</span></span> <span data-ttu-id="ca23d-133">Например, следующий код начнет отслеживать экземпляр блога как `Added` :</span><span class="sxs-lookup"><span data-stu-id="ca23d-133">For example, the following code will start tracking a Blog instance as `Added`:</span></span>

<!--
        var newBlog = new Blog();
        Debug.Assert(context.Entry(newBlog).State == EntityState.Detached);

        context.Entry(newBlog).State = EntityState.Added;
        Debug.Assert(context.Entry(newBlog).State == EntityState.Added);
-->
[!code-csharp[Work_with_the_entity_2](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_the_entity_2)]

> [!TIP]
> <span data-ttu-id="ca23d-134">В отличие от EF6, установка состояния отдельной сущности не приведет к отслеживанию всех подключенных сущностей.</span><span class="sxs-lookup"><span data-stu-id="ca23d-134">Unlike in EF6, setting the state of an individual entity will not cause all connected entities to be tracked.</span></span> <span data-ttu-id="ca23d-135">Это делает установку состояния таким образом, что операция более низкого уровня не вызывает `Add` , `Attach` или `Update` , которая действует на весь граф сущностей.</span><span class="sxs-lookup"><span data-stu-id="ca23d-135">This makes setting the state this way a lower-level operation than calling `Add`, `Attach`, or `Update`, which operate on an entire graph of entities.</span></span>

<span data-ttu-id="ca23d-136">В следующей таблице приведены способы использования Ентитентри для работы с целой сущностью.</span><span class="sxs-lookup"><span data-stu-id="ca23d-136">The following table summarizes ways to use an EntityEntry to work with an entire entity:</span></span>

| <span data-ttu-id="ca23d-137">Ентитентри, элемент</span><span class="sxs-lookup"><span data-stu-id="ca23d-137">EntityEntry member</span></span>                                                                                         | <span data-ttu-id="ca23d-138">Описание</span><span class="sxs-lookup"><span data-stu-id="ca23d-138">Description</span></span>
|:-----------------------------------------------------------------------------------------------------------|----------------------
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.State?displayProperty=nameWithType>         | <span data-ttu-id="ca23d-139">Возвращает и задает объект <xref:Microsoft.EntityFrameworkCore.EntityState> сущности.</span><span class="sxs-lookup"><span data-stu-id="ca23d-139">Gets and sets the <xref:Microsoft.EntityFrameworkCore.EntityState> of the entity.</span></span>
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Entity?displayProperty=nameWithType>        | <span data-ttu-id="ca23d-140">Возвращает экземпляр сущности.</span><span class="sxs-lookup"><span data-stu-id="ca23d-140">Gets the entity instance.</span></span>
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Context?displayProperty=nameWithType>       | <span data-ttu-id="ca23d-141">Объект <xref:Microsoft.EntityFrameworkCore.DbContext> , отслеживающий эту сущность.</span><span class="sxs-lookup"><span data-stu-id="ca23d-141">The <xref:Microsoft.EntityFrameworkCore.DbContext> that is tracking this entity.</span></span>
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Metadata?displayProperty=nameWithType>      | <span data-ttu-id="ca23d-142"><xref:Microsoft.EntityFrameworkCore.Metadata.IEntityType> метаданные для типа сущности.</span><span class="sxs-lookup"><span data-stu-id="ca23d-142"><xref:Microsoft.EntityFrameworkCore.Metadata.IEntityType> metadata for the type of entity.</span></span>
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.IsKeySet?displayProperty=nameWithType>      | <span data-ttu-id="ca23d-143">Задано ли значение ключа для сущности.</span><span class="sxs-lookup"><span data-stu-id="ca23d-143">Whether or not the entity has had its key value set.</span></span>
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Reload?displayProperty=nameWithType>        | <span data-ttu-id="ca23d-144">Перезаписывает значения свойств значениями, считанными из базы данных.</span><span class="sxs-lookup"><span data-stu-id="ca23d-144">Overwrites property values with values read from the database.</span></span>
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.DetectChanges?displayProperty=nameWithType> | <span data-ttu-id="ca23d-145">Принудительное обнаружение изменений только для этой сущности; см. раздел [Обнаружение изменений и уведомления](xref:core/change-tracking/change-detection).</span><span class="sxs-lookup"><span data-stu-id="ca23d-145">Forces detection of changes for this entity only; see [Change Detection and Notifications](xref:core/change-tracking/change-detection).</span></span>

### <a name="working-with-a-single-property"></a><span data-ttu-id="ca23d-146">Работа с одним свойством</span><span class="sxs-lookup"><span data-stu-id="ca23d-146">Working with a single property</span></span>

<span data-ttu-id="ca23d-147">Несколько перегрузок <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601.Property%2A?displayProperty=nameWithType> предоставляют доступ к сведениям об отдельном свойстве сущности.</span><span class="sxs-lookup"><span data-stu-id="ca23d-147">Several overloads of <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601.Property%2A?displayProperty=nameWithType> allow access to information about an individual property of an entity.</span></span> <span data-ttu-id="ca23d-148">Например, с помощью строго типизированного API-интерфейса Fluent:</span><span class="sxs-lookup"><span data-stu-id="ca23d-148">For example, using a strongly-typed, fluent-like API:</span></span>

<!--
            PropertyEntry<Blog, string> propertyEntry = context.Entry(blog).Property(e => e.Name);
-->
[!code-csharp[Work_with_a_single_property_1a](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_a_single_property_1a)]

<span data-ttu-id="ca23d-149">Вместо этого имя свойства можно передать в виде строки.</span><span class="sxs-lookup"><span data-stu-id="ca23d-149">The property name can instead be passed as a string.</span></span> <span data-ttu-id="ca23d-150">Например:</span><span class="sxs-lookup"><span data-stu-id="ca23d-150">For example:</span></span>

<!--
            PropertyEntry<Blog, string> propertyEntry = context.Entry(blog).Property<string>("Name");
-->
[!code-csharp[Work_with_a_single_property_1b](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_a_single_property_1b)]

<span data-ttu-id="ca23d-151">Затем возвращаемый объект <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry%602> можно использовать для доступа к сведениям о свойстве.</span><span class="sxs-lookup"><span data-stu-id="ca23d-151">The returned <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry%602> can then be used to access information about the property.</span></span> <span data-ttu-id="ca23d-152">Например, его можно использовать для получения и задания текущего значения свойства для этой сущности:</span><span class="sxs-lookup"><span data-stu-id="ca23d-152">For example, it can be used to get and set the current value of the property on this entity:</span></span>

<!--
            string currentValue = context.Entry(blog).Property(e => e.Name).CurrentValue;
            context.Entry(blog).Property(e => e.Name).CurrentValue = "1unicorn2";
-->
[!code-csharp[Work_with_a_single_property_1d](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_a_single_property_1d)]

<span data-ttu-id="ca23d-153">Оба метода свойства, используемые выше, возвращают универсальный экземпляр со строгим типом <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry%602> .</span><span class="sxs-lookup"><span data-stu-id="ca23d-153">Both of the Property methods used above return a strongly-typed generic <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry%602> instance.</span></span> <span data-ttu-id="ca23d-154">Использование этого универсального типа является предпочтительным, так как обеспечивает доступ к значениям свойств без [упаковки типов значений](/dotnet/csharp/programming-guide/types/boxing-and-unboxing).</span><span class="sxs-lookup"><span data-stu-id="ca23d-154">Using this generic type is preferred because it allows access to property values without [boxing value types](/dotnet/csharp/programming-guide/types/boxing-and-unboxing).</span></span> <span data-ttu-id="ca23d-155">Однако если тип сущности или свойства неизвестен во время компиляции, <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry> вместо этого можно получить неуниверсальное значение.</span><span class="sxs-lookup"><span data-stu-id="ca23d-155">However, if the type of entity or property is not known at compile-time, then a non-generic <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry> can be obtained instead:</span></span>

<!--
            var propertyEntry = context.Entry(blog).Property("Name");
-->
[!code-csharp[Work_with_a_single_property_1c](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_a_single_property_1c)]

<span data-ttu-id="ca23d-156">Это позволяет получить доступ к сведениям о свойствах для любого свойства, независимо от его типа, за счет использования типов значений упаковки.</span><span class="sxs-lookup"><span data-stu-id="ca23d-156">This allows access to property information for any property regardless of its type, at the expense of boxing value types.</span></span> <span data-ttu-id="ca23d-157">Например:</span><span class="sxs-lookup"><span data-stu-id="ca23d-157">For example:</span></span>

<!--
            object blog = context.Blogs.Single(e => e.Id == 1);

            object currentValue = context.Entry(blog).Property("Name").CurrentValue;
            context.Entry(blog).Property("Name").CurrentValue = "1unicorn2";
-->
[!code-csharp[Work_with_a_single_property_1e](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_a_single_property_1e)]

<span data-ttu-id="ca23d-158">В следующей таблице приведены сведения о свойствах, предоставляемых PropertyEntry.</span><span class="sxs-lookup"><span data-stu-id="ca23d-158">The following table summarizes property information exposed by PropertyEntry:</span></span>

| <span data-ttu-id="ca23d-159">PropertyEntry, элемент</span><span class="sxs-lookup"><span data-stu-id="ca23d-159">PropertyEntry member</span></span>                               | <span data-ttu-id="ca23d-160">Описание</span><span class="sxs-lookup"><span data-stu-id="ca23d-160">Description</span></span>
|:-------------------------------------------------|----------------------
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry%602.CurrentValue?displayProperty=nameWithType>  | <span data-ttu-id="ca23d-161">Возвращает и задает текущее значение свойства.</span><span class="sxs-lookup"><span data-stu-id="ca23d-161">Gets and sets the current value of the property.</span></span>
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry%602.OriginalValue?displayProperty=nameWithType> | <span data-ttu-id="ca23d-162">Возвращает и задает исходное значение свойства, если оно доступно.</span><span class="sxs-lookup"><span data-stu-id="ca23d-162">Gets and sets the original value of the property, if available.</span></span>
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry%602.EntityEntry?displayProperty=nameWithType>   | <span data-ttu-id="ca23d-163">Обратная ссылка на объект <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601> для сущности.</span><span class="sxs-lookup"><span data-stu-id="ca23d-163">A back reference to the <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601> for the entity.</span></span>
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.Metadata?displayProperty=nameWithType>          | <span data-ttu-id="ca23d-164"><xref:Microsoft.EntityFrameworkCore.Metadata.IProperty> метаданные для свойства.</span><span class="sxs-lookup"><span data-stu-id="ca23d-164"><xref:Microsoft.EntityFrameworkCore.Metadata.IProperty> metadata for the property.</span></span>
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.IsModified?displayProperty=nameWithType>        | <span data-ttu-id="ca23d-165">Указывает, помечено ли это свойство как измененное и допускает изменение этого состояния.</span><span class="sxs-lookup"><span data-stu-id="ca23d-165">Indicates whether this property is marked as modified, and allows this state to be changed.</span></span>
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.IsTemporary?displayProperty=nameWithType>       | <span data-ttu-id="ca23d-166">Указывает, помечено ли это свойство как [временное](xref:core/change-tracking/miscellaneous#temporary-values#temporary-values), и позволяет изменить это состояние.</span><span class="sxs-lookup"><span data-stu-id="ca23d-166">Indicates whether this property is marked as [temporary](xref:core/change-tracking/miscellaneous#temporary-values#temporary-values), and allows this state to be changed.</span></span>

<span data-ttu-id="ca23d-167">Примечания.</span><span class="sxs-lookup"><span data-stu-id="ca23d-167">Notes:</span></span>

- <span data-ttu-id="ca23d-168">Исходное значение свойства — это значение, которое имело свойство при запросе сущности из базы данных.</span><span class="sxs-lookup"><span data-stu-id="ca23d-168">The original value of a property is the value that the property had when the entity was queried from the database.</span></span> <span data-ttu-id="ca23d-169">Однако исходные значения недоступны, если сущность была отключена, а затем явно присоединена к другой DbContext, например с `Attach` или `Update` .</span><span class="sxs-lookup"><span data-stu-id="ca23d-169">However, original values are not available if the entity was disconnected and then explicitly attached to another DbContext, for example with `Attach` or `Update`.</span></span> <span data-ttu-id="ca23d-170">В этом случае возвращенное исходное значение будет таким же, как текущее значение.</span><span class="sxs-lookup"><span data-stu-id="ca23d-170">In this case, the original value returned will be the same as the current value.</span></span>
- <span data-ttu-id="ca23d-171"><xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> будет обновлять только свойства, помеченные как измененные.</span><span class="sxs-lookup"><span data-stu-id="ca23d-171"><xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> will only update properties marked as modified.</span></span> <span data-ttu-id="ca23d-172">Задайте значение <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.IsModified> true, чтобы принудительно обновить данное значение свойства EF Core или присвоить ему значение false, чтобы запретить EF Core обновления значения свойства.</span><span class="sxs-lookup"><span data-stu-id="ca23d-172">Set <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.IsModified> to true to force EF Core to update a given property value, or set it to false to prevent EF Core from updating the property value.</span></span>
- <span data-ttu-id="ca23d-173">[Временные значения](xref:core/change-tracking/miscellaneous) обычно создаются [генераторами значений](xref:core/modeling/generated-properties)EF Core.</span><span class="sxs-lookup"><span data-stu-id="ca23d-173">[Temporary values](xref:core/change-tracking/miscellaneous) are typically generated by EF Core [value generators](xref:core/modeling/generated-properties).</span></span> <span data-ttu-id="ca23d-174">Установка текущего значения свойства заменит временное значение указанным значением и помечает свойство как невременное.</span><span class="sxs-lookup"><span data-stu-id="ca23d-174">Setting the current value of a property will replace the temporary value with the given value and mark the property as not temporary.</span></span> <span data-ttu-id="ca23d-175">Задайте <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.IsTemporary> для параметра значение true, чтобы принудительно использовать временной даже после его явного задания.</span><span class="sxs-lookup"><span data-stu-id="ca23d-175">Set <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.IsTemporary> to true to force a value to be temporary even after it has been explicitly set.</span></span>

### <a name="working-with-a-single-navigation"></a><span data-ttu-id="ca23d-176">Работа с одной навигацией</span><span class="sxs-lookup"><span data-stu-id="ca23d-176">Working with a single navigation</span></span>

<span data-ttu-id="ca23d-177">Несколько перегрузок <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601.Reference%2A?displayProperty=nameWithType> , <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601.Collection%2A?displayProperty=nameWithType> и <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Navigation%2A?displayProperty=nameWithType> предоставляют доступ к сведениям об отдельных переходах.</span><span class="sxs-lookup"><span data-stu-id="ca23d-177">Several overloads of <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601.Reference%2A?displayProperty=nameWithType>, <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601.Collection%2A?displayProperty=nameWithType>, and <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Navigation%2A?displayProperty=nameWithType> allow access to information about an individual navigation.</span></span>

<span data-ttu-id="ca23d-178">Доступ к ссылочным переходам к одной связанной сущности осуществляется с помощью <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601.Reference%2A> методов.</span><span class="sxs-lookup"><span data-stu-id="ca23d-178">Reference navigations to a single related entity are accessed through the <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601.Reference%2A> methods.</span></span> <span data-ttu-id="ca23d-179">Навигационные ссылки указывают на сторону «один» связей «один ко многим» и обе стороны связей «один к одному».</span><span class="sxs-lookup"><span data-stu-id="ca23d-179">Reference navigations point to the "one" sides of one-to-many relationships, and both sides of one-to-one relationships.</span></span> <span data-ttu-id="ca23d-180">Например:</span><span class="sxs-lookup"><span data-stu-id="ca23d-180">For example:</span></span>

<!--
        ReferenceEntry<Post, Blog> referenceEntry1 = context.Entry(post).Reference(e => e.Blog);
        ReferenceEntry<Post, Blog> referenceEntry2 = context.Entry(post).Reference<Blog>("Blog");
        ReferenceEntry referenceEntry3 = context.Entry(post).Reference("Blog");
-->
[!code-csharp[Work_with_a_single_navigation_1](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_a_single_navigation_1)]

<span data-ttu-id="ca23d-181">Переходы также могут быть коллекциями связанных сущностей при использовании для связей "многие" между "один ко многим" и "многие ко многим".</span><span class="sxs-lookup"><span data-stu-id="ca23d-181">Navigations can also be collections of related entities when used for the "many" sides of one-to-many and many-to-many relationships.</span></span> <span data-ttu-id="ca23d-182"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601.Collection%2A>Методы используются для доступа к навигации по коллекциям.</span><span class="sxs-lookup"><span data-stu-id="ca23d-182">The <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601.Collection%2A> methods are used to access collection navigations.</span></span> <span data-ttu-id="ca23d-183">Например:</span><span class="sxs-lookup"><span data-stu-id="ca23d-183">For example:</span></span>

<!--
        CollectionEntry<Blog, Post> collectionEntry1 = context.Entry(blog).Collection(e => e.Posts);
        CollectionEntry<Blog, Post> collectionEntry2 = context.Entry(blog).Collection<Post>("Posts");
        CollectionEntry collectionEntry3 = context.Entry(blog).Collection("Posts");
-->
[!code-csharp[Work_with_a_single_navigation_2a](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_a_single_navigation_2a)]

<span data-ttu-id="ca23d-184">Некоторые операции являются общими для всех переходов.</span><span class="sxs-lookup"><span data-stu-id="ca23d-184">Some operations are common for all navigations.</span></span> <span data-ttu-id="ca23d-185">Они доступны для навигации по ссылке и коллекции с помощью <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Navigation%2A?displayProperty=nameWithType> метода.</span><span class="sxs-lookup"><span data-stu-id="ca23d-185">These can be accessed for both reference and collection navigations using the <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Navigation%2A?displayProperty=nameWithType> method.</span></span> <span data-ttu-id="ca23d-186">Обратите внимание, что при доступе ко всем переходам доступны только неуниверсальный доступ.</span><span class="sxs-lookup"><span data-stu-id="ca23d-186">Note that only non-generic access is available when accessing all navigations together.</span></span> <span data-ttu-id="ca23d-187">Например:</span><span class="sxs-lookup"><span data-stu-id="ca23d-187">For example:</span></span>

<!--
        NavigationEntry navigationEntry = context.Entry(blog).Navigation("Posts");
-->
[!code-csharp[Work_with_a_single_navigation_2b](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_a_single_navigation_2b)]

<span data-ttu-id="ca23d-188">В следующей таблице приведены способы использования <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ReferenceEntry%602> , <xref:Microsoft.EntityFrameworkCore.ChangeTracking.CollectionEntry%602> и <xref:Microsoft.EntityFrameworkCore.ChangeTracking.NavigationEntry> :</span><span class="sxs-lookup"><span data-stu-id="ca23d-188">The following table summarizes ways to use <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ReferenceEntry%602>, <xref:Microsoft.EntityFrameworkCore.ChangeTracking.CollectionEntry%602>, and <xref:Microsoft.EntityFrameworkCore.ChangeTracking.NavigationEntry>:</span></span>

| <span data-ttu-id="ca23d-189">Навигатионентри, элемент</span><span class="sxs-lookup"><span data-stu-id="ca23d-189">NavigationEntry member</span></span>                                                                                    | <span data-ttu-id="ca23d-190">Описание</span><span class="sxs-lookup"><span data-stu-id="ca23d-190">Description</span></span>
|:----------------------------------------------------------------------------------------------------------|----------------------
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.MemberEntry.CurrentValue?displayProperty=nameWithType> | <span data-ttu-id="ca23d-191">Возвращает и задает текущее значение навигации.</span><span class="sxs-lookup"><span data-stu-id="ca23d-191">Gets and sets the current value of the navigation.</span></span> <span data-ttu-id="ca23d-192">Это вся коллекция для навигации по коллекциям.</span><span class="sxs-lookup"><span data-stu-id="ca23d-192">This is the entire collection for collection navigations.</span></span>
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.NavigationEntry.Metadata?displayProperty=nameWithType> | <span data-ttu-id="ca23d-193"><xref:Microsoft.EntityFrameworkCore.Metadata.INavigationBase> метаданные для навигации.</span><span class="sxs-lookup"><span data-stu-id="ca23d-193"><xref:Microsoft.EntityFrameworkCore.Metadata.INavigationBase> metadata for the navigation.</span></span>
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.NavigationEntry.IsLoaded?displayProperty=nameWithType> | <span data-ttu-id="ca23d-194">Возвращает или задает значение, указывающее, была ли связанная сущность или коллекция полностью загружены из базы данных.</span><span class="sxs-lookup"><span data-stu-id="ca23d-194">Gets or sets a value indicating whether the related entity or collection has been fully loaded from the database.</span></span>
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.NavigationEntry.Load?displayProperty=nameWithType>     | <span data-ttu-id="ca23d-195">Загружает связанную сущность или коллекцию из базы данных; см. раздел [явная загрузка связанных данных](xref:core/querying/related-data/explicit).</span><span class="sxs-lookup"><span data-stu-id="ca23d-195">Loads the related entity or collection from the database; see [Explicit Loading of Related Data](xref:core/querying/related-data/explicit).</span></span>
| <xref:Microsoft.EntityFrameworkCore.ChangeTracking.NavigationEntry.Query?displayProperty=nameWithType>    | <span data-ttu-id="ca23d-196">Запрос EF Core будет использовать для загрузки этой навигации в виде `IQueryable` , который может составляться дальше; см. [явную загрузку связанных данных](xref:core/querying/related-data/explicit).</span><span class="sxs-lookup"><span data-stu-id="ca23d-196">The query EF Core would use to load this navigation as an `IQueryable` that can be further composed; see [Explicit Loading of Related Data](xref:core/querying/related-data/explicit).</span></span>

### <a name="working-with-all-properties-of-an-entity"></a><span data-ttu-id="ca23d-197">Работа со всеми свойствами сущности</span><span class="sxs-lookup"><span data-stu-id="ca23d-197">Working with all properties of an entity</span></span>

<span data-ttu-id="ca23d-198"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Properties?displayProperty=nameWithType> Возвращает объект <xref:System.Collections.Generic.IEnumerable%601> <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry> для каждого свойства сущности.</span><span class="sxs-lookup"><span data-stu-id="ca23d-198"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Properties?displayProperty=nameWithType> returns an <xref:System.Collections.Generic.IEnumerable%601> of <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry> for every property of the entity.</span></span> <span data-ttu-id="ca23d-199">Это можно использовать для выполнения действия для каждого свойства сущности.</span><span class="sxs-lookup"><span data-stu-id="ca23d-199">This can be used to perform an action for every property of the entity.</span></span> <span data-ttu-id="ca23d-200">Например, чтобы задать для любого свойства DateTime значение `DateTime.Now` :</span><span class="sxs-lookup"><span data-stu-id="ca23d-200">For example, to set any DateTime property to `DateTime.Now`:</span></span>

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

<span data-ttu-id="ca23d-201">Кроме того, Ентитентри содержит несколько методов для получения и установки всех значений свойств одновременно.</span><span class="sxs-lookup"><span data-stu-id="ca23d-201">In addition, EntityEntry contains several methods to get and set all property values at the same time.</span></span> <span data-ttu-id="ca23d-202">Эти методы используют <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyValues> класс, представляющий коллекцию свойств и их значений.</span><span class="sxs-lookup"><span data-stu-id="ca23d-202">These methods use the <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyValues> class, which represents a collection of properties and their values.</span></span> <span data-ttu-id="ca23d-203">Пропертивалуес можно получить для текущих или исходных значений или для значений, хранимых в настоящее время в базе данных.</span><span class="sxs-lookup"><span data-stu-id="ca23d-203">PropertyValues can be obtained for current or original values, or for the values as currently stored in the database.</span></span> <span data-ttu-id="ca23d-204">Например:</span><span class="sxs-lookup"><span data-stu-id="ca23d-204">For example:</span></span>

<!--
        var currentValues = context.Entry(blog).CurrentValues;
        var originalValues = context.Entry(blog).OriginalValues;
        var databaseValues = context.Entry(blog).GetDatabaseValues();
-->
[!code-csharp[Work_with_all_properties_of_an_entity_2a](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_all_properties_of_an_entity_2a)]

<span data-ttu-id="ca23d-205">Эти объекты Пропертивалуес не очень полезны.</span><span class="sxs-lookup"><span data-stu-id="ca23d-205">These PropertyValues objects are not very useful on their own.</span></span> <span data-ttu-id="ca23d-206">Однако их можно комбинировать для выполнения общих операций, необходимых при управлении сущностями.</span><span class="sxs-lookup"><span data-stu-id="ca23d-206">However, they can be combined to perform common operations needed when manipulating entities.</span></span> <span data-ttu-id="ca23d-207">Это полезно при работе с объектами перемещения данных и при разрешении [конфликтов оптимистичного параллелизма](xref:core/saving/concurrency).</span><span class="sxs-lookup"><span data-stu-id="ca23d-207">This is useful when working with data transfer objects and when resolving [optimistic concurrency conflicts](xref:core/saving/concurrency).</span></span> <span data-ttu-id="ca23d-208">В следующих разделах приведены некоторые примеры.</span><span class="sxs-lookup"><span data-stu-id="ca23d-208">The following sections show some examples.</span></span>

#### <a name="setting-current-or-original-values-from-an-entity-or-dto"></a><span data-ttu-id="ca23d-209">Установка текущих или исходных значений из сущности или DTO</span><span class="sxs-lookup"><span data-stu-id="ca23d-209">Setting current or original values from an entity or DTO</span></span>

<span data-ttu-id="ca23d-210">Текущие или исходные значения сущности могут быть обновлены путем копирования значений из другого объекта.</span><span class="sxs-lookup"><span data-stu-id="ca23d-210">The current or original values of an entity can be updated by copying values from another object.</span></span> <span data-ttu-id="ca23d-211">Например, рассмотрим `BlogDto` объект передачи данных (DTO) с теми же свойствами, что и для типа сущности:</span><span class="sxs-lookup"><span data-stu-id="ca23d-211">For example, consider a `BlogDto` data transfer object (DTO) with the same properties as the entity type:</span></span>

<!--
public class BlogDto
{
    public int Id { get; set; }
    public string Name { get; set; }
}
-->
[!code-csharp[BlogDto](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=BlogDto)]

<span data-ttu-id="ca23d-212">Это можно использовать для задания текущих значений отслеживающей сущности с помощью <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyValues.SetValues%2A?displayProperty=nameWithType> :</span><span class="sxs-lookup"><span data-stu-id="ca23d-212">This can be used to set the current values of a tracked entity using <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyValues.SetValues%2A?displayProperty=nameWithType>:</span></span>

<!--
        var blogDto = new BlogDto { Id = 1, Name = "1unicorn2" };

        context.Entry(blog).CurrentValues.SetValues(blogDto);
-->
[!code-csharp[Work_with_all_properties_of_an_entity_2b](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_all_properties_of_an_entity_2b)]

<span data-ttu-id="ca23d-213">Этот метод иногда используется при обновлении сущности со значениями, полученными от вызова службы или клиентом в n-уровневых приложении.</span><span class="sxs-lookup"><span data-stu-id="ca23d-213">This technique is sometimes used when updating an entity with values obtained from a service call or a client in an n-tier application.</span></span> <span data-ttu-id="ca23d-214">Обратите внимание, что используемый объект не должен быть того же типа, что и сущность, пока он имеет свойства, имена которых совпадают с именами сущности.</span><span class="sxs-lookup"><span data-stu-id="ca23d-214">Note that the object used does not have to be of the same type as the entity so long as it has properties whose names match those of the entity.</span></span> <span data-ttu-id="ca23d-215">В приведенном выше примере экземпляр DTO `BlogDto` используется для установки текущих значений отслеживающей `Blog` сущности.</span><span class="sxs-lookup"><span data-stu-id="ca23d-215">In the example above, an instance of the DTO `BlogDto` is used to set the current values of a tracked `Blog` entity.</span></span>

<span data-ttu-id="ca23d-216">Обратите внимание, что свойства будут помечаться как измененные, только если значение отличается от текущего значения.</span><span class="sxs-lookup"><span data-stu-id="ca23d-216">Note that properties will only be marked as modified if the value set differs from the current value.</span></span>

#### <a name="setting-current-or-original-values-from-a-dictionary"></a><span data-ttu-id="ca23d-217">Установка текущих или исходных значений из словаря</span><span class="sxs-lookup"><span data-stu-id="ca23d-217">Setting current or original values from a dictionary</span></span>

<span data-ttu-id="ca23d-218">В предыдущем примере задаются значения из экземпляра сущности или объекта DTO.</span><span class="sxs-lookup"><span data-stu-id="ca23d-218">The previous example set values from an entity or DTO instance.</span></span> <span data-ttu-id="ca23d-219">Такое же поведение доступно, когда значения свойств хранятся в словаре как пары "имя-значение".</span><span class="sxs-lookup"><span data-stu-id="ca23d-219">The same behavior is available when property values are stored as name/value pairs in a dictionary.</span></span> <span data-ttu-id="ca23d-220">Например:</span><span class="sxs-lookup"><span data-stu-id="ca23d-220">For example:</span></span>

<!--
        var blogDictionary = new Dictionary<string, object>
        {
            ["Id"] = 1,
            ["Name"] = "1unicorn2"
        };

        context.Entry(blog).CurrentValues.SetValues(blogDictionary);
-->
[!code-csharp[Work_with_all_properties_of_an_entity_2d](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_all_properties_of_an_entity_2d)]

#### <a name="setting-current-or-original-values-from-the-database"></a><span data-ttu-id="ca23d-221">Установка текущих или исходных значений из базы данных</span><span class="sxs-lookup"><span data-stu-id="ca23d-221">Setting current or original values from the database</span></span>

<span data-ttu-id="ca23d-222">Текущие или исходные значения сущности могут быть обновлены с использованием последних значений из базы данных путем вызова метода <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.GetDatabaseValues> или <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.GetDatabaseValuesAsync%2A> и использования возвращенного объекта для задания текущих или исходных значений или и того, и другого.</span><span class="sxs-lookup"><span data-stu-id="ca23d-222">The current or original values of an entity can be updated with the latest values from the database by calling <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.GetDatabaseValues> or <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.GetDatabaseValuesAsync%2A> and using the returned object to set current or original values, or both.</span></span> <span data-ttu-id="ca23d-223">Например:</span><span class="sxs-lookup"><span data-stu-id="ca23d-223">For example:</span></span>

<!--
        var databaseValues = context.Entry(blog).GetDatabaseValues();
        context.Entry(blog).CurrentValues.SetValues(databaseValues);
        context.Entry(blog).OriginalValues.SetValues(databaseValues);
-->
[!code-csharp[Work_with_all_properties_of_an_entity_2c](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_all_properties_of_an_entity_2c)]

#### <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a><span data-ttu-id="ca23d-224">Создание клонированного объекта, содержащего текущие, исходные значения или базы данных</span><span class="sxs-lookup"><span data-stu-id="ca23d-224">Creating a cloned object containing current, original, or database values</span></span>

<span data-ttu-id="ca23d-225">Объект Пропертивалуес, возвращаемый из Куррентвалуес, Оригиналвалуес или Жетдатабасевалуес, можно использовать для создания клона сущности с помощью <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyValues.ToObject?displayProperty=nameWithType> .</span><span class="sxs-lookup"><span data-stu-id="ca23d-225">The PropertyValues object returned from CurrentValues, OriginalValues, or GetDatabaseValues can be used to create a clone of the entity using <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyValues.ToObject?displayProperty=nameWithType>.</span></span> <span data-ttu-id="ca23d-226">Например:</span><span class="sxs-lookup"><span data-stu-id="ca23d-226">For example:</span></span>

<!--
var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
-->
[!code-csharp[Work_with_all_properties_of_an_entity_2e](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_all_properties_of_an_entity_2e)]

<span data-ttu-id="ca23d-227">Обратите внимание, что `ToObject` возвращает новый экземпляр, который не следит за DbContext.</span><span class="sxs-lookup"><span data-stu-id="ca23d-227">Note that `ToObject` returns a new instance that is not tracked by the DbContext.</span></span> <span data-ttu-id="ca23d-228">Возвращаемый объект также не имеет связей, настроенных для других сущностей.</span><span class="sxs-lookup"><span data-stu-id="ca23d-228">The returned object also does not have any relationships set to other entities.</span></span>

<span data-ttu-id="ca23d-229">Клонированный объект может быть полезен для устранения проблем, связанных с параллельными обновлениями базы данных, особенно при привязке данных к объектам определенного типа.</span><span class="sxs-lookup"><span data-stu-id="ca23d-229">The cloned object can be useful for resolving issues related to concurrent updates to the database, especially when data binding to objects of a certain type.</span></span> <span data-ttu-id="ca23d-230">Дополнительные сведения см. в разделе [оптимистичный параллелизм](xref:core/saving/concurrency) .</span><span class="sxs-lookup"><span data-stu-id="ca23d-230">See [optimistic concurrency](xref:core/saving/concurrency) for more information.</span></span>

### <a name="working-with-all-navigations-of-an-entity"></a><span data-ttu-id="ca23d-231">Работа со всеми навигациями сущности</span><span class="sxs-lookup"><span data-stu-id="ca23d-231">Working with all navigations of an entity</span></span>

<span data-ttu-id="ca23d-232"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Navigations?displayProperty=nameWithType> Возвращает объект <xref:System.Collections.Generic.IEnumerable%601> <xref:Microsoft.EntityFrameworkCore.ChangeTracking.NavigationEntry> для каждой навигации по сущности.</span><span class="sxs-lookup"><span data-stu-id="ca23d-232"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Navigations?displayProperty=nameWithType> returns an <xref:System.Collections.Generic.IEnumerable%601> of <xref:Microsoft.EntityFrameworkCore.ChangeTracking.NavigationEntry> for every navigation of the entity.</span></span> <span data-ttu-id="ca23d-233"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.References?displayProperty=nameWithType> и <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Collections?displayProperty=nameWithType> выполняют те же действия, но ограничены ссылками или навигациями по коллекциям соответственно.</span><span class="sxs-lookup"><span data-stu-id="ca23d-233"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.References?displayProperty=nameWithType> and <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Collections?displayProperty=nameWithType> do the same thing, but restricted to reference or collection navigations respectively.</span></span> <span data-ttu-id="ca23d-234">Это можно использовать для выполнения действий при каждой навигации по сущности.</span><span class="sxs-lookup"><span data-stu-id="ca23d-234">This can be used to perform an action for every navigation of the entity.</span></span> <span data-ttu-id="ca23d-235">Например, для принудительной загрузки всех связанных сущностей:</span><span class="sxs-lookup"><span data-stu-id="ca23d-235">For example, to force loading of all related entities:</span></span>

<!--
        foreach (var navigationEntry in context.Entry(blog).Navigations)
        {
            navigationEntry.Load();
        }
-->
[!code-csharp[Work_with_all_navigations_of_an_entity_1](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_all_navigations_of_an_entity_1)]

### <a name="working-with-all-members-of-an-entity"></a><span data-ttu-id="ca23d-236">Работа со всеми членами сущности</span><span class="sxs-lookup"><span data-stu-id="ca23d-236">Working with all members of an entity</span></span>

<span data-ttu-id="ca23d-237">Обычные свойства и свойства навигации имеют разное состояние и поведение.</span><span class="sxs-lookup"><span data-stu-id="ca23d-237">Regular properties and navigation properties have different state and behavior.</span></span> <span data-ttu-id="ca23d-238">Таким образом, обычно обработка переходов и несамостоятельных переходов выполняется отдельно, как показано в приведенных выше разделах.</span><span class="sxs-lookup"><span data-stu-id="ca23d-238">It is therefore common to process navigations and non-navigations separately, as shown in the sections above.</span></span> <span data-ttu-id="ca23d-239">Однако иногда может оказаться полезным сделать что-то с любым членом сущности, независимо от того, является ли он обычным свойством или навигацией.</span><span class="sxs-lookup"><span data-stu-id="ca23d-239">However, sometimes it can be useful to do something with any member of the entity, regardless of whether it is a regular property or navigation.</span></span> <span data-ttu-id="ca23d-240"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Member%2A?displayProperty=nameWithType> и <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Members?displayProperty=nameWithType> предоставляются для этой цели.</span><span class="sxs-lookup"><span data-stu-id="ca23d-240"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Member%2A?displayProperty=nameWithType> and <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Members?displayProperty=nameWithType> are provided for this purpose.</span></span> <span data-ttu-id="ca23d-241">Например:</span><span class="sxs-lookup"><span data-stu-id="ca23d-241">For example:</span></span>

<!--
        foreach (var memberEntry in context.Entry(blog).Members)
        {
            Console.WriteLine(
                $"Member {memberEntry.Metadata.Name} is of type {memberEntry.Metadata.ClrType.ShortDisplayName()} and has value {memberEntry.CurrentValue}");
        }
-->
[!code-csharp[Work_with_all_members_of_an_entity_1](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Work_with_all_members_of_an_entity_1)]

<span data-ttu-id="ca23d-242">При выполнении этого кода в блоге из примера создаются следующие выходные данные:</span><span class="sxs-lookup"><span data-stu-id="ca23d-242">Running this code on a blog from the sample generates the following output:</span></span>

```output
Member Id is of type int and has value 1
Member Name is of type string and has value .NET Blog
Member Posts is of type IList<Post> and has value System.Collections.Generic.List`1[Post]
```

> [!TIP]
> <span data-ttu-id="ca23d-243">В [представлении отладки "средство записи изменений](xref:core/change-tracking/debug-views) " отображаются такие сведения.</span><span class="sxs-lookup"><span data-stu-id="ca23d-243">The [change tracker debug view](xref:core/change-tracking/debug-views) shows information like this.</span></span> <span data-ttu-id="ca23d-244">Представление Отладка для всего средства записи изменений создается на основе отдельных объектов <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.DebugView?displayProperty=nameWithType> каждой записи.</span><span class="sxs-lookup"><span data-stu-id="ca23d-244">The debug view for the entire change tracker is generated from the individual <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.DebugView?displayProperty=nameWithType> of each tracked entity.</span></span>

## <a name="find-and-findasync"></a><span data-ttu-id="ca23d-245">Поиск и FindAsync</span><span class="sxs-lookup"><span data-stu-id="ca23d-245">Find and FindAsync</span></span>

<span data-ttu-id="ca23d-246"><xref:Microsoft.EntityFrameworkCore.DbContext.Find%2A?displayProperty=nameWithType>, <xref:Microsoft.EntityFrameworkCore.DbContext.FindAsync%2A?displayProperty=nameWithType> , <xref:Microsoft.EntityFrameworkCore.DbSet%601.Find%2A?displayProperty=nameWithType> и <xref:Microsoft.EntityFrameworkCore.DbSet%601.FindAsync%2A?displayProperty=nameWithType> предназначены для эффективного поиска одной сущности, если ее первичный ключ известен.</span><span class="sxs-lookup"><span data-stu-id="ca23d-246"><xref:Microsoft.EntityFrameworkCore.DbContext.Find%2A?displayProperty=nameWithType>, <xref:Microsoft.EntityFrameworkCore.DbContext.FindAsync%2A?displayProperty=nameWithType>, <xref:Microsoft.EntityFrameworkCore.DbSet%601.Find%2A?displayProperty=nameWithType>, and <xref:Microsoft.EntityFrameworkCore.DbSet%601.FindAsync%2A?displayProperty=nameWithType> are designed for efficient lookup of a single entity when its primary key is known.</span></span> <span data-ttu-id="ca23d-247">Найти сначала проверяет, была ли сущность уже отслеживанием, и, если это так, возвращает сущность немедленно.</span><span class="sxs-lookup"><span data-stu-id="ca23d-247">Find first checks if the entity is already tracked, and if so returns the entity immediately.</span></span> <span data-ttu-id="ca23d-248">Запрос к базе данных выполняется только в том случае, если сущность не была записана локально.</span><span class="sxs-lookup"><span data-stu-id="ca23d-248">A database query is only made if the entity is not tracked locally.</span></span> <span data-ttu-id="ca23d-249">Например, рассмотрим следующий код, который вызывает метод Find дважды для одной и той же сущности:</span><span class="sxs-lookup"><span data-stu-id="ca23d-249">For example, consider this code that calls Find twice for the same entity:</span></span>

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

<span data-ttu-id="ca23d-250">Выходные данные этого кода (включая ведение журнала EF Core) при использовании SQLite:</span><span class="sxs-lookup"><span data-stu-id="ca23d-250">The output from this code (including EF Core logging) when using SQLite is:</span></span>

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

<span data-ttu-id="ca23d-251">Обратите внимание, что первый вызов не находит сущность локально, поэтому выполняет запрос к базе данных.</span><span class="sxs-lookup"><span data-stu-id="ca23d-251">Notice that the first call does not find the entity locally and so executes a database query.</span></span> <span data-ttu-id="ca23d-252">И наоборот, второй вызов возвращает один и тот же экземпляр без запроса к базе данных, так как он уже отслеживается.</span><span class="sxs-lookup"><span data-stu-id="ca23d-252">Conversely, the second call returns the same instance without querying the database because it is already being tracked.</span></span>

<span data-ttu-id="ca23d-253">Функция Find возвращает значение null, если сущность с заданным ключом не отслеживанием локально и не существует в базе данных.</span><span class="sxs-lookup"><span data-stu-id="ca23d-253">Find returns null if an entity with the given key is not tracked locally and does not exist in the database.</span></span>

### <a name="composite-keys"></a><span data-ttu-id="ca23d-254">Составные ключи</span><span class="sxs-lookup"><span data-stu-id="ca23d-254">Composite keys</span></span>

<span data-ttu-id="ca23d-255">Поиск также можно использовать с составными ключами.</span><span class="sxs-lookup"><span data-stu-id="ca23d-255">Find can also be used with composite keys.</span></span> <span data-ttu-id="ca23d-256">Например, рассмотрим `OrderLine` сущность с составным ключом, состоящим из идентификатора заказа и идентификатора продукта:</span><span class="sxs-lookup"><span data-stu-id="ca23d-256">For example, consider an `OrderLine` entity with a composite key consisting of the order ID and the product ID:</span></span>

<!--
public class OrderLine
{
    public int OrderId { get; set; }
    public int ProductId { get; set; }

    //...
}
-->
[!code-csharp[OrderLine](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=OrderLine)]

<span data-ttu-id="ca23d-257">Составной ключ должен быть настроен в <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A?displayProperty=nameWithType> для определения ключевых частей _и их порядка_.</span><span class="sxs-lookup"><span data-stu-id="ca23d-257">The composite key must be configured in <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A?displayProperty=nameWithType> to define the key parts _and their order_.</span></span> <span data-ttu-id="ca23d-258">Например:</span><span class="sxs-lookup"><span data-stu-id="ca23d-258">For example:</span></span>

<!--
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder
            .Entity<OrderLine>()
            .HasKey(e => new { e.OrderId, e.ProductId });
    }
-->
[!code-csharp[OnModelCreating](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=OnModelCreating)]

<span data-ttu-id="ca23d-259">Обратите внимание, что `OrderId` является первой частью ключа и `ProductId` является второй частью ключа.</span><span class="sxs-lookup"><span data-stu-id="ca23d-259">Notice that `OrderId` is the first part of the key and `ProductId` is the second part of the key.</span></span> <span data-ttu-id="ca23d-260">Этот порядок следует использовать при передаче ключевых значений для поиска.</span><span class="sxs-lookup"><span data-stu-id="ca23d-260">This order must be used when passing key values to Find.</span></span> <span data-ttu-id="ca23d-261">Например:</span><span class="sxs-lookup"><span data-stu-id="ca23d-261">For example:</span></span>

<!--
        var orderline = context.OrderLines.Find(orderId, productId);
-->
[!code-csharp[Find_and_FindAsync_2](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Find_and_FindAsync_2)]

## <a name="using-changetrackerentries-to-access-all-tracked-entities"></a><span data-ttu-id="ca23d-262">Использование ChangeTracker. записи для доступа ко всем отслеживающим сущностям</span><span class="sxs-lookup"><span data-stu-id="ca23d-262">Using ChangeTracker.Entries to access all tracked entities</span></span>

<span data-ttu-id="ca23d-263">Пока мы получили доступ только к одному объекту <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry> за раз.</span><span class="sxs-lookup"><span data-stu-id="ca23d-263">So far we have accessed only a single <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry> at a time.</span></span> <span data-ttu-id="ca23d-264"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Entries?displayProperty=nameWithType> Возвращает Ентитентри для каждой сущности, которая в настоящее время отслеживании DbContext.</span><span class="sxs-lookup"><span data-stu-id="ca23d-264"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Entries?displayProperty=nameWithType> returns an EntityEntry for every entity currently tracked by the DbContext.</span></span> <span data-ttu-id="ca23d-265">Например:</span><span class="sxs-lookup"><span data-stu-id="ca23d-265">For example:</span></span>

<!--
        using var context = new BlogsContext();
        var blogs = context.Blogs.Include(e => e.Posts).ToList();

        foreach (var entityEntry in context.ChangeTracker.Entries())
        {
            Console.WriteLine($"Found {entityEntry.Metadata.Name} entity with ID {entityEntry.Property("Id").CurrentValue}");
        }
-->
[!code-csharp[Using_ChangeTracker_Entries_to_access_all_tracked_entities_1a](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Using_ChangeTracker_Entries_to_access_all_tracked_entities_1a)]

<span data-ttu-id="ca23d-266">Этот код создаст следующие выходные данные:</span><span class="sxs-lookup"><span data-stu-id="ca23d-266">This code generates the following output:</span></span>

```output
Found Blog entity with ID 1
Found Post entity with ID 1
Found Post entity with ID 2
```

<span data-ttu-id="ca23d-267">Обратите внимание, что возвращаются записи для блогов и записей.</span><span class="sxs-lookup"><span data-stu-id="ca23d-267">Notice that entries for both blogs and posts are returned.</span></span> <span data-ttu-id="ca23d-268">Вместо этого результаты можно отфильтровать по конкретному типу сущности с помощью <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Entries%60%601?displayProperty=nameWithType> универсальной перегрузки:</span><span class="sxs-lookup"><span data-stu-id="ca23d-268">The results can instead be filtered to a specific entity type using the <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Entries%60%601?displayProperty=nameWithType> generic overload:</span></span>

<!--
        foreach (var entityEntry in context.ChangeTracker.Entries<Post>())
        {
            Console.WriteLine(
                $"Found {entityEntry.Metadata.Name} entity with ID {entityEntry.Property(e => e.Id).CurrentValue}");
        }
-->
[!code-csharp[Using_ChangeTracker_Entries_to_access_all_tracked_entities_1b](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Using_ChangeTracker_Entries_to_access_all_tracked_entities_1b)]

<span data-ttu-id="ca23d-269">Выходные данные этого кода показывают, что возвращаются только записи.</span><span class="sxs-lookup"><span data-stu-id="ca23d-269">The output from this code shows that only posts are returned:</span></span>

```output
Found Post entity with ID 1
Found Post entity with ID 2
```

<span data-ttu-id="ca23d-270">Кроме того, при использовании универсальной перегрузки возвращаются универсальные <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601> экземпляры.</span><span class="sxs-lookup"><span data-stu-id="ca23d-270">Also, using the generic overload returns generic <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry%601> instances.</span></span> <span data-ttu-id="ca23d-271">Это позволяет обеспечить доступ к `Id` свойству в этом примере в свободном стиле.</span><span class="sxs-lookup"><span data-stu-id="ca23d-271">This is what allows that fluent-like access to the `Id` property in this example.</span></span>

<span data-ttu-id="ca23d-272">Универсальный тип, используемый для фильтрации, не обязательно должен быть сопоставленным типом сущности. Вместо этого можно использовать Несопоставленный базовый тип или интерфейс.</span><span class="sxs-lookup"><span data-stu-id="ca23d-272">The generic type used for filtering does not have to be a mapped entity type; an unmapped base type or interface can be used instead.</span></span> <span data-ttu-id="ca23d-273">Например, если все типы сущностей в модели реализуют интерфейс, определяющий его свойство ключа:</span><span class="sxs-lookup"><span data-stu-id="ca23d-273">For example, if all the entity types in the model implement an interface defining their key property:</span></span>

<!--
public interface IEntityWithKey
{
    int Id { get; set; }
}
-->
[!code-csharp[IEntityWithKey](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=IEntityWithKey)]

<span data-ttu-id="ca23d-274">Затем этот интерфейс можно использовать для работы с ключом любой отслеживающей сущности строго типизированным способом.</span><span class="sxs-lookup"><span data-stu-id="ca23d-274">Then this interface can be used to work with the key of any tracked entity in a strongly-typed manner.</span></span> <span data-ttu-id="ca23d-275">Например:</span><span class="sxs-lookup"><span data-stu-id="ca23d-275">For example:</span></span>

<!--
        foreach (var entityEntry in context.ChangeTracker.Entries<IEntityWithKey>())
        {
            Console.WriteLine(
                $"Found {entityEntry.Metadata.Name} entity with ID {entityEntry.Property(e => e.Id).CurrentValue}");
        }
-->
[!code-csharp[Using_ChangeTracker_Entries_to_access_all_tracked_entities_1c](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Using_ChangeTracker_Entries_to_access_all_tracked_entities_1c)]

## <a name="using-dbsetlocal-to-query-tracked-entities"></a><span data-ttu-id="ca23d-276">Использование DbSet. local для отслеживания запросов сущностей</span><span class="sxs-lookup"><span data-stu-id="ca23d-276">Using DbSet.Local to query tracked entities</span></span>

<span data-ttu-id="ca23d-277">EF Core запросы всегда выполняются в базе данных и возвращают только те сущности, которые были сохранены в базе данных.</span><span class="sxs-lookup"><span data-stu-id="ca23d-277">EF Core queries are always executed on the database, and only return entities that have been saved to the database.</span></span> <span data-ttu-id="ca23d-278"><xref:Microsoft.EntityFrameworkCore.DbSet%601.Local?displayProperty=nameWithType> предоставляет механизм для запроса DbContext для локальных, отслеживаний сущностей.</span><span class="sxs-lookup"><span data-stu-id="ca23d-278"><xref:Microsoft.EntityFrameworkCore.DbSet%601.Local?displayProperty=nameWithType> provides a mechanism to query the DbContext for local, tracked entities.</span></span>

<span data-ttu-id="ca23d-279">Так как `DbSet.Local` используется для запроса отслеживаний сущностей, обычно для загрузки сущностей в DbContext и последующей работы с загруженными сущностями.</span><span class="sxs-lookup"><span data-stu-id="ca23d-279">Since `DbSet.Local` is used to query tracked entities, it is typical to load entities into the DbContext and then work with those loaded entities.</span></span> <span data-ttu-id="ca23d-280">Это особенно справедливо для привязки данных, но также может быть полезным в других ситуациях.</span><span class="sxs-lookup"><span data-stu-id="ca23d-280">This is especially true for data binding, but can also be useful in other situations.</span></span> <span data-ttu-id="ca23d-281">Например, в следующем коде база данных сначала запрашивается для всех блогов и записей.</span><span class="sxs-lookup"><span data-stu-id="ca23d-281">For example, in the following code the database is first queried for all blogs and posts.</span></span> <span data-ttu-id="ca23d-282"><xref:Microsoft.EntityFrameworkCore.EntityFrameworkQueryableExtensions.Load%2A>Метод расширения используется для выполнения этого запроса с результатами, отслеживаемыми контекстом, без возврата непосредственно в приложение.</span><span class="sxs-lookup"><span data-stu-id="ca23d-282">The <xref:Microsoft.EntityFrameworkCore.EntityFrameworkQueryableExtensions.Load%2A> extension method is used to execute this query with the results tracked by the context without being returned directly to the application.</span></span> <span data-ttu-id="ca23d-283">(Использование `ToList` или аналогичное действие имеет тот же результат, но с издержками на создание возвращенного списка, что здесь не требуется.) Затем в примере используется `DbSet.Local` для доступа к локально отслеживающимся сущностям:</span><span class="sxs-lookup"><span data-stu-id="ca23d-283">(Using `ToList` or similar has the same effect but with the overhead of creating the returned list, which is not needed here.) The example then uses `DbSet.Local` to access the locally tracked entities:</span></span>

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

<span data-ttu-id="ca23d-284">Обратите внимание, что, в отличие от <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Entries?displayProperty=nameWithType> , `DbSet.Local` возвращает экземпляры сущности напрямую.</span><span class="sxs-lookup"><span data-stu-id="ca23d-284">Notice that, unlike <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Entries?displayProperty=nameWithType>, `DbSet.Local` returns entity instances directly.</span></span> <span data-ttu-id="ca23d-285">Конечно, Ентитентри можно всегда получать для возвращаемой сущности путем вызова метода <xref:System.Data.Entity.DbContext.Entry%2A?displayProperty=nameWithType> .</span><span class="sxs-lookup"><span data-stu-id="ca23d-285">An EntityEntry can, of course, always be obtained for the returned entity by calling <xref:System.Data.Entity.DbContext.Entry%2A?displayProperty=nameWithType>.</span></span>

### <a name="the-local-view"></a><span data-ttu-id="ca23d-286">Локальное представление</span><span class="sxs-lookup"><span data-stu-id="ca23d-286">The local view</span></span>

<span data-ttu-id="ca23d-287"><xref:Microsoft.EntityFrameworkCore.DbSet%601.Local?displayProperty=nameWithType> Возвращает представление локально отслеживаний сущностей, отражающих текущую <xref:Microsoft.EntityFrameworkCore.EntityState> сущность.</span><span class="sxs-lookup"><span data-stu-id="ca23d-287"><xref:Microsoft.EntityFrameworkCore.DbSet%601.Local?displayProperty=nameWithType> returns a view of locally tracked entities that reflects the current <xref:Microsoft.EntityFrameworkCore.EntityState> of those entities.</span></span> <span data-ttu-id="ca23d-288">В частности, это означает, что:</span><span class="sxs-lookup"><span data-stu-id="ca23d-288">Specifically, this means that:</span></span>

- <span data-ttu-id="ca23d-289">`Added` сущности включены.</span><span class="sxs-lookup"><span data-stu-id="ca23d-289">`Added` entities are included.</span></span> <span data-ttu-id="ca23d-290">Обратите внимание, что это не так для обычных запросов EF Core, так как `Added` сущности еще не существуют в базе данных и поэтому никогда не возвращаются запросом к базе данных.</span><span class="sxs-lookup"><span data-stu-id="ca23d-290">Note that this is not the case for normal EF Core queries, since `Added` entities do not yet exist in the database and so are therefore never returned by a database query.</span></span>
- <span data-ttu-id="ca23d-291">`Deleted` сущности исключаются.</span><span class="sxs-lookup"><span data-stu-id="ca23d-291">`Deleted` entities are excluded.</span></span> <span data-ttu-id="ca23d-292">Обратите внимание, что это еще не так для обычных запросов EF Core, так как `Deleted` сущности по-прежнему существуют в  базе данных и возвращаются запросами к базе данных.</span><span class="sxs-lookup"><span data-stu-id="ca23d-292">Note that this is again not the case for normal EF Core queries, since `Deleted` entities still exist in the database and so _are_ returned by database queries.</span></span>

<span data-ttu-id="ca23d-293">Это означает, что `DbSet.Local` это представление данных, отражающее текущее концептуальное состояние графа сущностей с `Added` включенными сущностями и `Deleted` исключенными сущностями.</span><span class="sxs-lookup"><span data-stu-id="ca23d-293">All of this means that `DbSet.Local` is view over the data that reflects the current conceptual state of the entity graph, with `Added` entities included and `Deleted` entities excluded.</span></span> <span data-ttu-id="ca23d-294">Это соответствует состоянию базы данных, которое должно быть после вызова SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="ca23d-294">This matches what database state is expected to be after SaveChanges is called.</span></span>

<span data-ttu-id="ca23d-295">Обычно это идеальное представление для привязки данных, так как оно представляет пользователю данные по мере их понимания на основе изменений, внесенных приложением.</span><span class="sxs-lookup"><span data-stu-id="ca23d-295">This is typically the ideal view for data binding, since it presents to the user the data as they understand it based on the changes made by the application.</span></span>

<span data-ttu-id="ca23d-296">В следующем примере кода показано, как пометить одну запись как, `Deleted` а затем добавить новую запись, пометив ее как `Added` :</span><span class="sxs-lookup"><span data-stu-id="ca23d-296">The following code demonstrates this my marking one post as `Deleted` and then adding a new post, marking it as `Added`:</span></span>

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

<span data-ttu-id="ca23d-297">Выходные данные этого кода:</span><span class="sxs-lookup"><span data-stu-id="ca23d-297">The output from this code is:</span></span>

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

<span data-ttu-id="ca23d-298">Обратите внимание, что удаленная запись удаляется из локального представления, а также включается добавленная запись.</span><span class="sxs-lookup"><span data-stu-id="ca23d-298">Notice that the deleted post is removed from the local view, and the added post is included.</span></span>

### <a name="using-local-to-add-and-remove-entities"></a><span data-ttu-id="ca23d-299">Использование Local для добавления и удаления сущностей</span><span class="sxs-lookup"><span data-stu-id="ca23d-299">Using Local to add and remove entities</span></span>

<span data-ttu-id="ca23d-300"><xref:Microsoft.EntityFrameworkCore.DbSet%601.Local?displayProperty=nameWithType> возвращает экземпляр <xref:Microsoft.EntityFrameworkCore.ChangeTracking.LocalView%601>.</span><span class="sxs-lookup"><span data-stu-id="ca23d-300"><xref:Microsoft.EntityFrameworkCore.DbSet%601.Local?displayProperty=nameWithType> returns an instance of <xref:Microsoft.EntityFrameworkCore.ChangeTracking.LocalView%601>.</span></span> <span data-ttu-id="ca23d-301">Это реализация <xref:System.Collections.Generic.ICollection%601> , которая создает и реагирует на уведомления при добавлении и удалении сущностей из коллекции.</span><span class="sxs-lookup"><span data-stu-id="ca23d-301">This is an implementation of <xref:System.Collections.Generic.ICollection%601> that generates and responds to notifications when entities are added and removed from the collection.</span></span> <span data-ttu-id="ca23d-302">(Это та же концепция, что <xref:System.Collections.ObjectModel.ObservableCollection%601> и, но реализованная как проекция поверх существующих EF Core записей отслеживания изменений, а не как независимая коллекция.)</span><span class="sxs-lookup"><span data-stu-id="ca23d-302">(This is the same concept as <xref:System.Collections.ObjectModel.ObservableCollection%601>, but implemented as a projection over existing EF Core change tracking entries, rather than as an independent collection.)</span></span>

<span data-ttu-id="ca23d-303">Уведомления локального представления подключаются к DbContext отслеживания изменений, так что локальное представление остается синхронизированным с DbContext.</span><span class="sxs-lookup"><span data-stu-id="ca23d-303">The local view's notifications are hooked into DbContext change tracking such that the local view stays in sync with the DbContext.</span></span> <span data-ttu-id="ca23d-304">В частности, внесены следующие изменения.</span><span class="sxs-lookup"><span data-stu-id="ca23d-304">Specifically:</span></span>

- <span data-ttu-id="ca23d-305">Добавление новой сущности приводит к `DbSet.Local` тому, что она будет относиться к DbContext, обычно в `Added` состоянии.</span><span class="sxs-lookup"><span data-stu-id="ca23d-305">Adding a new entity to `DbSet.Local` causes it to be tracked by the DbContext, typically in the `Added` state.</span></span> <span data-ttu-id="ca23d-306">(Если сущность уже имеет созданное значение ключа, он будет записан как `Unchanged` .)</span><span class="sxs-lookup"><span data-stu-id="ca23d-306">(If the entity already has a generated key value, then it is tracked as `Unchanged` instead.)</span></span>
- <span data-ttu-id="ca23d-307">Удаление сущности из `DbSet.Local` приводит к тому, что она будет помечена как `Deleted` .</span><span class="sxs-lookup"><span data-stu-id="ca23d-307">Removing an entity from `DbSet.Local` causes it to be marked as `Deleted`.</span></span>
- <span data-ttu-id="ca23d-308">Сущность, которая будет отслеживанием DbContext, будет автоматически отображаться в `DbSet.Local` коллекции.</span><span class="sxs-lookup"><span data-stu-id="ca23d-308">An entity that becomes tracked by the DbContext will automatically appear in the `DbSet.Local` collection.</span></span> <span data-ttu-id="ca23d-309">Например, выполнение запроса для большего числа сущностей автоматически приводит к обновлению локального представления.</span><span class="sxs-lookup"><span data-stu-id="ca23d-309">For example, executing a query to bring in more entities automatically causes the local view to be updated.</span></span>
- <span data-ttu-id="ca23d-310">Сущность, помеченная как, `Deleted` будет автоматически удалена из локальной коллекции.</span><span class="sxs-lookup"><span data-stu-id="ca23d-310">An entity that is marked as `Deleted` will be removed from the local collection automatically.</span></span>

<span data-ttu-id="ca23d-311">Это означает, что локальное представление можно использовать для управления отслеживающими сущностями, просто путем добавления и удаления из коллекции.</span><span class="sxs-lookup"><span data-stu-id="ca23d-311">This means the local view can be used to manipulate tracked entities simply by adding and removing from the collection.</span></span> <span data-ttu-id="ca23d-312">Например, позволяет изменить предыдущий пример кода для добавления и удаления записей из локальной коллекции:</span><span class="sxs-lookup"><span data-stu-id="ca23d-312">For example, lets modify the previous example code to add and remove posts from the local collection:</span></span>

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

<span data-ttu-id="ca23d-313">Выходные данные не изменяются из предыдущего примера, так как изменения, вносимые в локальное представление, синхронизируются с DbContext.</span><span class="sxs-lookup"><span data-stu-id="ca23d-313">The output remains unchanged from the previous example because changes made to the local view are synced with the DbContext.</span></span>

### <a name="using-the-local-view-for-windows-forms-or-wpf-data-binding"></a><span data-ttu-id="ca23d-314">Использование локального представления для Windows Forms или привязки данных WPF</span><span class="sxs-lookup"><span data-stu-id="ca23d-314">Using the local view for Windows Forms or WPF data binding</span></span>

<span data-ttu-id="ca23d-315"><xref:Microsoft.EntityFrameworkCore.DbSet%601.Local?displayProperty=nameWithType> формирует базу для привязки данных к EF Core сущностям.</span><span class="sxs-lookup"><span data-stu-id="ca23d-315"><xref:Microsoft.EntityFrameworkCore.DbSet%601.Local?displayProperty=nameWithType> forms the basis for data binding to EF Core entities.</span></span> <span data-ttu-id="ca23d-316">Однако как Windows Forms, так и WPF работают наилучшим образом при использовании с конкретным типом уведомления о предполагаемой коллекции.</span><span class="sxs-lookup"><span data-stu-id="ca23d-316">However, both Windows Forms and WPF work best when used with the specific type of notifying collection that they expect.</span></span> <span data-ttu-id="ca23d-317">Локальное представление поддерживает создание следующих типов коллекций:</span><span class="sxs-lookup"><span data-stu-id="ca23d-317">The local view supports creating these specific collection types:</span></span>

- <span data-ttu-id="ca23d-318"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.LocalView%601.ToObservableCollection?displayProperty=nameWithType> Возвращает <xref:System.Collections.ObjectModel.ObservableCollection%601> для привязки данных WPF.</span><span class="sxs-lookup"><span data-stu-id="ca23d-318"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.LocalView%601.ToObservableCollection?displayProperty=nameWithType> returns an <xref:System.Collections.ObjectModel.ObservableCollection%601> for WPF data binding.</span></span>
- <span data-ttu-id="ca23d-319"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.LocalView%601.ToBindingList?displayProperty=nameWithType> Возвращает <xref:System.ComponentModel.BindingList%601> для Windows Forms привязки данных.</span><span class="sxs-lookup"><span data-stu-id="ca23d-319"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.LocalView%601.ToBindingList?displayProperty=nameWithType> returns a <xref:System.ComponentModel.BindingList%601> for Windows Forms data binding.</span></span>

<span data-ttu-id="ca23d-320">Например:</span><span class="sxs-lookup"><span data-stu-id="ca23d-320">For example:</span></span>

<!--
        ObservableCollection<Post> observableCollection = context.Posts.Local.ToObservableCollection();
        BindingList<Post> bindingList = context.Posts.Local.ToBindingList();
-->
[!code-csharp[Using_DbSet_Local_to_query_tracked_entities_4](../../../samples/core/ChangeTracking/AccessingTrackedEntities/Samples.cs?name=Using_DbSet_Local_to_query_tracked_entities_4)]

<span data-ttu-id="ca23d-321">Дополнительные сведения о привязке данных WPF с EF Core см. в статье Начало [работы с WPF](xref:core/get-started/wpf) .</span><span class="sxs-lookup"><span data-stu-id="ca23d-321">See [Get Started with WPF](xref:core/get-started/wpf) for more information on WPF data binding with EF Core.</span></span>

> [!TIP]
> <span data-ttu-id="ca23d-322">Локальное представление для данного экземпляра DbSet создается отложенным при первом доступе, а затем кэшируется.</span><span class="sxs-lookup"><span data-stu-id="ca23d-322">The local view for a given DbSet instance is created lazily when first accessed and then cached.</span></span> <span data-ttu-id="ca23d-323">Создание Локалвиев выполняется быстро и не использует значительный объем памяти.</span><span class="sxs-lookup"><span data-stu-id="ca23d-323">LocalView creation itself is fast and it does not use significant memory.</span></span> <span data-ttu-id="ca23d-324">Однако он вызывает [DetectChanges](xref:core/change-tracking/change-detection), что может быть достаточно длительным для большого числа сущностей.</span><span class="sxs-lookup"><span data-stu-id="ca23d-324">However, it does call [DetectChanges](xref:core/change-tracking/change-detection), which can be slow for large numbers of entities.</span></span> <span data-ttu-id="ca23d-325">Коллекции, созданные `ToObservableCollection` и, `ToBindingList` также создаются отложенно, а затем кэшируются.</span><span class="sxs-lookup"><span data-stu-id="ca23d-325">The collections created by `ToObservableCollection` and `ToBindingList` are also created lazily and and then cached.</span></span> <span data-ttu-id="ca23d-326">Оба эти метода создают новые коллекции, которые могут работать достаточно долго и использовать большой объем памяти при использовании тысяч сущностей.</span><span class="sxs-lookup"><span data-stu-id="ca23d-326">Both of these methods create new collections, which can be slow and use a lot of memory when thousands of entities are involved.</span></span>
